# Secure ASP.NET Core API using Azure AD Authentication ( OAuth 2 - Implicit flow) - Step by step guide

## Agenda
In this example we will create a simple Web API using ASP.NET Core 3.1.  In order to secure this API, we will use Azure Active Directory ( Azure AD)

After creating Web API, I will show you how to access this API using client app - `POSTMAN` 

## Create API Project - VS-2019 Project Template <a name="example"></a>

- Open VS 2019
- Click on 'Create New Project'
- Select `ASP.NET Core Web Application` and click on `Next`
- Enter project name - `SecuredWebApi`  and click on `Create`
- On `Create a new ASP.NET Core web application` page 
  - Select framework as `.NET Core` ( It is Default )
  - Select version as `ASP.NET Core 3.1`
  - Select `API` project template, which is a template for creating an ASP.NET Core application with an example Controller for a RESTful HTTP service.
  - Click on - Change Authentication and select `Work or School Accounts`
  - Select appropriate domain based on your Azure account.  In this example I have selected domain as `digitaltechconsultinggmail.onmicrosoft.com`.  Click on OK
- Verify all the settings and click on `Create`
> Behind the scenes VS will register this WebApi application in Azure Active directory.  We will delete this application and re-create it from the scratch so that you know what is exactly happing behind the scenes.  In this demo I have kept `Docker Support` for this WebApi, this is completely optional and you can ignore if `docker` is not installed on your machine.

## Azure AD configuration for WebApi

- Now it is time to create an application in Azure AD that represents our API.
- In Azure Portal, Navigate to your default Active Directory
- Click on `New registration`
- Give the name as `SecureWebApi`
- Keep rest of the things default
> Note that, we don't need to configure Redirect URI for this API.

- Now click on `Register`
- Next, we want to expose this API
- Click on `Èxpose an API` and `Add a scope`
- Portal will prompt for `Application ID URI`, keep the default value and click on `Save and continue`
- Now you need to provide some configuration values create a scope.  
  - Scope Name -  `read`
  - Who can consent? - `Admin and Users`
  - Admin consent display name - `Read API access`
  - Admin consent description - `The application will be able to read data.`
  - State - `Enabled`
  - Finally click on `Add Scope`
  - Similarly create one more scope named `writeaccess`
  > At this stage we have our Azure AD application ready that represents our WebAPI.  Now its time to copy this application/client id and update our `Codebase`
- Navigate to `Overview` tab of `SecureWebApi` and copy GUID labelled as `Application (client) ID`
- Open our .Net Core solution and navigate to `appsettings.json` file and replace the `clientId` with the copied `Application (client) ID`

> TBD add code 

- Now our API project is ready.  Run the project and its ready to serve the requests to the Authenticated clients.

## Azure AD configuration for `POSTMAN`
  - Now it is time to create an application in Azure AD that represents our client ( `POSTMAN` ).
  - In Azure Portal, Navigate to your default Active Directory
  - Click on `New registration`
  - Give the name as `postmanapp`
  - Provide valid redirect URI - `https://postmant/this/can/be/anything`
  > We need to configure Redirect URI for this application. Value of this URI can be any valid URI starting with `https`  We will need this URI value while fetching the token from the `POSTMAN` UI.
  
  - Click on `Authentication` and we will configure few settings to enable `OAUTH 2 implicit` flow
  - Check `Access tokens` and set `Default client type` as `Public client`
  - Click on `Register` button
  - Now it is time to provide this application ( `POSTMAN` application)  to access our `SecureWebApi` 
  - Copy `Application (client) ID` of the `postmanapp`.  We will need this in next step
  - Go back to the registered application `SecureWebApi` on Azure Portal
  - Click on `Expose an API`
  - Click on `Add a client application`
  - Now past the `postmant` client id ( which we have copied in previous step )
  - Now we can decide which scope we want to authorize to this application
  - In out example we will allow both the scopes
    - `api://27a378bd-6725-4ee5-86e6-59cf7142374d/writeaccess`
    - `api://27a378bd-6725-4ee5-86e6-59cf7142374d/read`
  - Now click on `Add application`

> At this stage `postmanapp` can access our `SecureWebApi` through `Delegated` permission named `readaccess` and `writeaccess` 

Now we are ready to use this setup and our client (`postmanapp`) will be able to retrieve access token form identity server ( i.e. our Azure AD) which can be used to access `SecureWebApi`

## Request access token using `POSTMAN` UI
- Open postman UI.  You can down load it from [here]([https://link](https://www.postman.com/downloads/)) if you don't have it.
- Open a new tab to compose new GET request
- 
