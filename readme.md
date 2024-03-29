# **Web API Lab: Access Control - Auth0 Setup**
*Enda Lee 2022*


## Introduction

In this lab you will add access control based on user accounts. Users will be assigned roles which depending on their level of access, e.g., administrator,
manager, and customer.

### Access control involves two stages:

1.  **`Authentication`** – verify the identity of a user.

2.  **`Authorization`** – verify that the user has permission to access something.

All users will be required to provide their `username` and `password` to login.

While we could build all the required functionality from scratch or based on a library such as Passport (**<http://www.passportjs.org/>**), instead, we will use a third-party `Identity Management Service`.

Access control is very difficult to get right, and small mistake can lead to big security vulnerabilities. Using a specialised third-party service to manage identity, including user registration, login, password reset, authorization, MFA, SSO, etc. will greatly enhance the application security.

There are many services including Azure Active, Directory, AWS IAM, etc. and most provide a ‘free tier.’ We will be using **`Auth0`** as it is relatively easy to set up and most of its features are available as part of a free developer’s account. 

To register your account (required for this lab) see: **<https://auth0.com/signup>**

Auth0 will issue a signed `JWT` (token) to the application, after successful authentication. The  `token` will contain that user’s privileges for the application (roles and scope). The app can then verify the integrity of the token using Auth0’s `public key`.

![Application Diagram](./media/4fd050230f487cd178f14a1d05f7f4f4.png)

## Setting up users and apps in Auth0

To use Auth0, the application (both client and server side) must be registered as apps. User roles and permissions also need to be defined and assigned. A token obtained by the client app will passed to the API which can verify it with re-authenticating the user. For this to work the application(s) must be configured for single sign on.

### The API (server-side)

This is the server-side of the app. To configure, open the Applications menu in the Auth0 dashboard and then choose the API sub option:
![](media/d1e08d0429433fae623fd980b5e764ea.png)

Then add a new API application
![](media/9458f8705f50bcde7fbc6ab4d5adc494.png)

Give your new API a `name` and an `identifier` – a URL is recommended but it will never be used publicly so you can enter anything here.

The `signing key` will be used to `verify` tokens, the default is to use public key and the `RS256` algorithm.
![new api](./media/a995f56d1b839347dad2527bd342a596.png)

#### Click create to register the new application.

*Now open settings for the new API*, e.g.
![product-api](./media/02855d7a4791f6b4cefe24b4215b8533.png)

Later, some these settings will be used to by the application to communicate with Auth0 (e.g. for checking a user’s credentials). The Quick Start menu includes the settings required for some web platforms including Node.js

Further down the settings page:

1.  Enable Role Based Access Control (RBAC).

2.  Enable Add Permissions in the Access Token.

3.  Disable Allow Skipping User Consent.

These options enable the API to use Role based access control based on credentials included in a token.

Save the updated settings.

*Next open the Permissions page* (see links at top). Here you will add permissions (also called `scopes`) which can be assigned to users of this API. **Add scopes for each of the database CRUD functions: (note the format, e.g. `read:products`)**
![](media/7c29fac913f7cbed6da6da63892df4e6.png)

### Auth0 configuration for the client-side web application

The client web app also needs to be configured. Users will be redirected to Auth0 for authentication. If successful, id and access tokens will be returned to the web app.

Go back to the Applications menu and choose Applications
![](media/cd31ff9e6a24574e2bdc3a537a89d241.png)

The add a new application named Products Website, choosing the Single Page Web App type and create:
![](media/ceaed116af4aed25b9378b3c7c604677.png)

After its added, open the new application to view its settings, the configure the Application URIs and save changes. The client app will be hosted the on
**<http://localhost:5173>**
![](media/91fab7a2c7e4a2d3656461d31e56a767.png)

### Auth0 client web app configuration continued…

After saving click the Show Advanced Settings link, located just above the save button. In the OAuth section, enable the **OIDC Conformant** option. This will ensure conformance to the Open ID Connect specification for authentication and authorization ensuring that the token can be passed between the two (client and server) applications.
![](media/b78283be9b16d40c5511875cf90a60af.png)

Sources of users for the application can be set in Connections – ensure that Database is enabled.
![Products Website](media/a09022eb257fc1b43cc5fdaa7bcab530.png)

### The Logout URL

Now add **<http://localhost:5173>** to the list of allowed logout URLs. This is where the user will be redirected after logging out (typically a home page or sign in page):

Open the Tenant Settings (main account settings) using the link in the left menu (or direct link replace **elee-tudublin** with your account name:
[https://manage.auth0.com/dashboard/eu/**elee-tudublin**/tenant/advanced](https://manage.auth0.com/dashboard/eu/elee-tudublin/tenant/advanced))

Then add the URL and save

![Tenant Settings](media/dca4bb2ca1af5d73b73b8d3f08690a06.png)

### At this point, the applications are configured:

API Settings:
![API App settings](media/bcb8fb7eccc040e4984e3e578e934583.png)

Client Web App Settings:
![Client App Settings](media/0ca6c2ef45ca510829f8048e4efef7d3.png)

## 

## Users and Roles

Now, add some users and roles for testing later. Choose Roles from the User Management menu. Then create three roles, Admin, Customer, and Manager.
![Roles](media/fa16e944d2c29e76cc389aca9908a952.png)

### Set permissions for each role

For each role, in turn, click the 3 dots menu, the view details. Then open Permissions and click Add Permissions.
![Admin Role Permissions](media/f0c3c0be60872c331c6f3b0deb1f31e4.png)

Choose the Product API, and select permissions (from the ones added in the API earlier). Tick all of the Scope options for the Admin Role:
![Permissions](media/57bfc83410e3ca18632d8e084dd81d00.png)

For Manager, add all except delete, and for customer add only read permissions.

### Add Users and assign roles.

Users will be able to register later but for now add some users for testing purposes.

Choose Users from the User Management Menu
![](media/45e778904c73413b66857fb44c423441.png)

Then create the three users below:
![User Management](media/ee343e15656cf7a84347db3efdbc574c.png)

After creating, use the … menu to assign a role to each (based on username). 

That completes the Auth0 configuration.

## Exercise

Explore the other Auth0 features, including token validity period (lifetime), password policy, and Multi-factor Auth.
![](media/af1e21d3eb5e43ef143f135dc6f416b9.png)
