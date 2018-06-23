---
title: Azure Stack 1803 Update | Microsoft Docs
description: Meer informatie over wat er in de update 1803 voor Azure-Stack geïntegreerd systemen, de bekende problemen en waar u de update te downloaden.
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
ms.date: 06/22/2018
ms.author: brenduns
ms.reviewer: justini
ms.openlocfilehash: a74e77f84aa70519015a589cbc6e7478c0c41592
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2018
ms.locfileid: "36318806"
---
# <a name="azure-stack-1803-update"></a>Azure-Stack 1803 update

*Van toepassing op: Azure Stack geïntegreerd systemen*

In dit artikel worden de verbeteringen beschreven en oplossingen in het updatepakket met 1803, bekende problemen voor deze release en waar u de update te downloaden. Bekende problemen worden onderverdeeld in problemen met het rechtstreeks verband houden met het updateproces kan controleren en problemen met de build (na de installatie).

> [!IMPORTANT]        
> Dit updatepakket is alleen voor Azure-Stack geïntegreerd systemen. Pas dit pakket niet naar de Azure-Stack Development Kit.

## <a name="build-reference"></a>Verwijzing bouwen    
Het buildnummer van Azure Stack 1803 update **20180329.1**.


## <a name="before-you-begin"></a>Voordat u begint    
> [!IMPORTANT]    
> Probeer niet voor het maken van virtuele machines tijdens de installatie van deze update. Zie voor meer informatie over het beheren van updates, [beheren van updates in de Azure-Stack overzicht](azure-stack-updates.md#plan-for-updates).


### <a name="prerequisites"></a>Vereisten
- Installeer de Azure-Stack [1802 bijwerken](azure-stack-update-1802.md) voordat u de Azure-Stack 1803 update toepassen.   

- Installeer **AzS Hotfix – 1.0.180312.1-bouwen 20180222.2** voordat u de Azure-Stack 1803 update toepassen. Deze hotfix Windows Defender-updates en is beschikbaar wanneer u updates voor Azure-Stack downloaden.

  U kunt de hotfix installeren de normale procedures volgen voor [installeren van updates voor Azure-Stack](azure-stack-apply-updates.md). De naam van de update wordt weergegeven als **AzS Hotfix – 1.0.180312.1**, en bevat de volgende bestanden: 
    - PUPackageHotFix_20180222.2-1.exe
    - PUPackageHotFix_20180222.2-1.bin
    - Metadata.xml

  Na het uploaden van uitvoeren deze bestanden naar een opslagaccount en container, de installatie van de Update-tegel in het beheerportal. 
  
  In tegenstelling tot updates naar Azure Stack verandert u deze update installeert niet de versie van Azure-Stack. Bevestig deze update is geïnstalleerd door de lijst weergeven met **geïnstalleerde updates**.



### <a name="new-features"></a>Nieuwe functies 
Deze update bevat de volgende verbeteringen en oplossingen voor Azure-Stack.

- **Bijwerken van de Azure-Stack geheimen** - (Accounts en -certificaten). Zie voor meer informatie over het beheren van geheimen [draaien geheimen in Azure-Stack](azure-stack-rotate-secrets.md). 

- <!-- 1914853 --> **Automatische omleiden naar HTTPS** wanneer u HTTP gebruikt voor toegang tot de beheerder en gebruiker portals. Dankzij deze verbetering is gemaakt op basis van [UserVoice](https://feedback.azure.com/forums/344565-azure-stack/suggestions/32205385-it-would-be-great-if-there-was-a-automatic-redirec) feedback voor Azure-Stack. 

- <!-- 2202621  --> **Toegang tot de Marketplace** – u kunt nu de Stack Azure Marketplace openen met behulp van de [+ nieuw](https://ms.portal.azure.com/#create/hub) optie uit binnen de beheerder en gebruiker portals dezelfde manier als u doen in de Azure portals.
 
- <!-- 2202621 --> **Monitor voor Azure** -Azure-Stack voegt [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor) naar de beheerder en gebruiker portals. Dit omvat nieuwe explorers voor metrische gegevens en activiteit Logboeken. Poort voor toegang tot deze Monitor Azure externe netwerken, **13012** moeten worden geopend in de firewall-configuraties. Zie voor meer informatie over de poorten die vereist zijn voor Azure-Stack [Stack Azure datacenter integratie - eindpunten publiceren](azure-stack-integrate-endpoints.md).

   Ook als onderdeel hiervan wijzigen, klikt u onder **meer services**, *controlelogboeken* wordt nu weergegeven als *activiteitenlogboeken*. De functionaliteit is nu consistent met de Azure-portal. 

- <!-- 1664791 --> **Sparse bestanden** : wanneer u een nieuwe installatiekopie aan Azure-Stack toevoegen of toevoegen van een installatiekopie via de marketplace-syndicatie, de installatiekopie wordt geconverteerd naar een sparse-bestand. Installatiekopieën die zijn toegevoegd voordat u met Azure-Stack versie 1803 kunnen niet worden geconverteerd. In plaats daarvan moet u marketplace syndication opnieuw te verzenden die afbeeldingen om te profiteren van deze functie. 
 
   Sparse bestanden zijn een efficiënte bestandsindeling gebruikt voor het gebruik van opslag ruimte beperken en verbeteren van i/o.  Zie voor meer informatie [Fsutil sparse](https://docs.microsoft.com/windows-server/administration/windows-commands/fsutil-sparse) voor Windows Server. 

### <a name="fixed-issues"></a>Opgeloste problemen

- <!-- 1739988 --> Interne Load Balancing (ILB) nu verwerkt correct MAC-adressen voor back-end virtuele machines, waardoor ILB verwijderen van pakketten naar de back-endnetwerk wanneer u Linux, alle exemplaren in de back-end-netwerk. ILB werkt goed samen met Windows-exemplaren op de back-endnetwerk. 

- <!-- 1805496 --> Een probleem waarbij VPN-verbindingen tussen Azure Stack zou worden verbroken vanwege een Azure-Stack met behulp van verschillende instellingen voor het beleid IKE dan Azure. De waarden voor SALifetime (tijd) en SALiftetime (Bytes) zijn niet compatibel met Azure en in 1803 overeenkomen met de Azure-instellingen zijn gewijzigd. De waarde voor SALifetime (seconden) voordat 1803 heeft 14.400 en nu wijzigingen aan 27.000 1803. De waarde voor SALifetime (Bytes) voordat 1803 is 819,200 en wijzigingen aan 33,553,408 in 1803.

- <!-- 2209262 --> Het IP-probleem waarbij de VPN-verbindingen eerder zichtbaar in de portal is; inschakelen of uitschakelen van doorsturen via IP heeft echter geen effect. De functie is ingeschakeld standaard en de mogelijkheid om te wijzigen dat dit nog niet ondersteund.  Het besturingselement is verwijderd uit de portal. 

- <!-- 1766332 --> Azure-Stack biedt geen ondersteuning voor beleid gebaseerd VPN-Gateways, zelfs als de optie wordt weergegeven in de Portal.  De optie is verwijderd uit de Portal. 

- <!-- 1868283 --> Azure-Stack is nu voorkomt u dat de vergroten of verkleinen van een virtuele machine die wordt gemaakt met dynamische schijven. 

- <!-- 1756324 --> Gebruiksgegevens voor virtuele machines zijn nu gescheiden elk uur. Dit komt overeen met Azure. 

- <!--  2253274 --> Het probleem waar in de portals beheerder en gebruiker de blade instellingen voor vNet subnetten niet kan worden geladen. PowerShell gebruiken als een oplossing en de [Get-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig?view=azurermps-5.5.0) cmdlet weergeven en beheren van deze informatie.

- Wanneer u een virtuele machine, het bericht maakt *kan niet worden weergegeven prijzen* niet langer weergegeven bij het kiezen van een grootte voor de VM-grootte.

- Verschillende oplossingen voor prestaties, stabiliteit, beveiliging en het besturingssysteem dat wordt gebruikt door de Azure-Stack.


### <a name="changes"></a>Wijzigingen
- De manier wijzigen van de status van een nieuwe aanbieding van *persoonlijke* naar *openbare* of *buiten gebruik gestelde* is gewijzigd. Zie voor meer informatie [maken van een aanbieding](azure-stack-create-offer.md).


### <a name="known-issues-with-the-update-process"></a>Bekende problemen met het updateproces kan controleren    
<!-- 2328416 --> Tijdens de installatie van de update 1803, kunnen er uitvaltijd van de blob-service en interne services die gebruikmaken van blob-service. Het gaat hierbij om bepaalde bewerkingen van de virtuele machine. Uitvaltijd kan hierdoor mislukken van de tenant bewerkingen of waarschuwingen van de services die geen toegang gegevens tot. Dit probleem lost zichzelf zodra de update voor de installatie is voltooid. 



### <a name="post-update-steps"></a>Stappen na het bijwerken
- Na de installatie van 1803 toepasselijke Hotfixes te installeren. Raadpleeg voor meer informatie de volgende knowledge base-artikelen, evenals onze [beleid onderhoud](azure-stack-servicing-policy.md).

  - [KB 4341390 - Stack van Azure Hotfix 1.0.180424.12](https://support.microsoft.com/en-us/help/4341390).

- Nadat deze update is geïnstalleerd, Controleer de firewallconfiguratie van uw om ervoor te zorgen [nodig poorten](azure-stack-integrate-endpoints.md) zijn geopend. Bijvoorbeeld: met deze update geïntroduceerd *Azure Monitor* waaronder een wijziging van de controlelogboeken in de logboeken van de activiteit. Met deze wijziging wordt poort 13012 wordt nu gebruikt en moet ook zijn geopend.  


### <a name="known-issues-post-installation"></a>Bekende problemen (na de installatie)
Hieronder vindt u na de installatie bekende problemen voor de build **20180323.2**.

#### <a name="portal"></a>Portal
- <!-- 2332636 - IS -->  Wanneer u AD FS voor uw Azure-Stack identiteitssysteem en de update in deze versie van Azure-Stack, de standaardeigenaar van het abonnement van de provider standaard ingesteld op de ingebouwde **CloudAdmin** gebruiker.  
  Tijdelijke oplossing: Los dit probleem nadat u deze update installeert, gebruikt u stap 3 van de [Trigger automation voor het configureren van een claimprovider in Azure-Stack](azure-stack-integrate-identity.md#trigger-automation-to-configure-claims-provider-trust-in-azure-stack-1) procedure opnieuw instellen van de eigenaar van het standaard provider-abonnement.   

- De mogelijkheid [een nieuwe ondersteuningsaanvraag openen vanuit de vervolgkeuzelijst](azure-stack-manage-portals.md#quick-access-to-help-and-support) uit binnen de beheerder van de portal is niet beschikbaar. Gebruik in plaats daarvan de volgende koppeling:     
    - Gebruik voor Azure-Stack geïntegreerde systemen https://aka.ms/newsupportrequest.

- <!-- 2050709 --> In het beheerportal is het niet mogelijk te bewerken opslag metrische gegevens voor Blob-service, service tabel of Queue-service. Wanneer u gaat u naar de opslag en selecteer vervolgens de blob, table of wachtrij service tegel klikt, wordt een nieuwe blade geopend met een grafiek metrische gegevens voor de service. Als u vervolgens bewerken vanaf de bovenkant van de metrische gegevens grafiek tegel selecteert, wordt de blade grafiek bewerken wordt geopend maar geen opties voor het bewerken van metrische gegevens weer.

- Het is niet mogelijk om compute of opslag resources in de beheerdersportal weer te geven. De oorzaak van dit probleem is een fout opgetreden tijdens de installatie van de update zorgt ervoor dat de update niet correct worden gerapporteerd als geslaagd. Als dit probleem optreedt, moet u contact op met Microsoft Customer Support Services voor ondersteuning.

- U ziet een lege dashboard in de portal. Als u wilt herstellen het dashboard, selecteert u het pictogram tandwielpictogram in de rechterbovenhoek van de portal en selecteer vervolgens **standaardinstellingen herstellen**.

- Gebruiker abonnementen resulteert in een zwevende resources te verwijderen. Als tijdelijke oplossing Gebruikersbronnen of de hele resourcegroep eerst te verwijderen en verwijder vervolgens de gebruikersabonnementen.

- U kunt machtigingen aan uw abonnement met behulp van de portals Azure Stack niet weergeven. Als tijdelijke oplossing PowerShell te gebruiken om machtigingen te controleren.

- In het dashboard van de beheerportal mislukt de Update-tegel om meer informatie over updates weer te geven. U lost dit probleem, klikt u op de tegel te vernieuwen.

- In het beheerportal, ziet u mogelijk een kritieke waarschuwing voor de *Microsoft.Update.Admin* onderdeel. De naam van waarschuwing, beschrijving en herstel alle als weergegeven:  
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


#### <a name="marketplace"></a>Marketplace
- Gebruikers de volledige marketplace zonder een abonnement kunnen bladeren en beheeritems zoals plannen en aanbiedingen kunnen zien. Deze items zijn niet-functioneel voor gebruikers.



#### <a name="compute"></a>Compute
- Instellingen voor virtuele-machineschaalsets schalen zijn niet beschikbaar in de portal. Als tijdelijke oplossing kunt u [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Vanwege verschillen tussen versies van PowerShell, moet u de `-Name` parameter in plaats van `-VMScaleSetName`.

- Bij het maken van een beschikbaarheidsset in de portal door te gaan naar **nieuw** > **Compute** > **beschikbaarheidsset**, kunt u alleen maken een beschikbaarheidsset met een domein met fouten en het updatedomein van 1. Als tijdelijke oplossing bij het maken van een nieuwe virtuele machine, maakt u de beschikbaarheidsset met behulp van PowerShell, CLI of vanuit de portal.

- Wanneer u virtuele machines op de gebruikersportal van Azure-Stack maakt, wordt een onjuist aantal gegevensschijven dat aan een virtuele machine van de D-reeks koppelen kunt weergegeven in de portal. Alle ondersteunde D-reeks VMs aankan zo veel gegevensschijven als de configuratie van de Azure.

- Wanneer een installatiekopie van een virtuele machine niet kan worden gemaakt, kan een mislukte item dat u niet verwijderen, worden toegevoegd aan de VM-installatiekopieën compute-blade.

  Als tijdelijke oplossing door een nieuwe VM-installatiekopie te maken met een dummy VHD die kan worden gemaakt via de Hyper-V (New-VHD-pad C:\dummy.vhd-vaste - SizeBytes 1 GB). Dit proces los het probleem dat verhindert dat het mislukte item wordt verwijderd. Vervolgens kunt 15 minuten na het maken van de installatiekopie van het dummy, u met succes verwijderen.

  U kunt vervolgens redownload van de VM-installatiekopie die eerder is mislukt.

-  Als u een uitbreiding op de implementatie van een VM-inrichting te lang duurt, door gebruikers laten de inrichting time-outwaarde in plaats van bij het stoppen van het proces voor de toewijzing ongedaan maken of verwijderen van de virtuele machine.  

- <!-- 1662991 --> Diagnostische gegevens van Linux-VM wordt niet ondersteund in Azure-Stack. Wanneer u een Linux-VM met diagnostische gegevens van virtuele machine is ingeschakeld implementeert, mislukt de implementatie. De implementatie mislukt ook als u de Linux-VM basismetrieken via de instellingen voor diagnostische inschakelt.  


#### <a name="networking"></a>Netwerk
- Nadat een virtuele machine is gemaakt en gekoppeld aan een openbaar IP-adres, kunt u die virtuele machine uit dat IP-adres kan niet loskoppelen. Disassociation lijkt te werken, maar de eerder toegewezen openbaar IP-adres blijft gekoppeld aan de oorspronkelijke virtuele machine.

  Op dit moment moet u alleen nieuwe openbare IP-adressen voor de nieuwe virtuele machines die u maakt.

  Dit gebeurt zelfs als u een nieuwe virtuele machine het IP-adres toewijzen (gewoonlijk aangeduid als een *VIP's wisselen*). Alle toekomstige verbinding probeert te maken via dit resultaat van de IP-adres in van een verbinding met de oorspronkelijk gekoppelde virtuele machine en niet naar het nieuwe bericht.



- Azure-Stack ondersteunt één *lokale netwerkgateway* per IP-adres. Dit geldt voor alle abonnementen van de tenant. Nadat het maken van de eerste lokale gateway netwerkverbinding, daaropvolgende pogingen tot het maken van een lokale gateway netwerkbron met hetzelfde IP-adres worden geblokkeerd.

- Op een virtueel netwerk dat is gemaakt met een DNS-Server-instelling van *automatische*, wijzigen voor een aangepaste DNS-Server mislukt. De bijgewerkte instellingen zijn niet doorgevoerd in de virtuele machines in dit Vnet.

- Azure-Stack biedt geen ondersteuning voor extra netwerkinterfaces toevoegen aan een VM-instantie nadat de virtuele machine is geïmplementeerd. Als de virtuele machine meer dan één netwerkinterface vereist, moeten ze tijdens de implementatie worden gedefinieerd.

- <!-- 2096388 --> U kunt het beheerportal niet gebruiken om bij te werken van regels voor een netwerkbeveiligingsgroep. 

    Tijdelijke oplossing voor App Service: als u extern bureaublad op de exemplaren van de domeincontroller moet, u wijzigt de beveiligingsregels voor verbindingen binnen de netwerkbeveiligingsgroepen met PowerShell.  Hieronder vindt u voorbeelden van hoe *toestaan*, en zet u de configuratie van *weigeren*:  
    
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
- Controleer voordat u doorgaat, de belangrijke opmerking in [voordat u begint met](#before-you-begin) in de buurt van het begin van deze release-opmerkingen.

- Het kan maximaal één uur voordat gebruikers databases in een nieuwe implementatie van de SQL- of MySQL maken kunnen duren.

- Alleen de resourceprovider wordt voor het maken van items op servers die SQL-host of MySQL ondersteund. Items die worden gemaakt op een hostserver die niet zijn gemaakt door de resourceprovider kunnen leiden tot een niet-overeenkomende staat.  

- <!-- IS, ASDK --> Speciale tekens, inclusief spaties en perioden, worden niet ondersteund in de **familie** naam bij het maken van een SKU voor de resourceproviders SQL en MySQL.

> [!NOTE]  
> Nadat u naar Azure Stack 1803 bijwerkt, kunt u blijven gebruiken van de SQL- en MySQL resourceproviders die u eerder hebt geïmplementeerd.  U wordt aangeraden dat u SQL en MySQL bijwerken wanneer er een nieuwe versie beschikbaar. Als Azure-Stack updates toepassen op SQL- en MySQL resourceproviders sequentieel worden verwerkt.  Als u versie 1711, versie 1712 vervolgens 1802 eerst toe te passen en vervolgens bijwerken naar 1803.      
>   
> De installatie van update 1803 heeft geen invloed op het huidige gebruik van SQL- of MySQL resourceproviders door uw gebruikers.
> Ongeacht de versie van de resourceproviders die u gebruikt de gegevens van uw gebruikers in hun databases is niet aangeraakt, en blijft toegankelijk.    



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


## <a name="download-the-update"></a>De update downloaden
U kunt het pakket met de Azure-Stack 1803 update downloaden [hier](https://aka.ms/azurestackupdatedownload).


## <a name="see-also"></a>Zie ook
- Voor het gebruik van de bevoegde eindpunt (PEP) om te controleren en hervatten van updates, Zie [in Azure-Stack met behulp van de bevoegde endpoint-updates controleren](azure-stack-monitor-update.md).
- Zie voor een overzicht van de update management in Azure-Stack [beheren van updates in de Azure-Stack overzicht](azure-stack-updates.md).
- Zie voor meer informatie over het toepassen van updates met de Azure-Stack [toepassen van updates in Azure-Stack](azure-stack-apply-updates.md).
