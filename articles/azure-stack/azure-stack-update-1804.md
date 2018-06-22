---
title: Azure Stack 1804 Update | Microsoft Docs
description: Meer informatie over wat er in de update 1804 voor Azure-Stack geïntegreerd systemen, de bekende problemen en waar u de update te downloaden.
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
ms.date: 05/30/2018
ms.author: brenduns
ms.reviewer: justini
ms.openlocfilehash: 2c2813a7f2d909a23c8f5d4f5ac0280b3f932ba6
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "34700121"
---
# <a name="azure-stack-1804-update"></a>Azure-Stack 1804 update

*Van toepassing op: Azure Stack geïntegreerd systemen*

In dit artikel worden de verbeteringen beschreven en oplossingen in het updatepakket met 1804, bekende problemen voor deze release en waar u de update te downloaden. Bekende problemen worden onderverdeeld in problemen met het rechtstreeks verband houden met het updateproces kan controleren en problemen met de build (na de installatie).

> [!IMPORTANT]        
> Dit updatepakket is alleen voor Azure-Stack geïntegreerd systemen. Pas dit pakket niet naar de Azure-Stack Development Kit.

## <a name="build-reference"></a>Verwijzing bouwen    
Het buildnummer van Azure Stack 1804 update **20180513.1**.   

### <a name="new-features"></a>Nieuwe functies
Deze update bevat de volgende verbeteringen voor Azure-Stack.

- <!-- 15028744 - IS -->  **Visual Studio-ondersteuning voor niet-verbonden Azure Stack-implementaties met AD FS**. In Visual Studio nu kunt u abonnementen toevoegt en verifiëren federatieve met AD FS gebruikersreferenties. 
 
- <!-- 1779474, 1779458 - IS --> **Gebruik virtuele machines voor reeks Av2 en F**. Azure Stack kan nu gebruikmaken van virtuele machines op basis van de grootte van de virtuele machines Av2-serie en F-serie. Zie voor meer informatie [grootten van virtuele machines in Azure-Stack ondersteund](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes). 

- <!-- 1759172 - IS, ASDK --> **Nieuwe beheerdersrechten abonnementen**. Met 1804 zijn twee nieuwe typen van abonnementen beschikbaar in de portal. Deze nieuwe abonnement-typen worden naast de standaard Provider-abonnement en zichtbaar met de nieuwe Azure-Stack installaties vanaf versie 1804. *Deze nieuwe typen abonnementen niet gebruiken voor deze versie van Azure-Stack*. We zullen bekendmaken voor het gebruik van deze typen abonnement aan met een toekomstige update. 

  Als u Azure-Stack naar versie 1804 bijwerken, zijn de twee nieuwe abonnement typen niet zichtbaar. Echter nieuwe implementaties van Azure-Stack geïntegreerd systemen en installaties van de versie van de Azure-Stack Development Kit 1804 of hoger hebben toegang tot alle drie abonnement typen.  

  Deze nieuwe abonnement typen uitmaken deel van een grotere wijziging voor het beveiligen van de standaard Provider-abonnement en gemakkelijker gedeelde resources zoals Hosting van de SQL-servers implementeren. Als er meer onderdelen van deze groter is gewijzigd in toekomstige updates aan Azure-Stack toevoegen, kunnen resources geïmplementeerd onder deze nieuwe abonnement typen verloren zijn gegaan. 

  De drie abonnement typen nu zichtbaar zijn:  
  - Standaard Provider-abonnement: blijven gebruiken van dit abonnementstype. 
  - Softwarelicentiecontrole abonnement: *dit abonnementstype niet gebruiken.*
  - Abonnement voor verbruik: *gebruik geen dit abonnementstype*

  



## <a name="fixed-issues"></a>Opgeloste problemen

- <!-- IS, ASDK -->  In het beheerportal hoeft u niet langer de tegel Update vernieuwen voordat deze informatie wordt weergegeven.
 
- <!-- 2050709  -->  U kunt nu het beheerportal opslag metrische gegevens voor Blob-service, service Table en Queue-service bewerken.
 
- <!-- IS, ASDK --> Onder **Networking**, wanneer u klikt op **verbinding** voor het instellen van een VPN-verbinding, **Site-naar-site (IPsec)** is nu de enige beschikbare optie.

- **Verschillende oplossingen** voor prestaties, stabiliteit, beveiliging en het besturingssysteem dat wordt gebruikt door de Azure-Stack.

## <a name="additional-releases-timed-with-this-update"></a>Aanvullende versies is een time-out opgetreden met deze update  
De volgende zijn nu beschikbaar, maar geen Azure-Stack update 1804 vereist.
- **Bijwerken naar de Microsoft Azure-Stack System Center Operations Manager-Bewakingspakket**. Een nieuwe versie (1.0.3.0) van de Microsoft System Center Operations Manager Monitoring Pack voor Azure-Stack is beschikbaar voor [downloaden](https://www.microsoft.com/download/details.aspx?id=55184). Met deze versie kunt u de Service-Principals wanneer u een gekoppelde Azure-Stack-implementatie toevoegen. Deze versie biedt ook een updatebeheer ervaring die kunt u de herstelactie rechtstreeks vanuit binnen Operations Manager. Er zijn ook nieuwe dashboards die resourceproviders weergeven, schaaleenheden en schalen eenheid knooppunten.

- **Nieuwe Azure stapelen Admin PowerShell-versie 1.3.0**.  Azure-Stack PowerShell 1.3.0 is nu beschikbaar voor installatie. Deze versie bevat opdrachten voor alle resourceproviders van beheerder voor het beheren van Azure-Stack.  Sommige inhoud in deze versie wordt afgeschaft vanuit de Azure-Stack extra GitHub [opslagplaats](https://github.com/Azure/AzureStack-Tools). 

   Ga als volgt voor details over de installatie, de [instructies](azure-stack-powershell-install.md) of de [help](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.3.0) voor Azure Stack Module 1.3.0 inhoud. 

- **Initiële release van Azure Rest Stack API-verwijzing**. De [API-referentiemateriaal voor alle Azure-Stack Admin resourceproviders](https://docs.microsoft.com/rest/api/azure-stack/) nu is gepubliceerd. 


## <a name="before-you-begin"></a>Voordat u begint    

### <a name="prerequisites"></a>Vereisten
- Installeer de Azure-Stack [1803 bijwerken](azure-stack-update-1803.md) voordat u de Azure-Stack 1804 update toepassen.    

### <a name="known-issues-with-the-update-process"></a>Bekende problemen met het updateproces kan controleren   
- Tijdens de installatie van de update 1804 ziet u mogelijk waarschuwingen met de titel *fout: sjabloon voor FaultType UserAccounts.New ontbreekt.*  U kunt deze waarschuwingen negeren. Deze waarschuwingen wordt automatisch gesloten nadat de update naar 1804 is voltooid.   
 
- <!-- TBD - IS --> Probeer niet voor het maken van virtuele machines tijdens de installatie van deze update. Zie voor meer informatie over het beheren van updates, [beheren van updates in de Azure-Stack overzicht](azure-stack-updates.md#plan-for-updates).


### <a name="post-update-steps"></a>Stappen na het bijwerken
*Er zijn geen stappen na-update voor de update 1804.*



### <a name="known-issues-post-installation"></a>Bekende problemen (na de installatie)
Hieronder vindt u na de installatie bekende problemen voor de build **20180513.1**.

#### <a name="portal"></a>Portal
- <!-- 1272111 - IS --> Na het installeren of bijwerken naar deze versie van Azure-Stack, kunt u mogelijk niet schaaleenheden Azure Stack weergeven in de beheerportal.  
  Tijdelijke oplossing: Gebruik PowerShell om weer te geven informatie over schaaleenheden. Zie voor meer informatie de [help](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.3.0) voor Azure Stack Module 1.3.0 inhoud. 

- <!-- 2332636 - IS -->  Wanneer u AD FS voor uw Azure-Stack identiteitssysteem en de update in deze versie van Azure-Stack, de standaardeigenaar van het abonnement van de provider standaard ingesteld op de ingebouwde **CloudAdmin** gebruiker.  
  Tijdelijke oplossing: Los dit probleem nadat u deze update installeert, gebruikt u stap 3 van de [Trigger automation voor het configureren van een claimprovider in Azure-Stack](azure-stack-integrate-identity.md#trigger-automation-to-configure-claims-provider-trust-in-azure-stack-1) procedure opnieuw instellen van de eigenaar van het standaard provider-abonnement.   

- <!-- TBD - IS ASDK --> Sommige typen beheerdersrechten abonnementen zijn niet beschikbaar.  Wanneer u Azure-Stack naar deze versie bijwerkt, worden de typen van de twee abonnementen zijn [geïntroduceerd in versie 1804](#new-features) zijn niet zichtbaar in de console. Dit is normaal gedrag. Het abonnement niet beschikbaar-typen zijn *softwarelicentiecontrole abonnement*, en *verbruik abonnement*. Deze typen abonnement zichtbaar zijn in de nieuwe Azure-Stack omgevingen vanaf versie 1804 maar nog niet klaar voor gebruik. U moet blijven gebruiken de *Provider standaard* abonnementstype.  


- <!-- TBD -  IS ASDK -->De mogelijkheid [een nieuwe ondersteuningsaanvraag openen vanuit de vervolgkeuzelijst](azure-stack-manage-portals.md#quick-access-to-help-and-support) uit binnen de beheerder van de portal is niet beschikbaar. Gebruik in plaats daarvan de volgende koppeling:     
    - Gebruik voor Azure-Stack geïntegreerde systemen https://aka.ms/newsupportrequest.

- <!-- 2403291 - IS ASDK --> Hebt u mogelijk geen gebruik van de horizontale schuifbalk langs de onderkant van de portals beheerder en gebruiker. Als u geen toegang de horizontale schuifbalk tot, links gebruik de breadcrumbs om te navigeren naar een vorige blade in de portal voor u de naam van de blade selecteert u wilt weergeven in de lijst van de breadcrumb gevonden aan de bovenkant van de portal.
  ![Breadcrumb](media/azure-stack-update-1804/breadcrumb.png) 

- <!-- TBD - IS --> Het is niet mogelijk om compute of opslag resources in de beheerdersportal weer te geven. De oorzaak van dit probleem is een fout opgetreden tijdens de installatie van de update zorgt ervoor dat de update niet correct worden gerapporteerd als geslaagd. Als dit probleem optreedt, moet u contact op met Microsoft Customer Support Services voor ondersteuning.

- <!-- TBD - IS --> U ziet een lege dashboard in de portal. Als u wilt herstellen het dashboard, selecteert u het pictogram tandwielpictogram in de rechterbovenhoek van de portal en selecteer vervolgens **standaardinstellingen herstellen**.

- <!-- TBD - IS ASDK --> Gebruiker abonnementen resulteert in een zwevende resources te verwijderen. Als tijdelijke oplossing Gebruikersbronnen of de hele resourcegroep eerst te verwijderen en verwijder vervolgens de gebruikersabonnementen.

- <!-- TBD - IS ASDK --> U kunt machtigingen aan uw abonnement met behulp van de portals Azure Stack niet weergeven. Als tijdelijke oplossing PowerShell te gebruiken om machtigingen te controleren.

- <!-- TBD - IS ASDK --> In het beheerportal, ziet u mogelijk een kritieke waarschuwing voor de *Microsoft.Update.Admin* onderdeel. De naam van waarschuwing, beschrijving en herstel alle als weergegeven:  
    - *Fout - sjabloon voor FaultType ResourceProviderTimeout ontbreekt.*

  Deze waarschuwing kan worden genegeerd. 


#### <a name="health-and-monitoring"></a>Status en bewaking
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

  Beide waarschuwingen kunnen worden genegeerd. Ze worden automatisch gesloten na verloop van tijd.  
 

#### <a name="compute"></a>Compute
- <!-- TBD - IS --> Als u de grootte van een virtuele machine voor de implementatie van een virtuele machine, sommige F-serie VM-grootten zijn niet zichtbaar als onderdeel van de selector grootte wanneer u een virtuele machine maken. De volgende VM-grootten worden niet weergegeven in de selector: *F8s_v2*, *F16s_v2*, *F32s_v2*, en *F64s_v2*.  
  Als tijdelijke oplossing een van de volgende methoden te gebruiken voor het implementeren van een virtuele machine. In elke methode moet u opgeven van de VM-grootte die u wilt gebruiken.

  - **Azure Resource Manager-sjabloon:** als u een sjabloon gebruikt, stelt de *vmSize* in de sjabloon zodat deze overeenkomt met de gewenste VM-grootte. Bijvoorbeeld, het volgende wordt gebruikt voor het implementeren van een virtuele machine die gebruikmaakt van de *F32s_v2* grootte:  

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

- <!-- TBD - IS ASDK --> Wanneer u virtuele machines op de gebruikersportal van Azure-Stack maakt, wordt een onjuist aantal gegevensschijven dat aan een virtuele machine van de D-reeks koppelen kunt weergegeven in de portal. Alle ondersteunde D-reeks VMs aankan zo veel gegevensschijven als de configuratie van de Azure.

- <!-- TBD - IS ASDK --> Wanneer een installatiekopie van een virtuele machine niet kan worden gemaakt, kan een mislukte item dat u niet verwijderen, worden toegevoegd aan de VM-installatiekopieën compute-blade.

  Als tijdelijke oplossing door een nieuwe VM-installatiekopie te maken met een dummy VHD die kan worden gemaakt via de Hyper-V (New-VHD-pad C:\dummy.vhd-vaste - SizeBytes 1 GB). Dit proces los het probleem dat verhindert dat het mislukte item wordt verwijderd. Vervolgens kunt 15 minuten na het maken van de installatiekopie van het dummy, u met succes verwijderen.

  U kunt vervolgens redownload van de VM-installatiekopie die eerder is mislukt.

- <!-- TBD - IS ASDK --> Als u een uitbreiding op de implementatie van een VM-inrichting te lang duurt, door gebruikers laten de inrichting time-outwaarde in plaats van bij het stoppen van het proces voor de toewijzing ongedaan maken of verwijderen van de virtuele machine.  

- <!-- 1662991 IS ASDK --> Diagnostische gegevens van Linux-VM wordt niet ondersteund in Azure-Stack. Wanneer u een Linux-VM met diagnostische gegevens van virtuele machine is ingeschakeld implementeert, mislukt de implementatie. De implementatie mislukt ook als u de Linux-VM basismetrieken via de instellingen voor diagnostische inschakelt.  


#### <a name="networking"></a>Netwerken
- <!-- 1766332 - IS ASDK --> Onder **Networking**, als u op **VPN-Gateway maken** voor het instellen van een VPN-verbinding, **op basis van beleid** wordt vermeld als een VPN-type. Selecteer deze optie niet. Alleen de **Route gebaseerd** optie wordt ondersteund in Azure-Stack.

- <!-- 2388980 - IS ASDK --> Nadat een virtuele machine is gemaakt en gekoppeld aan een openbaar IP-adres, kunt u die virtuele machine uit dat IP-adres kan niet loskoppelen. Disassociation lijkt te werken, maar de eerder toegewezen openbaar IP-adres blijft gekoppeld aan de oorspronkelijke virtuele machine.

  Op dit moment moet u alleen nieuwe openbare IP-adressen voor de nieuwe virtuele machines die u maakt.

  Dit gebeurt zelfs als u een nieuwe virtuele machine het IP-adres toewijzen (gewoonlijk aangeduid als een *VIP's wisselen*). Alle toekomstige verbinding probeert te maken via dit resultaat van de IP-adres in van een verbinding met de oorspronkelijk gekoppelde virtuele machine en niet naar het nieuwe bericht.

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

#### <a name="sql-and-mysql"></a>SQL- en MySQL

- <!-- TBD - IS --> Alleen de resourceprovider wordt voor het maken van items op servers die SQL-host of MySQL ondersteund. Items die worden gemaakt op een hostserver die niet zijn gemaakt door de resourceprovider kunnen leiden tot een niet-overeenkomende staat.  

- <!-- IS, ASDK --> Speciale tekens, inclusief spaties en perioden, worden niet ondersteund in de **familie** of **laag** namen bij het maken van een SKU voor de resourceproviders SQL en MySQL.


> [!NOTE]  
> <!-- TBD - IS --> Nadat u naar Azure Stack 1804 bijwerkt, kunt u blijven gebruiken van de SQL- en MySQL resourceproviders die u eerder hebt geïmplementeerd.  U wordt aangeraden dat u SQL en MySQL bijwerken wanneer er een nieuwe versie beschikbaar. Als Azure-Stack updates toepassen op SQL- en MySQL resourceproviders sequentieel worden verwerkt.  Als u versie 1802, versie 1803 eerst toe te passen en vervolgens bijwerken naar 1804.      
>   
> De installatie van update 1804 heeft geen invloed op het huidige gebruik van SQL- of MySQL resourceproviders door uw gebruikers.
> Ongeacht de versie van de resourceproviders die u gebruikt de gegevens van uw gebruikers in hun databases is niet aangeraakt, en blijft toegankelijk.    



#### <a name="app-service"></a>App Service
- <!-- 2352906 - IS ASDK --> Gebruikers moeten de storage resourceprovider registreren voordat ze hun eerste Azure-functie in het abonnement maken.

- <!-- TBD - IS ASDK --> Om de infrastructuur (werknemers, beheer, front-rollen) uitbreiden, moet u PowerShell gebruiken, zoals beschreven in de release-opmerkingen voor Compute.

- <!-- TBD - IS ASDK --> App Service kan alleen worden geïmplementeerd in de **Provider-abonnement standaard** op dit moment.  In een toekomstige update App Service implementeert in het nieuwe abonnement softwarelicentiecontrole geïntroduceerd in Azure Stack 1804 en alle bestaande implementaties worden gemigreerd naar het nieuwe abonnement ook.

#### <a name="usage"></a>Gebruik  
- <!-- TBD - IS ASDK --> Gebruik van openbare IP-adres-gebruiksgegevens meter toont dezelfde *datum/tijd gebeurtenis* waarde voor elke record in plaats van de *TimeDate* tijdstempel die wordt aangegeven wanneer de record is gemaakt. U niet op dit moment kunnen deze gegevens gebruiken om uit te voeren nauwkeurige registratie van het gebruik van openbare IP-adres.


<!-- #### Identity -->
<!-- #### Marketplace --> 


## <a name="download-the-update"></a>De update downloaden
U kunt het pakket met de Azure-Stack 1804 update downloaden [hier](https://aka.ms/azurestackupdatedownload).


## <a name="see-also"></a>Zie ook
- Voor het gebruik van de bevoegde eindpunt (PEP) om te controleren en hervatten van updates, Zie [in Azure-Stack met behulp van de bevoegde endpoint-updates controleren](azure-stack-monitor-update.md).
- Zie voor een overzicht van de update management in Azure-Stack [beheren van updates in de Azure-Stack overzicht](azure-stack-updates.md).
- Zie voor meer informatie over het toepassen van updates met de Azure-Stack [toepassen van updates in Azure-Stack](azure-stack-apply-updates.md).
