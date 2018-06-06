---
title: Azure File-synchronisatie lokale firewall en proxy-instellingen | Microsoft Docs
description: Azure File-synchronisatie lokaal netwerkconfiguratie
services: storage
documentationcenter: ''
author: fauhse
manager: aungoo
editor: tamram
ms.assetid: ''
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2018
ms.author: fauhse
ms.openlocfilehash: 5014c8204b6b6da539a41aaa3308d8787fb517a7
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34738527"
---
# <a name="azure-file-sync-proxy-and-firewall-settings"></a>Proxy- en firewallinstellingen van Azure File Sync
Uw on-premises servers verbindt Azure bestand synchronisatie met Azure-bestanden, synchronisatie voor meerdere locaties en cloud tiering functies inschakelen. Als zodanig moet een on-premises server worden verbonden met internet. IT-beheerder nodig heeft om te bepalen het beste pad voor de server te bereiken in de Azure-cloudservices.

In dit artikel biedt inzicht in de specifieke vereisten en opties die beschikbaar is en veilige wijze verbinden van uw server om te synchroniseren van Azure-bestand.

> [!Important]
> Azure File-synchronisatie ondersteunt nog geen firewalls en virtuele netwerken voor een opslagaccount. 

## <a name="overview"></a>Overzicht
Azure File-synchronisatie fungeert als een service orchestration tussen uw Windows-Server, uw Azure-bestandsshare en verschillende andere Azure-services voor het synchroniseren van gegevens, zoals beschreven in de groep voor synchronisatie. Voor Azure bestand Sync correct te laten werken, moet u uw servers om te communiceren met de volgende Azure-services configureren:

- Azure Storage
- Azure File Sync
- Azure Resource Manager
- Verificatieservices

> [!Note]  
> De Azure-bestand Sync-agent op Windows Server initieert alle aanvragen met cloudservices, wat leidt tot alleen hoeven te overwegen van uitgaand verkeer vanuit het perspectief van een firewall. <br /> Er is geen Azure service initieert een verbinding met de Azure-bestand Sync-agent.


## <a name="ports"></a>Poorten
Azure File-synchronisatie wordt verplaatst bestandsgegevens en metagegevens uitsluitend via HTTPS en poort 443 worden openen uitgaande vereist.
Als gevolg hiervan wordt al het verkeer versleuteld.

## <a name="networks-and-special-connections-to-azure"></a>Netwerken en speciale verbindingen naar Azure
De Azure-bestand Sync-agent heeft geen vereisten met betrekking tot speciale kanalen, zoals [ExpressRoute](../../expressroute/expressroute-introduction.md), enz. naar Azure.

Azure File-synchronisatie werkt via middelen beschikbaar waarmee reach in Azure, automatisch aan te passen op verschillende netwerkkenmerken zoals bandbreedte, latentie, evenals een aanbieding beheer voor het aan te passen. Niet alle functies zijn beschikbaar op dit moment. Als u configureren specifiek gedrag wilt, laat ons weten [Azure bestanden UserVoice](https://feedback.azure.com/forums/217298-storage?category_id=180670).

## <a name="proxy"></a>Proxy
Azure File-synchronisatie ondersteunt momenteel machine proxy-instellingen. Deze proxy-instellingen is transparant voor de Azure-bestand Sync-agent als het gehele verkeer van de server wordt doorgestuurd via deze proxy.

App-specifiek proxy-instellingen zijn momenteel in ontwikkeling en worden ondersteund in een toekomstige versie van de Azure-bestand Sync-agent. Hierdoor kan de configuratie van een proxy specifiek voor Azure File-Sync-verkeer.

## <a name="firewall"></a>Firewall
Zoals vermeld in een eerder gedeelte, open poort 443 moet zo worden uitgaand. Op basis van beleid in uw datacenter, de vertakking of de regio, meer verkeer via deze poort te beperken tot specifieke domeinen mogelijk gewenst of vereist.

De volgende tabel staan de domeinen vereist voor communicatie:

| Service | Domein | Gebruik |
|---------|----------------|------------------------------|
| **Azure Resource Manager** | https://management.azure.com | Geen enkele aanroep van de gebruiker (zoals PowerShell) gaat via deze URL, met inbegrip van de eerste server registratie-aanroep. |
| **Azure Active Directory** | https://login.windows.net | Azure Resource Manager-aanroepen moeten worden gemaakt door een geverifieerde gebruiker. Te laten slagen, worden deze URL wordt gebruikt voor verificatie van gebruiker. |
| **Azure Active Directory** | https://graph.windows.net/ | Als onderdeel van de implementatie van Azure File-synchronisatie, wordt een service-principal in het abonnement Azure Active Directory gemaakt. Deze URL wordt gebruikt voor die. Deze principal wordt gebruikt voor het overdragen van een minimale set rechten voor de synchronisatieservice van Azure-bestand. De gebruiker die de eerste installatie van Azure File-synchronisatie wordt uitgevoerd, moet een geverifieerde gebruiker met bevoegdheden voor abonnement-eigenaar. |
| **Azure Storage** | &ast;.core.windows.net | Wanneer de server een bestand wordt gedownload, klikt u vervolgens de server worden uitgevoerd dat de gegevensverplaatsing efficiënter als praten rechtstreeks met de Azure-bestandsshare in de Storage-Account. De server heeft een SAS-sleutel die u alleen voor gerichte bestandsshare-toegang kunt. |
| **Azure File-synchronisatie** | &ast;.one.microsoft.com | Na de serverregistratie van de eerste ontvangt de server een regionale URL op voor het synchroniseren van Azure File service-exemplaar in deze regio. De server kan de URL gebruiken om te communiceren rechtstreeks en efficiënt met het verwerken van de synchronisatie-exemplaar. |

> [!Important]
> Wanneer u verkeer toestaat &ast;. one.microsoft.com, verkeer naar meer dan alleen de synchronisatieservice is mogelijk van de server. Er zijn veel meer Microsoft-services onder subdomeinen beschikbaar.

Als &ast;. one.microsoft.com te ruim is, kunt u de communicatie tussen de server doordat alleen expliciete regionale exemplaren van de Azure-bestanden Sync-service beperken. Welke instanties te kiezen, hangt af van het gebied van de synchronisatieservice opslag waarmee u hebt geïmplementeerd en de server is geregistreerd. Dit is de regio die u wilt toestaan voor de server. Binnenkort zal er meer URL's naar de nieuwe zakelijke continuïteit functies inschakelen. 

| Regio | Azure bestand Sync regionale eindpunt-URL |
|--------|---------------------------------------|
| Australië - oost | https://kailani-aue.one.microsoft.com |
| Canada - midden | https://kailani-cac.one.microsoft.com |
| VS - oost | https://kailani1.one.microsoft.com |
| Zuidoost-Azië | https://kailani10.one.microsoft.com |
| Verenigd Koninkrijk Zuid | https://kailani-uks.one.microsoft.com |
| West-Europa | https://kailani6.one.microsoft.com |
| VS - west | https://kailani.one.microsoft.com |

> [!Important]
> Als u deze gedetailleerde firewallregels definieert, vaak in dit document controleren en bijwerken van de firewallregels om serviceonderbrekingen vanwege achterhaalde of onvolledige URL-lijsten in uw firewall-instellingen te voorkomen.

## <a name="summary-and-risk-limitation"></a>Samenvatting en risico-beperking
De lijsten eerder in dit document bevatten Azure File-synchronisatie momenteel communiceert met URL's. Firewalls moeten u mogelijk verkeer uitgaand naar deze domeinen, evenals de antwoorden van deze toestaan. Microsoft wil deze lijst bijgewerkt te houden.

Domein beperken van de firewall-regels instelt, kan een maateenheid voor verbeterde beveiliging zijn. Als u deze firewallconfiguraties gebruikt, moet een Houd er rekening mee dat URL's worden toegevoegd en gewijzigd gedurende een bepaalde periode. Daarom is een meting verdient aanbeveling om te controleren van de tabellen in dit document als onderdeel van een wijzigingsbeheerproces van één versie van Azure bestand Sync-agent naar de andere in een test-implementatie van de nieuwste agent. Op deze manier kunt u ervoor zorgen dat uw firewall is geconfigureerd om verkeer te staan tot domeinen de meest recente agent vereist.

## <a name="next-steps"></a>Volgende stappen
- [Planning voor de implementatie van een Azure-bestand synchronisatie](storage-sync-files-planning.md)
- [Azure File-synchronisatie (preview) implementeren](storage-sync-files-deployment-guide.md)
