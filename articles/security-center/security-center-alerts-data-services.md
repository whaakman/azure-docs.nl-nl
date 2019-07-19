---
title: Detectie van bedreigingen voor gegevens Services in Azure Security Center | Microsoft Docs
description: Dit onderwerp bevat de beschik bare Data Services-waarschuwingen in Azure Security Center.
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
ms.author: v-mohabe
ms.openlocfilehash: 1cafd8a3c766e57aed67634d7da8498c9a6ee120
ms.sourcegitcommit: a8b638322d494739f7463db4f0ea465496c689c6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2019
ms.locfileid: "68295824"
---
# <a name="threat-detection-for-data-services-in-azure-security-center"></a>Detectie van bedreigingen voor gegevens Services in Azure Security Center

 Security Center analyseert de logboeken van de gegevens opslag en activeert waarschuwingen wanneer een bedreiging voor uw gegevens bronnen wordt gedetecteerd. In dit onderwerp vindt u de waarschuwingen die Security Center gegenereerd voor de volgende services:

* [Azure SQL Database en SQL Data Warehouse](#data-sql)
* [Azure Storage](#azure-storage)

## Azure SQL Database en SQL Data Warehouse<a name="data-sql"></a>

SQL-bedreigings detectie detecteert afwijkende activiteiten die een ongebruikelijke en potentieel schadelijke pogingen om toegang te krijgen tot of misbruik te maken van data base Security Center analyseert de SQL-controle logboeken en voert systeem eigen uitvoering uit in de SQL-engine.

|Waarschuwing|Description|
|---|---|
|**Beveiligings probleem met SQL-injectie**|Een toepassing heeft een mislukte SQL-instructie gegenereerd in de data base. Dit kan duiden op een mogelijke kwets baarheid voor SQL-injectie aanvallen. Er zijn twee mogelijke redenen voor het genereren van een foutieve instructie: Een defect in de toepassings code heeft de fout SQL-instructie samengesteld. Het is ook mogelijk dat toepassings code of opgeslagen procedures geen gebruikers invoer opschonen bij het samen stellen van de foutieve SQL-instructie, die kan worden misbruikt voor SQL-injectie.|
|**Mogelijke SQL-injectie**|Er is een actieve crack opgetreden op basis van een geïdentificeerde toepassing die kwetsbaar is voor SQL-injectie. Dit betekent dat een aanvaller schadelijke SQL-instructies wil injecteren met behulp van de kwets bare toepassings code of opgeslagen procedures.|
|**Toegang vanaf ongebruikelijke locatie**|Er is een wijziging aangebracht in het toegangs patroon voor SQL Server, waarbij iemand zich vanaf een ongebruikelijke geografische locatie heeft aangemeld bij de SQL-Server. In sommige gevallen detecteert de waarschuwing een legitieme actie (een nieuwe toepassing of onderhoud door ontwikkelaars). In andere gevallen detecteert de waarschuwing een schadelijke actie (voormalig werknemer, externe aanvaller).|
|**Toegang vanaf onbekende Principal**|Er is een wijziging aangebracht in het toegangs patroon voor de SQL-Server: iemand heeft zich aangemeld bij de SQL-Server met behulp van een ongebruikelijke principal (SQL-gebruiker). In sommige gevallen detecteert de waarschuwing een legitieme actie (een nieuwe toepassing of onderhoud door ontwikkelaars). In andere gevallen detecteert de waarschuwing een schadelijke actie (voormalig werknemer, externe aanvaller).|
|**Toegang via een mogelijk schadelijke toepassing**|Er is een mogelijk schadelijke toepassing gebruikt om toegang te krijgen tot de data base. In sommige gevallen detecteert de waarschuwing het uitvoeren van testen om binnen te dringen. In andere gevallen detecteert de waarschuwing een aanval met behulp van gebruikelijk aanvalsprogramma’s.|
|**SQL-referenties voor brute kracht**|Een abnormaal groot aantal mislukte aanmeldingen met andere referenties heeft plaatsgevonden. In sommige gevallen detecteert de waarschuwing het uitvoeren van testen om binnen te dringen. In andere gevallen detecteert de waarschuwing een Brute Force-aanval.|

Zie[Azure SQL database bedreigings detectie](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview)voor meer informatie over waarschuwingen voor detectie van SQL-dreigingen en Raadpleeg de sectie waarschuwingen voor detectie van bedreigingen. Kijk ook [hoe Azure Security Center een cyberattack kunt onthullen](https://azure.microsoft.com/blog/how-azure-security-center-helps-reveal-a-cyberattack/) om een voor beeld te zien van hoe Security Center schadelijke SQL-activiteit detectie gebruikt om een aanval te detecteren.

## Azure Storage<a name="azure-storage"></a>

>[!NOTE]
> Advanced Threat Protection voor Azure Storage is momenteel alleen beschikbaar voor Blob Storage. 

Advanced Threat Protection voor Azure Storage biedt een extra laag met beveiligingsinformatie voor de detectie van ongebruikelijke en mogelijk schadelijke pogingen om toegang te verkrijgen tot of aanvallen op opslagaccounts. Met deze beveiligingslaag kunt u bedreigingen aanpakken zonder dat u een beveiligings expert hoeft te zijn en beveiligings bewakings systemen te beheren.

Security Center analyseert Diagnostische logboeken met lees-, schrijf-en verwijder aanvragen naar Blob-opslag om bedreigingen te detecteren en genereert een waarschuwing wanneer afwijkingen in de activiteit optreden. Zie [Opslaganalyse logboek registratie configureren](https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account#configure-logging) voor meer informatie.

> [!div class="mx-tableFixed"]

|Waarschuwing|Description|
|---|---|
|**Afwijking van ongebruikelijke locatie toegang**|Voor beeld van netwerk verkeer analyse heeft afwijkende uitgaande Remote Desktop Protocol (RDP)-communicatie gedetecteerd die afkomstig is van een bron in uw implementatie. Deze activiteit wordt als abnormaal beschouwd voor deze omgeving en kan erop wijzen dat uw resource is aangetast en nu wordt gebruikt om een externe RDP-eind punt te forceren. Houd er rekening mee dat dit type activiteit ervoor kan zorgen dat uw IP-adres wordt gemarkeerd als kwaadaardig bij externe entiteiten.|
|**Afwijking van toepassings toegang**|Geeft aan dat een ongebruikelijke toepassing toegang heeft gekregen tot dit opslag account. Een mogelijke oorzaak is dat een aanvaller met een nieuwe toepassing toegang heeft tot uw opslag account.|
|**Afwijking anonieme toegang**|Geeft aan dat er een wijziging is in het toegangs patroon voor een opslag account. Zo is bijvoorbeeld anoniem toegang gekregen tot het account (zonder enige verificatie). Dit is onverwacht vergeleken met het recente toegangs patroon voor dit account. Een mogelijke oorzaak is dat een aanvaller open bare Lees toegang heeft voor een container die BLOB (s)-opslag bevat.|
|**Afwijkingen van gegevens exfiltration**|Geeft aan dat een ongebruikelijk grote hoeveelheid gegevens is geëxtraheerd vergeleken met recente activiteiten op deze opslag container. Een mogelijke oorzaak is dat een aanvaller een grote hoeveelheid gegevens heeft geëxtraheerd uit een container die BLOB (s)-opslag bevat.|
|**Onverwachte afwijkings verwijdering**|Hiermee wordt aangegeven dat er een of meer onverwachte verwijderings bewerkingen zijn uitgevoerd in een opslag account, vergeleken met recente activiteiten voor dit account. Een mogelijke oorzaak is dat een aanvaller gegevens uit uw opslag account heeft verwijderd.|
|**Azure-Cloud service pakket uploaden**|Geeft aan dat een Azure-Cloud service pakket (. cspkg-bestand) op een ongebruikelijke manier is geüpload naar een opslag account, vergeleken met de recente activiteit voor dit account. Een mogelijke oorzaak is dat een aanvaller bereid is om schadelijke code vanuit uw opslag account te implementeren naar een Azure-Cloud service.|
|**Afwijkings machtiging voor toegang**|Geeft aan dat de toegangs machtigingen van deze opslag container op een ongebruikelijke manier zijn gewijzigd. Mogelijke oorzaak is dat een aanvaller container machtigingen heeft gewijzigd om de beveiligings postuur te verzwakken of om persistentie te verkrijgen.|
|**Afwijkings toegang voor inspectie**|Geeft aan dat de toegangs machtigingen van een opslag account op een ongebruikelijke manier zijn geïnspecteerd, vergeleken met de recente activiteit voor dit account. Een mogelijke oorzaak is dat een aanvaller Reconnaissance heeft uitgevoerd voor een toekomstige aanval.|
|**Anomalie voor gegevens onderzoek**|Geeft aan dat blobs of containers in een opslag account op een abnormale manier zijn geïnventariseerd, vergeleken met de recente activiteit van dit account. Een mogelijke oorzaak is dat een aanvaller Reconnaissance heeft uitgevoerd voor een toekomstige aanval.|

>[!NOTE]
>Advanced Threat Protection voor Azure Storage is momenteel niet beschikbaar in azure Government en soevereine Cloud regio's.

Voor meer informatie over de waarschuwingen voor opslag raadpleegt u het artikel [Advanced Threat Protection voor Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-advanced-threat-protection) en raadpleegt u de sectie beveiligings waarschuwingen.
