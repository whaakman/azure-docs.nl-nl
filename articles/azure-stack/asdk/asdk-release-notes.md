---
title: Opmerkingen bij de release van Microsoft Azure-Stack Development Kit | Microsoft Docs
description: Verbeteringen, correcties en bekende problemen voor Azure Stack Development Kit.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 6b08c1793857fd6c6a6a04c0d450e76a36357597
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="azure-stack-development-kit-release-notes"></a>Azure-Stack Development Kit release-opmerkingen
Deze releaseopmerkingen bevatten informatie over verbeteringen, correcties en bekende problemen in Azure Stack Development Kit. Als u niet zeker weet welke versie u uitvoert, kunt u [de portal gebruiken om te controleren](.\.\azure-stack-updates.md#determine-the-current-version).

> Product up-to-date houden met wat is er nieuw in de ASDK Abonneer u op de [ ![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#) [feed](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#).

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

- Wanneer u de eigenschappen van een resource of resourcegroep bekijkt de **verplaatsen** knop is uitgeschakeld. Dit is verwacht gedrag. Verplaatsen van resources of resourcegroepen tussen resourcegroepen of abonnementen is momenteel niet ondersteund.
 
- U ziet een **activering vereist** waarschuwingsmelding dat aangeeft dat het registreren van uw Azure-Stack Development Kit noodzakelijk. Dit is verwacht gedrag.

- Gebruiker abonnementen resulteert in een zwevende resources te verwijderen. Als tijdelijke oplossing Gebruikersbronnen of de hele resourcegroep eerst te verwijderen en verwijder vervolgens de gebruikersabonnementen.

- U kunt machtigingen aan uw abonnement met behulp van de portals Azure Stack niet weergeven. Als tijdelijke oplossing PowerShell te gebruiken om machtigingen te controleren.

- In het dashboard van de beheerportal mislukt de Update-tegel om meer informatie over updates weer te geven. U lost dit probleem, klikt u op de tegel te vernieuwen.

-   In het beheerportal ziet u mogelijk een kritieke waarschuwing voor het onderdeel Microsoft.Update.Admin. De naam van waarschuwing, beschrijving en herstel alle als weergegeven:  
    - *Fout - sjabloon voor FaultType ResourceProviderTimeout ontbreekt.*

    Deze waarschuwing kan worden genegeerd. 

- In de beheerportal en gebruikersportal, de overzichtsblade niet kan worden geladen bij het selecteren van de blade overzicht van de storage-accounts die zijn gemaakt met een oudere versie van de API (voorbeeld: 2015-06-15). 

  Als tijdelijke oplossing kunt u PowerShell gebruiken om uit te voeren de **Start ResourceSynchronization.ps1** script toegang om de accountdetails van de opslag te herstellen. [Het script is beschikbaar via GitHub]( https://github.com/Azure/AzureStack-Tools/tree/master/Support/scripts), en moet worden uitgevoerd met service-beheerdersreferenties op de host van development kit als u de ASDK gebruikt.  


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

- Interne Load Balancing (ILB) verwerkt niet goed MAC-adressen voor back-end virtuele machines, waardoor ILB verbreken wanneer u Linux, alle exemplaren in de Back-End-netwerk.  ILB werkt goed samen met Windows-exemplaren in de Back-End-netwerk.

-   De functie doorsturen via IP is zichtbaar in de portal, maar inschakelen doorsturen via IP heeft geen effect. Deze functie is nog niet ondersteund.

- Azure-Stack ondersteunt één *lokale netwerkgateway* per IP-adres. Dit geldt voor alle abonnementen van de tenant. Nadat het maken van de eerste lokale gateway netwerkverbinding, daaropvolgende pogingen tot het maken van een lokale gateway netwerkbron met hetzelfde IP-adres worden geblokkeerd.

- Op een virtueel netwerk dat is gemaakt met een DNS-Server-instelling van *automatische*, wijzigen voor een aangepaste DNS-Server mislukt. De bijgewerkte instellingen zijn niet doorgevoerd in de virtuele machines in dit Vnet.
 
- Azure-Stack biedt geen ondersteuning voor extra netwerkinterfaces toevoegen aan een VM-instantie nadat de virtuele machine is geïmplementeerd. Als de virtuele machine meer dan één netwerkinterface vereist, moeten ze tijdens de implementatie worden gedefinieerd.

-   <!-- 2096388 --> You cannot use the admin portal to update rules for a network security group. 

    Tijdelijke oplossing voor App Service: als u extern bureaublad op de exemplaren van de domeincontroller moet, u wijzigt de beveiligingsregels voor verbindingen binnen de netwerkbeveiligingsgroepen met PowerShell.  Hieronder vindt u voorbeelden van hoe *toestaan*, en zet u de configuratie van *weigeren*: 
    
    - *Toestaan:*
 
      ```powershell    
      Login-AzureRMAccount -EnvironmentName AzureStackAdmin
      
      $nsg = Get-AzureRmNetworkSecurityGroup -Name "ControllersNsg" -ResourceGroupName "AppService.local"
      
      $RuleConfig_Inbound_Rdp_3389 =  $nsg | Get-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389"
      
      ##This doesn’t work. Need to set properties again even in case of edit
      
      #Set-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389" -NetworkSecurityGroup $nsg -Access Allow  
      
      Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
        -Name $RuleConfig_Inbound_Rdp_3389.Name `
        -Description "Inbound_Rdp_3389" `
        -Access Allow `
        -Protocol $RuleConfig_Inbound_Rdp_3389.Protocol `
        -Direction $RuleConfig_Inbound_Rdp_3389.Direction `
        -Priority $RuleConfig_Inbound_Rdp_3389.Priority `
        -SourceAddressPrefix $RuleConfig_Inbound_Rdp_3389.SourceAddressPrefix `
        -SourcePortRange $RuleConfig_Inbound_Rdp_3389.SourcePortRange `
        -DestinationAddressPrefix $RuleConfig_Inbound_Rdp_3389.DestinationAddressPrefix `
        -DestinationPortRange $RuleConfig_Inbound_Rdp_3389.DestinationPortRange
      
      # Commit the changes back to NSG
      Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
      ```

    - *Weigeren:*

        ```powershell
        
        Login-AzureRMAccount -EnvironmentName AzureStackAdmin
        
        $nsg = Get-AzureRmNetworkSecurityGroup -Name "ControllersNsg" -ResourceGroupName "AppService.local"
        
        $RuleConfig_Inbound_Rdp_3389 =  $nsg | Get-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389"
        
        ##This doesn’t work. Need to set properties again even in case of edit
    
        #Set-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389" -NetworkSecurityGroup $nsg -Access Allow  
    
        Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
          -Name $RuleConfig_Inbound_Rdp_3389.Name `
          -Description "Inbound_Rdp_3389" `
          -Access Deny `
          -Protocol $RuleConfig_Inbound_Rdp_3389.Protocol `
          -Direction $RuleConfig_Inbound_Rdp_3389.Direction `
          -Priority $RuleConfig_Inbound_Rdp_3389.Priority `
          -SourceAddressPrefix $RuleConfig_Inbound_Rdp_3389.SourceAddressPrefix `
          -SourcePortRange $RuleConfig_Inbound_Rdp_3389.SourcePortRange `
          -DestinationAddressPrefix $RuleConfig_Inbound_Rdp_3389.DestinationAddressPrefix `
          -DestinationPortRange $RuleConfig_Inbound_Rdp_3389.DestinationPortRange
          
        # Commit the changes back to NSG
        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg 
        ```


#### <a name="sql-and-mysql"></a>SQL- en MySQL 
- Het kan maximaal één uur voordat gebruikers databases in een nieuwe SQL- of MySQL SKU maken kunnen duren.

- De database die als host fungeert voor servers moet worden toegewezen voor gebruik door de resource provider en werkbelastingen. U kunt een exemplaar dat wordt gebruikt door andere verbruiker, met inbegrip van App-Services niet gebruiken.

#### <a name="app-service"></a>App Service
- Gebruikers moeten de storage resourceprovider registreren voordat ze hun eerste Azure-functie in het abonnement maken.

- Om de infrastructuur (werknemers, beheer, front-rollen) uitbreiden, moet u PowerShell gebruiken, zoals beschreven in de release-opmerkingen voor Compute.
 
#### <a name="usage-and-billing"></a>Gebruik en facturering
- Openbare IP-adres meter gebruiksgegevens toont dezelfde *datum/tijd gebeurtenis* waarde voor elke record in plaats van de *TimeDate* tijdstempel die wordt aangegeven wanneer de record is gemaakt. U niet op dit moment kunnen deze gegevens gebruiken om uit te voeren nauwkeurige registratie van het gebruik van openbare IP-adres.

<!--
#### Identity
-->

#### <a name="downloading-azure-stack-tools-from-github"></a>Hulpprogramma's voor Azure Stack downloaden vanuit GitHub
- Wanneer u de *aanroepen webrequest* PowerShell-cmdlet voor het downloaden van de Azure-Stack van hulpprogramma's voor vanuit Github, er een foutbericht:     
    -  *aanroepen webrequest: de aanvraag is afgebroken: kan het beveiligde SSL/TLS-kanaal niet maken.*     

  Deze fout treedt op omdat een recente GitHub ondersteuning afschaffing van de cryptografische standaarden Tlsv1 en Tlsv1.1 (de standaardinstelling voor PowerShell). Zie voor meer informatie [zwakke cryptografische standaarden verwijdering kennisgeving](https://githubengineering.com/crypto-removal-notice/).

  U lost dit probleem, toevoegen `[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12` boven aan het script om af te dwingen de PowerShell-console TLSv1.2 gebruiken bij het downloaden van de GitHub-opslagplaatsen.


## <a name="build-201801032"></a>Build 20180103.2

### <a name="new-features-and-fixes"></a>Nieuwe functies en oplossingen

- Zie de [nieuwe functies en oplossingen](.\.\azure-stack-update-1712.md#new-features-and-fixes) sectie van de Azure-Stack 1712 update release-opmerkingen voor Azure-Stack geïntegreerd systemen.

    > [!IMPORTANT]
    > Sommige van de items in de **nieuwe functies en oplossingen** sectie alleen relevant zijn voor Azure-Stack geïntegreerd systemen.

### <a name="known-issues"></a>Bekende problemen
 
#### <a name="deployment"></a>Implementatie
- U moet een tijd-server op IP-adres opgeven tijdens de implementatie.

#### <a name="infrastructure-management"></a>Beheer van infrastructuur
- Schakel geen infrastructuur back-up op de **infrastructuur back-up** blade.
- De baseboard management controller (BMC) IP-adres en het model worden niet weergegeven in de essentiële informatie van een scale unit-knooppunt. Dit is verwacht gedrag in Azure Stack Development Kit.

#### <a name="portal"></a>Portal
- Mogelijk ziet u een lege dashboard in de portal. Als u wilt herstellen het dashboard, selecteert u het pictogram tandwielpictogram in de rechterbovenhoek van de portal en selecteer vervolgens **standaardinstellingen herstellen**.
- Wanneer u de eigenschappen van een resourcegroep weergeven de **verplaatsen** knop is uitgeschakeld. Dit is verwacht gedrag. Brongroepen verplaatsen tussen abonnementen wordt momenteel niet ondersteund.
-  Voor elke werkstroom waarin u een abonnement, resourcegroep of locatie in een vervolgkeuzelijst selecteren, kan u een of meer van de volgende problemen optreden:

   - Mogelijk ziet u een lege rij boven aan de lijst. U moet nog steeds mogelijk een item selecteren zoals verwacht.
   - Als de lijst met items in de vervolgkeuzelijst kort is, kan het niet mogelijk om een van de namen van items weer te geven.
   - Als u meerdere gebruikersabonnementen hebt, kan de lijst met resources groep vervolgkeuzelijst leeg zijn. 

   Om de problemen met de laatste twee omzeilen, kunt u de naam van het abonnement of resourcegroep (als u deze kent) typen of u kunt in plaats daarvan PowerShell gebruiken.

- U ziet een **activering vereist** waarschuwingsmelding dat aangeeft dat het registreren van uw Azure-Stack Development Kit noodzakelijk. Dit is verwacht gedrag.
- Als de **onderdeel** koppeling wordt geklikt vanaf elke **infrastructuurrol** waarschuwing, wordt de resulterende **overzicht** blade probeert te laden en mislukt. Bovendien de ** overzicht ** blade biedt geen time-out.
- Gebruiker abonnementen resulteert in een zwevende resources te verwijderen. Als tijdelijke oplossing Gebruikersbronnen of de hele resourcegroep eerst te verwijderen en verwijder vervolgens de gebruikersabonnementen.
- U bent geen kunnen machtigingen aan uw abonnement weergeven met behulp van de Azure-Stack-portals. Als tijdelijke oplossing kunt u machtigingen controleren met behulp van PowerShell.
 
#### <a name="marketplace"></a>Marketplace
- Sommige marketplace-items zijn verwijderd in deze release vanwege compatibiliteit problemen. Deze worden opnieuw ingeschakeld na verdere validatie.
- Gebruikers de volledige marketplace zonder een abonnement kunnen bladeren en beheeritems zoals plannen en aanbiedingen kunnen zien. Deze items zijn niet-functioneel voor gebruikers.
 
#### <a name="compute"></a>Compute
- Gebruikers krijgen de optie voor het maken van een virtuele machine met geografisch redundante opslag. Deze configuratie zorgt ervoor dat de virtuele machine wordt gemaakt mislukken. 
- U kunt een virtuele machine beschikbaarheidsset alleen met een domein met fouten van een en een updatedomein van een configureren.
- Er is geen marketplace-ervaring voor het maken van virtuele-machineschaalsets. U kunt een schaal ingesteld met behulp van een sjabloon maken.
- Instellingen voor virtuele-machineschaalsets schalen zijn niet beschikbaar in de portal. Als tijdelijke oplossing kunt u [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Vanwege verschillen tussen versies van PowerShell, moet u de `-Name` parameter in plaats van `-VMScaleSetName`.

#### <a name="networking"></a>Netwerken
- U kunt een load balancer met een openbaar IP-adres maken met behulp van de portal. Als tijdelijke oplossing kunt u PowerShell gebruiken voor het maken van de load balancer.
- Wanneer u een network load balancer maken, moet u een regel network address translation (NAT) maken. Als u dat niet doet, ontvangt u een fout wanneer u probeert een NAT-regel toevoegen nadat de load balancer is gemaakt.
- Onder **Networking**, als u op **verbinding** voor het instellen van een VPN-verbinding, **VNet-naar-VNet** wordt vermeld als een mogelijke verbindingstype. Selecteer deze optie niet. Op dit moment wordt alleen de **Site-naar-site (IPsec)** optie wordt ondersteund.
- U kunt een openbare IP-adres van een virtuele machine (VM) kan niet loskoppelen nadat de virtuele machine is gemaakt en gekoppeld aan dat IP-adres. Disassociation wordt weergegeven om te werken, maar de eerder toegewezen openbaar IP-adres blijft gekoppeld aan de oorspronkelijke virtuele machine. Dit gebeurt zelfs als u een nieuwe virtuele machine het IP-adres toewijzen (gewoonlijk aangeduid als een *VIP's wisselen*). Alle toekomstige verbinding probeert te maken via dit resultaat van de IP-adres in van een verbinding met de oorspronkelijk gekoppelde virtuele machine en niet naar het nieuwe bericht. U moet nieuwe openbare IP-adressen op dit moment alleen gebruiken voor het maken van nieuwe virtuele machine.
- Azure Stack-operators kunnen mogelijk geen implementeren, verwijderen, wijzigen Vns of Netwerkbeveiligingsgroepen. Dit probleem is hoofdzakelijk zichtbaar op latere update pogingen van hetzelfde pakket. Dit wordt veroorzaakt door een probleem pakketten met een update die op dit moment onderzocht wordt.
- MAC-adressen verwerkt interne Load Balancing (ILB) niet goed voor back-end virtuele machines waarop Linux, alle exemplaren wordt verbroken.
 
#### <a name="sqlmysql"></a>SQL/MySQL 
- Dit kan een uur duren voordat tenants databases in een nieuwe SQL- of MySQL SKU kunnen maken. 
- Maken van artikelen die rechtstreeks op de SQL- en MySQL die als host fungeert voor servers die niet worden uitgevoerd door de resourceprovider wordt niet ondersteund en kan leiden tot een niet-overeenkomende staat.

#### <a name="app-service"></a>App Service
- Een gebruiker moet de storage resourceprovider registreren voordat ze hun eerste Azure-functie in het abonnement maken.
 
#### <a name="usage-and-billing"></a>Gebruik en facturering
- Openbare IP-adres meter gebruiksgegevens toont dezelfde *datum/tijd gebeurtenis* waarde voor elke record in plaats van de *TimeDate* tijdstempel die wordt aangegeven wanneer de record is gemaakt. U niet op dit moment kunnen deze gegevens gebruiken om uit te voeren nauwkeurige registratie van het gebruik van openbare IP-adres.

#### <a name="identity"></a>Identiteit

In Azure Active Directory Federation Services (ADFS) geïmplementeerd omgevingen, de **azurestack\azurestackadmin** account is niet langer de eigenaar van de standaard Provider-abonnement. In plaats van de aanmelding bij de **beheerportal / adminmanagement eindpunt** met de **azurestack\azurestackadmin**, kunt u de **azurestack\cloudadmin** account, dus u kunt beheren en gebruiken de standaard Provider-abonnement.

> [!IMPORTANT]
> Zelfs de **azurestack\cloudadmin** account is de eigenaar van het abonnement van de Provider standaard in omgevingen met ADFS geïmplementeerd, heeft geen machtigingen voor RDP in de host. Blijven gebruiken de **azurestack\azurestackadmin** account of het lokale administrator-account aanmelden, toegang tot en beheren van de host, indien nodig.


## <a name="build-201711221"></a>Build 20171122.1

### <a name="new-features-and-fixes"></a>Nieuwe functies en oplossingen

- Zie de [nieuwe functies en oplossingen](.\.\azure-stack-update-1711.md#new-features-and-fixes) sectie van de Azure-Stack 1711 update release-opmerkingen voor Azure-Stack geïntegreerd systemen.

    > [!IMPORTANT]
    > Sommige van de items in de **nieuwe functies en oplossingen** sectie alleen relevant zijn voor Azure-Stack geïntegreerd systemen.

### <a name="known-issues"></a>Bekende problemen

 
#### <a name="deployment"></a>Implementatie
- U moet een tijd-server op IP-adres opgeven tijdens de implementatie.
- Vanaf versie 1711, **CloudAdmin** is een gereserveerde naam en mag niet worden handmatig opgegeven wanneer u de development kit implementeert. 

#### <a name="infrastructure-management"></a>Beheer van infrastructuur
- Schakel geen infrastructuur back-up op de **infrastructuur back-up** blade.
- De baseboard management controller (BMC) IP-adres en het model worden niet weergegeven in de essentiële informatie van een scale unit-knooppunt. Dit is verwacht gedrag in Azure Stack Development Kit.

#### <a name="portal"></a>Portal
- Mogelijk ziet u een lege dashboard in de portal. Als u wilt herstellen het dashboard, selecteert u het pictogram tandwielpictogram in de rechterbovenhoek van de portal en selecteer vervolgens **standaardinstellingen herstellen**.
- Wanneer u de eigenschappen van een resourcegroep weergeven de **verplaatsen** knop is uitgeschakeld. Dit is verwacht gedrag. Brongroepen verplaatsen tussen abonnementen wordt momenteel niet ondersteund.
-  Voor elke werkstroom waarin u een abonnement, resourcegroep of locatie in een vervolgkeuzelijst selecteren, kan u een of meer van de volgende problemen optreden:

   - Mogelijk ziet u een lege rij boven aan de lijst. U moet nog steeds mogelijk een item selecteren zoals verwacht.
   - Als de lijst met items in de vervolgkeuzelijst kort is, kan het niet mogelijk om een van de namen van items weer te geven.
   - Als u meerdere gebruikersabonnementen hebt, kan de lijst met resources groep vervolgkeuzelijst leeg zijn. 

   Om de problemen met de laatste twee omzeilen, kunt u de naam van het abonnement of resourcegroep (als u deze kent) typen of u kunt in plaats daarvan PowerShell gebruiken.

- U ziet een **activering vereist** waarschuwingsmelding dat aangeeft dat het registreren van uw Azure-Stack Development Kit noodzakelijk. Dit is verwacht gedrag.
- Als de **onderdeel** koppeling wordt geklikt vanaf elke **infrastructuurrol** waarschuwing, wordt de resulterende **overzicht** blade probeert te laden en mislukt. Bovendien de **overzicht** blade biedt geen time-out.
- Gebruiker abonnementen resulteert in een zwevende resources te verwijderen. Als tijdelijke oplossing Gebruikersbronnen of de hele resourcegroep eerst te verwijderen en verwijder vervolgens de gebruikersabonnementen.
- U bent geen kunnen machtigingen aan uw abonnement weergeven met behulp van de Azure-Stack-portals. Als tijdelijke oplossing kunt u machtigingen controleren met behulp van PowerShell.
 
#### <a name="marketplace"></a>Marketplace
- Wanneer u probeert items toevoegen aan de Stack van Azure marketplace met behulp van de **toevoegen uit Azure** optie, niet alle items mogelijk niet zichtbaar voor downloaden.
- Gebruikers de volledige marketplace zonder een abonnement kunnen bladeren en beheeritems zoals plannen en aanbiedingen kunnen zien. Deze items zijn niet-functioneel voor gebruikers.
 
#### <a name="compute"></a>Compute
- Gebruikers krijgen de optie voor het maken van een virtuele machine met geografisch redundante opslag. Deze configuratie zorgt ervoor dat de virtuele machine wordt gemaakt mislukken. 
- U kunt een virtuele machine beschikbaarheidsset alleen met een domein met fouten van een en een updatedomein van een configureren.
- Er is geen marketplace-ervaring voor het maken van virtuele-machineschaalsets. U kunt een schaal ingesteld met behulp van een sjabloon maken.
- Instellingen voor virtuele-machineschaalsets schalen zijn niet beschikbaar in de portal. Als tijdelijke oplossing kunt u [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Vanwege verschillen tussen versies van PowerShell, moet u de `-Name` parameter in plaats van `-VMScaleSetName`.

#### <a name="networking"></a>Netwerken
- U kunt een load balancer met een openbaar IP-adres maken met behulp van de portal. Als tijdelijke oplossing kunt u PowerShell gebruiken voor het maken van de load balancer.
- Wanneer u een network load balancer maken, moet u een regel network address translation (NAT) maken. Als u dat niet doet, ontvangt u een fout wanneer u probeert een NAT-regel toevoegen nadat de load balancer is gemaakt.
- Onder **Networking**, als u op **verbinding** voor het instellen van een VPN-verbinding, **VNet-naar-VNet** wordt vermeld als een mogelijke verbindingstype. Selecteer deze optie niet. Op dit moment wordt alleen de **Site-naar-site (IPsec)** optie wordt ondersteund.
- U kunt een openbare IP-adres van een virtuele machine (VM) kan niet loskoppelen nadat de virtuele machine is gemaakt en gekoppeld aan dat IP-adres. Disassociation wordt weergegeven om te werken, maar de eerder toegewezen openbaar IP-adres blijft gekoppeld aan de oorspronkelijke virtuele machine. Dit gebeurt zelfs als u een nieuwe virtuele machine het IP-adres toewijzen (gewoonlijk aangeduid als een *VIP's wisselen*). Alle toekomstige verbinding probeert te maken via dit resultaat van de IP-adres in van een verbinding met de oorspronkelijk gekoppelde virtuele machine en niet naar het nieuwe bericht. U moet nieuwe openbare IP-adressen op dit moment alleen gebruiken voor het maken van nieuwe virtuele machine.
- Azure Stack-operators kunnen mogelijk geen implementeren, verwijderen, wijzigen Vns of Netwerkbeveiligingsgroepen. Dit probleem is hoofdzakelijk zichtbaar op latere update pogingen van hetzelfde pakket. Dit wordt veroorzaakt door een probleem pakketten met een update die op dit moment onderzocht wordt.
- MAC-adressen verwerkt interne Load Balancing (ILB) niet goed voor back-end virtuele machines waarop Linux, alle exemplaren wordt verbroken.
 
#### <a name="sqlmysql"></a>SQL/MySQL 
- Dit kan een uur duren voordat tenants databases in een nieuwe SQL- of MySQL SKU kunnen maken. 
- Maken van artikelen die rechtstreeks op de SQL- en MySQL die als host fungeert voor servers die niet worden uitgevoerd door de resourceprovider wordt niet ondersteund en kan leiden tot een niet-overeenkomende staat.

    > [!NOTE]
    > Zie de afzonderlijke [SQL](https://docs.microsoft.com/azure/azure-stack/azure-stack-sql-resource-provider-deploy) en [MySQL](https://docs.microsoft.com/azure/azure-stack/azure-stack-mysql-resource-provider-deploy) artikelen voor compatibiliteit met de versie instellen.

#### <a name="app-service"></a>App Service
- Een gebruiker moet de storage resourceprovider registreren voordat ze hun eerste Azure-functie in het abonnement maken.
 
#### <a name="usage-and-billing"></a>Gebruik en facturering
- Openbare IP-adres meter gebruiksgegevens toont dezelfde *datum/tijd gebeurtenis* waarde voor elke record in plaats van de *TimeDate* tijdstempel die wordt aangegeven wanneer de record is gemaakt. U niet op dit moment kunnen deze gegevens gebruiken om uit te voeren nauwkeurige registratie van het gebruik van openbare IP-adres.

#### <a name="identity"></a>Identiteit

In Azure Active Directory Federation Services (ADFS) geïmplementeerd omgevingen, de **azurestack\azurestackadmin** account is niet langer de eigenaar van de standaard Provider-abonnement. In plaats van de aanmelding bij de **beheerportal / adminmanagement eindpunt** met de **azurestack\azurestackadmin**, kunt u de **azurestack\cloudadmin** account, dus u kunt beheren en gebruiken de standaard Provider-abonnement.

> [!IMPORTANT]
> Zelfs de **azurestack\cloudadmin** account is de eigenaar van het abonnement van de Provider standaard in omgevingen met ADFS geïmplementeerd, heeft geen machtigingen voor RDP in de host. Blijven gebruiken de **azurestack\azurestackadmin** account of het lokale administrator-account aanmelden, toegang tot en beheren van de host, indien nodig.

