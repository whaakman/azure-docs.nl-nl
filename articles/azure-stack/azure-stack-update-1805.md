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
ms.date: 06/20/2018
ms.author: brenduns
ms.reviewer: justini
ms.openlocfilehash: 80ed0d2353fc6ea3a515c0d05475c713920abe46
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36295743"
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
- We het probleem opgelost dat geblokkeerd [openen van een nieuw ondersteuningsverzoek uit de vervolgkeuzelijst](azure-stack-manage-portals.md#quick-access-to-help-and-support) uit in de beheerportal. Deze optie nu werkt zoals bedoeld. 

- **Verschillende oplossingen** voor prestaties, stabiliteit, beveiliging en het besturingssysteem dat wordt gebruikt door de Azure-Stack.


<!-- # Additional releases timed with this update    -->



## <a name="before-you-begin"></a>Voordat u begint    

### <a name="prerequisites"></a>Vereisten
- Installeer de Azure-Stack [1804 bijwerken](azure-stack-update-1804.md) voordat u de Azure-Stack 1805 update toepassen.    
- Voordat u begint de installatie van update 1805, voert u [Test AzureStack](azure-stack-diagnostic-test.md) valideren van de status van uw Azure-Stack en los eventuele operationele problemen gevonden. Ook actieve waarschuwingen bekijken en los eventuele waarvoor actie is vereist. 

### <a name="known-issues-with-the-update-process"></a>Bekende problemen met het updateproces kan controleren   
- Tijdens de installatie van de update 1805 ziet u mogelijk waarschuwingen met de titel *fout: sjabloon voor FaultType UserAccounts.New ontbreekt.*  U kunt deze waarschuwingen negeren. Deze waarschuwingen wordt automatisch gesloten nadat de update naar 1805 is voltooid.   

- <!-- 2489559 - IS --> Probeer niet voor het maken van virtuele machines tijdens de installatie van deze update. Voor meer informatie over het beheren van updates, seSe [beheren van updates in de Azure-Stack overzicht](azure-stack-updates.md#plan-for-updates).


### <a name="post-update-steps"></a>Stappen na het bijwerken
Na de installatie van 1805 toepasselijke Hotfixes te installeren. Raadpleeg voor meer informatie de volgende knowledge base-artikelen, evenals onze [beleid onderhoud](azure-stack-servicing-policy.md).  
 - [KB 4340474 - Stack van Azure Hotfix 1.1805.4.53](https://support.microsoft.com/en-us/help/4340474).


## <a name="known-issues-post-installation"></a>Bekende problemen (na de installatie)
Hier volgen bekende problemen voor deze buildversie na de installatie.

### <a name="portal"></a>Portal  
- <!-- 2551834 - IS, ASDK --> Wanneer u selecteert **overzicht** voor een opslagaccount in de beheerder of de gebruiker-portals, de gegevens van de *Essentials* deelvenster wordt niet weergegeven.  Het deelvenster Essentials geeft informatie weer over het account zoals de *resourcegroep*, *locatie*, en *abonnements-ID*.  Andere opties voor overzicht zijn toegankelijk zijn, zoals *Services* en *bewaking*, ook als opties voor *openen in Verkenner* of *opslagaccount verwijderen* . 

  Als u de informatie niet beschikbaar is, gebruikt u de [Get azureRMstorageaccount](https://docs.microsoft.com/powershell/module/azurerm.storage/get-azurermstorageaccount?view=azurermps-6.2.0) PowerShell-cmdlet. 

- <!-- 2551834 - IS, ASDK --> Wanneer u selecteert **labels** voor een opslagaccount in portals van de beheerder of de gebruiker de gegevens niet kan worden geladen en niet wordt weergegeven.  

  Als u de informatie niet beschikbaar is, gebruikt u de [Get-AzureRmTag](https://docs.microsoft.com/powershell/module/azurerm.tags/get-azurermtag?view=azurermps-6.2.0) PowerShell-cmdlet.


- <!-- 2332636 - IS -->  Wanneer u AD FS voor uw Azure-Stack identiteitssysteem en de update in deze versie van Azure-Stack, de standaardeigenaar van het abonnement van de provider standaard ingesteld op de ingebouwde **CloudAdmin** gebruiker.  
  Tijdelijke oplossing: Los dit probleem nadat u deze update installeert, gebruikt u stap 3 van de [Trigger automation voor het configureren van een claimprovider in Azure-Stack](azure-stack-integrate-identity.md#trigger-automation-to-configure-claims-provider-trust-in-azure-stack-1) procedure opnieuw instellen van de eigenaar van het standaard provider-abonnement.   

- <!-- TBD - IS ASDK --> Sommige typen beheerdersrechten abonnementen zijn niet beschikbaar.  Wanneer u Azure-Stack naar deze versie bijwerkt, worden de typen van de twee abonnementen zijn [geïntroduceerd in versie 1804](azure-stack-update-1804.md#new-features) zijn niet zichtbaar in de console. Dit is normaal gedrag. Het abonnement niet beschikbaar-typen zijn *softwarelicentiecontrole abonnement*, en *verbruik abonnement*. Deze typen abonnement zichtbaar zijn in de nieuwe Azure-Stack omgevingen vanaf versie 1804 maar nog niet klaar voor gebruik. U moet blijven gebruiken de *Provider standaard* abonnementstype.  

- <!-- 2403291 - IS ASDK --> Hebt u mogelijk geen gebruik van de horizontale schuifbalk langs de onderkant van de portals beheerder en gebruiker. Als u geen toegang de horizontale schuifbalk tot, links gebruik de breadcrumbs om te navigeren naar een vorige blade in de portal voor u de naam van de blade selecteert u wilt weergeven in de lijst van de breadcrumb gevonden aan de bovenkant van de portal.
  ![Breadcrumb](media/azure-stack-update-1804/breadcrumb.png)

- <!-- TBD - IS --> Het is niet mogelijk om compute of opslag resources in de beheerdersportal weer te geven. De oorzaak van dit probleem is een fout opgetreden tijdens de installatie van de update zorgt ervoor dat de update niet correct worden gerapporteerd als geslaagd. Als dit probleem optreedt, moet u contact op met Microsoft Customer Support Services voor ondersteuning.

- <!-- TBD - IS --> U ziet een lege dashboard in de portal. Als u wilt herstellen het dashboard, selecteert u het pictogram tandwielpictogram in de rechterbovenhoek van de portal en selecteer vervolgens **standaardinstellingen herstellen**.

- <!-- TBD - IS ASDK --> Gebruiker abonnementen resulteert in een zwevende resources te verwijderen. Als tijdelijke oplossing Gebruikersbronnen of de hele resourcegroep eerst te verwijderen en verwijder vervolgens de gebruikersabonnementen.

- <!-- TBD - IS ASDK --> U kunt machtigingen aan uw abonnement met behulp van de portals Azure Stack niet weergeven. Als tijdelijke oplossing PowerShell te gebruiken om machtigingen te controleren.


### <a name="health-and-monitoring"></a>Status en bewaking
- <!-- 1264761 - IS ASDK -->  Mogelijk ziet u waarschuwingen voor de *Health-domeincontroller* onderdeel dat u hebt de volgende details:  

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

- <!-- 2368581 - IS. ASDK --> Een Azure-Stack-operator, als u een waarschuwing weinig geheugen ontvangt en virtuele machines van tenants niet te implementeren met een *fout bij het maken van de Fabric VM*, is het mogelijk dat de Azure-Stack stempel heeft te weinig geheugen beschikbaar. Gebruik de [Azure Stack Capacity Planner](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) beste leert de beschikbare capaciteit voor uw werkbelastingen. 


### <a name="compute"></a>Compute
- <!-- TBD - IS, ASDK --> Als u de grootte van een virtuele machine voor de implementatie van een virtuele machine, sommige F-serie VM-grootten zijn niet zichtbaar als onderdeel van de selector grootte wanneer u een virtuele machine maken. De volgende VM-grootten worden niet weergegeven in de selector: *F8s_v2*, *F16s_v2*, *F32s_v2*, en *F64s_v2*.  
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


- <!-- TBD - IS ASDK --> Instellingen voor virtuele-machineschaalsets schalen zijn niet beschikbaar in de portal. Als tijdelijke oplossing kunt u [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Vanwege verschillen tussen versies van PowerShell, moet u de `-Name` parameter in plaats van `-VMScaleSetName`.

- <!-- TBD - IS --> Bij het maken van een beschikbaarheidsset in de portal door te gaan naar **nieuw** > **Compute** > **beschikbaarheidsset**, kunt u alleen maken een beschikbaarheidsset met een domein met fouten en het updatedomein van 1. Als tijdelijke oplossing bij het maken van een nieuwe virtuele machine, maakt u de beschikbaarheidsset met behulp van PowerShell, CLI of vanuit de portal.

- <!-- TBD - IS ASDK --> Wanneer u virtuele machines op de gebruikersportal van Azure-Stack maakt, wordt een onjuist aantal gegevensschijven dat aan een DS-serie VM koppelen kunt in de portal weergegeven. DS-serie VMs aankan zo veel gegevensschijven als de configuratie van de Azure.

- <!-- TBD - IS ASDK --> Wanneer een installatiekopie van een virtuele machine niet kan worden gemaakt, kan een mislukte item dat u niet verwijderen, worden toegevoegd aan de VM-installatiekopieën compute-blade.

  Als tijdelijke oplossing door een nieuwe VM-installatiekopie te maken met een dummy VHD die kan worden gemaakt via de Hyper-V (New-VHD-pad C:\dummy.vhd-vaste - SizeBytes 1 GB). Dit proces los het probleem dat verhindert dat het mislukte item wordt verwijderd. Vervolgens kunt 15 minuten na het maken van de installatiekopie van het dummy, u met succes verwijderen.

  U kunt vervolgens redownload van de VM-installatiekopie die eerder is mislukt.

- <!-- TBD - IS ASDK --> Als u een uitbreiding op de implementatie van een VM-inrichting te lang duurt, door gebruikers laten de inrichting time-outwaarde in plaats van bij het stoppen van het proces voor de toewijzing ongedaan maken of verwijderen van de virtuele machine.  

- <!-- 1662991 IS ASDK --> Diagnostische gegevens van Linux-VM wordt niet ondersteund in Azure-Stack. Wanneer u een Linux-VM met diagnostische gegevens van virtuele machine is ingeschakeld implementeert, mislukt de implementatie. De implementatie mislukt ook als u de Linux-VM basismetrieken via de instellingen voor diagnostische inschakelt.  


### <a name="networking"></a>Netwerken
- <!-- TBD - IS ASDK --> U kan geen gebruiker gedefinieerde routes maken in de portal-beheerder of gebruiker. Gebruik als tijdelijke oplossing [Azure PowerShell](https://docs.microsoft.com/azure/virtual-network/tutorial-create-route-table-powershell).

- <!-- 1766332 - IS ASDK --> Onder **Networking**, als u op **VPN-Gateway maken** voor het instellen van een VPN-verbinding, **op basis van beleid** wordt vermeld als een VPN-type. Selecteer deze optie niet. Alleen de **Route gebaseerd** optie wordt ondersteund in Azure-Stack.

- <!-- 2388980 - IS ASDK --> Nadat een virtuele machine is gemaakt en gekoppeld aan een openbaar IP-adres, kunt u die virtuele machine uit dat IP-adres kan niet loskoppelen. Disassociation lijkt te werken, maar de eerder toegewezen openbaar IP-adres blijft gekoppeld aan de oorspronkelijke virtuele machine.

  Op dit moment moet u alleen nieuwe openbare IP-adressen voor de nieuwe virtuele machines die u maakt.

  Dit gebeurt zelfs als u een nieuwe virtuele machine het IP-adres toewijzen (gewoonlijk aangeduid als een *VIP's wisselen*). Alle toekomstige verbinding probeert te maken via dit resultaat van de IP-adres in van een verbinding met de oorspronkelijke virtuele machine en niet naar het nieuwe bericht.

- <!-- 2292271 - IS ASDK --> Als u een quotumlimiet voor een netwerkbron die deel uitmaakt van een aanbieding en het Plan dat is gekoppeld aan een tenantabonnement verhoogt, wordt de nieuwe limiet niet toegepast op dat abonnement. De nieuwe limiet is echter van toepassing op nieuwe abonnementen die zijn gemaakt nadat het quotum wordt verhoogd.

  Gebruik een invoegtoepassing plan quotum te verhogen een netwerk wanneer het abonnement al gekoppeld aan een abonnement is om dit probleem omzeilen. Zie voor meer informatie hoe [beschikbaar maken van een plan voor de invoegtoepassing](azure-stack-subscribe-plan-provision-vm.md#to-make-an-add-on-plan-available).

- <!-- 2304134 IS ASDK --> U kunt een abonnement met DNS-Zone-bronnen of routetabel resources zijn gekoppeld niet verwijderen. Om het abonnement is verwijderd, moet u eerst een DNS-Zone en routetabel resources verwijderen uit het tenantabonnement.


- <!-- 1902460 - IS ASDK --> Azure-Stack ondersteunt één *lokale netwerkgateway* per IP-adres. Dit geldt voor alle abonnementen van de tenant. Nadat het maken van de eerste lokale gateway netwerkverbinding, daaropvolgende pogingen tot het maken van een lokale gateway netwerkbron met hetzelfde IP-adres worden geblokkeerd.

- <!-- 16309153 - IS ASDK --> Op een virtueel netwerk dat is gemaakt met een DNS-Server-instelling van *automatische*, wijzigen voor een aangepaste DNS-Server mislukt. De bijgewerkte instellingen zijn niet doorgevoerd in de virtuele machines in dit Vnet.

- <!-- TBD - IS ASDK --> Azure-Stack biedt geen ondersteuning voor extra netwerkinterfaces toevoegen aan een VM-instantie nadat de virtuele machine is geïmplementeerd. Als de virtuele machine meer dan één netwerkinterface vereist, moeten ze tijdens de implementatie worden gedefinieerd.

- <!-- 2096388 IS --> U kunt het beheerportal niet gebruiken om bij te werken van regels voor een netwerkbeveiligingsgroep.

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

- <!-- TBD - IS --> Alleen de resourceprovider wordt voor het maken van items op servers die SQL-host of MySQL ondersteund. Items die worden gemaakt op een hostserver die niet zijn gemaakt door de resourceprovider kunnen leiden tot een niet-overeenkomende staat.  

- <!-- IS, ASDK --> Speciale tekens, inclusief spaties en perioden, worden niet ondersteund in de **familie** of **laag** namen bij het maken van een SKU voor de resourceproviders SQL en MySQL.


> [!NOTE]  
> <!-- TBD - IS --> Nadat u naar Azure Stack 1805 bijwerkt, kunt u blijven gebruiken van de SQL- en MySQL resourceproviders die u eerder hebt geïmplementeerd.  U wordt aangeraden dat u SQL en MySQL bijwerken wanneer er een nieuwe versie beschikbaar. Als Azure-Stack updates toepassen op SQL- en MySQL resourceproviders sequentieel worden verwerkt. Als u versie 1803, versie 1804 eerst toe te passen en vervolgens bijwerken naar 1805.      
>   
> De installatie van update 1805 heeft geen invloed op het huidige gebruik van SQL- of MySQL resourceproviders door uw gebruikers.
> Ongeacht de versie van de resourceproviders die u gebruikt de gegevens van uw gebruikers in hun databases is niet aangeraakt, en blijft toegankelijk.    



### <a name="app-service"></a>App Service
- <!-- 2352906 - IS ASDK --> Gebruikers moeten de storage resourceprovider registreren voordat ze hun eerste Azure-functie in het abonnement maken.

- <!-- 2489178 - IS ASDK --> Om de infrastructuur (werknemers, beheer, front-rollen) uitbreiden, moet u PowerShell gebruiken, zoals beschreven in de release-opmerkingen voor Compute.

- <!-- TBD - IS ASDK --> App Service kan alleen worden geïmplementeerd in de *standaard Provider-abonnement* op dit moment. In een toekomstige update App Service implementeert in de nieuwe *softwarelicentiecontrole abonnement* die is geïntroduceerd in Azure Stack 1804. Wanneer de meting wordt ondersteund voor gebruik, worden alle bestaande implementaties worden gemigreerd naar dit nieuwe abonnementstype.


### <a name="usage"></a>Gebruik  
- <!-- TBD - IS ASDK --> Gebruik van openbare IP-adres-gebruiksgegevens meter toont dezelfde *datum/tijd gebeurtenis* waarde voor elke record in plaats van de *TimeDate* tijdstempel die wordt aangegeven wanneer de record is gemaakt. U niet op dit moment kunnen deze gegevens gebruiken om uit te voeren nauwkeurige registratie van het gebruik van openbare IP-adres.


<!-- #### Identity -->
<!-- #### Marketplace -->


## <a name="download-the-update"></a>De update downloaden
U kunt het pakket met de Azure-Stack 1805 update downloaden [hier](https://aka.ms/azurestackupdatedownload).


## <a name="see-also"></a>Zie ook
- Voor het gebruik van de bevoegde eindpunt (PEP) om te controleren en hervatten van updates, Zie [in Azure-Stack met behulp van de bevoegde endpoint-updates controleren](azure-stack-monitor-update.md).
- Zie voor een overzicht van de update management in Azure-Stack [beheren van updates in de Azure-Stack overzicht](azure-stack-updates.md).
- Zie voor meer informatie over het toepassen van updates met de Azure-Stack [toepassen van updates in Azure-Stack](azure-stack-apply-updates.md).
