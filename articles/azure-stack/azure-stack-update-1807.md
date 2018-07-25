---
title: Azure Stack 1807 Update | Microsoft Docs
description: Meer informatie over wat is er nieuw in de update 1807 voor geïntegreerde Azure Stack-systemen, met inbegrip van de bekende problemen en waar u de update te downloaden.
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
ms.date: 07/24/2018
ms.author: brenduns
ms.reviewer: justini
ms.openlocfilehash: fe37a62d364d53d03a232b6fdc41e38a9ae2e30f
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228013"
---
# <a name="azure-stack-1807-update"></a>Azure Stack 1807 update

*Is van toepassing op: Azure Stack-geïntegreerde systemen*

Dit artikel wordt de inhoud van het updatepakket 1807 beschreven. Deze update bevat verbeteringen, correcties en bekende problemen voor deze versie van Azure Stack en waar u de update te downloaden. Bekende problemen zijn onderverdeeld in problemen direct verband houden met het updateproces en problemen met de build (na de installatie).

> [!IMPORTANT]        
> Dit pakket is alleen voor geïntegreerde Azure Stack-systemen. Dit pakket niet van toepassing op de Azure Stack Development Kit.

## <a name="build-reference"></a>Naslaginformatie over bouwen    
De Azure Stack 1807 update build-nummer **1.1807.XX.X**.  


### <a name="new-features"></a>Nieuwe functies
Deze update bevat de volgende verbeteringen voor Azure Stack.

- <!-- 1658937 | ASDK, IS --> **Starten van back-ups volgens een vooraf gedefinieerd schema** -als een apparaat, Azure Stack kan nu automatisch activeren infrastructuur back-ups regelmatig om te voorkomen van menselijk handelen. Azure Stack wordt ook automatisch opschonen van de externe share voor back-ups die ouder dan de opgegeven bewaarperiode zijn. 

- <!-- 2496385 | ASDK, IS --> **Toegevoegde tijd in de totale back-uptijd voor gegevensoverdracht.**

-   <!-- 1702130 | ASDK, IS --> **Back-up externe capaciteit wordt nu de juiste capaciteit van de externe share.** (Dit was eerder programmeren tot 10 GB.)

- <!-- 2508488 |  IS   -->   Capaciteit van [extra scale unit knooppunten toe te voegen](azure-stack-add-scale-node.md).



### <a name="fixed-issues"></a>Opgeloste problemen

- <!-- 448955 | IS ASDK --> U kunt nu activiteitenlogboeken voor systemen die zijn geïmplementeerd in een tijdzone UTC + N is opvragen.    

- <!-- 2319627 |  ASDK, IS --> Controle vooraf voor de back-upconfiguratie parameters (pad/gebruikersnaam/wachtwoord/Encryption Key) wordt niet langer onjuiste instellingen ingesteld op de back-upconfiguratie. (Voorheen onjuiste instellingen zijn ingesteld in de back-up en back-up wilt, klikt u vervolgens mislukken wanneer tirggered.)

- <!-- 2215948 |  ASDK, IS --> De lijst met back-up wordt nu vernieuwd wanneer u de back-up handmatig van de externe share verwijderen.

- <!-- 2332636 - IS -->  Update naar deze versie wordt niet meer de standaardeigenaar van het abonnement van de provider standaard teruggezet naar de ingebouwde CloudAdmin gebruiker bij de implementatie met AD FS.

- <!-- 2360715 |  ASDK, IS -->  Bij het instellen van datacenter-integratie, u geen toegang meer tot de AD FS-bestand met metagegevens van een share. Zie voor meer informatie, [AD FS-integratie instellen door op te geven van bestand met federatieve metagegevens](azure-stack-integrate-identity.md#setting-up-ad-fs-integration-by-providing-federation-metadata-file). 

- <!-- 2388980 | ASDK, IS --> We een probleem opgelost dat gebruikers van een bestaande openbare IP-adres toegewezen die was eerder zijn toegewezen aan een netwerkinterface of Load Balancer met een nieuwe netwerkinterface of Load Balancer.  

-   <!--2292271 | ASDK, IS --> We een probleem opgelost waarbij een gewijzigde quotumlimiet is niet van toepassing op bestaande abonnementen.  Nu, wanneer u een quotumlimiet voor een netwerkbron die deel uitmaakt van een aanbieding verhogen en Plan dat is gekoppeld aan een tenantabonnement, de nieuwe limiet van toepassing op de bestaande abonnementen, evenals de nieuwe abonnementen.

- **Verschillende oplossingen** voor prestaties, stabiliteit, beveiliging en het besturingssysteem dat wordt gebruikt door Azure Stack.


<!-- ### Additional releases timed with this update    -->



## <a name="before-you-begin"></a>Voordat u begint    

### <a name="prerequisites"></a>Vereisten
- De Azure-Stack installeren [1805 bijwerken](azure-stack-update-1805.md) voordat u de Azure Stack 1807 update toepassen.  Er is geen update 1806.  

- Voordat u begint met de installatie van update 1807, voert u [Test AzureStack](azure-stack-diagnostic-test.md) om te valideren van de status van uw Azure-Stack en los eventuele operationele problemen gevonden. Ook actieve waarschuwingen bekijken en op te lossen die actie is vereist.

### <a name="known-issues-with-the-update-process"></a>Bekende problemen met het updateproces   
- <!-- 2468613 - IS --> Tijdens de installatie van deze update, ziet u mogelijk waarschuwingen met de titel *fout: sjabloon voor FaultType UserAccounts.New ontbreekt.*  U kunt deze waarschuwingen negeren. Deze waarschuwingen worden automatisch gesloten nadat de installatie van deze update is voltooid.   

- <!-- 2489559 - IS --> Probeer niet voor het maken van virtuele machines tijdens de installatie van deze update. Zie voor meer informatie over het beheren van updates [-updates beheren in Azure Stack-overzicht](azure-stack-updates.md#plan-for-updates).


### <a name="post-update-steps"></a>Stappen na het bijwerken
*Er zijn geen stappen na het bijwerken van update 1807.*

<!-- After the installation of this update, install any applicable Hotfixes. For more information view the following knowledge base articles, as well as our [Servicing Policy](azure-stack-servicing-policy.md).  
 - Link to KB  
 -->

## <a name="known-issues-post-installation"></a>Bekende problemen (na de installatie)
Hier volgen na de installatie bekende problemen voor deze buildversie.

### <a name="portal"></a>Portal  
- <!-- 2551834 - IS, ASDK --> Wanneer u selecteert **overzicht** voor een opslagaccount in de beheerder of de gebruiker-portals, de gegevens uit de *Essentials* deelvenster niet wordt weergegeven.  Het deelvenster Essentials geeft informatie weer over het account, zoals de *resourcegroep*, *locatie*, en *abonnements-ID*.  Andere opties voor overzicht zijn beschikbaar zijn, zoals *Services* en *bewaking*, ook als opties voor *openen in Explorer* of *storage-account verwijderen* .

  Als u de informatie niet beschikbaar is, gebruikt u de [Get-azureRMstorageaccount](https://docs.microsoft.com/powershell/module/azurerm.storage/get-azurermstorageaccount?view=azurermps-6.2.0) PowerShell-cmdlet.

- <!-- 2551834 - IS, ASDK --> Wanneer u selecteert **Tags** voor een opslagaccount in de beheerder of de gebruiker-portals, de gegevens te laden is mislukt en niet wordt weergegeven.  

  Als u de informatie niet beschikbaar is, gebruikt u de [Get-AzureRmTag](https://docs.microsoft.com/powershell/module/azurerm.tags/get-azurermtag?view=azurermps-6.2.0) PowerShell-cmdlet. 

- <!-- TBD - IS ASDK --> Sommige typen administratieve abonnementen zijn niet beschikbaar.  Wanneer u een upgrade uitvoert van Azure Stack in deze versie, worden de twee abonnementstypen zijn [geïntroduceerd in versie 1804](azure-stack-update-1804.md#new-features) zijn niet zichtbaar in de console. Dit is normaal gedrag. Het niet beschikbaar-abonnementstypen zijn *softwarelicentiecontrole abonnement*, en *verbruik abonnement*. Deze abonnementstypen zijn zichtbaar in de nieuwe Azure Stack-omgevingen vanaf versie 1804 maar nog niet klaar voor gebruik. U moet echter ook doorgaan met de *Provider standaard* abonnementstype.  

- <!-- 2403291 - IS ASDK --> Hebt u mogelijk geen gebruik van de horizontale schuifbalk langs de onderkant van de beheerder en gebruiker portals. Als u geen toegang de horizontale schuifbalk tot, blijven gebruiken de breadcrumbs om naar een vorige blade in de portal navigeren zijn door de naam van de blade te selecteren dat u wilt weergeven in de breadcrumb-lijst gevonden aan de bovenkant van de portal.
  ![Breadcrumb](media/azure-stack-update-1804/breadcrumb.png)

- <!-- TBD - IS --> Het is mogelijk niet mogelijk om te berekenen of opslaan resources weergeven in de beheerdersportal. De oorzaak van dit probleem is een fout opgetreden tijdens de installatie van de update die ervoor zorgt dat de update worden niet correct gerapporteerd als geslaagd. Als dit probleem optreedt, moet u contact op met Microsoft Customer Support Services voor ondersteuning.

- <!-- TBD - IS --> U ziet een leeg dashboard in de portal. Als u wilt herstellen in het dashboard, selecteert u het tandwielpictogram in de rechterbovenhoek van de portal en selecteer vervolgens **standaardinstellingen herstellen**.

- <!-- TBD - IS ASDK --> Verwijderen van zwevende resources leidt van gebruiker-abonnementen. Als tijdelijke oplossing, eerst Gebruikersbronnen of de hele resourcegroep verwijderen en verwijder vervolgens gebruikersabonnementen.

- <!-- TBD - IS ASDK --> U kunt machtigingen aan uw abonnement met behulp van de Azure Stack-portals niet weergeven. Als tijdelijke oplossing, moet u PowerShell gebruiken om machtigingen te controleren.


### <a name="health-and-monitoring"></a>Status en bewaking
- <!-- 1264761 - IS ASDK -->  Mogelijk ziet u waarschuwingen voor de *Health controller* onderdeel waarvoor u de volgende gegevens:  

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

  Beide waarschuwingen kunnen worden genegeerd en wordt automatisch gesloten na verloop van tijd.  

- <!-- 2368581 - IS. ASDK --> Azure Stack-operators, als u een waarschuwing voor een beperkte hoeveelheid geheugen ontvangt en virtuele machines van tenants niet te implementeren met een *fout bij het maken van infrastructuur-VM*, is het mogelijk dat de Azure Stack-stempel heeft te weinig geheugen beschikbaar. Gebruik de [Azure Stack Capacity Planner](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) naar het beste informatie over de beschikbare capaciteit voor uw workloads.


### <a name="compute"></a>Compute
- <!-- 2724873 - IS --> Bij het gebruik van de PowerShell-cmdlets **Start AzsScaleUnitNode** of **Stop-AzsScaleunitNode** voor het beheren van schaaleenheden, de eerste poging om te starten of stoppen van de schaaleenheid kan mislukken. Als de cmdlet voor de eerste keer uitvoert mislukt, wordt de cmdlet nogmaals uitvoeren. De tweede uitvoering moet slagen om de bewerking te voltooien. 

- <!-- 2494144 - IS, ASDK --> Bij het selecteren van een VM-grootte voor de implementatie van een virtuele machine, sommige F-serie VM-grootten zijn niet zichtbaar als onderdeel van de grootte selector bij het maken van een virtuele machine. De volgende VM-grootten worden niet weergegeven in de kiezer: *F8s_v2*, *F16s_v2*, *F32s_v2*, en *F64s_v2*.  
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

  U kunt vervolgens opnieuw proberen het downloaden van de VM-installatiekopie die eerder is mislukt.

- <!-- TBD - IS ASDK --> Als een uitbreiding op een VM-implementatie de inrichting te lang duurt, laat gebruikers de time-out van de inrichting in plaats van bij stoppen van de procedure voor de toewijzing ongedaan of verwijder de virtuele machine.  

- <!-- 1662991 IS ASDK --> Diagnostische gegevens over Linux-VM wordt niet ondersteund in Azure Stack. Wanneer u een Linux-VM met VM diagnostics is ingeschakeld implementeren, wordt de implementatie mislukt. De implementatie mislukt ook als u de Linux-VM eenvoudige metrische gegevens via diagnostische instellingen inschakelen.  


### <a name="networking"></a>Netwerken  

- <!-- 1766332 - IS ASDK --> Onder **netwerken**, als u klikt op **VPN-Gateway maken** voor het instellen van een VPN-verbinding, **op basis van beleid** wordt vermeld als een VPN-type. Selecteer deze optie niet. Alleen de **Route op basis van** optie wordt ondersteund in Azure Stack.

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
> <!-- TBD - IS --> Nadat u hebt bijgewerkt naar deze versie van Azure Stack, kunt u echter ook doorgaan met de SQL- en MySQL-resourceproviders die u eerder hebt geïmplementeerd.  U wordt aangeraden dat u SQL- en MySQL bijwerken wanneer er een nieuwe versie beschikbaar. Net als Azure Stack, updates toepassen op SQL- en MySQL resourceproviders sequentieel worden verwerkt. Als u versie 1804, versie 1805 eerst toe te passen en vervolgens bijwerken naar 1807.      
>   
> De installatie van deze update heeft geen invloed op het huidige gebruik van SQL- of MySQL resourceproviders door uw gebruikers.
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
U kunt de Azure Stack 1807 update-pakket van downloaden [hier](https://aka.ms/azurestackupdatedownload).


## <a name="see-also"></a>Zie ook
- Voor het gebruik van de bevoegde eindpunt (PEP) om te controleren en hervatten van updates, Zie [-updates in Azure Stack met behulp van het eindpunt van de bevoegde controleren](azure-stack-monitor-update.md).
- Zie voor een overzicht van de update management in Azure Stack, [-updates beheren in Azure Stack-overzicht](azure-stack-updates.md).
- Zie voor meer informatie over het toepassen van updates met Azure Stack [toepassen van updates in Azure Stack](azure-stack-apply-updates.md).
