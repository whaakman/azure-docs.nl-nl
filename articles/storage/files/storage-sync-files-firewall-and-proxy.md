---
title: Azure File Sync on-premises firewall en proxy-instellingen | Microsoft Docs
description: Azure File Sync on-premises netwerk configuratie
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 06/24/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 69f4c767b9fc1da90db021ffb3eb8704983ca69b
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2019
ms.locfileid: "68699307"
---
# <a name="azure-file-sync-proxy-and-firewall-settings"></a>Proxy- en firewallinstellingen van Azure File Sync
Azure File Sync maakt verbinding met uw on-premises servers met Azure Files, waarbij functies voor multi-site synchronisatie en Cloud lagen worden ingeschakeld. Als zodanig moet een on-premises server zijn verbonden met internet. Een IT-beheerder moet het beste pad bepalen dat de server kan bereiken in azure Cloud Services.

In dit artikel vindt u inzicht in specifieke vereisten en opties die beschikbaar zijn voor een geslaagde en veilige verbinding van uw server met Azure File Sync.

## <a name="overview"></a>Overzicht
Azure File Sync fungeert als een Orchestration-service tussen uw Windows-Server, uw Azure-bestands share en verschillende andere Azure-Services om gegevens te synchroniseren zoals beschreven in de synchronisatie groep. Azure File Sync om goed te laten werken, moet u uw servers configureren om te communiceren met de volgende Azure-Services:

- Azure Storage
- Azure File Sync
- Azure Resource Manager
- Verificatie services

> [!Note]  
> De Azure File Sync-agent op Windows server initieert alle aanvragen voor Cloud Services, waardoor het uitgaande verkeer van een firewall perspectief alleen hoeft te worden overwogen. <br /> Geen enkele Azure-service initieert een verbinding met de Azure File Sync-agent.

## <a name="ports"></a>Poorten
Azure File Sync verplaatst bestands gegevens en meta gegevens uitsluitend via HTTPS en vereist dat poort 443 open-uitgaand moet zijn.
Als gevolg hiervan wordt al het verkeer versleuteld.

## <a name="networks-and-special-connections-to-azure"></a>Netwerken en speciale verbindingen met Azure
De Azure File Sync-agent heeft geen vereisten met betrekking tot speciale kanalen zoals [ExpressRoute](../../expressroute/expressroute-introduction.md), etc. naar Azure.

Azure File Sync beschik bare manieren om in azure te kunnen bereiken, automatisch aan verschillende netwerk kenmerken, zoals band breedte, latentie en het bieden van beheer beheer voor het afstemmen. Niet alle functies zijn op dit moment beschikbaar. Als u specifiek gedrag wilt configureren, laat u ons weten via [Azure files UserVoice](https://feedback.azure.com/forums/217298-storage?category_id=180670).

## <a name="proxy"></a>Proxy
Azure File Sync biedt ondersteuning voor app-specifieke proxy-instellingen voor de hele machine.

Met **app-specifieke proxy-instellingen** kunt u een proxy configureren die specifiek is voor Azure file sync verkeer. App-specifieke proxy-instellingen worden ondersteund op Agent versie 4.0.1.0 of hoger en kunnen worden geconfigureerd tijdens de installatie van de agent of met behulp van de Power shell-cmdlet Set-StorageSyncProxyConfiguration.

Power shell-opdrachten voor het configureren van app-specifieke proxy-instellingen:
```powershell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Set-StorageSyncProxyConfiguration -Address <url> -Port <port number> -ProxyCredential <credentials>
```
**Proxy-instellingen** voor alle computers zijn transparant voor de Azure file sync-agent, omdat het hele verkeer van de server via de proxy wordt doorgestuurd.

Volg de onderstaande stappen voor het configureren van proxy-instellingen voor alle computers: 

1. Proxy-instellingen voor .NET-toepassingen configureren 

   - Bewerk deze twee bestanden:  
     C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config  
     C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config\machine.config

   - Voeg de sectie < System. net > in de machine. config-bestanden toe (onder het gedeelte < System. service model >).  Wijzig 127.0.01:8888 in het IP-adres en de poort voor de proxy server. 
     ```
      <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
          <proxy autoDetect="false" bypassonlocal="false" proxyaddress="http://127.0.0.1:8888" usesystemdefault="false" />
        </defaultProxy>
      </system.net>
     ```

2. De WinHTTP-proxy-instellingen instellen 

   - Voer de volgende opdracht uit vanaf een opdracht prompt met verhoogde bevoegdheid of Power shell om de bestaande proxy-instelling te bekijken:   

     Netsh WinHTTP show proxy

   - Voer de volgende opdracht uit vanaf een opdracht prompt met verhoogde bevoegdheid of Power shell om de proxy instelling in te stellen (127.0.01:8888 wijzigen in het IP-adres en de poort voor de proxy server):  

     Netsh WinHTTP set proxy 127.0.0.1:8888

3. Start de opslag synchronisatie Agent-service opnieuw door de volgende opdracht uit te voeren vanaf een opdracht prompt met verhoogde bevoegdheid of Power shell: 

      net stop filesyncsvc

      Opmerking: De filesyncsvc-service (Storage Sync agent) wordt automatisch gestart zodra deze is gestopt.

## <a name="firewall"></a>Firewall
Zoals vermeld in een vorige sectie, moet poort 443 open-uitgaand zijn. Afhankelijk van het beleid in uw Data Center, vertakking of regio kunt u het verkeer dat via deze poort wordt beperkt tot specifieke domeinen, mogelijk gewenst of vereist.

In de volgende tabel worden de vereiste domeinen voor communicatie beschreven:

| Service | Open bare-Cloud eindpunt | Azure Government-eind punt | Gebruik |
|---------|----------------|---------------|------------------------------|
| **Azure Resource Manager** | https://management.azure.com | https://management.usgovcloudapi.net | Elke gebruikers aanroep (zoals Power shell) gaat naar/via deze URL, met inbegrip van de eerste registratie aanroep van de server. |
| **Azure Active Directory** | https://login.windows.net | https://login.microsoftonline.us | Azure Resource Manager-aanroepen moeten worden uitgevoerd door een geverifieerde gebruiker. Deze URL wordt gebruikt voor gebruikers verificatie. |
| **Azure Active Directory** | https://graph.windows.net/ | https://graph.windows.net/ | Als onderdeel van de implementatie van Azure File Sync wordt een Service-Principal in de Azure Active Directory van het abonnement gemaakt. Deze URL wordt gebruikt voor die. Deze principal wordt gebruikt voor het delegeren van een minimale set rechten voor de Azure File Sync-Service. De gebruiker die de eerste installatie van Azure File Sync uitvoert, moet een geverifieerde gebruiker zijn met de bevoegdheid eigenaar van het abonnement. |
| **Azure Storage** | &ast;.core.windows.net | &ast;.core.usgovcloudapi.net | Wanneer een bestand door de server wordt gedownload, voert de server de gegevens efficiënter uit wanneer deze rechtstreeks met de Azure-bestands share in het opslag account communiceren. De server heeft een SAS-sleutel die alleen toegang tot de doel bestands shares toestaat. |
| **Azure File Sync** | &ast;.one.microsoft.com | &ast;.afs.azure.us | Na de eerste registratie van de server ontvangt de server een regionale URL voor het Azure File Sync service-exemplaar in die regio. De-server kan de URL gebruiken om rechtstreeks en efficiënt te communiceren met het exemplaar dat de synchronisatie afhandelt. |
| **Microsoft PKI** | `https://www.microsoft.com/pki/mscorp`<br /><http://ocsp.msocsp.com> | `https://www.microsoft.com/pki/mscorp`<br /><http://ocsp.msocsp.com> | Zodra de Azure File Sync-agent is geïnstalleerd, wordt de PKI-URL gebruikt voor het downloaden van tussenliggende certificaten die nodig zijn om te communiceren met de Azure File Sync-Service en de Azure-bestands share. De OCSP-URL wordt gebruikt om de status van een certificaat te controleren. |

> [!Important]
> Bij het toestaan van &ast;verkeer naar One.Microsoft.com, is verkeer naar meer dan alleen de synchronisatie service mogelijk van de server. Er zijn veel meer micro soft-services beschikbaar onder subdomeinen.

Als &ast;. One.Microsoft.com te breed is, kunt u de communicatie van de server beperken door communicatie toe te staan alleen expliciete regionale instanties van de Azure files-synchronisatie service. Welk exemplaar (en) u kiest, is afhankelijk van de regio van de opslag synchronisatie service die u hebt geïmplementeerd en de server hebt geregistreerd bij. Deze regio wordt ' primaire eind punt-URL ' genoemd in de volgende tabel.

Voor bedrijfs continuïteit en herstel na nood gevallen (BCDR) kunt u uw Azure-bestands shares opgeven in een globaal redundante (GRS)-opslag account. Als dat het geval is, zal de Azure-bestands shares een failover uitvoeren naar de gekoppelde regio in het geval van een blijvende regionale storing. Azure File Sync gebruikt dezelfde regionale koppeling als opslag. Als u dus GRS-opslag accounts gebruikt, moet u extra Url's inschakelen zodat uw server kan communiceren met de gekoppelde regio voor Azure File Sync. In de volgende tabel wordt deze ' gekoppelde regio ' aangeroepen. Daarnaast is er een Traffic Manager-profiel-URL die ook moet worden ingeschakeld. Zo zorgt u ervoor dat netwerk verkeer naadloos opnieuw kan worden gerouteerd naar de gekoppelde regio in het geval van een failover en wordt "detectie-URL" genoemd in de onderstaande tabel.

| Cloud  | Regio | URL van het primaire eind punt | Gekoppelde regio | Detectie-URL |
|--------|--------|----------------------|---------------|---------------|
| Public |Australië - oost | https:\//kailani-Aue.One.Microsoft.com | Australië - zuidoost | https:\//TM-kailani-Aue.One.Microsoft.com |
| Public |Australië - zuidoost | https:\//kailani-aus.One.Microsoft.com | Australië - oost | https:\//TM-kailani-aus.One.Microsoft.com |
| Public | Brazilië - zuid | https:\//brazilsouth01.afs.azure.net | US - zuid-centraal | https:\//TM-brazilsouth01.AFS.Azure.net |
| Public | Canada - midden | https:\//kailani-CAC.One.Microsoft.com | Canada - oost | https:\//TM-kailani-CAC.One.Microsoft.com |
| Public | Canada - oost | https:\//kailani-CAE.One.Microsoft.com | Canada - midden | https:\//TM-kailani.CAE.One.Microsoft.com |
| Public | India - centraal | https:\//kailani-cin.One.Microsoft.com | India - zuid | https:\//TM-kailani-cin.One.Microsoft.com |
| Public | US - centraal | https:\//kailani-cus.One.Microsoft.com | US - oost 2 | https:\//TM-kailani-cus.One.Microsoft.com |
| Public | Azië - oost | https:\//kailani11.one.microsoft.com | Azië - zuidoost | https:\//TM-kailani11.One.Microsoft.com |
| Public | East US | https:\//kailani1.one.microsoft.com | US - west | https:\//TM-kailani1.One.Microsoft.com |
| Public | US - oost 2 | https:\//kailani-ESS.One.Microsoft.com | US - centraal | https:\//TM-kailani-ESS.One.Microsoft.com |
| Public | Japan - oost | https:\//japaneast01.AFS.Azure.net | Japan - west | https:\//TM-japaneast01.AFS.Azure.net |
| Public | Japan - west | https:\//japanwest01.AFS.Azure.net | Japan - oost | https:\//TM-japanwest01.AFS.Azure.net |
| Public | Korea - centraal | https:\//koreacentral01.AFS.Azure.net/ | Korea - zuid | https:\//TM-koreacentral01.AFS.Azure.net/ |
| Public | Korea - zuid | https:\//koreasouth01.afs.azure.net/ | Korea - centraal | https:\//tm-koreasouth01.afs.azure.net/ |
| Public | US - noord-centraal | https:\//northcentralus01.AFS.Azure.net | US - zuid-centraal | https:\//TM-northcentralus01.AFS.Azure.net |
| Public | Europa - noord | https:\//kailani7.One.Microsoft.com | Europa -west | https:\//TM-kailani7.One.Microsoft.com |
| Public | US - zuid-centraal | https:\//southcentralus01.AFS.Azure.net | US - noord-centraal | https:\//TM-southcentralus01.AFS.Azure.net |
| Public | India - zuid | https:\//kailani-Sin.One.Microsoft.com | India - centraal | https:\//TM-kailani-Sin.One.Microsoft.com |
| Public | Azië - zuidoost | https:\//kailani10.One.Microsoft.com | Azië - oost | https:\//TM-kailani10.One.Microsoft.com |
| Public | Verenigd Koninkrijk Zuid | https:\//kailani-UKs.One.Microsoft.com | Verenigd Koninkrijk West | https:\//TM-kailani-UKs.One.Microsoft.com |
| Public | Verenigd Koninkrijk West | https:\//kailani-UKW.One.Microsoft.com | Verenigd Koninkrijk Zuid | https:\//TM-kailani-UKW.One.Microsoft.com |
| Public | US - west-centraal | https:\//westcentralus01.AFS.Azure.net | US - west 2 | https:\//TM-westcentralus01.AFS.Azure.net |
| Public | Europa -west | https:\//kailani6.one.microsoft.com | Europa - noord | https:\//TM-kailani6.One.Microsoft.com |
| Public | US - west | https:\//kailani.one.microsoft.com | East US | https:\//TM-kailani.One.Microsoft.com |
| Public | US - west 2 | https:\//westus201.AFS.Azure.net | US - west-centraal | https:\//tm-westus201.afs.azure.net |
| Overheid | VS (overheid) - Arizona | https:\//usgovarizona01.AFS.Azure.us | VS (overheid) - Texas | https:\//TM-usgovarizona01.AFS.Azure.us |
| Overheid | VS (overheid) - Texas | https:\//usgovtexas01.afs.azure.us | VS (overheid) - Arizona | https:\//tm-usgovtexas01.afs.azure.us |

- Als u lokaal redundante (LRS) of zone redundante (ZRS) opslag accounts gebruikt, hoeft u alleen de URL in te scha kelen die wordt weer gegeven onder URL van het primaire eind punt.

- Als u wereld wijd redundante opslag accounts (GRS) gebruikt, schakelt u drie Url's in.

**Voorbeeld:** U implementeert een opslag synchronisatie service `"West US"` in en registreert deze bij de server. De Url's waarmee de server kan communiceren voor deze aanvraag:

> - https:\//kailani.One.Microsoft.com (primair eind punt: VS-West)
> - https:\//kailani1.One.Microsoft.com (gekoppelde failover-overlopende regio: VS-Oost)
> - https:\//TM-kailani.One.Microsoft.com (detectie-URL van de primaire regio)

## <a name="summary-and-risk-limitation"></a>Samen vatting en risico beperking
De lijsten eerder in dit document bevatten de Url's Azure File Sync momenteel communiceert met. Firewalls moeten uitgaand verkeer naar deze domeinen kunnen toestaan. Micro soft streeft ernaar om deze lijst bijgewerkt te laten worden.

Het instellen van domein beperking van firewall regels kan een maat eenheid zijn voor het verbeteren van de beveiliging. Als deze firewall configuraties worden gebruikt, moet er een of meer Url's worden toegevoegd. Dit kan zelfs na verloop van tijd worden gewijzigd. Controleer dit artikel regel matig.

## <a name="next-steps"></a>Volgende stappen
- [Planning voor de implementatie van een Azure File Sync](storage-sync-files-planning.md)
- [Azure Files SYNC implementeren](storage-sync-files-deployment-guide.md)
- [Azure File Sync bewaken](storage-sync-files-monitoring.md)
