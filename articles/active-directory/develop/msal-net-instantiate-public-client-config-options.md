---
title: Exemplaar maken van een openbare client-app met opties (Microsoft Authentication Library voor .NET) | Azure
description: Informatie over het instantiëren van een openbare client-toepassing met configuratie-opties met behulp van de Microsoft Authentication Library voor .NET (MSAL.NET).
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/30/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 125bbf9aed54fb00f039aeffddd5cc1aad3360a6
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2019
ms.locfileid: "65544406"
---
# <a name="instantiate-a-public-client-application-with-configuration-options-using-msalnet"></a>Het instantiëren van een openbare client-toepassing met configuratie-opties met behulp van MSAL.NET

In dit artikel wordt beschreven hoe u exemplaar maken van een [openbare clienttoepassing](msal-client-applications.md) met behulp van Microsoft Authentication Library voor .NET (MSAL.NET).  De toepassing wordt gestart met configuratie-opties die zijn gedefinieerd in een bestand met instellingen.

Voordat u het initialiseren van een toepassing, moet u eerst [registreren](quickstart-register-app.md) deze zodat uw app kan worden geïntegreerd met het Microsoft identity-platform. Na de registratie moet u mogelijk de volgende informatie (die kan worden gevonden in de Azure-portal):

- De client-ID (een tekenreeks die een GUID)
- De URL van de id-provider (met de naam van het exemplaar) en de doelgroep aanmelden voor uw toepassing. Deze twee parameters bekend staan als de instantie.
- De tenant-ID als u een line-of-business-toepassing uitsluitend voor uw organisatie (ook met de naam één tenant toepassing ontwikkelt).
- Voor web-apps, en soms voor openbare client-apps (met name wanneer uw app nodig heeft om te gebruiken een broker), hebt u instellen de redirectUri waar de id-provider contact op met back-uw toepassing met de beveiligingstokens.


Een .NET Core-consoletoepassing kan hebben de volgende *appsettings.json* configuratiebestand:

```json
{
  "Authentication": {
    "AzureCloudInstance": "AzurePublic",
    "AadAuthorityAudience": "AzureAdMultipleOrgs",
    "ClientId": "ebe2ab4d-12b3-4446-8480-5c3828d04c50"
  },

  "WebAPI": {
    "MicrosoftGraphBaseEndpoint": "https://graph.microsoft.com"
  }
}
```

De volgende code leest dit bestand met .NET framework-configuratie:

```csharp
public class SampleConfiguration
{
    /// <summary>
    /// Authentication options
    /// </summary>
    public PublicClientApplicationOptions PublicClientApplicationOptions { get; set; }

    /// <summary>
    /// Base URL for Microsoft Graph (it varies depending on whether the application is ran
    /// in Microsoft Azure public clouds or national / sovereign clouds
    /// </summary>
    public string MicrosoftGraphBaseEndpoint { get; set; }

    /// <summary>
    /// Reads the configuration from a json file
    /// </summary>
    /// <param name="path">Path to the configuration json file</param>
    /// <returns>SampleConfiguration as read from the json file</returns>
    public static SampleConfiguration ReadFromJsonFile(string path)
    {
        // .NET configuration
        IConfigurationRoot Configuration;
        var builder = new ConfigurationBuilder()
          .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile(path);
        Configuration = builder.Build();

        // Read the auth and graph endpoint config
        SampleConfiguration config = new SampleConfiguration()
        {
            PublicClientApplicationOptions = new PublicClientApplicationOptions()
        };
        Configuration.Bind("Authentication", config.PublicClientApplicationOptions);
        config.MicrosoftGraphBaseEndpoint = Configuration.GetValue<string>("WebAPI:MicrosoftGraphBaseEndpoint");
        return config;
    }
}
```

De volgende code wordt gemaakt van uw toepassing, met behulp van de configuratie van het bestand met instellingen:

```csharp
SampleConfiguration config = SampleConfiguration.ReadFromJsonFile("appsettings.json");
var app = PublicClientApplicationBuilder.CreateWithApplicationOptions(config.PublicClientApplicationOptions)
           .Build();
```

