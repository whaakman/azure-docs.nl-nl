---
title: De status van uw virtuele Azure-machines begrijpen | Microsoft Docs
description: In dit artikel wordt beschreven hoe u meer informatie over de status van virtuele machines en onderliggende besturingssystemen met Azure Monitor voor virtuele machines.
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
ms.date: 05/22/2019
ms.author: magoedte
ms.openlocfilehash: 2bf891f8cfecbb9e78e511dcee7ed1c61c170016
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/24/2019
ms.locfileid: "67340149"
---
# <a name="understand-the-health-of-your-azure-virtual-machines"></a>De status van uw virtuele Azure-machines begrijpen

Azure services voor specifieke rollen of taken in de ruimte bewaking bevat, maar het biedt geen gedetailleerde status perspectieven van besturingssystemen (OSs) die worden gehost op Azure virtual machines (VM's). U kunt Azure Monitor gebruiken voor verschillende voorwaarden, is het niet ontworpen modelleren en vertegenwoordigt de status van de belangrijkste onderdelen of de algemene status van virtuele machines.

Met behulp van Azure Monitor voor de gezondheid van virtuele machines, kunt u de beschikbaarheid en prestaties van een Windows- of Linux-Gast OS actief bewaken. De health-functie maakt gebruik van een model dat staat voor belangrijke onderdelen en hun relaties, vindt u de criteria die bepaalt hoe de onderdeelstatus meten en verzendt een waarschuwing wanneer er een slecht voorwaarde wordt gedetecteerd.

Weergeven van de algehele status van een Azure-VM en het onderliggende besturingssysteem kan worden waargenomen vanuit twee perspectieven: rechtstreeks vanuit een virtuele machine of voor alle virtuele machines in een resourcegroep van Azure Monitor.

Dit artikel leest hoe u snel beoordelen, onderzoeken en oplossen van statusproblemen wanneer ze worden gedetecteerd door de Azure-Monitor voor status-functie voor virtuele machines.

Zie voor meer informatie over het configureren van Azure Monitor voor virtuele machines [Azure Monitor inschakelen voor virtuele machines](vminsights-enable-overview.md).

## <a name="monitoring-configuration-details"></a>Informatie over de configuratie controleren

In deze sectie geeft een overzicht van de standaard status criteria voor het bewaken van Azure Windows en Linux-machines. Alle health criteria zijn vooraf geconfigureerd voor het verzenden van een waarschuwing wordt geactiveerd wanneer ze een slechte status voorwaarde detecteren.

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
- Percentage niet-actieve tijd van de schijf
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
- Totale percentage processortijd
- Besturingssysteem beschikbaar geheugen (MB)

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Om aan te melden, gaat u naar de [Azure-portal](https://portal.azure.com).

## <a name="introduction-to-azure-monitor-for-vms-health"></a>Inleiding tot Azure Monitor voor de gezondheid van virtuele machines

Voordat u de health-functie voor een enkele virtuele machine of een groep virtuele machines gebruiken, is het belangrijk om te begrijpen hoe de informatie wordt weergegeven en wat de visualisaties vertegenwoordigen.

### <a name="view-health-directly-from-a-vm"></a>Weergave wordt de status rechtstreeks van een virtuele machine

Als u de status van een Azure-VM, selecteer **inzichten (preview)** in het linkerdeelvenster van de virtuele machine. Op de pagina van de inzichten die virtuele machine de **Health** tabblad is standaard geopend en toont de statusweergave van de virtuele machine.

![Azure Monitor voor een overzicht van virtuele machines van een geselecteerde Azure-machine](./media/vminsights-health/vminsights-directvm-health.png)

In de **Health** tabblad onder **Gast-VM-status**, de tabel toont de status van de virtuele machine en het totale aantal VM-statusmeldingen die worden gegenereerd door een slecht onderdeel.

Zie voor meer informatie, [waarschuwingen](#alerts).

De statussen die is gedefinieerd voor een virtuele machine worden in de volgende tabel beschreven:

|Pictogram |Status |Betekenis |
|-----|-------------|---------------|
| |In orde |De VM zich in de status van de gedefinieerde voorwaarden. Deze status geeft aan dat er geen problemen worden gedetecteerd en de virtuele machine normaal functioneert. Met een totaliseringsmonitor bovenliggende status wordt getotaliseerd en weerspiegelt de beste of slechtste status van de onderliggende.|
| |Kritiek |De status is niet binnen de gedefinieerde gezondheidstoestand, die aangeeft dat een of meer kritieke problemen zijn gedetecteerd. Deze problemen moeten worden opgelost als u wilt herstellen van de normale functionaliteit. Met een updatepakket bovenliggende monitor de status getotaliseerd en weerspiegelt de beste of slechtste status van de onderliggende.|
| |Waarschuwing |De status is tussen de twee drempelwaarden voor de voorwaarde gedefinieerde status, waarbij een geeft aan een waarschuwingsstatus en de andere geeft aan dat een kritieke status (de drie health state drempelwaarden kunnen worden geconfigureerd), of wanneer een probleem met niet-kritieke kritieke problemen kan veroorzaken als niet-omgezette. Met een totaliseringsmonitor bovenliggende als een of meer onderliggende items in de waarschuwingsstatus heeft, is weer de bovenliggende een waarschuwingsstatus. Als één onderliggend item zich in een kritieke status en een andere onderliggende in de waarschuwingsstatus heeft, ziet u het updatepakket bovenliggende de status als kritiek.|
| |Onbekend |De status kan niet worden berekend om verschillende redenen. De volgende sectie biedt extra informatie en mogelijke oplossingen. |

Een onbekende status kan worden veroorzaakt door de volgende problemen:

- De agent opnieuw is geconfigureerd en niet meer rapporten in de werkruimte opgegeven wanneer de Azure-Monitor voor virtuele machines is ingeschakeld. Het configureren van de agent om te rapporteren aan de werkruimte zien, [toevoegen of verwijderen van een werkruimte](../platform/agent-manage.md#adding-or-removing-a-workspace).
- De virtuele machine is verwijderd.
- De werkruimte die is gekoppeld aan Azure Monitor voor virtuele machines is verwijderd. Als u Premier-ondersteuning hebt, kunt u de werkruimte herstellen. Ga naar [Premier](https://premier.microsoft.com/) en open een ondersteuningsaanvraag.
- De oplossing afhankelijkheden zijn verwijderd. Als u wilt de ServiceMap en InfrastructureInsights oplossingen in uw Log Analytics-werkruimte opnieuw inschakelt, installeert u deze oplossingen opnieuw met behulp van de [Azure Resource Manager-sjabloon](vminsights-enable-at-scale-powershell.md#install-the-servicemap-and-infrastructureinsights-solutions). Of gebruik de optie werkruimte configureren op het tabblad aan de slag.
- De virtuele machine is afgesloten.
- De virtuele machine van Azure-service is niet beschikbaar of onderhoud wordt uitgevoerd.
- De werkruimte [dagelijkse gegevens of retentie limiet](../platform/manage-cost-storage.md) is voldaan.

Selecteer **status diagnostische gegevens weergeven** om een pagina weergegeven van alle onderdelen van een virtuele machine, te openen die zijn gekoppeld criteria voor beveiligingsstatus, statuswijzigingen en andere problemen gedetecteerd door de bewaking van onderdelen die betrekking hebben op de virtuele machine.

Zie voor meer informatie, [diagnostische gegevens over gezondheid](#health-diagnostics).

In de **Health** sectie een tabel ziet u de totalisering van status van onderdelen van de prestaties bewaakt door de criteria voor servicestatus. Deze onderdelen zijn onder andere **CPU**, **geheugen**, **schijf**, en **netwerk**. Een onderdeel selecteren opent een pagina met een lijst met alle het criterium voor controle en de status van dit onderdeel.

Wanneer u de gezondheid van een Azure-VM waarop Windows wordt uitgevoerd, de status van de top vijf belangrijke Windows-services wordt weergegeven onder **Core services health**. Wanneer u een van de services, wordt een pagina met een lijst met de criteria voor beveiligingsstatus controleren voor het betreffende onderdeel samen met de status geopend.

De naam van de health-criteria selecteren, opent u het deelvenster de eigenschap. In dit deelvenster kunt u de configuratiegegevens, met inbegrip van de criteria voor beveiligingsstatus hebben een corresponderende Azure Monitor-waarschuwing bekijken.

Zie voor meer informatie, [status diagnostische gegevens en werken met de criteria voor beveiligingsstatus](#health-diagnostics).

### <a name="aggregate-vm-perspective"></a>Totale VM-perspectief

Als u wilt weergeven voor verzamelen van de statussen voor alle virtuele machines in een resourcegroep, selecteer **Azure Monitor** in de navigatielijst in de portal en selecteer vervolgens **virtuele Machines (preview)** .

![VM-inzichten bewakingsweergave van Azure Monitor](./media/vminsights-health/vminsights-aggregate-health.png)

In de **abonnement** en **resourcegroep** vervolgkeuzelijsten, selecteer de juiste resourcegroep met de virtuele machines die met betrekking tot de groep om hun gerapporteerde status weer te geven. Uw selectie wordt alleen toegepast op de functie voor health en niet meegenomen naar **prestaties** of **kaart** tabbladen.

De **Health** tabblad bevat de volgende informatie:

* Hoeveel virtuele machines zich bevinden in een status Kritiek of slecht, ten opzichte van het aantal in orde zijn of niet verzenden van gegevens (aangeduid als een onbekende status).
* Welke en hoeveel VM's door het besturingssysteem een slechte status rapporteren.
* Hoeveel virtuele machines vanwege een probleem aangetroffen met een processor, schijfruimte, geheugen of netwerkadapter, onderverdeeld naar de status niet in orde zijn.
* Hoeveel virtuele machines vanwege een probleem aangetroffen met een core OS-service, onderverdeeld naar de status niet in orde zijn.

Op de **Health** tabblad kunt u de kritieke problemen gedetecteerd door de criteria voor beveiligingsstatus controleren van de virtuele machine, en de Waarschuwingsdetails controleren en de bijbehorende Knowledge Base-artikelen identificeren. Deze artikelen kunnen helpen bij de diagnose en herstel van problemen. Selecteer een van de ernstcategorieën openen de [alle waarschuwingen](../../azure-monitor/platform/alerts-overview.md#all-alerts-page) pagina gefilterd op basis van die ernst.

De **VM distributie per besturingssysteem** lijst ziet u VM's die door de editie van Windows of Linux-distributie, samen met hun versie weergegeven. In elke categorie OS, de virtuele machines zijn uitgesplitst op basis van de status van de virtuele machine.

![VM-inzichten virtuele machine distributie perspectief](./media/vminsights-health/vminsights-vmdistribution-by-os.png)

Selecteer een kolom met inbegrip van **aantal VM's**, **kritieke**, **waarschuwing**, **orde**, of **onbekende**. De lijst weergeven met de gefilterde resultaten in de **virtuele Machines** pagina die overeenkomen met de geselecteerde kolom.

Bijvoorbeeld, als u wilt controleren van alle virtuele machines met Red Hat Enterprise Linux-release 7.5, selecteer de **aantal VM's** waarde voor dat besturingssysteem, en er wordt een lijst met de virtuele machines die overeenkomt met het filter en hun huidige status.

![Voorbeeld van totalisering van Red Hat Linux-VM 's](./media/vminsights-health/vminsights-rollup-vm-rehl-01.png)

In de **virtuele Machines** pagina als u de naam van een virtuele machine in de kolom selecteren **VM-naam**, u bent omgeleid naar de **VM-exemplaar** pagina. Deze pagina vindt u meer informatie over de waarschuwingen en statusproblemen van criteria die zijn die betrekking hebben op de geselecteerde VM. De details van de health-status filteren door te selecteren **status** pictogram in de linkerbovenhoek van de pagina om te zien welke onderdelen zijn niet in orde. U kunt ook VM-status waarschuwingen die worden gegenereerd door een slecht onderdeel onderverdeeld naar de ernst van waarschuwing weergeven.

Uit de **VM-lijst** weergeven, selecteert u de naam van een virtuele machine en opent de **Health** pagina voor die VM op dezelfde manier als wanneer u hebt geselecteerd **inzichten (preview)** van de virtuele machine rechtstreeks.

![Inzicht van de virtuele machine van een geselecteerde Azure-machine](./media/vminsights-health/vminsights-directvm-health.png)

De **inzichten (preview)** pagina ziet u een updatepakket health-status voor de virtuele machine en waarschuwingen. Deze health-status wordt gecategoriseerd door de ernst, waarmee de virtuele machine statusmeldingen treedt op wanneer de status van goed in worden gewijzigd naar slecht, op basis van criteria. Selecteren **virtuele machines in essentiële voorwaarde** opent een pagina met een lijst met een of meer virtuele machines in een kritieke status heeft.

Selecteren van de status voor een van de virtuele machines ziet de **diagnostische gegevens over gezondheid** weergave van de virtuele machine. In deze weergave kunt u bepalen welke criteria health is een probleem met de status spiegelen. Wanneer de **diagnostische gegevens over gezondheid** pagina wordt geopend, wordt alle onderdelen van de virtuele machine en de bijbehorende status criteria met de huidige status.

Zie voor meer informatie, [diagnostische gegevens over gezondheid](#health-diagnostics).

Selecteren **weergeven van alle criteria voor beveiligingsstatus** opent een pagina met een lijst met alle health-criteria die beschikbaar zijn met deze functie. De informatie kan verder worden gefilterd op basis van de volgende opties:

* **Type**. Er zijn drie typen criteria voorwaarden beoordelen en de algehele status van een bewaakte VM getotaliseerde status:
    - **Eenheid**. Meet een bepaald aspect van een virtuele machine. Dit criteriatype health mogelijk worden monitor een prestatiemeteritem controleren om te bepalen van de prestaties van het onderdeel, een script voor een synthetische transactie uitvoeren of een op een gebeurtenis die duidt op een fout. Het filter is standaard ingesteld op de eenheid.
    - **Afhankelijkheid**. Een totalisering van status tussen verschillende entiteiten bevat. Deze criteria health kunnen de status van een entiteit afhankelijk is van de status van een ander type entiteit die deze afhankelijk is voor een goede werking.
    - **Aggregate**. Biedt een gecombineerde status van de dezelfde status criteria. Unit- en afhankelijkheidsmonitors health criterium worden doorgaans geconfigureerd onder een criterium cumulatieve status. Naast het voorzien in betere algemene organisatie van de vele verschillende health criteria gericht op een entiteit, biedt cumulatieve status criterium een unieke status voor verschillende categorieën van de entiteiten.

* **Categorie**. Het type van de criteria voor beveiligingsstatus gebruikt om dezelfde criteria voor rapportagedoeleinden gebruikt. Deze categorieën worden **beschikbaarheid** en **prestaties**.

Als u wilt zien welke instanties niet in orde zijn, selecteert u een waarde onder de **niet in orde onderdeel** kolom. Op deze pagina bevat een tabel de onderdelen die zich in een kritieke status heeft.

## <a name="health-diagnostics"></a>Status diagnostische gegevens

De **diagnostische gegevens over gezondheid** op de pagina kunt u voor het visualiseren van het statusmodel van een virtuele machine. Deze pagina geeft een lijst van alle VM-onderdelen, criteria van de bijbehorende status, statuswijzigingen en andere belangrijke problemen geïdentificeerd door de bewaakte onderdelen die betrekking hebben op de virtuele machine.

![Voorbeeld van diagnostische gegevens van Health-pagina voor een virtuele machine](./media/vminsights-health/health-diagnostics-page-01.png)

Status diagnostische gegevens starten met behulp van de volgende methoden:

* Op basis van status totaliseren voor alle virtuele machines van de cumulatieve perspectief van de virtuele machine in Azure Monitor:

    1. Op de **Health** pagina, selecteert u het pictogram voor **kritieke**, **waarschuwing**, **orde**, of **onbekende** status onder de sectie **Gast-VM-status**.
    2. Ga naar de pagina met een lijst met alle virtuele machines die overeenkomen met die gefilterde categorie.
    3. Selecteer de waarde in de **status** kolom om te openen van de diagnostische gegevens over de status binnen het bereik van die virtuele machine.

* Door het besturingssysteem van de cumulatieve perspectief van de virtuele machine in Azure Monitor. Onder **VM distributie**, het selecteren van een van de kolomwaarden wordt geopend de **virtuele Machines** pagina en geeft u een lijst in de tabel die overeenkomt met de gefilterde categorie. De waarde onder **status** kolom wordt de gezondheid van diagnostische gegevens voor de geselecteerde virtuele machine geopend.
 
* Van de Gast-VM op de Azure-Monitor voor virtuele machines **Health** tabblad hiervoor **status diagnostische gegevens weergeven**.

Diagnostische gegevens over gezondheid ordent statusgegevens in twee categorieën: beschikbaarheid en prestaties.
 
Alle health criteria gedefinieerd voor een onderdeel, zoals logische schijf, CPU, enzovoort, kunnen zonder te filteren op de twee categorieën worden weergegeven. Deze weergaven kunnen zich in een volledige weergave van de criteria of via de resultaten te filteren op een categorie wanneer u selecteert **beschikbaarheid** of **prestaties**.

Ook, naast de categorie criteria kunt gezien de **Criteria voor beveiligingsstatus** kolom. Als de criteria die niet overeenkomen met de geselecteerde categorie, een bericht weergegeven dat **niet beschikbaar voor de geselecteerde categorie van de criteria voor beveiligingsstatus** wordt weergegeven in de **Criteria voor beveiligingsstatus** kolom.

De status van een health-criteria wordt gedefinieerd door een van de vier typen: **Kritieke**, **waarschuwing**, **in orde**, en **onbekende**. De eerste drie kunnen worden geconfigureerd, wat betekent dat kunt u de drempelwaarden van de monitors die rechtstreeks in de **Criteria voor beveiligingsstatus** deelvenster configuratie. Dit is ook mogelijk met behulp van de Azure Monitor REST API [monitor de updatebewerking](https://docs.microsoft.com/rest/api/monitor/microsoft.workloadmonitor/monitors/update). **Onbekende** niet kunnen worden geconfigureerd en is gereserveerd voor specifieke scenario's.

De **diagnostische gegevens over gezondheid** pagina heeft drie belangrijkste secties:

* Onderdeelmodel
* Statuscriteria
* Statuswijzigingen

![Secties van diagnostische gegevens van Health-pagina](./media/vminsights-health/health-diagnostics-page-02.png)

### <a name="component-model"></a>Onderdeelmodel

De meest linkse kolom in de **diagnostische gegevens over gezondheid** pagina is **Onderdeelmodel**. Alle onderdelen die gekoppeld aan de virtuele machine zijn, worden weergegeven in deze kolom, samen met hun huidige status.

In het volgende voorbeeld wordt de gedetecteerde onderdelen zijn **schijf**, **logische schijf**, **Processor**, **geheugen**, en  **Besturingssysteem**. Meerdere exemplaren van deze onderdelen worden gedetecteerd en weergegeven in deze kolom.

Bijvoorbeeld, de volgende afbeelding ziet u dat de virtuele machine twee exemplaren van logische schijven heeft **C:** en **D:** , die zich in een foutloze toestand bevindt:

![Onderdeel voorbeeldmodel weergegeven in de gezondheid van diagnostische gegevens](./media/vminsights-health/health-diagnostics-page-component.png)

### <a name="health-criteria"></a>Criteria voor servicestatus

De middelste kolom in de diagnostische gegevens van Health-pagina is **Criteria voor beveiligingsstatus**. Het statusmodel gedefinieerd voor de virtuele machine wordt weergegeven in een hiërarchische structuur. Het statusmodel van een virtuele machine bestaat uit eenheid en de van statistische gezondheidscriteria.

![Voorbeeld van de status criteria die zijn gepresenteerd in de gezondheid van diagnostische gegevens](./media/vminsights-health/health-diagnostics-page-healthcriteria.png)

Een criterium status meet de status van een bewaakte exemplaar, wat erop kan een drempelwaarde, de status van een entiteit, enzovoort. Een criterium health heeft twee of drie configureerbare health state drempelwaarden, zoals eerder beschreven. Het criterium health kan op elk gewenst moment worden in slechts één potentiële status.

Het statusmodel definieert criteria om te bepalen van de status van het algehele doel en de onderdelen van het doel. De hiërarchie van criteria wordt weergegeven in de **Criteria voor beveiligingsstatus** sectie op de **diagnostische gegevens over gezondheid** pagina.

Het beleid voor totalisering van status maakt deel uit van de configuratie van de cumulatieve status criteria (de standaardwaarde is ingesteld op **slechtste van**). U vindt een standaardset aan criteria voor beveiligingsstatus uitgevoerd als onderdeel van deze functie in de [bewaking configuratiedetails](#monitoring-configuration-details) sectie van dit artikel.

U kunt ook de REST-API van Azure Monitor gebruiken [monitor exemplaren lijst door resource](https://docs.microsoft.com/rest/api/monitor/microsoft.workloadmonitor/monitorinstances/listbyresource) voor een lijst van alle health criteria. Deze criteria bevat informatie over de configuratie uitvoeren op basis van de Azure VM-resource.

De **eenheid** criteriumtype health kan de configuratie ervan gewijzigd door de drie puntjes aan de rechterkant klikken hebben. Selecteer **Details weergeven** om de configuratie van deelvenster te openen.

![Een voorbeeld van een health criteria configureren](./media/vminsights-health/health-diagnostics-vm-example-02.png)

In het deelvenster configuratie voor de status van de geselecteerde criteria, als u het voorbeeld **gemiddelde seconden Per schrijven**, de drempelwaarde kan worden geconfigureerd met een andere numerieke waarde. Het is een monitor voor twee statussen, wat betekent dat deze kan alleen wijzigen vanuit **orde** naar **waarschuwing**.

Andere criteria health maken soms gebruik drie statussen, waarin u de waarde voor waarschuwingen en kritieke drempelwaarden voor status kunt configureren. U kunt ook een drempel wijzigen met behulp van Azure Monitor REST API [monitorconfiguratie](https://docs.microsoft.com/rest/api/monitor/microsoft.workloadmonitor/monitors/update).

>[!NOTE]
>Configuratiewijzigingen voor de gezondheid van criteria toepassen op één exemplaar, wordt deze toegepast op alle gecontroleerde exemplaren. Als u bijvoorbeeld **schijf D:-1** en wijzig vervolgens de **gemiddelde seconden Per schrijven** drempelwaarde, de wijziging wordt toegepast op alle exemplaren gedetecteerd en bewaakt op de virtuele machine.


![Configureren van een health-criteria van het voorbeeld van een unit-monitor](./media/vminsights-health/health-diagnostics-criteria-config-01.png)

Als u meer informatie over de criteria voor beveiligingsstatus wilt, hebben we opgenomen Knowledge Base-artikelen als u wilt vaststellen, problemen, oorzaken en oplossingen. Selecteer **informatie weergeven** op de pagina om te zien van de gerelateerde kennisartikel.

Alle Knowledge Base-artikelen die worden opgenomen met Azure Monitor voor de gezondheid van VM's, Zie [documentatie over de kennis van de Azure Monitor health](https://docs.microsoft.com/azure/monitoring/infrastructure-health/).

### <a name="state-changes"></a>Statuswijzigingen

De kolom helemaal rechts van de **diagnostische gegevens over gezondheid** pagina is **statuswijzigingen**. Deze kolom bevat alle wijzigingen die zijn gekoppeld aan de criteria voor beveiligingsstatus geselecteerd in de **Criteria voor beveiligingsstatus** sectie of de wijziging in de status van de virtuele machine als een virtuele machine is geselecteerd in de **Onderdeelmodel** of  **Criteria voor beveiligingsstatus** kolom van de tabel.

![Voorbeeld van de wijzigingen die zijn gepresenteerd in de gezondheid van diagnostische gegevens](./media/vminsights-health/health-diagnostics-page-statechanges.png)

De volgende sectie ziet u de status van de criteria voor servicestatus en de bijbehorende tijd. Deze informatie toont de meest recente status aan de bovenkant van de kolom.

### <a name="association-of-component-model-health-criteria-and-state-changes-columns"></a>Koppeling van de kolommen Onderdeelmodel, Criteria voor servicestatus en statuswijzigingen

De drie kolommen zijn onderling met elkaar verbonden. Wanneer u een exemplaar in de **Onderdeelmodel** kolom de **Criteria voor beveiligingsstatus** kolom wordt gefilterd op dat onderdeel bekijken. Dienovereenkomstig, de **statuswijzigingen** kolom wordt bijgewerkt op basis van de status van de geselecteerde criteria.

![Voorbeeld van het bewaakte exemplaar en de resultaten selecteren](./media/vminsights-health/health-diagnostics-vm-example-01.png)

Als u bijvoorbeeld *schijf - 1 D:* in de lijst onder **Onderdeelmodel**, **Criteria voor beveiligingsstatus** -filters op *schijf - 1 D:* , en  **Status van wijzigingen** toont de wijziging in de status op basis van de beschikbaarheid van *schijf - 1 D:* .

Als u wilt een bijgewerkte status zien, kunt u de diagnostische gegevens van Health-pagina vernieuwen door het selecteren van de **vernieuwen** koppeling. Als er een update aan de status van de health-criterium op basis van het vooraf gedefinieerde polling-interval, wordt deze taak kunt u om te voorkomen dat de wachtrij en geeft de status van de meest recente. De **Criteria status** is een filter waarmee u het bereik van de resultaten op basis van de geselecteerde status: In orde, waarschuwing, kritiek, onbekende, en alle. De **laatst bijgewerkt** tijd in de rechterbovenhoek vertegenwoordigt de laatste keer dat de diagnostische gegevens van Health-pagina is vernieuwd.

## <a name="alerts"></a>Waarschuwingen

Azure Monitor voor de gezondheid van virtuele machines worden geïntegreerd met [Azure-waarschuwingen](../../azure-monitor/platform/alerts-overview.md). Er wordt een waarschuwing gegeven wanneer vooraf gedefinieerde criteria, wanneer wordt gedetecteerd, niet wijzigen van een goede status in orde. Waarschuwingen worden gecategoriseerd op basis van ernst van Sev 0 tot en met 4 van de ernst, met Sev 0 als het hoogste niveau.

Waarschuwingen zijn niet gekoppeld aan een actiegroep om u te waarschuwen wanneer de waarschuwing is geactiveerd. Eigenaar van het abonnement moet meldingen configureren met de volgende stappen in de [waarschuwingen configureren](#configure-alerts) sectie.

Het totale aantal VM-status waarschuwingen op basis van ernst worden gecategoriseerd is beschikbaar op de **Health** dashboard onder de **waarschuwingen** sectie. Wanneer u het totale aantal waarschuwingen ofwel het nummer overeenkomt met een ernstniveau selecteert de **waarschuwingen** pagina wordt geopend en geeft een lijst van alle waarschuwingen die overeenkomen met uw selectie.

Bijvoorbeeld, als u de rij die overeenkomt met **ernst niveau 1**, ziet u de volgende weergave:

![Voorbeeld van alle ernst niveau 1-waarschuwingen](./media/vminsights-health/vminsights-sev1-alerts-01.png)

De **alle waarschuwingen** pagina is niet gericht op het weergeven van alleen de waarschuwingen die overeenkomen met uw selectie. Het ook gefilterd door **resourcetype** om weer te geven alleen de gezondheid van waarschuwingen die worden gegenereerd door een VM-resource. Deze indeling wordt weergegeven in de lijst met waarschuwingen, onder de kolom **Doelresource**, waar te zien is de virtuele machine van Azure de waarschuwing geactiveerd wanneer een slechte status voorwaarde is voldaan.

Waarschuwingen van andere resourcetypen of services zijn niet bedoeld om te worden opgenomen in deze weergave. Deze waarschuwingen bevatten waarschuwingen, die zijn gebaseerd op Logboeken-query's of metrische waarschuwingen die u normaal uit het standaardbeleid voor Azure Monitor weergeven zou [alle waarschuwingen](../../azure-monitor/platform/alerts-overview.md#all-alerts-page) pagina.

U kunt deze weergave filteren op waarden selecteren in de vervolgkeuzelijsten boven aan de pagina.

|Kolom |Description |
|-------|------------|
|Abonnement |Selecteer een Azure-abonnement. Alleen waarschuwingen in het geselecteerde abonnement zijn opgenomen in de weergave. |
|Resourcegroep |Selecteer één resourcegroep bestaan. Alleen waarschuwingen met doelen in de geselecteerde resourcegroep zijn opgenomen in de weergave. |
|Resourcetype |Selecteer een of meer resourcetypen. Standaard worden alleen waarschuwingen van de doel- **virtuele machines** is geselecteerd en opgenomen in deze weergave. Deze kolom is alleen beschikbaar nadat u een resourcegroep is opgegeven. |
|Resource |Selecteer een resource. Alleen waarschuwingen met die bron als doel zijn opgenomen in de weergave. Deze kolom is alleen beschikbaar als een resourcetype is opgegeven. |
|Severity |Selecteer de ernst van een waarschuwing of selecteer **alle** om op te nemen van waarschuwingen van alle ernstcategorieën. |
|Bewakingsvoorwaarde |Selecteer een controleconditie op filter waarschuwingen als ze zijn geactiveerd of opgelost door het systeem als de voorwaarde niet langer actief is. Of selecteer **alle** om op te nemen van waarschuwingen van alle voorwaarden. |
|Waarschuwingsstatus |Selecteer een waarschuwingsstatus **nieuw**, **bevestigen**, **gesloten**, of **alle** om op te nemen van waarschuwingen van alle Staten. |
|Bewakingsservice |Selecteer een service of selecteer **alle** om op te nemen alle services. Alleen waarschuwingen van de virtuele machine inzichten worden ondersteund voor deze functie.|
|Tijdsbereik| Alleen waarschuwingen geactiveerd in het geselecteerde tijdvenster zijn opgenomen in de weergave. Ondersteunde waarden zijn het afgelopen uur, de afgelopen 24 uur, de afgelopen 7 dagen en de afgelopen 30 dagen. |

Wanneer u een waarschuwing, selecteert de **waarschuwen details** pagina wordt weergegeven. Deze pagina vindt u informatie over de waarschuwing en kunt u de status te wijzigen.

Zie voor meer informatie over het beheren van waarschuwingen, [maken, weergeven en beheren van waarschuwingen via Azure Monitor](../../azure-monitor/platform/alerts-metric.md).

>[!NOTE]
>Het maken van nieuwe waarschuwingen op basis van criteria voor beveiligingsstatus of aanpassen bestaande health waarschuwingsregels in Azure Monitor van de portal wordt momenteel niet ondersteund.


![Deelvenster met Waarschuwingsdetails voor een geselecteerde waarschuwing](./media/vminsights-health/alert-details-pane-01.png)

U kunt een waarschuwingsstatus voor een of meer waarschuwingen wijzigen door ze te selecteren en vervolgens de optie **in een statuswijziging** uit de **alle waarschuwingen** pagina in de linkerbovenhoek. Selecteer een van de Staten op de **Waarschuwingsstatus wijzigen** deelvenster toevoegen van een beschrijving van de wijziging in de **Opmerking** veld en selecteer vervolgens **Ok** om uw wijzigingen toe te. Wanneer de gegevens worden geverifieerd en de wijzigingen zijn toegepast, de voortgang volgen onder **meldingen** in het menu.

### <a name="configure-alerts"></a>Waarschuwingen configureren
U kunt bepaalde beheertaken voor de waarschuwing niet beheren vanuit Azure portal. Deze taken moeten worden uitgevoerd met behulp van de [Azure Monitor REST API](https://docs.microsoft.com/rest/api/monitor/microsoft.workloadmonitor/components). Specifiek:

- In- of uitschakelen van een waarschuwing voor de criteria voor servicestatus
- Meldingen voor waarschuwingen van de health-criteria instellen

Elk voorbeeld wordt [ARMClient](https://github.com/projectkudu/armclient) op uw Windows-computer. Als u niet bekend met deze methode bent, Zie [ARMClient met behulp van](../platform/rest-api-walkthrough.md#use-armclient).

#### <a name="enable-or-disable-an-alert-rule"></a>In- of uitschakelen van een waarschuwingsregel

Inschakelen of uitschakelen van een waarschuwing voor de van specifieke gezondheidscriteria, de eigenschap **alertGeneration** moet worden gewijzigd met een waarde van **uitgeschakelde** of **ingeschakeld**.

Om te identificeren de *monitorId* voor de van specifieke gezondheidscriteria, het volgende voorbeeld ziet u hoe u query's voor die waarde voor de criteria **logische Schijf\gemiddelde schijf seconden Per Transfer**:

1. Typ in een terminalvenster **armclient.exe aanmelding**. In dat geval vraagt u zich aanmeldt bij Azure.

2. Voer de volgende opdracht voor het ophalen van alle health criterium die actief zijn op een specifieke virtuele machine en identificeren van de waarde voor *monitorId* eigenschap:

    ```
    armclient GET "subscriptions/subscriptionId/resourceGroups/resourcegroupName/providers/Microsoft.Compute/virtualMachines/vmName/providers/Microsoft.WorkloadMonitor/monitors?api-version=2018-08-31-preview”
    ```

    Het volgende voorbeeld ziet de uitvoer van de *armclient GET* opdracht. Noteer de waarde van *MonitorId*. Deze waarde is vereist voor de volgende stap, waar we moet de ID van de health-criteria opgeven en de eigenschap voor het maken van een waarschuwing wijzigen.

    ```
    "id": "/subscriptions/a7f23fdb-e626-4f95-89aa-3a360a90861e/resourcegroups/Lab/providers/Microsoft.Compute/virtualMachines/SVR01/providers/Microsoft.WorkloadMonitor/monitors/ComponentTypeId='LogicalDisk',MonitorId='Microsoft_LogicalDisk_AvgDiskSecPerRead'",
      "name": "ComponentTypeId='LogicalDisk',MonitorId='Microsoft_LogicalDisk_AvgDiskSecPerRead'",
      "type": "Microsoft.WorkloadMonitor/virtualMachines/monitors"
    },
    {
      "properties": {
        "description": "Monitor the performance counter LogicalDisk\\Avg Disk Sec Per Transfer",
        "monitorId": "Microsoft_LogicalDisk_AvgDiskSecPerTransfer",
        "monitorName": "Microsoft.LogicalDisk.AvgDiskSecPerTransfer",
        "monitorDisplayName": "Average Logical Disk Seconds Per Transfer",
        "parentMonitorName": null,
        "parentMonitorDisplayName": null,
        "monitorType": "Unit",
        "monitorCategory": "PerformanceHealth",
        "componentTypeId": "LogicalDisk",
        "componentTypeName": "LogicalDisk",
        "componentTypeDisplayName": "Logical Disk",
        "monitorState": "Enabled",
        "criteria": [
          {
            "healthState": "Warning",
            "comparisonOperator": "GreaterThan",
            "threshold": 0.1
          }
        ],
        "alertGeneration": "Enabled",
        "frequency": 1,
        "lookbackDuration": 17,
        "documentationURL": "https://aka.ms/Ahcs1r",
        "configurable": true,
        "signalType": "Metrics",
        "signalName": "VMHealth_Avg. Logical Disk sec/Transfer"
      },
      "etag": null,
    ```

3. Voer de volgende opdracht om te wijzigen de *alertGeneration* eigenschap:

    ```
    armclient patch subscriptions/subscriptionId/resourceGroups/resourcegroupName/providers/Microsoft.Compute/virtualMachines/vmName/providers/Microsoft.WorkloadMonitor/monitors/Microsoft_LogicalDisk_AvgDiskSecPerTransfer?api-version=2018-08-31-preview "{'properties':{'alertGeneration':'Disabled'}}"
    ```   

4. Voer de opdracht GET gebruikt in stap 2 om te controleren dat de waarde van de eigenschap is ingesteld op **uitgeschakelde**.

#### <a name="associate-an-action-group-with-health-criteria"></a>Een actiegroep koppelen aan criteria voor servicestatus

Azure Monitor voor de gezondheid van virtuele machines ondersteunt SMS en e-mailmeldingen wanneer waarschuwingen worden gegenereerd op basis van criteria voor beveiligingsstatus niet in orde. Noteer de naam van de geconfigureerde actiegroep om SMS- en e-mailmeldingen te verzenden voor het configureren van meldingen.

>[!NOTE]
>Deze actie moet worden uitgevoerd tegen elke bewaakte VM die u wilt een melding ontvangen. Het is niet van toepassing op alle virtuele machines in een resourcegroep.

1. Voer in een terminalvenster *armclient.exe aanmelding*. In dat geval vraagt u zich aanmeldt bij Azure.

2. Voer de volgende opdracht om te koppelen van een actiegroep met regels voor waarschuwingen:
 
    ```
    $payload = "{'properties':{'ActionGroupResourceIds':['/subscriptions/subscriptionId/resourceGroups/resourcegroupName/providers/microsoft.insights/actionGroups/actiongroupName']}}"
    armclient PUT https://management.azure.com/subscriptions/subscriptionId/resourceGroups/resourcegroupName/providers/Microsoft.Compute/virtualMachines/vmName/providers/Microsoft.WorkloadMonitor/notificationSettings/default?api-version=2018-08-31-preview $payload
    ```

3. Om te controleren of de waarde van de eigenschap **actionGroupResourceIds** met succes is bijgewerkt, voer de volgende opdracht:

    ```
    armclient GET "subscriptions/subscriptionName/resourceGroups/resourcegroupName/providers/Microsoft.Compute/virtualMachines/vmName/providers/Microsoft.WorkloadMonitor/notificationSettings?api-version=2018-08-31-preview"
    ```

    De uitvoer moet eruitzien als de volgende criteria:
    
    ```
    {
      "value": [
        {
          "properties": {
            "actionGroupResourceIds": [
              "/subscriptions/a7f23fdb-e626-4f95-89aa-3a360a90861e/resourceGroups/Lab/providers/microsoft.insights/actionGroups/Lab-IT%20Ops%20Notify"
            ]
          },
          "etag": null,
          "id": "/subscriptions/a7f23fdb-e626-4f95-89aa-3a360a90861e/resourcegroups/Lab/providers/Microsoft.Compute/virtualMachines/SVR01/providers/Microsoft.WorkloadMonitor/notificationSettings/default",
          "name": "notificationSettings/default",
          "type": "Microsoft.WorkloadMonitor/virtualMachines/notificationSettings"
        }
      ],
      "nextLink": null
    }
    ```

## <a name="next-steps"></a>Volgende stappen

- Zie voor het identificeren van beperkingen en de algehele prestaties van de virtuele machine, [weergave Azure VM-prestaties](vminsights-performance.md).
- Zie voor meer informatie over gedetecteerde toepassingsafhankelijkheden, [weergave Azure Monitor voor virtuele machines kaart](vminsights-maps.md).
