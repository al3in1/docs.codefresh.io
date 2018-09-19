---
title: "Setting Up OpenID Connect Federated Single Sign-On (SSO)"
description: ""
group: enterprise
sub_group: single-sign-on
redirect_from:
  - /docs/sso/sso-setup-oauth2/
toc: true
---

Codefresh natively supports login using Github, Bitbucket and Gitlab using OpenID Connect (OAUTH 2.0) protocol. This guide will review how to add additional SSO integrations based on OAUTH 2.0 as part of Codefresh Enterprise plan.

  
## Prerequisites

In order to add successfully an identity provider in Codefresh you need to do some preparatory work with both Codefresh and the provider.

1. You need to inform your Identify provider that it will provide SSO services to Codefresh
1. You need to setup Codefresh and point it to your identity provider.

The first procedure differs according to you Identity provider, but the second one is common for all providers.

Note that SSO is only available to Enterprise customers. Please [contact sales](https://codefresh.io/contact-sales/) in order to enable it for your Codefresh account.


## Identity provider options

Codefresh currently supports

  * Azure 
  * Okta
  * Auth0

You can setup each provider

 1. At the [Codefresh customer level]({{ site.baseurl }}/docs/enterprise/ent-account-mng/) 
 1. At the Codefresh account level 
 1. At both levels. Integrations that were created from the customer level can only be edited or removed by the customer administrator from that customer management view. The Account administrator won’t be able to edit those.

The specific way depends on your own organization and how you have chosen to give Codefresh access to your users.

To access the SSO configuration at the account level.

1.  Click on your avatar at the top right of the GUI and select *Account settings*
1. In the new screen, select *Single Sign-on* from the left sidebar

To access the SSO configuration at the customer level

1. Click on your avatar at the top right of the GUI and select any customer from the *Customers* subsection
1. In the new screen, select *Single Sign-on* from the left sidebar

In both cases you will arrive to the following screen

{% include image.html 
lightbox="true" 
file="/images/enterprise/sso/add-sso-dropdown.png" 
url="/images/enterprise/sso/add-sso-dropdown.png"
alt="SSO provider settings"
caption="SSO provider settings"
max-width="70%"
%}

To connect an identity provider click the *add single-sign-on* button and select your provider from the drop-down menu.


## Codefresh SSO setup

Regardless of the Identity provider that you have chosen, the Codefresh setup is the similar for all of them. You need to provide several fields to Codefresh to activate SSO. The common ones are:

* *Display Name* - A name for your Identity provider 
* *Client ID* - An ID that will be used for the connection
* *Client Secret* - A secret associated with the ID

Some providers also need
additional fields which are specific to that provider. 

The process to obtain the values for these fields depends on the individual Identity provider. In the following
sections we will outline the details for each one.


### Setting Azure as an Identity provider

To setup Azure Active Directory for SSO

1. Create a new application in Azure AD

Login to *Microsoft Azure* and choose *Azure Active Directory* from the sidebar.

{% include image.html 
lightbox="true" 
file="/images/enterprise/sso/azure/azure-step01.png" 
url="/images/enterprise/sso/azure/azure-step01.png"
alt="Azure Active Directory"
caption="Azure Active Directory"
max-width="70%"
%}

Then under *MANAGE*, select *App registrations*.

{% include image.html 
lightbox="true" 
file="/images/enterprise/sso/azure/azure-step02.png" 
url="/images/enterprise/sso/azure/azure-step02.png"
alt="Azure App Registrations"
caption="Azure App Registrations"
max-width="70%"
%}

Then click on the *+ ADD button* to add a new application.

Enter a name for the application (e.g. *Codefresh*), select *Web app/API* as the Application Type, and for Sign-on URL enter `https://g.codefresh.io`

{% include image.html 
lightbox="true" 
file="/images/enterprise/sso/azure/azure-step03.png" 
url="/images/enterprise/sso/azure/azure-step03.png"
alt="Azure App Registration create"
caption="Azure App Registration create"
max-width="70%"
%}


{:start="2"}
1. Configure the permissions

Once the application has been created, you will have to configure the permissions. Click on the name of the application to open the *Settings* section.

Click *Required permissions*.

{% include image.html 
lightbox="true" 
file="/images/enterprise/sso/azure/azure-step04.png" 
url="/images/enterprise/sso/azure/azure-step04.png"
alt="Azure App Permissions"
caption="Azure App Permissions"
max-width="70%"
%}

Then click on *Windows Azure Active Directory* to change the access levels.


{% include image.html 
lightbox="true" 
file="/images/enterprise/sso/azure/azure-step05.png" 
url="/images/enterprise/sso/azure/azure-step05.png"
alt="Azure App Change Permissions"
caption="Azure App Change Permissions"
max-width="70%"
%}

The next step is to modify permissions for the app. Under *DELEGATED PERMISSIONS* check next to *Sign in and read user profile* and *Read directory data*. Finally click the *Save* button.

{:start="3"}
1. Create the key

Next you will need to create a key which will be used as the Client Secret in Codefresh connection. Click on *Keys* from the *Settings* menu.

{% include image.html 
lightbox="true" 
file="/images/enterprise/sso/azure/azure-step06.png" 
url="/images/enterprise/sso/azure/azure-step06.png"
alt="Change keys"
caption="Change keys"
max-width="70%"
%}

Enter a name for the key and choose the desired duration.

**Note:**. If you choose an expiring key, make sure to record the expiration date in your calendar, as you will need to renew the key (get a new one) before that day in order to ensure users don't experience a service interruption.

{% include image.html 
lightbox="true" 
file="/images/enterprise/sso/azure/azure-step07.png" 
url="/images/enterprise/sso/azure/azure-step07.png"
alt="Create key"
caption="Create key"
max-width="70%"
%}

Click on *Save* and the key will be displayed. **Make sure to copy the value of this key before leaving this screen**, otherwise you may need to create a new key. This value will need to be provided to Codefresh securely.

{% include image.html 
lightbox="true" 
file="/images/enterprise/sso/azure/azure-step08.png" 
url="/images/enterprise/sso/azure/azure-step08.png"
alt="Create key"
caption="Create key"
max-width="70%"
%}


{:start="4"}
1. Go back to the SSO settings screen described in the first part of this guide inside the Codefresh GUI.

You need to enter the following:

* *Display Name* - Shown as display name in Azure
* *client id* - your Azure Application ID (see below)
* *client secret* - the key from step 3
* *tenant* - `<Your Microsoft Azure AD Domain>.onmicrosoft.com`
* *Object ID* - your Azure Object ID (see below)


{% include image.html 
lightbox="true" 
file="/images/enterprise/sso/azure/azure-step10.png" 
url="/images/enterprise/sso/azure/azure-step10.png"
alt="Application ID"
caption="Application ID"
max-width="70%"
%}

Once you save the Identity provider, Codefresh will assign a `client-name` to it which identifies the SSO configuration.

{% include image.html 
lightbox="true" 
file="/images/enterprise/sso/azure/client-name.png" 
url="/images/enterprise/sso/azure/client-name.png"
alt="SSO Client Name"
caption="SSO Client Name"
max-width="50%"
%}

{:start="5"}
1. Configure reply URLs

As a last step you need to ensure that your Codefresh callback URL is listed in allowed reply URLs for the created application. Navigate to *Azure Active Directory* -> *Apps registrations* and select your app. Then click *Settings -> Reply URLs* and add:

```
https://g.codefresh.io/api/auth/<your_codefresh_sso_client_name>/callback

```

where `<your_codefresh_sso_client_name>` is the client name shown in the SSO configuration.

{% include image.html 
lightbox="true" 
file="/images/enterprise/sso/azure/azure-step09.png" 
url="/images/enterprise/sso/azure/azure-step09.png"
alt="Reply URLs"
caption="Reply URLs"
max-width="70%"
%}

This concludes the SSO setup for Azure. See the following sections on how to test the integration, activate SSO for collaborators and create sync jobs.

### Setting Okta as an Identity provider

1. Log in to your Okta account. If you don't already have one, you will need to create one.

On the general Okta dashboard, click *Admin*. This takes you to the Okta Admin Dashboard.

{% include image.html 
lightbox="true" 
file="/images/enterprise/sso/okta/image5.png" 
url="/images/enterprise/sso/okta/image5.png"
alt="Okta Dashboard"
caption="Okta Dashboard"
max-width="70%"
%}

Using the list of shortcuts at the right-hand side of the screen, click *Add Applications*.

{% include image.html 
lightbox="true" 
file="/images/enterprise/sso/okta/image8.png" 
url="/images/enterprise/sso/okta/image8.png"
alt="Okta Applications"
caption="Okta Applications"
max-width="70%"
%}

On the *Add Application* page, select *Create New App*.

{% include image.html 
lightbox="true" 
file="/images/enterprise/sso/okta/image9.png" 
url="/images/enterprise/sso/okta/image9.png"
alt="Create new application"
caption="Create new application"
max-width="70%"
%}

On the *Create a New Application Integration* pop-up window, select Web as the Platform for Codefresh application, and choose OpenID Connect as the *Sign on method*. Click Create to proceed.

{% include image.html 
lightbox="true" 
file="/images/enterprise/sso/okta/image1.png" 
url="/images/enterprise/sso/okta/image1.png"
alt="Choose Sign-on method"
caption="Choose Sign-on method"
max-width="70%"
%}

{:start="2"}
1. You will now create your OIDC integration. On the *General Settings* page, provide the following:

* App name (e.g. Codefresh)
* App logo (optional). Feel free to download and add this [picture]({{site.baseurl}}/images/enterprise/sso/okta/codefresh-logo.png)
* Login redirect URI: `https://g.codefresh.io/api/auth/<your_codefresh_client_name>/callback` you’ll be able to extract your codefresh client name a bit later in the process so we’ll need to come back to this and update it again - for now please use a temp value such as `https://g.codefresh.io/api/auth/temp/callback`


{% include image.html 
lightbox="true" 
file="/images/enterprise/sso/okta/image4.png" 
url="/images/enterprise/sso/okta/image4.png"
alt="OpenID integration"
caption="OpenID integration"
max-width="70%"
%}

Click *Save* to proceed.

{:start="3"}
1. Go back to the SSO settings screen described in the first part of this guide inside the Codefresh GUI.

You need to enter the following:

* *Display Name* - Shown as application name in OKTA
* *client id* - your OKTA application client ID (see below)
* client secret* - your OKTA application client secret (see below)
* Client Host - your OKTA organisation url (e.g `https://<company>.okta.com`). Keep in mind you don’t copy it from the admin view (e.g. `https://<company>-admin.okta.com`) because it’ll not work.
* *Access Token* (optional) - OKTA API token that will be used to sync groups and users from OKTA to Codefresh. The token can be generated in OKTA by going to the security tab->API (see below)
* *App ID* (optional) - your Codefresh application ID in your OKTA organization that will be used to sync groups and users from OKTA to Codefresh. This ID can be taken by navigating to your Codefresh APP in OKTA and copy it from the url (see below)

{% include image.html 
lightbox="true" 
file="/images/enterprise/sso/okta/image7.png" 
url="/images/enterprise/sso/okta/image7.png"
alt="Client ID and secret"
caption="Client ID and secret"
max-width="70%"
%}

{% include image.html 
lightbox="true" 
file="/images/enterprise/sso/okta/image2.png" 
url="/images/enterprise/sso/okta/image2.png"
alt="Access token"
caption="Access token"
max-width="70%"
%}

{% include image.html 
lightbox="true" 
file="/images/enterprise/sso/okta/image3.png" 
url="/images/enterprise/sso/okta/image3.png"
alt="App ID"
caption="App ID"
max-width="70%"
%}

{:start="4"}
1. Once you save the Identity provider, Codefresh will assign a client-name to it which identifies the SSO configuration.
Note it down.

{% include image.html 
lightbox="true" 
file="/images/enterprise/sso/okta/image6.png" 
url="/images/enterprise/sso/okta/image6.png"
alt="Client name"
caption="Client name"
max-width="50%"
%}

{:start="5"}
1. Go Back to your OKTA Application General Settings and update the following 2 configurations with the client name generated by Codefresh:

* Login redirect URIs - `https://g.codefresh.io/api/auth/<your_codefresh_client_name>/callback`
* Initiate login URI - `https://g.codefresh.io/api/auth/okta?client=<your_codefresh_client_name>`

This concludes the SSO setup for Okta. See the following sections on how to test the integration, activate SSO for collaborators and create sync jobs.


### Setting Auth0 as an Identity provider

Coming soon...




## Testing your Identity provider

Once you setup the Identity provider do the following

1. Go to the collaborators screen by clicking on *Collaborators* on the left sidebar
1. Add an active user that will be used for testing. We recommend you use your own user
 


{% include image.html 
lightbox="true" 
file="/images/enterprise/sso/collaborators.png" 
url="/images/enterprise/sso/collaborators.png"
alt="Adding collaborators"
caption="Adding collaborators"
max-width="70%"
%}


{:start="3"}
1. Keep the current browser session open, and login via Corporate SSO in an incognito tab (or another browser).



{% include image.html 
lightbox="true" 
file="/images/enterprise/sso/sign-with-sso.png" 
url="/images/enterprise/sso/sign-with-sso.png"
alt="Sign-in with SSO"
caption="Sign-in with SSO"
max-width="50%"
%}

{:start="4"}
1. If everything works ok add more users

## Selecting SSO method for collaborators

To add users and select their SSO method, go to *Collaborators* from the left sidebar. Then add the email or Codefresh username of a user. 

In addition to their role you can now select the SSO method they will use


{% include image.html 
lightbox="true" 
file="/images/enterprise/sso/select-user-sso.png" 
url="/images/enterprise/sso/select-user-sso.png"
alt="Selecting SSO method"
caption="Selecting SSO method"
max-width="50%"
%}

>Notice that users that are added either manually or via synchronization (described in the next section) are by default **NOT set** to login via SSO. Remember to select the SSO method for each one.

It possible to use a different SSO method for each user (if you have multiple SSO configurations). 


## Syncing of teams after initial SSO setup

Once the initial setup is done, you can also sync your teams between Codefresh and the Identity provider.
You can do this via the [Codefresh Cli](https://codefresh-io.github.io/cli/) and specifically the [sync command](https://codefresh-io.github.io/cli/teams/synchronize-teams/).

For example to sync you azure teams you can execute

```
codefresh synchronize teams my-client-name -t azure

```

You can find the client-name from the SSO UI.

{% include image.html 
lightbox="true" 
file="/images/enterprise/sso/azure/client-name.png" 
url="/images/enterprise/sso/azure/client-name.png"
alt="SSO Client Name"
caption="SSO Client Name"
max-width="50%"
%}

Even though you can run this command manually it makes more sense to run it periodically as a job. And the obvious
way to perform this, is with a Codefresh pipeline. The CLI can be used as a [freestyle step]({{site.baseurl}}/docs/codefresh-yaml/steps/freestyle/).

You can create a git repository with a [codefresh.yml]({{site.baseurl}}/docs/codefresh-yaml/what-is-the-codefresh-yaml/) file with the following contents:

`YAML`
{% highlight yaml %}
{% raw %}
version: '1.0'
steps:
  syncMyTeams:
    title: syncTeams
    image: codefresh/cli
    commands:
      - 'codefresh synchronize teams my-client-name -t azure'
    when:
      branch:
        only:
          - master
{% endraw %}
{% endhighlight %}

To fully automate this pipeline you should set a [cron trigger]({{site.baseurl}}/docs/configure-ci-cd-pipeline/triggers/cron-triggers/) for this pipeline. The cron-trigger will be responsible for running this pipeline (and therefore synchronizing the teams) in a fully automated manner.

This way you can synchronize your teams every day/week/hour depending on you cron trigger setup.
