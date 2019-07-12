---
title: Bedreigingen voor de laag van Azure-service in Azure Security Center | Microsoft Docs
description: In dit onderwerp wordt de Azure-service-laag waarschuwingen beschikbaar in Azure Security Center.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: 33c45447-3181-4b75-aa8e-c517e76cd50d
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: monhaber
ms.openlocfilehash: 8c1733877834f82d9ee2524cf8bf54f532e7d9c4
ms.sourcegitcommit: 1e347ed89854dca2a6180106228bfafadc07c6e5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67571723"
---
# <a name="threat-detection-for-azure-service-layer-in-azure-security-center"></a>Detectie van bedreigingen voor de laag van Azure-service in Azure Security Center

In dit onderwerp geeft de waarschuwingen van Beveiligingscentrum beschikbaar bij het bewaken van de volgende Azure-service-lagen.

* [Azure Network-laag](#network-layer)
* [Azure Management-laag (Azure Resource Manager) (Preview)](#management-layer)

>[!NOTE]
>De analytics hieronder vindt u zijn de telemetrie die Security Center maakt gebruik van van toegang te krijgen tot interne Azure-feeds gebruikt, van toepassing op alle resourcetypen.

## Azure Network layer<a name="network-layer"></a>

Security Center-netwerklaag analytics zijn gebaseerd op voorbeeld [IPFIX gegevens](https://en.wikipedia.org/wiki/IP_Flow_Information_Export), die zijn verzameld door Azure core routers pakketheaders. Security Center machine learning-modellen op basis van deze gegevensfeed, identificeren en markeren van schadelijk verkeer activiteiten. Te verrijken IP-adressen, Security Center maakt gebruik van de database van Microsoft Threat Intelligence.

> [!div class="mx-tableFixed"]

|Waarschuwing|Description|
|---|---|
|**Verdachte activiteiten voor uitgaande RDP-netwerk**|Sample netwerkverkeeranalyse heeft afwijkende uitgaande Remote Desktop Protocol (RDP)-communicatie die afkomstig zijn van een resource in uw implementatie gedetecteerd. Deze activiteit abnormaal is voor deze omgeving en kan erop wijzen dat uw resource is aangetast en nu wordt gebruikt om beveiligingsaanvallen externe RDP-eindpunt. Houd er rekening mee dat dit type activiteit ervoor kan zorgen dat uw IP-adres wordt gemarkeerd als kwaadaardig bij externe entiteiten.|
|**Suspicious outgoing RDP network activity naar meerdere bestemmingen**|Sample netwerkverkeeranalyse heeft afwijkende uitgaande Remote Desktop Protocol (RDP)-communicatie die afkomstig zijn van een resource in uw implementatie naar meerdere bestemmingen gedetecteerd. Deze activiteit abnormaal is voor deze omgeving en kan erop wijzen dat uw resource is aangetast en nu wordt gebruikt om beveiligingsaanvallen externe RDP-eindpunten. Houd er rekening mee dat dit type activiteit ervoor kan zorgen dat uw IP-adres wordt gemarkeerd als kwaadaardig bij externe entiteiten.|
|**Verdachte uitgaande SSH network activity**|Sample netwerkverkeeranalyse heeft afwijkende uitgaande Secure Shell (SSH)-communicatie die afkomstig zijn van een resource in uw implementatie gedetecteerd. Deze activiteit abnormaal is voor deze omgeving en kan erop wijzen dat uw resource is aangetast en nu wordt gebruikt om beveiligingsaanvallen externe SSH-eindpunt. Houd er rekening mee dat dit type activiteit ervoor kan zorgen dat uw IP-adres wordt gemarkeerd als kwaadaardig bij externe entiteiten.|
|**Suspicious outgoing SSH network activity naar meerdere bestemmingen**|Sample netwerkverkeeranalyse heeft afwijkende uitgaande Secure Shell (SSH)-communicatie die afkomstig zijn van een resource in uw implementatie naar meerdere bestemmingen gedetecteerd. Deze activiteit abnormaal is voor deze omgeving en kan erop wijzen dat uw resource is aangetast en nu wordt gebruikt om beveiligingsaanvallen externe SSH-eindpunten. Houd er rekening mee dat dit type activiteit ervoor kan zorgen dat uw IP-adres wordt gemarkeerd als kwaadaardig bij externe entiteiten.|
|**Verdachte binnenkomende SSH network activity uit meerdere bronnen**|Sample netwerkverkeeranalyse heeft afwijkende binnenkomende SSH-communicatie uit meerdere bronnen naar een resource gedetecteerd in uw implementatie. Verbinding maken met uw resource verschillende unieke IP-adressen is abnormaal is voor deze omgeving. Deze activiteit kan duiden op een poging te vallen de SSH-interface vanaf meerdere hosts (Botnet).|
|**Verdachte activiteiten voor binnenkomende SSH-netwerk**|Sample netwerkverkeeranalyse heeft afwijkende binnenkomende SSH-communicatie naar een resource gedetecteerd in uw implementatie. Relatief hoog aantal binnenkomende verbindingen naar uw resource is abnormaal is voor deze omgeving. Deze activiteit kan duiden op een poging te vallen de SSH-interface.
|**Verdachte binnenkomende RDP network activity uit meerdere bronnen**|Sample netwerkverkeeranalyse heeft afwijkende binnenkomende RDP-communicatie uit meerdere bronnen naar een resource gedetecteerd in uw implementatie. Verbinding maken met uw resource verschillende unieke IP-adressen is abnormaal is voor deze omgeving. Deze activiteit kan duiden op een poging te vallen de RDP-interface vanaf meerdere hosts (Botnet).|
|**Verdachte activiteiten voor binnenkomende RDP-netwerk**|Sample netwerkverkeeranalyse heeft afwijkende binnenkomende RDP-communicatie naar een resource gedetecteerd in uw implementatie. Relatief hoog aantal binnenkomende verbindingen naar uw resource is abnormaal is voor deze omgeving. Deze activiteit kan duiden op een poging te vallen de SSH-interface.|

Om te begrijpen hoe Security Center kan gebruikmaken van netwerk gerelateerde signalen voor het toepassen van beveiliging tegen bedreigingen, Zie [heuristische DNS detecties in Azure Security Center](https://azure.microsoft.com/blog/heuristic-dns-detections-in-azure-security-center/).
## Azure Management-laag (Azure Resource Manager) (Preview)<a name ="management-layer"></a>

>[!NOTE]
>Security Center-beschermingslaag die is gebaseerd op Azure Resource Manager is momenteel in preview.

Security Center biedt een extra beschermingslaag door gebruik te maken van Azure Resource Manager-gebeurtenissen, die wordt beschouwd als het besturingselement vlak voor Azure. Door het analyseren van de Azure Resource Manager-records, detecteert Security Center ongebruikelijke of mogelijk schadelijke activiteiten in de omgeving van de Azure-abonnement.

> [!div class="mx-tableFixed"]

|Waarschuwing|Description|
|---|---|
|**MicroBurst toolkit run**|Een bekende cloudomgeving reconnaissance toolkit uitvoeren is gedetecteerd in uw omgeving. Het hulpprogramma 'MicroBurst' (Zie https://github.com/NetSPI/MicroBurst) kan worden gebruikt door een aanvaller (of indringingstests tester) aan uw abonnement(en) resources worden toegewezen, het identificeren van onveilig configuraties en het lekken van vertrouwelijke informatie.|
|**Azurite toolkit uitvoeren**|Een bekende cloudomgeving reconnaissance toolkit uitvoeren is gedetecteerd in uw omgeving. Het hulpprogramma 'Azurite' (Zie https://github.com/mwrlabs/Azurite) kan worden gebruikt door een aanvaller (of een testprogramma voor indringingstests) voor het toewijzen van de resources van uw abonnementen en identificeren van onbeveiligde configuraties.|
|**Verdachte beheersessie met behulp van een inactief account**|Analyse van Logboeken activiteit abonnement heeft een verdacht gedrag gedetecteerd. Een principal die is niet in gebruik voor een lange periode, is nu bezig met acties waarmee persistentie voor een aanvaller kunnen worden beveiligd.|
|**Verdachte beheersessie met behulp van PowerShell**|Analyse van Logboeken activiteit abonnement heeft een verdacht gedrag gedetecteerd. Een principal dat PowerShell niet regelmatig worden gebruikt voor het beheren van de abonnementomgeving, is nu met behulp van PowerShell en het uitvoeren van acties waarmee persistentie voor een aanvaller kunnen worden beveiligd.|
|**Gebruik van geavanceerde Azure persistentie technieken**|Analyse van Logboeken activiteit abonnement heeft een verdacht gedrag gedetecteerd. Aangepaste rollen hebben zijn legitimized identiteit entiteiten gekregen. Dit kan leiden dat de aanvaller handhaving in een Azure-klant-omgeving.|
|**Activiteit vanuit niet-gangbaar land**|Activiteit vanaf een locatie die niet onlangs of nooit door een gebruiker in de organisatie bezocht is is opgetreden.<br/>Deze detectie rekening gehouden met het verleden activiteit locaties om te bepalen van de nieuwe en onregelmatige locaties. De anomaliedetectie-engine bevat informatie over de voorgaande locaties die worden gebruikt door gebruikers in de organisatie. 
|**Activiteit vanaf anonieme IP-adressen**|De activiteiten van de gebruikers van een IP-adres dat is geïdentificeerd als een anonieme proxy IP-adres is gedetecteerd. <br/>Deze proxy's worden gebruikt door mensen die u wilt verbergen, IP-adres van hun apparaat en kunnen voor kwade bedoelingen worden gebruikt. Deze detectie maakt gebruik van een machine learning-algoritme dat vermindert 'valse positieven', zoals onjuiste label IP-adressen die veel door gebruikers in de organisatie worden gebruikt.|
|**Onmogelijk traject gedetecteerd**|Twee gebruikersactiviteiten (is één of meerdere sessies) hebben plaatsgevonden, die afkomstig zijn van geografisch verafgelegen locaties binnen een periode die korter is dan de tijd die het zou hebben genomen van de eerste locatie naar de tweede gaan van de gebruiker. Hiermee wordt aangegeven dat een andere gebruiker dezelfde referenties gebruikt. <br/>Deze detectie maakt gebruik van een machine learning-algoritme dat wordt genegeerd voor de hand liggende 'valse positieven' bijdragen aan de voorwaarden onmogelijk traject, zoals VPN-verbindingen en de locaties die regelmatig worden gebruikt door andere gebruikers in de organisatie. De detectie is een eerste leerperiode van zeven dagen gedurende welke het activiteitenpatroon van een nieuwe gebruiker hoort.|

>[!NOTE]
> Aantal van de bovenstaande analytics worden aangestuurd door Microsoft Cloud App Security (MCAS). Als u wilt profiteren van deze analyses, is een geactiveerde MCAS-licentie vereist. Als u een MCAS-licentie hebt, zijn klikt u vervolgens deze waarschuwingen standaard ingeschakeld. Om deze uitschakelen:
>
> 1. Selecteer in de blade Security Center **beveiligingsbeleid**. Voor het abonnement dat u wilt wijzigen, klikt u op **instellingen bewerken**.
> 2. Klik op **detectie van bedreigingen**.
> 3. Onder **inschakelen integraties**, schakel het selectievakje **toestaan dat Microsoft Cloud App Security voor toegang tot mijn gegevens**, en klikt u op **opslaan**.

>[!NOTE]
>Azure Security Center worden beveiligingsgerelateerde klantgegevens opgeslagen in hetzelfde geografische gebied als de resource. Als Microsoft nog geen Azure Security Center in geografisch gebied van de resource is geïmplementeerd, klikt u vervolgens slaat het de gegevens in de Verenigde Staten. Wanneer Microsoft Cloud App Security (MCAS) is ingeschakeld, worden deze gegevens worden opgeslagen in overeenstemming met de regels van de geografische locatie van MCAS. Zie [gegevensopslag voor niet-regionale services voor meer informatie](http://azuredatacentermap.azurewebsites.net/).
