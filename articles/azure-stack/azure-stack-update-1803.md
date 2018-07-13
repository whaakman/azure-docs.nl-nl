---
title: Azure Stack 1803 Update | Microsoft Docs
description: Meer informatie over wat er in de update 1803 voor Azure Stack geïntegreerde systemen, de bekende problemen en het downloaden van de update.
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
ms.date: 07/11/2018
ms.author: brenduns
ms.reviewer: justini
ms.openlocfilehash: 11430a0d194a722c0c0520c936db3c08b1a6b863
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38989568"
---
# <a name="azure-stack-1803-update"></a>Azure Stack-1803 update

*Is van toepassing op: Azure Stack-geïntegreerde systemen*

Dit artikel worden de verbeteringen en in het updatepakket met 1803, bekende problemen voor deze release en het downloaden van de update worden opgelost. Bekende problemen zijn onderverdeeld in problemen direct verband houden met het updateproces en problemen met de build (na de installatie).

> [!IMPORTANT]        
> Dit pakket is alleen voor geïntegreerde Azure Stack-systemen. Dit pakket niet van toepassing op de Azure Stack Development Kit.

## <a name="build-reference"></a>Naslaginformatie over bouwen    
De Azure Stack 1803 update build-nummer **20180329.1**.


## <a name="before-you-begin"></a>Voordat u begint    
> [!IMPORTANT]    
> Probeer niet voor het maken van virtuele machines tijdens de installatie van deze update. Zie voor meer informatie over het beheren van updates [-updates beheren in Azure Stack-overzicht](azure-stack-updates.md#plan-for-updates).


### <a name="prerequisites"></a>Vereisten
- De Azure-Stack installeren [Update 1802](azure-stack-update-1802.md) voordat u de Azure Stack 1803 update toepassen.   

- Installeer **AzS Hotfix – 1.0.180312.1-bouwen 20180222.2** voordat u de Azure Stack 1803 update toepassen. Deze hotfix Windows Defender-updates en is beschikbaar wanneer u updates voor Azure Stack downloaden.

  Installeer de hotfix, volgt u de normale procedures voor [installeren van updates voor Azure Stack](azure-stack-apply-updates.md). De naam van de update wordt weergegeven als **AzS Hotfix – 1.0.180312.1**, en bevat de volgende bestanden: 
    - PUPackageHotFix_20180222.2-1.exe
    - PUPackageHotFix_20180222.2-1.bin
    - Metadata.xml

  Nadat u hebt deze bestanden naar een opslagaccount en container te uploaden, moet u de installatie uitvoeren vanaf de tegel Update in de beheerportal. 
  
  In tegenstelling tot de updates voor Azure Stack verandert u deze update installeert niet de versie van Azure Stack. Bekijk de lijst om te bevestigen dat deze update is geïnstalleerd, **geïnstalleerde updates**.



### <a name="new-features"></a>Nieuwe functies 
Deze update bevat de volgende verbeteringen en oplossingen voor Azure Stack.

- **Azure Stack-geheimen bijwerken** - (Accounts en certificaten). Zie voor meer informatie over het beheren van geheimen [draaien geheimen in Azure Stack](azure-stack-rotate-secrets.md). 

- <!-- 1914853 --> **Automatische omleiden naar HTTPS** wanneer u HTTP gebruikt voor toegang tot de beheerder en gebruiker portals. Deze verbetering is gemaakt op basis van [UserVoice](https://feedback.azure.com/forums/344565-azure-stack/suggestions/32205385-it-would-be-great-if-there-was-a-automatic-redirec) feedback voor Azure Stack. 

- <!-- 2202621  --> **Toegang tot de Marketplace** – u kunt nu de Azure Stack Marketplace openen met behulp van de [+ nieuw](https://ms.portal.azure.com/#create/hub) optie uit binnen de beheerder en gebruiker portals de dezelfde manier als in de Azure-portals.
 
- <!-- 2202621 --> **Azure Monitor** -Azure Stack toegevoegd [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor) naar de beheerder en gebruiker portals. Dit omvat nieuwe explorers voor metrische gegevens en activiteitenlogboeken. Voor toegang tot deze Azure-Monitor van externe netwerken, poort **13012** moet worden geopend in de firewall-configuraties. Zie voor meer informatie over de poorten die zijn vereist voor Azure Stack [datacenter-integratie van Azure Stack - eindpunten publiceren](azure-stack-integrate-endpoints.md).

   Ook als onderdeel hiervan wijzigt, onder **meer services**, *auditlogboeken* wordt nu weergegeven als *activiteitenlogboeken*. De functionaliteit is nu consistent zijn met de Azure-portal. 

- <!-- 1664791 --> **Sparse bestanden** : wanneer u een nieuwe afbeelding aan Azure Stack toevoegen of toevoegen van een installatiekopie van een via marketplace syndication, de installatiekopie wordt geconverteerd naar een sparse-bestand. Installatiekopieën die zijn toegevoegd voordat u met behulp van Azure Stack, versie 1803 kunnen niet worden geconverteerd. In plaats daarvan moet u de marketplace-syndicatie opnieuw indienen deze installatiekopieën om te profiteren van deze functie gebruiken. 
 
   Sparse bestanden zijn een efficiënte gebruikt voor het opslaggebruik voor de ruimte verminderen en verbeteren van i/o-bestandsindeling.  Zie voor meer informatie, [Fsutil sparse](https://docs.microsoft.com/windows-server/administration/windows-commands/fsutil-sparse) voor Windows Server. 

### <a name="fixed-issues"></a>Opgeloste problemen

- <!-- 1739988 --> Interne Load Balancing (ILB) nu verwerkt correct MAC-adressen voor back-end VM's, waardoor ILB om te verwijderen van pakketten naar de back-endnetwerk bij het gebruik van Linux-exemplaren in de back-endnetwerk. ILB werkt goed samen met Windows-exemplaren in de back-end-netwerk. 

- <!-- 1805496 --> Een probleem waarbij VPN-verbindingen tussen Azure Stack zou worden verbroken vanwege een Azure-Stack maken met verschillende instellingen voor de IKE-beleid dan Azure. De waarden voor SALifetime (tijd) en SALiftetime (Bytes) zijn niet compatibel is met Azure en in 1803 zodat deze overeenkomen met de Azure-instellingen zijn gewijzigd. De waarde voor SALifetime (seconden) voordat u 1803 is resulteren in 14.400 en nu wijzigingen aan 27.000 in 1803. De waarde voor SALifetime (Bytes) vóór 1803 is 819,200 en wijzigingen aan 33,553,408 in 1803.

- <!-- 2209262 --> Het IP-probleem waarbij VPN-verbindingen eerder zichtbaar in de portal is; in- of bij het omschakelen van doorsturen via IP heeft echter geen effect. De functie is ingeschakeld standaard en de mogelijkheid om te wijzigen van dit nog niet ondersteund.  Het besturingselement is verwijderd uit de portal. 

- <!-- 1766332 --> Azure Stack biedt geen ondersteuning voor beleid op basis van VPN-Gateways, zelfs als de optie wordt weergegeven in de Portal.  De optie is verwijderd uit de Portal. 

- <!-- 1868283 --> Azure Stack wordt nu voorkomen dat vergroten of verkleinen van een virtuele machine die wordt gemaakt met dynamische schijven. 

- <!-- 1756324 --> Gebruiksgegevens voor virtuele machines zijn nu gescheiden elk uur. Dit komt overeen met Azure. 

- <!--  2253274 --> Het probleem waar in de portals beheerder en gebruiker het tabblad instellingen voor vNet subnetten niet kan worden geladen. PowerShell gebruiken als tijdelijke oplossing, en de [Get-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig?view=azurermps-5.5.0) cmdlet weergeven en beheren van deze informatie.

- Wanneer u een virtuele machine, het bericht maakt *kan niet worden weergegeven prijzen* niet meer wordt weergegeven bij het kiezen van een grootte voor de VM-grootte.

- Verschillende oplossingen voor prestaties, stabiliteit, beveiliging en het besturingssysteem dat wordt gebruikt door Azure Stack.


### <a name="changes"></a>Wijzigingen
- De manier om te wijzigen van de status van een nieuwe aanbieding van *persoonlijke* naar *openbare* of *uit bedrijf genomen* is gewijzigd. Zie voor meer informatie, [maken van een aanbieding](azure-stack-create-offer.md).


### <a name="known-issues-with-the-update-process"></a>Bekende problemen met het updateproces    
<!-- 2328416 --> Tijdens de installatie van de update 1803, kunnen er uitvaltijd van de blob-service en de interne services die gebruikmaken van blob-service. Het gaat hierbij om bepaalde bewerkingen van de virtuele machine. Hierdoor uitvaltijd kunnen problemen ontstaan van tenant bewerkingen of waarschuwingen van services die geen toegang gegevens tot. Dit probleem wordt opgelost zelf wanneer de update is voltooid. 



### <a name="post-update-steps"></a>Stappen na het bijwerken
- Na de installatie van 1803, eventuele van toepassing Hotfixes te installeren. Raadpleeg voor meer informatie de volgende knowledge base-artikelen, evenals onze [beleid onderhoud](azure-stack-servicing-policy.md).

  - [KB 4344115 - Azure Stack-Hotfix 1.0.180427.15](https://support.microsoft.com/help/4344115).

- Nadat u deze update installeert, controleert u uw firewallconfiguratie om ervoor te zorgen [noodzakelijke poorten](azure-stack-integrate-endpoints.md) zijn geopend. Bijvoorbeeld, deze update worden geïntroduceerd *Azure Monitor* waaronder een wijziging van de auditlogboeken voor activiteitenlogboeken. Met deze wijziging, poort 13012 wordt nu gebruikt en moet ook zijn geopend.  


### <a name="known-issues-post-installation"></a>Bekende problemen (na de installatie)
Hieronder vindt u na de installatie bekende problemen voor de build **20180323.2**.

#### <a name="portal"></a>Portal
- <!-- 2332636 - IS -->  Wanneer u AD FS voor uw Azure Stack-identiteitssysteem bij te werken naar deze versie van Azure Stack, de standaardeigenaar van het standaardabonnement provider opnieuw wordt ingesteld in de ingebouwde **CloudAdmin** gebruiker.  
  Tijdelijke oplossing: Gebruiken om op te lossen dit probleem nadat u deze update installeert, stap 3 van de [Trigger automation configureren claims provider vertrouwensrelatie in Azure Stack](azure-stack-integrate-identity.md#trigger-automation-to-configure-claims-provider-trust-in-azure-stack-1) procedure voor het opnieuw instellen van de eigenaar van de provider standaardabonnement.   

- De mogelijkheid [een nieuwe ondersteuningsaanvraag openen in de vervolgkeuzelijst](azure-stack-manage-portals.md#quick-access-to-help-and-support) uit binnen de beheerder van de portal is niet beschikbaar. In plaats daarvan gebruikt u de volgende koppeling:     
    - Gebruik voor Azure Stack geïntegreerde systemen https://aka.ms/newsupportrequest.

- <!-- 2050709 --> In de beheerportal is het niet mogelijk om te bewerken van metrische opslaggegevens voor Blob-service, tabelservice of Queue-service. Wanneer u gaat u naar de opslag, en selecteer vervolgens de blob-, tabel- of tegel voor queue-service, wordt er een nieuwe blade geopend waarin u een grafiek met metrische gegevens voor die service. Als u vervolgens bewerken vanaf de bovenkant van de metrische gegevens grafiek tegel selecteert, wordt de grafiek bewerken-blade geopend maar geen opties voor het bewerken van metrische gegevens weer.

- Het is mogelijk niet mogelijk om te berekenen of opslaan resources weergeven in de beheerdersportal. De oorzaak van dit probleem is een fout opgetreden tijdens de installatie van de update die ervoor zorgt dat de update worden niet correct gerapporteerd als geslaagd. Als dit probleem optreedt, moet u contact op met Microsoft Customer Support Services voor ondersteuning.

- U ziet een leeg dashboard in de portal. Als u wilt herstellen in het dashboard, selecteert u het tandwielpictogram in de rechterbovenhoek van de portal en selecteer vervolgens **standaardinstellingen herstellen**.

- Verwijderen van zwevende resources leidt van gebruiker-abonnementen. Als tijdelijke oplossing, eerst Gebruikersbronnen of de hele resourcegroep verwijderen en verwijder vervolgens gebruikersabonnementen.

- U kunt machtigingen aan uw abonnement met behulp van de Azure Stack-portals niet weergeven. Als tijdelijke oplossing, moet u PowerShell gebruiken om machtigingen te controleren.

- In het dashboard van de beheerportal, wordt de tegel Update mislukt om meer informatie over updates weer te geven. U lost dit probleem, klikt u op de tegel te vernieuwen.

- In de beheerportal ziet u mogelijk een kritieke waarschuwing voor de *Microsoft.Update.Admin* onderdeel. De naam van de waarschuwing, beschrijving en herstel alle als weergegeven:  
    - *Fout - sjabloon voor FaultType ResourceProviderTimeout ontbreekt.*

  Deze waarschuwing kan veilig worden genegeerd. 


#### <a name="health-and-monitoring"></a>Status en bewaking
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

  Beide waarschuwingen kunnen worden genegeerd. Ze worden automatisch gesloten na verloop van tijd.  


#### <a name="marketplace"></a>Marketplace
- Gebruikers de volledige marketplace zonder abonnement kunnen zoeken en met beheerdersrechten objecten, zoals plannen en aanbiedingen kunnen zien. Deze items zijn niet functioneel voor gebruikers.



#### <a name="compute"></a>Compute
- Instellingen voor vergroten/verkleinen voor schaalsets voor virtuele machines zijn niet beschikbaar in de portal. Als tijdelijke oplossing, kunt u [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Vanwege de verschillen tussen versies van PowerShell, moet u de `-Name` parameter in plaats van `-VMScaleSetName`.

- Bij het maken van een beschikbaarheidsset in de portal door te gaan naar **nieuw** > **Compute** > **beschikbaarheidsset**, kunt u alleen maken een beschikbaarheidsset met een foutdomein en updatedomein van 1. Als tijdelijke oplossing, bij het maken van een nieuwe virtuele machine, maak de beschikbaarheidsset met behulp van PowerShell, CLI, of rechtstreeks vanuit de portal.

- Wanneer u virtuele machines op de gebruikersportal van Azure Stack maakt, wordt een onjuist aantal gegevensschijven die aan een virtuele machine uit de D-serie kunt koppelen door de portal weergegeven. Alle ondersteunde D-serie VM's kan zo veel gegevensschijven bevatten als de Azure-configuratie.

- Wanneer een VM-installatiekopie niet kan worden gemaakt, kan een mislukte-item dat u niet verwijderen worden toegevoegd aan de VM-installatiekopieën compute-blade.

  Als tijdelijke oplossing, kunt u een nieuwe VM-installatiekopie maken met een dummy VHD dat kan worden gemaakt via Hyper-V (New-VHD-pad C:\dummy.vhd-vaste - SizeBytes 1 GB). Dit proces los het probleem dat verhindert dat het mislukte item wordt verwijderd. Vervolgens, 15 minuten na het maken van de installatiekopie van het pop, u kunt met succes verwijderen.

  U kunt vervolgens redownload van de VM-installatiekopie die eerder is mislukt.

-  Als een uitbreiding op een VM-implementatie de inrichting te lang duurt, laat gebruikers de time-out van de inrichting in plaats van bij stoppen van de procedure voor de toewijzing ongedaan of verwijder de virtuele machine.  

- <!-- 1662991 --> Diagnostische gegevens over Linux-VM wordt niet ondersteund in Azure Stack. Wanneer u een Linux-VM met VM diagnostics is ingeschakeld implementeren, wordt de implementatie mislukt. De implementatie mislukt ook als u de Linux-VM eenvoudige metrische gegevens via diagnostische instellingen inschakelen.  


#### <a name="networking"></a>Netwerken
- Nadat een virtuele machine is gemaakt en die zijn gekoppeld aan een openbaar IP-adres, kunt u deze VM op basis van IP-adres kan niet loskoppelen. Ontkoppeling lijkt te werken, maar het eerder toegewezen openbare IP-adres blijft gekoppeld aan de oorspronkelijke virtuele machine.

  Op dit moment moet u alleen nieuwe openbare IP-adressen voor nieuwe virtuele machines die u maakt.

  Dit gebeurt zelfs als u het IP-adres aan een nieuwe virtuele machine opnieuw toewijzen (vaak aangeduid als een *wisselen van VIP*). Alle toekomstige probeert verbinding maken via dit resultaat van de IP-adres in een verbinding naar de oorspronkelijk gekoppelde virtuele machine en niet naar het nieuwe certificaat.



- Azure Stack biedt ondersteuning voor een enkel *lokale netwerkgateway* per IP-adres. Dit geldt voor alle tenant-abonnementen. Nadat het maken van de eerste lokale gateway netwerkverbinding, volgende pogingen tot het maken van een resource van een lokale netwerkgateway met hetzelfde IP-adres worden geblokkeerd.

- In een Virtueelnetwerk dat is gemaakt met een DNS-Server-instelling van *automatische*, wijzigen naar een aangepaste DNS-Server mislukt. De bijgewerkte instellingen worden niet gepusht naar virtuele machines in dit Vnet.

- Azure Stack biedt geen ondersteuning voor extra netwerkinterfaces toevoegen aan een VM-exemplaar, nadat de virtuele machine is geïmplementeerd. Als de virtuele machine meer dan één netwerkinterface vereist, moeten ze worden gedefinieerd tijdens de implementatie.

- <!-- 2096388 --> U kunt de beheerportal niet gebruiken voor het bijwerken van regels voor een netwerkbeveiligingsgroep. 

    Tijdelijke oplossing voor App Service: als u extern bureaublad naar de Controller-exemplaren wilt, wijzigt u de beveiligingsregels binnen de netwerkbeveiligingsgroepen met PowerShell.  Hieronder vindt u voorbeelden van hoe u *toestaan*, en herstel vervolgens de configuratie van *weigeren*:  
    
    - *Toestaan:*
 
      ```powershell    
      Add-AzureRmAccount -EnvironmentName AzureStackAdmin
      
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
        
        Add-AzureRmAccount -EnvironmentName AzureStackAdmin
        
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
- Controleer voordat u doorgaat, de belangrijke opmerking in [voordat u begint met](#before-you-begin) in de buurt van het begin van deze opmerkingen bij de release.

- Duurt maximaal één uur voordat gebruikers databases in een nieuwe SQL- of MySQL-implementatie kunnen maken.

- Alleen de resourceprovider wordt ondersteund voor het maken van items op servers die SQL-host of MySQL. Items die zijn gemaakt op een host-server die niet zijn gemaakt door de resourceprovider kunnen leiden tot een niet-overeenkomende staat.  

- <!-- IS, ASDK --> Speciale tekens, inclusief spaties en perioden, worden niet ondersteund in de **familie** naam wanneer u een SKU voor de SQL- en MySQL-resourceproviders.

> [!NOTE]  
> Nadat u naar Azure Stack 1803 bijwerken, kunt u echter ook doorgaan met de SQL- en MySQL-resourceproviders die u eerder hebt geïmplementeerd.  U wordt aangeraden dat u SQL- en MySQL bijwerken wanneer er een nieuwe versie beschikbaar. Net als Azure Stack, updates toepassen op SQL- en MySQL resourceproviders sequentieel worden verwerkt.  Als u versie 1711, versie 1712 en vervolgens 1802 eerst toe te passen en vervolgens bijwerken naar 1803.      
>   
> De installatie van update 1803 heeft geen invloed op het huidige gebruik van SQL- of MySQL resourceproviders door uw gebruikers.
> Ongeacht de versie van de resourceproviders die u gebruikt, de gegevens van uw gebruikers in hun databases is niet aangeraakt en toegankelijk blijft.    



#### <a name="app-service"></a>App Service
- Gebruikers moeten de storage resourceprovider registreren voordat ze hun eerste Azure-functie in het abonnement maken.

- Als u wilt de infrastructuur (werknemers, beheer, front-rollen) uitbreiden, moet u PowerShell gebruiken, zoals beschreven in de opmerkingen bij de release voor Compute.


#### <a name="usage"></a>Gebruik  
- Gebruik openbare IP-adres-gebruiksgegevens meter toont dezelfde *datum/tijd evenement* waarde voor elke record in plaats van de *TimeDate* stempel waarin wordt weergegeven wanneer de record is gemaakt. U kunt deze gegevens op dit moment niet gebruiken om uit te voeren nauwkeurige accounting van gebruik van openbare IP-adres.

<!--
#### Identity
-->



#### <a name="downloading-azure-stack-tools-from-github"></a>Hulpprogramma's voor Azure Stack downloaden vanuit GitHub
- Wanneer u de *invoke-webrequest* PowerShell-cmdlet voor het downloaden van de Azure Stack-hulpprogramma's vanuit Github, wordt er een fout opgetreden:     
    -  *Invoke-webrequest: de aanvraag is afgebroken: kan geen beveiligde SSL/TLS-kanaal maken.*     

  Deze fout treedt op vanwege een recente GitHub ondersteuning afschaffing van de cryptografische standaarden Tlsv1 en Tlsv1.1 (de standaardinstelling voor PowerShell). Zie voor meer informatie, [zwakke cryptografische standaarden verwijdering kennisgeving](https://githubengineering.com/crypto-removal-notice/).

  U lost dit probleem, toevoegen `[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12` naar de bovenkant van het script om af te dwingen de PowerShell-console ondersteuning voor TLSv1.2 gebruik bij het downloaden van GitHub-opslagplaatsen.


## <a name="download-the-update"></a>De update downloaden
U kunt de Azure Stack 1803 update-pakket van downloaden [hier](https://aka.ms/azurestackupdatedownload).


## <a name="see-also"></a>Zie ook
- Voor het gebruik van de bevoegde eindpunt (PEP) om te controleren en hervatten van updates, Zie [-updates in Azure Stack met behulp van het eindpunt van de bevoegde controleren](azure-stack-monitor-update.md).
- Zie voor een overzicht van de update management in Azure Stack, [-updates beheren in Azure Stack-overzicht](azure-stack-updates.md).
- Zie voor meer informatie over het toepassen van updates met Azure Stack [toepassen van updates in Azure Stack](azure-stack-apply-updates.md).
