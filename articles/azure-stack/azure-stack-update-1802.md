---
title: Azure Stack 1802 Update | Microsoft Docs
description: Meer informatie over wat er in de update 1802 voor Azure-Stack geïntegreerd systemen, de bekende problemen en waar u de update te downloaden.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2018
ms.author: brenduns
ms.reviewer: justini
ms.openlocfilehash: b3a3c07446ad04a58d5180793404fc04677749b2
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="azure-stack-1802-update"></a>Azure-Stack 1802 update

*Van toepassing op: Azure Stack geïntegreerd systemen*

In dit artikel worden de verbeteringen beschreven en oplossingen in het updatepakket met 1802, bekende problemen voor deze release en waar u de update te downloaden. Bekende problemen worden onderverdeeld in problemen met het rechtstreeks verband houden met het updateproces kan controleren en problemen met de build (na de installatie).

> [!IMPORTANT]        
> Dit updatepakket is alleen voor Azure-Stack geïntegreerd systemen. Pas dit pakket niet naar de Azure-Stack Development Kit.

## <a name="build-reference"></a>Verwijzing bouwen    
Het buildnummer van Azure Stack 1802 update **20180302.1**.  


## <a name="before-you-begin"></a>Voordat u begint    
> [!IMPORTANT]    
> Probeer niet voor het maken van virtuele machines tijdens de installatie van deze update. Zie voor meer informatie over het beheren van updates, [beheren van updates in de Azure-Stack overzicht](/azure-stack-updates#plan-for-updates).


### <a name="prerequisites"></a>Vereisten
- Installeer de Azure-Stack [1712 bijwerken](azure-stack-update-1712.md) voordat u de Azure-Stack 1802 update toepassen.    

- Installeer **AzS Hotfix – 1.0.180312.1-bouwen 20180222.2** voordat u de Azure-Stack 1802 update toepassen. Deze hotfix Windows Defender-updates en is beschikbaar wanneer u updates voor Azure-Stack downloaden.

  U kunt de hotfix installeren de normale procedures volgen voor [installeren van updates voor Azure-Stack](azure-stack-apply-updates.md). De naam van de update wordt weergegeven als **AzS Hotfix – 1.0.180312.1**, en bevat de volgende bestanden: 
    - PUPackageHotFix_20180222.2-1.exe
    - PUPackageHotFix_20180222.2-1.bin
    - Metadata.xml

  Na het uploaden van uitvoeren deze bestanden naar een opslagaccount en container, de installatie van de Update-tegel in het beheerportal. 
  
  In tegenstelling tot updates naar Azure Stack verandert u deze update installeert niet de versie van Azure-Stack.  Bevestig deze update is geïnstalleerd door de lijst weergeven met **geïnstalleerde updates**.
 


### <a name="post-update-steps"></a>Stappen na het bijwerken
*Er zijn geen stappen na-update voor de update 1802.*


### <a name="new-features-and-fixes"></a>Nieuwe functies en oplossingen
Deze update bevat de volgende verbeteringen en oplossingen voor Azure-Stack.

- **Er is ondersteuning toegevoegd voor de volgende versies van de API van Azure Storage-Service**:
    - 2017-04-17 
    - 2016-05-31 
    - 2015-12-11 
    - 2015-07-08 
    
    Zie voor meer informatie [Azure Stack Storage: verschillen en overwegingen](/azure/azure-stack/user/azure-stack-acs-differences).

- **Ondersteuning voor grotere [blok-Blobs](azure-stack-acs-differences.md)**:
    - De maximale toegestane blokgrootte is verhoogd van 4 MB op 100 MB.
    - De maximale blob wordt vergroot van 195 GB 4.75 TB.  

- **Back-up van de infrastructuur** nu wordt weergegeven in de tegel Resourceproviders en waarschuwingen voor back-up zijn ingeschakeld. Zie voor meer informatie over de infrastructuur Backup-Service, [back-up en gegevensherstel voor Azure-Stack met de infrastructuur Backup-Service](azure-stack-backup-infrastructure-backup.md).

- **Bijwerken naar de *Test AzureStack* cmdlet** voor het verbeteren van diagnostische gegevens voor opslag. Zie voor meer informatie over deze cmdlet [validatie voor Azure-Stack](azure-stack-diagnostic-test.md).

- **Op rollen gebaseerde toegangsbeheer (RBAC) verbeteringen** -u kunt nu RBAC gebruiken voor het overdragen van machtigingen voor universele groepen wanneer Azure Stack wordt geïmplementeerd met AD FS. Zie voor meer informatie over RBAC, [RBAC beheren](azure-stack-manage-permissions.md).

- **Er is ondersteuning toegevoegd voor meerdere domeinen met fouten**.  Zie voor meer informatie [hoge beschikbaarheid voor Azure-Stack](azure-stack-key-features.md#high-availability-for-azure-stack).

- **Verschillende oplossingen** voor prestaties, stabiliteit, beveiliging en het besturingssysteem dat wordt gebruikt door Azure stack.

<!--
#### New features
-->


<!--
#### Fixes
-->


### <a name="known-issues-with-the-update-process"></a>Bekende problemen met het updateproces kan controleren    
*Er zijn geen bekende problemen voor de installatie van update 1802.*


### <a name="known-issues-post-installation"></a>Bekende problemen (na de installatie)
Hieronder vindt u na de installatie bekende problemen voor de build **20180302.1**

#### <a name="portal"></a>Portal
- De mogelijkheid [een nieuwe ondersteuningsaanvraag openen vanuit de vervolgkeuzelijst](azure-stack-manage-portals.md#quick-access-to-help-and-support) uit binnen de beheerder van de portal is niet beschikbaar. Gebruik in plaats daarvan de volgende koppeling:     
    - Gebruik voor Azure-Stack geïntegreerde systemen https://aka.ms/newsupportrequest.

- <!-- 2050709 --> In the admin portal, it is not possible to edit storage metrics for Blob service, Table service, or Queue service. When you go to Storage, and then select the blob, table, or queue service tile, a new blade opens that displays a metrics chart for that service. If you then select Edit from the top of the metrics chart tile, the Edit Chart blade opens but does not display options to edit metrics.

- Het is niet mogelijk om compute of opslag resources in de beheerdersportal weer te geven. De oorzaak van dit probleem is een fout opgetreden tijdens de installatie van de update zorgt ervoor dat de update niet correct worden gerapporteerd als geslaagd. Als dit probleem optreedt, moet u contact op met Microsoft Customer Support Services voor ondersteuning.

- U ziet een lege dashboard in de portal. Als u wilt herstellen het dashboard, selecteert u het pictogram tandwielpictogram in de rechterbovenhoek van de portal en selecteer vervolgens **standaardinstellingen herstellen**.

- Wanneer u de eigenschappen van een resource of resourcegroep bekijkt de **verplaatsen** knop is uitgeschakeld. Dit is verwacht gedrag. Verplaatsen van resources of resourcegroepen tussen resourcegroepen of abonnementen is momenteel niet ondersteund.

- Gebruiker abonnementen resulteert in een zwevende resources te verwijderen. Als tijdelijke oplossing Gebruikersbronnen of de hele resourcegroep eerst te verwijderen en verwijder vervolgens de gebruikersabonnementen.

- U kunt machtigingen aan uw abonnement met behulp van de portals Azure Stack niet weergeven. Als tijdelijke oplossing PowerShell te gebruiken om machtigingen te controleren.

- In het dashboard van de beheerportal mislukt de Update-tegel om meer informatie over updates weer te geven. U lost dit probleem, klikt u op de tegel te vernieuwen.

-   In het beheerportal ziet u mogelijk een kritieke waarschuwing voor het onderdeel Microsoft.Update.Admin. De naam van waarschuwing, beschrijving en herstel alle als weergegeven:  
    - *Fout - sjabloon voor FaultType ResourceProviderTimeout ontbreekt.*

    Deze waarschuwing kan worden genegeerd. 

- <!-- 2253274 --> In the admin and user portals, the Settings blade for vNet Subnets fails to load. As a workaround, use PowerShell and the [Get-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig?view=azurermps-5.5.0) cmdlet to view and  manage this information.

- In de beheerportal en gebruikersportal, de overzichtsblade niet kan worden geladen bij het selecteren van de blade overzicht van de storage-accounts die zijn gemaakt met een oudere versie van de API (voorbeeld: 2015-06-15). Dit omvat system storage-accounts zoals **updateadminaccount** die wordt gebruikt tijdens de patch en bij te werken. 

  Als tijdelijke oplossing kunt u PowerShell gebruiken om uit te voeren de **Start ResourceSynchronization.ps1** script toegang om de accountdetails van de opslag te herstellen. [Het script is beschikbaar via GitHub]( https://github.com/Azure/AzureStack-Tools/tree/master/Support/scripts), en moet worden uitgevoerd met servicebeheerdersreferenties op de bevoorrechte eindpunt. 

- De **servicestatus** blade niet kan worden geladen. Wanneer u de status van de Service-blade opent in de beheerder of de gebruiker-portal, Azure-Stack een fout wordt weergegeven en wordt de informatie niet geladen. Dit is verwacht gedrag. Hoewel u kunt selecteren en servicestatus opent, wordt deze functie is nog niet beschikbaar maar zullen worden uitgevoerd in een toekomstige versie van Azure-Stack.


#### <a name="health-and-monitoring"></a>Status en bewaking
Er zijn geen bekende problemen na het bijwerken van 1802.

#### <a name="marketplace"></a>Marketplace
- Gebruikers de volledige marketplace zonder een abonnement kunnen bladeren en beheeritems zoals plannen en aanbiedingen kunnen zien. Deze items zijn niet-functioneel voor gebruikers.

#### <a name="compute"></a>Compute
- Instellingen voor virtuele-machineschaalsets schalen zijn niet beschikbaar in de portal. Als tijdelijke oplossing kunt u [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Vanwege verschillen tussen versies van PowerShell, moet u de `-Name` parameter in plaats van `-VMScaleSetName`.

- Azure-Stack ondersteunt het gebruik van alleen vaste type VHD's. Enkele afbeeldingen die worden aangeboden via de marketplace op Azure-Stack dynamische VHD's gebruiken maar die zijn verwijderd. Vergroten of verkleinen van een virtuele machine (VM) met een dynamische schijf is gekoppeld, blijft de virtuele machine een mislukte status.

  Om dit probleem beperken, verwijdert u de virtuele machine zonder te verwijderen van de VM-schijf, een VHD-blob in een opslagaccount. Vervolgens de VHD van een dynamische schijf converteren naar een vaste schijf en de virtuele machine opnieuw maken.

- Bij het maken van een beschikbaarheidsset in de portal door te gaan naar **nieuw** > **Compute** > **beschikbaarheidsset**, kunt u alleen maken een beschikbaarheidsset met een domein met fouten en het updatedomein van 1. Als tijdelijke oplossing bij het maken van een nieuwe virtuele machine, maakt u de beschikbaarheidsset met behulp van PowerShell, CLI of vanuit de portal.

- Wanneer u virtuele machines op de gebruikersportal van Azure-Stack maakt, wordt een onjuist aantal gegevensschijven dat aan een DS-serie VM koppelen kunt in de portal weergegeven. DS-serie VMs aankan zo veel gegevensschijven als de configuratie van de Azure.

- Wanneer een installatiekopie van een virtuele machine niet kan worden gemaakt, kan een mislukte item dat u niet verwijderen, worden toegevoegd aan de VM-installatiekopieën compute-blade.

  Als tijdelijke oplossing door een nieuwe VM-installatiekopie te maken met een dummy VHD die kan worden gemaakt via de Hyper-V (New-VHD-pad C:\dummy.vhd-vaste - SizeBytes 1 GB). Dit proces los het probleem dat verhindert dat het mislukte item wordt verwijderd. Vervolgens kunt 15 minuten na het maken van de installatiekopie van het dummy, u met succes verwijderen.

  U kunt vervolgens redownload van de VM-installatiekopie die eerder is mislukt.

-  Als u een uitbreiding op de implementatie van een VM-inrichting te lang duurt, door gebruikers laten de inrichting time-outwaarde in plaats van bij het stoppen van het proces voor de toewijzing ongedaan maken of verwijderen van de virtuele machine.  

- <!-- 1662991 --> Linux VM diagnostics is not supported in Azure Stack. When you deploy a Linux VM with VM diagnostics enabled, the deployment fails. The deployment also fails if you enable the Linux VM basic metrics through diagnostic settings.  




#### <a name="networking"></a>Netwerken
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
 - Controleer voordat u doorgaat, de belangrijke opmerking in [voordat u begint met](#before-you-begin) in de buurt van het begin van deze release-opmerkingen.
- Het kan maximaal één uur voordat gebruikers databases in een nieuwe implementatie van de SQL- of MySQL maken kunnen duren.

- Alleen de resourceprovider wordt voor het maken van items op servers die SQL-host of MySQL ondersteund. Items die worden gemaakt op een hostserver die niet zijn gemaakt door de resourceprovider kunnen leiden tot een niet-overeenkomende staat.  


> [!NOTE]  
> Nadat u naar Azure Stack 1802 bijwerkt, kunt u blijven gebruiken van de SQL- en MySQL resourceproviders die u eerder hebt geïmplementeerd.  U wordt aangeraden dat u SQL en MySQL bijwerken wanneer er een nieuwe versie beschikbaar. Als Azure-Stack updates toepassen op SQL- en MySQL resourceproviders sequentieel worden verwerkt.  Als u versie 1710, versie 1711 en vervolgens 1712 eerst toe te passen en vervolgens bijwerken naar 1802.      
>   
> De installatie van update 1802 heeft geen invloed op het huidige gebruik van SQL- of MySQL resourceproviders door uw gebruikers.
> Ongeacht de versie van de resourceproviders die u gebruikt de gegevens van uw gebruikers in hun databases is niet aangeraakt, en blijft toegankelijk.    


#### <a name="app-service"></a>App Service
- Gebruikers moeten de storage resourceprovider registreren voordat ze hun eerste Azure-functie in het abonnement maken.

- Om de infrastructuur (werknemers, beheer, front-rollen) uitbreiden, moet u PowerShell gebruiken, zoals beschreven in de release-opmerkingen voor Compute.

<!--
#### Identity
-->

#### <a name="downloading-azure-stack-tools-from-github"></a>Hulpprogramma's voor Azure Stack downloaden vanuit GitHub
- Wanneer u de *aanroepen webrequest* PowerShell-cmdlet voor het downloaden van de Azure-Stack van hulpprogramma's voor vanuit Github, er een foutbericht:     
    -  *aanroepen webrequest: de aanvraag is afgebroken: kan het beveiligde SSL/TLS-kanaal niet maken.*     

  Deze fout treedt op omdat een recente GitHub ondersteuning afschaffing van de cryptografische standaarden Tlsv1 en Tlsv1.1 (de standaardinstelling voor PowerShell). Zie voor meer informatie [zwakke cryptografische standaarden verwijdering kennisgeving](https://githubengineering.com/crypto-removal-notice/).

  U lost dit probleem, toevoegen `[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12` boven aan het script om af te dwingen de PowerShell-console TLSv1.2 gebruiken bij het downloaden van de GitHub-opslagplaatsen.


## <a name="download-the-update"></a>De update downloaden
U kunt het pakket met de Azure-Stack 1802 update downloaden [hier](https://aka.ms/azurestackupdatedownload).


## <a name="more-information"></a>Meer informatie
Microsoft is een manier om te controleren en updates met behulp van de bevoegde eindpunt (PEP) is geïnstalleerd met Update 1710 hervatten opgegeven.

- Zie de [in Azure-Stack met behulp van de bevoegde endpoint-documentatie-updates controleren](https://docs.microsoft.com/azure/azure-stack/azure-stack-monitor-update).

## <a name="see-also"></a>Zie ook

- Zie voor een overzicht van de update management in Azure-Stack [beheren van updates in de Azure-Stack overzicht](azure-stack-updates.md).
- Zie voor meer informatie over het toepassen van updates met de Azure-Stack [toepassen van updates in Azure-Stack](azure-stack-apply-updates.md).
