---
title: Opmerkingen bij de release van Microsoft Azure-Stack Development Kit | Microsoft Docs
description: Verbeteringen, correcties en bekende problemen voor Azure Stack Development Kit.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: brenduns
ms.reviewer: misainat
ms.openlocfilehash: 2aa6663ae598b32979411fd10e7bb8a2eacd21de
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="azure-stack-development-kit-release-notes"></a>Azure-Stack Development Kit release-opmerkingen
Deze releaseopmerkingen bevatten informatie over verbeteringen, correcties en bekende problemen in Azure Stack Development Kit. Als u niet zeker weet welke versie u uitvoert, kunt u [de portal gebruiken om te controleren](.\.\azure-stack-updates.md#determine-the-current-version).

> De hoogte blijven van wat is er nieuw in de ASDK Abonneer u op de [ ![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#) [feed](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#).


## <a name="build-201805131"></a>Build 20180513.1

### <a name="new-features"></a>Nieuwe functies 
Deze versie bevat de volgende verbeteringen en oplossingen voor Azure-Stack.  

- <!-- 1759172 - IS, ASDK --> **More granular administrative subscriptions**. With version 1804 and later, the Default Provider subscription is now complemented with two additional subscriptions. The additions facilitate separating the management of core infrastructure, additional resource providers, and workloads. The following three subscriptions are available:
  - *Standaard Provider-abonnement*. Dit abonnement voor basisinfrastructuur alleen gebruiken. Implementeer geen resources of resourceproviders voor dit abonnement.
  - *Softwarelicentiecontrole abonnement*. Gebruik dit abonnement voor de implementatie van resource provider. Resources die zijn geïmplementeerd voor dit abonnement niet in rekening worden gebracht.
  - *Abonnement voor verbruik*. Gebruik dit abonnement voor elke werkbelasting die u wilt implementeren. Resources geïmplementeerd hier normaal gebruik prijzen in rekening worden gebracht.


### <a name="fixed-issues"></a>Opgeloste problemen
- <!-- IS, ASDK -->  In the admin portal, you no longer have to refresh the Update tile before it displays information. 

- <!-- 2050709 - IS, ASDK -->  You can now use the admin portal to edit storage metrics for Blob service, Table service, and Queue service.

- <!-- IS, ASDK --> Under **Networking**, when you click **Connection** to set up a VPN connection, **Site-to-site (IPsec)** is now the only available option. 

- **Verschillende oplossingen** voor prestaties, stabiliteit, beveiliging en het besturingssysteem dat wordt gebruikt door Azure Stack

<!-- ### Changes  --> 
### <a name="additional-releases-timed-with-this-update"></a>Aanvullende versies is een time-out opgetreden met deze update  
De volgende zijn nu beschikbaar, maar geen Azure-Stack update 1804 vereist.
- **Bijwerken naar de Microsoft Azure-Stack System Center Operations Manager-Bewakingspakket**. Een nieuwe versie (1.0.3.0) van de Microsoft System Center Operations Manager Monitoring Pack voor Azure-Stack is beschikbaar voor [downloaden](https://www.microsoft.com/download/details.aspx?id=55184). Met deze versie kunt u de Service-Principals wanneer u een gekoppelde Azure-Stack-implementatie toevoegen. Deze versie biedt ook een updatebeheer ervaring die kunt u de herstelactie rechtstreeks vanuit binnen Operations Manager. Er zijn ook nieuwe dashboards die resourceproviders weergeven, schaaleenheden en schalen eenheid knooppunten.

- **Nieuwe Azure stapelen Admin PowerShell-versie 1.2.12**.  Azure-Stack PowerShell 1.2.12 is nu beschikbaar voor installatie. Deze versie bevat opdrachten voor alle resourceproviders van beheerder voor het beheren van Azure-Stack.  Sommige inhoud in deze versie wordt afgeschaft vanuit de Azure-Stack extra GitHub [opslagplaats](https://github.com/Azure/AzureStack-Tools). 

   Ga als volgt voor details over de installatie, de [instructies](.\.\azure-stack-powershell-install.md) of de [help](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.2.12) voor Azure Stack Module 1.2.12 inhoud. 

- **Initiële release van Azure Rest Stack API-verwijzing**. De API-verwijzing voor alle Azure-Stack Admin Resource Provider is nu gepubliceerd

### <a name="known-issues"></a>Bekende problemen
 
#### <a name="portal"></a>Portal
- <!-- TBD - IS ASDK --> The ability [to open a new support request from the dropdown](.\.\azure-stack-manage-portals.md#quick-access-to-help-and-support) from within the administrator portal isn’t available. Instead, use the following link:     
    - Gebruik voor Azure Stack Development Kit https://aka.ms/azurestackforum.    

- <!-- 2403291 - IS ASDK --> You might not have use of the horizontal scroll bar along the bottom of the admin and user portals. If you can’t access the horizontal scroll bar, use the breadcrumbs to navigate to a previous blade in the portal by selecting the name of the blade you want to view from the breadcrumb list found at the top left of the portal.
  ![Breadcrumb](media/asdk-release-notes/breadcrumb.png)

- <!-- TBD -  IS ASDK --> Deleting user subscriptions results in orphaned resources. As a workaround, first delete user resources or the entire resource group, and then delete user subscriptions.

- <!-- TBD -  IS ASDK --> You cannot view permissions to your subscription using the Azure Stack portals. As a workaround, use PowerShell to verify permissions.

-   <!-- TBD -  IS ASDK --> In the admin portal, you might see a critical alert for the Microsoft.Update.Admin component. The Alert name, description, and remediation all display as:  
    - *Fout - sjabloon voor FaultType ResourceProviderTimeout ontbreekt.*

    Deze waarschuwing kan worden genegeerd. 

#### <a name="compute"></a>Compute
- <!-- TBD -  IS ASDK --> Scaling settings for virtual machine scale sets are not available in the portal. As a workaround, you can use [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Because of PowerShell version differences, you must use the `-Name` parameter instead of `-VMScaleSetName`.

- <!-- TBD -  IS ASDK --> When you create virtual machines on the Azure Stack user portal, the portal displays an incorrect number of data disks that can attach to a DS series VM. DS series VMs can accommodate as many data disks as the Azure configuration.

- <!-- TBD -  IS ASDK --> When a VM image fails to be created, a failed item that you cannot delete might be added to the VM images compute blade.

  Als tijdelijke oplossing door een nieuwe VM-installatiekopie te maken met een dummy VHD die kan worden gemaakt via de Hyper-V (New-VHD-pad C:\dummy.vhd-vaste - SizeBytes 1 GB). Dit proces los het probleem dat verhindert dat het mislukte item wordt verwijderd. Vervolgens kunt 15 minuten na het maken van de installatiekopie van het dummy, u met succes verwijderen.

  U kunt vervolgens redownload van de VM-installatiekopie die eerder is mislukt.

- <!-- TBD -  IS ASDK --> If provisioning an extension on a VM deployment takes too long, users should let the provisioning time-out instead of trying to stop the process to deallocate or delete the VM.  

- <!-- 1662991 - IS ASDK --> Linux VM diagnostics is not supported in Azure Stack. When you deploy a Linux VM with VM diagnostics enabled, the deployment fails. The deployment also fails if you enable the Linux VM basic metrics through diagnostic settings. 

#### <a name="networking"></a>Netwerken
- <!-- 1766332 - IS, ASDK --> Under **Networking**, if you click **Create VPN Gateway** to set up a VPN connection, **Policy Based** is listed as a VPN type. Do not select this option. Only the **Route Based** option is supported in Azure Stack.

- <!-- 2388980 -  IS ASDK --> After a VM is created and associated with a public IP address, you can't disassociate that VM from that IP address. Disassociation appears to work, but the previously assigned public IP address remains associated with the original VM.

  Op dit moment moet u alleen nieuwe openbare IP-adressen voor de nieuwe virtuele machines die u maakt.

  Dit gebeurt zelfs als u een nieuwe virtuele machine het IP-adres toewijzen (gewoonlijk aangeduid als een *VIP's wisselen*). Alle toekomstige verbinding probeert te maken via dit resultaat van de IP-adres in van een verbinding met de oorspronkelijk gekoppelde virtuele machine en niet naar het nieuwe bericht.

- <!-- 2292271 - IS ASDK --> If you raise a Quota limit for a Network resource that is part of an Offer and Plan that is associated with a tenant subscription, the new limit is not applied to that subscription. However, the new limit does apply to new subscriptions that are created after the quota is increased. 

  Gebruik een invoegtoepassing plan quotum te verhogen een netwerk wanneer het abonnement al gekoppeld aan een abonnement is om dit probleem omzeilen. Zie voor meer informatie hoe [beschikbaar maken van een plan voor de invoegtoepassing](.\.\azure-stack-subscribe-plan-provision-vm.md#to-make-an-add-on-plan-available).

- <!-- 2304134 IS ASDK --> You cannot delete a subscription that has DNS Zone resources or Route Table resources associated with it. To successfully delete the subscription, you must first delete DNS Zone and Route Table resources from the tenant subscription. 


- <!-- 1902460 -  IS ASDK --> Azure Stack supports a single *local network gateway* per IP address. This is true across all tenant subscriptions. After the creation of the first local network gateway connection, subsequent attempts to create a local network gateway resource with the same IP address are blocked.

- <!-- 16309153 -  IS ASDK --> On a Virtual Network that was created with a DNS Server setting of *Automatic*, changing to a custom DNS Server fails. The updated settings are not pushed to VMs in that Vnet.
 
- <!-- TBD -  IS ASDK --> Azure Stack does not support adding additional network interfaces to a VM instance after the VM is deployed. If the VM requires more than one network interface, they must be defined at deployment time.


#### <a name="sql-and-mysql"></a>SQL- en MySQL 
- <!-- TBD - ASDK --> The database hosting servers must be dedicated for use by the resource provider and user workloads. You cannot use an instance that is being used by any other consumer, including App Services.

- <!-- IS, ASDK --> Special characters, including spaces and periods, are not supported in the **Family** name when you create a SKU for the SQL and MySQL resource providers. 

#### <a name="app-service"></a>App Service
- <!-- TBD -  IS ASDK --> Users must register the storage resource provider before they create their first Azure Function in the subscription.

- <!-- TBD -  IS ASDK --> In order to scale out infrastructure (workers, management, front-end roles), you must use PowerShell as described in the release notes for Compute.
 
#### <a name="usage"></a>Gebruik  
- <!-- TBD -  IS ASDK --> Usage Public IP address usage meter data shows the same *EventDateTime* value for each record instead of the *TimeDate* stamp that shows when the record was created. Currently, you can’t use this data to perform accurate accounting of public IP address usage.

<!-- #### Identity -->






## <a name="build-201803291"></a>Build 20180329.1

### <a name="new-features-and-fixes"></a>Nieuwe functies en oplossingen
De nieuwe functies en oplossingen die zijn vrijgegeven voor Azure-Stack geïntegreerde systemen versie 1803 van toepassing op de Azure-Stack Development Kit. Zie de [nieuwe functies](.\.\azure-stack-update-1803.md#new-features) en [problemen opgelost](.\.\azure-stack-update-1803.md#fixed-issues) secties van de Azure-Stack 1803 bijwerken releaseopmerkingen voor meer informatie.  
> [!IMPORTANT]    
> Sommige van de items in de **nieuwe functies** en **problemen opgelost** secties alleen relevant zijn voor Azure-Stack geïntegreerd systemen.

### <a name="changes"></a>Wijzigingen
- De manier wijzigen van de status van een nieuwe aanbieding van *persoonlijke* naar *openbare* of *buiten gebruik gestelde* is gewijzigd. Zie voor meer informatie [maken van een aanbieding](.\.\azure-stack-create-offer.md). 


### <a name="known-issues"></a>Bekende problemen
 
#### <a name="portal"></a>Portal
- De mogelijkheid [een nieuwe ondersteuningsaanvraag openen vanuit de vervolgkeuzelijst](.\.\azure-stack-manage-portals.md#quick-access-to-help-and-support) uit binnen de beheerder van de portal is niet beschikbaar. Gebruik in plaats daarvan de volgende koppeling:     
    - Gebruik voor Azure Stack Development Kit https://aka.ms/azurestackforum.    

- <!-- 2050709 --> In the admin portal, it is not possible to edit storage metrics for Blob service, Table service, or Queue service. When you go to Storage, and then select the blob, table, or queue service tile, a new blade opens that displays a metrics chart for that service. If you then select Edit from the top of the metrics chart tile, the Edit Chart blade opens but does not display options to edit metrics.  

- U ziet een **activering vereist** waarschuwingsmelding dat aangeeft dat het registreren van uw Azure-Stack Development Kit noodzakelijk. Dit is verwacht gedrag.

- Gebruiker abonnementen resulteert in een zwevende resources te verwijderen. Als tijdelijke oplossing Gebruikersbronnen of de hele resourcegroep eerst te verwijderen en verwijder vervolgens de gebruikersabonnementen.

- U kunt machtigingen aan uw abonnement met behulp van de portals Azure Stack niet weergeven. Als tijdelijke oplossing PowerShell te gebruiken om machtigingen te controleren.

- In het dashboard van de beheerportal mislukt de Update-tegel om meer informatie over updates weer te geven. U lost dit probleem, klikt u op de tegel te vernieuwen.

-   In het beheerportal ziet u mogelijk een kritieke waarschuwing voor het onderdeel Microsoft.Update.Admin. De naam van waarschuwing, beschrijving en herstel alle als weergegeven:  
    - *Fout - sjabloon voor FaultType ResourceProviderTimeout ontbreekt.*

    Deze waarschuwing kan worden genegeerd. 



#### <a name="marketplace"></a>Marketplace
- Gebruikers de volledige marketplace zonder een abonnement kunnen bladeren en beheeritems zoals plannen en aanbiedingen kunnen zien. Deze items zijn niet-functioneel voor gebruikers.
 
#### <a name="compute"></a>Compute
- Instellingen voor virtuele-machineschaalsets schalen zijn niet beschikbaar in de portal. Als tijdelijke oplossing kunt u [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Vanwege verschillen tussen versies van PowerShell, moet u de `-Name` parameter in plaats van `-VMScaleSetName`.

- Wanneer u virtuele machines op de gebruikersportal van Azure-Stack maakt, wordt een onjuist aantal gegevensschijven dat aan een DS-serie VM koppelen kunt in de portal weergegeven. DS-serie VMs aankan zo veel gegevensschijven als de configuratie van de Azure.

- Wanneer een installatiekopie van een virtuele machine niet kan worden gemaakt, kan een mislukte item dat u niet verwijderen, worden toegevoegd aan de VM-installatiekopieën compute-blade.

  Als tijdelijke oplossing door een nieuwe VM-installatiekopie te maken met een dummy VHD die kan worden gemaakt via de Hyper-V (New-VHD-pad C:\dummy.vhd-vaste - SizeBytes 1 GB). Dit proces los het probleem dat verhindert dat het mislukte item wordt verwijderd. Vervolgens kunt 15 minuten na het maken van de installatiekopie van het dummy, u met succes verwijderen.

  U kunt vervolgens redownload van de VM-installatiekopie die eerder is mislukt.

-  Als u een uitbreiding op de implementatie van een VM-inrichting te lang duurt, door gebruikers laten de inrichting time-outwaarde in plaats van bij het stoppen van het proces voor de toewijzing ongedaan maken of verwijderen van de virtuele machine.  

- <!-- 1662991 --> Linux VM diagnostics is not supported in Azure Stack. When you deploy a Linux VM with VM diagnostics enabled, the deployment fails. The deployment also fails if you enable the Linux VM basic metrics through diagnostic settings. 


#### <a name="networking"></a>Netwerken
- Onder **Networking**, als u op **verbinding** voor het instellen van een VPN-verbinding, **VNet-naar-VNet** wordt vermeld als een mogelijke verbindingstype. Selecteer deze optie niet. Op dit moment wordt alleen de **Site-naar-site (IPsec)** optie wordt ondersteund.

- Nadat een virtuele machine is gemaakt en gekoppeld aan een openbaar IP-adres, kunt u die virtuele machine uit dat IP-adres kan niet loskoppelen. Disassociation lijkt te werken, maar de eerder toegewezen openbaar IP-adres blijft gekoppeld aan de oorspronkelijke virtuele machine.

  Op dit moment moet u alleen nieuwe openbare IP-adressen voor de nieuwe virtuele machines die u maakt.

  Dit gebeurt zelfs als u een nieuwe virtuele machine het IP-adres toewijzen (gewoonlijk aangeduid als een *VIP's wisselen*). Alle toekomstige verbinding probeert te maken via dit resultaat van de IP-adres in van een verbinding met de oorspronkelijk gekoppelde virtuele machine en niet naar het nieuwe bericht.



- Azure-Stack ondersteunt één *lokale netwerkgateway* per IP-adres. Dit geldt voor alle abonnementen van de tenant. Nadat het maken van de eerste lokale gateway netwerkverbinding, daaropvolgende pogingen tot het maken van een lokale gateway netwerkbron met hetzelfde IP-adres worden geblokkeerd.

- Op een virtueel netwerk dat is gemaakt met een DNS-Server-instelling van *automatische*, wijzigen voor een aangepaste DNS-Server mislukt. De bijgewerkte instellingen zijn niet doorgevoerd in de virtuele machines in dit Vnet.
 
- Azure-Stack biedt geen ondersteuning voor extra netwerkinterfaces toevoegen aan een VM-instantie nadat de virtuele machine is geïmplementeerd. Als de virtuele machine meer dan één netwerkinterface vereist, moeten ze tijdens de implementatie worden gedefinieerd.



#### <a name="sql-and-mysql"></a>SQL- en MySQL 
- Het kan maximaal één uur voordat gebruikers databases in een nieuwe SQL- of MySQL SKU maken kunnen duren.

- De database die als host fungeert voor servers moet worden toegewezen voor gebruik door de resource provider en werkbelastingen. U kunt een exemplaar dat wordt gebruikt door andere verbruiker, met inbegrip van App-Services niet gebruiken.

- <!-- IS, ASDK --> Special characters, including spaces and periods, are not supported in the **Family** or **Tier** names when you create a SKU for the SQL and MySQL resource providers.

#### <a name="app-service"></a>App Service
- Gebruikers moeten de storage resourceprovider registreren voordat ze hun eerste Azure-functie in het abonnement maken.

- Om de infrastructuur (werknemers, beheer, front-rollen) uitbreiden, moet u PowerShell gebruiken, zoals beschreven in de release-opmerkingen voor Compute.
 
#### <a name="usage"></a>Gebruik  
- Gebruik van openbare IP-adres-gebruiksgegevens meter toont dezelfde *datum/tijd gebeurtenis* waarde voor elke record in plaats van de *TimeDate* tijdstempel die wordt aangegeven wanneer de record is gemaakt. U niet op dit moment kunnen deze gegevens gebruiken om uit te voeren nauwkeurige registratie van het gebruik van openbare IP-adres.
<!--
#### Identity
-->

#### <a name="downloading-azure-stack-tools-from-github"></a>Hulpprogramma's voor Azure Stack downloaden vanuit GitHub
- Wanneer u de *aanroepen webrequest* PowerShell-cmdlet voor het downloaden van de Azure-Stack van hulpprogramma's voor vanuit Github, er een foutbericht:     
    -  *aanroepen webrequest: de aanvraag is afgebroken: kan het beveiligde SSL/TLS-kanaal niet maken.*     

  Deze fout treedt op omdat een recente GitHub ondersteuning afschaffing van de cryptografische standaarden Tlsv1 en Tlsv1.1 (de standaardinstelling voor PowerShell). Zie voor meer informatie [zwakke cryptografische standaarden verwijdering kennisgeving](https://githubengineering.com/crypto-removal-notice/).

  U lost dit probleem, toevoegen `[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12` boven aan het script om af te dwingen de PowerShell-console TLSv1.2 gebruiken bij het downloaden van de GitHub-opslagplaatsen.






## <a name="build-201803021"></a>Build 20180302.1

### <a name="new-features-and-fixes"></a>Nieuwe functies en oplossingen
Zie de [nieuwe functies en oplossingen](.\.\azure-stack-update-1802.md#new-features-and-fixes) sectie van de Azure-Stack 1802 update release-opmerkingen voor Azure-Stack geïntegreerd systemen.

> [!IMPORTANT]    
> Sommige van de items in de **nieuwe functies en oplossingen** sectie alleen relevant zijn voor Azure-Stack geïntegreerd systemen.


### <a name="known-issues"></a>Bekende problemen
 
#### <a name="portal"></a>Portal
- De mogelijkheid [een nieuwe ondersteuningsaanvraag openen vanuit de vervolgkeuzelijst](.\.\azure-stack-manage-portals.md#quick-access-to-help-and-support) uit binnen de beheerder van de portal is niet beschikbaar. Gebruik in plaats daarvan de volgende koppeling:     
    - Gebruik voor Azure Stack Development Kit https://aka.ms/azurestackforum.    

- <!-- 2050709 --> In the admin portal, it is not possible to edit storage metrics for Blob service, Table service, or Queue service. When you go to Storage, and then select the blob, table, or queue service tile, a new blade opens that displays a metrics chart for that service. If you then select Edit from the top of the metrics chart tile, the Edit Chart blade opens but does not display options to edit metrics.  

- U ziet een **activering vereist** waarschuwingsmelding dat aangeeft dat het registreren van uw Azure-Stack Development Kit noodzakelijk. Dit is verwacht gedrag.

- Gebruiker abonnementen resulteert in een zwevende resources te verwijderen. Als tijdelijke oplossing Gebruikersbronnen of de hele resourcegroep eerst te verwijderen en verwijder vervolgens de gebruikersabonnementen.

- U kunt machtigingen aan uw abonnement met behulp van de portals Azure Stack niet weergeven. Als tijdelijke oplossing PowerShell te gebruiken om machtigingen te controleren.

- In het dashboard van de beheerportal mislukt de Update-tegel om meer informatie over updates weer te geven. U lost dit probleem, klikt u op de tegel te vernieuwen.

-   In het beheerportal ziet u mogelijk een kritieke waarschuwing voor het onderdeel Microsoft.Update.Admin. De naam van waarschuwing, beschrijving en herstel alle als weergegeven:  
    - *Fout - sjabloon voor FaultType ResourceProviderTimeout ontbreekt.*

    Deze waarschuwing kan worden genegeerd. 

- In de beheerportal en gebruikersportal, de overzichtsblade niet kan worden geladen bij het selecteren van de blade overzicht van de storage-accounts die zijn gemaakt met een oudere versie van de API (voorbeeld: 2015-06-15). 

  Als tijdelijke oplossing kunt u PowerShell gebruiken om uit te voeren de **Start ResourceSynchronization.ps1** script toegang om de accountdetails van de opslag te herstellen. [Het script is beschikbaar via GitHub]( https://github.com/Azure/AzureStack-Tools/tree/master/Support/scripts), en moet worden uitgevoerd met service-beheerdersreferenties op de host van development kit als u de ASDK gebruikt.  

- De **servicestatus** blade niet kan worden geladen. Wanneer u de status van de Service-blade opent in de beheerder of de gebruiker-portal, Azure-Stack een fout wordt weergegeven en wordt de informatie niet geladen. Dit is verwacht gedrag. Hoewel u kunt selecteren en servicestatus opent, wordt deze functie is nog niet beschikbaar maar zullen worden uitgevoerd in een toekomstige versie van Azure-Stack.

#### <a name="health-and-monitoring"></a>Status en bewaking
In de Stack van Azure-beheerportal, ziet u mogelijk een kritieke waarschuwing met de naam **in behandeling zijnde vervaldatum van het externe certificaat**.  Deze waarschuwing kan worden genegeerd en heeft invloed op de bewerkingen van de Azure-Stack Development Kit. 


#### <a name="marketplace"></a>Marketplace
- Gebruikers de volledige marketplace zonder een abonnement kunnen bladeren en beheeritems zoals plannen en aanbiedingen kunnen zien. Deze items zijn niet-functioneel voor gebruikers.
 
#### <a name="compute"></a>Compute
- Instellingen voor virtuele-machineschaalsets schalen zijn niet beschikbaar in de portal. Als tijdelijke oplossing kunt u [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Vanwege verschillen tussen versies van PowerShell, moet u de `-Name` parameter in plaats van `-VMScaleSetName`.

- Azure-Stack ondersteunt het gebruik van alleen vaste type VHD's. Enkele afbeeldingen die worden aangeboden via de marketplace op Azure-Stack dynamische VHD's gebruiken maar die zijn verwijderd. Vergroten of verkleinen van een virtuele machine (VM) met een dynamische schijf is gekoppeld, blijft de virtuele machine een mislukte status.

  Om dit probleem beperken, verwijdert u de virtuele machine zonder te verwijderen van de VM-schijf, een VHD-blob in een opslagaccount. Vervolgens de VHD van een dynamische schijf converteren naar een vaste schijf en de virtuele machine opnieuw maken.

- Wanneer u virtuele machines op de gebruikersportal van Azure-Stack maakt, wordt een onjuist aantal gegevensschijven dat aan een DS-serie VM koppelen kunt in de portal weergegeven. DS-serie VMs aankan zo veel gegevensschijven als de configuratie van de Azure.

- Wanneer een installatiekopie van een virtuele machine niet kan worden gemaakt, kan een mislukte item dat u niet verwijderen, worden toegevoegd aan de VM-installatiekopieën compute-blade.

  Als tijdelijke oplossing door een nieuwe VM-installatiekopie te maken met een dummy VHD die kan worden gemaakt via de Hyper-V (New-VHD-pad C:\dummy.vhd-vaste - SizeBytes 1 GB). Dit proces los het probleem dat verhindert dat het mislukte item wordt verwijderd. Vervolgens kunt 15 minuten na het maken van de installatiekopie van het dummy, u met succes verwijderen.

  U kunt vervolgens redownload van de VM-installatiekopie die eerder is mislukt.

-  Als u een uitbreiding op de implementatie van een VM-inrichting te lang duurt, door gebruikers laten de inrichting time-outwaarde in plaats van bij het stoppen van het proces voor de toewijzing ongedaan maken of verwijderen van de virtuele machine.  

- <!-- 1662991 --> Linux VM diagnostics is not supported in Azure Stack. When you deploy a Linux VM with VM diagnostics enabled, the deployment fails. The deployment also fails if you enable the Linux VM basic metrics through diagnostic settings. 


#### <a name="networking"></a>Netwerken
- Onder **Networking**, als u op **verbinding** voor het instellen van een VPN-verbinding, **VNet-naar-VNet** wordt vermeld als een mogelijke verbindingstype. Selecteer deze optie niet. Op dit moment wordt alleen de **Site-naar-site (IPsec)** optie wordt ondersteund.

- Nadat een virtuele machine is gemaakt en gekoppeld aan een openbaar IP-adres, kunt u die virtuele machine uit dat IP-adres kan niet loskoppelen. Disassociation lijkt te werken, maar de eerder toegewezen openbaar IP-adres blijft gekoppeld aan de oorspronkelijke virtuele machine.

  Op dit moment moet u alleen nieuwe openbare IP-adressen voor de nieuwe virtuele machines die u maakt.

  Dit gebeurt zelfs als u een nieuwe virtuele machine het IP-adres toewijzen (gewoonlijk aangeduid als een *VIP's wisselen*). Alle toekomstige verbinding probeert te maken via dit resultaat van de IP-adres in van een verbinding met de oorspronkelijk gekoppelde virtuele machine en niet naar het nieuwe bericht.

-   De functie doorsturen via IP is zichtbaar in de portal, maar inschakelen doorsturen via IP heeft geen effect. Deze functie is nog niet ondersteund.

- Azure-Stack ondersteunt één *lokale netwerkgateway* per IP-adres. Dit geldt voor alle abonnementen van de tenant. Nadat het maken van de eerste lokale gateway netwerkverbinding, daaropvolgende pogingen tot het maken van een lokale gateway netwerkbron met hetzelfde IP-adres worden geblokkeerd.

- Op een virtueel netwerk dat is gemaakt met een DNS-Server-instelling van *automatische*, wijzigen voor een aangepaste DNS-Server mislukt. De bijgewerkte instellingen zijn niet doorgevoerd in de virtuele machines in dit Vnet.
 
- Azure-Stack biedt geen ondersteuning voor extra netwerkinterfaces toevoegen aan een VM-instantie nadat de virtuele machine is geïmplementeerd. Als de virtuele machine meer dan één netwerkinterface vereist, moeten ze tijdens de implementatie worden gedefinieerd.



#### <a name="sql-and-mysql"></a>SQL- en MySQL 
- Het kan maximaal één uur voordat gebruikers databases in een nieuwe SQL- of MySQL SKU maken kunnen duren.

- De database die als host fungeert voor servers moet worden toegewezen voor gebruik door de resource provider en werkbelastingen. U kunt een exemplaar dat wordt gebruikt door andere verbruiker, met inbegrip van App-Services niet gebruiken.

- <!-- IS, ASDK --> Special characters, including spaces and periods, are not supported in the **Family** name when you create a SKU for the SQL and MySQL resource providers.

#### <a name="app-service"></a>App Service
- Gebruikers moeten de storage resourceprovider registreren voordat ze hun eerste Azure-functie in het abonnement maken.

- Om de infrastructuur (werknemers, beheer, front-rollen) uitbreiden, moet u PowerShell gebruiken, zoals beschreven in de release-opmerkingen voor Compute.
 
#### <a name="usage"></a>Gebruik  
- Gebruik van openbare IP-adres-gebruiksgegevens meter toont dezelfde *datum/tijd gebeurtenis* waarde voor elke record in plaats van de *TimeDate* tijdstempel die wordt aangegeven wanneer de record is gemaakt. U niet op dit moment kunnen deze gegevens gebruiken om uit te voeren nauwkeurige registratie van het gebruik van openbare IP-adres.
<!--
#### Identity
-->

#### <a name="downloading-azure-stack-tools-from-github"></a>Hulpprogramma's voor Azure Stack downloaden vanuit GitHub
- Wanneer u de *aanroepen webrequest* PowerShell-cmdlet voor het downloaden van de Azure-Stack van hulpprogramma's voor vanuit Github, er een foutbericht:     
    -  *aanroepen webrequest: de aanvraag is afgebroken: kan het beveiligde SSL/TLS-kanaal niet maken.*     

  Deze fout treedt op omdat een recente GitHub ondersteuning afschaffing van de cryptografische standaarden Tlsv1 en Tlsv1.1 (de standaardinstelling voor PowerShell). Zie voor meer informatie [zwakke cryptografische standaarden verwijdering kennisgeving](https://githubengineering.com/crypto-removal-notice/).

  U lost dit probleem, toevoegen `[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12` boven aan het script om af te dwingen de PowerShell-console TLSv1.2 gebruiken bij het downloaden van de GitHub-opslagplaatsen.

