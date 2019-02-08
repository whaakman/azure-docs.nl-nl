---
title: Status van de virtuele machine bewaken met Azure Monitor voor virtuele machines (preview) | Microsoft Docs
description: Dit artikel wordt beschreven hoe u de status van de virtuele machine en het onderliggende besturingssysteem met Azure Monitor begrijpen voor virtuele machines.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/01/2019
ms.author: magoedte
ms.openlocfilehash: 17fdcb4a57379fd11e841715a6908a4a0d280bd9
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55891467"
---
# <a name="understand-the-health-of-your-azure-virtual-machines-with-azure-monitor-for-vms-preview"></a>Meer informatie over de status van uw Azure virtual machines met Azure Monitor voor virtuele machines (preview)
Azure bevat meerdere services die afzonderlijk uitvoeren van een specifieke rol of de taak in de ruimte bewaking, maar biedt een perspectief gedetailleerde status van het besturingssysteem die worden gehost op Azure virtual machines niet beschikbaar was.  Terwijl u verschillende voorwaarden met behulp van Log Analytics of Azure Monitor controleren kan, zijn ze niet ontworpen om te modelleren en status van de belangrijkste onderdelen of algemene status van de virtuele machine vertegenwoordigen.  Met Azure Monitor voor virtuele machines de gezondheid van functie bewaakt het proactief de beschikbaarheid en prestaties van de Windows- of Linux Gast-OS met een model met daarin belangrijke onderdelen en hun relaties, criteria waarmee wordt aangegeven hoe u voor het meten van de status van deze onderdelen, en u waarschuwen wanneer een slechte status voorwaarde wordt gedetecteerd.  

De algehele status van de Azure-VM weergave en het onderliggende besturingssysteem kunnen worden waargenomen vanuit twee perspectieven met Azure Monitor status van de virtuele machines, rechtstreeks vanuit de virtuele machine of voor alle virtuele machines in een resourcegroep van Azure Monitor.

In dit artikel helpt u begrijpen hoe u snel beoordelen, onderzoeken en oplossen van statusproblemen gedetecteerd.

Zie voor meer informatie over het configureren van Azure Monitor voor virtuele machines [Azure Monitor inschakelen voor virtuele machines](vminsights-onboard.md).

>[!NOTE]
>Vanaf 11 februari 2019 zullen we u migreert vanaf het huidige statusmodel in Azure Monitor voor de functie de gezondheid van virtuele machines, dat weergegeven wordt wanneer u zich in de ervaring van de gezondheid van diagnostische gegevens van vandaag, naar een nieuwe versie van het statusmodel. Deze update verbetert de prestaties van de status rollup-verwerking en bevat een verfijnde statusmodel die zijn gepresenteerd in de statusweergave van de diagnostische gegevens. 
>
>Met het nieuwe statusmodel zijn rollup van onderliggende health criteria bovenliggende entiteit/niveau health criteria sneller en als gevolg hiervan de status van de bovenliggende updates naar de status van het gewenste of de doelserver met minder latentie. U kunt nog steeds de criteria voor beveiligingsstatus onder filteren de **prestaties** en **beschikbaarheid** categorieën in tegenstelling tot de vorige methode tabblad op basis van een categorie selecteren in de weergave.
>
>Raadpleeg voor meer informatie over de ervaring van de diagnostische gegevens over status, diagnostische gegevens over de status [sectie](#health-diagnostics) in dit artikel. 
>
>Deze update wordt verbeterd het volgende: 
>
>- Statustotalisatie verwerking met lagere latentie  
>- Sneller waarschuwen voor statuswijzigingen 
>- Sneller van de status in de weergave samengevoegde virtuele machine voor alle virtuele machines bijwerken 
>
>Er is geen regressie van alle functionaliteit die vandaag nog met de Health-functie van Azure Monitor worden geleverd voor VM's.

>Als gevolg van deze wijziging zullen er enkele wordt onderbroken voor een korte periode met de service en de statusgeschiedenis. De twee ervaringen in de gezondheid van diagnostische gegevens zijn getroffen - geschiedenis van de wijziging wordt opnieuw ingesteld en statuswijzigingen in de vorige voor criteria voor beveiligingsstatus niet meer beschikbaar voor controle in de kolom Status wijzigen van de Health-pagina voor diagnostische gegevens. Als u geïnteresseerd in de historische gegevens van een bent essentiële kritieke VM vervolgens u een schermafbeelding van de statusgegevens van de criteria en de bijbehorende statuswijzigingen ter referentie neemt. 

## <a name="monitoring-configuration-details"></a>Informatie over de configuratie controleren
In deze sectie geeft een overzicht van de standaard status criteria gedefinieerd voor het bewaken van Azure Windows en Linux-machines. Alle health criteria zijn vooraf geconfigureerd voor een waarschuwing wanneer het niet in orde voorwaarde wordt voldaan. 

### <a name="windows-vms"></a>Virtuele Windows-machines

- Beschikbaar geheugen in Megabytes 
- Gemiddelde seconden Per schrijfbewerking (logische schijf)
- Gemiddelde seconden Per schrijfbewerking (schijf)
- Gemiddelde logische schijf seconden Per leesbewerking
- Gemiddelde logische schijf seconden Per overdracht
- Gemiddelde seconden Per leesbewerking
- Gemiddelde seconden Per overdracht
- Huidige wachtrijlengte voor schijf (logische schijf)
- Huidige wachtrijlengte voor schijf (schijf)
- Percentage niet-actieve tijd van de schijf
- Bestandssysteemfout of beschadiging
- Beschikbare ruimte logische schijf (%) Laag
- Logische schijf beschikbare ruimte (MB): weinig
- Percentage niet-actieve tijd van de logische schijf
- Geheugenpagina's Per seconde
- Percentage bandbreedte gebruikt lezen
- Percentage bandbreedte gebruikt Totaal
- Percentage bandbreedte gebruikt schrijven
- Percentage toegewezen geheugen in gebruik
- Percentage niet-actieve tijd van de fysieke schijf
- Servicestatus voor DHCP-Client
- Status van DNS-Client-Service
- RPC-servicestatus
- Status van Server-Service
- Percentage van totale CPU-gebruik
- Status van Windows Event Log-Service
- Status van Windows Firewall-Service
- Status van Windows Remote Management-Service

### <a name="linux-vms"></a>Virtuele Linux-machines
- Gemiddelde van de schijf Schijfoverdrachten per seconde 
- Gemiddelde van de schijf Schijf sec/lezen 
- Gemiddelde van de schijf Schijf sec/schrijven 
- Status van de schijf
- Logical Disk Free Space
- Logische Schijf\percentage vrije ruimte
- Logische schijf % vrije Inodes
- Status van de netwerkadapter
- Percentage DPC-tijd van processor
- Percentage processortijd van processor
- Totale percentage processortijd
- Totale percentage DPC-tijd
- Besturingssysteem beschikbaar geheugen (MB)

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal
Meld u aan bij [Azure Portal](https://portal.azure.com). 

## <a name="introduction-to-health-experience"></a>Inleiding tot de Health-ervaring
Voordat u met behulp van de Health-functie voor een enkele virtuele machine of een groep virtuele machines, is het belangrijk bieden we een korte inleiding, zodat u begrijpt hoe de informatie wordt weergegeven en wat de visualisaties vertegenwoordigen.  

## <a name="view-health-directly-from-a-virtual-machine"></a>Weergave wordt de status rechtstreeks van een virtuele machine 
Als u de status van een Azure-VM, selecteer **inzichten (preview)** in het linkerdeelvenster van de virtuele machine. Klik op de pagina VM insights **Health** standaard open is en toont de statusweergave van de virtuele machine.  

![Azure Monitor voor een overzicht van virtuele machines van een geselecteerde Azure-machine](./media/vminsights-health/vminsights-directvm-health.png)

Op de **Health** tabblad onder de sectie **Gast-VM-status**, de tabel ziet u de huidige status van uw virtuele machine en het totale aantal VM-status waarschuwingen die worden gegenereerd door een slecht onderdeel. Raadpleeg de sectie waarschuwingen voor meer informatie over de ervaring voor waarschuwingen.  

De statussen die is gedefinieerd voor een virtuele machine worden in de volgende tabel beschreven: 

|Pictogram |Status |Betekenis |
|-----|-------------|------------|
| |In orde |De status is in orde als het is binnen de gedefinieerde statusaanduidingen, waarmee wordt aangegeven geen problemen vastgesteld voor de virtuele machine en deze zoals vereist werkt. Hiermee wordt de health-up en het is de beste of slechtste status van de onderliggende inclusief in het geval van een updatepakket bovenliggende monitor.|
| |Kritiek |De status is essentieel als deze niet binnen de gedefinieerde gezondheidstoestand, waarmee wordt aangegeven dat een of meer kritieke problemen zijn gedetecteerd, die moeten worden opgelost als u wilt herstellen van de normale functionaliteit. Hiermee wordt de health-up en het is de beste of slechtste status van de onderliggende inclusief in het geval van een updatepakket bovenliggende monitor.|
| |Waarschuwing |Status is een waarschuwing als tussen de twee drempelwaarden voor de voorwaarde gedefinieerde status is, waarbij een geeft aan dat een *waarschuwing* status en de andere geeft aan dat een *kritieke* (drie health state drempelwaarden kunnen de status worden geconfigureerd), of wanneer een niet-kritieke probleem wordt aangetroffen die kritieke problemen veroorzaken kunnen als niet is opgelost. In het geval van een updatepakket bovenliggende monitor, als er een of meer van de onderliggende objecten wordt een waarschuwingsstatus en vervolgens de bovenliggende weer *waarschuwing* staat. Als er een onderliggend object dat zich in een *kritieke* en een andere onderliggende in een *waarschuwing* status, de bovenliggende updatepakket wordt weergegeven een status van *kritieke*.|
| |Onbekend |Status is in een *onbekende* status wanneer de status kan niet worden berekend om verschillende redenen, zoals kan niet voor het verzamelen van gegevens, niet-geïnitialiseerde-service, enzovoort. Deze status kan niet worden geconfigureerd.| 

Selecteren **status diagnostische gegevens weergeven** opent een pagina weergegeven van alle onderdelen van de virtuele machine, criteria van de bijbehorende status, statuswijzigingen en andere belangrijke problemen aangetroffen door de bewaking van onderdelen die betrekking hebben op de virtuele machine. Zie voor meer informatie, [diagnostische gegevens over gezondheid](#health-diagnostics). 

Onder de **status onderdeel** sectie, de tabel ziet u een updatepakket status van de prestaties van primaire categorieën bewaakt door de criteria voor beveiligingsstatus voor deze gebieden, specifiek **CPU**,  **Geheugen**, **schijf**, en **netwerk**.  Een van de onderdelen te selecteren, wordt een overzicht van alle van de afzonderlijke status criterium bewaking aspecten van dit onderdeel en de bijbehorende status van elke pagina geopend.  

Bij het openen van status van een Azure-VM met het Windows-besturingssysteem, de status van de top 5 core Windows-services worden weergegeven onder de sectie **Core services health**.  Een van de services te selecteren, wordt een pagina weergegeven van de criteria voor beveiligingsstatus controleren dat onderdeel en de status geopend.  Het deelvenster met de eigenschap te klikken op de naam van de criteria voor servicestatus wordt geopend, en vanaf hier kunt u de configuratiegegevens, met inbegrip van de criteria voor beveiligingsstatus heeft een bijbehorende Azure Monitor-waarschuwing gedefinieerd bekijken. Zie voor meer informatie, [status diagnostische gegevens en werken met de criteria voor beveiligingsstatus](#health-diagnostics).  

## <a name="aggregate-virtual-machine-perspective"></a>Totale virtuele-machine-perspectief
Als u wilt weergeven voor verzamelen van statussen voor al uw virtuele machines in een resourcegroep in de navigatielijst in de portal, selecteert u **Azure Monitor** en selecteer vervolgens **virtuele Machines (preview)**.  

![VM-inzichten bewakingsweergave van Azure Monitor](./media/vminsights-health/vminsights-aggregate-health.png)

Uit de **abonnement** en **resourcegroep** vervolgkeuzelijsten, selecteer de juiste resourcegroep met de virtuele machines die met betrekking tot de groep, als u wilt weergeven van de gerapporteerde status.  Uw selectie wordt alleen toegepast op de Health-functie en wordt niet meegenomen naar de prestaties of de kaart.

Op de **Health** tabblad, kunt u de volgende informatie:

* Hoeveel virtuele machines zijn in een status Kritiek of slecht, ten opzichte van het aantal in orde is of niet verzenden gegevens zijn (aangeduid als een onbekende status)?
* Welke VM's door het besturingssysteem (OS) een slechte status rapporteren en hoeveel?
* Hoeveel virtuele machines niet in orde zijn vanwege een probleem aangetroffen met een processor, schijfruimte, geheugen of netwerkadapter, onderverdeeld op basis van status?  
* Hoeveel virtuele machines niet in orde zijn vanwege een probleem aangetroffen met een service core besturingssysteem, onderverdeeld op basis van status?

Hier kunt u snel identificeren van de bovenste kritieke problemen gedetecteerd door de criteria voor beveiligingsstatus proactief bewaken van de virtuele machine en bekijk de details voor VM-status-waarschuwing en bijbehorende kennisartikel bedoeld om te helpen bij de diagnose en herstel van het probleem.  Selecteer een van de ernstcategorieën openen de [alle waarschuwingen](../../azure-monitor/platform/alerts-overview.md#all-alerts-page) pagina gefilterd op basis van die ernst.

De **VM distributie per besturingssysteem** lijst ziet u VM's die door de editie van Windows of Linux-distributie, samen met hun versie weergegeven. In elke categorie besturingssysteem, de virtuele machines zijn uitgesplitst op basis van de status van de virtuele machine. 

![VM-inzichten virtuele machine distributie perspectief](./media/vminsights-health/vminsights-vmdistribution-by-os.png)

U kunt klikken op een willekeurig kolomvelditem - **aantal VM's**, **kritieke**, **waarschuwing**, **orde** of **onbekende** naar Inzoomen op de **virtuele Machines** pagina voor een overzicht van de gefilterde resultaten die overeenkomen met de geselecteerde kolom. Bijvoorbeeld, als we willen bekijken van alle virtuele machines met **Red Hat Enterprise Linux-release 7.5**, klikt u op de **aantal VM's** voor dat besturingssysteem en de volgende pagina wordt geopend met de virtuele machines die overeenkomt met de waarde met het filter en hun bekende status.  

![Voorbeeld van totalisering van Red Hat Linux-VM 's](./media/vminsights-health/vminsights-rollup-vm-rehl-01.png)
 
Op de **virtuele Machines** pagina als u de naam van een virtuele machine in de kolom selecteren **VM-naam**, u bent omgeleid naar de pagina van de VM-exemplaar met meer details van de waarschuwingen en statusproblemen criteria geïdentificeerd die zijn die betrekking hebben op de geselecteerde VM.  Hier kunt u de details van de health-status filteren door te klikken op **status** pictogram in de linkerbovenhoek van de pagina om te zien welke onderdelen niet in orde zijn, of u kunt VM-status waarschuwingen die worden gegenereerd door een slecht onderdeel weergeven gecategoriseerd door de ernst van waarschuwing.    

Van de VM-lijstweergave klikt op de naam van een virtuele machine wordt geopend de **Health** pagina voor geselecteerd VM, op dezelfde manier als wanneer u hebt geselecteerd **inzichten (preview)** van de virtuele machine rechtstreeks.

![Inzicht van de virtuele machine van een geselecteerde Azure-machine](./media/vminsights-health/vminsights-directvm-health.png)

Hier wordt een updatepakket **Integriteitsstatus** voor de virtuele machine en **waarschuwingen**, gecategoriseerd op ernst, die de VM-status waarschuwingen treedt op wanneer de status verandert van status naar slecht voor een criteria voor servicestatus.  Selecteren **virtuele machines in essentiële voorwaarde** opent een pagina met een lijst met een of meer virtuele machines die zich in een kritieke status heeft.  Te klikken op de status voor een van de virtuele machines in de lijst ziet u de **diagnostische gegevens over gezondheid** weergave van de virtuele machine.  U vindt hier informatie welke criteria health is een probleem met de status status spiegelen. Wanneer de **diagnostische gegevens over gezondheid** pagina wordt geopend, het bevat de onderdelen van de virtuele machine en de bijbehorende status criteria met de huidige status.  Raadpleeg de [Health diagnostische](#health-diagnostics) sectie voor meer informatie.  

Selecteren **weergeven van alle criteria voor beveiligingsstatus** opent een pagina met een lijst met alle health-criteria die beschikbaar zijn met deze functie.  De informatie kan verder worden gefilterd op basis van de volgende opties:

* **Type** : Er zijn drie typen van de gezondheid van criteria die u wilt beoordelen van de voorwaarden en getotaliseerde algehele status van de bewaakte virtuele machine.  
    a. **Eenheid** – meet een bepaald aspect van de virtuele machine. Dit criteriatype health mogelijk worden monitor een prestatiemeteritem controleren om te bepalen van de prestaties van het onderdeel, een script voor een synthetische transactie uitvoeren, of controleren op een gebeurtenis die duidt op een fout.  Het filter is standaard ingesteld op de eenheid.  
    b. **Afhankelijkheid** -monitor totaliseert de status tussen verschillende entiteiten. Deze criteria health kunnen de status van een entiteit afhankelijk is van de status van een ander type entiteit die deze afhankelijk is voor een goede werking.  
    c. **Cumulatieve** -biedt een gecombineerde status van de dezelfde status criteria. Criterium voor het health Unit- en afhankelijkheidsmonitors worden doorgaans geconfigureerd onder een criterium cumulatieve status. Naast het voorzien in betere algemene organisatie van de vele verschillende health criteria gericht op een entiteit, biedt cumulatieve status criterium een unieke status voor verschillende categorieën van de entiteiten.

* **Categorie** -Type van de van gezondheidscriteria voor de groep criteria voor hetzelfde type voor rapportagedoeleinden gebruikt.  Ze zijn **beschikbaarheid** of **prestaties**.

U kunt inzoomen verder naar beneden om te zien welke instanties zijn niet in orde door te klikken op een waarde onder de **niet in orde onderdeel** kolom.  Klik op de pagina bevat een tabel de onderdelen, die zich in een kritieke status heeft.    

## <a name="health-diagnostics"></a>Status diagnostische gegevens
Thge **diagnostische gegevens over gezondheid** op de pagina kunt u voor het visualiseren van het statusmodel van een virtuele machine, lijst van alle onderdelen van de virtuele machine, criteria voor beveiligingsstatus, statuswijzigingen, die zijn gekoppeld en andere belangrijke problemen geïdentificeerd door bewaakte onderdelen die betrekking hebben aan de virtuele machine.

![Voorbeeld van diagnostische gegevens van Health-pagina voor een virtuele machine](./media/vminsights-health/health-diagnostics-page-01.png)

U kunt de gezondheid van diagnostische gegevens starten in de volgende manieren.

* Op basis van status totaliseren voor alle virtuele machines van de cumulatieve perspectief van de virtuele machine in Azure Monitor.  Op de **health** pagina, klikt u op het pictogram voor **kritieke**, **waarschuwing**, **orde**, of **onbekende** status onder de sectie **Gast-VM-status** en Zoom in op de pagina met een lijst met alle virtuele machines die overeenkomen met die gefilterde categorie.  Te klikken op de waarde in de **status** kolom Status diagnostische gegevens binnen het bereik van die bepaalde virtuele machine wordt geopend.      

* Door het besturingssysteem van de cumulatieve perspectief van de virtuele machine in Azure Monitor. Onder **VM distributie**, het selecteren van een van de kolomwaarden wordt geopend de **virtuele Machines** pagina en geeft u een lijst in de tabel die overeenkomt met de gefilterde categorie.  Te klikken op de waarde onder **status** kolom wordt de gezondheid van diagnostische gegevens voor de geselecteerde virtuele machine geopend.    
 
* Van de Gast-VM op de Azure-Monitor voor virtuele machines **Health** tabblad hiervoor **status diagnostische gegevens weergeven** 

Diagnostische gegevens over gezondheid ordent statusgegevens in de volgende categorieën: 

* Beschikbaarheid
* Prestaties
 
Alle health criteria gedefinieerd voor een specifiek onderdeel, zoals logische schijf, CPU, enzovoort. Bovendien de categorie van de monitor kan worden gezien ernaast in het **Criteria voor beveiligingsstatus** kolom.  

Status van een health-criteria wordt gedefinieerd door een van de vier statussen – *kritieke*, *waarschuwing*, *orde*, en *onbekende*. De eerste drie kunnen worden geconfigureerd, wat betekent dat u kunt de drempelwaarden van de monitors met behulp van de [Workload Monitor API](https://docs.microsoft.com/rest/api/monitor/microsoft.workloadmonitor/monitors/update). *Onbekende* is niet kunnen worden geconfigureerd en is gereserveerd voor specifieke scenario's.  

Diagnostische gegevens statuspagina heeft drie belangrijkste secties:

* Componentmodel 
* Statuscriteria
* Statuswijzigingen 

![Secties van diagnostische gegevens van Health-pagina](./media/vminsights-health/health-diagnostics-page-02.png)

### <a name="component-model"></a>Onderdeelmodel
De meest linkse kolom in de pagina Status diagnostische gegevens is het Onderdeelmodel. Alle onderdelen, die gekoppeld aan de virtuele machine zijn, worden weergegeven in deze kolom, samen met hun huidige status. 

In het volgende voorbeeld worden de gedetecteerde onderdelen schijf, logische schijf, processor, geheugen en besturingssysteem. Meerdere exemplaren van deze onderdelen worden gedetecteerd en weergegeven in deze kolom. De onderstaande afbeelding ziet u bijvoorbeeld dat de virtuele machine heeft twee exemplaren van logische schijven - C: en D:, die zich in een foutloze toestand bevindt.  

![Onderdeel voorbeeldmodel weergegeven in de gezondheid van diagnostische gegevens](./media/vminsights-health/health-diagnostics-page-component.png)

### <a name="health-criteria"></a>Criteria voor servicestatus
De middelste kolom in de diagnostische gegevens van Health-pagina wordt de **Criteria voor beveiligingsstatus** kolom. Het statusmodel gedefinieerd voor de virtuele machine wordt weergegeven in een hiërarchische structuur. Het statusmodel van een virtuele machine bestaat uit eenheid en de van statistische gezondheidscriteria.  

![Voorbeeld van de status criteria die zijn gepresenteerd in de gezondheid van diagnostische gegevens](./media/vminsights-health/health-diagnostics-page-healthcriteria.png)

Een criterium status meet de status van het bewaakte-exemplaar met bepaalde criteria, wat erop kan een drempelwaarde, de status van een entiteit, enzovoort. Een criterium health heeft twee of drie configureerbare health state drempelwaarden, zoals eerder beschreven. Het criterium health kan op elk gewenst moment worden in slechts één potentiële status. 

De algemene status van een doel wordt bepaald door de status van elk van de health-criteria gedefinieerd in het status-model. Dit is een combinatie van criteria voor beveiligingsstatus gericht rechtstreeks op het doel, status-criteria die zijn gericht op onderdelen getotaliseerd in het doel via een criterium cumulatieve status. Deze hiërarchie wordt weergegeven in de **Criteria voor beveiligingsstatus** sectie van de diagnostische gegevens van Health-pagina. Het beleid voor statustotalisatie maakt deel uit van de configuratie van de cumulatieve status criteria (standaard is ingesteld op *slechtste van*). U vindt een lijst van de standaardset van criteria voor beveiligingsstatus uitgevoerd als onderdeel van deze functie onder de sectie [bewaking configuratiedetails](#monitoring-configuration-details).  

**Eenheid** Health criteriumtype kan hebben hun configuratie is gewijzigd door te klikken op de ellips koppeling naar de uiterst rechts en vervolgens **Details weergeven** om de configuratie van deelvenster te openen. 

![Een voorbeeld van een health criteria configureren](./media/vminsights-health/health-diagnostics-vm-example-02.png)

Met behulp van het voorbeeld in het deelvenster configuratie voor de geselecteerde status criteria **gemiddelde seconden Per schrijven**, de drempelwaarde kan worden geconfigureerd met een andere numerieke waarde. Het is een monitor voor twee statussen, wat betekent dat deze alleen de wijzigingen van goed in waarschuwing. Andere health-criterium mogelijk drie statussen, waarin u de waarde voor de statusdrempel waarschuwingen en kritieke status kunt configureren.  

>[!NOTE]
>Configuratiewijzigingen voor de gezondheid van criteria toepassen op één exemplaar wordt toegepast op alle gecontroleerde exemplaren.  Als u bijvoorbeeld **schijf D:-1** en wijzig de **gemiddelde seconden Per schrijven** drempelwaarde, geldt niet voor alleen dat exemplaar, maar alle instanties van andere schijf gedetecteerd en bewaakt op de virtuele machine.
>

![Configureren van een health-criteria van het voorbeeld van een unit-monitor](./media/vminsights-health/health-diagnostics-criteria-config-01.png)

Als u meer informatie over de status indicator wilt, Knowledge Base-artikelen worden te identificeren van problemen, oorzaken en oplossingen. Klik op de **informatie weergeven** koppeling op de pagina en er wordt een nieuw tabblad geopend in uw browser waarin het specifieke kennisartikel. Op elk gewenst moment kunt u bekijken alle van de status criterium Knowledge Base-artikelen opgenomen met Azure Monitor voor virtuele machines Health functie [hier](https://docs.microsoft.com/azure/monitoring/infrastructure-health/).
  
### <a name="state-changes"></a>Statuswijzigingen
De meest rechtse kolom in de diagnostische gegevens van Health-pagina **statuswijzigingen**. Geeft een lijst van alle wijzigingen die zijn gekoppeld aan de criteria voor beveiligingsstatus dat is geselecteerd in de **Criteria voor beveiligingsstatus** sectie of de wijziging in de status van de virtuele machine als een virtuele machine is geselecteerd in de **Onderdeelmodel** of **Criteria voor beveiligingsstatus** kolom van de tabel. 

![Voorbeeld van de wijzigingen die zijn gepresenteerd in de gezondheid van diagnostische gegevens](./media/vminsights-health/health-diagnostics-page-statechanges.png)

In deze sectie bestaat uit de status van de criteria en de bijbehorende tijd gesorteerd op basis van de meest recente status bovenaan.   

### <a name="association-of-component-model-health-criteria-and-state-change-columns"></a>Koppeling van Onderdeelmodel, Criteria voor servicestatus en status van kolommen wijzigen 
De drie kolommen zijn onderling met elkaar verbonden. Wanneer u de instantie van een gedetecteerde in selecteert de **Onderdeelmodel** sectie de **Criteria voor beveiligingsstatus** sectie naar de onderdeelweergave van dit wordt gefilterd en dienovereenkomstig de **statuswijziging**sectie wordt bijgewerkt op basis van de status van de geselecteerde criteria. 

![Voorbeeld van het bewaakte exemplaar en de resultaten selecteren](./media/vminsights-health/health-diagnostics-vm-example-01.png)

In het bovenstaande voorbeeld, wanneer u selecteert **schijf - 1 D:**, de structuur van de Criteria voor servicestatus wordt gefilterd op **schijf - 1 D:**. De **van Statuswijzigingsgebeurtenissen voor** kolom ziet u de wijziging in de status op basis van de beschikbaarheid van **schijf - 1 D:**. 

Als u wilt een bijgewerkte status zien, kunt u de diagnostische gegevens van Health-pagina vernieuwen door te klikken op de **vernieuwen** koppeling.  Als er een update aan de status van de health-criterium op basis van het vooraf gedefinieerde polling-interval, wordt deze taak kunt u om te voorkomen dat de wachtrij en geeft de status van de meest recente.  De **Criteria status** een filter is zodat u kunt het bereik van de resultaten op basis van de geselecteerde status - *orde*, *waarschuwing*, *kritieke*, *Onbekende*, en *alle*.  De **laatst bijgewerkt** tijd in de rechterbovenhoek vertegenwoordigt de laatste tijd wanneer de diagnostische gegevens van Health-pagina is vernieuwd.  

## <a name="alerts"></a>Waarschuwingen
Azure Monitor voor de functie van de status van de virtuele machines worden geïntegreerd met [Azure-waarschuwingen](../../azure-monitor/platform/alerts-overview.md) en genereert een waarschuwing wanneer de status van de vooraf gedefinieerde criteria van goed in in een slechte status wijzigt als de voorwaarde wordt gedetecteerd. Waarschuwingen worden gecategoriseerd op basis van ernst - Sev 0 t/m 4, met Sev 0 staat voor het hoogste ernstniveau.  

Totale aantal VM-status waarschuwingen op basis van ernst worden gecategoriseerd is beschikbaar op de **Health** dashboard onder de sectie **waarschuwingen**. Wanneer u het totale aantal waarschuwingen ofwel het nummer overeenkomt met een ernstniveau selecteert de **waarschuwingen** pagina wordt geopend en geeft een lijst van alle waarschuwingen die overeenkomen met uw selectie.  Bijvoorbeeld, als u hebt geselecteerd de rij die overeenkomt met **ernst niveau 1**, ziet u de volgende weergave:

![Voorbeeld van alle ernst niveau 1-waarschuwingen](./media/vminsights-health/vminsights-sev1-alerts-01.png)

Op de **waarschuwingen** pagina, het heeft niet alleen een bereik om weer te geven van waarschuwingen die overeenkomen met uw selectie, maar ook worden gefilterd op **resourcetype** alleen health u waarschuwingen wilt weergeven die worden gegenereerd door de bron van de virtuele machine.  Dit wordt weergegeven in de lijst met waarschuwingen, onder de kolom **Doelresource**, waarbij u hier ziet u de Azure-VM die de waarschuwing is gegeven voor wanneer de status van bepaalde-criteria niet in orde voorwaarde is voldaan.  

Waarschuwingen van andere resourcetypen of services zijn niet bedoeld om te worden opgenomen in deze weergave, zoals waarschuwingen op basis van Log Analytics query's of metrische die waarschuwingen u normaal zou weergeven uit het standaardbeleid voor Azure Monitor [alle waarschuwingen](../../azure-monitor/platform/alerts-overview.md#all-alerts-page) pagina. 

U kunt deze weergave filteren op waarden selecteren in het vervolgkeuzemenu's aan de bovenkant van de pagina.

|Kolom |Description | 
|-------|------------| 
|Abonnement |Selecteer een Azure-abonnement. Alleen waarschuwingen in het geselecteerde abonnement zijn opgenomen in de weergave. | 
|Resourcegroep |Selecteer één resourcegroep bestaan. Alleen waarschuwingen met doelen in de geselecteerde resourcegroep zijn opgenomen in de weergave. | 
|Resourcetype |Selecteer een of meer resourcetypen. Standaard worden alleen waarschuwingen van de doel- **virtuele machines** is geselecteerd en opgenomen in deze weergave. Deze kolom is alleen beschikbaar nadat u een resourcegroep is opgegeven. | 
|Resource |Selecteer een resource. Alleen waarschuwingen met die bron als doel zijn opgenomen in de weergave. Deze kolom is alleen beschikbaar nadat u een resourcetype is opgegeven. | 
|Severity |aangeven of u de ernst van een waarschuwing of selecteer *alle* om op te nemen van waarschuwingen van alle ernstcategorieën. | 
|Monitorconditie |Selecteer een controleconditie voor het filteren van waarschuwingen als ze zijn *Fired* door het systeem of *opgelost* door het systeem als de voorwaarde niet langer actief is. Of selecteer *alle* om op te nemen van waarschuwingen van alle voorwaarden. | 
|Waarschuwingsstatus |Selecteer een waarschuwingsstatus *nieuw*, *bevestigen*, *gesloten*, of selecteer *alle* om op te nemen van waarschuwingen van alle Staten. | 
|Service bewaken |Selecteer een service of selecteer *alle* om op te nemen alle services. Alleen waarschuwingen uit *VM Insights* worden ondersteund voor deze functie.| 
|Tijdsbereik| Alleen waarschuwingen geactiveerd in het geselecteerde tijdvenster zijn opgenomen in de weergave. Ondersteunde waarden zijn het afgelopen uur, de afgelopen 24 uur, de afgelopen 7 dagen en de afgelopen 30 dagen. | 

De **waarschuwen details** pagina wordt weergegeven wanneer u een waarschuwing geven details van de waarschuwing en zodat u kunt de status te veranderen. Zie voor meer informatie over het beheren van waarschuwingen, [maken, weergeven en beheren van waarschuwingen via Azure Monitor](../../azure-monitor/platform/alerts-metric.md).  

>[!NOTE]
>Op deze tijd, het maken van nieuwe waarschuwingen op basis van criteria voor beveiligingsstatus of wijzigen van de gezondheid van de bestaande regels voor waarschuwingen in Azure Monitor vanuit de portal wordt niet ondersteund.  
>

![Deelvenster met Waarschuwingsdetails voor een geselecteerde waarschuwing](./media/vminsights-health/alert-details-pane-01.png)

Waarschuwing status kan ook worden gewijzigd voor een of meer waarschuwingen door ze te selecteren en vervolgens de optie **in een statuswijziging** uit de **alle waarschuwingen** pagina in de linkerbovenhoek. Op de **Waarschuwingsstatus wijzigen** deelvenster selecteert u een van de Staten toevoegen een beschrijving van de wijziging in de **Opmerking** veld en klik vervolgens op **Ok** uw wijzigingen. Hoewel de informatie wordt gecontroleerd en de wijzigingen zijn toegepast, u kunt de voortgang bijhouden onder **meldingen** in het menu.  

## <a name="next-steps"></a>Volgende stappen
Voor het identificeren van knelpunten en het algehele gebruik met de prestaties van uw VM's, Zie [weergave Azure VM-prestaties](vminsights-performance.md), of als u afhankelijkheden van gedetecteerde toepassingen, Zie [weergave Azure Monitor voor virtuele machines kaart](vminsights-maps.md). 
