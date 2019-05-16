---
title: Logboekregistratie in MSAL toepassingen | Azure
description: Meer informatie over het registreren in Microsoft Authentication Library (MSAL)-toepassingen.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/22/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 58f18995d46ca61ae68a7b226bbfc9a286e73a0b
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2019
ms.locfileid: "65544099"
---
# <a name="logging"></a>Logboekregistratie
Apps in Microsoft Authentication Library (MSAL) voor het genereren van logboekberichten waarmee u kunnen problemen vaststellen en vindt u informatie. Een app kunt logboekregistratie configureren met een paar regels code en aangepaste controle over de mate van detail en of de persoonlijke en organisatorische gegevens worden geregistreerd. Het is raadzaam dat u een retouraanroep MSAL logboekregistratie instellen en een manier voor gebruikers bieden om Logboeken te verzenden wanneer ze verificatieproblemen ondervindt.

## <a name="logging-levels"></a>Niveaus voor logboekregistratie

De MSAL logger kan voor verschillende detailniveaus moet vastleggen:

- Fout: Geeft aan dat is iets fout gegaan en is een fout gegenereerd. Gebruik voor foutopsporing en om problemen te identificeren.
- Waarschuwing: Gebeurtenissen van vraag en de app moet u meer informatie op. Er is niet noodzakelijkerwijs is een fout of een fout, maar bedoeld voor diagnose en dicht problemen.
- Info: MSAL wordt logboekgebeurtenissen bestemd voor informatief, niet noodzakelijkerwijs bedoeld voor foutopsporing.
- uitgebreide: Standaard. MSAL worden een grote hoeveelheid gegevens geregistreerd en volledige details geven in welke bibliotheek-gedrag.

## <a name="personal-and-organizational-data"></a>Persoonlijke en organisatie-gegevens
Standaard wordt de logger MSAL zeer gevoelige persoonlijke of zakelijke gegevens niet vastgelegd. De bibliotheek biedt u de optie voor het inschakelen van logboekregistratie van privégegevens en organisatie-gegevens als u besluit om dit te doen.

## <a name="logging-in-msalnet"></a>Logboekregistratie in MSAL.NET
In MSAL 3.x logboekregistratie is ingesteld per toepassing op de app maken met behulp van de `.WithLogging` builder modifier. Deze methode heeft de volgende optionele parameters:

- *Niveau* kunt u beslissen welk niveau van logboekregistratie die u wilt. Instellen op fouten krijgt alleen fouten
- *PiiLoggingEnabled* kunt u persoonlijke en organisatorische gegevens vastleggen als is ingesteld op true. Standaard is dit ingesteld op false, zodat uw toepassing geen persoonlijke gegevens logboekgebeurtenissen worden.
- *LogCallback* is ingesteld op een gemachtigde die door de logboekregistratie worden ondersteund. Als *PiiLoggingEnabled* is ingesteld op true, deze methode wordt twee keer de berichten ontvangen: eenmaal met de *containsPii* parameter gelijk is aan ONWAAR heeft en dat het bericht zonder de persoonlijke gegevens en een tweede keer met de *containsPii* parameter gelijk is aan True en het bericht kan persoonlijke gegevens bevatten. In sommige gevallen (wanneer het bericht geen persoonlijke gegevens bevat), het bericht zijn hetzelfde.
- *DefaultLoggingEnabled* Hiermee schakelt u de standaard logboekregistratie voor het platform. Standaard is deze waarde false. Als u deze op waar het instelt gebruikmaakt van Event Tracing in Desktop/UWP-toepassingen, NSLog op iOS- en logcat op Android.

```csharp
class Program
 {
  private static void Log(LogLevel level, string message, bool containsPii)
  {
     if (containsPii)
     {
        Console.ForegroundColor = ConsoleColor.Red;
     }
     Console.WriteLine($"{level} {message}");
     Console.ResetColor();
  }

  static void Main(string[] args)
  {
    var scopes = new string[] { "User.Read" };

    var application = PublicClientApplicationBuilder.Create("<clientID>")
                      .WithLogging(Log, LogLevel.Info, true)
                      .Build();

    AuthenticationResult result = application.AcquireTokenInteractive(scopes)
                                             .ExecuteAsnc();
  }
 }
 ```

 ## <a name="logging-in-msaljs"></a>Logboekregistratie in MSAL.js

 U kunt logboekregistratie inschakelen in MSAL.js door een logger-object doorgeven tijdens de configuratie voor het maken van een `UserAgentApplication` exemplaar. Dit object logger heeft de volgende eigenschappen:

- *localCallback*: een retouraanroep-instantie die kan worden geboden door de ontwikkelaar te gebruiken en logboeken op een aangepaste manier publiceren. De methode localCallback, afhankelijk van hoe u Logboeken moet omleiden wilt implementeren.

- *niveau* (optioneel): het configureerbare logboek-niveau. De ondersteunde logboekniveaus zijn: Fout, waarschuwing, uitgebreide informatie. Standaardwaarde is Info.

- *piiLoggingEnabled* (optioneel): Hiermee kunt u persoonlijke en organisatorische gegevens vastleggen als is ingesteld op true. Standaard is dit ingesteld op false zodat uw toepassing geen persoonlijke gegevens logboekgebeurtenissen worden. Logboeken met persoonlijke gegevens worden nooit naar de uitvoer standaard als Console, Logcat of NSLog geschreven. Standaard is ingesteld op false.

- *correlationId* (optioneel): een unieke id, die wordt gebruikt voor het toewijzen van de aanvraag met het antwoord voor foutopsporing. De standaardkleur RFC4122 versie 4 guid (128-bits).

```javascript

function loggerCallback(logLevel, message, containsPii) {
   console.log(message);
}

var msalConfig = {
    auth: {
        clientId: “abcd-ef12-gh34-ikkl-ashdjhlhsdg”,
    },
    system: {
        logger: {
            localCallback: loggerCallback,
            level: Msal.LogLevel.Verbose,
            piiLoggingEnabled: false,
            correlationId: '1234'
        }
    }
}

var UserAgentApplication = new Msal.UserAgentApplication(msalConfig);
```
