# DotNet Development - Setting up local user secrets for solutions
## Overview
This document details how to set up a local environment in Visual Studio 2022, running on a Windows 10, x64 laptop, using a simple function app to use the local secrets function in dotnet.  This is where when you are developing locally you can keep your secrets in a secrets file that is separate from your project and so you don't need to worry about accidentally committing to Github.
Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Session Manager\Environment' -Name "FuncAppCert" -Value "test1234"

In ASP.NET Core applications, you can use the User Secrets feature to store sensitive data during development. User Secrets are stored outside of your project directory and are associated with your user profile. This helps prevent accidental check-ins of sensitive information. You can access these secrets using the IConfiguration interface.

1 - Install the Microsoft.Extensions.Configuration.UserSecrets package if you haven't already:
```powershell
dotnet add package Microsoft.Extensions.Configuration.UserSecrets
```

2 - Enable User Secrets for your project. You can do this via the command line:
```powershell
dotnet user-secrets init
```

This command will initialize User Secrets for your project.

3 - Add your secrets using the dotnet user-secrets set command:
```powershell
dotnet user-secrets set "Sftp:Username" "<yourUserName>"
```

4 - You can access your secrets file from powershell:
```powershell
cd "$env:APPDATA\Microsoft\UserSecrets"
```
5 - If you have more than one project using secrets then you'll see all the directories by issuing:
```powershell
ls
```

5 - Open your secrets file in eg vscode by cd to the relevant directory, the directory may be in a guid like identifier so cd to the <guidId>:
```powershell
cd secretdirectory
code secrets.json
```

6 - Your secrets file should open.

7 - It turns out there is a bug that affects running the secrets.json file within a function app locally see github issue: [Azure Functions Secrets Bug Workaround](https://github.com/Azure/azure-functions-core-tools/issues/2413#issuecomment-1260415071)  In order to then use the secrets.json file.  The code below should work for a basic function app:

```c#
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.Functions.Worker;
using Microsoft.Azure.Functions.Worker.Http;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging;
using System.IO;

public static class Function1
{
    [Function("Function1")]
    public static void Run([HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] HttpRequestData req, FunctionContext context)
    {
        var logger = context.GetLogger("Function1");

        var config = new ConfigurationBuilder()
             .AddConfiguration(GetConfigurationBuilder())
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("secrets.json", optional: true, reloadOnChange: true)
            .AddEnvironmentVariables()
            .Build();

        var secretValue = config["MySecretKey"];
        logger.LogInformation($"Secret Value: {secretValue}");

        // Your function logic here
    }

    //In order to use secrets.json in development
    static IConfiguration GetConfigurationBuilder() =>
    new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddUserSecrets<Program>()
        .AddEnvironmentVariables()
        .Build();
}
```

8 - Now run the code and it should read the value from secrets.json and log it out into the console.  This way you avoid the risk of committing your secret values to github.

