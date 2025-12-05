# Winter of Code 8.0
## Cyber Labs Information Security Division

Winter of Code 8.0 is a month-long recruitment hackathon of Cyber Labs, IIT (ISM) Dhanbad.

Few resources are provided to get started, but explore freely. Avoid cloning projects; build your own solutions following the statements.

The Most Important Resource: `Your Search Engine + LLMs`

You're free to use any LLMs or tools available to you, in fact, we encourage it!
But use them as teachers, not as builders. Let them help you think, debug, explore ideas, and learn… **not generate your entire project.**
It is critical that each line written is original (ie written by you) with a clear understanding of its importance/functionality.

Your creativity, understanding, and originality are what truly matter.

Dive into the problem statements below and make the most of this month.

Good luck! 

---
The problem statements for WOC 8.0 (Infosec Division) are:
---
# Building a minimal Container runtime

**Description:** Build a minimal container runtime similar to Docker that uses core Linux primitives to isolate and run processes. The goal is to demonstrate how containers work internally by creating a lightweight system that launches applications inside independent namespaces and a custom root filesystem.

**Key Features:**
- Use Linux namespaces (PID, UTS, MNT; optional USER/NET) to isolate processes.
- Provide a separate root filesystem.
- Run arbitrary commands inside the isolated environment through a simple CLI: mdocker run /bin/bash
- Set basic resource limits using cgroups (CPU, memory, pids).
- Re-exec the program to enter child mode and create an isolated process tree.
- Forward the container's stdin, stdout, and stderr to the host terminal.
- Optional Features: Implement basic virtual networking using veth pairs or isolated network namespaces.

**Resources:**
- https://codingchallenges.fyi/challenges/challenge-docker/
- namespaces: https://man7.org/linux/man-pages/man7/namespaces.7.html
- cgroups: https://docs.kernel.org/admin-guide/cgroup-v2.html
- network namespaces: https://www.redhat.com/en/blog/net-namespaces
- golang tutorial for reference: https://www.infoq.com/articles/build-a-container-golang/
- docker-type runtime written in python for reference: https://github.com/tonybaloney/mocker
- python tutorial for reference: https://muhammadraza.me/2024/building-container-runtime-python/
- docker implementation in C for reference: https://github.com/ananthvk/docker-clone

---
# Developing Malware for Real-Time Network Monitoring and Keystroke Capture to Secure User Credentials

**Description:** Design a malware system that captures and monitors user activity on specific websites to secure credentials while remaining undetected by security tools.

**Key Features:**
- Continuously monitor network traffic and send data to a remote server.
- Activate a keylogger to capture input on specific websites (e.g., Instagram, Facebook) and maintain real-time records in a database.
- Design a user-friendly interface for administrators to monitor activities.
- Optional Features: Clearing saved passwords to prompt re-entry, anti-detection mechanisms, and additional innovative features.

**Resources:**
- https://computer.howstuffworks.com/question525.htm
- https://en.wikipedia.org/wiki/Network_packet

Scapy :
- https://scapy.net/
- https://thepacketgeek.com/scapy/

Python socket library :
- https://www.tutorialspoint.com/python/python_networking.htm
- https://docs.python.org/3/library/socket.html

Flask:
- https://flask.palletsprojects.com/en/3.0.x/quickstart/

Network programming:
- https://beej.us/guide/bgnet/

Pynput
- https://pynput.readthedocs.io/en/latest/

Requests – Data Exfiltration to Remote Server
- https://docs.python-requests.org/en/
- https://realpython.com/python-requests/

pygetwindow – Detect Active Window / Target Website
- https://pypi.org/project/pygetwindow/
- https://github.com/asweigart/pygetwindow

---
# End-to-End Encrypted Web-Based File Storage and Sharing Platform

**Description:** Build a secure, browser-accessible platform that allows users to upload, store, organize, and share files while ensuring strong authentication, access control, and encryption. The system must provide a private, secure alternative to traditional cloud storage.

**Key Features:**
- User authentication with MFA code generation.
- Role-based access control for shared folders and files.
- Client-side or server-side encryption for all stored content.
- Secure upload, download, and sharing links.
- Detailed audit logs for all actions.
- NOTE: The MFA code generation algorithm must be written from scratch.

**Resources:**
- Building a Web File Store | Tutorials | Docs https://docs.codecapsules.io/tutorials/building-a-web-file-store
- Multifactor Authentication - GeeksforGeeks https://www.geeksforgeeks.org/computer-networks/multifactor-authentication/
- What are the different ways to implement Multifactor Authentication? https://auth0.com/blog/different-ways-to-implement-multifactor/
- https://cheatsheetseries.owasp.org/cheatsheets/Multifactor_Authentication_Cheat_Sheet.html
- https://dev.to/varshithvhegde/how-i-created-a-file-sharing-website-using-simple-javascript-355g
- https://www.coudo.ai/blog/design-a-secure-file-sharing-system-a-comprehensive-guide
- https://cheatsheetseries.owasp.org/cheatsheets/File_Upload_Cheat_Sheet.html
- https://www.hivenet.com/post/compliance-file-transfer-essential-standards-and-best-practices-for-secure-data-exchange
- https://www.zengrc.com/blog/audit-log-best-practices-for-information-security/
- https://whenderson.dev/blog/two-factor-authentication-from-scratch/

---

# Universal Steganography Tool

**Description:** Develop a universal steganography toolkit capable of automatically identifying file types and applying relevant steganographic detection techniques. The system should unify scanning, embedding, and extraction workflows for images, audio, video, and text.

**Key Features:**
- Automatically detect input file type and apply suitable steganalysis methods.
- Analyze files for hidden information via: LSB inspection, Metadata anomalies, Appended/extra data structures.
- Provide a modular plugin system to add new analysis algorithms.
- Command-line interface or optional graphical interface.
- Enable standard steganography operations like embedding and extraction in supported formats.
- Custom Steganography Detection: Create your method for hiding data in images.

**Resources:**

Scripting
- https://www.w3schools.com/python/
- https://www.shellscript.sh/

Steganography Fundamentals
- https://www.geeksforgeeks.org/image-based-steganography-using-python/
- https://www.geeksforgeeks.org/python/image-based-steganography-using-python/

File Signatures (Magic Numbers)
- https://en.wikipedia.org/wiki/List_of_file_signatures
- https://www.garykessler.net/library/file_sigs.html

*Useful Python Modules*

- Image Manipulation https://pillow.readthedocs.io/en/stable/
- Audio Processing https://docs.python.org/3/library/wave.html
- Binary Data Handling https://www.geeksforgeeks.org/reading-writing-binary-files-in-python/
- CLI Arguments https://docs.python.org/3/library/argparse.html

---

