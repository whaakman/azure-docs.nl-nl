---
title: Azure File Sync on-premises firewall en proxy-instellingen | Microsoft Docs
description: Azure File Sync on-premises netwerkconfiguratie
services: storage
author: fauhse
ms.service: storage
ms.topic: article
ms.date: 08/08/2018
ms.author: fauhse
ms.component: files
ms.openlocfilehash: 25bec4bdbae5b206d23fa300f139f55f86b8167e
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47434378"
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

**App-specifieke proxy-instellingen** configuratie van een proxy specifiek voor Azure File Sync-verkeer toestaan. App-specifieke proxy-instellingen worden ondersteund op agentversie 3.0.12.0 of hoger en kunnen worden geconfigureerd tijdens de installatie van de agent of met behulp van de Set-StorageSyncProxyConfiguration PowerShell-cmdlet.

PowerShell-opdrachten voor het app-specifieke proxy-instellingen configureren:
```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Set-StorageSyncProxyConfiguration -Address <url> -Port <port number> -ProxyCredential <credentials>
```
**Proxy-instellingen machine hele** transparant voor de Azure File Sync-agent zijn als het hele verkeer van de server wordt doorgestuurd via de proxy.

Voor het configureren van machine-proxy-instellingen, de volgende stappen uit te voeren: 

1. Proxy-instellingen voor .NET-toepassingen configureren 

  - Bewerk deze twee bestanden:  
    C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config  
    C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config\machine.config

  - De sectie < system.net > toevoegen in het bestand Machine.config (onder de sectie < system.serviceModel >).  Wijzig 127.0.01:8888 in de IP-adres en poort voor de proxyserver. 
  ```
      <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
          <proxy autoDetect="false" bypassonlocal="false" proxyaddress="http://127.0.0.1:8888" usesystemdefault="false" />
        </defaultProxy>
      </system.net>
  ```

2. Stel de WinHTTP-proxy-instellingen 

  - Voer de volgende opdracht vanaf een opdrachtprompt met verhoogde bevoegdheid of PowerShell om te zien van de bestaande proxy-instellingen:   

    proxy van netsh winhttp show

  - Voer de volgende opdracht vanaf een opdrachtprompt met verhoogde bevoegdheid of PowerShell om in te stellen van de proxy-instellingen (127.0.01:8888 wijzigen in de IP-adres en poort voor de proxyserver):  

    netsh winhttp instellen proxy 127.0.0.1:8888

3. Met de volgende opdracht vanaf een opdrachtprompt met verhoogde bevoegdheid of PowerShell om de opslag-Sync-Agent-service opnieuw te starten: 

      net stop filesyncsvc

      Opmerking: De service opslag-Sync-Agent (filesyncsvc) wordt automatisch wordt gestart zodra deze is gestopt.

## <a name="firewall"></a>Firewall
Zoals vermeld in een vorige sectie, open poort 443 moet uitgaande. Op basis van beleid in uw datacenter, de vertakking of de regio, verder beperken van verkeer via deze poort tot specifieke domeinen mogelijk gewenst of vereist.

De volgende tabel beschrijft de vereiste domeinen voor de communicatie:

| Service | Domain | Gebruik |
|---------|----------------|------------------------------|
| **Azure Resource Manager** | https://management.azure.com | Elke aanroep van de gebruiker (zoals PowerShell) gaat via deze URL, met inbegrip van de eerste server registratie-aanroep. |
| **Azure Active Directory** | https://login.windows.net | Azure Resource Manager-aanroepen moeten worden gemaakt door een geverifieerde gebruiker. Als u wilt slagen, is deze URL wordt gebruikt voor verificatie van de gebruiker. |
| **Azure Active Directory** | https://graph.windows.net/ | Als onderdeel van de implementatie van Azure File Sync, wordt een service-principal in van het abonnement Azure Active Directory gemaakt. Deze URL wordt gebruikt voor die. Deze principal wordt gebruikt voor het overdragen van een minimale set rechten voor de Azure File Sync-service. De gebruiker die de eerste installatie van Azure File Sync uitvoert moet een geverifieerde gebruiker met bevoegdheden voor het abonnement eigenaren. |
| **Azure Storage** | &ast;.core.windows.net | Wanneer de server een bestand downloadt, klikt u vervolgens de server uitvoert die gegevensverplaatsing efficiënter bij het bespreken van rechtstreeks naar de Azure-bestandsshare in de Storage-Account. De server heeft een SAS-sleutel die alleen voor de betreffende bestandsshare-toegang toestaat. |
| **Azure File Sync** | &ast;.one.microsoft.com | Na de registratie van de eerste server ontvangt de server een regionale URL voor de Azure File Sync-service-exemplaar in deze regio. De server kan de URL gebruiken om te communiceren rechtstreeks en efficiënt met het verwerken van de synchronisatie-exemplaar. |
| **Microsoft PKI** | http://www.microsoft.com/pki/mscorp  http://ocsp.msocsp.com | Nadat de Azure File Sync-agent is geïnstalleerd, wordt de PKI-URL wordt gebruikt voor het downloaden van tussenliggende certificaten nodig zijn voor communicatie met de Azure File Sync-service en Azure-bestandsshare. De OCSP-URL wordt gebruikt om de status van een certificaat te controleren. |

> [!Important]
> Wanneer het verkeer naar &ast;. one.microsoft.com, verkeer naar meer dan alleen de sync-service is mogelijk van de server. Er zijn veel meer Microsoft-services beschikbaar onder subdomeinen.

Als &ast;. one.microsoft.com te ruim is, kunt u de communicatie van de server beperken door toe te staan communicatie met alleen een expliciete regionale exemplaren van de Azure File Sync-service. Welke instantie (s) om te kiezen, is afhankelijk van de regio van de opslagsynchronisatieservice hebt u de server geregistreerd en geïmplementeerd. Deze regio heet 'primaire eindpunt-URL' in de onderstaande tabel.

Voor zakelijke continuïteit en noodherstel (BCDR) recovery redenen kan u hebt opgegeven dat uw Azure-bestandsshares in een wereldwijd redundant (GRS) storage-account. Als dit het geval is, wordt klikt u vervolgens uw Azure-bestandsshares failover naar de gekoppelde regio in geval van een langdurige regionale uitval. Azure File Sync maakt gebruik van de dezelfde regionale koppelingen als opslag. Dus als u GRS-opslagaccounts gebruiken, moet u om in te schakelen als u meer URL's voor het toestaan van uw server om te communiceren met de gekoppelde regio voor Azure File Sync. De onderstaande tabel roept deze 'gepaarde regio'. Daarnaast is er een traffic manager-profiel-URL die moet ook worden ingeschakeld. Dit zorgt ervoor netwerkverkeer kan worden naadloos opnieuw doorgestuurd naar de gekoppelde regio in het geval van een failover en met de naam 'Detectie-URL' in de onderstaande tabel.

| Regio | Primair eindpunt-URL | Gekoppelde regio | Detectie-URL |
|--------|---------------------------------------|--------|---------------------------------------|
| Australië - oost | https://kailani-aue.one.microsoft.com | Australië Souteast | https://kailani-aue.one.microsoft.com |
| Australië - zuidoost | https://kailani-aus.one.microsoft.com | Australië - oost | https://tm-kailani-aus.one.microsoft.com |
| Canada - midden | https://kailani-cac.one.microsoft.com | Canada - oost | https://tm-kailani-cac.one.microsoft.com |
| Canada - oost | https://kailani-cae.one.microsoft.com | Canada - midden | https://tm-kailani.cae.one.microsoft.com |
| US - centraal | https://kailani-cus.one.microsoft.com | US - oost 2 | https://tm-kailani-cus.one.microsoft.com |
| Azië - oost | https://kailani11.one.microsoft.com | Azië - zuidoost | https://tm-kailani11.one.microsoft.com |
| US - oost | https://kailani1.one.microsoft.com | US - west | https://tm-kailani1.one.microsoft.com |
| US - oost 2 | https://kailani-ess.one.microsoft.com | US - centraal | https://tm-kailani-ess.one.microsoft.com |
| Europa - noord | https://kailani7.one.microsoft.com | Europa -west | https://tm-kailani7.one.microsoft.com |
| Azië - zuidoost | https://kailani10.one.microsoft.com | Azië - oost | https://tm-kailani10.one.microsoft.com |
| Verenigd Koninkrijk Zuid | https://kailani-uks.one.microsoft.com | Verenigd Koninkrijk West | https://tm-kailani-uks.one.microsoft.com |
| Verenigd Koninkrijk West | https://kailani-ukw.one.microsoft.com | Verenigd Koninkrijk Zuid | https://tm-kailani-ukw.one.microsoft.com |
| Europa -west | https://kailani6.one.microsoft.com | Europa - noord | https://tm-kailani6.one.microsoft.com |
| US - west | https://kailani.one.microsoft.com | US - oost | https://tm-kailani.one.microsoft.com |

- Als u lokaal redundant (LRS) of zone redundant (ZRS) storage-accounts gebruiken, moet u alleen de URL die wordt vermeld onder 'primaire eindpunt-URL' inschakelen.

- Als u wereldwijd redundant (GRS) storage-accounts gebruiken, schakelt u drie URL's.

**Voorbeeld:** implementeren van een opslagsynchronisatieservice in `"West US"` en registreer de server met het. De URL's waarmee de server om te communiceren met in dit geval zijn:

> - https://kailani.one.microsoft.com (primaire eindpunt: VS-West)
> - https://kailani1.one.microsoft.com (gekoppelde failover regio: VS-Oost)
> - https://tm-kailani.one.microsoft.com (detectie-URL van de primaire regio)

## <a name="summary-and-risk-limitation"></a>Samenvatting en risico-beperking
De lijsten die u eerder in dit document bevat de URL's met Azure File Sync momenteel communiceert met. Firewalls moeten in staat om toe te staan verkeer uitgaand naar deze domeinen zijn. Microsoft streeft ernaar te houden deze lijst bijgewerkt.

Instellen van het domein te beperken van firewall-regels mag een meting om beveiliging te verbeteren. Als deze firewallconfiguraties worden gebruikt, moet een Houd er rekening mee dat URL's worden toegevoegd en kunnen zelfs na verloop van tijd worden gewijzigd. Controleer regelmatig in dit artikel.

## <a name="next-steps"></a>Volgende stappen
- [Planning voor de implementatie van een Azure File Sync](storage-sync-files-planning.md)
- [Azure Files Sync implementeren](storage-sync-files-deployment-guide.md)
