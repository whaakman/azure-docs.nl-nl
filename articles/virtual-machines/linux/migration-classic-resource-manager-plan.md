---
title: Planning voor de migratie van IaaS-resources van klassiek naar Azure Resource Manager | Microsoft Docs
description: Planning voor de migratie van IaaS-resources van klassiek naar Azure Resource Manager
services: virtual-machines-linux
documentationcenter: ''
author: singhkays
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 78492a2c-2694-4023-a7b8-c97d3708dcb7
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/01/2017
ms.author: kasing
ms.openlocfilehash: 40e0c8c91c44e67c2c409d2efb1e917fdbe0bf33
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2019
ms.locfileid: "55562093"
---
# <a name="planning-for-migration-of-iaas-resources-from-classic-to-azure-resource-manager"></a>Planning voor de migratie van IaaS-resources van klassiek naar Azure Resource Manager
Azure Resource Manager biedt een aantal geweldige functies, is het essentieel is voor het plannen van de migratie om ervoor zaken soepel verloopt. Tijd besteed aan het plannen, zorgt u ervoor dat u geen problemen ondervindt tijdens het uitvoeren van migratieactiviteiten. 

> [!NOTE] 
> De volgende richtlijnen is sterk bijgedragen aan door de Azure Customer Advisory team en uw oplossing in de Cloud-architecten die werken met klanten in grote omgevingen migreren. Naarmate deze dit document wordt steeds bijgewerkt als nieuwe patronen van succes indelingsversies beschikbaar komen, dus kom terug in tijd op de tijd om te zien of er geen nieuwe aanbevelingen zijn.

Er zijn vier algemene fasen van de migratie:

![Migratiefasen](../media/virtual-machines-windows-migration-classic-resource-manager/plan-labtest-migrate-beyond.png)

## <a name="plan"></a>Plannen

### <a name="technical-considerations-and-tradeoffs"></a>Technische overwegingen voor- en nadelen

Afhankelijk van de grootte van de technische vereisten, locaties en operationele procedures, is het raadzaam om te overwegen:

1. Waarom is de Azure Resource Manager voor uw organisatie vereist?  Wat zijn de zakelijke redenen voor een migratie?
2. Wat zijn de technische redenen voor Azure Resource Manager?  Wat (indien aanwezig) extra Azure-services wilt u gebruiken?
3. Welke toepassing (of sets van virtuele machines) is inbegrepen bij de migratie?
4. Welke scenario's worden ondersteund met de migratie-API?  Controleer de [niet-ondersteunde functies en configuraties](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#unsupported-features-and-configurations).
5. Biedt uw operationele teams nu ondersteuning voor toepassingen/VM's in zowel klassieke als Azure Resource Manager?
6. Hoe (indien helemaal) Azure Resource Manager wijzigt de VM-implementatie, beheer, bewaking en rapportage van processen?  Moeten uw implementatiescripts om te worden bijgewerkt?
7. Wat is de communicatie van plan bent om te waarschuwen belanghebbenden (eindgebruikers, toepassingseigenaren en eigenaren van infrastructuur)?
8. Afhankelijk van de complexiteit van de omgeving, moet er een onderhoudsperiode waar de toepassing niet beschikbaar voor eindgebruikers en toepassingseigenaren van is?  Zo ja, hoe lang?
9. Wat is de trainingsplan opstellen om ervoor te zorgen belanghebbenden deskundig en kundig in Azure Resource Manager?
10. Wat is het Programmabeheer of project management plan voor de migratie?
11. Wat zijn de tijdlijnen voor de migratie van Azure Resource Manager en andere gerelateerde technologie wegenkaarten?  Zijn ze optimaal uitgelijnd?

### <a name="patterns-of-success"></a>Patronen van geslaagd

Geslaagde klanten gedetailleerde plannen waar de voorgaande vragen zijn besproken, gedocumenteerd en geregeld.  Zorg ervoor dat de migratieplannen breed worden gecommuniceerd naar sponsors en belanghebbenden.  Geef uzelf met kennis over uw opties voor de migratie; lezen door deze migratie-document dat hieronder wordt sterk aanbevolen.

* [Overzicht van het platform ondersteunde migratie van IaaS-resources van klassiek naar Azure Resource Manager](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Technische details over door platforms ondersteunde migratie van klassiek naar Azure Resource Manager](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Planning voor de migratie van IaaS-resources van het klassieke implementatiemodel naar Azure Resource Manager](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [PowerShell gebruiken voor het migreren van IaaS-resources van klassiek naar Azure Resource Manager](../windows/migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [CLI gebruiken voor het migreren van IaaS-resources van klassiek naar Azure Resource Manager](migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Community-hulpprogramma's voor hulp bij de migratie van IaaS-resources van klassiek naar Azure Resource Manager](../windows/migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Bekijk de meest voorkomende migratiefouten](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Lees de veelgestelde vragen over migratie IaaS-resources van klassiek naar Azure Resource Manager](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="pitfalls-to-avoid"></a>Valkuilen om te voorkomen

- Fout bij het plannen.  De stappen van de technologie van deze migratie hebben bewezen en het resultaat is voorspelbare.
- De veronderstelling dat het platform ondersteunde migratie die API wordt rekening gehouden met alle scenario's. Lees de [niet-ondersteunde functies en configuraties](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#unsupported-features-and-configurations) om te begrijpen welke scenario's worden ondersteund.
- Mogelijke onderbreking van de toepassing niet van plan voor eindgebruikers.  Plan onvoldoende buffer om te waarschuwen voldoende eindgebruikers van de tijd van de toepassing mogelijk niet beschikbaar.


## <a name="lab-test"></a>Test Lab 

**Het repliceren van uw omgeving en een test-migratie**
  > [!NOTE]
  > Exacte replicatie van uw bestaande omgeving wordt uitgevoerd met behulp van een door de community geleverde hulpprogramma dat wordt niet officieel ondersteund door Microsoft Support. Daarom is het een **optionele** stap maar dit is de beste manier om erachter te komen problemen zonder dat u uw productie-omgevingen. Als met een door de community geleverde hulpprogramma kan niet worden gebruikt, lees vervolgens over de aanbeveling valideren/voorbereiden/afbreken droge uitvoeren hieronder.
  >
  
  Uitvoering van een test lab van uw exacte scenario (compute, netwerken en opslag), is de beste manier om te controleren of een soepele migratie. Dit helpt ervoor te zorgen:

  - Een volledig afzonderlijke lab of een bestaande niet-productie-omgeving te testen. U wordt aangeraden een volledig afzonderlijke lab die meerdere malen kan worden gemigreerd en destructief kan worden gewijzigd.  Scripts voor het verzamelen/hydraat metagegevens uit de echte abonnementen worden hieronder vermeld.
  - Het is een goed idee om het lab maken in een afzonderlijk abonnement. De reden is dat de testomgeving wordt herhaaldelijk omlaag worden verwijderd, en wanneer u een afzonderlijke, geïsoleerd abonnement wordt de kans kleiner dat iets echte wordt per ongeluk verwijderd.

  Dit kan worden bereikt met behulp van het hulpprogramma AsmMetadataParser. [Meer informatie over dit hulpprogramma hier](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/AsmToArmMigrationApiToolset)

### <a name="patterns-of-success"></a>Patronen van geslaagd

De volgende zijn problemen gedetecteerd in veel van de grotere migraties. Dit is geen uitputtende lijst en u moet verwijzen naar de [niet-ondersteunde functies en configuraties](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#unsupported-features-and-configurations) voor meer informatie. U kan of kan niet deze technische problemen optreden, maar in dat geval het oplossen van deze vóór de migratie zorgt ervoor dat een soepeler ervaring.

- **Een uitvoering van de testmodus valideren/voorbereiden/afbreken doen** -dit is misschien wel de belangrijkste stap om ervoor te zorgen van klassiek naar Azure Resource Manager migratie geslaagd. De migratie-API heeft drie belangrijke stappen: Valideren, voorbereiden en vastleggen. Valideren van wordt de status van uw klassieke omgeving lezen en retourneert een resultaat van alle problemen. Echter, omdat sommige problemen in de Azure Resource Manager-stack voorkomen kunnen, valideren zal niet alles onderscheppen. De volgende stap in het migratieproces, voorbereiden kunt weergeven die problemen. Voorbereiden worden de metagegevens van klassiek naar Azure Resource Manager verplaatsen, maar wordt niet doorvoeren van de verplaatsing en wordt niet verwijderen of wijzigen iets aan de kant van het klassieke model. De uitvoering droge omvat het voorbereiden van de migratie en vervolgens wordt afgebroken (**niet doorvoeren**) voorbereiden van de migratie. Het doel van valideren/voorbereiden/afbreken droge uitvoering is om te zien van de metagegevens in de Azure Resource Manager-stack, onderzoekt het (*via programmacode of via de Portal*), en controleer of alles goed migreert en werken via technische problemen.  Ook krijgt u een idee van de duur van de migratie, zodat u dienovereenkomstig plannen voor uitvaltijd maken kan.  Een valideren/voorbereiden/afbreken niet leidt tot geen uitvaltijd gebruiker; Daarom is het niet-verstorend voor gebruik van de toepassing.
  - De volgende items moeten worden opgelost voordat het droge uitvoeren, maar een test testmodus wordt ook gewoon leegmaken van deze stappen voor gegevensvoorbereiding als ze zijn gemist. Tijdens de migratie van de onderneming, hebben we het droge uitvoeren op een veilige en waardevolle manier om te controleren of de gereedheid van de migratie worden ontdekt.
  - Als voorbereiding wordt uitgevoerd, het besturingselement vlak (Azure management-bewerkingen) wordt vergrendeld voor het hele virtuele netwerk, zodat er kunnen geen wijzigingen aangebracht in de metagegevens van de virtuele machine tijdens het valideren/voorbereiden/afbreken.  Maar anders een functie van de toepassing (extern bureaublad-Sessiehost, VM-gebruik, enzovoort), worden niet.  Gebruikers van de virtuele machines weet niet dat de uitvoering droge wordt uitgevoerd.

- **Express Route-Circuits en VPN-**. Express Route-Gateways met autorisatielinks kunnen momenteel niet worden gemigreerd zonder downtime. Zie voor een tijdelijke oplossing, [migreren ExpressRoute-circuits en gekoppelde virtuele netwerken van het klassieke naar het Resource Manager-implementatiemodel](../../expressroute/expressroute-migration-classic-resource-manager.md).

- **VM-extensies** -extensies voor virtuele machines worden mogelijk een van de grootste obstakels voor migratie van actieve VM's. Herstel van VM-extensies kan nemen al 1-2 dagen, dus dienovereenkomstig te plannen.  Een werkende Azure-agent is nodig voor het rapporteren van VM-extensie-status van actieve virtuele machines. Als de status voor een actieve virtuele machine terug als beschadigd gaat, wordt deze migratie stoppen. De agent zelf hoeft niet in staat om in te schakelen van de migratie, maar als extensies op de virtuele machine bestaat, klikt u vervolgens zowel een agent werkt en uitgaande internetverbinding (met DNS) er nodig zijn voor migratie naar voren verplaatsen.
  - Als de verbinding met een DNS-server verbroken tijdens de migratie van alle VM-extensies, met uitzondering BGInfo v1 is. \* moet eerst worden verwijderd uit elke virtuele machine voordat het voorbereiden van de migratie en weer opnieuw vervolgens toegevoegd aan de virtuele machine na de migratie van Azure Resource Manager.  **Dit is alleen voor virtuele machines die worden uitgevoerd.**  Als de virtuele machines zijn gestopt toewijzing ongedaan wordt gemaakt, hoeft VM-extensies niet worden verwijderd. **Opmerking:** Veel extensies, zoals Azure diagnostics en security center controle zal zichzelf opnieuw installeren na de migratie, ze te verwijderen is geen probleem.
  - Bovendien, zorg ervoor dat Netwerkbeveiligingsgroepen worden niet uitgaande toegang tot internet beperken. Dit kan gebeuren bij sommige configuraties Netwerkbeveiligingsgroepen. Uitgaande toegang tot internet (en DNS) is nodig voor VM-extensies worden gemigreerd naar Azure Resource Manager. 
  - Er zijn twee versies van de BGInfo-extensie: v1 en v2.  Als de virtuele machine is gemaakt met behulp van de Azure portal of PowerShell, wordt de virtuele machine de v1-extensie waarschijnlijk daarop hebben. Deze extensie niet hoeft te worden verwijderd en worden overgeslagen (niet gemigreerd) door de migratie-API. Echter, als de klassieke virtuele machine is gemaakt met de nieuwe Azure portal, waarschijnlijk is er de JSON-gebaseerde v2-versie van BGInfo, die kan worden gemigreerd naar Azure Resource Manager de agent werkt en uitgaande toegang tot internet (en DNS) is opgegeven. 
  - **Herstel-optie 1**. Als u weet dat uw VM's geen uitgaande internet toegang tot een werkende DNS-service en werkt de Azure-agents op de virtuele machines, verwijdert u alle VM-extensies als onderdeel van de migratie voordat voorbereiden, installeert u de VM-extensies opnieuw na de migratie. 
  - **Herstel-optie 2**. Als VM-extensies te groot is van een drempel zijn, is een andere optie het afsluiten/toewijzing ongedaan maken alle VM's vóór de migratie. De toewijzing ongedaan gemaakt virtuele machines migreren, en start ze vervolgens aan de Azure Resource Manager. Het voordeel hier is dat de VM-extensies worden gemigreerd. Het nadeel is dat alle openbare virtuele IP-adressen gerichte verbroken worden (dit kan een niet-starter zijn), en natuurlijk de virtuele machines worden afgesloten veroorzaakt een veel grotere invloed op toepassingen werken.

    > [!NOTE] 
    > Als een Azure Security Center-beleid is geconfigureerd op basis van de actieve virtuele machines die wordt gemigreerd, het beveiligingsbeleid moet worden gestopt voordat u extensies verwijdert, anders wordt de bewaking van de extensie van de beveiliging wordt automatisch opnieuw geïnstalleerd op de virtuele machine nadat deze is verwijderd.

- **Beschikbaarheidssets** : als u een virtueel netwerk (vNet) worden gemigreerd naar Azure Resource Manager, de klassieke implementatie (dat wil zeggen cloudservice) ingesloten virtuele machines moeten zich allemaal in één beschikbaarheidsset of de virtuele machines moet niet deel in een beschikbaarheidsset. Met meer dan één beschikbaarheidsset in de cloudservice is niet compatibel met Azure Resource Manager en migratie wordt onderbroken.  Er kan niet ook enkele VM's in een beschikbaarheidsset en sommige virtuele machines niet in een beschikbaarheidsset. U kunt dit oplossen, moet u herstellen of verplaatsen van uw cloudservice.  Plan als dit wordt mogelijk tijd in beslag nemen. 

- **Web/Worker-Rolimplementaties** -Cloud Services met web-en werkrollen niet migreren naar Azure Resource Manager. De web-/ werkrollen moeten eerst worden verwijderd van het virtuele netwerk voordat de migratie kunt starten.  Een typische oplossing is alleen in een afzonderlijke klassiek virtueel netwerk dat is ook gekoppeld aan een ExpressRoute-circuit naar instanties van de web-/ werkrol of voor het migreren van de code naar nieuwere PaaS-App-Services (deze bespreking is buiten het bereik van dit document). In het eerste geval implementeren, een nieuwe klassiek virtueel netwerk maken, verplaatsen/implementeert u opnieuw de web-/ werkrollen naar deze nieuw virtueel netwerk en verwijderen van de implementaties van het virtuele netwerk wordt verplaatst. Er zijn geen codewijzigingen vereist. De nieuwe [Peering in virtuele netwerken](../../virtual-network/virtual-network-peering-overview.md) mogelijkheid kan worden gebruikt om samen het klassieke virtuele netwerk met de web-/ werkrollen en andere virtuele netwerken in dezelfde Azure-regio, zoals het virtuele netwerk dat wordt gemigreerd (**nadat het virtuele netwerkmigratie is voltooid als de gekoppelde virtuele netwerken kunnen niet worden gemigreerd**), daarom bieden dezelfde mogelijkheden met zonder prestatieverlies van en geen sancties latentie/bandbreedte. Het toevoegen van gegeven [Peering in virtuele netwerken](../../virtual-network/virtual-network-peering-overview.md), web/worker-rolimplementaties nu eenvoudig kan worden verholpen en blokkeert de migratie naar Azure Resource Manager.

- **Azure Resource Manager-quota** -Azure-regio's hebben afzonderlijke quota/limieten voor zowel klassieke als Azure Resource Manager. Hoewel in een migratiescenario nieuwe hardware is niet wordt verbruikt *(we bent wisselen van bestaande VM's van klassiek naar Azure Resource Manager)*, Azure Resource Manager-quota nog moeten worden voldaan met voldoende capaciteit voor migratie kunt starten. Hieronder volgen de belangrijkste limieten die we hebben gezien problemen veroorzaken.  Open een ondersteuningsticket quotum te verhogen van de limieten. 

    > [!NOTE]
    > Deze limieten moeten worden gegenereerd in dezelfde regio als uw huidige omgeving moeten worden gemigreerd.
    >

    - Netwerkinterfaces
    - Load balancers
    - Openbare IP-adressen
    - Statische openbare IP-adressen
    - Kerngeheugens
    - Netwerkbeveiligingsgroepen
    - Routetabellen

    U kunt uw huidige Azure Resource Manager-quota's die met de volgende opdrachten met de meest recente versie van Azure CLI kunt controleren.

    **COMPUTE** *(kernen, Sets van beschikbaarheid)*

    ```bash
    az vm list-usage -l <azure-region> -o jsonc 
    ```

    **Netwerk** *(virtuele netwerken, statische openbare IP-adressen, openbare IP-adressen, de Netwerkbeveiligingsgroepen Network Interfaces, Load Balancers, routetabellen)*
    
    ```bash
    az network list-usages -l <azure-region> -o jsonc
    ```

    **Opslag** *(Opslagaccount)*
    
    ```bash
    az storage account show-usage
    ```

- **Azure Resource Manager API beperkingslimieten** : als u een groot genoeg omgeving (zoals hebt) > 400 VM's in een VNET), kunt u de standaard-API beperkingslimieten voor schrijfbewerkingen bereikt (momenteel **1200-schrijfbewerkingen/uur**) in Azure Resource Manager. Voordat u begint met de migratie, moet u een ondersteuningsticket om deze limiet voor uw abonnement te verhogen verhogen.

- **Inrichting is een time-out opgetreden van VM-Status** : als een virtuele machine de status van heeft **inrichting is een time-out**, dit moet worden opgelost vóór de migratie. De enige manier om dit te doen is met uitvaltijd door het opheffen van inrichting/beëindiging van de virtuele machine (verwijderen, houden van de schijf en opnieuw maken van de virtuele machine). 

- **De Status van de VM RoleStateUnknown** - als de migratie stopt vanwege een **onbekende Rolstatus** foutbericht, de virtuele machine met behulp van de portal controleren en zorg ervoor dat deze wordt uitgevoerd. Deze fout wordt meestal verdwijnen op haar eigen (geen herstel is vereist) na een paar minuten en is vaak een tijdelijke type vaak gezien tijdens een virtuele Machine **start**, **stoppen**, **opnieuwstarten** bewerkingen. **Aanbevolen procedures:** migratie opnieuw na een paar minuten opnieuw proberen. 

- **Fabric-Cluster bestaat niet** : In sommige gevallen kan bepaalde VM's om diverse oneven redenen kunnen niet worden gemigreerd. Een van deze gevallen bekend is als de virtuele machine is onlangs gemaakt (in de afgelopen week of zo) en is er gebeurd met het land van een Azure-cluster die nog niet is uitgerust voor Azure Resource Manager-workloads.  U krijgt een foutmelding waarin wordt gemeld **fabric-cluster bestaat niet** en de virtuele machine kan niet worden gemigreerd. Wacht een paar dagen wordt dit specifieke probleem meestal opgelost als het cluster binnenkort Azure Resource Manager is ingeschakeld krijgt. Echter, een directe oplossing is om te `stop-deallocate` de virtuele machine, vervolgens naar voren te gaan met migratie en start de virtuele machine een back-up in Azure Resource Manager nadat de migratie.

### <a name="pitfalls-to-avoid"></a>Valkuilen om te voorkomen

- Geen sneltoetsen en laat u de migratie valideren, voorbereiden/afbreken droge uitvoeren.
- Meeste, zo niet alle verschijnt van de potentiële problemen tijdens het valideren/voorbereiden/afbreken stappen.

## <a name="migration"></a>Migratie

### <a name="technical-considerations-and-tradeoffs"></a>Technische overwegingen voor- en nadelen

U bent nu klaar omdat u via de bekende problemen met uw omgeving hebt gewerkt.

Voor de echte migraties is het raadzaam om te overwegen:

1. Plan en plannen van het virtuele netwerk (kleinste eenheid van de migratie) met prioriteit verhogen.  Voer eerst de eenvoudige virtuele netwerken en de voortgang van de meer gecompliceerde virtuele netwerken.
2. De meeste klanten hebben niet-productie-en productieomgevingen.  De laatste productie plannen.
3. **(OPTIONEEL)**  Plannen van een uitvaltijd wegens onderhoud met veel van de buffer in het geval zich onverwachte problemen voordoen.
4. Communiceren met en afgestemd op uw ondersteuningsteams in het geval zich problemen voordoen.

### <a name="patterns-of-success"></a>Patronen van geslaagd

De technische hulp van de bovenstaande sectie van de Test Lab moet worden beschouwd als en verholpen voordat u een echte migratie.  Met het testen is de migratie daadwerkelijk een gebeurtenisafhankelijke.  Voor productieomgevingen, is het mogelijk handig om aanvullende ondersteuning, zoals een vertrouwde Microsoft-partner of Microsoft Premier services.

### <a name="pitfalls-to-avoid"></a>Valkuilen om te voorkomen

Niet volledig testen kan problemen veroorzaken en vertraging in de migratie.  

## <a name="beyond-migration"></a>Na migratie

### <a name="technical-considerations-and-tradeoffs"></a>Technische overwegingen voor- en nadelen

Nu dat u in Azure Resource Manager, maximaliseert u het platform.  Lees de [overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) voor meer informatie over extra voordelen.

Dingen om te overwegen:

- Bundeling van de migratie met andere activiteiten.  De meeste klanten kiezen voor een onderhoudsvenster van toepassing.  Als dit het geval is, is het raadzaam deze uitvaltijd gebruiken om in te schakelen van andere Azure Resource Manager-mogelijkheden, zoals versleuteling en migreren naar Managed Disks.
- Terug naar de technische en zakelijke redenen voor Azure Resource Manager. Schakel de extra services alleen beschikbaar op Azure Resource Manager die betrekking hebben op uw omgeving.
- Moderniseer uw omgeving met PaaS-services.

### <a name="patterns-of-success"></a>Patronen van geslaagd

Worden doelgericht op welke services u nu wilt inschakelen in Azure Resource Manager.  Veel klanten vinden het hieronder aantrekkelijk voor hun Azure-omgevingen:

- [Op rollen gebaseerd toegangsbeheer](../../role-based-access-control/overview.md).
- [Azure Resource Manager-sjablonen voor de implementatie eenvoudiger en meer gecontroleerde](../../azure-resource-manager/resource-group-overview.md#template-deployment).
- [Tags](../../azure-resource-manager/resource-group-using-tags.md).
- [Activiteit-besturingselement](../../azure-resource-manager/resource-group-audit.md)
- [Beleid voor Azure](../../azure-policy/azure-policy-introduction.md)

### <a name="pitfalls-to-avoid"></a>Valkuilen om te voorkomen

Houd er rekening mee waarom u deze van klassiek naar Azure Resource Manager-migratie is gestart.  Wat was de oorspronkelijke zakelijke redenen? U de zakelijke reden is bereikt?


## <a name="next-steps"></a>Volgende stappen

* [Overzicht van het platform ondersteunde migratie van IaaS-resources van klassiek naar Azure Resource Manager](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Technische details over door platforms ondersteunde migratie van klassiek naar Azure Resource Manager](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Planning voor de migratie van IaaS-resources van het klassieke implementatiemodel naar Azure Resource Manager](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [PowerShell gebruiken voor het migreren van IaaS-resources van klassiek naar Azure Resource Manager](../windows/migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Community-hulpprogramma's voor hulp bij de migratie van IaaS-resources van klassiek naar Azure Resource Manager](../windows/migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Bekijk de meest voorkomende migratiefouten](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Lees de veelgestelde vragen over migratie IaaS-resources van klassiek naar Azure Resource Manager](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
