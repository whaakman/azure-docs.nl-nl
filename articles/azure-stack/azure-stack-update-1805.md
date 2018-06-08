---
title: Azure Stack 1805 Update | Microsoft Docs
description: Meer informatie over wat is er nieuw in de update 1805 voor Azure-Stack geïntegreerd systemen, met inbegrip van de bekende problemen en waar u de update te downloaden.
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
ms.date: 06/07/2018
ms.author: brenduns
ms.reviewer: justini
ms.openlocfilehash: cb6c4d5cd1d63403c102f7d09741eba4932a79bb
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34850505"
---
# <a name="azure-stack-1805-update"></a>Azure-Stack 1805 update

*Van toepassing op: Azure Stack geïntegreerd systemen*

In dit artikel worden de verbeteringen beschreven en oplossingen in het updatepakket met 1805, bekende problemen voor deze versie en waar u de update te downloaden. Bekende problemen worden onderverdeeld in problemen met het rechtstreeks verband houden met het updateproces kan controleren en problemen met de build (na de installatie).

> [!IMPORTANT]        
> Dit updatepakket is alleen voor Azure-Stack geïntegreerd systemen. Pas dit pakket niet naar de Azure-Stack Development Kit.

## <a name="build-reference"></a>Verwijzing bouwen    
Het buildnummer van Azure Stack 1805 update **1.1805.1.47**.  

> [!TIP]  
> Op basis van feedback van klanten, wordt er een update van het schema van de versie voor Microsoft Azure-Stack in gebruik is.  Beginnen met deze update 1805, het nieuwe schema beter vertegenwoordigt voor de huidige cloudversie.  
> 
> Het schema versie is nu *Version.YearYearMonthMonth.MinorVersion.BuildNumber* waar de tweede en derde sets geven de versie en release. Bijvoorbeeld: 1805.1 vertegenwoordigt de *release RTM* (RTM) versie van 1805.  


### <a name="new-features"></a>Nieuwe functies
Deze update bevat de volgende verbeteringen voor Azure-Stack.
<!-- 2297790 - IS, ASDK --> 
- **Azure Stack bevat nu een *Syslog* client** als een *preview-functie*. Deze client kunt het doorsturen van controle en beveiliging logboeken die betrekking hebben op de Stack van Azure-infrastructuur naar een Syslog-server of beveiligingsgroep informatie en gebeurtenis management (SIEM) software die zich buiten de Azure-Stack. De Syslog-client ondersteunt momenteel alleen niet-geverifieerde UDP-verbindingen via de standaardpoort 514. De nettolading van elk Syslog-bericht is ingedeeld in algemene gebeurtenis-indeling (CEF). 

  Voor de configuratie van de Syslog-client gebruikt de **Set SyslogServer** cmdlet worden weergegeven in het bevoegde eindpunt. 

  Met dit voorbeeld ziet u mogelijk de volgende drie waarschuwingen. Wanneer door Azure Stack wordt ingediend, neemt u deze waarschuwingen *beschrijvingen* en *herstel* richtlijnen. 
  - TITEL: Code-integriteit uitschakelen  
  - TITEL: Code-integriteit in de controlemodus 
  - TITEL: Gebruikersaccount die is gemaakt

  Hoewel deze functie in de preview, moet deze niet worden aangehaald in productieomgevingen.   




### <a name="fixed-issues"></a>Opgeloste problemen

<!-- # - applicability -->


- **Verschillende oplossingen** voor prestaties, stabiliteit, beveiliging en het besturingssysteem dat wordt gebruikt door de Azure-Stack.


<!-- # Additional releases timed with this update    -->



## <a name="before-you-begin"></a>Voordat u begint    

### <a name="prerequisites"></a>Vereisten
- Installeer de Azure-Stack [1804 bijwerken](azure-stack-update-1804.md) voordat u de Azure-Stack 1805 update toepassen.    
- Voordat u begint de installatie van update 1805, voert u [Test AzureStack](azure-stack-diagnostic-test.md) valideren van de status van uw Azure-Stack en los eventuele operationele problemen gevonden. Ook actieve waarschuwingen bekijken en los eventuele waarvoor actie is vereist. 

### <a name="known-issues-with-the-update-process"></a>Bekende problemen met het updateproces kan controleren   
- Tijdens de installatie van de update 1805 ziet u mogelijk waarschuwingen met de titel *fout: sjabloon voor FaultType UserAccounts.New ontbreekt.*  U kunt deze waarschuwingen negeren. Deze waarschuwingen wordt automatisch gesloten nadat de update naar 1805 is voltooid.   

- <!-- 2489559 - IS --> Do not attempt to create virtual machines during the installation of this update. For more information about managing updates, see [Manage updates in Azure Stack overview](azure-stack-updates.md#plan-for-updates).


### <a name="post-update-steps"></a>Stappen na het bijwerken
*Er zijn geen stappen na-update voor de update 1805.*


## <a name="known-issues-post-installation"></a>Bekende problemen (na de installatie)
Hier volgen bekende problemen voor deze buildversie na de installatie.

### <a name="portal"></a>Portal  
- <!-- 2551834 - IS, ASDK --> When you select **Overview** for a storage account in either the admin or user portals, the information from the *Essentials* pane does not display.  The Essentials pane displays information about the account like its *Resource group*, *Location*, and *Subscription ID*.  Other options for Overview  are accessible, like *Services* and *Monitoring*, as well as options to *Open in Explorer* or to *Delete storage account*. 

  Als u de informatie niet beschikbaar is, gebruikt u de [Get azureRMstorageaccount](https://docs.microsoft.com/powershell/module/azurerm.storage/get-azurermstorageaccount?view=azurermps-6.2.0) PowerShell-cmdlet. 

- <!-- 2332636 - IS -->  When you use AD FS for your Azure Stack identity system and update to this version of Azure Stack, the default owner of the default provider subscription is reset to the built-in **CloudAdmin** user.  
  Tijdelijke oplossing: Los dit probleem nadat u deze update installeert, gebruikt u stap 3 van de [Trigger automation voor het configureren van een claimprovider in Azure-Stack](azure-stack-integrate-identity.md#trigger-automation-to-configure-claims-provider-trust-in-azure-stack-1) procedure opnieuw instellen van de eigenaar van het standaard provider-abonnement.   

- <!-- TBD - IS ASDK --> Some administrative subscription types are not available.  When you upgrade Azure Stack to this version, the two subscription types that were [introduced with version 1804](azure-stack-update-1804.md#new-features) are not visible in the console. This is expected. The unavailable subscription types are *Metering subscription*, and *Consumption subscription*. These subscription types are visible in new Azure Stack environments beginning with version 1804 but are not yet ready for use. You should continue to use the *Default Provider* subscription type.  

- <!-- 2403291 - IS ASDK --> You might not have use of the horizontal scroll bar along the bottom of the admin and user portals. If you can’t access the horizontal scroll bar, use the breadcrumbs to navigate to a previous blade in the portal by selecting the name of the blade you want to view from the breadcrumb list found at the top left of the portal.
  ![Breadcrumb](media/azure-stack-update-1804/breadcrumb.png)

- <!-- TBD - IS --> It might not be possible to view compute or storage resources in the administrator portal. The cause of this issue is an error during the installation of the update that causes the update to be incorrectly reported as successful. If this issue occurs, contact Microsoft Customer Support Services for assistance.

- <!-- TBD - IS --> You might see a blank dashboard in the portal. To recover the dashboard, select the gear icon in the upper right corner of the portal, and then select **Restore default settings**.

- <!-- TBD - IS ASDK --> Deleting user subscriptions results in orphaned resources. As a workaround, first delete user resources or the entire resource group, and then delete user subscriptions.

- <!-- TBD - IS ASDK --> You cannot view permissions to your subscription using the Azure Stack portals. As a workaround, use PowerShell to verify permissions.


### <a name="health-and-monitoring"></a>Status en bewaking
- <!-- 1264761 - IS ASDK -->  You might see alerts for the *Health controller* component that have the following details:  

   Waarschuwing #1:
   - NAAM: De functie van de infrastructuur is slecht
   - ERNST: waarschuwing
   - ONDERDEEL: Health-domeincontroller
   - Beschrijving: De health-domeincontroller Heartbeat-Scanner is niet beschikbaar. Dit kan beïnvloeden statusrapporten en metrische gegevens.  

  Waarschuwing #2:
   - NAAM: De functie van de infrastructuur is slecht
   - ERNST: waarschuwing
   - ONDERDEEL: Health-domeincontroller
   - Beschrijving: De health-domeincontroller veroorzaakt Scanner is niet beschikbaar. Dit kan beïnvloeden statusrapporten en metrische gegevens.

  Beide waarschuwingen kunnen worden genegeerd en ze automatisch gesloten na verloop van tijd.  

- <!-- 2368581 - IS. ASDK --> An Azure Stack operator, if you receive a low memory alert and tenant virtual machines fail to deploy with a *Fabric VM creation error*, it is possible that the Azure Stack stamp is out of available memory. Use the [Azure Stack Capacity Planner](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) to best understand the capacity available for your workloads. 


### <a name="compute"></a>Compute
- <!-- TBD - IS, ASDK --> When selecting a virtual machine size for a virtual machine deployment, some F-Series VM sizes are not visible as part of the size selector when you create a VM. The following VM sizes do not appear in the selector: *F8s_v2*, *F16s_v2*, *F32s_v2*, and *F64s_v2*.  
  Als tijdelijke oplossing een van de volgende methoden te gebruiken voor het implementeren van een virtuele machine. In elke methode moet u opgeven van de VM-grootte die u wilt gebruiken.

  - **Azure Resource Manager-sjabloon:** als u een sjabloon gebruikt, stelt de *vmSize* in de sjabloon zodat deze overeenkomt met de VM-grootte die u wilt gebruiken. Bijvoorbeeld de volgende vermelding wordt gebruikt voor het implementeren van een virtuele machine die gebruikmaakt van de *F32s_v2* grootte:  

    ```
        "properties": {
        "hardwareProfile": {
                "vmSize": "Standard_F32s_v2"
        },
    ```  
  - **Azure CLI:** kunt u de [az vm maken](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create) opdracht en de VM-grootte opgeven als een parameter, vergelijkbaar met `--size "Standard_F32s_v2"`.

  - **PowerShell:** met PowerShell kunt u [nieuw AzureRMVMConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvmconfig?view=azurermps-6.0.0) met de parameter waarmee de VM-grootte, vergelijkbaar met `-VMSize "Standard_F32s_v2"`.


- <!-- TBD - IS ASDK --> Scaling settings for virtual machine scale sets are not available in the portal. As a workaround, you can use [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Because of PowerShell version differences, you must use the `-Name` parameter instead of `-VMScaleSetName`.

- <!-- TBD - IS --> When you create an availability set in the portal by going to **New** > **Compute** > **Availability set**, you can only create an availability set with a fault domain and update domain of 1. As a workaround, when creating a new virtual machine, create the availability set by using PowerShell, CLI, or from within the portal.

- <!-- TBD - IS ASDK --> When you create virtual machines on the Azure Stack user portal, the portal displays an incorrect number of data disks that can attach to a DS series VM. DS series VMs can accommodate as many data disks as the Azure configuration.

- <!-- TBD - IS ASDK --> When a VM image fails to be created, a failed item that you cannot delete might be added to the VM images compute blade.

  Als tijdelijke oplossing door een nieuwe VM-installatiekopie te maken met een dummy VHD die kan worden gemaakt via de Hyper-V (New-VHD-pad C:\dummy.vhd-vaste - SizeBytes 1 GB). Dit proces los het probleem dat verhindert dat het mislukte item wordt verwijderd. Vervolgens kunt 15 minuten na het maken van de installatiekopie van het dummy, u met succes verwijderen.

  U kunt vervolgens redownload van de VM-installatiekopie die eerder is mislukt.

- <!-- TBD - IS ASDK --> If provisioning an extension on a VM deployment takes too long, users should let the provisioning time-out instead of trying to stop the process to deallocate or delete the VM.  

- <!-- 1662991 IS ASDK --> Linux VM diagnostics is not supported in Azure Stack. When you deploy a Linux VM with VM diagnostics enabled, the deployment fails. The deployment also fails if you enable the Linux VM basic metrics through diagnostic settings.  


### <a name="networking"></a>Netwerken
- <!-- 1766332 - IS ASDK --> Under **Networking**, if you click **Create VPN Gateway** to set up a VPN connection, **Policy Based** is listed as a VPN type. Do not select this option. Only the **Route Based** option is supported in Azure Stack.

- <!-- 2388980 - IS ASDK --> After a VM is created and associated with a public IP address, you can't disassociate that VM from that IP address. Disassociation appears to work, but the previously assigned public IP address remains associated with the original VM.

  Op dit moment moet u alleen nieuwe openbare IP-adressen voor de nieuwe virtuele machines die u maakt.

  Dit gebeurt zelfs als u een nieuwe virtuele machine het IP-adres toewijzen (gewoonlijk aangeduid als een *VIP's wisselen*). Alle toekomstige verbinding probeert te maken via dit resultaat van de IP-adres in van een verbinding met de oorspronkelijke virtuele machine en niet naar het nieuwe bericht.

- <!-- 2292271 - IS ASDK --> If you raise a Quota limit for a Network resource that is part of an Offer and Plan that is associated with a tenant subscription, the new limit is not applied to that subscription. However, the new limit does apply to new subscriptions that are created after the quota is increased.

  Gebruik een invoegtoepassing plan quotum te verhogen een netwerk wanneer het abonnement al gekoppeld aan een abonnement is om dit probleem omzeilen. Zie voor meer informatie hoe [beschikbaar maken van een plan voor de invoegtoepassing](azure-stack-subscribe-plan-provision-vm.md#to-make-an-add-on-plan-available).

- <!-- 2304134 IS ASDK --> You cannot delete a subscription that has DNS Zone resources or Route Table resources associated with it. To successfully delete the subscription, you must first delete DNS Zone and Route Table resources from the tenant subscription.


- <!-- 1902460 - IS ASDK --> Azure Stack supports a single *local network gateway* per IP address. This is true across all tenant subscriptions. After the creation of the first local network gateway connection, subsequent attempts to create a local network gateway resource with the same IP address are blocked.

- <!-- 16309153 - IS ASDK --> On a Virtual Network that was created with a DNS Server setting of *Automatic*, changing to a custom DNS Server fails. The updated settings are not pushed to VMs in that Vnet.

- <!-- TBD - IS ASDK --> Azure Stack does not support adding additional network interfaces to a VM instance after the VM is deployed. If the VM requires more than one network interface, they must be defined at deployment time.

- <!-- 2096388 IS --> You cannot use the admin portal to update rules for a network security group.

    Tijdelijke oplossing voor App Service: als u extern bureaublad op de exemplaren van de domeincontroller moet, u wijzigt de beveiligingsregels voor verbindingen binnen de netwerkbeveiligingsgroepen met PowerShell.  Hieronder vindt u voorbeelden van hoe *toestaan*, en zet u de configuratie van *weigeren*:  

    - *Toestaan:*

      ```powershell    
      Connect-AzureRmAccount -EnvironmentName AzureStackAdmin

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

        Connect-AzureRmAccount -EnvironmentName AzureStackAdmin

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


### <a name="sql-and-mysql"></a>SQL- en MySQL

- <!-- TBD - IS --> Only the resource provider is supported to create items on servers that host SQL or MySQL. Items created on a host server that are not created by the resource provider might result in a mismatched state.  

- <!-- IS, ASDK --> Special characters, including spaces and periods, are not supported in the **Family** or **Tier** names when you create a SKU for the SQL and MySQL resource providers.


> [!NOTE]  
> <!-- TBD - IS --> After you update to Azure Stack 1805, you can continue to use the SQL and MySQL resource providers that you previously deployed.  We recommend you update SQL and MySQL when a new release becomes available. Like Azure Stack, apply updates to SQL and MySQL resource providers sequentially. For example, if you use version 1803, first apply version 1804, and then update to 1805.      
>   
> De installatie van update 1805 heeft geen invloed op het huidige gebruik van SQL- of MySQL resourceproviders door uw gebruikers.
> Ongeacht de versie van de resourceproviders die u gebruikt de gegevens van uw gebruikers in hun databases is niet aangeraakt, en blijft toegankelijk.    



### <a name="app-service"></a>App Service
- <!-- 2352906 - IS ASDK --> Users must register the storage resource provider before they create their first Azure Function in the subscription.

- <!-- 2489178 - IS ASDK --> In order to scale out infrastructure (workers, management, front-end roles), you must use PowerShell as described in the release notes for Compute.

- <!-- TBD - IS ASDK --> App Service can only be deployed into the *Default Provider subscription* at this time. In a future update, App Service will deploy into the new *Metering subscription* that was introduced in Azure Stack 1804. When Metering is supported for use, all existing deployments will be migrated to this new subscription type.


### <a name="usage"></a>Gebruik  
- <!-- TBD - IS ASDK --> Usage Public IP address usage meter data shows the same *EventDateTime* value for each record instead of the *TimeDate* stamp that shows when the record was created. Currently, you can’t use this data to perform accurate accounting of public IP address usage.


<!-- #### Identity -->
<!-- #### Marketplace -->


## <a name="download-the-update"></a>De update downloaden
U kunt het pakket met de Azure-Stack 1805 update downloaden [hier](https://aka.ms/azurestackupdatedownload).


## <a name="see-also"></a>Zie ook
- Voor het gebruik van de bevoegde eindpunt (PEP) om te controleren en hervatten van updates, Zie [in Azure-Stack met behulp van de bevoegde endpoint-updates controleren](azure-stack-monitor-update.md).
- Zie voor een overzicht van de update management in Azure-Stack [beheren van updates in de Azure-Stack overzicht](azure-stack-updates.md).
- Zie voor meer informatie over het toepassen van updates met de Azure-Stack [toepassen van updates in Azure-Stack](azure-stack-apply-updates.md).
