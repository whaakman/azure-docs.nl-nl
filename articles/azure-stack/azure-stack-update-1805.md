---
title: Azure Stack 1805 Update | Microsoft Docs
description: Meer informatie over wat is er nieuw in de update 1805 voor geïntegreerde Azure Stack-systemen, met inbegrip van de bekende problemen en waar u de update te downloaden.
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
ms.date: 08/27/2018
ms.author: brenduns
ms.reviewer: justini
ms.openlocfilehash: 4db0ce5e877f3054cc41e8940e8d9e672f7632c6
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43110345"
---
# <a name="azure-stack-1805-update"></a>Azure Stack 1805 update

*Is van toepassing op: Azure Stack-geïntegreerde systemen*

Dit artikel worden de verbeteringen en in het updatepakket met 1805, bekende problemen voor deze versie en het downloaden van de update worden opgelost. Bekende problemen zijn onderverdeeld in problemen direct verband houden met het updateproces en problemen met de build (na de installatie).

> [!IMPORTANT]        
> Dit pakket is alleen voor geïntegreerde Azure Stack-systemen. Dit pakket niet van toepassing op de Azure Stack Development Kit.

## <a name="build-reference"></a>Naslaginformatie over bouwen    
De Azure Stack 1805 update build-nummer **1.1805.1.47**.  

> [!TIP]  
> Op basis van feedback van klanten, wordt er een update voor de versie-schema voor Microsoft Azure Stack in gebruik is.  Beginnen met deze update 1805, geeft het nieuwe schema beter de huidige cloudversie.  
> 
> De versie-schema is nu *Version.YearYearMonthMonth.MinorVersion.BuildNumber* waar de tweede en derde sets geven de versie en release. Bijvoorbeeld, 1805.1 vertegenwoordigt de *RTM release* (RTM)-versie van 1805.  


### <a name="new-features"></a>Nieuwe functies
Deze update bevat de volgende verbeteringen voor Azure Stack.
<!-- 2297790 - IS, ASDK --> 
- **Azure Stack bevat nu een *Syslog* client** als een *preview-functie*. Deze client kunt het doorsturen van Logboeken voor controle en beveiliging met betrekking tot de Azure Stack-infrastructuur naar een Syslog-server of security information en event management (SIEM) software die zich buiten Azure Stack. De Syslog-client ondersteunt momenteel alleen niet-geverifieerde UDP-verbindingen via standaardpoort 514. De nettolading van elk Syslog-bericht is opgemaakt in Common Event Format (CEF). 

  Gebruik voor het configureren van de Syslog-client de **Set SyslogServer** cmdlet die beschikbaar zijn in het Privileged eindpunt. 

  Met deze Preview-versie ziet u mogelijk de volgende drie waarschuwingen. Wanneer wordt aangeboden door Azure Stack, deze waarschuwingen bevatten *beschrijvingen* en *herstel* richtlijnen. 
  - TITEL: Code-integriteit uitschakelen  
  - TITEL: Code-integriteit in de controlemodus 
  - TITEL: Gebruikersaccount die is gemaakt

  Hoewel deze functie nog in preview, moet deze niet wordt vertrouwd in een productieomgeving.   




### <a name="fixed-issues"></a>Opgeloste problemen

<!-- # - applicability -->
- We het probleem opgelost dat geblokkeerd [een nieuwe ondersteuningsaanvraag te openen in de vervolgkeuzelijst](azure-stack-manage-portals.md#quick-access-to-help-and-support) uit in de beheerportal. Nu deze optie werkt zoals bedoeld. 

- **Verschillende oplossingen** voor prestaties, stabiliteit, beveiliging en het besturingssysteem dat wordt gebruikt door Azure Stack.


<!-- # Additional releases timed with this update    -->



## <a name="before-you-begin"></a>Voordat u begint    

### <a name="prerequisites"></a>Vereisten
- De Azure-Stack installeren [1804 bijwerken](azure-stack-update-1804.md) voordat u de Azure Stack 1805 update toepassen.  
- Installeer de meest recente beschikbare [update of hotfix voor versie 1804](azure-stack-update-1804.md#post-update-steps).   
- Voordat u begint met de installatie van update 1805, voert u [Test AzureStack](azure-stack-diagnostic-test.md) om te valideren van de status van uw Azure-Stack en los eventuele operationele problemen gevonden. Ook actieve waarschuwingen bekijken en op te lossen die actie is vereist. 

### <a name="known-issues-with-the-update-process"></a>Bekende problemen met het updateproces   
- Tijdens de installatie van de update 1805, ziet u mogelijk waarschuwingen met de titel *fout: sjabloon voor FaultType UserAccounts.New ontbreekt.*  U kunt deze waarschuwingen negeren. Deze waarschuwingen worden automatisch gesloten nadat de update van 1805 is voltooid.   

- <!-- 2489559 - IS --> Probeer niet voor het maken van virtuele machines tijdens de installatie van deze update. Zie voor meer informatie over het beheren van updates [-updates beheren in Azure Stack-overzicht](azure-stack-updates.md#plan-for-updates).


### <a name="post-update-steps"></a>Stappen na het bijwerken
Na de installatie van 1805, eventuele van toepassing Hotfixes te installeren. Raadpleeg voor meer informatie de volgende knowledge base-artikelen, evenals onze [beleid onderhoud](azure-stack-servicing-policy.md).  
 - [KB 4344102 - Azure Stack-Hotfix 1.1805.7.57](https://support.microsoft.com/help/4344102).


## <a name="known-issues-post-installation"></a>Bekende problemen (na de installatie)
Hier volgen na de installatie bekende problemen voor deze buildversie.

### <a name="portal"></a>Portal  
- <!-- 2931230 – IS  ASDK --> Abonnementen die zijn toegevoegd aan een gebruikersabonnement als een aanvullende plan kunnen niet worden verwijderd, zelfs wanneer u het abonnement uit het gebruikersabonnement verwijderen. Het abonnement blijft totdat de abonnementen die verwijzen naar het aanvullende plan worden ook verwijderd. 

- <!-- TBD - IS ASDK --> U kunt geen updates voor stuurprogramma's toepassen met behulp van een OEM-uitbreidingspakket met deze versie van Azure Stack.  Er is geen oplossing voor dit probleem.

- <!-- 2551834 - IS, ASDK --> Wanneer u selecteert **overzicht** voor een opslagaccount in de beheerder of de gebruiker-portals, de gegevens uit de *Essentials* deelvenster niet wordt weergegeven.  Het deelvenster Essentials geeft informatie weer over het account, zoals de *resourcegroep*, *locatie*, en *abonnements-ID*.  Andere opties voor overzicht zijn beschikbaar zijn, zoals *Services* en *bewaking*, ook als opties voor *openen in Explorer* of *storage-account verwijderen* . 

  Als u de informatie niet beschikbaar is, gebruikt u de [Get-azureRMstorageaccount](https://docs.microsoft.com/powershell/module/azurerm.storage/get-azurermstorageaccount?view=azurermps-6.2.0) PowerShell-cmdlet. 

- <!-- 2551834 - IS, ASDK --> Wanneer u selecteert **Tags** voor een opslagaccount in de beheerder of de gebruiker-portals, de gegevens te laden is mislukt en niet wordt weergegeven.  

  Als u de informatie niet beschikbaar is, gebruikt u de [Get-AzureRmTag](https://docs.microsoft.com/powershell/module/azurerm.tags/get-azurermtag?view=azurermps-6.2.0) PowerShell-cmdlet.


- <!-- 2332636 - IS -->  Wanneer u AD FS voor uw Azure Stack-identiteitssysteem bij te werken naar deze versie van Azure Stack, de standaardeigenaar van het standaardabonnement provider opnieuw wordt ingesteld in de ingebouwde **CloudAdmin** gebruiker.  
  Tijdelijke oplossing: Gebruiken om op te lossen dit probleem nadat u deze update installeert, stap 3 van de [Trigger automation configureren claims provider vertrouwensrelatie in Azure Stack](azure-stack-integrate-identity.md#trigger-automation-to-configure-claims-provider-trust-in-azure-stack-1) procedure voor het opnieuw instellen van de eigenaar van de provider standaardabonnement.   

- <!-- TBD - IS ASDK --> Sommige typen administratieve abonnementen zijn niet beschikbaar.  Wanneer u een upgrade uitvoert van Azure Stack in deze versie, worden de twee abonnementstypen zijn [geïntroduceerd in versie 1804](azure-stack-update-1804.md#new-features) zijn niet zichtbaar in de console. Dit is normaal gedrag. Het niet beschikbaar-abonnementstypen zijn *softwarelicentiecontrole abonnement*, en *verbruik abonnement*. Deze abonnementstypen zijn zichtbaar in de nieuwe Azure Stack-omgevingen vanaf versie 1804 maar nog niet klaar voor gebruik. U moet echter ook doorgaan met de *Provider standaard* abonnementstype.  

- <!-- 2403291 - IS ASDK --> Hebt u mogelijk geen gebruik van de horizontale schuifbalk langs de onderkant van de beheerder en gebruiker portals. Als u geen toegang de horizontale schuifbalk tot, blijven gebruiken de breadcrumbs om naar een vorige blade in de portal navigeren zijn door de naam van de blade te selecteren dat u wilt weergeven in de breadcrumb-lijst gevonden aan de bovenkant van de portal.
  ![Breadcrumb](media/azure-stack-update-1804/breadcrumb.png)

- <!-- TBD - IS --> Het is mogelijk niet mogelijk om te berekenen of opslaan resources weergeven in de beheerdersportal. De oorzaak van dit probleem is een fout opgetreden tijdens de installatie van de update die ervoor zorgt dat de update worden niet correct gerapporteerd als geslaagd. Als dit probleem optreedt, moet u contact op met Microsoft Customer Support Services voor ondersteuning.

- <!-- TBD - IS --> U ziet een leeg dashboard in de portal. Als u wilt herstellen in het dashboard, selecteert u het tandwielpictogram in de rechterbovenhoek van de portal en selecteer vervolgens **standaardinstellingen herstellen**.

- <!-- TBD - IS ASDK --> Verwijderen van zwevende resources leidt van gebruiker-abonnementen. Als tijdelijke oplossing, eerst Gebruikersbronnen of de hele resourcegroep verwijderen en verwijder vervolgens gebruikersabonnementen.

- <!-- TBD - IS ASDK --> U kunt machtigingen aan uw abonnement met behulp van de Azure Stack-portals niet weergeven. Als tijdelijke oplossing, moet u PowerShell gebruiken om machtigingen te controleren.


### <a name="health-and-monitoring"></a>Status en bewaking
- <!-- 1264761 - IS ASDK -->  Mogelijk ziet u waarschuwingen voor de *Health controller* onderdeel waarvoor u de volgende gegevens:  
- 
   Waarschuwing #1:
   - NAAM: De functie van de infrastructuur is niet in orde
   - ERNST: waarschuwing
   - ONDERDEEL: De gezondheid van controller
   - Beschrijving: De health-controller Heartbeat-Scanner is niet beschikbaar. Dit kan invloed hebben op statusrapporten en metrische gegevens.  

  Waarschuwing #2:
   - NAAM: De functie van de infrastructuur is niet in orde
   - ERNST: waarschuwing
   - ONDERDEEL: De gezondheid van controller
   - Beschrijving: De health-controller fouttolerantie Scanner is niet beschikbaar. Dit kan invloed hebben op statusrapporten en metrische gegevens.

  Beide waarschuwingen #1 en 2 # kunnen veilig worden genegeerd en wordt automatisch gesloten na verloop van tijd. 

  U ziet ook de volgende waarschuwing voor *capaciteit*. Voor deze waarschuwing kan verschillen van het percentage beschikbaar geheugen is geïdentificeerd in de beschrijving:  

  Waarschuwing #3:
   - NAAM: Onvoldoende geheugen beschikbaar capaciteit
   - ERNST: kritiek
   - COMPONENT: capaciteit
   - Beschrijving: De regio heeft meer dan 80,00% van het beschikbare geheugen verbruikt. Het maken van virtuele machines met grote hoeveelheden geheugen kan mislukken.  

  In deze versie van Azure Stack, kan deze waarschuwing niet correct gestart. Als virtuele machines van tenants succesvol zijn geïmplementeerd blijft, kunt u veilig deze waarschuwing negeren. 
  
  Waarschuwing #3 wordt niet automatisch gesloten. Als u deze waarschuwing sluiten maakt Azure Stack dezelfde waarschuwing binnen 15 minuten.  

- <!-- 2368581 - IS. ASDK --> Als Azure Stack-operators, als u een waarschuwing voor een beperkte hoeveelheid geheugen ontvangt en virtuele machines van tenants niet te implementeren met een *fout bij het maken van infrastructuur-VM*, is het mogelijk dat de Azure Stack-stempel heeft te weinig geheugen beschikbaar. Gebruik de [Azure Stack Capacity Planner](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) naar het beste informatie over de beschikbare capaciteit voor uw workloads. 


### <a name="compute"></a>Compute
- <!-- TBD - IS, ASDK --> Bij het selecteren van een VM-grootte voor de implementatie van een virtuele machine, sommige F-serie VM-grootten zijn niet zichtbaar als onderdeel van de grootte selector bij het maken van een virtuele machine. De volgende VM-grootten worden niet weergegeven in de kiezer: *F8s_v2*, *F16s_v2*, *F32s_v2*, en *F64s_v2*.  
  Als tijdelijke oplossing, gebruikt u een van de volgende methoden om een VM te implementeren. In elke methode moet u om op te geven van de VM-grootte die u wilt gebruiken.

  - **Azure Resource Manager-sjabloon:** wanneer u een sjabloon gebruikt, stelt de *vmSize* in de sjabloon zodat deze overeenkomt met de VM-grootte die u wilt gebruiken. Bijvoorbeeld, de volgende vermelding wordt gebruikt voor het implementeren van een virtuele machine die gebruikmaakt van de *F32s_v2* grootte:  

    ```
        "properties": {
        "hardwareProfile": {
                "vmSize": "Standard_F32s_v2"
        },
    ```  
  - **Azure CLI:** kunt u de [az vm maken](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create) opdracht en geeft u de VM-grootte als een parameter, die vergelijkbaar is met `--size "Standard_F32s_v2"`.

  - **PowerShell:** met PowerShell kunt u [New-AzureRMVMConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvmconfig?view=azurermps-6.0.0) met de parameter die aangeeft van de VM-grootte, die vergelijkbaar is met `-VMSize "Standard_F32s_v2"`.


- <!-- TBD - IS ASDK --> Instellingen voor vergroten/verkleinen voor schaalsets voor virtuele machines zijn niet beschikbaar in de portal. Als tijdelijke oplossing, kunt u [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Vanwege de verschillen tussen versies van PowerShell, moet u de `-Name` parameter in plaats van `-VMScaleSetName`.

- <!-- TBD - IS --> Bij het maken van een beschikbaarheidsset in de portal door te gaan naar **nieuw** > **Compute** > **beschikbaarheidsset**, kunt u alleen maken een beschikbaarheidsset met een foutdomein en updatedomein van 1. Als tijdelijke oplossing, bij het maken van een nieuwe virtuele machine, maak de beschikbaarheidsset met behulp van PowerShell, CLI, of rechtstreeks vanuit de portal.

- <!-- TBD - IS ASDK --> Wanneer u virtuele machines op de gebruikersportal van Azure Stack maakt, wordt een onjuist aantal gegevensschijven die aan een virtuele machine uit de DS-serie kunt koppelen door de portal weergegeven. DS-serie VM's kan zo veel gegevensschijven bevatten als de Azure-configuratie.

- <!-- TBD - IS ASDK --> Wanneer een VM-installatiekopie niet kan worden gemaakt, kan een mislukte-item dat u niet verwijderen worden toegevoegd aan de VM-installatiekopieën compute-blade.

  Als tijdelijke oplossing, kunt u een nieuwe VM-installatiekopie maken met een dummy VHD dat kan worden gemaakt via Hyper-V (New-VHD-pad C:\dummy.vhd-vaste - SizeBytes 1 GB). Dit proces los het probleem dat verhindert dat het mislukte item wordt verwijderd. Vervolgens, 15 minuten na het maken van de installatiekopie van het pop, u kunt met succes verwijderen.

  U kunt vervolgens redownload van de VM-installatiekopie die eerder is mislukt.

- <!-- TBD - IS ASDK --> Als een uitbreiding op een VM-implementatie de inrichting te lang duurt, laat gebruikers de time-out van de inrichting in plaats van bij stoppen van de procedure voor de toewijzing ongedaan of verwijder de virtuele machine.  

- <!-- 1662991 IS ASDK --> Diagnostische gegevens over Linux-VM wordt niet ondersteund in Azure Stack. Wanneer u een Linux-VM met VM diagnostics is ingeschakeld implementeren, wordt de implementatie mislukt. De implementatie mislukt ook als u de Linux-VM eenvoudige metrische gegevens via diagnostische instellingen inschakelen.  


### <a name="networking"></a>Netwerken
- <!-- TBD - IS ASDK --> U kan geen gebruiker gedefinieerde routes maken in de portal-beheerder of gebruiker. Als tijdelijke oplossing gebruiken [Azure PowerShell](https://docs.microsoft.com/azure/virtual-network/tutorial-create-route-table-powershell).

- <!-- 1766332 - IS ASDK --> Onder **netwerken**, als u klikt op **VPN-Gateway maken** voor het instellen van een VPN-verbinding, **op basis van beleid** wordt vermeld als een VPN-type. Selecteer deze optie niet. Alleen de **Route op basis van** optie wordt ondersteund in Azure Stack.

- <!-- 2388980 - IS ASDK --> Nadat een virtuele machine is gemaakt en die zijn gekoppeld aan een openbaar IP-adres, kunt u deze VM op basis van IP-adres kan niet loskoppelen. Ontkoppeling lijkt te werken, maar het eerder toegewezen openbare IP-adres blijft gekoppeld aan de oorspronkelijke virtuele machine.

  Op dit moment moet u alleen nieuwe openbare IP-adressen voor nieuwe virtuele machines die u maakt.

  Dit gebeurt zelfs als u het IP-adres aan een nieuwe virtuele machine opnieuw toewijzen (vaak aangeduid als een *wisselen van VIP*). Alle toekomstige probeert verbinding maken via dit resultaat van de IP-adres in een verbinding met de oorspronkelijke virtuele machine en niet naar de nieuwe vertakking.

- <!-- 2292271 - IS ASDK --> Als u een quotumlimiet voor een netwerkbron die deel uitmaakt van een aanbieding en een Plan dat is gekoppeld aan een tenantabonnement verhoogt, wordt de nieuwe limiet niet toegepast op dat aan het abonnement. De nieuwe limiet is echter van toepassing op nieuwe abonnementen die zijn gemaakt nadat het quotum is verhoogd.

  U kunt dit probleem omzeilen, door een aanvullende plan te gebruiken voor het verhogen van een quotum op het netwerk wanneer het abonnement al gekoppeld aan een abonnement is. Zie voor meer informatie over het [beschikbaar maken voor een aanvullende plan](azure-stack-subscribe-plan-provision-vm.md#to-make-an-add-on-plan-available).

- <!-- 2304134 IS ASDK --> U kunt een abonnement met DNS-Zone resources of de routetabel-resources die zijn gekoppeld aan het niet verwijderen. Als u wilt het abonnement is verwijderd, moet u eerst de DNS-Zone en routetabel resources uit het tenantabonnement verwijderen.


- <!-- 1902460 - IS ASDK --> Azure Stack biedt ondersteuning voor een enkel *lokale netwerkgateway* per IP-adres. Dit geldt voor alle tenant-abonnementen. Nadat het maken van de eerste lokale gateway netwerkverbinding, volgende pogingen tot het maken van een resource van een lokale netwerkgateway met hetzelfde IP-adres worden geblokkeerd.

- <!-- 16309153 - IS ASDK --> In een Virtueelnetwerk dat is gemaakt met een DNS-Server-instelling van *automatische*, wijzigen naar een aangepaste DNS-Server mislukt. De bijgewerkte instellingen worden niet gepusht naar virtuele machines in dit Vnet.

- <!-- TBD - IS ASDK --> Azure Stack biedt geen ondersteuning voor extra netwerkinterfaces toevoegen aan een VM-exemplaar, nadat de virtuele machine is geïmplementeerd. Als de virtuele machine meer dan één netwerkinterface vereist, moeten ze worden gedefinieerd tijdens de implementatie.

- <!-- 2096388 IS --> U kunt de beheerportal niet gebruiken voor het bijwerken van regels voor een netwerkbeveiligingsgroep.

    Tijdelijke oplossing voor App Service: als u extern bureaublad naar de Controller-exemplaren wilt, wijzigt u de beveiligingsregels binnen de netwerkbeveiligingsgroepen met PowerShell.  Hieronder vindt u voorbeelden van hoe u *toestaan*, en herstel vervolgens de configuratie van *weigeren*:  

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

- <!-- TBD - IS --> Alleen de resourceprovider wordt ondersteund voor het maken van items op servers die SQL-host of MySQL. Items die zijn gemaakt op een host-server die niet zijn gemaakt door de resourceprovider kunnen leiden tot een niet-overeenkomende staat.  

- <!-- IS, ASDK --> Speciale tekens, inclusief spaties en perioden, worden niet ondersteund in de **familie** of **laag** namen wanneer u een SKU voor de SQL- en MySQL-resourceproviders.


> [!NOTE]  
> <!-- TBD - IS --> Nadat u naar Azure Stack 1805 bijwerken, kunt u echter ook doorgaan met de SQL- en MySQL-resourceproviders die u eerder hebt geïmplementeerd.  U wordt aangeraden dat u SQL- en MySQL bijwerken wanneer er een nieuwe versie beschikbaar. Net als Azure Stack, updates toepassen op SQL- en MySQL resourceproviders sequentieel worden verwerkt. Als u versie 1803, versie 1804 eerst toe te passen en vervolgens bijwerken naar 1805.      
>   
> De installatie van update 1805 heeft geen invloed op het huidige gebruik van SQL- of MySQL resourceproviders door uw gebruikers.
> Ongeacht de versie van de resourceproviders die u gebruikt, de gegevens van uw gebruikers in hun databases is niet aangeraakt en toegankelijk blijft.    



### <a name="app-service"></a>App Service
- <!-- 2352906 - IS ASDK --> Gebruikers moeten de storage resourceprovider registreren voordat ze hun eerste Azure-functie in het abonnement maken.

- <!-- 2489178 - IS ASDK --> Als u wilt de infrastructuur (werknemers, beheer, front-rollen) uitbreiden, moet u PowerShell gebruiken, zoals beschreven in de opmerkingen bij de release voor Compute.

- <!-- TBD - IS ASDK --> App Service kan alleen worden geïmplementeerd in de *standaard providerabonnement* op dit moment. In een toekomstige update App Service wordt geïmplementeerd in de nieuwe *softwarelicentiecontrole abonnement* die is geïntroduceerd in Azure Stack 1804. Wanneer de meting wordt ondersteund voor gebruik, worden alle bestaande implementaties worden gemigreerd naar dit nieuwe abonnementstype.


### <a name="usage"></a>Gebruik  
- <!-- TBD - IS ASDK --> Gebruik openbare IP-adres-gebruiksgegevens meter toont dezelfde *datum/tijd evenement* waarde voor elke record in plaats van de *TimeDate* stempel waarin wordt weergegeven wanneer de record is gemaakt. U kunt deze gegevens op dit moment niet gebruiken om uit te voeren nauwkeurige accounting van gebruik van openbare IP-adres.


<!-- #### Identity -->
<!-- #### Marketplace -->


## <a name="download-the-update"></a>De update downloaden
U kunt de Azure Stack 1805 update-pakket van downloaden [hier](https://aka.ms/azurestackupdatedownload).


## <a name="see-also"></a>Zie ook
- Voor het gebruik van de bevoegde eindpunt (PEP) om te controleren en hervatten van updates, Zie [-updates in Azure Stack met behulp van het eindpunt van de bevoegde controleren](azure-stack-monitor-update.md).
- Zie voor een overzicht van de update management in Azure Stack, [-updates beheren in Azure Stack-overzicht](azure-stack-updates.md).
- Zie voor meer informatie over het toepassen van updates met Azure Stack [toepassen van updates in Azure Stack](azure-stack-apply-updates.md).
