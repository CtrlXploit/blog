# COTD#7

First we unzip the archive (`corrupted_repo.zip`).
```bash
unzip corrupted_repo.zip
```

```bash
Archive:  corrupted_repo.zip
   creating: final_challenge_package/06/
 extracting: final_challenge_package/06/a439dee4e65aea21ec16adbc52f2a0e628dca6
   creating: final_challenge_package/25/
 extracting: final_challenge_package/25/8339df0d455a32cb19e07f5aad1a7a0221e419
   creating: final_challenge_package/9c/
 extracting: final_challenge_package/9c/ef096cf9d4501f7b018af1ca7e0602f5f566fa
   creating: final_challenge_package/ab/
 extracting: final_challenge_package/ab/9ea3fd35f5071efd8c380d9d28bfb382f62487
   creating: final_challenge_package/b7/
 extracting: final_challenge_package/b7/34560b1cf66dc657e326f8acf5a1181626fe1a
   creating: final_challenge_package/d4/
 extracting: final_challenge_package/d4/bc06836a2469c5350d63e3161e88992cf8e7d9
   creating: final_challenge_package/info/
 extracting: final_challenge_package/last_known_commit.txt
   creating: final_challenge_package/pack/
```

*Note: You can list contents without extracting by using `unzip -l corrupted_repo.zip`*.

The layout immediately suggests a _Git object store_ rather than a normal project tree.
Two-character directories with hex filenames and `info` / `pack` are classic signs.

This is either a deliberately corrupted/misplaced `.git/objects` tree, or a Git bundle / packdump meant for us to dig in with plumbing commands or by manually inflating zlib-compressed objects.

Files in `.git/objects/` are stored as `<first2chars>/<next38chars>` (a 40-hex SHA1) and are zlib-compressed objects.

**The "vulnerability" is an _exposed Git object store_ which contains secrets in a blob (committed file) that would normally be hidden in the `.git` directory.
If such a store is accessible publicly (e.g., a `.git/` directory leaked to web root), sensitive data can be recovered.**

Typical real-world mistakes that lead to this:

-   `.git/` accidentally uploaded to a web-accessible directory, allowing download of object files.
-   A backup or exported git object store made available without the working tree.

**Security Lesson**: Never expose `.git/` directory.

## Exploitation Strategies

### Approach A - Manual inflate of zlib objects

This is often the fastest and least intrusive approach when you only need to read a few objects.

Decompress the zlib stream and drop the git header (header is like `blob <size>\x00`) with Python:

```python
import zlib, sys
with open('final_challenge_package/b7/34560b1cf66dc657e326f8acf5a1181626fe1a','rb') as f:
    data = f.read()
print(zlib.decompress(data))
```

After a process of hit and trial, we receive,

```bash
b'blob 90\x00project_name: secret_sauce_v1\ndatabase_url: dev.local\npassword: cotd{g1t_plumb1ng_m@st3r}\n'
```

This prints the entire decompressed object (header included).

Git objects are stored as `zlib.compress(b"<type> <len>\x00" + payload)`.

### Approach B - Reconstruct `.git` objects and use Git plumbing

A more robust, replicable path is to recreate a minimal `.git` object store locally, then invoke `git` plumbing commands to traverse commits/trees and print files.

1.  Create working dir and initialize an empty Git repo (not strictly necessary to be a working repo, but `git` commands expect a `.git` folder):
    

```bash
mkdir recovered_repo && cd recovered_repo
git init
```

```bash
Initialized empty Git repository in /home/miracleinvoker/recovered_repo/.git/
```

This creates a `.git` structure for Git to operate against.
    

2.  Copy object files into the `.git/objects` directory
```bash
cp -r ../final_challenge_package/[0-9a-f][0-9a-f] .git/objects/
```
This places the compressed object files exactly where Git expects them.

3.  Tell Git about the HEAD commit (optional but helpful).
`last_known_commit.txt` contains the commit SHA `06a439dee4e65aea21ec16adbc52f2a0e628dca6`.
We can set `HEAD` to that in `refs/heads/recovered`:
    

```bash
mkdir -p .git/refs/heads
echo '06a439dee4e65aea21ec16adbc52f2a0e628dca6' > .git/refs/heads/recovered
```

This creates a branch pointing at that commit (if commit object exists in .git/objects)    

4.  Run `git fsck` to verify and reveal object graph / dangling objects:

```bash
git fsck --full --no-reflogs --unreachable
```

```bash
Checking ref database: 100% (1/1), done.
Checking object directories: 100% (256/256), done.
notice: HEAD points to an unborn branch (master)
```

`git fsck` checks connectivity. It will reveal missing objects and list what is present.
    

5.  Inspect the commit object and tree(s):

```bash
git cat-file -p 06a439dee4e65aea21ec16adbc52f2a0e628dca6
```

```bash
tree 9cef096cf9d4501f7b018af1ca7e0602f5f566fa
parent d4bc06836a2469c5350d63e3161e88992cf8e7d9
author MiracleInvoker <miracle@invoker.net> 1759906553 +0530
committer MiracleInvoker <miracle@invoker.net> 1759906553 +0530

Remove sensitive data from config
```

This prints commit metadata: author, committer, commit message and a `tree <sha>` line.

6.  To view a file content from a commit, use `git show`:
    

```bash
git show 06a439dee4e65aea21ec16adbc52f2a0e628dca6
```

```bash
commit 06a439dee4e65aea21ec16adbc52f2a0e628dca6 (recovered)
Author: MiracleInvoker <miracle@invoker.net>
Date:   Wed Oct 8 12:25:53 2025 +0530

    Remove sensitive data from config

diff --git a/config.yml b/config.yml
index b734560..258339d 100644
--- a/config.yml
+++ b/config.yml
@@ -1,3 +1,2 @@
 project_name: secret_sauce_v1
 database_url: dev.local
-password: cotd{g1t_plumb1ng_m@st3r}
```