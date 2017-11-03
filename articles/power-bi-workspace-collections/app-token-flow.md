---
title: Verificatie en autorisatie bij Power BI werkruimte verzamelingen | Microsoft Docs
description: Verificatie en autorisatie bij Power BI werkruimte verzamelingen.
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ROBOTS: NOINDEX
ms.assetid: 1c1369ea-7dfd-4b6e-978b-8f78908fd6f6
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 09/20/2017
ms.author: asaxton
ms.openlocfilehash: ae9627c6bb5e7bb099598acaa2eb29375c35593e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="authenticating-and-authorizing-with-power-bi-workspace-collections"></a>Verificatie en autorisatie bij Power BI werkruimte verzamelingen

Power BI werkruimte verzamelingen gebruik **sleutels** en **App-Tokens** voor verificatie en autorisatie in plaats van expliciete eindgebruiker verificatie. In dit model wordt beheerd door uw toepassing verificatie en autorisatie voor uw eindgebruikers. Indien nodig, wordt uw app maakt en verzendt het app-tokens voor onze service voor het weergeven van het gevraagde rapport. Dit ontwerp hoeft niet uw app met Azure Active Directory voor verificatie en autorisatie, hoewel u nog steeds kunt.

> [!IMPORTANT]
> Power BI Workspace Collections is afgeschaft en is beschikbaar tot juni 2018 of tot de datum die in uw contract wordt aangegeven. Om onderbreking van uw toepassing te voorkomen, wordt u geadviseerd om een migratie naar Power BI Embedded te plannen. Voor meer informatie over het migreren van gegevens naar Power BI Embedded raadpleegt u [How to migrate Power BI Workspace Collections content to Power BI Embedded](https://powerbi.microsoft.com/documentation/powerbi-developer-migrate-from-powerbi-embedded/) (Inhoud van Power BI-werkruimteverzamelingen migreren naar Power BI Embedded).

## <a name="two-ways-to-authenticate"></a>Twee manieren om te verifiëren

**Sleutel** -kunt u sleutels gebruiken voor alle Power BI werkruimte verzamelingen REST API-aanroepen. De sleutels kunnen worden gevonden in de **Microsoft Azure-portal** door te selecteren **alle instellingen** en vervolgens **toegangssleutels**. Uw sleutel altijd behandeld alsof deze een wachtwoord zijn. Deze sleutels hebben machtigingen voor alle REST-API niet aanroepen voor een bepaalde werkruimteverzameling maken.

Voor informatie over het gebruik van een sleutel op een REST-aanroep toevoegen de volgende autorisatie-header:

    Authorization: AppKey {your key}

**App-token** -App-tokens worden gebruikt voor alle insluiten aanvragen. Ze zijn ontworpen om te worden uitgevoerd aan de clientzijde. Het token is beperkt tot één rapport en de beste praktijken een verlooptijd instellen.

App-tokens zijn een JWT (JSON Web Token) die is ondertekend door een van uw sleutels.

Uw app-token kan de volgende claims bevatten:

| Claim | Beschrijving |
| --- | --- |
| **ver** |De versie van het app-token. 0.2.0 is de huidige versie. |
| **AUD** |De beoogde ontvanger van het token. Voor Power BI werkruimte verzamelingen gebruikt: "https://analysis.windows.net/powerbi/api." |
| **ISS** |Een tekenreeks die aangeeft van de toepassing die het token heeft uitgegeven. |
| **type** |Het type van app-token wordt gemaakt. Huidige aantal is het enige ondersteunde type **insluiten**. |
| **draadloze** |Naam van de verzameling werkruimte het token wordt uitgegeven. |
| **WID** |Het token van de werkruimte-ID wordt uitgegeven. |
| **RID** |Rapport-ID van het token wordt uitgegeven. |
| **gebruikersnaam** (optioneel) |Gebruikt voor beveiliging op Rijniveau, de gebruikersnaam is een tekenreeks waarmee de gebruiker te identificeren bij het toepassen van RLS regels. |
| **rollen** (optioneel) |Een tekenreeks met de rollen selecteren bij het toepassen van regels voor beveiliging op rijniveau. Als meer dan één rol wordt doorgegeven, moeten worden doorgegeven als een String-matrix. |
| **SCP** (optioneel) |Een tekenreeks met de machtigingen bereiken. Als meer dan één rol wordt doorgegeven, moeten worden doorgegeven als een String-matrix. |
| **EXP** (optioneel) |Geeft de tijd waarin het token verloopt. De waarde moet worden doorgegeven als Unix tijdstempels. |
| **NBF** (optioneel) |Geeft de tijd waarin begint het token geldig wordt. De waarde moet worden doorgegeven als Unix tijdstempels. |

Een voorbeeld-app-token ziet eruit als:

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ2ZXIiOiIwLjIuMCIsInR5cGUiOiJlbWJlZCIsIndjbiI6Ikd1eUluQUN1YmUiLCJ3aWQiOiJkNGZlMWViMS0yNzEwLTRhNDctODQ3Yy0xNzZhOTU0NWRhZDgiLCJyaWQiOiIyNWMwZDQwYi1kZTY1LTQxZDItOTMyYy0wZjE2ODc2ZTNiOWQiLCJzY3AiOiJSZXBvcnQuUmVhZCIsImlzcyI6IlBvd2VyQklTREsiLCJhdWQiOiJodHRwczovL2FuYWx5c2lzLndpbmRvd3MubmV0L3Bvd2VyYmkvYXBpIiwiZXhwIjoxNDg4NTAyNDM2LCJuYmYiOjE0ODg0OTg4MzZ9.v1znUaXMrD1AdMz6YjywhJQGY7MWjdCR3SmUSwWwIiI
```

Wanneer gedecodeerd, er ongeveer als volgt:

```
Header

{
    typ: "JWT",
    alg: "HS256:
}

Body

{
  "ver": "0.2.0",
  "wcn": "SupportDemo",
  "wid": "ca675b19-6c3c-4003-8808-1c7ddc6bd809",
  "rid": "96241f0f-abae-4ea9-a065-93b428eddb17",
  "iss": "PowerBISDK",
  "aud": "https://analysis.windows.net/powerbi/api",
  "exp": 1360047056,
  "nbf": 1360043456
}

```

Er zijn methoden beschikbaar zijn in de SDK's die het maken van app-tokens te vereenvoudigen. Bijvoorbeeld: voor .NET kunt u bekijken de [Microsoft.PowerBI.Security.PowerBIToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken) klasse en de [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) methoden.

Voor de .NET SDK, kunt u verwijzen naar [Scopes](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.scopes).

## <a name="scopes"></a>Scopes

Wanneer u insluiten tokens gebruikt, kunt u beperkt gebruik van de resources die u toegang te geven. Daarom kunt u een token met bereik machtigingen genereren.

Hieronder vindt u de beschikbare scopes voor Power BI werkruimte verzamelingen.

|Bereik|Beschrijving|
|---|---|
|Dataset.Read|Machtiging voor het lezen van de opgegeven gegevensset bevat.|
|Dataset.Write|Machtigingen voor schrijven naar de opgegeven gegevensset bevat.|
|Dataset.ReadWrite|Machtiging voor lezen en schrijven naar de opgegeven gegevensset bevat.|
|Report.Read|Machtiging voor het weergeven van het opgegeven rapport bevat.|
|Report.ReadWrite|Machtiging voor het weergeven en bewerken van het opgegeven rapport bevat.|
|Workspace.Report.Create|Machtiging voor het maken van een nieuw rapport binnen de opgegeven werkruimte biedt.|
|Workspace.Report.Copy|Machtiging voor het klonen van een bestaand rapport binnen de opgegeven werkruimte biedt.|

U kunt meerdere scopes opgeven met behulp van een ruimte tussen de scopes als volgt.

```
string scopes = "Dataset.Read Workspace.Report.Create";
```

**Vereiste claims - scopes**

SCP: {scopesClaim} scopesClaim mag een tekenreeks of een matrix van tekenreeksen, zoek ook de toegestane machtigingen voor werkruimte resources (rapport, gegevensset, enz.)

Een gecodeerde token met het bereik dat is gedefinieerd, zou er ongeveer uitzien:

```
Header

{
    typ: "JWT",
    alg: "HS256:
}

Body

{
  "ver": "0.2.0",
  "wcn": "SupportDemo",
  "wid": "ca675b19-6c3c-4003-8808-1c7ddc6bd809",
  "rid": "96241f0f-abae-4ea9-a065-93b428eddb17",
  "scp": "Report.Read",
  "iss": "PowerBISDK",
  "aud": "https://analysis.windows.net/powerbi/api",
  "exp": 1360047056,
  "nbf": 1360043456
}

```

### <a name="operations-and-scopes"></a>Bewerkingen en -scopes

|Bewerking|Doelbron|Token machtigingen|
|---|---|---|
|Een nieuw rapport op basis van een gegevensset voor (in het geheugen) maken.|Gegevensset|Dataset.Read|
|(In het geheugen) Maak een nieuw rapport op basis van een gegevensset en sla het rapport.|Gegevensset|* Dataset.Read<br>* Workspace.Report.Create|
|(In het geheugen) een bestaand rapport verkennen/bewerken en weergeven. Report.Read impliceert Dataset.Read. Er kan geen Report.Read opslaan van wijzigingen.|Rapport|Report.Read|
|Bewerken en opslaan van een bestaand rapport.|Rapport|Report.ReadWrite|
|Sla een kopie van een rapport (OpslaanAls).|Rapport|* Report.Read<br>* Workspace.Report.Copy|

## <a name="heres-how-the-flow-works"></a>Hier wordt de werking van de stroom
1. Kopieer de API-sleutels voor uw toepassing. U kunt de sleutels krijgen in **Azure-portal**.
   
    ![Waar vind ik de API-sleutels in de Azure portal](media/get-started-sample/azure-portal.png)
1. Token een claim asserts en heeft een verlooptijd.
   
    ![App-tokenstroom - token asserts claim](media/get-started-sample/token-2.png)
1. Token opgehaald ondertekend met een API-sleutels voor toegang.
   
    ![App-tokenstroom - token opgehaald ondertekend](media/get-started-sample/token-3.png)
1. Gebruikersaanvragen om een rapport weer te geven.
   
    ![App-tokenstroom - gebruiker vraagt om een rapport weer te geven](media/get-started-sample/token-4.png)
1. Token is gevalideerd met een API-sleutels voor toegang.
   
   ![App-tokenstroom - token wordt gevalideerd](media/get-started-sample/token-5.png)
1. Power BI werkruimte verzamelingen kunt u een rapport naar gebruikers verzonden.
   
   ![App-tokenstroom - service rapport naar gebruiker verzenden](media/get-started-sample/token-6.png)

Na **Power BI werkruimte verzamelingen** verzendt een rapport met de gebruiker, de gebruiker het rapport in uw aangepaste app kunnen weergeven. Bijvoorbeeld, als u hebt geïmporteerd de [analyseren verkoop gegevens PBIX voorbeeld](http://download.microsoft.com/download/1/4/E/14EDED28-6C58-4055-A65C-23B4DA81C4DE/Analyzing_Sales_Data.pbix), de voorbeeld-web-app eruit als:

![Voorbeeld van het rapport ingesloten in toepassing](media/get-started-sample/sample-web-app.png)

## <a name="see-also"></a>Zie ook

[CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_)  
[Aan de slag met Microsoft Power BI werkruimte verzamelingen voorbeeld](get-started-sample.md)  
[Algemene scenario's voor Microsoft Power BI werkruimte verzamelingen](scenarios.md)  
[Aan de slag met Microsoft Power BI werkruimte verzamelingen](get-started.md)  
[Power BI-CSharp Git-opslagplaats](https://github.com/Microsoft/PowerBI-CSharp)

Nog vragen? [Probeer de Power BI-community](http://community.powerbi.com/)