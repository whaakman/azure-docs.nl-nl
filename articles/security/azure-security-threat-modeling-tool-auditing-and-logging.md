---
title: Controle en logboekregistratie van Azure - Microsoft Threat Modeling hulpprogramma - | Microsoft Docs
description: oplossingen voor bedreigingen die worden weergegeven in het hulpprogramma Threat Modeling
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 3f1933fc59862eca7ae6ee40bbd5136e449e5cf1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="security-frame-auditing-and-logging--mitigations"></a>Beveiliging Frame: Controle en logboekregistratie | Oplossingen 
| Product/Service | Artikel |
| --------------- | ------- |
| **Dynamics CRM**    | <ul><li>[Gevoelige entiteiten in uw oplossing voor het identificeren en te implementeren controle van wijzigingen](#sensitive-entities)</li></ul> |
| **Webtoepassing** | <ul><li>[Zorg ervoor dat de controle en logboekregistratie wordt afgedwongen voor de toepassing](#auditing)</li><li>[Zorg ervoor dat logrotatie en scheiding aanwezig zijn](#log-rotation)</li><li>[Zorg ervoor dat de toepassing wordt niet geregistreerd voor gevoelige gebruikersgegevens](#log-sensitive-data)</li><li>[Zorg ervoor dat u controle- en logboekbestanden beperkte toegang hebben](#log-restricted-access)</li><li>[Zorg ervoor dat de gebruiker Management gebeurtenissen worden geregistreerd](#user-management)</li><li>[Zorg ervoor dat het systeem heeft ingebouwde beveiliging tegen misbruik](#inbuilt-defenses)</li><li>[Logboekregistratie van diagnostische gegevens van web-apps in Azure App Service](#diagnostics-logging)</li></ul> |
| **Database** | <ul><li>[Zorg ervoor dat de controle van de aanmelding is ingeschakeld op SQL Server](#identify-sensitive-entities)</li><li>[Detectie van dreigingen in Azure SQL inschakelen](#threat-detection)</li></ul> |
| **Azure Storage** | <ul><li>[Azure Storage Analytics gebruiken voor het controleren van de toegang van Azure Storage](#analytics)</li></ul> |
| **WCF** | <ul><li>[Voldoende logboekregistratie implementeren](#sufficient-logging)</li><li>[Afhandeling van voldoende Audit fout implementeren](#audit-failure-handling)</li></ul> |
| **Web-API** | <ul><li>[Zorg ervoor dat de controle en logboekregistratie wordt afgedwongen voor Web-API](#logging-web-api)</li></ul> |
| **Veld IoT Gateway** | <ul><li>[Zorg ervoor dat de juiste voor controle en logboekregistratie wordt afgedwongen voor Veldgateway](#logging-field-gateway)</li></ul> |
| **IoT-Cloudgateway** | <ul><li>[Zorg ervoor dat de juiste voor controle en logboekregistratie wordt afgedwongen voor Cloudgateway](#logging-cloud-gateway)</li></ul> |

## <a id="sensitive-entities"></a>Gevoelige entiteiten in uw oplossing voor het identificeren en te implementeren controle van wijzigingen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Dynamics CRM | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen**                   | Entiteiten in uw oplossing met gevoelige gegevens identificeren en te implementeren op deze entiteiten en velden controle van wijzigingen |

## <a id="auditing"></a>Zorg ervoor dat de controle en logboekregistratie wordt afgedwongen voor de toepassing

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen**                   | Inschakelen van controle en logboekregistratie voor alle onderdelen. Controlelogboeken moeten gebruikerscontext vastleggen. Alle belangrijke gebeurtenissen identificeren en deze gebeurtenissen. Centrale logboekregistratie implementeren |

## <a id="log-rotation"></a>Zorg ervoor dat logrotatie en scheiding aanwezig zijn

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen**                   | <p>Logrotatie is een geautomatiseerd proces gebruikt in Systeembeheer waarin datum logboekbestanden worden gearchiveerd. Servers die vaak grote toepassingen uitvoeren Meld elke aanvraag: met betrekking tot zware logboeken logrotatie is een manier om te beperken van de totale grootte van de logboeken terwijl u nog steeds analyse van recente gebeurtenissen. </p><p>Meld u scheiding in feite betekent dat u hebt voor het opslaan van uw logboek bestanden op een andere partitie als waar uw OS/toepassing om te sluiten van een Denial of service-aanval of het downgraden van uw toepassing wordt uitgevoerd op de prestaties</p>|

## <a id="log-sensitive-data"></a>Zorg ervoor dat de toepassing wordt niet geregistreerd voor gevoelige gebruikersgegevens

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen**                   | <p>Controleer of u zich geen gevoelige gegevens die een gebruiker verzendt niet aanmelden bij uw site. Controleer of opzettelijk logboekregistratie, evenals bijwerkingen veroorzaakt door problemen van ontwerp. Voorbeelden van gevoelige gegevens omvatten:</p><ul><li>De referenties van gebruiker</li><li>Sociaal-fiscaal nummer of andere identiteitsgegevens</li><li>Creditcardnummers of andere financiële gegevens</li><li>Statusgegevens</li><li>Persoonlijke sleutels of andere gegevens die kunnen worden gebruikt om versleutelde gegevens te ontsleutelen</li><li>Informatie van systemen of toepassingen die kan worden gebruikt voor de toepassing effectiever aanvallen</li></ul>|

## <a id="log-restricted-access"></a>Zorg ervoor dat u controle- en logboekbestanden beperkte toegang hebben

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen**                   | <p>Controleer of de rechten voor de logboekbestanden op de juiste wijze zijn ingesteld. Toepassing accounts moeten alleen-schrijven toegang en operators en ondersteuningspersoneel alleen-lezen toegang moeten hebben, indien nodig.</p><p>Administrators-accounts zijn de enige accounts die volledige toegang moeten hebben. Controleer Windows ACL voor logboekbestanden om te controleren of dat ze goed beperkte zijn:</p><ul><li>Accounts van de toepassing moeten alleen-schrijven toegang hebben</li><li>Operators en iemand van ondersteuning moeten alleen-lezen toegang hebben indien nodig</li><li>Beheerders zijn de enige accounts volledige toegang hebben</li></ul>|

## <a id="user-management"></a>Zorg ervoor dat de gebruiker Management gebeurtenissen worden geregistreerd

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen**                   | <p>Zorg dat de toepassing wordt bewaakt gebeurtenissen voor het beheer van gebruikers zoals geslaagde en mislukte aanmeldingen, wachtwoorden opnieuw instellen, wijzigen van wachtwoorden, accountvergrendeling, gebruikersregistratie. Dit helpt om te detecteren en reageren op mogelijk verdacht gedrag doet. Bovendien kunnen voor het verzamelen van operationele gegevens; bijvoorbeeld, om bij te houden wie toegang heeft tot de toepassing</p>|

## <a id="inbuilt-defenses"></a>Zorg ervoor dat het systeem heeft ingebouwde beveiliging tegen misbruik

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen**                   | <p>Besturingselementen moeten aanwezig zijn die beveiligingsuitzondering in geval van een toepassing misbruik genereert. Bijvoorbeeld, als validatie voor invoer geïmplementeerd is en een aanvaller probeert te injecteren schadelijke code die komt niet overeen met de reguliere expressie, een beveiligingsuitzondering kan worden gegenereerd die een aanduiden system misbruik kan zijn</p><p>Bijvoorbeeld: u wordt aangeraden beveiligingsuitzonderingen vastgelegd en acties die worden uitgevoerd voor de volgende problemen:</p><ul><li>Validatie voor invoer</li><li>CSRF schendingen</li><li>Beveiligingsaanvallen (bovengrens voor het aantal aanvragen per gebruiker per resource)</li><li>Bestand uploaden schendingen</li><ul>|

## <a id="diagnostics-logging"></a>Logboekregistratie van diagnostische gegevens van web-apps in Azure App Service

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | EnvironmentType - Azure |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | <p>Azure biedt ingebouwde diagnostische gegevens om te helpen bij foutopsporing van een App Service-web-app. Dit geldt ook voor de API-apps en mobiele apps. App Service-web-apps bieden diagnostische functionaliteit voor logboekregistratie informatie van de webserver en de webtoepassing.</p><p>Deze zijn logisch verdeeld in de web server diagnostics en application diagnostics</p>|

## <a id="identify-sensitive-entities"></a>Zorg ervoor dat de controle van de aanmelding is ingeschakeld op SQL Server

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [Aanmelding controle configureren](https://msdn.microsoft.com/library/ms175850.aspx) |
| **Stappen** | <p>Server aanmelding Databasecontrole moet zijn ingeschakeld voor aanvallen te detecteren/Bevestig het wachtwoord. Het is belangrijk om vast te leggen mislukte aanmeldingspogingen. Vastleggen van beide geslaagde en mislukte aanmeldingspogingen biedt een extra voordeel tijdens forensische onderzoeken</p>|

## <a id="threat-detection"></a>Detectie van dreigingen in Azure SQL inschakelen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | SQL Azure |
| **Kenmerken**              | SQL-versie - V12 |
| **Verwijzingen**              | [Aan de slag met detectie van dreigingen van SQL Database](https://azure.microsoft.com/documentation/articles/sql-database-threat-detection-get-started/)|
| **Stappen** |<p>Detectie van dreigingen detecteert afwijkende databaseactiviteiten die wijzen op beveiligingsdreigingen voor de database. Het biedt een nieuwe laag van beveiliging, waarmee klanten om te detecteren en op mogelijke bedreigingen reageert wanneer deze zich voordoen doordat beveiligingswaarschuwingen op vreemde activiteiten worden gedetecteerd.</p><p>Gebruikers kunnen de verdachte gebeurtenissen om te bepalen of ze het gevolg zijn van een poging om te openen, inbreuk of misbruik van gegevens in de database met behulp van Azure SQL Database Auditing verkennen.</p><p>Detectie van dreigingen kunt u eenvoudig op mogelijke bedreigingen adres met de database hoeft te worden van een deskundige beveiliging of systemen bewaking van de geavanceerde beveiliging beheren</p>|

## <a id="analytics"></a>Azure Storage Analytics gebruiken voor het controleren van de toegang van Azure Storage

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Storage | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t. |
| **Verwijzingen**              | [Met behulp van Storage Analytics autorisatie type bewaken](https://azure.microsoft.com/documentation/articles/storage-security-guide/#storage-analytics) |
| **Stappen** | <p>Voor elk opslagaccount kan een Azure Storage Analytics logboekregistratie uitvoeren en metrische gegevens opslaan inschakelen. De logboeken van storage analytics bieden belangrijke informatie zoals de verificatiemethode die door iemand wordt gebruikt wanneer ze toegang hebben tot opslag.</p><p>Dit is heel nuttig als u toegang tot opslag nauw zijn beveiligen. Bijvoorbeeld in Blob Storage kunt u alle van de containers ingesteld op persoonlijke en het gebruik van een SAS-service implementeren in uw toepassingen. Vervolgens kunt u de logboeken regelmatig om te zien als uw blobs worden geopend met behulp van de opslagaccountsleutels, dit kunnen duiden op een schending van beveiliging, of als de blobs openbaar zijn, maar ze mag niet controleren.</p>|

## <a id="sufficient-logging"></a>Voldoende logboekregistratie implementeren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | .NET framework |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Voeg Koninkrijk](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Stappen** | <p>Het ontbreken van een juiste audittrail na een beveiligingsincident kunt forensische inspanningen belemmeren. Windows Communication Foundation (WCF) biedt de mogelijkheid aan te melden geslaagd en/of mislukte verificatiepogingen.</p><p>Logboekregistratie van mislukte verificatiepogingen kan beheerders van mogelijke aanvallen met brute kracht waarschuwing. Op deze manier kunt vastleggen van gebeurtenissen voor geslaagde verificatie bieden een handig audittrail bij een legitieme account is geknoeid. Inschakelen van WCF-service-onderdeel voor controle van beveiliging |

### <a name="example"></a>Voorbeeld
Hier volgt een voorbeeldconfiguratie met de controle is ingeschakeld
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

## <a id="audit-failure-handling"></a>Afhandeling van voldoende Audit fout implementeren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | .NET framework |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Voeg Koninkrijk](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Stappen** | <p>Ontwikkelde oplossing is geconfigureerd niet voor het genereren van een uitzondering om te schrijven naar een controlelogboek is mislukt. Als WCF is geconfigureerd niet voor een uitzondering te genereren wanneer deze kan niet schrijven naar een controlelogboek, het programma wordt niet gewaarschuwd van de fout en de controle van kritieke beveiligingsgebeurtenissen mogelijk niet plaatsvinden.</p>|

### <a name="example"></a>Voorbeeld
De `<behavior/>` element van het configuratiebestand WCF Hiermee geeft u WCF de toepassing niet melden als WCF om te schrijven naar een controlelogboek is mislukt.
````
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
````
WCF om de hoogte van het programma wanneer deze kan niet schrijven naar een controlelogboek te configureren. Het programma moet beschikken over een alternatieve notification-schema te waarschuwen dat de organisatie die audittrails niet worden nageleefd. 

## <a id="logging-web-api"></a>Zorg ervoor dat de controle en logboekregistratie wordt afgedwongen voor Web-API

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Web-API | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | Inschakelen van controle en logboekregistratie op Web-API's. Controlelogboeken moeten gebruikerscontext vastleggen. Alle belangrijke gebeurtenissen identificeren en deze gebeurtenissen. Centrale logboekregistratie implementeren |

## <a id="logging-field-gateway"></a>Zorg ervoor dat de juiste voor controle en logboekregistratie wordt afgedwongen voor Veldgateway

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Veld IoT Gateway | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | <p>Als meerdere apparaten verbinding met een Veldgateway, zorg ervoor dat de verbindingspogingen en de status van verificatie (slagen of mislukken) voor afzonderlijke apparaten geregistreerd en beheerd op de Gateway veld.</p><p>Controleer ook de controle wordt uitgevoerd, wanneer deze referenties worden opgehaald in gevallen waar Veldgateway voor het onderhoud van de referenties van de IoT Hub voor afzonderlijke apparaten. Ontwikkel een proces voor het periodiek de logboeken te uploaden naar Azure IoT Hub/storage voor het bewaren van lange termijn.</p> |

## <a id="logging-cloud-gateway"></a>Zorg ervoor dat de juiste voor controle en logboekregistratie wordt afgedwongen voor Cloudgateway

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | IoT-Cloudgateway | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [Inleiding tot IoT Hub bewerkingen controleren](https://azure.microsoft.com/documentation/articles/iot-hub-operations-monitoring/) |
| **Stappen** | <p>Ontwerpen voor het verzamelen en opslaan van controlegegevens die zijn verzameld via IoT Hub Operations Monitoring. De volgende categorieën bewaking inschakelen:</p><ul><li>Bewerkingen voor apparaat-id</li><li>Apparaat-naar-cloud-communicatie</li><li>Cloud-naar-apparaat communicatie</li><li>Verbindingen</li><li>Uploaden van bestanden</li></ul>|