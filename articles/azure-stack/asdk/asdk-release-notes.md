---
title: Opmerkingen bij de release van Microsoft Azure Stack Development Kit | Microsoft Docs
description: Verbeteringen, correcties en bekende problemen voor Azure Stack Development Kit.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/03/2018
ms.author: sethm
ms.reviewer: misainat
ms.openlocfilehash: a4be229e1c491c3062b14e631cfec5b14185bb82
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52957654"
---
# <a name="asdk-release-notes"></a>Opmerkingen bij de release van ASDK  
In dit artikel bevat informatie over verbeteringen, correcties en bekende problemen in Azure Stack Development Kit (ASDK). Als u niet zeker weet welke versie u uitvoert, kunt u [de portal gebruiken om te controleren](../azure-stack-updates.md#determine-the-current-version).

> De hoogte blijven van wat is er nieuw in de ASDK Abonneer u op de [ ![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#) [feed](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#).

## <a name="build-11809090"></a>Build 1.1809.0.90

### <a name="new-features"></a>Nieuwe functies
Deze versie bevat de volgende verbeteringen en oplossingen voor Azure Stack.  

<!--  2712869   | IS  ASDK -->  
- **Azure Stack syslog-client (algemene beschikbaarheid)** deze client kunt u het doorsturen van controles, waarschuwingen en -logboeken met betrekking tot de Azure Stack-infrastructuur naar een syslog-server of security information en event management (SIEM) software extern naar Azure Stack. De syslog-client biedt nu ondersteuning voor de poort waarop de syslog-server luistert op te geven.

Met deze release de syslog-client is algemeen beschikbaar en deze kan worden gebruikt in een productieomgeving.

Zie voor meer informatie, [Azure Stack syslog doorsturen](../azure-stack-integrate-security.md).

### <a name="fixed-issues"></a>Problemen opgelost

<!-- TBD - IS ASDK -->
- In de portal is de geheugen-grafiek reporting capaciteit vrij/gebruikt nu nauwkeurig. U kunt nu het aantal virtuele machines kunt maken meer betrouwbare wijze voorspellen.

<!-- TBD - IS ASDK --> 
- Er is een probleem waarin u virtuele machines gemaakt op de gebruikersportal van Azure Stack en de portal weergegeven een onjuist aantal gegevensschijven die aan een virtuele machine uit de DS-serie kunt koppelen opgelost. DS-serie VM's kan zo veel gegevensschijven bevatten als de Azure-configuratie.

- <!-- 2702741 -  IS, ASDK --> Probleem opgelost in welke openbare IP-adressen die zijn geïmplementeerd met behulp van de dynamische toewijzing methode zijn niet gegarandeerd te behouden nadat een Stop-toewijzing is uitgegeven. Ze blijven nu behouden.

- <!-- 3078022 - IS, ASDK --> Als een virtuele machine stop-deallocated voordat u 1808 is kan deze niet worden opnieuw worden toegewezen na de 1808 update.  Dit probleem is opgelost in 1809. Exemplaren die in deze status zijn en kunnen niet worden gestart kunnen worden gestart in 1809 met deze oplossing. Het probleem voorkomt ook dat dit probleem opnieuw optreedt.

- **Verschillende oplossingen** voor prestaties, stabiliteit, beveiliging en het besturingssysteem dat wordt gebruikt door Azure Stack

### <a name="changes"></a>Wijzigingen

### <a name="known-issues"></a>Bekende problemen

#### <a name="portal"></a>Portal  

<!-- 2515955   | IS ,ASDK--> 
- *Alle services* vervangt *meer services* in de Azure Stack-beheerder en gebruiker portals. U kunt nu *alle services* als alternatief voor het navigeren in de Azure Stack-portals de dezelfde manier als in de Azure-portals.

<!--  TBD – IS, ASDK --> 
- *Basic A* grootten van virtuele machines buiten gebruik worden gesteld voor [het maken van virtuele-machineschaalsets](../azure-stack-compute-add-scalesets.md) (VMSS) via de portal. Als u wilt een VMSS te maken met deze grootte, PowerShell of een sjabloon te gebruiken.

#### <a name="health-and-monitoring"></a>Status en bewaking

<!-- 1264761 - IS ASDK -->  
- Mogelijk ziet u waarschuwingen voor de *Health controller* onderdeel waarvoor u de volgende gegevens:  

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

<!-- 2368581 - IS. ASDK --> 
- Azure Stack-operators, als u een waarschuwing voor een beperkte hoeveelheid geheugen ontvangt en virtuele machines van tenants niet te implementeren met een *fout bij het maken van infrastructuur-VM*, is het mogelijk dat de Azure Stack-stempel heeft te weinig geheugen beschikbaar. Gebruik de [Azure Stack Capacity Planner](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) naar het beste informatie over de beschikbare capaciteit voor uw workloads.


#### <a name="compute"></a>Compute 

<!-- 3164607 – IS, ASDK -->
- Nadat een losgekoppelde schijf aan de dezelfde virtuele machine (VM) met dezelfde naam en LUN mislukt met foutmelding zoals **gegevens schijf 'datadisk' niet koppelen aan virtuele machine 'vm1'**. De fout treedt op omdat de schijf momenteel wordt losgekoppeld of de laatste koppelingsbewerking is mislukt. Wacht totdat de schijf volledig is losgekoppeld en vervolgens opnieuw of verwijderen/loskoppelen van de schijf expliciet opnieuw. De oplossing is om te koppelen met een andere naam of op een andere logische eenheid. 

<!-- 3235634 – IS, ASDK -->
- Het implementeren van VM's met grootten met een **v2** achtervoegsel; bijvoorbeeld, **Standard_A2_v2**, geef het achtervoegsel als **Standard_A2_v2** (kleine letters v). Gebruik geen **Standard_A2_V2** (V hoofdletters). Dit werkt in de globale Azure en is een inconsistentie in Azure Stack.

<!-- 3099544 – IS, ASDK --> 
- Wanneer u een nieuwe virtuele machine (VM) met behulp van de Azure Stack-portal maakt, en u de VM-grootte selecteert, wordt de kolom USD/maand weergegeven met een **niet beschikbaar** bericht. Deze kolom mag niet weergegeven. prijzen kolom, dat is het weergeven van de virtuele machine wordt niet ondersteund in Azure Stack.

<!-- 2869209 – IS, ASDK --> 
- Wanneer u de [ **toevoegen AzsPlatformImage** cmdlet](https://docs.microsoft.com/powershell/module/azs.compute.admin/add-azsplatformimage?view=azurestackps-1.4.0), moet u de **- OsUri** parameter als het opslagaccount URI waar de schijf is geüpload. Als u het lokale pad van de schijf gebruikt, wordt de cmdlet mislukt met de volgende fout: *langdurige bewerking is mislukt met de status 'Mislukt'*. 

<!--  2966665 – IS, ASDK --> 
- SSD-gegevensschijven koppelen aan de grootte van de premium beheerde schijf virtuele machines (DS, DSv2, Fs, Fs_V2) mislukt met fout: *schijven voor de virtuele machine 'vmname' Fout bij het bijwerken is mislukt: gevraagde bewerking kan niet worden uitgevoerd omdat het type opslagaccount ' Premium_LRS wordt niet ondersteund voor VM-grootte ' Standard_DS/Ds_V2/FS/Fs_v2)*

   U kunt dit probleem omzeilen, gebruikt u *Standard_LRS* gegevensschijven in plaats van *Premium_LRS schijven*. Het gebruik van *Standard_LRS* gegevensschijven IOP's of de kosten voor facturering niet wijzigen.  

<!--  2795678 – IS, ASDK --> 
- Wanneer u de portal virtuele machines (VM) maken in een premium VM-grootte (DS, Ds_v2, FS, FSv2) gebruikt, wordt de virtuele machine wordt gemaakt in een standard storage-account. Het maken van in een standard storage-account heeft geen invloed op functioneel, IOP's, of facturering. 

   U kunt gewoon de waarschuwing dat negeren: *u hebt gekozen voor het gebruik van een standaard schijf op een grootte die premium-schijven ondersteunt. Dit kan invloed hebben op prestaties van besturingssysteem en wordt niet aanbevolen. Overweeg het gebruik van premium-opslag (SSD) in plaats daarvan.*

<!-- 2967447 - IS, ASDK --> 
- De virtuele-machineschaalset (VMSS) aan die conferenties 7.2 CentOS gebaseerde biedt als een optie voor implementatie. Omdat die installatiekopie niet beschikbaar op Azure Stack is, selecteert u een ander besturingssysteem voor uw implementatie of u een Azure Resource Manager-sjabloon op te geven van een andere CentOS-installatiekopie die door de operator voorafgaand aan de implementatie van de marketplace is gedownload.

<!-- TBD -  IS ASDK --> 
- Instellingen voor vergroten/verkleinen voor schaalsets voor virtuele machines zijn niet beschikbaar in de portal. Als tijdelijke oplossing, kunt u [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Vanwege de verschillen tussen versies van PowerShell, moet u de `-Name` parameter in plaats van `-VMScaleSetName`.

<!-- TBD -  IS ASDK --> 
- Wanneer een VM-installatiekopie niet kan worden gemaakt, kan een mislukte-item dat u niet verwijderen worden toegevoegd aan de VM-installatiekopieën compute-blade.

  Als tijdelijke oplossing, kunt u een nieuwe VM-installatiekopie maken met een dummy VHD dat kan worden gemaakt via Hyper-V (New-VHD-pad C:\dummy.vhd-vaste - SizeBytes 1 GB). Dit proces los het probleem dat verhindert dat het mislukte item wordt verwijderd. Vervolgens, 15 minuten na het maken van de installatiekopie van het pop, u kunt met succes verwijderen.

  U kunt vervolgens opnieuw proberen het downloaden van de VM-installatiekopie die eerder is mislukt.

<!-- TBD -  IS ASDK --> 
- Als een uitbreiding op een VM-implementatie de inrichting te lang duurt, laat gebruikers de time-out van de inrichting in plaats van bij stoppen van de procedure voor de toewijzing ongedaan of verwijder de virtuele machine.  

<!-- 1662991 - IS ASDK --> 
- Diagnostische gegevens over Linux-VM wordt niet ondersteund in Azure Stack. Wanneer u een Linux-VM met VM diagnostics is ingeschakeld implementeren, wordt de implementatie mislukt. De implementatie mislukt ook als u de Linux-VM eenvoudige metrische gegevens via diagnostische instellingen inschakelen.

<!-- 2724961- IS ASDK --> 
- Wanneer u registreert de **Microsoft.Insight** resourceprovider in instellingen voor abonnement, en een Windows-VM maken met gast OS diagnostische ingeschakeld, wordt de grafiek CPU-Percentage in de overzichtspagina van virtuele machine niet mogelijk om metrische gegevens weer te geven.
 
  Als u zoekt de grafiek CPU-Percentage voor de virtuele machine, gaat u naar de **metrische gegevens** blade en het weergeven van alle ondersteunde Windows-VM-Gast metrische gegevens.

 

#### <a name="networking"></a>Netwerken
<!-- 1766332 - IS, ASDK --> 
- Onder **netwerken**, als u klikt op **VPN-Gateway maken** voor het instellen van een VPN-verbinding, **op basis van beleid** wordt vermeld als een VPN-type. Selecteer deze optie niet. Alleen de **Route op basis van** optie wordt ondersteund in Azure Stack.

<!-- 1902460 -  IS ASDK --> 
- Azure Stack biedt ondersteuning voor een enkel *lokale netwerkgateway* per IP-adres. Dit geldt voor alle tenant-abonnementen. Nadat het maken van de eerste lokale gateway netwerkverbinding, volgende pogingen tot het maken van een resource van een lokale netwerkgateway met hetzelfde IP-adres worden geblokkeerd.

<!-- 16309153 -  IS ASDK --> 
- In een Virtueelnetwerk dat is gemaakt met een DNS-Server-instelling van *automatische*, wijzigen naar een aangepaste DNS-Server mislukt. De bijgewerkte instellingen worden niet gepusht naar virtuele machines in dit Vnet.

<!-- 2702741 -  IS ASDK --> 
- Openbare IP-adressen die zijn geïmplementeerd met behulp van de dynamische toewijzingsmethode zijn niet noodzakelijkerwijs te behouden nadat een Stop-toewijzing is uitgegeven.

<!-- 2529607 - IS ASDK --> 
- Tijdens de Azure Stack *geheim rotatie*, er is een periode waarin openbare IP-adressen niet bereikbaar voor twee tot vijf minuten zijn.

<!-- 2664148 - IS ASDK --> 
- Kunnen optreden in scenario's waar de tenant er toegang hun virtuele machines tot heeft met behulp van een S2S-VPN-tunnel, een scenario waarbij verbindingspogingen mislukt als de on-premises-subnet is toegevoegd aan de lokale netwerkgateway nadat de gateway is al gemaakt. 


<!--  #### SQL and MySQL  -->


#### <a name="app-service"></a>App Service
<!-- 2352906 - IS ASDK --> 
- Gebruikers moeten de storage resourceprovider registreren voordat ze hun eerste Azure-functie in het abonnement maken.

<!-- TBD - IS ASDK --> 
- Als u wilt de infrastructuur (werknemers, beheer, front-rollen) uitbreiden, moet u PowerShell gebruiken, zoals beschreven in de opmerkingen bij de release voor Compute.  


#### <a name="usage"></a>Gebruik  
<!-- TBD -  IS ASDK --> 
- Gebruik openbare IP-adres-gebruiksgegevens meter toont dezelfde *datum/tijd evenement* waarde voor elke record in plaats van de *TimeDate* stempel waarin wordt weergegeven wanneer de record is gemaakt. U kunt deze gegevens op dit moment niet gebruiken om uit te voeren nauwkeurige accounting van gebruik van openbare IP-adres.

<!-- #### Identity -->

## <a name="build-11808097"></a>Build 1.1808.0.97

### <a name="new-features"></a>Nieuwe functies
Deze versie bevat de volgende verbeteringen en oplossingen voor Azure Stack.  

<!-- 1658937 | ASDK, IS --> 
- **Starten van back-ups volgens een vooraf gedefinieerd schema** -als een apparaat, Azure Stack kan nu automatisch activeren infrastructuur back-ups regelmatig om te voorkomen van menselijk handelen. Azure Stack wordt ook automatisch opschonen van de externe share voor back-ups die ouder dan de opgegeven bewaarperiode zijn. Zie voor meer informatie, [back-up inschakelen voor Azure Stack met PowerShell](../azure-stack-backup-enable-backup-powershell.md).

<!-- 2496385 | ASDK, IS -->  
- **Toegevoegde tijd in de totale back-uptijd voor gegevensoverdracht.** Zie voor meer informatie, [back-up inschakelen voor Azure Stack met PowerShell](../azure-stack-backup-enable-backup-powershell.md).

<!-- 1702130 | ASDK, IS --> 
- **Back-up externe capaciteit wordt nu de juiste capaciteit van de externe share.** (Dit was eerder programmeren tot 10 GB.) Zie voor meer informatie, [back-up inschakelen voor Azure Stack met PowerShell](../azure-stack-backup-enable-backup-powershell.md).
 
<!-- 2753130 |  IS, ASDK   -->  
- **Azure Resource Manager-sjablonen bieden nu ondersteuning voor het element voorwaarde** -u kunt nu een resource in een Azure Resource Manager-sjabloon met behulp van een voorwaarde implementeren. U kunt de sjabloon voor het implementeren van een resource op basis van een voorwaarde, zoals het evalueren van als een parameterwaarde bevindt ontwerpen. Zie voor meer informatie over het gebruik van een sjabloon als een voorwaarde [een resource voorwaardelijk implementeren](https://docs.microsoft.com/azure/architecture/building-blocks/extending-templates/conditional-deploy) en [sectie met variabelen van Azure Resource Manager-sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-templates-variables) in de documentatie van Azure. 

   U kunt ook sjablonen [resources implementeren op meer dan één abonnement of resourcegroep](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-cross-resource-group-deployment).  

<!--2753073 | IS, ASDK -->  
- **Ondersteuning voor de versie van de Microsoft.COMPUTE API-resource is bijgewerkt** ondersteuning voor API-versie 2017-10-01, 2015-06-15 voor Azure Stack-netwerkbronnen. Ondersteuning voor resource-versies tussen 10-01-2017 en 15-06-2015 is niet opgenomen in deze release. Raadpleeg [overwegingen voor Azure Stack-netwerken](../user/azure-stack-network-differences.md) voor functionaliteit verschillen.

<!-- 2272116 | IS, ASDK   -->  
- **Azure Stack is ondersteuning toegevoegd voor omgekeerde DNS-zoekacties voor dergelijke infrastructuur-eindpunten voor Azure Stack** (dat is voor de portal, adminportal, beheer en adminmanagement). Hiermee kunt Azure Stack Extern eindpunt namen worden omgezet vanaf een IP-adres.

<!-- 2780899 |  IS, ASDK   --> 
- **Azure Stack biedt nu ondersteuning voor extra netwerkinterfaces toevoegen aan een bestaande virtuele machine.**  Deze functionaliteit is beschikbaar via de portal, PowerShell en CLI. Zie voor meer informatie, [toevoegen of verwijderen van netwerkinterfaces](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface-vm) in de documentatie van Azure. 

<!-- 2222444 | IS, ASDK   -->  
- **Verbeteringen in de nauwkeurigheid en zeer flexibel zijn aangebracht aan netwerken gebruik meters**. Netwerk gebruik meters zijn nu meer nauwkeurige en houden rekening onderbroken abonnementen, perioden van de serviceonderbreking en racevoorwaarden.

<!-- 2297790 | IS, ASDK -->  
- **Verbeteringen in de Azure Stack-syslog-client (preview-functie)**. Deze client kunt het doorsturen van controle en logboeken met betrekking tot de Azure Stack-infrastructuur naar een syslog-server of security information en event management (SIEM) software buiten Azure Stack. De syslog-client ondersteunt nu het TCP-protocol met tekst zonder opmaak of TLS 1.2-versleuteling, de laatste is de standaardconfiguratie. U kunt de TLS-verbinding configureren met alleen-server of wederzijdse verificatie.

  Als u wilt configureren hoe de syslog-client (zoals protocol, versleuteling en -verificatie) communiceert met de syslog-server, gebruikt u de cmdlet Set-SyslogServer. Deze cmdlet is beschikbaar via het eindpunt van de bevoegde (PEP). 

- <!-- ASDK --> **Galerij-items voor Virtual Machine Scale Sets zijn nu ingebouwde**.  Virtuele-Machineschaalset galerie-items worden nu beschikbaar in de gebruiker en beheerder portals gemaakt zonder deze te downloaden. 

- <!-- IS, ASDK --> **Virtuele-Machineschaalset schalen**.  U kunt de portal gebruiken om [schaal van een virtuele-Machineschaalset](../azure-stack-compute-add-scalesets.md#scale-a-virtual-machine-scale-set) (VMSS).   

- <!-- 2489570 | IS ASDK--> **Ondersteuning voor aangepaste configuraties voor beleid voor IPSec/IKE** voor [VPN-gateways in Azure Stack](/azure/azure-stack/azure-stack-vpn-gateway-about-vpn-gateways).

- <!-- | IS ASDK--> **Marketplace-item voor Kubernetes**. U kunt nu met behulp van Kubernetes-clusters implementeren de [Kubernetes Marketplace-item](/azure/azure-stack/azure-stack-solution-template-kubernetes-cluster-add). Gebruikers kunnen selecteert u het Kubernetes-item en vul een aantal parameters voor het implementeren van een Kubernetes-cluster in Azure Stack. Het doel van de sjablonen is het eenvoudig te voor gebruikers voor setup dev/test Kubernetes-implementaties in een paar stappen.

<!-- ####### | IS, ASDK -->  
- **Azure Resource Manager bevat de regionaam van de.** Met deze release bevat objecten die zijn opgehaald van de Azure Resource Manager nu het naamkenmerk regio. Als een bestaande PowerShell-script wordt het object rechtstreeks aan een andere cmdlet doorgegeven, kan het script een foutmelding weergegeven en mislukken. Dit gedrag van Azure Resource Manager-compatibel is en vereist dat de aanroepende client om af te trekken van het kenmerk regio. Voor meer informatie over de Azure Resource Manager-Zie [Azure Resource Manager-documentatie](https://docs.microsoft.com/azure/azure-resource-manager/).

<!-- TBD | IS, ASDK -->  
- **Abonnementen verplaatsen tussen gedelegeerde Providers.** U kunt abonnementen nu verplaatsen tussen abonnementen van nieuwe of bestaande Provider overgedragen die deel uitmaken van dezelfde Directory-tenant. Abonnementen die behoren tot het abonnement van de Provider standaard kunnen ook worden verplaatst naar de Provider overgedragen abonnementen in dezelfde Directory-tenant. Zie voor meer informatie [aanbiedingen in Azure Stack delegeren](../azure-stack-delegated-provider.md).
 
<!-- 2536808 IS ASDK --> 
- **Aanmaaktijd van de virtuele machine verbeterd** voor virtuele machines die zijn gemaakt met de installatiekopieën die u vanuit de Azure marketplace downloaden.

### <a name="fixed-issues"></a>Problemen opgelost
- <!-- IS ASDK--> We het probleem voor het maken van een beschikbaarheidsset in de portal dat heeft geresulteerd in de set met een domein met fouten en een updatedomein van 1 is opgelost.

<!-- TBD | ASDK, IS --> 
- Er zijn verschillende verbeteringen aangebracht in het updateproces zodat deze betrouwbaarder. Bovendien zijn verbeteringen aangebracht aan onderliggende infrastructuur, wat zorgt voor betere knooppunt drain, waardoor de mogelijke downtime voor workloads tijdens het bijwerken worden geminimaliseerd.

<!--2292271 | ASDK, IS --> 
- We een probleem opgelost waarbij een gewijzigde quotumlimiet is niet van toepassing op bestaande abonnementen.  Nu, wanneer u een quotumlimiet voor een netwerkbron die deel uitmaakt van een aanbieding verhogen en Plan dat is gekoppeld aan een gebruikersabonnement, de nieuwe limiet van toepassing op de bestaande abonnementen, evenals de nieuwe abonnementen.

<!-- 2448955 | IS ASDK --> 
- U kunt nu activiteitenlogboeken voor systemen die zijn geïmplementeerd in een tijdzone UTC + N is opvragen.    

<!-- 2319627 |  ASDK, IS --> 
- Controle vooraf voor de back-upconfiguratie parameters (pad/gebruikersnaam/wachtwoord/Encryption Key) wordt niet langer onjuiste instellingen ingesteld op de back-upconfiguratie. (Voorheen onjuiste instellingen zijn ingesteld in de back-up en back-up, klikt u vervolgens mislukken wanneer ze worden geactiveerd.)

<!-- 2215948 |  ASDK, IS --> 
- De lijst met back-up wordt nu vernieuwd wanneer u de back-up handmatig van de externe share verwijderen.

<!-- 2360715 |  ASDK, IS -->  
- Bij het instellen van datacenter-integratie, u geen toegang meer tot de AD FS-bestand met metagegevens van een share. Zie voor meer informatie, [AD FS-integratie instellen door op te geven van bestand met federatieve metagegevens](../azure-stack-integrate-identity.md#setting-up-ad-fs-integration-by-providing-federation-metadata-file). 

<!-- 2388980 | ASDK, IS --> 
- We een probleem opgelost dat gebruikers van een bestaande openbare IP-adres toegewezen die was eerder zijn toegewezen aan een netwerkinterface of Load Balancer met een nieuwe netwerkinterface of Load Balancer.  

<!-- 2551834 - IS, ASDK --> 
- Wanneer u selecteert *overzicht* voor een opslagaccount in de beheerder of de gebruiker-portals, de *Essentials* deelvenster nu correct wordt weergegeven de verwachte gegevens. 

<!-- 2551834 - IS, ASDK --> 
- Wanneer u selecteert *Tags* voor een opslagaccount in de beheerder of de gebruiker-portals, nu de gegevens correct wordt weergegeven.

<!-- TBD - IS ASDK --> 
- Deze versie van Azure Stack kunt u het probleem waardoor de toepassing van updates voor stuurprogramma's van OEM-extensiepakketten.

<!-- 2055809- IS ASDK --> 
- We vastgesteld dat een probleem dat u niet het verwijderen van virtuele machines op de blade compute wanneer de virtuele machine kan niet worden gemaakt.  

<!--  2643962 IS ASDK -->  
- De waarschuwing voor *geheugencapaciteit van lage* langer ten onrechte wordt weergegeven.

<!--  TBD ASDK --> 
- De virtuele machine die als host fungeert voor het eindpunt van de bevoegdheden (PEP) is verhoogd tot 4GB. In de ASDK heet deze virtuele machine AzS-ERCS01.

- <!--  TBD – IS, ASDK --> *Basic A* grootten van virtuele machines buiten gebruik worden gesteld voor [het maken van virtuele-machineschaalsets](../azure-stack-compute-add-scalesets.md) (VMSS) via de portal. Als u wilt een VMSS te maken met deze grootte, PowerShell of een sjabloon te gebruiken. 

### <a name="known-issues"></a>Bekende problemen

#### <a name="portal"></a>Portal  
- <!-- 2967387 – IS, ASDK --> Het account waarmee u zich aanmeldt bij de portal voor Azure Stack-beheerder of gebruiker worden weergegeven als **onbekende gebruiker**. Dit treedt op wanneer het account beschikt niet over een een *eerste* of *laatste* naam die is opgegeven. U kunt dit probleem omzeilen, het gebruikersaccount voor de naam van de eerste of laatste te bewerken. U moet vervolgens Meld u af en meld u vervolgens terug naar de portal. 

-  <!--  2873083 - IS ASDK --> Wanneer gebruikt u de portal voor het maken van een virtuele-machineschaalset instellen (VMSS), de *exemplaargrootte* vervolgkeuzelijst niet juist geladen wanneer u Internet Explorer gebruikt. U kunt dit probleem omzeilen, gebruikt u een andere browser tijdens het gebruik van de portal voor het maken van een VMSS.  

#### <a name="portal"></a>Portal  
<!-- 2931230 – IS  ASDK --> 
- Abonnementen die zijn toegevoegd aan een gebruikersabonnement als een aanvullende plan kunnen niet worden verwijderd, zelfs wanneer u het abonnement uit het gebruikersabonnement verwijderen. Het abonnement blijft totdat de abonnementen die verwijzen naar het aanvullende plan worden ook verwijderd. 

<!--2760466 – IS  ASDK --> 
- Wanneer u een nieuwe Azure Stack-omgeving met deze versie installeert, de waarschuwing die aangeeft *activering vereist* mogelijk niet weergegeven. [Activering](../azure-stack-registration.md) is vereist voordat u de marketplace-publicatie kunt gebruiken. 

<!-- TBD - IS ASDK --> 
- De twee administratieve abonnementstypen die zijn geïntroduceerd in versie 1804 mag niet worden gebruikt. De abonnementstypen zijn **softwarelicentiecontrole abonnement**, en **verbruik abonnement**. Deze abonnementstypen zijn **softwarelicentiecontrole abonnement**, en **verbruik abonnement**. Deze abonnementstypen zijn zichtbaar in de nieuwe Azure Stack-omgevingen vanaf versie 1804 maar nog niet klaar voor gebruik. U moet echter ook doorgaan met de **standaard providerabonnement** type.

<!-- 2403291 - IS ASDK --> 
- Hebt u mogelijk geen gebruik van de horizontale schuifbalk langs de onderkant van de beheerder en gebruiker portals. Als u geen toegang de horizontale schuifbalk tot, blijven gebruiken de breadcrumbs om naar een vorige blade in de portal navigeren zijn door de naam van de blade te selecteren dat u wilt weergeven in de breadcrumb-lijst gevonden aan de bovenkant van de portal.
  ![Breadcrumb](media/asdk-release-notes/breadcrumb.png)

<!-- TBD -  IS ASDK --> 
- Verwijderen van zwevende resources leidt van gebruiker-abonnementen. Als tijdelijke oplossing, eerst Gebruikersbronnen of de hele resourcegroep verwijderen en verwijder vervolgens gebruikersabonnementen.

<!-- TBD -  IS ASDK --> 
- U kunt machtigingen aan uw abonnement met behulp van de Azure Stack-portals niet weergeven. Als tijdelijke oplossing, moet u PowerShell gebruiken om machtigingen te controleren.

<!--  TBD | ASDK -->  
- De standaardtijdzone voor uw Azure Stack-implementatie wordt nu ophalen ingesteld op UTC. U kunt een tijdzone selecteren bij het installeren van Azure Stack, maar deze wordt automatisch hersteld naar UTC als standaardwaarde tijdens de installatie.

#### <a name="health-and-monitoring"></a>Status en bewaking
<!-- 1264761 - IS ASDK -->  
- Mogelijk ziet u waarschuwingen voor de *Health controller* onderdeel waarvoor u de volgende gegevens:  

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

<!-- 2368581 - IS. ASDK --> 
- Azure Stack-operators, als u een waarschuwing voor een beperkte hoeveelheid geheugen ontvangt en virtuele machines van tenants niet te implementeren met een *fout bij het maken van infrastructuur-VM*, is het mogelijk dat de Azure Stack-stempel heeft te weinig geheugen beschikbaar. Gebruik de [Azure Stack Capacity Planner](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) naar het beste informatie over de beschikbare capaciteit voor uw workloads.

<!-- TBD - IS. ASDK --> 
- Bij het uitvoeren van de **Test AzureStack** cmdlet het bevoegde eindpunt (PEP), de **prestaties van Azure Stack-infrastructuur rol instanties** test een bericht van de waarschuwing wordt gegenereerd voor de VM ERCS. U kunt veilig negeren van het bericht waarschuwen en echter ook doorgaan met de ASDK.

#### <a name="compute"></a>Compute
<!-- 2494144 - IS, ASDK --> 
- Bij het selecteren van een VM-grootte voor de implementatie van een virtuele machine, sommige F-serie VM-grootten zijn niet zichtbaar als onderdeel van de grootte selector bij het maken van een virtuele machine. De volgende VM-grootten worden niet weergegeven in de kiezer: *F8s_v2*, *F16s_v2*, *F32s_v2*, en *F64s_v2*.  
  Als tijdelijke oplossing, gebruikt u een van de volgende methoden om een VM te implementeren. In elke methode moet u om op te geven van de VM-grootte die u wilt gebruiken.

- <!-- 3099544 – IS, ASDK --> Wanneer u een nieuwe virtuele machine (VM) met behulp van de Azure Stack-portal maakt, en u de VM-grootte selecteert, wordt de kolom USD/maand weergegeven met een **niet beschikbaar** bericht. Deze kolom mag niet weergegeven. prijzen kolom, dat is het weergeven van de virtuele machine wordt niet ondersteund in Azure Stack.

- <!-- 2869209 – IS, ASDK --> Wanneer u de [ **toevoegen AzsPlatformImage** cmdlet](https://docs.microsoft.com/powershell/module/azs.compute.admin/add-azsplatformimage?view=azurestackps-1.4.0), moet u de **- OsUri** parameter als het opslagaccount URI waar de schijf is geüpload. Als u het lokale pad van de schijf gebruikt, wordt de cmdlet mislukt met de volgende fout: *langdurige bewerking is mislukt met de status 'Mislukt'*. 

- <!--  2966665 – IS, ASDK --> SSD-gegevensschijven koppelen aan de grootte van de premium beheerde schijf virtuele machines (DS, DSv2, Fs, Fs_V2) mislukt met fout: *schijven voor de virtuele machine 'vmname' Fout bij het bijwerken is mislukt: gevraagde bewerking kan niet worden uitgevoerd omdat het type opslagaccount ' Premium_LRS wordt niet ondersteund voor VM-grootte ' Standard_DS/Ds_V2/FS/Fs_v2)*

   U kunt dit probleem omzeilen, gebruikt u *Standard_LRS* gegevensschijven in plaats van *Premium_LRS schijven*. Het gebruik van *Standard_LRS* gegevensschijven IOP's of de kosten voor facturering niet wijzigen.  

- <!--  2795678 – IS, ASDK --> Wanneer u de portal virtuele machines (VM) maken in een premium VM-grootte (DS, Ds_v2, FS, FSv2) gebruikt, wordt de virtuele machine wordt gemaakt in een standard storage-account. Het maken van in een standard storage-account heeft geen invloed op functioneel, IOP's, of facturering. 

   U kunt gewoon de waarschuwing dat negeren: *u hebt gekozen voor het gebruik van een standaard schijf op een grootte die premium-schijven ondersteunt. Dit kan invloed hebben op prestaties van besturingssysteem en wordt niet aanbevolen. Overweeg het gebruik van premium-opslag (SSD) in plaats daarvan.*

- <!-- 2967447 - IS, ASDK --> De virtuele-machineschaalset (VMSS) aan die conferenties 7.2 CentOS gebaseerde biedt als een optie voor implementatie. Omdat die installatiekopie niet beschikbaar op Azure Stack is, selecteert u een ander besturingssysteem voor uw implementatie of u een ARM-sjabloon op te geven van een andere CentOS-installatiekopie die door de operator voorafgaand aan de implementatie van de marketplace is gedownload.

<!-- TBD -  IS ASDK --> 
- Instellingen voor vergroten/verkleinen voor schaalsets voor virtuele machines zijn niet beschikbaar in de portal. Als tijdelijke oplossing, kunt u [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Vanwege de verschillen tussen versies van PowerShell, moet u de `-Name` parameter in plaats van `-VMScaleSetName`.

<!-- TBD -  IS ASDK --> 
- Wanneer u virtuele machines op de gebruikersportal van Azure Stack maakt, wordt een onjuist aantal gegevensschijven dat een virtuele machine uit de D-serie kunt koppelen door de portal weergegeven. Alle ondersteunde D-serie VM's kan zo veel gegevensschijven bevatten als de Azure-configuratie.

<!-- TBD -  IS ASDK --> 
- Wanneer een VM-installatiekopie niet kan worden gemaakt, kan een mislukte-item dat u niet verwijderen worden toegevoegd aan de VM-installatiekopieën compute-blade.

  Als tijdelijke oplossing, kunt u een nieuwe VM-installatiekopie maken met een dummy VHD dat kan worden gemaakt via Hyper-V (New-VHD-pad C:\dummy.vhd-vaste - SizeBytes 1 GB). Dit proces los het probleem dat verhindert dat het mislukte item wordt verwijderd. Vervolgens, 15 minuten na het maken van de installatiekopie van het pop, u kunt met succes verwijderen.

  U kunt vervolgens opnieuw proberen het downloaden van de VM-installatiekopie die eerder is mislukt.

<!-- TBD -  IS ASDK --> 
- Als een uitbreiding op een VM-implementatie de inrichting te lang duurt, laat gebruikers de time-out van de inrichting in plaats van bij stoppen van de procedure voor de toewijzing ongedaan of verwijder de virtuele machine.  

<!-- 1662991 - IS ASDK --> 
- Diagnostische gegevens over Linux-VM wordt niet ondersteund in Azure Stack. Wanneer u een Linux-VM met VM diagnostics is ingeschakeld implementeren, wordt de implementatie mislukt. De implementatie mislukt ook als u de Linux-VM eenvoudige metrische gegevens via diagnostische instellingen inschakelen.

<!-- 2724961- IS ASDK --> 
- Wanneer u registreert de **Microsoft.Insight** resourceprovider in instellingen voor abonnement, en een virtuele Windows-machine maken met gast OS diagnostische ingeschakeld, de overzichtspagina van de virtuele machine metrische gegevens niet weergeven. 

 

#### <a name="networking"></a>Netwerken
<!-- 1766332 - IS, ASDK --> 
- Onder **netwerken**, als u klikt op **VPN-Gateway maken** voor het instellen van een VPN-verbinding, **op basis van beleid** wordt vermeld als een VPN-type. Selecteer deze optie niet. Alleen de **Route op basis van** optie wordt ondersteund in Azure Stack.

<!-- 1902460 -  IS ASDK --> 
- Azure Stack biedt ondersteuning voor een enkel *lokale netwerkgateway* per IP-adres. Dit geldt voor alle tenant-abonnementen. Nadat het maken van de eerste lokale gateway netwerkverbinding, volgende pogingen tot het maken van een resource van een lokale netwerkgateway met hetzelfde IP-adres worden geblokkeerd.

<!-- 16309153 -  IS ASDK --> 
- In een Virtueelnetwerk dat is gemaakt met een DNS-Server-instelling van *automatische*, wijzigen naar een aangepaste DNS-Server mislukt. De bijgewerkte instellingen worden niet gepusht naar virtuele machines in dit Vnet.

<!-- 2702741 -  IS ASDK --> 
- Openbare IP-adressen die zijn geïmplementeerd met behulp van de dynamische toewijzingsmethode zijn niet noodzakelijkerwijs te behouden nadat een Stop-toewijzing is uitgegeven.

<!-- 2529607 - IS ASDK --> 
- Tijdens de Azure Stack *geheim rotatie*, er is een periode waarin openbare IP-adressen niet bereikbaar voor twee tot vijf minuten zijn.

<!-- 2664148 - IS ASDK --> 
- Kunnen optreden in scenario's waar de tenant er toegang hun virtuele machines tot heeft met behulp van een S2S-VPN-tunnel, een scenario waarbij verbindingspogingen mislukt als de on-premises-subnet is toegevoegd aan de lokale netwerkgateway nadat de gateway is al gemaakt. 


#### <a name="sql-and-mysql"></a>SQL- en MySQL
<!-- TBD - ASDK --> 
- De database die als host fungeert servers moet worden toegewezen voor gebruik door de resource-provider en gebruiker werkbelastingen. U kunt een exemplaar dat wordt gebruikt door andere consumenten, met inbegrip van App Services niet gebruiken.

<!-- IS, ASDK --> 
- Speciale tekens, inclusief spaties en perioden, worden niet ondersteund in de **familie** naam wanneer u een SKU voor de SQL- en MySQL-resourceproviders.

#### <a name="app-service"></a>App Service
<!-- 2352906 - IS ASDK --> 
- Gebruikers moeten de storage resourceprovider registreren voordat ze hun eerste Azure-functie in het abonnement maken.

<!-- TBD - IS ASDK --> 
- Als u wilt de infrastructuur (werknemers, beheer, front-rollen) uitbreiden, moet u PowerShell gebruiken, zoals beschreven in de opmerkingen bij de release voor Compute.  

<!-- TBD - IS ASDK --> 
- App Service kan alleen worden geïmplementeerd in de *standaard providerabonnement* op dit moment.

#### <a name="usage"></a>Gebruik  
<!-- TBD -  IS ASDK --> 
- Gebruik openbare IP-adres-gebruiksgegevens meter toont dezelfde *datum/tijd evenement* waarde voor elke record in plaats van de *TimeDate* stempel waarin wordt weergegeven wanneer de record is gemaakt. U kunt deze gegevens op dit moment niet gebruiken om uit te voeren nauwkeurige accounting van gebruik van openbare IP-adres.

<!-- #### Identity -->


## <a name="build-11807076"></a>Build 1.1807.0.76

### <a name="new-features"></a>Nieuwe functies
Deze versie bevat de volgende verbeteringen en oplossingen voor Azure Stack.  

- <!-- 1658937 | ASDK, IS --> **Starten van back-ups volgens een vooraf gedefinieerd schema** -als een apparaat, Azure Stack kan nu automatisch activeren infrastructuur back-ups regelmatig om te voorkomen van menselijk handelen. Azure Stack wordt ook automatisch opschonen van de externe share voor back-ups die ouder dan de opgegeven bewaarperiode zijn. Zie voor meer informatie, [back-up inschakelen voor Azure Stack met PowerShell](../azure-stack-backup-enable-backup-powershell.md).

- <!-- 2496385 | ASDK, IS -->  **Toegevoegde tijd in de totale back-uptijd voor gegevensoverdracht.** Zie voor meer informatie, [back-up inschakelen voor Azure Stack met PowerShell](../azure-stack-backup-enable-backup-powershell.md).

-   <!-- 1702130 | ASDK, IS -->  **Back-up externe capaciteit wordt nu de juiste capaciteit van de externe share.** (Dit was eerder programmeren tot 10 GB.) Zie voor meer informatie, [back-up inschakelen voor Azure Stack met PowerShell](../azure-stack-backup-enable-backup-powershell.md).
 
- <!-- 2753130 |  IS, ASDK   -->  **Azure Resource Manager-sjablonen bieden nu ondersteuning voor het element voorwaarde** -u kunt nu een resource in een Azure Resource Manager-sjabloon met behulp van een voorwaarde implementeren. U kunt de sjabloon voor het implementeren van een resource op basis van een voorwaarde, zoals het evalueren van als een parameterwaarde bevindt ontwerpen. Zie voor meer informatie over het gebruik van een sjabloon als een voorwaarde [een resource voorwaardelijk implementeren](https://docs.microsoft.com/azure/architecture/building-blocks/extending-templates/conditional-deploy) en [sectie met variabelen van Azure Resource Manager-sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-templates-variables) in de documentatie van Azure. 

   U kunt ook sjablonen [resources implementeren op meer dan één abonnement of resourcegroep](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-cross-resource-group-deployment).  

- <!--2753073 | IS, ASDK -->  **Ondersteuning voor de versie van de Microsoft.COMPUTE API-resource is bijgewerkt** ondersteuning voor API-versie 2017-10-01, 2015-06-15 voor Azure Stack-netwerkbronnen.  Ondersteuning voor resource-versies tussen 10-01-2017 en 15-06-2015 is niet opgenomen in deze release, maar worden opgenomen in een toekomstige release.  Raadpleeg [overwegingen voor Azure Stack-netwerken](../user/azure-stack-network-differences.md) voor functionaliteit verschillen.

- <!-- 2272116 | IS, ASDK   -->  **Azure Stack is ondersteuning toegevoegd voor omgekeerde DNS-zoekacties voor dergelijke infrastructuur-eindpunten voor Azure Stack** (dat is voor de portal, adminportal, beheer en adminmanagement). Hiermee kunt Azure Stack Extern eindpunt namen worden omgezet vanaf een IP-adres.

- <!-- 2780899 |  IS, ASDK   --> **Azure Stack biedt nu ondersteuning voor extra netwerkinterfaces toevoegen aan een bestaande virtuele machine.**  Deze functionaliteit is beschikbaar via de portal, PowerShell en CLI. Zie voor meer informatie, [toevoegen of verwijderen van netwerkinterfaces](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface-vm) in de documentatie van Azure. 

- <!-- 2222444 | IS, ASDK   -->  **Verbeteringen in de nauwkeurigheid en zeer flexibel zijn aangebracht aan netwerken gebruik meters**. Netwerk gebruik meters zijn nu meer nauwkeurige en houden rekening onderbroken abonnementen, perioden van de serviceonderbreking en racevoorwaarden.

- <!-- 2297790 | IS, ASDK -->  **Verbeteringen in de Azure Stack-syslog-client (preview-functie)**. Deze client kunt het doorsturen van controle en logboeken met betrekking tot de Azure Stack-infrastructuur naar een syslog-server of security information en event management (SIEM) software buiten Azure Stack. De syslog-client ondersteunt nu het TCP-protocol met tekst zonder opmaak of TLS 1.2-versleuteling, de laatste is de standaardconfiguratie. U kunt de TLS-verbinding configureren met alleen-server of wederzijdse verificatie.

<!-- 2297790 - IS, ASDK --> 
- **Azure Stack bevat nu een *Syslog* client** als een *preview-functie*. Deze client kunt het doorsturen van Logboeken voor controle en beveiliging met betrekking tot de Azure Stack-infrastructuur naar een Syslog-server of security information en event management (SIEM) software die zich buiten Azure Stack. De Syslog-client ondersteunt momenteel alleen niet-geverifieerde UDP-verbindingen via standaardpoort 514. De nettolading van elk Syslog-bericht is opgemaakt in Common Event Format (CEF).

  Met deze Preview-versie, kunt u een veel groter aantal controles en waarschuwingen zien. 

  Omdat deze functie nog in preview is, afhankelijk u niet van het in een productieomgeving.

  Zie voor meer informatie, [Azure Stack syslog doorsturen](../azure-stack-integrate-security.md).

- <!-- ####### | IS, ASDK -->  **Azure Resource Manager bevat de regionaam van de.** Met deze release bevat objecten die zijn opgehaald van de Azure Resource Manager nu het naamkenmerk regio. Als een bestaande PowerShell-script wordt het object rechtstreeks aan een andere cmdlet doorgegeven, kan het script een foutmelding weergegeven en mislukken. Dit gedrag van Azure Resource Manager-compatibel is en vereist dat de aanroepende client om af te trekken van het kenmerk regio. Voor meer informatie over de Azure Resource Manager-Zie [Azure Resource Manager-documentatie](https://docs.microsoft.com/azure/azure-resource-manager/).

- <!-- TBD | IS, ASDK -->  **Abonnementen verplaatsen tussen gedelegeerde Providers.** U kunt abonnementen nu verplaatsen tussen abonnementen van nieuwe of bestaande Provider overgedragen die deel uitmaken van dezelfde Directory-tenant. Abonnementen die behoren tot het abonnement van de Provider standaard kunnen ook worden verplaatst naar de Provider overgedragen abonnementen in dezelfde Directory-tenant. Zie voor meer informatie [aanbiedingen in Azure Stack delegeren](../azure-stack-delegated-provider.md).
 
- <!-- 2536808 IS ASDK --> **Aanmaaktijd van de virtuele machine verbeterd** voor virtuele machines die zijn gemaakt met de installatiekopieën die u vanuit de Azure marketplace downloaden.

### <a name="fixed-issues"></a>Problemen opgelost

- <!-- TBD | ASDK, IS --> Er zijn verschillende verbeteringen aangebracht in het updateproces zodat deze betrouwbaarder. Bovendien zijn verbeteringen aangebracht aan onderliggende infrastructuur, wat zorgt voor betere knooppunt drain, waardoor de mogelijke downtime voor workloads tijdens het bijwerken worden geminimaliseerd.

-   <!--2292271 | ASDK, IS --> We een probleem opgelost waarbij een gewijzigde quotumlimiet is niet van toepassing op bestaande abonnementen.  Nu, wanneer u een quotumlimiet voor een netwerkbron die deel uitmaakt van een aanbieding verhogen en Plan dat is gekoppeld aan een gebruikersabonnement, de nieuwe limiet van toepassing op de bestaande abonnementen, evenals de nieuwe abonnementen.

- <!-- 2448955 | IS ASDK --> U kunt nu activiteitenlogboeken voor systemen die zijn geïmplementeerd in een tijdzone UTC + N is opvragen.    

- <!-- 2319627 |  ASDK, IS --> Controle vooraf voor de back-upconfiguratie parameters (pad/gebruikersnaam/wachtwoord/Encryption Key) wordt niet langer onjuiste instellingen ingesteld op de back-upconfiguratie. (Voorheen onjuiste instellingen zijn ingesteld in de back-up en back-up, klikt u vervolgens mislukken wanneer ze worden geactiveerd.)

- <!-- 2215948 |  ASDK, IS --> De lijst met back-up wordt nu vernieuwd wanneer u de back-up handmatig van de externe share verwijderen.

- <!-- 2360715 |  ASDK, IS -->  Bij het instellen van datacenter-integratie, u geen toegang meer tot de AD FS-bestand met metagegevens van een share. Zie voor meer informatie, [AD FS-integratie instellen door op te geven van bestand met federatieve metagegevens](../azure-stack-integrate-identity.md#setting-up-ad-fs-integration-by-providing-federation-metadata-file). 

- <!-- 2388980 | ASDK, IS --> We een probleem opgelost dat gebruikers van een bestaande openbare IP-adres toegewezen die was eerder zijn toegewezen aan een netwerkinterface of Load Balancer met een nieuwe netwerkinterface of Load Balancer.  

- <!-- 2551834 - IS, ASDK --> Wanneer u selecteert *overzicht* voor een opslagaccount in de beheerder of de gebruiker-portals, de *Essentials* deelvenster nu correct wordt weergegeven de verwachte gegevens. 

- <!-- 2551834 - IS, ASDK --> Wanneer u selecteert *Tags* voor een opslagaccount in de beheerder of de gebruiker-portals, nu de gegevens correct wordt weergegeven.

- <!-- TBD - IS ASDK --> Deze versie van Azure Stack kunt u het probleem waardoor de toepassing van updates voor stuurprogramma's van OEM-extensiepakketten.

-   <!-- 2055809- IS ASDK --> We vastgesteld dat een probleem dat u niet het verwijderen van virtuele machines op de blade compute wanneer de virtuele machine kan niet worden gemaakt.  

- <!--  2643962 IS ASDK -->  De waarschuwing voor *geheugencapaciteit van lage* langer ten onrechte wordt weergegeven.

<!--  TBD ASDK --> 
- De virtuele machine die als host fungeert voor het eindpunt van de bevoegdheden (PEP) is verhoogd tot 4GB. In de ASDK heet deze virtuele machine AzS-ERCS01.

- **Verschillende oplossingen** voor prestaties, stabiliteit, beveiliging en het besturingssysteem dat wordt gebruikt door Azure Stack


<!-- ### Changes  -->
<!--   ### Additional releases timed with this update  -->


### <a name="known-issues"></a>Bekende problemen

#### <a name="portal"></a>Portal
<!-- 2931230 – IS  ASDK --> 
- Abonnementen die zijn toegevoegd aan een gebruikersabonnement als een aanvullende plan kunnen niet worden verwijderd, zelfs wanneer u het abonnement uit het gebruikersabonnement verwijderen. Het abonnement blijft totdat de abonnementen die verwijzen naar het aanvullende plan worden ook verwijderd. 

<!-- 2551834 - IS, ASDK --> 
- Wanneer u selecteert **overzicht** voor een opslagaccount in de beheerder of de gebruiker-portals, de gegevens uit de *Essentials* deelvenster niet wordt weergegeven.  Het deelvenster Essentials geeft informatie weer over het account, zoals de *resourcegroep*, *locatie*, en *abonnements-ID*.  Andere opties voor overzicht zijn beschikbaar zijn, zoals *Services* en *bewaking*, ook als opties voor *openen in Explorer* of *storage-account verwijderen* .  

  Als u de informatie niet beschikbaar is, gebruikt u de [Get-azureRMstorageaccount](https://docs.microsoft.com/powershell/module/azurerm.storage/get-azurermstorageaccount?view=azurermps-6.2.0) PowerShell-cmdlet.

<!-- 2551834 - IS, ASDK --> 
- Wanneer u selecteert **Tags** voor een opslagaccount in de beheerder of de gebruiker-portals, de gegevens te laden is mislukt en niet wordt weergegeven.  

  Als u de informatie niet beschikbaar is, gebruikt u de [Get-AzureRmTag](https://docs.microsoft.com/powershell/module/azurerm.tags/get-azurermtag?view=azurermps-6.2.0) PowerShell-cmdlet.

<!-- TBD - IS ASDK --> 
- Gebruik niet de nieuwe administratieve abonnementstypen van *softwarelicentiecontrole abonnement*, en *verbruik abonnement*. Deze nieuwe abonnementstypen zijn geïntroduceerd in versie 1804 maar nog niet klaar voor gebruik. U moet echter ook doorgaan met de *Provider standaard* abonnementstype.
  
<!-- TBD - IS ASDK --> 
- U kunt geen updates voor stuurprogramma's toepassen met behulp van een OEM-uitbreidingspakket met deze versie van Azure Stack.  Er is geen oplossing voor dit probleem.
 
<!-- TBD - IS ASDK --> 
- De mogelijkheid [een nieuwe ondersteuningsaanvraag openen in de vervolgkeuzelijst](../azure-stack-manage-portals.md#quick-access-to-help-and-support) uit binnen de beheerder van de portal is niet beschikbaar. In plaats daarvan gebruikt u de volgende koppeling:     
    - Gebruik voor Azure Stack Development Kit https://aka.ms/azurestackforum.    

<!-- 2403291 - IS ASDK --> 
- Hebt u mogelijk geen gebruik van de horizontale schuifbalk langs de onderkant van de beheerder en gebruiker portals. Als u geen toegang de horizontale schuifbalk tot, blijven gebruiken de breadcrumbs om naar een vorige blade in de portal navigeren zijn door de naam van de blade te selecteren dat u wilt weergeven in de breadcrumb-lijst gevonden aan de bovenkant van de portal.
  ![Breadcrumb](media/asdk-release-notes/breadcrumb.png)

<!-- TBD -  IS ASDK --> 
- Verwijderen van zwevende resources leidt van gebruiker-abonnementen. Als tijdelijke oplossing, eerst Gebruikersbronnen of de hele resourcegroep verwijderen en verwijder vervolgens gebruikersabonnementen.

<!-- TBD -  IS ASDK --> 
- U kunt machtigingen aan uw abonnement met behulp van de Azure Stack-portals niet weergeven. Als tijdelijke oplossing, moet u PowerShell gebruiken om machtigingen te controleren.

- <!--  TBD | ASDK -->  De standaardtijdzone voor uw Azure Stack-implementatie wordt nu ophalen ingesteld op UTC. U kunt een tijdzone selecteren bij het installeren van Azure Stack, maar deze wordt automatisch hersteld naar UTC als standaardwaarde tijdens de installatie.

#### <a name="health-and-monitoring"></a>Status en bewaking
<!-- 1264761 - IS ASDK -->  
- Mogelijk ziet u waarschuwingen voor de *Health controller* onderdeel waarvoor u de volgende gegevens:  

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

<!-- 2368581 - IS. ASDK --> 
- Azure Stack-operators, als u een waarschuwing voor een beperkte hoeveelheid geheugen ontvangt en virtuele machines van tenants niet te implementeren met een *fout bij het maken van infrastructuur-VM*, is het mogelijk dat de Azure Stack-stempel heeft te weinig geheugen beschikbaar. Gebruik de [Azure Stack Capacity Planner](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) naar het beste informatie over de beschikbare capaciteit voor uw workloads.

<!-- TBD - IS. ASDK --> 
- Bij het uitvoeren van de cmdlet Test-AzureStack op het eindpunt van de bevoegdheden (PEP), wordt een bericht waarschuwen voor de VM ERCS gegenereerd. U kunt echter ook doorgaan met de ASDK. 


#### <a name="compute"></a>Compute
<!-- TBD - IS, ASDK --> 
- Bij het selecteren van een VM-grootte voor de implementatie van een virtuele machine, sommige F-serie VM-grootten zijn niet zichtbaar als onderdeel van de grootte selector bij het maken van een virtuele machine. De volgende VM-grootten worden niet weergegeven in de kiezer: *F8s_v2*, *F16s_v2*, *F32s_v2*, en *F64s_v2*.  
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


<!-- TBD -  IS ASDK --> 
- Instellingen voor vergroten/verkleinen voor schaalsets voor virtuele machines zijn niet beschikbaar in de portal. Als tijdelijke oplossing, kunt u [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Vanwege de verschillen tussen versies van PowerShell, moet u de `-Name` parameter in plaats van `-VMScaleSetName`.

<!-- TBD -  IS ASDK --> 
- Wanneer u virtuele machines op de gebruikersportal van Azure Stack maakt, wordt een onjuist aantal gegevensschijven dat een virtuele machine uit de D-serie kunt koppelen door de portal weergegeven. Alle ondersteunde D-serie VM's kan zo veel gegevensschijven bevatten als de Azure-configuratie.

<!-- TBD -  IS ASDK --> 
- Wanneer een VM-installatiekopie niet kan worden gemaakt, kan een mislukte-item dat u niet verwijderen worden toegevoegd aan de VM-installatiekopieën compute-blade.

  Als tijdelijke oplossing, kunt u een nieuwe VM-installatiekopie maken met een dummy VHD dat kan worden gemaakt via Hyper-V (New-VHD-pad C:\dummy.vhd-vaste - SizeBytes 1 GB). Dit proces los het probleem dat verhindert dat het mislukte item wordt verwijderd. Vervolgens, 15 minuten na het maken van de installatiekopie van het pop, u kunt met succes verwijderen.

  U kunt vervolgens opnieuw proberen het downloaden van de VM-installatiekopie die eerder is mislukt.

<!-- TBD -  IS ASDK --> 
- Als een uitbreiding op een VM-implementatie de inrichting te lang duurt, laat gebruikers de time-out van de inrichting in plaats van bij stoppen van de procedure voor de toewijzing ongedaan of verwijder de virtuele machine.  

<!-- 1662991 - IS ASDK --> 
- Diagnostische gegevens over Linux-VM wordt niet ondersteund in Azure Stack. Wanneer u een Linux-VM met VM diagnostics is ingeschakeld implementeren, wordt de implementatie mislukt. De implementatie mislukt ook als u de Linux-VM eenvoudige metrische gegevens via diagnostische instellingen inschakelen.

#### <a name="networking"></a>Netwerken
<!-- TBD - IS ASDK --> 
- U kan geen gebruiker gedefinieerde routes maken in de portal-beheerder of gebruiker. Als tijdelijke oplossing gebruiken [Azure PowerShell](https://docs.microsoft.com/azure/virtual-network/tutorial-create-route-table-powershell).

<!-- 1766332 - IS, ASDK --> 
- Onder **netwerken**, als u klikt op **VPN-Gateway maken** voor het instellen van een VPN-verbinding, **op basis van beleid** wordt vermeld als een VPN-type. Selecteer deze optie niet. Alleen de **Route op basis van** optie wordt ondersteund in Azure Stack.

<!-- 2388980 -  IS ASDK --> 
- Nadat een virtuele machine is gemaakt en die zijn gekoppeld aan een openbaar IP-adres, kunt u deze VM op basis van IP-adres kan niet loskoppelen. Ontkoppeling lijkt te werken, maar het eerder toegewezen openbare IP-adres blijft gekoppeld aan de oorspronkelijke virtuele machine.

  Op dit moment moet u alleen nieuwe openbare IP-adressen voor nieuwe virtuele machines die u maakt.

  Dit gebeurt zelfs als u het IP-adres aan een nieuwe virtuele machine opnieuw toewijzen (vaak aangeduid als een *wisselen van VIP*). Alle toekomstige probeert verbinding maken via dit resultaat van de IP-adres in een verbinding met de oorspronkelijke virtuele machine en niet naar de nieuwe vertakking.

<!-- 2292271 - IS ASDK --> 
- Als u een quotumlimiet voor een netwerkbron die deel uitmaakt van een aanbieding en een Plan dat is gekoppeld aan een tenantabonnement verhoogt, wordt de nieuwe limiet niet toegepast op dat aan het abonnement. De nieuwe limiet is echter van toepassing op nieuwe abonnementen die zijn gemaakt nadat het quotum is verhoogd.

  U kunt dit probleem omzeilen, door een aanvullende plan te gebruiken voor het verhogen van een quotum op het netwerk wanneer het abonnement al gekoppeld aan een abonnement is. Zie voor meer informatie over het [beschikbaar maken voor een aanvullende plan](../azure-stack-subscribe-plan-provision-vm.md#to-make-an-add-on-plan-available).

<!-- 2304134 IS ASDK --> 
- U kunt een abonnement met DNS-Zone resources of de routetabel-resources die zijn gekoppeld aan het niet verwijderen. Als u wilt het abonnement is verwijderd, moet u eerst de DNS-Zone en routetabel resources uit het tenantabonnement verwijderen.

<!-- 1902460 -  IS ASDK --> 
- Azure Stack biedt ondersteuning voor een enkel *lokale netwerkgateway* per IP-adres. Dit geldt voor alle tenant-abonnementen. Nadat het maken van de eerste lokale gateway netwerkverbinding, volgende pogingen tot het maken van een resource van een lokale netwerkgateway met hetzelfde IP-adres worden geblokkeerd.

<!-- 16309153 -  IS ASDK --> 
- In een Virtueelnetwerk dat is gemaakt met een DNS-Server-instelling van *automatische*, wijzigen naar een aangepaste DNS-Server mislukt. De bijgewerkte instellingen worden niet gepusht naar virtuele machines in dit Vnet.

<!-- TBD -  IS ASDK --> 
- Azure Stack biedt geen ondersteuning voor extra netwerkinterfaces toevoegen aan een VM-exemplaar, nadat de virtuele machine is geïmplementeerd. Als de virtuele machine meer dan één netwerkinterface vereist, moeten ze worden gedefinieerd tijdens de implementatie.


#### <a name="sql-and-mysql"></a>SQL- en MySQL
<!-- TBD - ASDK --> 
- De database die als host fungeert servers moet worden toegewezen voor gebruik door de resource-provider en gebruiker werkbelastingen. U kunt een exemplaar dat wordt gebruikt door andere consumenten, met inbegrip van App Services niet gebruiken.

<!-- IS, ASDK --> 
- Speciale tekens, inclusief spaties en perioden, worden niet ondersteund in de **familie** naam wanneer u een SKU voor de SQL- en MySQL-resourceproviders.

#### <a name="app-service"></a>App Service
<!-- 2352906 - IS ASDK --> 
- Gebruikers moeten de storage resourceprovider registreren voordat ze hun eerste Azure-functie in het abonnement maken.

<!-- TBD - IS ASDK --> 
- Als u wilt de infrastructuur (werknemers, beheer, front-rollen) uitbreiden, moet u PowerShell gebruiken, zoals beschreven in de opmerkingen bij de release voor Compute.  

<!-- TBD - IS ASDK --> 
- App Service kan alleen worden geïmplementeerd in de *standaard providerabonnement* op dit moment. <!-- In a future update, App Service will deploy into the new *Metering subscription* that was introduced in Azure Stack 1804. When Metering is supported for use, all existing deployments will be migrated to this new subscription type. -->

#### <a name="usage"></a>Gebruik  
<!-- TBD -  IS ASDK --> 
- Gebruik openbare IP-adres-gebruiksgegevens meter toont dezelfde *datum/tijd evenement* waarde voor elke record in plaats van de *TimeDate* stempel waarin wordt weergegeven wanneer de record is gemaakt. U kunt deze gegevens op dit moment niet gebruiken om uit te voeren nauwkeurige accounting van gebruik van openbare IP-adres.

<!-- #### Identity -->
