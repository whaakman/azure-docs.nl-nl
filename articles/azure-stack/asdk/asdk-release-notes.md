---
title: Opmerkingen bij de release van Microsoft Azure Stack Development Kit | Microsoft Docs
description: Verbeteringen, correcties en bekende problemen voor Azure Stack Development Kit.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2018
ms.author: brenduns
ms.reviewer: misainat
ms.openlocfilehash: 2f732dfdfe9bf4aff2753114c3041f8f646421c2
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39344729"
---
# <a name="azure-stack-development-kit-release-notes"></a>Releaseopmerkingen voor Azure Stack Development Kit
Deze releaseopmerkingen bevatten informatie over verbeteringen, correcties en bekende problemen in Azure Stack Development Kit. Als u niet zeker weet welke versie u uitvoert, kunt u [de portal gebruiken om te controleren](.\.\azure-stack-updates.md#determine-the-current-version).

> De hoogte blijven van wat is er nieuw in de ASDK Abonneer u op de [ ![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#) [feed](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#).

## <a name="build-11805142"></a>Build 1.1805.1.42

<!-- ### New features 
This build includes the following improvements and fixes for Azure Stack.  
-->


### <a name="fixed-issues"></a>Opgeloste problemen

- **Verschillende oplossingen** voor prestaties, stabiliteit, beveiliging en het besturingssysteem dat wordt gebruikt door Azure Stack


<!-- ### Changes  --> 


<!--   ### Additional releases timed with this update  -->


### <a name="known-issues"></a>Bekende problemen
 
#### <a name="portal"></a>Portal
- <!-- TBD - IS ASDK --> U kunt geen updates voor stuurprogramma's toepassen met behulp van een OEM-uitbreidingspakket met deze versie van Azure Stack.  Er is geen oplossing voor dit probleem.
 
- <!-- TBD - IS ASDK --> Gebruik niet de nieuwe administratieve abonnementstypen van *softwarelicentiecontrole abonnement*, en *verbruik abonnement*. Deze nieuwe abonnementstypen zijn geïntroduceerd in versie 1804 maar nog niet klaar voor gebruik. U moet echter ook doorgaan met de *Provider standaard* abonnementstype.  

- <!-- 2403291 - IS ASDK --> Hebt u mogelijk geen gebruik van de horizontale schuifbalk langs de onderkant van de beheerder en gebruiker portals. Als u geen toegang de horizontale schuifbalk tot, blijven gebruiken de breadcrumbs om naar een vorige blade in de portal navigeren zijn door de naam van de blade te selecteren dat u wilt weergeven in de breadcrumb-lijst gevonden aan de bovenkant van de portal.
  ![Breadcrumb](media/asdk-release-notes/breadcrumb.png)

- <!-- TBD -  IS ASDK --> Verwijderen van zwevende resources leidt van gebruiker-abonnementen. Als tijdelijke oplossing, eerst Gebruikersbronnen of de hele resourcegroep verwijderen en verwijder vervolgens gebruikersabonnementen.

- <!-- TBD -  IS ASDK --> U kunt machtigingen aan uw abonnement met behulp van de Azure Stack-portals niet weergeven. Als tijdelijke oplossing, moet u PowerShell gebruiken om machtigingen te controleren.


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

#### <a name="compute"></a>Compute
- <!-- TBD -  IS ASDK --> Instellingen voor vergroten/verkleinen voor schaalsets voor virtuele machines zijn niet beschikbaar in de portal. Als tijdelijke oplossing, kunt u [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Vanwege de verschillen tussen versies van PowerShell, moet u de `-Name` parameter in plaats van `-VMScaleSetName`.

- <!-- TBD -  IS ASDK --> Wanneer u virtuele machines op de gebruikersportal van Azure Stack maakt, wordt een onjuist aantal gegevensschijven dat een virtuele machine uit de D-serie kunt koppelen door de portal weergegeven. Alle ondersteunde D-serie VM's kan zo veel gegevensschijven bevatten als de Azure-configuratie.

- <!-- TBD -  IS ASDK --> Wanneer een VM-installatiekopie niet kan worden gemaakt, kan een mislukte-item dat u niet verwijderen worden toegevoegd aan de VM-installatiekopieën compute-blade.

  Als tijdelijke oplossing, kunt u een nieuwe VM-installatiekopie maken met een dummy VHD dat kan worden gemaakt via Hyper-V (New-VHD-pad C:\dummy.vhd-vaste - SizeBytes 1 GB). Dit proces los het probleem dat verhindert dat het mislukte item wordt verwijderd. Vervolgens, 15 minuten na het maken van de installatiekopie van het pop, u kunt met succes verwijderen.

  U kunt vervolgens redownload van de VM-installatiekopie die eerder is mislukt.

- <!-- TBD -  IS ASDK --> Als een uitbreiding op een VM-implementatie de inrichting te lang duurt, laat gebruikers de time-out van de inrichting in plaats van bij stoppen van de procedure voor de toewijzing ongedaan of verwijder de virtuele machine.  

- <!-- 1662991 - IS ASDK --> Diagnostische gegevens over Linux-VM wordt niet ondersteund in Azure Stack. Wanneer u een Linux-VM met VM diagnostics is ingeschakeld implementeren, wordt de implementatie mislukt. De implementatie mislukt ook als u de Linux-VM eenvoudige metrische gegevens via diagnostische instellingen inschakelen. 

#### <a name="networking"></a>Netwerken
- <!-- 1766332 - IS, ASDK --> Onder **netwerken**, als u klikt op **VPN-Gateway maken** voor het instellen van een VPN-verbinding, **op basis van beleid** wordt vermeld als een VPN-type. Selecteer deze optie niet. Alleen de **Route op basis van** optie wordt ondersteund in Azure Stack.

- <!-- 2388980 -  IS ASDK --> Nadat een virtuele machine is gemaakt en die zijn gekoppeld aan een openbaar IP-adres, kunt u deze VM op basis van IP-adres kan niet loskoppelen. Ontkoppeling lijkt te werken, maar het eerder toegewezen openbare IP-adres blijft gekoppeld aan de oorspronkelijke virtuele machine.

  Op dit moment moet u alleen nieuwe openbare IP-adressen voor nieuwe virtuele machines die u maakt.

  Dit gebeurt zelfs als u het IP-adres aan een nieuwe virtuele machine opnieuw toewijzen (vaak aangeduid als een *wisselen van VIP*). Alle toekomstige probeert verbinding maken via dit resultaat van de IP-adres in een verbinding naar de oorspronkelijk gekoppelde virtuele machine en niet naar het nieuwe certificaat.

- <!-- 2292271 - IS ASDK --> Als u een quotumlimiet voor een netwerkbron die deel uitmaakt van een aanbieding en een Plan dat is gekoppeld aan een tenantabonnement verhoogt, wordt de nieuwe limiet niet toegepast op dat aan het abonnement. De nieuwe limiet is echter van toepassing op nieuwe abonnementen die zijn gemaakt nadat het quotum is verhoogd. 

  U kunt dit probleem omzeilen, door een aanvullende plan te gebruiken voor het verhogen van een quotum op het netwerk wanneer het abonnement al gekoppeld aan een abonnement is. Zie voor meer informatie over het [beschikbaar maken voor een aanvullende plan](.\.\azure-stack-subscribe-plan-provision-vm.md#to-make-an-add-on-plan-available).

- <!-- 2304134 IS ASDK --> U kunt een abonnement met DNS-Zone resources of de routetabel-resources die zijn gekoppeld aan het niet verwijderen. Als u wilt het abonnement is verwijderd, moet u eerst de DNS-Zone en routetabel resources uit het tenantabonnement verwijderen. 


- <!-- 1902460 -  IS ASDK --> Azure Stack biedt ondersteuning voor een enkel *lokale netwerkgateway* per IP-adres. Dit geldt voor alle tenant-abonnementen. Nadat het maken van de eerste lokale gateway netwerkverbinding, volgende pogingen tot het maken van een resource van een lokale netwerkgateway met hetzelfde IP-adres worden geblokkeerd.

- <!-- 16309153 -  IS ASDK --> In een Virtueelnetwerk dat is gemaakt met een DNS-Server-instelling van *automatische*, wijzigen naar een aangepaste DNS-Server mislukt. De bijgewerkte instellingen worden niet gepusht naar virtuele machines in dit Vnet.
 
- <!-- TBD -  IS ASDK --> Azure Stack biedt geen ondersteuning voor extra netwerkinterfaces toevoegen aan een VM-exemplaar, nadat de virtuele machine is geïmplementeerd. Als de virtuele machine meer dan één netwerkinterface vereist, moeten ze worden gedefinieerd tijdens de implementatie.


#### <a name="sql-and-mysql"></a>SQL- en MySQL 
- <!-- TBD - ASDK --> De database die als host fungeert servers moet worden toegewezen voor gebruik door de resource-provider en gebruiker werkbelastingen. U kunt een exemplaar dat wordt gebruikt door andere consumenten, met inbegrip van App Services niet gebruiken.

- <!-- IS, ASDK --> Speciale tekens, inclusief spaties en perioden, worden niet ondersteund in de **familie** naam wanneer u een SKU voor de SQL- en MySQL-resourceproviders. 

#### <a name="app-service"></a>App Service
- <!-- 2352906 - IS ASDK --> Gebruikers moeten de storage resourceprovider registreren voordat ze hun eerste Azure-functie in het abonnement maken.

- <!-- TBD - IS ASDK --> Als u wilt de infrastructuur (werknemers, beheer, front-rollen) uitbreiden, moet u PowerShell gebruiken, zoals beschreven in de opmerkingen bij de release voor Compute.  

- <!-- TBD - IS ASDK --> App Service kan alleen worden geïmplementeerd in de *standaard providerabonnement* op dit moment. In een toekomstige update App Service wordt geïmplementeerd in de nieuwe *softwarelicentiecontrole abonnement* die is geïntroduceerd in Azure Stack 1804. Wanneer de meting wordt ondersteund voor gebruik, worden alle bestaande implementaties worden gemigreerd naar dit nieuwe abonnementstype.

#### <a name="usage"></a>Gebruik  
- <!-- TBD -  IS ASDK --> Gebruik openbare IP-adres-gebruiksgegevens meter toont dezelfde *datum/tijd evenement* waarde voor elke record in plaats van de *TimeDate* stempel waarin wordt weergegeven wanneer de record is gemaakt. U kunt deze gegevens op dit moment niet gebruiken om uit te voeren nauwkeurige accounting van gebruik van openbare IP-adres.

<!-- #### Identity -->



## <a name="build-201805131"></a>Build 20180513.1

### <a name="new-features"></a>Nieuwe functies 
Deze versie bevat de volgende verbeteringen en oplossingen voor Azure Stack.  

- <!-- 1759172 - IS, ASDK --> **Nieuwe administratieve abonnementen**. Met 1804 zijn twee typen voor nieuwe abonnementen beschikbaar in de portal. Deze nieuwe abonnementstypen zijn naast de standaard providerabonnement en zichtbaar met nieuwe Azure Stack-installaties vanaf versie 1804. *Gebruik deze nieuwe abonnementstypen niet met deze versie van Azure Stack*. We kondigen de beschikbaarheid voor het gebruik van deze abonnementstypen aan met een toekomstige update. 

  Deze nieuwe abonnementstypen zijn zichtbaar, maar deel uitmaakt van een grotere wijziging voor het beveiligen van de Provider standaard-abonnement en zodat u gemakkelijk gedeelde bronnen, zoals het hosten van de SQL-servers implementeren. 

  De abonnementstypen drie nu beschikbaar zijn:  
  - Standaard providerabonnement: echter ook doorgaan met dit abonnementstype. 
  - Betaling van abonnement: *gebruik niet het type van dit abonnement.*
  - Gebruik-abonnement: *gebruik geen dit abonnementstype*

### <a name="fixed-issues"></a>Opgeloste problemen
- <!-- IS, ASDK -->  In de beheerportal hebt niet meer tot de tegel Update vernieuwen voordat wordt informatie weergegeven. 

- <!-- 2050709 - IS, ASDK -->  U kunt nu de admin-portal gebruiken om te bewerken van metrische opslaggegevens voor Blob-service, tabelservice en Queue-service.

- <!-- IS, ASDK --> Onder **netwerken**, wanneer u klikt op **verbinding** voor het instellen van een VPN-verbinding, **Site-naar-site (IPsec)** is nu de enige beschikbare optie. 

- **Verschillende oplossingen** voor prestaties, stabiliteit, beveiliging en het besturingssysteem dat wordt gebruikt door Azure Stack

<!-- ### Changes  --> 
### <a name="additional-releases-timed-with-this-update"></a>Als u meer versies is een time-out opgetreden met deze update  
De volgende zijn nu beschikbaar, maar geen Azure Stack update 1804 vereist.
- **Bijwerken naar de Microsoft Azure Stack System Center Operations Manager-Bewakingspakket**. Een nieuwe versie (1.0.3.0) van de Microsoft System Center Operations Manager Monitoring Pack for Azure Stack is beschikbaar voor [downloaden](https://www.microsoft.com/download/details.aspx?id=55184). U kunt met deze versie, Service-Principals gebruiken wanneer u een verbonden Azure Stack-implementatie toevoegen. Deze versie biedt ook een Update Management-ervaring waarmee u de herstelactie rechtstreeks vanuit in Operations Manager. Er zijn ook nieuwe dashboards die weergeven van resourceproviders, schaaleenheden en schalen van knooppunten van de eenheid.

- **Nieuwe Azure Stack-Admin PowerShell-versie 1.3.0**.  PowerShell voor Azure Stack 1.3.0 is nu beschikbaar voor installatie. Deze versie bevat opdrachten voor alle beheer-resourceproviders voor het beheren van Azure Stack.  Met deze release deel van de inhoud van de Azure Stack-hulpprogramma's GitHub wordt afgeschaft [opslagplaats](https://github.com/Azure/AzureStack-Tools). 

   Voor installatie-informatie, volgt u de [instructies](.\.\azure-stack-powershell-install.md) of de [help](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.3.0) inhoud voor Azure Stack-Module 1.3.0. 

- **Voor de eerste release van Azure Stack API-verwijzing voor Rest**. De [API-naslaginformatie voor alle resourceproviders op Azure Stack Admin](https://docs.microsoft.com/rest/api/azure-stack/) nu wordt gepubliceerd. 

### <a name="known-issues"></a>Bekende problemen
 
#### <a name="portal"></a>Portal
- <!-- TBD - IS ASDK --> U kunt geen updates voor stuurprogramma's toepassen met behulp van een OEM-uitbreidingspakket met deze versie van Azure Stack.  Er is geen oplossing voor dit probleem.
 
- <!-- TBD - IS ASDK --> De mogelijkheid [een nieuwe ondersteuningsaanvraag openen in de vervolgkeuzelijst](.\.\azure-stack-manage-portals.md#quick-access-to-help-and-support) uit binnen de beheerder van de portal is niet beschikbaar. In plaats daarvan gebruikt u de volgende koppeling:     
    - Gebruik voor Azure Stack Development Kit https://aka.ms/azurestackforum.    

- <!-- 2403291 - IS ASDK --> Hebt u mogelijk geen gebruik van de horizontale schuifbalk langs de onderkant van de beheerder en gebruiker portals. Als u geen toegang de horizontale schuifbalk tot, blijven gebruiken de breadcrumbs om naar een vorige blade in de portal navigeren zijn door de naam van de blade te selecteren dat u wilt weergeven in de breadcrumb-lijst gevonden aan de bovenkant van de portal.
  ![Breadcrumb](media/asdk-release-notes/breadcrumb.png)

- <!-- TBD -  IS ASDK --> Verwijderen van zwevende resources leidt van gebruiker-abonnementen. Als tijdelijke oplossing, eerst Gebruikersbronnen of de hele resourcegroep verwijderen en verwijder vervolgens gebruikersabonnementen.

- <!-- TBD -  IS ASDK --> U kunt machtigingen aan uw abonnement met behulp van de Azure Stack-portals niet weergeven. Als tijdelijke oplossing, moet u PowerShell gebruiken om machtigingen te controleren.

-   <!-- TBD -  IS ASDK --> In de beheerportal ziet u mogelijk een kritieke waarschuwing voor het onderdeel Microsoft.Update.Admin. De naam van de waarschuwing, beschrijving en herstel alle als weergegeven:  
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
- <!-- TBD -  IS ASDK --> Instellingen voor vergroten/verkleinen voor schaalsets voor virtuele machines zijn niet beschikbaar in de portal. Als tijdelijke oplossing, kunt u [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Vanwege de verschillen tussen versies van PowerShell, moet u de `-Name` parameter in plaats van `-VMScaleSetName`.

- <!-- TBD -  IS ASDK --> Wanneer u virtuele machines op de gebruikersportal van Azure Stack maakt, wordt een onjuist aantal gegevensschijven die aan een virtuele machine uit de DS-serie kunt koppelen door de portal weergegeven. DS-serie VM's kan zo veel gegevensschijven bevatten als de Azure-configuratie.

- <!-- TBD -  IS ASDK --> Wanneer een VM-installatiekopie niet kan worden gemaakt, kan een mislukte-item dat u niet verwijderen worden toegevoegd aan de VM-installatiekopieën compute-blade.

  Als tijdelijke oplossing, kunt u een nieuwe VM-installatiekopie maken met een dummy VHD dat kan worden gemaakt via Hyper-V (New-VHD-pad C:\dummy.vhd-vaste - SizeBytes 1 GB). Dit proces los het probleem dat verhindert dat het mislukte item wordt verwijderd. Vervolgens, 15 minuten na het maken van de installatiekopie van het pop, u kunt met succes verwijderen.

  U kunt vervolgens redownload van de VM-installatiekopie die eerder is mislukt.

- <!-- TBD -  IS ASDK --> Als een uitbreiding op een VM-implementatie de inrichting te lang duurt, laat gebruikers de time-out van de inrichting in plaats van bij stoppen van de procedure voor de toewijzing ongedaan of verwijder de virtuele machine.  

- <!-- 1662991 - IS ASDK --> Diagnostische gegevens over Linux-VM wordt niet ondersteund in Azure Stack. Wanneer u een Linux-VM met VM diagnostics is ingeschakeld implementeren, wordt de implementatie mislukt. De implementatie mislukt ook als u de Linux-VM eenvoudige metrische gegevens via diagnostische instellingen inschakelen. 

#### <a name="networking"></a>Netwerken
- <!-- 1766332 - IS, ASDK --> Onder **netwerken**, als u klikt op **VPN-Gateway maken** voor het instellen van een VPN-verbinding, **op basis van beleid** wordt vermeld als een VPN-type. Selecteer deze optie niet. Alleen de **Route op basis van** optie wordt ondersteund in Azure Stack.

- <!-- 2388980 -  IS ASDK --> Nadat een virtuele machine is gemaakt en die zijn gekoppeld aan een openbaar IP-adres, kunt u deze VM op basis van IP-adres kan niet loskoppelen. Ontkoppeling lijkt te werken, maar het eerder toegewezen openbare IP-adres blijft gekoppeld aan de oorspronkelijke virtuele machine.

  Op dit moment moet u alleen nieuwe openbare IP-adressen voor nieuwe virtuele machines die u maakt.

  Dit gebeurt zelfs als u het IP-adres aan een nieuwe virtuele machine opnieuw toewijzen (vaak aangeduid als een *wisselen van VIP*). Alle toekomstige probeert verbinding maken via dit resultaat van de IP-adres in een verbinding naar de oorspronkelijk gekoppelde virtuele machine en niet naar het nieuwe certificaat.

- <!-- 2292271 - IS ASDK --> Als u een quotumlimiet voor een netwerkbron die deel uitmaakt van een aanbieding en een Plan dat is gekoppeld aan een tenantabonnement verhoogt, wordt de nieuwe limiet niet toegepast op dat aan het abonnement. De nieuwe limiet is echter van toepassing op nieuwe abonnementen die zijn gemaakt nadat het quotum is verhoogd. 

  U kunt dit probleem omzeilen, door een aanvullende plan te gebruiken voor het verhogen van een quotum op het netwerk wanneer het abonnement al gekoppeld aan een abonnement is. Zie voor meer informatie over het [beschikbaar maken voor een aanvullende plan](.\.\azure-stack-subscribe-plan-provision-vm.md#to-make-an-add-on-plan-available).

- <!-- 2304134 IS ASDK --> U kunt een abonnement met DNS-Zone resources of de routetabel-resources die zijn gekoppeld aan het niet verwijderen. Als u wilt het abonnement is verwijderd, moet u eerst de DNS-Zone en routetabel resources uit het tenantabonnement verwijderen. 


- <!-- 1902460 -  IS ASDK --> Azure Stack biedt ondersteuning voor een enkel *lokale netwerkgateway* per IP-adres. Dit geldt voor alle tenant-abonnementen. Nadat het maken van de eerste lokale gateway netwerkverbinding, volgende pogingen tot het maken van een resource van een lokale netwerkgateway met hetzelfde IP-adres worden geblokkeerd.

- <!-- 16309153 -  IS ASDK --> In een Virtueelnetwerk dat is gemaakt met een DNS-Server-instelling van *automatische*, wijzigen naar een aangepaste DNS-Server mislukt. De bijgewerkte instellingen worden niet gepusht naar virtuele machines in dit Vnet.
 
- <!-- TBD -  IS ASDK --> Azure Stack biedt geen ondersteuning voor extra netwerkinterfaces toevoegen aan een VM-exemplaar, nadat de virtuele machine is geïmplementeerd. Als de virtuele machine meer dan één netwerkinterface vereist, moeten ze worden gedefinieerd tijdens de implementatie.


#### <a name="sql-and-mysql"></a>SQL- en MySQL 
- <!-- TBD - ASDK --> De database die als host fungeert servers moet worden toegewezen voor gebruik door de resource-provider en gebruiker werkbelastingen. U kunt een exemplaar dat wordt gebruikt door andere consumenten, met inbegrip van App Services niet gebruiken.

- <!-- IS, ASDK --> Speciale tekens, inclusief spaties en perioden, worden niet ondersteund in de **familie** naam wanneer u een SKU voor de SQL- en MySQL-resourceproviders. 

#### <a name="app-service"></a>App Service
- <!-- TBD -  IS ASDK --> Gebruikers moeten de storage resourceprovider registreren voordat ze hun eerste Azure-functie in het abonnement maken.

- <!-- TBD -  IS ASDK --> Als u wilt de infrastructuur (werknemers, beheer, front-rollen) uitbreiden, moet u PowerShell gebruiken, zoals beschreven in de opmerkingen bij de release voor Compute.
 
#### <a name="usage"></a>Gebruik  
- <!-- TBD -  IS ASDK --> Gebruik openbare IP-adres-gebruiksgegevens meter toont dezelfde *datum/tijd evenement* waarde voor elke record in plaats van de *TimeDate* stempel waarin wordt weergegeven wanneer de record is gemaakt. U kunt deze gegevens op dit moment niet gebruiken om uit te voeren nauwkeurige accounting van gebruik van openbare IP-adres.

<!--
#### Identity
-->



#### <a name="downloading-azure-stack-tools-from-github"></a>Hulpprogramma's voor Azure Stack downloaden vanuit GitHub
- Wanneer u de *invoke-webrequest* PowerShell-cmdlet voor het downloaden van de Azure Stack-hulpprogramma's vanuit Github, wordt er een fout opgetreden:     
    -  *Invoke-webrequest: de aanvraag is afgebroken: kan geen beveiligde SSL/TLS-kanaal maken.*     

  Deze fout treedt op vanwege een recente GitHub ondersteuning afschaffing van de cryptografische standaarden Tlsv1 en Tlsv1.1 (de standaardinstelling voor PowerShell). Zie voor meer informatie, [zwakke cryptografische standaarden verwijdering kennisgeving](https://githubengineering.com/crypto-removal-notice/).

<!-- #### Identity -->



## <a name="build-201803021"></a>Build 20180302.1

### <a name="new-features-and-fixes"></a>Nieuwe functies en oplossingen
De nieuwe functies en oplossingen die zijn uitgebracht voor Azure Stack-geïntegreerde systemen, versie 1803 van toepassing op de Azure Stack Development Kit. Zie de [nieuwe functies](.\.\azure-stack-update-1803.md#new-features) en [problemen opgelost](.\.\azure-stack-update-1803.md#fixed-issues) secties van de Azure Stack-1803 bijwerken opmerkingen bij de release voor meer informatie.  
> [!IMPORTANT]    
> Enkele van de items die worden vermeld in de **nieuwe functies** en **problemen opgelost** secties alleen relevant zijn voor geïntegreerde Azure Stack-systemen.

### <a name="changes"></a>Wijzigingen
- De manier om te wijzigen van de status van een nieuwe aanbieding van *persoonlijke* naar *openbare* of *uit bedrijf genomen* is gewijzigd. Zie voor meer informatie, [maken van een aanbieding](.\.\azure-stack-create-offer.md). 


### <a name="known-issues"></a>Bekende problemen
 
#### <a name="portal"></a>Portal
- De mogelijkheid [een nieuwe ondersteuningsaanvraag openen in de vervolgkeuzelijst](.\.\azure-stack-manage-portals.md#quick-access-to-help-and-support) uit binnen de beheerder van de portal is niet beschikbaar. In plaats daarvan gebruikt u de volgende koppeling:     
    - Gebruik voor Azure Stack Development Kit https://aka.ms/azurestackforum.    

- <!-- 2050709 --> In de beheerportal is het niet mogelijk om te bewerken van metrische opslaggegevens voor Blob-service, tabelservice of Queue-service. Wanneer u gaat u naar de opslag, en selecteer vervolgens de blob-, tabel- of tegel voor queue-service, wordt er een nieuwe blade geopend waarin u een grafiek met metrische gegevens voor die service. Als u vervolgens bewerken vanaf de bovenkant van de metrische gegevens grafiek tegel selecteert, wordt de grafiek bewerken-blade geopend maar geen opties voor het bewerken van metrische gegevens weer.  

- U ziet een **activering vereist** waarschuwing die aangeeft voor het registreren van uw Azure Stack Development Kit. Dit gedrag is verwacht.

- Verwijderen van zwevende resources leidt van gebruiker-abonnementen. Als tijdelijke oplossing, eerst Gebruikersbronnen of de hele resourcegroep verwijderen en verwijder vervolgens gebruikersabonnementen.

- U kunt machtigingen aan uw abonnement met behulp van de Azure Stack-portals niet weergeven. Als tijdelijke oplossing, moet u PowerShell gebruiken om machtigingen te controleren.

- In het dashboard van de beheerportal, wordt de tegel Update mislukt om meer informatie over updates weer te geven. U lost dit probleem, klikt u op de tegel te vernieuwen.

-   In de beheerportal ziet u mogelijk een kritieke waarschuwing voor het onderdeel Microsoft.Update.Admin. De naam van de waarschuwing, beschrijving en herstel alle als weergegeven:  
    - *Fout - sjabloon voor FaultType ResourceProviderTimeout ontbreekt.*

    Deze waarschuwing kan veilig worden genegeerd. 

- In de beheerportal en de gebruikersportal, wordt de blade overzicht mislukt laden wanneer u de blade overzicht van storage-accounts die zijn gemaakt met een oudere API-versie selecteert (voorbeeld: 15-06-2015). 

  Als tijdelijke oplossing, kunt u PowerShell gebruiken om uit te voeren de **Start ResourceSynchronization.ps1** script toegang herstellen naar details van het opslagaccount. [Het script is beschikbaar via GitHub]( https://github.com/Azure/AzureStack-Tools/tree/master/Support/scripts), en met service-beheerdersreferenties op de host van development kit moet uitvoeren als u de ASDK gebruikt.  

- De **servicestatus** blade niet wordt geladen. Wanneer u de Service Health-blade opent in de beheerder of de gebruiker-portal, Azure Stack, een fout wordt weergegeven en informatie niet kan worden geladen. Dit is normaal. Hoewel u kunt selecteren en opent u de Health-Service, wordt deze functie is nog niet beschikbaar maar geïmplementeerd in een toekomstige versie van Azure Stack.


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

- In de Azure Stack-beheerportal, ziet u mogelijk een kritieke waarschuwing met de naam van de **in afwachting van extern certificaat verlopen**.  Deze waarschuwing kan veilig worden genegeerd en is van invloed op bewerkingen van de Azure Stack Development Kit. 


#### <a name="marketplace"></a>Marketplace
- Gebruikers de volledige marketplace zonder abonnement kunnen zoeken en met beheerdersrechten objecten, zoals plannen en aanbiedingen kunnen zien. Deze items zijn niet functioneel voor gebruikers.
 
#### <a name="compute"></a>Compute
- Instellingen voor vergroten/verkleinen voor schaalsets voor virtuele machines zijn niet beschikbaar in de portal. Als tijdelijke oplossing, kunt u [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Vanwege de verschillen tussen versies van PowerShell, moet u de `-Name` parameter in plaats van `-VMScaleSetName`.

- Wanneer u virtuele machines op de gebruikersportal van Azure Stack maakt, wordt een onjuist aantal gegevensschijven die aan een virtuele machine uit de DS-serie kunt koppelen door de portal weergegeven. DS-serie VM's kan zo veel gegevensschijven bevatten als de Azure-configuratie.

- Wanneer een VM-installatiekopie niet kan worden gemaakt, kan een mislukte-item dat u niet verwijderen worden toegevoegd aan de VM-installatiekopieën compute-blade.

  Als tijdelijke oplossing, kunt u een nieuwe VM-installatiekopie maken met een dummy VHD dat kan worden gemaakt via Hyper-V (New-VHD-pad C:\dummy.vhd-vaste - SizeBytes 1 GB). Dit proces los het probleem dat verhindert dat het mislukte item wordt verwijderd. Vervolgens, 15 minuten na het maken van de installatiekopie van het pop, u kunt met succes verwijderen.

  U kunt vervolgens redownload van de VM-installatiekopie die eerder is mislukt.

-  Als een uitbreiding op een VM-implementatie de inrichting te lang duurt, laat gebruikers de time-out van de inrichting in plaats van bij stoppen van de procedure voor de toewijzing ongedaan of verwijder de virtuele machine.  

- <!-- 1662991 --> Diagnostische gegevens over Linux-VM wordt niet ondersteund in Azure Stack. Wanneer u een Linux-VM met VM diagnostics is ingeschakeld implementeren, wordt de implementatie mislukt. De implementatie mislukt ook als u de Linux-VM eenvoudige metrische gegevens via diagnostische instellingen inschakelen. 


#### <a name="networking"></a>Netwerken
- Onder **netwerken**, als u klikt op **verbinding** voor het instellen van een VPN-verbinding, **VNet-naar-VNet** wordt vermeld als een mogelijke verbindingstype. Selecteer deze optie niet. Op dit moment alleen de **Site-naar-site (IPsec)** optie wordt ondersteund.

- Nadat een virtuele machine is gemaakt en die zijn gekoppeld aan een openbaar IP-adres, kunt u deze VM op basis van IP-adres kan niet loskoppelen. Ontkoppeling lijkt te werken, maar het eerder toegewezen openbare IP-adres blijft gekoppeld aan de oorspronkelijke virtuele machine.

  Op dit moment moet u alleen nieuwe openbare IP-adressen voor nieuwe virtuele machines die u maakt.

  Dit gebeurt zelfs als u het IP-adres aan een nieuwe virtuele machine opnieuw toewijzen (vaak aangeduid als een *wisselen van VIP*). Alle toekomstige probeert verbinding maken via dit resultaat van de IP-adres in een verbinding naar de oorspronkelijk gekoppelde virtuele machine en niet naar het nieuwe certificaat.



- Azure Stack biedt ondersteuning voor een enkel *lokale netwerkgateway* per IP-adres. Dit geldt voor alle tenant-abonnementen. Nadat het maken van de eerste lokale gateway netwerkverbinding, volgende pogingen tot het maken van een resource van een lokale netwerkgateway met hetzelfde IP-adres worden geblokkeerd.

- In een Virtueelnetwerk dat is gemaakt met een DNS-Server-instelling van *automatische*, wijzigen naar een aangepaste DNS-Server mislukt. De bijgewerkte instellingen worden niet gepusht naar virtuele machines in dit Vnet.
 
- Azure Stack biedt geen ondersteuning voor extra netwerkinterfaces toevoegen aan een VM-exemplaar, nadat de virtuele machine is geïmplementeerd. Als de virtuele machine meer dan één netwerkinterface vereist, moeten ze worden gedefinieerd tijdens de implementatie.



#### <a name="sql-and-mysql"></a>SQL- en MySQL 
- Duurt maximaal één uur voordat gebruikers databases in een nieuwe SQL- of MySQL SKU kunnen maken.

- De database die als host fungeert servers moet worden toegewezen voor gebruik door de resource-provider en gebruiker werkbelastingen. U kunt een exemplaar dat wordt gebruikt door andere consumenten, met inbegrip van App Services niet gebruiken.

- <!-- IS, ASDK --> Speciale tekens, inclusief spaties en perioden, worden niet ondersteund in de **familie** of **laag** namen wanneer u een SKU voor de SQL- en MySQL-resourceproviders.

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
