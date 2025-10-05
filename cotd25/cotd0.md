# COTD 0
On opening the provided link, we get a login page.
Since we do not have any credentials, nor any other information, we check the website source code.

There seems a suspicious javascript file
`<script src="/static/script.js"></script>`

On opening the file, we can see that the credentials are hardcoded on the frontend itself
```js
if (username === 'iamgreedy' && password === 'Gr33di5ther00tof4ll3v1l')
```

Loggin in with the creds gives us the flag.
`cotd{4lw4ys_4u7h_0n_s3rv3r_0nly}`
