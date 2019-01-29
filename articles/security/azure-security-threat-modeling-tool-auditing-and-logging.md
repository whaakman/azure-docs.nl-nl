---
title: Controle en logboekregistratie - Microsoft Threat Modeling Tool - Azure | Microsoft Docs
description: oplossingen voor bedreigingen die beschikbaar zijn in de Threat Modeling Tool
services: security
documentationcenter: na
author: jegeib
manager: jegeib
editor: jegeib
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2017
ms.author: jegeib
ms.openlocfilehash: 1d67f981991796b81ba3ab6540631e6d62be8077
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55092140"
---
# <a name="security-frame-auditing-and-logging--mitigations"></a>Beveiliging-Frame: Controle en logboekregistratie | Oplossingen 
| Product/Service | Artikel |
| --------------- | ------- |
| **Dynamics CRM**    | <ul><li>[Gevoelige entiteiten in uw oplossing te identificeren en implementeren van de controle van wijzigingen](#sensitive-entities)</li></ul> |
| **Web-App** | <ul><li>[Zorg ervoor dat de controle en logboekregistratie wordt afgedwongen op de toepassing](#auditing)</li><li>[Zorg ervoor dat logboekrotatie en scheiding aanwezig zijn](#log-rotation)</li><li>[Zorg ervoor dat de toepassing worden geen logboekgebeurtenissen gevoelige gebruikersgegevens](#log-sensitive-data)</li><li>[Zorg ervoor dat u controle- en logboekbestanden beperkte toegang hebben](#log-restricted-access)</li><li>[Zorg ervoor dat de gebruiker Management gebeurtenissen worden vastgelegd](#user-management)</li><li>[Zorg ervoor dat het systeem heeft ingebouwde bescherming tegen misbruik](#inbuilt-defenses)</li><li>[Diagnostische logboekregistratie inschakelen voor web-apps in Azure App Service](#diagnostics-logging)</li></ul> |
| **Database** | <ul><li>[Zorg ervoor dat de controle van de aanmelding is ingeschakeld op SQL Server](#identify-sensitive-entities)</li><li>[Detectie van bedreigingen op SQL Azure inschakelen](#threat-detection)</li></ul> |
| **Azure Storage** | <ul><li>[Met Azure Storage Analytics kunt u toegang tot Azure Storage controleren](#analytics)</li></ul> |
| **WCF** | <ul><li>[Voldoende logboekregistratie implementeren](#sufficient-logging)</li><li>[Implementeren met het verwerken van voldoende controle mislukt](#audit-failure-handling)</li></ul> |
| **Web-API** | <ul><li>[Zorg ervoor dat de controle en logboekregistratie wordt afgedwongen op Web-API](#logging-web-api)</li></ul> |
| **IoT Field Gateway** | <ul><li>[Zorg ervoor dat de juiste controle en logboekregistratie wordt afgedwongen op Veldgateway](#logging-field-gateway)</li></ul> |
| **IoT Cloud Gateway** | <ul><li>[Zorg ervoor dat de juiste controle en logboekregistratie wordt afgedwongen op Cloudgateway](#logging-cloud-gateway)</li></ul> |

## <a id="sensitive-entities"></a>Gevoelige entiteiten in uw oplossing te identificeren en implementeren van de controle van wijzigingen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Dynamics CRM | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen**                   | Entiteiten in uw oplossing met gevoelige gegevens identificeren en te implementeren op deze entiteiten en velden controleren |

## <a id="auditing"></a>Zorg ervoor dat de controle en logboekregistratie wordt afgedwongen op de toepassing

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen**                   | Inschakelen van controle en logboekregistratie op alle onderdelen. De logboeken voor controle moeten gebruikerscontext vastleggen. Alle belangrijke gebeurtenissen te identificeren en deze gebeurtenissen. Centrale logboekregistratie implementeren |

## <a id="log-rotation"></a>Zorg ervoor dat logboekrotatie en scheiding aanwezig zijn

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen**                   | <p>Logboekrotatie is een geautomatiseerd proces gebruikt in Systeembeheer waarin een gedateerde logboekbestanden worden gearchiveerd. Servers die vaak grote toepassingen uitvoeren elke aanvraag zich: bij zware logboeken logrotatie is een manier om te beperken van de totale grootte van de logboeken terwijl u nog steeds analyse van recente gebeurtenissen. </p><p>Scheiding aanmelden in feite betekent dat u hebt voor het opslaan van uw logboek bestanden op een andere partitie als waar uw besturingssysteem/toepassing om te sluiten van een Denial of service-aanval of het downgraden van uw toepassing wordt uitgevoerd op de prestaties</p>|

## <a id="log-sensitive-data"></a>Zorg ervoor dat de toepassing worden geen logboekgebeurtenissen gevoelige gebruikersgegevens

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen**                   | <p>Controleer of u zich niet gevoelige gegevens die een gebruiker worden verzonden naar uw site. Controleer voor opzettelijke logboekregistratie, evenals neveneffecten veroorzaakt door problemen van ontwerp. Voorbeelden van gevoelige gegevens zijn:</p><ul><li>Gebruikersreferenties</li><li>Sociaal-fiscaal nummer of andere identificerende gegevens</li><li>Creditcardnummers of andere financiële gegevens</li><li>Gegevens over de servicestatus</li><li>Persoonlijke sleutels of andere gegevens die kunnen worden gebruikt voor het ontsleutelen van versleutelde gegevens</li><li>Systeem- of gegevens die kan worden gebruikt voor de toepassing effectiever aanvallen</li></ul>|

## <a id="log-restricted-access"></a>Zorg ervoor dat u controle- en logboekbestanden beperkte toegang hebben

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen**                   | <p>Controleer of de toegangsrechten voor de logboekbestanden op de juiste wijze zijn ingesteld. Toepassingsaccounts moeten hebben alleen-schrijven toegang en operators en ondersteunend personeel van alleen-lezen toegang moeten hebben, indien nodig.</p><p>Accounts van beheerders zijn de enige accounts die volledige toegang moeten hebben. Controleer Windows ACL voor logboekbestanden om te zorgen dat ze zijn goed met beperkte toegang:</p><ul><li>Toepassingsaccounts moeten alleen-schrijven toegang hebben</li><li>Operators en ondersteunend personeel moeten alleen-lezen toegang hebben indien nodig</li><li>Beheerders zijn de enige accounts die volledige toegang moeten hebben</li></ul>|

## <a id="user-management"></a>Zorg ervoor dat de gebruiker Management gebeurtenissen worden vastgelegd

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen**                   | <p>Zorg ervoor dat de toepassing bewaakt gebeurtenissen voor het beheer van gebruikers zoals geslaagde en mislukte aanmeldingen, wachtwoorden, wijzigen van wachtwoorden, accountvergrendeling, gebruikersregistratie. Dit helpt om te detecteren en reageren op mogelijk verdacht gedrag doen. Bovendien kunnen voor het verzamelen van operationele gegevens; bijvoorbeeld, om bij te houden wie toegang heeft tot de toepassing</p>|

## <a id="inbuilt-defenses"></a>Zorg ervoor dat het systeem heeft ingebouwde bescherming tegen misbruik

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen**                   | <p>Besturingselementen moeten aanwezig zijn die throw beveiligingsuitzondering optreden in het geval van misbruik van de toepassing. Bijvoorbeeld, als Invoervalidatie aanwezig is en een aanvaller probeert te injecteren schadelijke code die komt niet overeen met de reguliere expressie, een beveiligingsuitzondering optreden kan worden gegenereerd die een indicatieve van systeem misbruik van kan zijn</p><p>Bijvoorbeeld, is het aanbevolen om beveiligingsuitzonderingen dat is geregistreerd en acties die worden uitgevoerd voor de volgende problemen:</p><ul><li>Invoervalidatie</li><li>CSRF schendingen</li><li>Beveiligingsaanvallen (bovenste limiet voor het aantal aanvragen per gebruiker per resource)</li><li>Bestand uploaden schendingen</li><ul>|

## <a id="diagnostics-logging"></a>Diagnostische logboekregistratie inschakelen voor web-apps in Azure App Service

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | EnvironmentType - Azure |
| **Verwijzingen**              | N/A  |
| **Stappen** | <p>Azure bevat ingebouwde diagnosefuncties voor de ondersteuning bij foutopsporing in een App Service-web-app. Dit geldt ook voor API apps en mobiele apps. App Service WebApps bieden diagnosefunctionaliteit voor waardevolle informatie uit de webserver en de web-App.</p><p>Deze zijn logisch gescheiden in web server diagnostics en application diagnostics</p>|

## <a id="identify-sensitive-entities"></a>Zorg ervoor dat de controle van de aanmelding is ingeschakeld op SQL Server

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Controle van aanmelding configureren](https://msdn.microsoft.com/library/ms175850.aspx) |
| **Stappen** | <p>Database Server-aanmelding audit moet zijn ingeschakeld om te detecteren/Bevestig het wachtwoord wordt geraden aanvallen. Het is belangrijk om vast te leggen van de mislukte aanmeldingspogingen. Vastleggen van zowel geslaagde als mislukte aanmeldingspogingen biedt een extra voordeel tijdens forensische onderzoeken</p>|

## <a id="threat-detection"></a>Detectie van bedreigingen op SQL Azure inschakelen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | SQL Azure |
| **Kenmerken**              | Versie van SQL - V12 |
| **Verwijzingen**              | [Aan de slag met SQL Database Threat Detection](https://azure.microsoft.com/documentation/articles/sql-database-threat-detection-get-started/)|
| **Stappen** |<p>Threat Detection detecteert afwijkende activiteiten die wijzen op mogelijke beveiligingsrisico's met de database. Het biedt een nieuwe beveiligingslaag, waarmee klanten een om te detecteren en op mogelijke bedreigingen reageert zodra ze zich voordoen, dankzij beveiligingswaarschuwingen over afwijkende activiteiten.</p><p>Gebruikers kunnen de verdachte gebeurtenissen met Azure SQL Database Auditing om te bepalen of die het gevolg van een poging tot toegang tot, of misbruik te maken gegevens in de database te onderzoeken.</p><p>Detectie van bedreigingen is het eenvoudig om potentiële bedreigingen voor de database zonder de noodzaak om te worden van een beveiligingsexpert of het beheren van geavanceerde bewakingssystemen</p>|

## <a id="analytics"></a>Met Azure Storage Analytics kunt u toegang tot Azure Storage controleren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Storage | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A |
| **Verwijzingen**              | [Met behulp van Storage Analytics voor het bewaken van autorisatietype](https://azure.microsoft.com/documentation/articles/storage-security-guide/#storage-analytics) |
| **Stappen** | <p>Voor elke storage-account inschakelen een Azure Storage Analytics uitvoeren van logboekregistratie en metrische gegevens opslaan. De storage analytics-logboeken bieden belangrijke informatie zoals de verificatiemethode die wordt gebruikt door iemand anders wanneer ze toegang hebben tot opslag.</p><p>Dit is heel handig als u toegang tot opslag nauw zijn beveiligen. Bijvoorbeeld, in Blob-opslag kunt u alle van de containers ingesteld op privé en het gebruik van een SAS-service implementeren in uw toepassingen. Vervolgens kunt u de logboeken regelmatig om te zien of uw blobs worden geopend met behulp van de storage-accountsleutels, wat op een schending van beveiliging duiden kunnen, of als de blobs openbaar zijn, maar ze mag niet controleren.</p>|

## <a id="sufficient-logging"></a>Voldoende logboekregistratie implementeren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | .NET Framework |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Voeg Koninkrijk](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_insufficient_logging) |
| **Stappen** | <p>Het ontbreken van een juiste audittrail na een beveiligingsincident kunt forensische inspanningen belemmeren. Windows Communication Foundation (WCF) biedt de mogelijkheid om aan te melden geslaagd en/of mislukte verificatiepogingen.</p><p>Mislukte authenticatiepogingen kan beheerders van potentiële brute-force-aanvallen worden gewaarschuwd. Op deze manier kan vastleggen van gebeurtenissen voor geslaagde verificatie bieden een handige audittrail wanneer een legitieme account is aangetast. De WCF-service security audit functie inschakelen |

### <a name="example"></a>Voorbeeld
Hier volgt een voorbeeldconfiguratie met controle is ingeschakeld
```
<system.serviceModel>
    <behaviors>
        <serviceBehaviors>
            <behavior name=""NewBehavior"">
                <serviceSecurityAudit auditLogLocation=""Default""
                suppressAuditFailure=""false"" 
                serviceAuthorizationAuditLevel=""SuccessAndFailure""
                messageAuthenticationAuditLevel=""SuccessAndFailure"" />
                ...
            </behavior>
        </servicebehaviors>
    </behaviors>
</system.serviceModel>
```

## <a id="audit-failure-handling"></a>Implementeren met het verwerken van voldoende controle mislukt

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | .NET Framework |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Voeg Koninkrijk](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_insufficient_audit_failure_handling) |
| **Stappen** | <p>Ontwikkelde oplossing is geconfigureerd niet voor een uitzondering gegenereerd als het schrijven naar een auditlogboek is mislukt. Als WCF is geconfigureerd niet voor een uitzondering te genereren wanneer deze kan niet schrijven naar een auditlogboek, het programma wordt niet ingelicht over de fout en controle van belangrijke beveiligingsgebeurtenissen mogelijk niet plaatsvinden.</p>|

### <a name="example"></a>Voorbeeld
De `<behavior/>` element van het configuratiebestand WCF geeft WCF niet de toepassing informeert wanneer WCF niet kan schrijven naar een auditlogboek.
```
<behaviors>
    <serviceBehaviors>
        <behavior name="NewBehavior">
            <serviceSecurityAudit auditLogLocation="Application"
            suppressAuditFailure="true"
            serviceAuthorizationAuditLevel="Success"
            messageAuthenticationAuditLevel="Success" />
        </behavior>
    </serviceBehaviors>
</behaviors>
```
WCF om de hoogte van het programma wanneer deze kan niet schrijven naar een auditlogboek te configureren. Het programma moet beschikken over een alternatieve notification-schema op waarschuwing van de organisatie die audittrails worden niet bijgehouden. 

## <a id="logging-web-api"></a>Zorg ervoor dat de controle en logboekregistratie wordt afgedwongen op Web-API

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Web-API | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Inschakelen van controle en logboekregistratie op Web-API's. De logboeken voor controle moeten gebruikerscontext vastleggen. Alle belangrijke gebeurtenissen te identificeren en deze gebeurtenissen. Centrale logboekregistratie implementeren |

## <a id="logging-field-gateway"></a>Zorg ervoor dat de juiste controle en logboekregistratie wordt afgedwongen op Veldgateway

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | IoT Field Gateway | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | <p>Als meerdere apparaten verbinding met een Veldgateway, zorg ervoor dat verbindingspogingen en verificatiestatus (slagen of mislukken) voor afzonderlijke apparaten worden geregistreerd en beheerd op de Gateway van het veld.</p><p>Zorg er ook voor dat de controle wordt uitgevoerd wanneer deze referenties worden opgehaald in gevallen waar Veldgateway voor het onderhoud van de IoT Hub-referenties voor afzonderlijke apparaten. Ontwikkel een proces voor het regelmatig de logboeken te uploaden naar Azure IoT Hub/storage voor de lange termijn wordt bewaard.</p> |

## <a id="logging-cloud-gateway"></a>Zorg ervoor dat de juiste controle en logboekregistratie wordt afgedwongen op Cloudgateway

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | IoT Cloud Gateway | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Inleiding tot IoT Hub-bewerkingen controleren](https://azure.microsoft.com/documentation/articles/iot-hub-operations-monitoring/) |
| **Stappen** | <p>Ontwerp voor het verzamelen en opslaan van controlegegevens die zijn verzameld via IoT Hub bewerkingen controleren. De volgende categorieën van de bewaking inschakelen:</p><ul><li>Bewerkingen voor apparaat-id</li><li>Communicatie van apparaat-naar-cloud</li><li>Communicatie van cloud-naar-apparaat</li><li>Verbindingen</li><li>Uploaden van bestanden</li></ul>|