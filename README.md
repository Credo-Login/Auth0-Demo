[<img src="https://lh3.googleusercontent.com/proxy/ECLYzIMU0Hq5pCgGw5-DmAj5S8viRe-Yn2gd9J0Bz0xsy2zNWXWo4SfDUA-Qw3wzTOVOZf_iA2qgcZx8rFVmePbVDpfOd4PjNgpnBWVVPiSOJDqT-oM6TGKkYFGQ5LvbTyph13caA42qgjquHV-DRN14ssA" height="35">](https://play.google.com/store/apps/details?id=com.usecredo)

## Use Credo with Auth0

<img src="https://lh3.googleusercontent.com/8e-UAH4Cv8zaymujX8nK03TxFunnLwNnkq0MidEvNkkBKOdznC7-7n8G96ES812Nyw=s180-rw" alt="Credo" height="50" style="background-color:white;padding:5px 10px"/>
<img src="https://cms-assets.tutsplus.com/uploads/users/769/posts/31134/preview_image/auth0.png" height="50" alt="Auht0"/>

##### Live Demo

[Click Here](https://credo-login.github.io/Auth0-Demo/)

---

This tutorial describes how to connect Credo with Auth0 and easily implement passwordless login using Auth0-js and Auth0 Lock.

To follow this demo you will need:

- Pyhton 2 or Python 3
- Optional: Credo Developer Account
- Optional: Auth 0 Account

**Note:** This demo uses Python web server for the sake of simplicity but any other web server can be used.

## Instrutions

### Run this demo (in under 30 seconds!)

It is recommended to firstly run this demo using the default Credo Demo Application, that the code is already configured for. To do so follow the steps:

- Clone this repository

  `git clone https://github.com/Credo-Login/Auth0-Demo.git`

- Navigate to repository folder

  `cd Auth0-Demo`

- Using Python start an HTTP server
  - Python 2 `python -m SimpleHTTPServer 3000`
  - Pyhton 3 `python -m http.server 3000`

* Visit http://localhost:3000

### Obtain client id and client secret

[Contact us.](https://forms.gle/woPHxFCcpSwtKGn4A)

### Configure Auth0 "Custom Social Connections" extension

1. Log into your Auth0 developer account.
2. On the left hand side menu, go to Extensions.
3. Click and activte "Cusomt Social Connection" extension.
4. On the extension page, click the "New Connection" button.
5. Fill in the following details:

- Name: Credo
- Client Id: `<YOUR CREDO APP CLIENT ID>`
- Client Secret: `<YOUR CREDO APP CLIENT SECRET>`
  - Authorization URL: `http://usecredo.com/api/oauth2/authorize` 
  - Token URL: `http://usecredo.com:8000/api/oauth2/token` 
  - Fetch User Profile Script:

```
function(accessToken, ctx, cb) { 
     request.get('http://usecredo.com:8000/user', {
        headers: {
         'Authorization': 'Bearer ' + accessToken.value
        }
     }, function(e, r, b) {
        if (e) return cb(e);
        if (r.statusCode !== 200) return cb(new Error('StatusCode: ' + r.statusCode));
        var profile = JSON.parse(b);
        profile.user_id = profile.id;
        profile.name = profile.username;
        cb(null, profile);
     });
}
```

### Configure AUth0 Application

1. Navigate to Auth0 developer dashboard.
2. On the left hand side menu, go to Applications.
3. Click one of the applications you enabled Credo login for.
4. On the page of the application, under Settings tab scrol down to "Allowed Callback URLs"
5. Add the following URLs (coma separated): `http://localhost:3000/callbackLock.html, http://localhost:3000/callbackAuthjs.html`

### Add your Auth0 Domain and Client Id in the code

1. In the Auth0 application page, under Settings tab, Basic information section, find "Domain" and "Client ID".
2. Copy those into `config.js` of this project.

### Run demo using your application

- Navigate to repository folder

  `cd Auth0-Demo`

- Using Python start an HTTP server
  - Python 2 `python -m SimpleHTTPServer 3000`
  - Pyhton 3 `python -m http.server 3000`

* Visit http://localhost:3000
