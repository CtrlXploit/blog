# COTD 1

On opening the website we see some information about our username and a random string.

We also know that the flag is in `/app/flag.txt`

<img width="750" height="228" alt="image" src="https://github.com/user-attachments/assets/2a9c2ec3-5266-495f-90d0-f87fe00509ec" />

Something that catches attention is the fact that it is reading from a file with our username and displaying its contents.

Which means, if we somehow control the username, we can possible manipulate the path that is being read from.

The obvious place to look for this is the browser cookies which are also hinted at by the original story as "biscuits"

We can change the cookie to `../../../../app/flag` to get the flag.

`cotd{cr0ssing_p4ths}`
