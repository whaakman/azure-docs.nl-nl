---
title: Bedreigingen voor dataservices in Azure Security Center | Microsoft Docs
description: In dit onderwerp wordt de services gegevenswaarschuwingen beschikbaar in Azure Security Center.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: da960861-0b6c-4d80-932d-898cdebb4f83
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: monhaber
ms.openlocfilehash: 87cfd2769e473d26c2dcae1b7b418f6fb1739915
ms.sourcegitcommit: c0419208061b2b5579f6e16f78d9d45513bb7bbc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/08/2019
ms.locfileid: "67626294"
---
# <a name="threat-detection-for-data-services-in-azure-security-center"></a>Detectie van bedreigingen voor dataservices in Azure Security Center

 Security Center analyseert de logboeken van services voor gegevensopslag en activeert waarschuwingen wanneer er een bedreiging voor uw gegevensbronnen die worden gedetecteerd. Dit onderwerp worden de waarschuwingen die Security Center worden gegenereerd voor de volgende services:

* [Azure SQL Database en SQL datawarehouse](#data-sql)
* [Azure Storage](#azure-storage)

## Azure SQL Database en SQL datawarehouse <a name="data-sql"></a>

SQL threat detection detecteert afwijkende activiteiten die ongebruikelijke en potentieel schadelijke aangeeft probeert te openen of misbruik te maken van databases. Security Center analyseert de SQL-controlelogboeken en wordt standaard uitgevoerd in de SQL-engine.

|Waarschuwing|Description|
|---|---|
|**Kwetsbaarheid voor SQL-injectie**|Een toepassing, een foutieve SQL-instructie gegenereerd in de database. Dit kan duiden op een mogelijk beveiligingsprobleem met SQL-injectieaanvallen. Er zijn twee mogelijke redenen voor het genereren van een foutieve instructie: Een fout in de toepassingscode gebouwd, de foutieve SQL-instructie. Of toepassingscode of opgeslagen procedures niet gebruikersinvoer opschonen bij het maken van de foutieve SQL-instructie, die kan worden misbruikt voor SQL-injectie.|
|**Mogelijke SQL-injectie**|Een actieve aanval is tegen een geïdentificeerde toepassing kwetsbaar voor SQL-injectie opgetreden. Dit betekent dat een aanvaller probeert te injecteren schadelijke SQL-instructies met behulp van de kwetsbare toepassingscode of opgeslagen procedures.|
|**Toegang vanaf ongebruikelijke locatie**|Er is een wijziging in het toegangspatroon tot de SQL server, waarbij iemand heeft aangemeld bij de SQL server vanuit een ongebruikelijke geografische locatie. In sommige gevallen detecteert de waarschuwing een legitieme actie (een nieuwe toepassing of onderhoud door ontwikkelaars). In andere gevallen detecteert de waarschuwing een schadelijke actie (voormalig werknemer, externe aanvaller).|
|**Toegang vanaf ongebruikelijke**|Er is een wijziging in het toegangspatroon tot de SQL server - iemand zich heeft aangemeld met de SQL-server met behulp van een ongebruikelijke klant (SQL-gebruiker). In sommige gevallen detecteert de waarschuwing een legitieme actie (een nieuwe toepassing of onderhoud door ontwikkelaars). In andere gevallen detecteert de waarschuwing een schadelijke actie (voormalig werknemer, externe aanvaller).|
|**Toegang vanaf een mogelijk schadelijke toepassing**|Een toepassing die mogelijk schadelijk is gebruikt voor toegang tot de database. In sommige gevallen detecteert de waarschuwing het uitvoeren van testen om binnen te dringen. In andere gevallen detecteert de waarschuwing een aanval met behulp van gebruikelijk aanvalsprogramma’s.|
|**Brute force SQL-referenties**|Een abnormaal groot aantal mislukte aanmeldingen met andere referenties zijn opgetreden. In sommige gevallen detecteert de waarschuwing het uitvoeren van testen om binnen te dringen. In andere gevallen detecteert de waarschuwing een Brute Force-aanval.|

Voor meer informatie over SQL threat detection waarschuwingen Zie[Azure SQL Database threat detection](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview), en bekijk de sectie waarschuwingen van threat detection. Zie ook [hoe Azure Security Center helpt een Cyberattack onthullen](https://azure.microsoft.com/blog/how-azure-security-center-helps-reveal-a-cyberattack/) om weer te geven een voorbeeld van hoe Security Center schadelijke SQL-activiteit detectie gebruikt voor het detecteren van een aanval.

## Azure Storage<a name="azure-storage"></a>

>[!NOTE]
> Advanced Threat Protection voor Azure Storage is alleen beschikbaar voor Blob-opslag. 

Advanced Threat Protection voor Azure Storage biedt een extra laag met beveiligingsinformatie voor de detectie van ongebruikelijke en mogelijk schadelijke pogingen om toegang te verkrijgen tot of aanvallen op opslagaccounts. Deze extra beveiliging kunt u zonder dat u een beveiligingsexpert en beheren van de bewakingssystemen security om bedreigingen.

Security Center analyseert diagnostische logboeken van lees-, schrijf- en delete-aanvragen naar Blob-opslag voor het detecteren van bedreigingen en het activeert waarschuwingen wanneer afwijkingen in de activiteit plaatsvindt. Zie voor meer informatie, [Opslaganalyse, logboekregistratie configureren](https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account#configure-logging) voor meer informatie.

> [!div class="mx-tableFixed"]

|Waarschuwing|Description|
|---|---|
|**Ongebruikelijke locatie toegang anomaliedetectie**|Sample netwerkverkeeranalyse heeft afwijkende uitgaande Remote Desktop Protocol (RDP)-communicatie die afkomstig zijn van een resource in uw implementatie gedetecteerd. Deze activiteit abnormaal is voor deze omgeving en kan erop wijzen dat uw resource is aangetast en nu met brute kracht externe RDP-eindpunt wordt gebruikt. Houd er rekening mee dat dit type activiteit ervoor kan zorgen dat uw IP-adres wordt gemarkeerd als kwaadaardig bij externe entiteiten.|
|**Toepassing toegang anomaliedetectie**|Geeft aan dat een ongebruikelijke toepassing toegang gehad dit opslagaccount wordt gebruikt tot heeft. Een mogelijke oorzaak is dat een aanvaller toegang heeft gehad tot uw storage-account met behulp van een nieuwe toepassing.|
|**Anonieme toegang voor anomaliedetectie**|Geeft aan dat er een wijziging in het toegangspatroon tot een opslagaccount. Bijvoorbeeld, zijn het account geopend anoniem (zonder verificatie), die is onverwacht vergeleken met de recente toegangspatroon voor dit account. Een mogelijke oorzaak is dat een aanvaller misbruik heeft gemaakt van openbare leestoegang voor een container die bewaring storage blobs.|
|**Data Exfiltration anomaliedetectie**|Geeft aan dat een ongebruikelijk grote hoeveelheden gegevens is uitgepakt in vergelijking met recente activiteit op dit storage-container. Een mogelijke oorzaak is dat een aanvaller een grote hoeveelheid gegevens is geëxtraheerd uit een container die bewaring storage blobs.|
|**Onverwachte verwijderen van afwijkingen**|Geeft aan dat een of meer onverwachte delete-bewerkingen in een storage-account, in vergelijking met recente activiteiten voor dit account heeft plaatsgevonden. Een mogelijke oorzaak is dat een aanvaller gegevens uit uw storage-account is verwijderd.|
|**Azure Cloud Service-pakket uploaden**|Geeft aan dat een Azure Cloud Service-pakket (.cspkg-bestand) is geüpload naar een opslagaccount in een ongebruikelijke manier, in vergelijking met recente activiteiten voor dit account. Een mogelijke oorzaak is dat een aanvaller heeft is voorbereid om schadelijke code van uw opslagaccount in een Azure cloudservice te implementeren.|
|**Machtiging toegang anomaliedetectie**|Geeft aan dat de machtigingen van de opslagcontainer op ongewone wijze zijn gewijzigd. Mogelijke oorzaak is dat een aanvaller containermachtigingen te verzwakken zijn beveiligingspostuur of persistentie is gewijzigd.|
|**Controle van toegang afwijkingen**|Geeft aan dat de toegangsrechten van een storage-account zijn gecontroleerd op een ongebruikelijke manier, in vergelijking met recente activiteiten voor dit account. Een mogelijke oorzaak is dat een aanvaller reconnaissance is uitgevoerd voor een toekomstige aanvallen.|
|**Gegevens verkennen anomaliedetectie**|Geeft aan dat blobs of containers in een storage-account is geïnventariseerd op abnormale wijze, vergeleken met recente activiteiten voor dit account. Een mogelijke oorzaak is dat een aanvaller reconnaissance is uitgevoerd voor een toekomstige aanvallen.|

>[!NOTE]
>Advanced Threat Protection voor Azure Storage is momenteel niet beschikbaar in Azure government en onafhankelijke cloud-regio's.

Zie voor meer informatie over de waarschuwingen voor de opslag, de [Advanced Threat Protection voor Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-advanced-threat-protection) artikel en Raadpleeg de sectie waarschuwingen van de beveiliging.
