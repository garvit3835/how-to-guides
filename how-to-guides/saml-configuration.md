# SAML Configuration

All of Aviator’s cloud accounts support Google SSO based login. Self-hosted Aviator deployments support Google, Okta, Active Directory, or other SAML SSO providers.

## Google SSO for onprem

To setup Google SSO for on-premise installation, you will need to create Oauth authorization credentials in the Google developer console to identify the application to Google's OAuth 2.0 server.

1. Go to the [<mark style="color:blue;">Developer Credentials page</mark>](https://console.developers.google.com/apis/credentials).
2. Click **Create credentials > OAuth client ID**.
3. Select the **Web application** application type.
4. Name your OAuth 2.0 client and add the Javascript origins and redirect urls replacing the following with your domain.

![](<../.gitbook/assets/Screen Shot 2022-05-10 at 11.28.34 AM.png>)

5\. Add the Google Client ID and Client Secret that is on this page to your docker `.env`  file.

```shell
GOOGLE_CLIENT_ID={YOUR_GOOGLE_CLIENT_ID}
GOOGLE_CLIENT_SECRET={YOUR_GOOGLE_CLIENT_SECRET}
```

Restart the server, and Google SSO should work.

## SAML 2.0

Aviator supports SAML 2.0 based authentication. To request SAML authentication for your account, please contact [<mark style="color:blue;">howto@aviator.co</mark>](mailto:howto@aviator.co). See instructions below for Okta. If you have any other identity provider, please contact us for instructions.

### Note for onprem users

Please replace _app.aviator.co_ with with **aviator.yourdomain.com** in the instructions below.

### Okta setup

1. Sign into Okta as an administrator.
2. Switch to the **Classic UI**, using the drop-down in the upper left.
3. Go to Admin Dashboard > Applications > **Add Application**.
4. Click **Create New App** and choose **SAML 2.0** as the Sign on method.
5. Enter **General Settings** for the application:
   * App name: **Aviator**
   * **App logo** (optional). You can download the application logo for the application, you can download one from [here](https://mergequeue.com/static/img/merge.png). \[INSERT NEW LOGO]
6. Log into Aviator and go to SAML configuration page: [<mark style="color:blue;">https://app.aviator.co/saml/okta/configure</mark>](https://app.aviator.co/saml/okta/configure)<mark style="color:blue;"></mark>
7. Copy the unique **Single Sign on url**, of format: [**https://app.aviator.co/saml/sso/**](https://aviator.yourdomain.com/saml/sso/)**\<sso-key>**

![identity provider setup](<../.gitbook/assets/Screen Shot 2023-02-09 at 10.25.39 AM.png>)

8\. Enter SAML Settings, including:

* Single sign on URL: enter the URL you copied in Step 6
* Audience URI: `mergequeue`
* Default Relay state: \<leave empty>
* Name ID format: `EmailAddress`
* Application username: `Email`

9\. Enter the attribute statements, which will be used to map attributes between Okta and Aviator. Please note that these values are case-sensitive.

![](<../.gitbook/assets/Screen Shot 2022-05-10 at 12.07.08 PM.png>)

10\. Click **Next**. Then, set Okta support parameters for the application. Recommended settings:

* I’m an Okta customer adding an internal app
* This is an internal app that we have created.

11\. Click **Finish**. On the next screen, click the Sign On tab and click on **Identity Provider metadata.**

![](<../.gitbook/assets/Screen Shot 2022-05-10 at 12.08.48 PM.png>)

12\. Copy the url that it opens, this is your **Metadata URL**.

13\. Go to the **Assignments** tab, and assign the app to the appropriate groups / users to access.

14\. Go back to the SAML configuration page and update the following properties: [https://app.aviator.co/saml/okta/configure](https://app.aviator.co/saml/okta/configure)

* **Metadata url**: Paste the Metadata URL copied from step 11
* **Email domains to allow**: enter your company email domain, e.g. [**example.com**](http://example.com)
* **Click Save and Activate**

This should enable the Okta configuration for your organization. Please verify this by logging out and logging in directly from Okta portal.

Notes:

* This is idp initiated authentication, so you can login in directly from the Okta portal.
* It’s also recommended to post an announcement for your users to explain how the migration will work.

Contact: [<mark style="color:blue;">support@aviator.co</mark>](mailto:support@aviator.co) if you have any issues with the setup.
