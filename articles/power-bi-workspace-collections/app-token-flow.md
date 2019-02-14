---
title: Verifiëren en autoriseren met Power BI Workspace Collections | Microsoft Docs
description: Verifiëren en autoriseren met Power BI Workspace Collections.
services: power-bi-embedded
author: markingmyname
ROBOTS: NOINDEX
ms.assetid: 1c1369ea-7dfd-4b6e-978b-8f78908fd6f6
ms.service: power-bi-embedded
ms.topic: article
ms.workload: powerbi
ms.date: 09/20/2017
ms.author: maghan
ms.openlocfilehash: 1dd6be61aab91bb15cc42939689e3c7399bee3db
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56236529"
---
# <a name="authenticating-and-authorizing-with-power-bi-workspace-collections"></a>Verifiëren en autoriseren met Power BI-Werkruimteverzamelingen

Power BI Workspace Collections gebruiken **sleutels** en **App-Tokens** voor verificatie en autorisatie, in plaats van expliciete verificatie van eindgebruikers. In dit model wordt beheerd door uw toepassing verificatie en autorisatie voor uw eindgebruikers. Wanneer dat nodig is, wordt uw app wordt gemaakt en verzendt het app-tokens die vertellen onze service om het gevraagde rapport weer te geven. Dit ontwerp zijn vereist om uw app met Azure Active Directory voor verificatie en autorisatie, maar u nog steeds kan.

> [!IMPORTANT]
> Power BI Workspace Collections is afgeschaft en is beschikbaar tot juni 2018 of tot de datum die in uw contract wordt aangegeven. Om onderbreking van uw toepassing te voorkomen, wordt u geadviseerd om een migratie naar Power BI Embedded te plannen. Voor meer informatie over het migreren van gegevens naar Power BI Embedded raadpleegt u [How to migrate Power BI Workspace Collections content to Power BI Embedded](https://powerbi.microsoft.com/documentation/powerbi-developer-migrate-from-powerbi-embedded/) (Inhoud van Power BI-werkruimteverzamelingen migreren naar Power BI Embedded).

## <a name="two-ways-to-authenticate"></a>Twee manieren om te verifiëren

**Sleutel** -u kunt sleutels gebruiken voor alle Power BI-werkruimte verzamelingen REST-API-aanroepen. De sleutels kunnen u vinden in de **Microsoft Azure portal** hiervoor **alle instellingen** en vervolgens **toegangssleutels**. Uw sleutel altijd behandelen alsof het een wachtwoord. Deze sleutels hebben machtigingen voor het maken van een REST-API aanroepen op een bepaalde werkruimte-verzameling.

Voor het gebruik van een sleutel op een REST-aanroep, voeg de volgende autorisatie-header:

    Authorization: AppKey {your key}

**App-token** -App-tokens worden gebruikt voor alle aanvragen voor insluiten. Ze zijn ontworpen om te worden uitgevoerd aan de clientzijde. Het token is beperkt tot één rapport en de aanbevolen procedure om in te stellen van een verlooptijd.

App-tokens zijn een JWT (JSON Web Token) die is ondertekend door een van uw sleutels.

Uw app-token kan de volgende claims bevatten:

| Claim | Description |    
| --- | --- |
| **ver** |De versie van het app-token. 0.2.0 is de huidige versie. |
| **aud** |De beoogde ontvanger van het token. Voor Power BI Workspace Collections gebruiken: *https:\//analysis.windows.net/powerbi/api*. |
| **iss** |Een tekenreeks die aangeeft van de toepassing die het token is uitgegeven. |
| **type** |Het type van de app-token dat wordt gemaakt. Huidige aantal is het enige ondersteunde type **insluiten**. |
| **wcn** |Naam van werkruimteverzameling het token wordt uitgegeven. |
| **wid** |Werkruimte-ID het token wordt uitgegeven. |
| **rid** |Rapport-ID van het token wordt uitgegeven. |
| **gebruikersnaam** (optioneel) |Beveiliging op rijniveau gebruikt, de gebruikersnaam is een tekenreeks die kan helpen bij het identificeren van de gebruiker bij het toepassen van RLS-regels. |
| **rollen** (optioneel) |Een tekenreeks met de rollen selecteren bij het toepassen van beveiliging op rijniveau regels. Als meer dan één rol wordt doorgegeven, moeten ze worden doorgegeven als een String-matrix. |
| **SCP** (optioneel) |Een tekenreeks met de machtigingen bereiken. Als meer dan één rol wordt doorgegeven, moeten ze worden doorgegeven als een String-matrix. |
| **EXP** (optioneel) |Geeft de tijd waarin het token is verlopen. De waarde moet worden doorgegeven als Unix tijdstempels. |
| **NBF** (optioneel) |Geeft de tijd waarin begint het token geldig wordt. De waarde moet worden doorgegeven als Unix tijdstempels. |

De token van een voorbeeld-app ziet eruit zoals:

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

Er zijn methoden beschikbaar zijn in de SDK's die het maken van app-tokens te vereenvoudigen. Bijvoorbeeld, voor .NET kunt u bekijken de [Microsoft.PowerBI.Security.PowerBIToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken) klasse en de [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN) methoden.

Voor de .NET SDK, kunt u verwijzen naar [Scopes](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.scopes).

## <a name="scopes"></a>Bereiken

Wanneer u insluittokens gebruikt, kunt u slechts beperkt gebruik van de resources die u toegang te geven. Daarom kunt u een token met scoped machtigingen genereren.

Hieronder vindt u de beschikbare bereiken voor Power BI Workspace Collections.

|Bereik|Description|
|---|---|
|Dataset.Read|Machtiging voor het lezen van de opgegeven gegevensset bevat.|
|Dataset.Write|Machtigingen voor schrijven naar de opgegeven gegevensset bevat.|
|Dataset.ReadWrite|Machtiging voor lezen en schrijven naar de opgegeven gegevensset bevat.|
|Report.Read|Machtiging voor het weergeven van het opgegeven rapport bevat.|
|Report.ReadWrite|Machtiging voor het weergeven en bewerken van het opgegeven rapport bevat.|
|Workspace.Report.Create|Machtiging voor het maken van een nieuw rapport binnen de opgegeven werkruimte biedt.|
|Workspace.Report.Copy|Machtiging voor het klonen van een bestaand rapport in de opgegeven werkruimte biedt.|

U kunt meerdere scopes opgeven met behulp van een spatie tussen de bereiken als volgt uit.

```csharp
string scopes = "Dataset.Read Workspace.Report.Create";
```

**Vereiste claims - bereiken**

SCP: {scopesClaim} scopesClaim mag een tekenreeks of een matrix van tekenreeksen, waarbij de toegestane machtigingen voor werkruimteresources (rapport, gegevensset, enz.)

Een token gedecodeerde zou met scopes die zijn gedefinieerd, er ongeveer als volgt:

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
|(In-memory) een nieuw rapport op basis van een gegevensset maken.|Gegevensset|Dataset.Read|
|(In-memory) Maak een nieuw rapport op basis van een gegevensset en sla het rapport.|Gegevensset|* Dataset.Read<br>* Workspace.Report.Create|
|(In-memory) een bestaand rapport verkennen/bewerken en weergeven. Report.Read impliceert Dataset.Read. Report.Read kan geen wijzigingen opslaan.|Rapport|Report.Read|
|Bewerken en opslaan van een bestaand rapport.|Rapport|Report.ReadWrite|
|Sla een kopie van een rapport (OpslaanAls).|Rapport|* Report.Read<br>* Workspace.Report.Copy|

## <a name="heres-how-the-flow-works"></a>De stroom werkt als volgt
1. Kopieer de API-sleutels voor uw toepassing. Krijgt u de sleutels bij **Azure-portal**.
   
    ![Waar vind ik de API-sleutels in Azure portal](media/get-started-sample/azure-portal.png)
1. Token worden bevestigd met een claim en een verlooptijd heeft.
   
    ![App-tokenstroom - token worden bevestigd met claim](media/get-started-sample/token-2.png)
1. Token opgehaald ondertekend met een API-sleutels voor toegang.
   
    ![App-tokenstroom - token wordt ondertekend](media/get-started-sample/token-3.png)
1. Aanvragen van gebruikers om een rapport weer te geven.
   
    ![App-tokenstroom - gebruiker vraagt om een rapport weer te geven](media/get-started-sample/token-4.png)
1. Token is gevalideerd met een API-sleutels voor toegang.
   
   ![App-tokenstroom - token wordt gevalideerd](media/get-started-sample/token-5.png)
1. Power BI Workspace Collections verzonden een rapport naar gebruikers.
   
   ![App-tokenstroom - service rapport naar gebruiker verzenden](media/get-started-sample/token-6.png)

Na **Power BI Workspace Collections** verzendt een rapport voor de gebruiker, de gebruiker het rapport in uw aangepaste app kunnen weergeven. Bijvoorbeeld, als u hebt geïmporteerd de [analyseren verkoop gegevens PBIX voorbeeld](https://download.microsoft.com/download/1/4/E/14EDED28-6C58-4055-A65C-23B4DA81C4DE/Analyzing_Sales_Data.pbix), de voorbeeldweb-app zou er als volgt uitzien:

![Voorbeeld van het rapport ingesloten in de toepassing](media/get-started-sample/sample-web-app.png)

## <a name="see-also"></a>Zie ook

[CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN)  
[Aan de slag met Microsoft Power BI Workspace Collections-voorbeeld](get-started-sample.md)  
[Algemene scenario's voor Microsoft Power BI-Werkruimteverzamelingen](scenarios.md)  
[Aan de slag met Microsoft Power BI-Werkruimteverzamelingen](get-started.md)  
[Git-opslagplaats voor Power BI-CSharp](https://github.com/Microsoft/PowerBI-CSharp)

Nog vragen? [Probeer de Power BI-community](http://community.powerbi.com/)
