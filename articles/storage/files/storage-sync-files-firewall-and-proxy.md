---
title: Azure File Sync on-premises firewall en proxy-instellingen | Microsoft Docs
description: Azure File Sync on-premises netwerkconfiguratie
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
ms.openlocfilehash: 7d86082abb6412072af44a6b2d794bcf536fa18d
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2018
ms.locfileid: "37342723"
---
# <a name="azure-file-sync-proxy-and-firewall-settings"></a>Proxy- en firewallinstellingen van Azure File Sync
Azure File Sync verbindt uw on-premises servers naar Azure Files, synchronisatie van meerdere locaties en cloud-opslaglagen functies inschakelen. Als zodanig moet een on-premises server worden verbonden met internet. IT-beheerder nodig heeft om te bepalen van het beste pad voor de server te bereiken in Azure cloudservices.

In dit artikel biedt inzicht in de specifieke vereisten en opties die beschikbaar zijn met succes en veilig verbinding maken als uw server met Azure File Sync.

> [!Important]
> Azure File Sync nog ondersteunt geen firewalls en virtuele netwerken voor een opslagaccount. 

## <a name="overview"></a>Overzicht
Azure File Sync fungeert als een service orchestration tussen uw Windows-Server, uw Azure-bestandsshare en verschillende andere Azure-services om te synchroniseren van gegevens, zoals beschreven in de groep voor synchronisatie. Voor Azure File Sync correct te laten werken, moet u de servers om te communiceren met de volgende Azure-services configureren:

- Azure Storage
- Azure File Sync
- Azure Resource Manager
- Verificatieservices

> [!Note]  
> De Azure File Sync-agent op Windows Server initieert alle aanvragen naar de cloud services, wat leidt tot alleen die rekening houden met het uitgaande verkeer vanuit het oogpunt van de firewall. <br /> Er is geen Azure-service maken van verbinding met de Azure File Sync-agent.


## <a name="ports"></a>Poorten
Azure File Sync verplaatst gegevens uit een bestand en metagegevens uitsluitend via HTTPS en vereist-poort 443 uitgaande worden geopend.
Als gevolg hiervan wordt al het verkeer versleuteld.

## <a name="networks-and-special-connections-to-azure"></a>Netwerken en speciale verbindingen naar Azure
De Azure File Sync-agent heeft geen vereisten met betrekking tot speciale kanalen, zoals [ExpressRoute](../../expressroute/expressroute-introduction.md), enzovoort naar Azure.

Azure File Sync werkt via middelen beschikbaar waarmee het bereik in Azure, automatisch aan te passen aan verschillende netwerkkenmerken, zoals bandbreedte, latentie, evenals aanbieding-beheer voor het aan te passen. Niet alle functies zijn beschikbaar op dit moment. Als u wilt om bepaald gedrag te configureren, laat het ons weten [Azure bestanden UserVoice](https://feedback.azure.com/forums/217298-storage?category_id=180670).

## <a name="proxy"></a>Proxy
Azure File Sync biedt ondersteuning voor app-specifieke en alle computers proxy-instellingen.

Machine-proxy-instellingen zijn transparant voor de Azure File Sync-agent omdat het volledige verkeer van de server wordt doorgestuurd via de proxy.

App-specifieke proxy-instellingen van de configuratie van een proxy specifiek voor Azure File Sync-verkeer toestaan. App-specifieke proxy-instellingen worden ondersteund op agentversie 3.0.12.0 of hoger en kunnen worden geconfigureerd tijdens de installatie van de agent of met behulp van de Set-StorageSyncProxyConfiguration PowerShell-cmdlet.

PowerShell-opdrachten voor het app-specifieke proxy-instellingen configureren:
```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Set-StorageSyncProxyConfiguration -Address <url> -Port <port number> -ProxyCredential <credentials>
```

## <a name="firewall"></a>Firewall
Zoals vermeld in een vorige sectie, open poort 443 moet uitgaande. Op basis van beleid in uw datacenter, de vertakking of de regio, verder beperken van verkeer via deze poort tot specifieke domeinen mogelijk gewenst of vereist.

De volgende tabel beschrijft de vereiste domeinen voor de communicatie:

| Service | Domein | Gebruik |
|---------|----------------|------------------------------|
| **Azure Resource Manager** | https://management.azure.com | Elke aanroep van de gebruiker (zoals PowerShell) gaat via deze URL, met inbegrip van de eerste server registratie-aanroep. |
| **Azure Active Directory** | https://login.windows.net | Azure Resource Manager-aanroepen moeten worden gemaakt door een geverifieerde gebruiker. Als u wilt slagen, is deze URL wordt gebruikt voor verificatie van de gebruiker. |
| **Azure Active Directory** | https://graph.windows.net/ | Als onderdeel van de implementatie van Azure File Sync, wordt een service-principal in van het abonnement Azure Active Directory gemaakt. Deze URL wordt gebruikt voor die. Deze principal wordt gebruikt voor het overdragen van een minimale set rechten voor de Azure File Sync-service. De gebruiker die de eerste installatie van Azure File Sync uitvoert moet een geverifieerde gebruiker met bevoegdheden voor het abonnement eigenaren. |
| **Azure Storage** | &ast;.core.windows.net | Wanneer de server een bestand downloadt, klikt u vervolgens de server uitvoert die gegevensverplaatsing efficiënter bij het bespreken van rechtstreeks naar de Azure-bestandsshare in de Storage-Account. De server heeft een SAS-sleutel die alleen voor de betreffende bestandsshare-toegang toestaat. |
| **Azure File Sync** | &ast;.one.microsoft.com | Na de registratie van de eerste server ontvangt de server een regionale URL voor de Azure File Sync-service-exemplaar in deze regio. De server kan de URL gebruiken om te communiceren rechtstreeks en efficiënt met het verwerken van de synchronisatie-exemplaar. |

> [!Important]
> Wanneer het verkeer naar &ast;. one.microsoft.com, verkeer naar meer dan alleen de sync-service is mogelijk van de server. Er zijn veel meer Microsoft-services beschikbaar onder subdomeinen.

Als &ast;. one.microsoft.com te ruim is, kunt u de communicatie van de server beperken door toe te staan alleen expliciete regionale exemplaren van de Azure File Sync-service. Welke instantie (s) om te kiezen, is afhankelijk van de regio van de Opslagsynchronisatieservice waarnaar u hebt geïmplementeerd en de server geregistreerd. Dat is de regio die u wilt toestaan voor de server. Binnenkort zullen er meer URL's voor het inschakelen van nieuwe functies voor bedrijfscontinuïteit. 

| Regio | Azure File Sync regionale eindpunt-URL |
|--------|---------------------------------------|
| Australië - oost | https://kailani-aue.one.microsoft.com |
| Canada - midden | https://kailani-cac.one.microsoft.com |
| VS - oost | https://kailani1.one.microsoft.com |
| Zuidoost-Azië | https://kailani10.one.microsoft.com |
| Verenigd Koninkrijk Zuid | https://kailani-uks.one.microsoft.com |
| West-Europa | https://kailani6.one.microsoft.com |
| VS - west | https://kailani.one.microsoft.com |

> [!Important]
> Als u deze gedetailleerde firewallregels definieert, in dit document vaak controleren en bijwerken van uw firewall-regels om te voorkomen dat de service wordt onderbroken vanwege een verouderde of onvolledige URL-aanbiedingen in de firewall-instellingen.

## <a name="summary-and-risk-limitation"></a>Samenvatting en risico-beperking
De lijsten die u eerder in dit document bevat de URL's met Azure File Sync momenteel communiceert met. Firewalls moeten u mogelijk verkeer uitgaand naar deze domeinen, evenals de antwoorden van deze toestaan. Microsoft streeft ernaar te houden deze lijst bijgewerkt.

Instellen van het domein te beperken van firewall-regels mag een meting om beveiliging te verbeteren. Als deze firewallconfiguraties worden gebruikt, moet een Houd er rekening mee dat URL's worden toegevoegd en gewijzigd na verloop van tijd. Daarom is het een voorzichtige meting om te controleren of de tabellen in dit document als onderdeel van een beheerproces van het wijzigen van een Azure File Sync-agent-versie naar een andere op een test-implementatie van de meest recente agent. Op deze manier kunt u ervoor zorgen dat uw firewall is geconfigureerd voor het toestaan van verkeer naar domeinen de meest recente agent vereist.

## <a name="next-steps"></a>Volgende stappen
- [Planning voor de implementatie van een Azure File Sync](storage-sync-files-planning.md)
- [Azure File Sync implementeren (preview)](storage-sync-files-deployment-guide.md)
