# COTD 2

We are told that the flag is in one of the subdirectories which are of the form "/dir<num>"

Manually checking these would be painful.

We can write a simple python script to automate this.

```py
import requests

for i in range(101):
    r = requests.get('http://dirs.infosec.org.in/dir' + str(i))
    if "cotd" in r.text:
        print(f"Found in subdir: {i}")
        break
```

It found the flag in /dir66

`cotd{w3_g0t_4_d1rbust3r}`
