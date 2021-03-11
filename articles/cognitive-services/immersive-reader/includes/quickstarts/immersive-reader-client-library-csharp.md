---
title: Snabb start för C#-klient bibliotek för intensiv läsare
titleSuffix: Azure Cognitive Services
description: I den här snabb starten skapar du en webbapp från grunden och lägger till API-funktionen för avancerad läsare.
services: cognitive-services
author: nitinme
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: include
ms.date: 09/14/2020
ms.author: nitinme
ms.custom: devx-track-js, devx-track-csharp
ms.openlocfilehash: 442f51759b136200fd61706864568e580265e572
ms.sourcegitcommit: d135e9a267fe26fbb5be98d2b5fd4327d355fe97
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/10/2021
ms.locfileid: "102619259"
---
[Avancerad läsare](https://www.onenote.com/learningtools) är ett särskilt utformat verktyg som implementerar beprövade tekniker för att förbättra Läs förståelse för nya läsare, språkutbildningar och personer med inlärnings skillnader som Dyslexia. Du kan använda avancerad läsare i dina program för att isolera text för att förbättra fokus, Visa bilder för vanliga ord, markera delar av tal, läsa den markerade texten, översätta ord och fraser i real tid och mycket mer.

I den här snabb starten skapar du en webbapp från grunden och integrerar avancerad läsare med klient biblioteket för avancerad läsare. Ett fullständigt fungerande exempel på den här snabb starten finns [på GitHub](https://github.com/microsoft/immersive-reader-sdk/tree/master/js/samples/quickstart-csharp).

## <a name="prerequisites"></a>Förutsättningar

* Azure-prenumeration – [skapa en kostnads fritt](https://azure.microsoft.com/free/cognitive-services)
* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads)
* En fördjupad läsar resurs som kon figurer ATS för Azure Active Directory autentisering. Följ [dessa instruktioner](../../how-to-create-immersive-reader.md) för att konfigurera. Du behöver några av de värden som skapas här när du konfigurerar exempel projekt egenskaperna. Spara utdata från sessionen i en textfil för framtida bruk.

## <a name="create-a-web-app-project"></a>Skapa ett webbappsprojekt

Skapa ett nytt projekt i Visual Studio med hjälp av mallen ASP.NET Core Web App med inbyggd modell-View-Controller och ASP.NET Core 2,1. Ge projektet namnet "QuickstartSampleWebApp".

![Nytt projekt – C #](../../media/quickstart-csharp/1-createproject.png)

![Konfigurera nytt projekt – C #](../../media/quickstart-csharp/2-configureproject.png)

![Ny ASP.NET Core webb program-C #](../../media/quickstart-csharp/3-createmvc.png)

## <a name="set-up-authentication"></a>Konfigurera autentisering

### <a name="configure-authentication-values"></a>Konfigurera värden för autentisering

Högerklicka på projektet i _Solution Explorer_ och välj **hantera användar hemligheter**. Då öppnas en fil med namnet _secrets.jspå_. Den här filen är inte markerad i käll kontrollen. Läs mer [här](/aspnet/core/security/app-secrets?tabs=windows). Ersätt innehållet i _secrets.jspå_ med följande, och ange de värden som anges när du skapade din fördjupade läsare-resurs.

```json
{
  "TenantId": "YOUR_TENANT_ID",
  "ClientId": "YOUR_CLIENT_ID",
  "ClientSecret": "YOUR_CLIENT_SECRET",
  "Subdomain": "YOUR_SUBDOMAIN"
}
```

### <a name="install-active-directory-nuget-package"></a>Installera Active Directory NuGet-paket

I följande kod används objekt från **Microsoft. IdentityModel. clients. ActiveDirectory** NuGet-paketet så du måste lägga till en referens till det paketet i projektet.

Öppna NuGet Package Manager-konsolen från **verktyg-> NuGet Package Manager – > Package Manager-konsolen** och kör följande kommando:

```powershell
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 5.2.0
```

### <a name="update-the-controller-to-acquire-the-token"></a>Uppdatera styrenheten för att hämta token 

Öppna _Controllers\HomeController.cs_ och Lägg till följande kod efter _using_ -instruktionerna överst i filen.

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Nu ska vi konfigurera styrenheten för att hämta Azure AD-värden från _secrets.js_. Lägg till följande kod högst upp i _HomeController_ -klassen ```public class HomeController : Controller {``` .

```csharp
private readonly string TenantId;     // Azure subscription TenantId
private readonly string ClientId;     // Azure AD ApplicationId
private readonly string ClientSecret; // Azure AD Application Service Principal password
private readonly string Subdomain;    // Immersive Reader resource subdomain (resource 'Name' if the resource was created in the Azure portal, or 'CustomSubDomain' option if the resource was created with Azure CLI Powershell. Check the Azure portal for the subdomain on the Endpoint in the resource Overview page, for example, 'https://[SUBDOMAIN].cognitiveservices.azure.com/')

public HomeController(Microsoft.Extensions.Configuration.IConfiguration configuration)
{
    TenantId = configuration["TenantId"];
    ClientId = configuration["ClientId"];
    ClientSecret = configuration["ClientSecret"];
    Subdomain = configuration["Subdomain"];

    if (string.IsNullOrWhiteSpace(TenantId))
    {
        throw new ArgumentNullException("TenantId is null! Did you add that info to secrets.json?");
    }

    if (string.IsNullOrWhiteSpace(ClientId))
    {
        throw new ArgumentNullException("ClientId is null! Did you add that info to secrets.json?");
    }

    if (string.IsNullOrWhiteSpace(ClientSecret))
    {
        throw new ArgumentNullException("ClientSecret is null! Did you add that info to secrets.json?");
    }

    if (string.IsNullOrWhiteSpace(Subdomain))
    {
        throw new ArgumentNullException("Subdomain is null! Did you add that info to secrets.json?");
    }
}

/// <summary>
/// Get an Azure AD authentication token
/// </summary>
private async Task<string> GetTokenAsync()
{
    string authority = $"https://login.windows.net/{TenantId}";
    const string resource = "https://cognitiveservices.azure.com/";

    AuthenticationContext authContext = new AuthenticationContext(authority);
    ClientCredential clientCredential = new ClientCredential(ClientId, ClientSecret);

    AuthenticationResult authResult = await authContext.AcquireTokenAsync(resource, clientCredential);

    return authResult.AccessToken;
}

[HttpGet]
public async Task<JsonResult> GetTokenAndSubdomain()
{
    try
    {
        string tokenResult = await GetTokenAsync();

        return new JsonResult(new { token = tokenResult, subdomain = Subdomain });
    }
    catch (Exception e)
    {
        string message = "Unable to acquire Azure AD token. Check the debugger for more information.";
        Debug.WriteLine(message, e);
        return new JsonResult(new { error = message });
    }
}
```

## <a name="add-sample-content"></a>Lägg till exempel innehåll
Börja med att öppna _Views\Shared\Layout.cshtml_. ```</head>```Lägg till följande kod innan raden:

```html
@RenderSection("Styles", required: false)
```

Nu ska vi lägga till exempel innehåll till den här webbappen. Öppna _Views\Home\Index.cshtml_ och ersätt all automatiskt genererad kod med det här exemplet:

```html
@{
    ViewData["Title"] = "Immersive Reader C# Quickstart";
}

@section Styles {
    <style type="text/css">
        .immersive-reader-button {
            background-color: white;
            margin-top: 5px;
            border: 1px solid black;
            float: right;
        }
    </style>
}

<div class="container">
    <button class="immersive-reader-button" data-button-style="iconAndText" data-locale="en"></button>

    <h1 id="ir-title">About Immersive Reader</h1>
    <div id="ir-content" lang="en-us">
        <p>
            Immersive Reader is a tool that implements proven techniques to improve reading comprehension for emerging readers, language learners, and people with learning differences.
            The Immersive Reader is designed to make reading more accessible for everyone. The Immersive Reader
            <ul>
                <li>
                    Shows content in a minimal reading view
                </li>
                <li>
                    Displays pictures of commonly used words
                </li>
                <li>
                    Highlights nouns, verbs, adjectives, and adverbs
                </li>
                <li>
                    Reads your content out loud to you
                </li>
                <li>
                    Translates your content into another language
                </li>
                <li>
                    Breaks down words into syllables
                </li>
            </ul>
        </p>
        <h3>
            The Immersive Reader is available in many languages.
        </h3>
        <p lang="es-es">
            El Lector inmersivo está disponible en varios idiomas.
        </p>
        <p lang="zh-cn">
            沉浸式阅读器支持许多语言
        </p>
        <p lang="de-de">
            Der plastische Reader ist in vielen Sprachen verfügbar.
        </p>
        <p lang="ar-eg" dir="rtl" style="text-align:right">
            يتوفر \"القارئ الشامل\" في العديد من اللغات.
        </p>
    </div>
</div>
```

Observera att all text har ett **lang** -attribut som beskriver språk i texten. Det här attributet hjälper den fördjupade läsaren att tillhandahålla relevanta språk-och grammatiska funktioner.

## <a name="add-javascript-to-handle-launching-immersive-reader"></a>Lägg till Java Script till hantering starta avancerad läsare

Biblioteket för avancerad läsare innehåller funktioner som att starta den fördjupade läsaren och återge knappar för avancerad läsare. Läs mer [här](../../reference.md).

Lägg till följande kod längst ned i _Views\Home\Index.cshtml_:

```html
@section Scripts
{
    <script src="https://contentstorage.onenote.office.net/onenoteltir/immersivereadersdk/immersive-reader-sdk.1.0.0.js"></script>
    <script>
        function getTokenAndSubdomainAsync() {
            return new Promise(function (resolve, reject) {
                $.ajax({
                    url: "@Url.Action("GetTokenAndSubdomain", "Home")",
                    type: "GET",
                    success: function (data) {
                        if (data.error) {
                            reject(data.error);
                        } else {
                            resolve(data);
                        }
                    },
                    error: function (err) {
                        reject(err);
                    }
                });
            });
        }
    
        $(".immersive-reader-button").click(function () {
            handleLaunchImmersiveReader();
        });
    
        function handleLaunchImmersiveReader() {
            getTokenAndSubdomainAsync()
                .then(function (response) {
                    const token = response["token"];
                    const subdomain = response["subdomain"];
    
                    // Learn more about chunk usage and supported MIME types https://docs.microsoft.com/en-us/azure/cognitive-services/immersive-reader/reference#chunk
                    const data = {
                        title: $("#ir-title").text(),
                        chunks: [{
                            content: $("#ir-content").html(),
                            mimeType: "text/html"
                        }]
                    };
    
                    // Learn more about options https://docs.microsoft.com/en-us/azure/cognitive-services/immersive-reader/reference#options
                    const options = {
                        "onExit": exitCallback,
                        "uiZIndex": 2000
                    };
    
                    ImmersiveReader.launchAsync(token, subdomain, data, options)
                        .catch(function (error) {
                            alert("Error in launching the Immersive Reader. Check the console.");
                            console.log(error);
                        });
                })
                .catch(function (error) {
                    alert("Error in getting the Immersive Reader token and subdomain. Check the console.");
                    console.log(error);
                });
        }
    
        function exitCallback() {
            console.log("This is the callback function. It is executed when the Immersive Reader closes.");
        }
    </script>
}
```

## <a name="build-and-run-the-app"></a>Kompilera och köra appen

I meny raden väljer du **felsök > starta fel sökning** eller trycker på **F5** för att starta programmet.

I webbläsaren bör du se:

![Exempel App-C #](../../media/quickstart-csharp/4-buildapp.png)

## <a name="launch-the-immersive-reader"></a>Starta den fördjupade läsaren

När du klickar på knappen "avancerad läsare" visas den fördjupade läsaren som lanserades med innehållet på sidan.

![Avancerad läsare-C #](../../media/quickstart-csharp/5-viewimmersivereader.png)

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Skapa en resurs och konfigurera AAD](../../how-to-create-immersive-reader.md)