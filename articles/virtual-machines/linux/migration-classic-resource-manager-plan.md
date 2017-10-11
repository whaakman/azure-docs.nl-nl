---
title: Planning voor de migratie van IaaS-middelen van klassiek naar Azure Resource Manager | Microsoft Docs
description: Migratie van IaaS-middelen van klassiek naar Azure Resource Manager plannen
services: virtual-machines-linux
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 78492a2c-2694-4023-a7b8-c97d3708dcb7
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/01/2017
ms.author: kasing
ms.openlocfilehash: 5db4e5b18ad385e7eba125a1296a9c5054213446
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="planning-for-migration-of-iaas-resources-from-classic-to-azure-resource-manager"></a>Migratie van IaaS-middelen van klassiek naar Azure Resource Manager plannen
Hoewel Azure Resource Manager veel mogelijkheden biedt, is het essentieel dat u van plan bent om uw migratie reis naar zeker dat u besturingssysteemupgrades soepel verlopen. Tijd besteedt over het plannen, zorgt u ervoor dat u geen problemen ondervindt tijdens het uitvoeren van de migratieactiviteiten worden bewaakt. 

> [!NOTE] 
> De volgende hulp is sterk hebben bijgedragen door de Azure Customer Advisory team en de oplossing Cloud-architecten die werken met klanten op migreren grote enviornments. Als u dergelijke dit document blijven als er ontstaan nieuwe patronen van succes wordt bijgewerkt, dus controleer terug vanaf het moment dat op de tijd om te zien of er zijn geen nieuwe aanbevelingen.

Er zijn vier algemene fasen van het traject migratie:

![Fasen van de migratie](../media/virtual-machines-windows-migration-classic-resource-manager/plan-labtest-migrate-beyond.png)

## <a name="plan"></a>Plannen

### <a name="technical-considerations-and-tradeoffs"></a>Technische overwegingen en de voor-en nadelen

Afhankelijk van de grootte van de technische vereisten, locaties en operationele procedures, is het raadzaam te overwegen:

1. Waarom Azure Resource Manager vereist is voor uw organisatie?  Wat zijn de zakelijke redenen voor het migreren?
2. Wat zijn de technische redenen voor Azure Resource Manager?  Wat (indien aanwezig) extra Azure-services wilt u gebruiken?
3. Welke toepassing (of sets van virtuele machines) is opgenomen in de migratie?
4. Welke scenario's worden ondersteund met de migratie API?  Controleer de [niet-ondersteunde functies en configuraties](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#unsupported-features-and-configurations).
5. Biedt uw operationele teams nu ondersteuning voor toepassingen/virtuele machines in zowel klassieke en Azure Resource Manager?
6. Hoe (als op alle) Azure Resource Manager verandert uw VM-implementatie, beheer, bewaking en rapportage van processen?  Moeten uw implementatiescripts worden bijgewerkt?
7. Wat is de communicatie van plan bent om te waarschuwen belanghebbenden (eindgebruikers, toepassingseigenaars en eigenaren van infrastructuur)?
8. Afhankelijk van de complexiteit van de omgeving, moet er ook een onderhoudsperiode waar de toepassing niet beschikbaar voor eindgebruikers en eigenaren van de toepassing is?  Zo ja, hoe lang?
9. Wat is de trainingsplan om ervoor te zorgen belanghebbenden geïnformeerde en kundig in Azure Resource Manager?
10. Wat is het Programmabeheer of het project management-plan voor de migratie?
11. Wat zijn de tijdlijnen voor de migratie van Azure Resource Manager en andere gerelateerde technologie weg maps?  Zijn ze optimaal uitgelijnd?

### <a name="patterns-of-success"></a>Patronen van geslaagd

Geslaagde klanten hebben gedetailleerde plannen waar de bovenstaande vragen zijn besproken, gedocumenteerd en bepaald.  Zorg ervoor dat de migratieplannen grote schaal worden gecommuniceerd naar sponsors en belanghebbenden.  Geef uzelf met kennis over uw opties voor de migratie; lezen door deze migratie-document dat is ingesteld onder wordt sterk aanbevolen.

* [Overzicht van de migratie van IaaS resources van klassieke in Azure Resource Manager-platform ondersteund](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Technische deep dive op platform ondersteund migratie van klassiek naar Azure Resource Manager](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Planning voor de migratie van IaaS-resources van het klassieke implementatiemodel naar Azure Resource Manager](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [PowerShell gebruiken voor het migreren van IaaS-middelen van klassiek naar Azure Resource Manager](../windows/migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [CLI gebruiken voor het migreren van IaaS-middelen van klassiek naar Azure Resource Manager](migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Community-hulpprogramma's voor hulp bij de migratie van IaaS-middelen van klassiek naar Azure Resource Manager](../windows/migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Bekijk de meest voorkomende migratiefouten](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Lees de veelgestelde vragen over IaaS Resourcemigratie van klassiek naar Azure Resource Manager](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="pitfalls-to-avoid"></a>Valkuilen om te voorkomen

- Plan is mislukt.  De stappen van de technologie van deze migratie zijn beproefde en het resultaat is voorspelbaar.
- De veronderstelling dat het platform migratie die API wordt rekening voor alle scenario's ondersteund. Lees de [niet-ondersteunde functies en configuraties](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#unsupported-features-and-configurations) om te begrijpen welke scenario's worden ondersteund.
- Mogelijke onderbreking van deze toepassing niet van plan voor eindgebruikers.  Plan onvoldoende buffer voldoende gewaarschuwd eindgebruikers van tijd van de toepassing mogelijk niet beschikbaar.


## <a name="lab-test"></a>Test Lab 

**Uw omgeving repliceren en een testmigratie uitvoeren**
  > [!NOTE]
  > Exacte replicatie van uw bestaande omgeving wordt uitgevoerd met behulp van een hulpmiddel community bijgedragen wordt niet officieel ondersteund door Microsoft Support. Daarom is het een **optionele** stap maar dit is de beste manier om erachter te komen problemen zonder dat u uw productieomgevingen. Als met een hulpprogramma community bijgedragen kan niet worden gebruikt, Lees over de aanbeveling Prepare-valideren/afbreken vulprocedure hieronder.
  >
  
  Uitvoering van een test lab van uw exacte scenario (compute, netwerk en opslag), is de beste manier om te zorgen voor een soepele migratie. Dit helpt ervoor te zorgen:

  - Een geheel afzonderlijke testomgeving of een bestaande niet-productieomgeving om te testen. U wordt aangeraden een geheel afzonderlijke lab die herhaaldelijk kan worden gemigreerd en destructief kan worden gewijzigd.  Scripts verzamelen/hydraat metagegevens uit de echte abonnementen worden hieronder vermeld.
  - Het is een goed idee om het lab maken in een apart abonnement. De reden is dat de testomgeving verwijderd herhaaldelijk en met een afzonderlijke, geïsoleerd abonnement u de kans beperkt dat er iets echte per ongeluk ontvangt verwijderd.

  Dit kan worden bewerkstelligd met het hulpprogramma AsmMetadataParser. [Lees meer over dit hulpprogramma hier](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/AsmToArmMigrationApiToolset)

### <a name="patterns-of-success"></a>Patronen van geslaagd

De volgende zijn problemen ontdekt in veel van de grotere migraties. Dit is geen uitputtende lijst en u moet verwijzen naar de [niet-ondersteunde functies en configuraties](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#unsupported-features-and-configurations) voor meer informatie. U kan of kan geen deze technische problemen ondervinden, maar als u dit doet het oplossen van deze voordat u de migratie een soepeler ervaring zorgt ervoor.

- **Een voorbereiden-valideren/afbreken vulprocedure doen** -dit is mogelijk de belangrijkste stap om ervoor te zorgen klassieke op Azure Resource Manager migratie geslaagd. De migratie API heeft drie stappen: valideren, voorbereiden en vastleggen. Valideren wordt de status van uw omgeving klassieke lees- en een resultaat van alle problemen te retourneren. Echter, omdat sommige problemen kunnen bestaan in de Azure Resource Manager-stack, valideren zal niet alles onderscheppen. De volgende stap in het migratieproces voorbereiden kunt weergeven die problemen. Voorbereiden wordt verplaatsen van de metagegevens van klassieke in Azure Resource Manager, maar wordt niet doorvoeren van de verplaatsing en wordt niet verwijderen of wijzigen iets aan de kant van het klassieke model. De vulprocedure omvat het voorbereiden van de migratie en vervolgens wordt afgebroken (**niet doorvoeren**) voorbereiden van de migratie. Het doel van vulprocedure valideren/voorbereiden/afbreken is om te kunnen zien alle metagegevens in de Azure Resource Manager-stack van onderzocht (*via programmacode of via de Portal*), en controleren of alles correct migreert en technische problemen doorlopen.  Ook krijgt u een idee van de duur van de migratie zodat u voor downtime dienovereenkomstig plannen kunt.  Een valideren/voorbereiden/afbreken veroorzaakt geen gebruiker uitvaltijd; het is daarom niet verstoren voor gebruik van de toepassing.
  - De volgende items moet worden opgelost voordat de vulprocedure, maar een test-test wordt ook gewoon uit deze voorbereidende stappen leegmaken als ze zijn gemist. We hebben de vulprocedure naar een veilige en waardevol zijn manier om te controleren of de gereedheid van de migratie worden gevonden tijdens de migratie van de onderneming.
  - Wanneer voorbereidt wordt uitgevoerd, het besturingselement vlak (Azure beheerbewerkingen) wordt vergrendeld voor de hele virtuele netwerk, dus er kunnen geen wijzigingen aangebracht in de metagegevens van de virtuele machine tijdens het valideren/voorbereiden/afbreken.  Maar anders een functie van de toepassing (extern bureaublad, VM-gebruik, enz.) niet worden gewijzigd.  Gebruikers van de virtuele machines weet niet dat de vulprocedure wordt uitgevoerd.

- **Express Route-Circuits en VPN-**. Momenteel kunnen niet Express Route-Gateways met autorisatie koppelingen worden gemigreerd zonder uitvaltijd. Zie voor een tijdelijke oplossing, [migreren ExpressRoute-circuits en bijbehorende virtuele netwerken van het klassieke naar het Resource Manager-implementatiemodel](../../expressroute/expressroute-migration-classic-resource-manager.md).

- **VM-extensies** -extensies van virtuele Machine worden mogelijk een van de grootste problemen te migreren actieve virtuele machines. Herstel van de VM-extensies kan al 1-2 dagen duren, dus plan dienovereenkomstig.  Een werkende Azure-agent is vereist voor het rapporteren van VM-extensie-status van actieve virtuele machines. Als de status weer wordt als beschadigd voor een actieve virtuele machine, is deze migratie wordt gestopt. De agent zelf niet hoeft te worden in staat om in te schakelen migratie, maar als uitbreidingen op de virtuele machine bestaat, vervolgens zowel een agent werkt en uitgaande internetverbinding (met DNS) vereist voor de migratie om te gaan.
  - Als de verbinding met een DNS-server verbroken tijdens de migratie alle VM-extensies behalve BGInfo v1 wordt. \* moet eerst worden verwijderd uit elke VM voordat voorbereiden voor migratie en weer opnieuw vervolgens toegevoegd aan de virtuele machine na de migratie van Azure Resource Manager.  **Dit is alleen voor virtuele machines die worden uitgevoerd.**  Als de virtuele machines zijn gestopt toewijzing ongedaan is gemaakt, hoeft VM-extensies niet worden verwijderd. **Opmerking:** veel extensies zoals Azure diagnostics en security center controle zal zichzelf opnieuw installeren na de migratie, dus te verwijderen is geen probleem.
  - Bovendien moet ervoor Netwerkbeveiligingsgroepen niet uitgaande toegang tot internet beperken. Dit kan gebeuren bij sommige configuraties Netwerkbeveiligingsgroepen. Uitgaande toegang tot internet (en DNS) is nodig voor VM-extensies voor Azure Resource Manager worden gemigreerd. 
  - Er zijn twee versies van de BGInfo-extensie: v1- en v2.  Als de virtuele machine is gemaakt met de klassieke portal of PowerShell, wordt de virtuele machine waarschijnlijk de v1-extensie hebben erop. Deze extensie niet hoeft te worden verwijderd en wordt overgeslagen (niet gemigreerd) door de API van de migratie. Echter, als de klassieke virtuele machine is gemaakt met de nieuwe Azure portal, waarschijnlijk wel de JSON-gebaseerde v2-versie van BGInfo die kan worden gemigreerd in Azure Resource Manager de agent werkt en uitgaande toegang tot internet (en DNS) is opgegeven. 
  - **Herstel optie 1**. Als u weet dat uw virtuele machines geen uitgaande internet toegang tot een werkende DNS-service, en werkt de Azure-agents op de virtuele machines, verwijdert u alle VM-extensies als onderdeel van de migratie voordat voorbereiden, opnieuw installeren van de VM-extensies na de migratie. 
  - **Herstel optie 2**. VM-extensies te groot is van een drempel, een andere optie is als shutdown/toewijzing alle VM's vóór de migratie. De toewijzing ongedaan is gemaakt virtuele machines migreert en vervolgens opnieuw aan de kant van de Azure Resource Manager. Hier het voordeel is dat de VM-extensies worden gemigreerd. Het nadeel is dat alle openbare geconfronteerd met virtuele IP's verloren gaan (kan dit een niet-starter), en natuurlijk de virtuele machines wordt afgesloten waardoor een veel grotere invloed op toepassingen werken.

    > [!NOTE] 
    > Als een Azure Security Center-beleid is geconfigureerd op basis van de actieve virtuele machines worden gemigreerd, wordt het beveiligingsbeleid moet worden gestopt voordat u uitbreidingen verwijdert, anders de bewaking van de uitbreiding van de beveiliging wordt automatisch opnieuw geïnstalleerd op de virtuele machine nadat deze is verwijderd.

- **Beschikbaarheidssets** : voor een virtueel netwerk (vNet) worden gemigreerd naar de Azure Resource Manager, de klassieke implementatie (dat wil zeggen cloudservice) ingesloten virtuele machines moeten deel uitmaken van één beschikbaarheidsset of de virtuele machines moet geen deel uitmaken van een beschikbaarheidsset. Met meer dan één beschikbaarheidsset voor de cloudservice is niet compatibel met Azure Resource Manager en migratie wordt gestopt.  Er kan niet ook een aantal virtuele machines in een beschikbaarheidsset en een aantal virtuele machines niet in een beschikbaarheidsset. U kunt dit oplossen door moet herstellen of verplaatsen van de cloudservice.  Plan dienovereenkomstig dit wordt mogelijk veel tijd in beslag. 

- **Implementaties van de rol web/Worker** -Cloud-Services met web-en werkrollen in Azure Resource Manager kan niet worden gemigreerd. De rollen web/worker moeten eerst worden verwijderd van het virtuele netwerk voordat migratie kunt starten.  Een typische oplossing is zojuist web/worker rolinstanties verplaatsen naar een afzonderlijke klassiek virtueel netwerk dat is ook gekoppeld aan een ExpressRoute-circuit of voor het migreren van de code naar nieuwere PaaS-App-Services (deze discussie is buiten het bereik van dit document). In het eerste geval implementeren, een nieuwe klassiek virtueel netwerk maken, de rollen web/worker die nieuw virtueel netwerk verplaatsen/opnieuw en verwijderen van de implementaties van het virtuele netwerk wordt verplaatst. Er zijn geen codewijzigingen is vereist. De nieuwe [virtuele netwerk Peering](../../virtual-network/virtual-network-peering-overview.md) mogelijkheid kan worden gebruikt als peer samen de klassiek virtueel netwerk met de web/worker-rollen en andere virtuele netwerken in dezelfde Azure-regio zoals het virtuele netwerk dat wordt gemigreerd (**nadat het virtuele netwerkmigratie is voltooid, zoals virtuele netwerken brengen kunnen niet worden gemigreerd**), daarom biedt dezelfde mogelijkheden met zonder prestatieverlies en geen sancties latentie/bandbreedte. Het toevoegen van gegeven [virtuele netwerk Peering](../../virtual-network/virtual-network-peering-overview.md), web/worker-rolimplementaties kunnen nu eenvoudig worden opgevangen en geen blokkeert de migratie naar Azure Resource Manager.

- **Azure Resource Manager-quota** -Azure-regio's hebben afzonderlijke quota/limieten voor zowel klassieke en Azure Resource Manager. Hoewel in een migratiescenario nieuwe hardware is niet worden verbruikt *(we je wisselen bestaande virtuele machines van klassiek naar Azure Resource Manager)*, Azure Resource Manager-quota nog moeten worden voldaan met voldoende capaciteit voordat de migratie kunt starten. Hieronder vindt de belangrijkste limieten die we hebt gezien problemen veroorzaken.  Open een ondersteuningsticket quotum te verhogen van de limieten. 

    > [!NOTE]
    > Deze limieten moeten worden gegenereerd in dezelfde regio bevinden als uw huidige omgeving moeten worden gemigreerd.
    >

    - Netwerkinterfaces
    - Taakverdelers
    - Openbare IP-adressen
    - Statische openbare IP-adressen
    - Cores
    - Netwerkbeveiligingsgroepen
    - Routetabellen

    U kunt uw huidige Azure Resource Manager-quota's met de volgende opdrachten met de meest recente versie van Azure CLI 2.0 controleren.

    **COMPUTE** *(kernen, Avaiability Sets)*

    ```bash
    az vm list-usage -l <azure-region> -o jsonc 
    ```

    **Netwerk** *(virtuele netwerken, statische openbare IP-adressen, openbare IP-adressen, de Netwerkbeveiligingsgroepen Network Interfaces, Load Balancers routetabellen)*
    
    ```bash
    az network list-usages -l <azure-region> -o jsonc
    ```

    **Opslag** *(Storage-Account)*
    
    ```bash
    az storage account show-usage
    ```

- **Azure Resource Manager-API beperking limieten** - als er een groot genoeg omgeving (bv. > 400 VM's in een VNET), kunt u de standaard API beperking limieten voor schrijfacties bereikt (momenteel **1200 schrijfbewerkingen per uur**) in Azure Resource Manager. Voordat u de migratie, moet u een ondersteuningsticket voor een verhoging van deze limiet voor uw abonnement te verhogen.

- **Inrichting is een time-out opgetreden van VM-Status** - als een virtuele machine de status van heeft **inrichting time-out**, moet dit worden opgelost vóór de migratie. De enige manier om dit te doen is met uitvaltijd door het opheffen van inrichting/reprovisioning van de virtuele machine (verwijderen, houd de schijf en opnieuw maken van de virtuele machine). 

- **VM-Status RoleStateUnknown** - als de migratie wordt afgebroken vanwege een **Rolstatus onbekend** foutbericht, inspecteren van de virtuele machine via de portal en zorg ervoor dat deze wordt uitgevoerd. Deze fout wordt meestal opgeslagen gaat van de eigenaar (geen herstel mogelijk vereist) na een paar minuten en is vaak een tijdelijke type vaak zichtbaar tijdens een virtuele Machine **start**, **stoppen**, **starten** bewerkingen. **Aanbevolen praktijken:** migratie opnieuw na een paar minuten opnieuw proberen. 

- **Fabric-Cluster bestaat niet** - In sommige gevallen bepaalde VMs oneven redenen kunnen niet worden gemigreerd. Een van deze gevallen bekend is als de virtuele machine is onlangs gemaakt (binnen de afgelopen week of doet) en is er gebeurd met een Azure-cluster die nog niet voor werkbelastingen met Azure Resource Manager is uitgerust land.  U krijgt een foutmelding waarin wordt gemeld **fabric-cluster bestaat niet** en de virtuele machine kan niet worden gemigreerd. Wacht een paar dagen wordt doorgaans bepaald kunt dit probleem oplossen als het cluster binnenkort Azure Resource Manager is ingeschakeld krijgt. Een onmiddellijke tijdelijke oplossing is echter `stop-deallocate` de VM vervolgens doorsturen gaan met migratie en start de virtuele machine een back-up in Azure Resource Manager na de migratie.

### <a name="pitfalls-to-avoid"></a>Valkuilen om te voorkomen

- Sneltoetsen en laat de migraties vulprocedure valideren, voorbereiden/afbreken niet.
- Meest, als niet alle, wordt van uw potentiële problemen surface tijdens het valideren, voorbereiden/afbreken stappen.

## <a name="migration"></a>Migratie

### <a name="technical-considerations-and-tradeoffs"></a>Technische overwegingen en de voor-en nadelen

U bent nu klaar omdat u via de bekende problemen met uw omgeving hebt gewerkt.

Voor de echte migraties, is het raadzaam te overwegen:

1. Plan en plannen van het virtuele netwerk (waarbij de kleinste eenheid van de migratie) met de prioriteit verhogen.  De eenvoudige virtuele netwerken eerst doen en de voortgang van de gecompliceerdere virtuele netwerken.
2. De meeste klanten hebben niet-productieve en productieomgevingen.  De laatste productie plannen.
3. **(OPTIONEEL)**  Plannen van een uitvaltijd onderhoud met veel van de buffer voor het geval zich onverwachte problemen voordoen.
4. Communicatie met en zijn afgestemd op uw ondersteuningsteams als zich problemen voordoen.

### <a name="patterns-of-success"></a>Patronen van geslaagd

De technische documentatie van de bovenstaande sectie in de Test Lab moet worden gezien en vóór de migratie van een echte verholpen.  Met voldoende testen is de migratie daadwerkelijk een gebeurtenisafhankelijke.  Voor productieomgevingen, is mogelijk handig om aanvullende ondersteuning, zoals een vertrouwde Microsoft-partner of Microsoft Premier-services.

### <a name="pitfalls-to-avoid"></a>Valkuilen om te voorkomen

Niet volledig testen kan problemen veroorzaken en vertraging in de migratie.  

## <a name="beyond-migration"></a>Na migratie

### <a name="technical-considerations-and-tradeoffs"></a>Technische overwegingen en de voor-en nadelen

Nu dat u in Azure Resource Manager, het maximaliseren van het platform.  Lees de [overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) voor meer informatie over extra voordelen.

Aandachtspunten:

- De migratie met andere activiteiten bundeling.  De meeste klanten ervoor kiezen voor een onderhoudsvenster van toepassing.  Als dit het geval is, wilt u mogelijk gebruikmaken van deze uitvaltijd voor andere Azure Resource Manager-mogelijkheden, zoals versleuteling en migratie naar schijven beheerd.
- Terugkeren naar de technische en zakelijke redenen voor Azure Resource Manager; Schakel de extra services alleen beschikbaar op Azure Resource Manager die betrekking hebben op uw omgeving.
- Moderniseren uw omgeving met PaaS-services.

### <a name="patterns-of-success"></a>Patronen van geslaagd

Worden doelgericht op welke services u nu wilt inschakelen in Azure Resource Manager.  Veel klanten vinden het hieronder dwingende voor Azure-omgevingen:

- [Toegangsbeheer op basis van rollen](../../azure-resource-manager/resource-group-overview.md#access-control).
- [Azure Resource Manager-sjablonen voor de implementatie van eenvoudiger en meer gecontroleerde](../../azure-resource-manager/resource-group-overview.md#template-deployment).
- [Labels](../../azure-resource-manager/resource-group-using-tags.md).
- [Controle van de activiteit](../../azure-resource-manager/resource-group-audit.md)
- [Bronbeleid](../../azure-resource-manager/resource-manager-policy.md)

### <a name="pitfalls-to-avoid"></a>Valkuilen om te voorkomen

Houd er rekening mee waarom u deze van klassiek naar Azure Resource Manager migratie reis gestart.  Wat zijn de oorspronkelijke zakelijke redenen? U de zakelijke reden bereiken?


## <a name="next-steps"></a>Volgende stappen

* [Overzicht van de migratie van IaaS resources van klassieke in Azure Resource Manager-platform ondersteund](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Technische deep dive op platform ondersteund migratie van klassiek naar Azure Resource Manager](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Planning voor de migratie van IaaS-resources van het klassieke implementatiemodel naar Azure Resource Manager](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [PowerShell gebruiken voor het migreren van IaaS-middelen van klassiek naar Azure Resource Manager](../windows/migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Community-hulpprogramma's voor hulp bij de migratie van IaaS-middelen van klassiek naar Azure Resource Manager](../windows/migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Bekijk de meest voorkomende migratiefouten](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Lees de veelgestelde vragen over IaaS Resourcemigratie van klassiek naar Azure Resource Manager](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
