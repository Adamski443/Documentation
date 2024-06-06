# Report Engine MFA API And Caching Issues

Double check how to access report engine for current cptadmin and super

### BWUAT - SOAP Service Old Report Engine
Open excel
Agresso Excelerator > Log in
Connection type - U4BW Web Service
Windows Authentication
Web service url eg bwuat.test.co.uk/BusinessWorld-reportengine/Service.asmx

Click log in
Logs in using authenticator

Load current sheet - works as expected
Unload current sheet - works as expected

On the user remove access to report engine related roles (in the old web service these are ones that have access to the report architect menu item)

Click log in
Log in using authenticator

Load current sheet - loads the data (iis has cached the session)
unload current sheet - unloads the data (iis has cached the session)

Go into IIS on web server and recycle the Report Engine app pool

Click log in
Log in using authenticator

Load current sheet - sheet errors as user does not have access to data (iis has beeb cleared so users access has changed)

### BWUAT - New WEB API With OAUTH
User has access to public api for report engine (API-REPENG)

Open excel
Agresso Excelerator > Log in
IDSClient - xxxx-reportengine-pkce
Connection type - ERP 7 Web API
IDS Authentication
Web api url eg https://bwuatxxxx.test.co.uk/BusinessWorld-web-api

Click log in
Takes out to U4IDS to agree to access / mfa challenge
Returns message for user to close window and return to report engine

Load current sheet - works as expected
Unload current sheet - works as expected

On the user remove access to report engine related role

Click log in
Log in using authenticator

Load current sheet - loads the data (iis has cached the session)
unload current sheet - unloads the data (iis has cached the session)

Go into IIS on web server and recycle the Report Engine app pool

Click log in
Log in using authenticator

Load current sheet - sheet errors as user does not have access to data (iis has beeb cleared so users access has changed)