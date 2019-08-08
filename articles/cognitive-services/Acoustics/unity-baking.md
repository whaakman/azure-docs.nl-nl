---
title: Zelf studie voor het maken van de project-unit-eenheid
titlesuffix: Azure Cognitive Services
description: In deze zelf studie worden akoestische Baking met project akoestische in eenheid beschreven.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: e26df58de08d0941b5e3165852ed0b26f8890f66
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68854938"
---
# <a name="project-acoustics-unity-bake-tutorial"></a>Zelf studie voor het maken van de project-unit-eenheid
In deze zelf studie worden akoestische Baking met project akoestische in eenheid beschreven.

Software vereisten:
* [Unity 2018.2 +](https://unity3d.com) voor Windows
* [Invoeg toepassing voor project akoestische, geïntegreerd in uw Unity-project](unity-integration.md) of de [voorbeeld inhoud van de project akoestische-eenheid](unity-quickstart.md)
* Optioneel: Een [Azure batch account](create-azure-account.md) voor het versnellen van Bakes met behulp van Cloud Computing

## <a name="open-the-project-acoustics-bake-window"></a>Open het maken-venster voor project akoestische
Kies **venster > geluids** van het Unity-menu:

![Scherm afbeelding van Unity editor met de menu optie akoestische venster gemarkeerd](media/window-acoustics.png)

## <a name="create-a-navigation-mesh"></a>Een navigatie-net maken
Project akoestische maakt gebruik van een navigatie net om listener-test punten te plaatsen voor simulatie. U kunt de [werk stroom](https://docs.unity3d.com/Manual/nav-BuildingNavMesh.html)voor de navigatie-net-eenheid van unit gebruiken of een ander 3D-model pakket gebruiken om uw eigen net te ontwerpen. 

## <a name="mark-acoustic-scene-objects"></a>Akoestische scène objecten markeren
De geluids elementen van het project zijn afhankelijk van twee soorten scène objecten voor simulatie: de objecten die worden weer spie geld en occlude in de simulatie, en het Media Player-net waarmee listener-test punten in simulatie worden beperkt. Beide object typen zijn gemarkeerd met het tabblad **objecten** . 

Omdat het markeren van objecten de onderdelen **AcousticsGeometry** of **AcousticsNavigation** aan het object toevoegt, kunt u ook de [werk stroom van het standaard eenheids onderdeel](https://docs.unity3d.com/Manual/UsingComponents.html) gebruiken om objecten te markeren of te markeren. Alleen netrenderers en Terreinsen kunnen worden gemarkeerd. Alle andere object typen worden genegeerd. Met de selectie vakjes worden alle betrokken objecten gemarkeerd of gemarkeerd.

### <a name="mark-acoustic-occlusion-and-reflection-geometry"></a>Akoestische bedekking-en reflectie geometrie markeren
Open het tabblad **objecten** van het **akoestische** venster. Markeer objecten als **akoestische geometrie** als ze occlude, reflectie of absorberende klank moeten hebben. De geometrie van de akoestische kan zaken bevatten zoals grond, wanden, daken, Windows & glas, vloeren en grote meubelen. U kunt elk wille keurig niveau van complexiteit voor deze objecten gebruiken. Omdat de scène voxelized is vóór simulatie, zijn zeer gedetailleerde netten, zoals structuren met veel kleine bladeren, niet meer kostbaar voor maken dan vereenvoudigde objecten.

Neem geen dingen op die niet van invloed zijn op de akoestische, zoals onzichtbare botsingen.

De trans formatie van een object op het moment van de berekening van de test (via het tabblad **tests** hieronder) wordt opgelost in de maken-resultaten. Als u een van de gemarkeerde objecten in de scène verplaatst, moet u de test berekening opnieuw uitvoeren en de scène rebakingen.

### <a name="mark-the-navigation-mesh"></a>Het navigatie net markeren
Navigatie netten die zijn gemaakt met de werk stroom van de eenheid, worden door het akoestische systeem opgehaald. Als u uw eigen netten wilt gebruiken, markeert u deze op het tabblad **objecten** .

### <a name="for-reference-the-objects-tab-parts"></a>Ter referentie: De onderdelen op het tabblad objecten
De onderdelen van de tabblad pagina zijn:

1. De knoppen voor het tabblad selectie (tabblad**objecten** geselecteerd). Gebruik deze knoppen om de verschillende stappen voor het uitvoeren van een akoestische maken, van links naar rechts door te lopen.
2. Een korte beschrijving van wat u moet doen met deze pagina.
3. Beschik bare filters voor het hiërarchie venster. Gebruik deze optie om het hiërarchie venster te filteren op objecten van het opgegeven type, zodat u ze eenvoudiger kunt markeren. Als u nog niets hebt gemarkeerd voor akoestische items, wordt er niets weer gegeven wanneer u de laatste twee opties selecteert. Ze kunnen echter nuttig zijn om gemarkeerde objecten te vinden zodra u dit hebt gedaan.
4. Als er geen objecten zijn geselecteerd, wordt in deze sectie de status van alle objecten in de scène weer gegeven:
    * Totaal-het totaal aantal actieve, niet-verborgen objecten in de scène.
    * Genegeerd: het aantal objecten dat geen netrenderers of terrein is.
    * Net-het aantal NetRenderer-objecten in de scène
    * Terrein: het aantal terrein objecten in de scène
    * Geometrie: het aantal net-of terrein objecten in de scène dat is gemarkeerd als "akoestische geometrie"
    * Navigatie: het aantal net-of terrein objecten in de scène dat is gemarkeerd als akoestische navigatie. Dit nummer omvat niet de NavMesh van de unit.
5. Toont het totale aantal objecten met de markering ' in de scène ', wat alleen netrenderers en terrein is. Hiermee worden selectie vakjes weer gegeven die u kunt gebruiken om deze objecten als geometrie of navigatie voor akoestische te markeren (toevoegen aan).
6. Als er niets is geselecteerd, wordt in deze notitie aan u herinnerd om, indien nodig, objecten voor markering te selecteren. U kunt ook een of beide selectie vakjes inschakelen om alle objecten in de scène te markeren zonder iets te selecteren.
7. Als objecten zijn geselecteerd, wordt in deze sectie de status van alleen de geselecteerde objecten weer gegeven.
8. Toont het totale aantal geselecteerde objecten met de markering. Als u de selectie vakjes uitschakelt of uitschakelt, worden alleen de geselecteerde objecten gemarkeerd of onduidelijk.

Als er niets is geselecteerd in uw scène, ziet het tabblad objecten eruit als in de volgende afbeelding:

![Scherm afbeelding van het tabblad akoestische objecten zonder selectie](media/objects-tab-no-selection-detail.png)

Als er iets in uw scène-of hiërarchie venster is geselecteerd, ziet het eruit als in de volgende afbeelding:

![Scherm afbeelding van het tabblad akoestische objecten waarvoor de selectie wordt weer gegeven](media/objects-tab-selection-detail.png)

Als sommige objecten zijn gemarkeerd en andere niet, wordt in het juiste selectie vakje een ' gemengde ' waarde weer gegeven:

![Scherm afbeelding van het tabblad akoestische objecten met het pictogram gemengde selectie gemarkeerd](media/mixed-object-selection-detail.png)

Als u op het selectie vakje klikt, worden alle objecten gemarkeerd en wordt de markering van alle objecten opgeheven.

Objecten kunnen worden gemarkeerd voor geometrie en navigatie.

## <a name="select-acoustic-materials"></a>Akoestische materialen selecteren
Als uw objecten zijn gemarkeerd, klikt u op de knop **materialen** en wijst u geluids materiaal toe. Het systeem voor de geluids elementen van het project is gekoppeld aan het systeem voor het visualiseren van Unit: voor twee objecten moet er sprake zijn van afzonderlijke Visual-materialen.

De akoestische materialen bepalen de hoeveelheid geluids energie die van elk Opper vlak weer gegeven wordt. Het standaard geluids materiaal heeft een absorptie die vergelijkbaar is met beton. In Project akoestische worden materialen voorgesteld op basis van de naam van het visuele element. U kunt het akoestische materiaal ' aangepast ' toewijzen aan een materiaal om een absorptie coëfficiënt in te scha kelen.

De reverberation tijd van een bepaald materiaal in een kamer is inkerend gerelateerd aan de absorptie coëfficiënt, waarbij de meeste materialen met absorptie waarden in het 0,01 tot 0,20 bereik. Materialen met absorptie coëfficiënten buiten dit bereik zijn zeer absorberend.

![Grafiek met een negatieve correlatie tussen reverberation tijd en absorptie coëfficiënt](media/reverb-time-graph.png)

### <a name="for-reference-parts-of-the-materials-tab"></a>Ter referentie: Onderdelen van het tabblad materialen
![Scherm afbeelding van het tabblad geluids materialen in eenheid](media/materials-tab-detail.png)

1. De knop met het tabblad **materialen** wordt gebruikt om deze pagina te openen.
2. Een korte beschrijving van wat u moet doen met deze pagina.
3. Wanneer dit selectie vakje is ingeschakeld, worden alleen materialen weer gegeven die worden gebruikt door objecten die zijn gemarkeerd als **akoestische geometrie** . Anders worden alle materialen vermeld die in de scène worden gebruikt.
4. Gebruik deze opties om de volg orde te wijzigen van de vervolg keuzelijst die wordt weer gegeven wanneer u op een vervolg keuzelijst in de kolom akoestische (#6) klikt. **Naam** sorteert het akoestische materiaal op naam. Met ' absorptivity ' worden deze in volg orde van absorptivity van laag naar hoog gesorteerd.
5. De lijst met materialen die in de scène worden gebruikt, alfabetisch gesorteerd. Als het selectie vakje **alleen gemarkeerde items weer geven** is ingeschakeld (#3), worden alleen materialen weer gegeven die worden gebruikt door objecten die zijn gemarkeerd als **akoestische geometrie** . Als u hier op een materiaal klikt, worden alle objecten in de scène geselecteerd die dit materiaal gebruiken.
6. Toont het akoestische materiaal waaraan het scène materiaal is toegewezen. Klik op een vervolg keuzelijst om een scène materiaal opnieuw toe te wijzen aan een ander akoestisch materiaal. U kunt de sorteer volgorde van het menu dat wordt weer gegeven, wijzigen door te klikken op een item met behulp van de opties **voor het sorteren op de volgende** (#4).
7. Toont de akoestische absorptie coëfficiënt van het materiaal dat is geselecteerd in de vorige kolom. Een waarde van nul betekent perfect reflectie (geen absorptie), terwijl een waarde van 1 betekent perfect absorptive (geen reflectie). De absorptie coëfficiënt kan alleen worden gewijzigd als het geselecteerde materiaal ' aangepast ' is.
8. Voor een materiaal dat is toegewezen aan ' aangepast ', is de schuif regelaar niet meer uitgeschakeld en kunt u de absorptie coëfficiënt kiezen met de schuif regelaar of door een waarde te typen.

## <a name="calculate-and-review-listener-probe-locations"></a>Test locaties voor listeners berekenen en controleren
Nadat u de materialen hebt toegewezen, gaat u naar het tabblad probes en klikt u op **Calculate** om de test punten van de listener voor simulatie te plaatsen.

### <a name="what-the-calculate-button-calculates"></a>Wat is het ' berekenen... ' knop berekent
De knop **berekenen...** maakt gebruik van de geselecteerde akoestische scène geometrie om uw scène voor te bereiden voor simulatie:

1. Het neemt de geometrie van de scène netten en berekent een Voxel-volume. Het Voxel-volume is een driedimensionaal volume dat uw hele scène omsluit en bestaat uit kleine kubieke ' voxels '. De grootte van de voxels wordt bepaald door de simulatie frequentie, die wordt ingesteld door de instelling **simulatie oplossing** . Elke Voxel is gemarkeerd als ' lucht openen ' of met een scène geometrie. Als een Voxel Geometry bevat, wordt de Voxel gelabeld met de absorptie coëfficiënt van het materiaal dat aan die geometrie is toegewezen.
2. De navigatie-mesh (sen) wordt gebruikt om de test punten van de listener te plaatsen. Het algoritme verdeelt de concurrerende bezorgdheid van ruimtelijke dekking en simulatie tijd en bestands grootte, en zorgt ervoor dat smalle gangen en kleine ruimten altijd een zekere mate van dekking hebben. Typische test punt aantallen variëren van 100 voor kleine scènes tot een paar duizend voor grote scènes.

Afhankelijk van de grootte van uw scène en de snelheid van uw computer, kan het enkele minuten duren voordat deze berekeningen worden uitgevoerd.

### <a name="review-voxel-and-probe-placement"></a>Voxel en test plaatsing controleren
Bekijk een voor beeld van de Voxel-gegevens en de test punt locaties om te controleren of u klaar bent om uw scène te maken. Een onvolledig navigatie-net of ontbrekende of extra akoestische geometrie is doorgaans snel zichtbaar in de preview-versie. Voxel en test plaatsing kunnen worden in-of uitgeschakeld met behulp van het menu Gizmos:

![Scherm opname van het Gizmos-menu in eenheid](media/gizmos-menu.png)

Voxels die akoestische geometrie bevatten, worden weer gegeven als groene kubussen. Verken uw scène en controleer of alles dat geometrie zou moeten zijn voxels heeft. De scène camera moet binnen ongeveer 5 meters van het object liggen om de voxels weer te geven.

Als u de voxels die is gemaakt met ruwe resolutie en een nauw keurige resolutie vergelijkt, ziet u dat de ruwe voxels twee maal zo groot zijn.

![Scherm afbeelding van grof voxels preview in Unity editor](media/voxel-cubes-preview.png)

Simulatie resultaten worden in runtime geinterpolatie tussen de locaties van de listener-test punten. Controleer of er test punten in de buurt staan van de speler die naar verwachting in de scène gaat rijden.

![Scherm afbeelding van de proef versie van Unity editor](media/probes-preview.png)

### <a name="take-care-with-scene-renames"></a>Wees voorzichtig met het wijzigen van de namen van scènes
De scène naam wordt gebruikt om de scène te verbinden met bestanden die de plaatsing van het test punt en de voxelization opslaan. Als de naam van de scène wordt gewijzigd nadat de test punten zijn berekend, gaan de toewijzings-en plaatsings gegevens van de materialen verloren en moeten deze opnieuw worden uitgevoerd.

### <a name="for-reference-parts-of-the-probes-tab"></a>Ter referentie: Onderdelen van het tabblad tests
![Scherm opname van akoestische tests tabblad in eenheid](media/probes-tab-detail.png)

1. De knop met het tabblad probe die wordt gebruikt om deze pagina te openen
2. Een korte beschrijving van wat u met deze pagina moet doen
3. U kunt deze gebruiken om een grove of fijne simulatie resolutie te kiezen. Grof is sneller, maar heeft bepaalde afwegingen. Zie de onderstaande [maken-oplossing](bake-resolution.md) voor meer informatie.
4. Kies de locatie waar de akoestische gegevens bestanden moeten worden geplaatst met dit veld. Klik op de knop met '... ' een Mapkiezer gebruiken. De standaard waarde is **assets/AcousticsData**. Er wordt ook een **Editor** -submap gemaakt onder deze locatie. Zie [gegevens bestanden](#Data-Files) hieronder voor meer informatie over gegevens bestanden.
5. De gegevens bestanden voor deze scène worden aangeduid met het voor voegsel dat u hier opgeeft. De standaard waarde is ' Acoustics_ [scène naam] '.
6. Nadat de tests zijn berekend, worden de bovenstaande besturings elementen uitgeschakeld. Klik op de knop **wissen** om de berekeningen te wissen en de besturings elementen in te scha kelen, zodat u opnieuw kunt berekenen met nieuwe instellingen.
7. Klik op de knop **berekenen...** om de scène te voxelize en de locaties van het test punt te berekenen. Dit wordt lokaal op uw computer uitgevoerd en moet worden uitgevoerd voordat u een maken kunt uitvoeren.

In deze versie van project akoestische kunnen tests niet hand matig worden geplaatst en moeten ze worden geplaatst via het geautomatiseerde proces dat op het tabblad **tests** wordt vermeld.

Zie [maken-oplossing](bake-resolution.md) voor meer informatie over grove nauw keurigheid.

## <a name="bake-your-scene-using-azure-batch"></a>Maken uw scène met behulp van Azure Batch
U kunt uw scène maken met een berekenings cluster in de Cloud met behulp van de Azure Batch-service. De invoeg toepassing voor het project akoestische unit-eenheid maakt rechtstreeks verbinding met Azure Batch om een Azure Batch cluster te instantiëren, te beheren en te verbreken voor elke maken. Voer uw Azure-referenties in op het tabblad **maken** , selecteer een type en grootte voor de cluster machine en klik op **maken**.

### <a name="for-reference-parts-of-the-bake-tab"></a>Ter referentie: Delen van het tabblad maken
![Scherm afbeelding van het tabblad akoestische maken in eenheid](media/bake-tab-details.png)

1. De knop met het tabblad maken wordt gebruikt om deze pagina te openen.
2. Een korte beschrijving van wat u op deze pagina moet doen.
3. Velden om uw Azure-referenties in te voeren nadat uw Azure-account is gemaakt. Zie [een Azure batch-account maken](create-azure-account.md)voor meer informatie.
4. Docker-afbeeldings code voor de akoestische hulp programmaset.
5. Start Azure Portal voor het beheren van uw abonnementen, het controleren van het gebruik en het weer geven van facturerings gegevens, enzovoort. 
6. Type Azure batch Compute-knoop punt dat moet worden gebruikt voor de berekening. Het knooppunt type moet worden ondersteund door de Azure Data Center-locatie. Als dat niet het geval is, moet u op **Standard_F8s_v2**.
7. Het aantal knoop punten dat moet worden gebruikt voor deze berekening. Het nummer dat u hier invoert, is van invloed op de tijd voor het volt ooien van de maken en wordt beperkt door uw Azure Batch core-toewijzing. De standaard toewijzing biedt alleen ondersteuning voor twee 8 kern knooppunten of een 1 16-kern knooppunt, maar kan worden uitgebreid. Zie [een Azure batch-account maken](create-azure-account.md)voor meer informatie over kern toewijzings beperkingen.
8. Schakel dit selectie vakje in om uw reken groep te configureren voor het gebruik van [knoop punten met een lage prioriteit](https://docs.microsoft.com/azure/batch/batch-low-pri-vms). Reken knooppunten met lage prioriteit hebben veel lagere kosten, maar zijn mogelijk niet altijd beschikbaar of kunnen op elk gewenst moment worden voorranglen.
9. Het aantal tests voor uw scène zoals berekend op het tabblad **tests** . Het aantal tests bepaalt het aantal simulaties dat moet worden uitgevoerd in de Cloud. U kunt niet meer knoop punten opgeven dan er tests zijn.
10. De hoeveelheid tijd die nodig is om uw taak uit te voeren in de Cloud. Dit geldt niet voor de start tijd van het knoop punt. Zodra de taak is gestart, weet u hoe lang deze moet zijn voordat u de resultaten terugkrijgt. Houd er rekening mee dat dit slechts een schatting is.
11. De totale hoeveelheid computer tijd die nodig is om de simulaties uit te voeren. Dit is de totale hoeveelheid reken tijd van het knoop punt die wordt gebruikt in Azure. Zie [schatting](#Estimating-bake-cost) van de kosten voor maken hieronder voor meer informatie over het gebruik van deze waarde.
12. Dit bericht geeft aan waar de resultaten van de maken worden opgeslagen zodra de taak is voltooid.
13. (Alleen Geavanceerd gebruik) Als u om de een of andere reden het afdwingen van de eenheid moet verg eten over een maken die u hebt verzonden (bijvoorbeeld als u de resultaten hebt gedownload met een andere machine), klikt u op de knop **status wissen** om te verg eten over de verzonden taak. Dit betekent dat het resultaat bestand, wanneer dit klaar is, **niet** kan worden gedownload en **Dit is niet hetzelfde als het annuleren van de taak**. Als de taak wordt uitgevoerd, wordt deze nog steeds uitgevoerd in de Cloud.
14. Klik op de knop **maken** om de maken naar de cloud te verzenden. Terwijl een taak wordt uitgevoerd, wordt in plaats daarvan de **taak annuleren** weer gegeven.
15. Bereidt zich voor [op de verwerking van akoestische simulaties op uw PC](#Local-bake).
16. In dit gebied wordt de status van de maken weer gegeven. Als u klaar bent, moethet gedownloade weer geven.

U kunt altijd volledige informatie krijgen over actieve taken, reken Pools en opslag op het [Azure Portal](https://portal.azure.com).

Terwijl een taak wordt uitgevoerd, wordt de **maken** -knop gewijzigd in **taak annuleren**. Gebruik deze knop om de taak in voortgang te annuleren. U wordt gevraagd om te bevestigen voordat de taak wordt geannuleerd. Het annuleren van een taak kan niet ongedaan worden gemaakt, er zijn geen resultaten meer beschikbaar en er worden nog steeds kosten in rekening gebracht voor alle gebruikte Azure-reken tijd.

Zodra u een maken hebt gestart, kunt u de eenheid sluiten. Afhankelijk van het project, het knooppunt type en het aantal knoop punten kan een Cloud-maken enkele uren duren. De taak status maken wordt bijgewerkt wanneer u het project opnieuw laadt en het akoestische venster opent. Als de taak is voltooid, wordt het uitvoer bestand gedownload.

De Azure-referenties worden veilig opgeslagen op uw lokale machine en gekoppeld aan uw Unity-editor. Ze worden alleen gebruikt om een beveiligde verbinding met Azure tot stand te brengen.

### <a name="Estimating-bake-cost"></a>Kosten voor Azure maken schatten

Als u wilt schatten wat een bepaalde maken kost, neemt u de waarde voor **geschatte reken kosten**op, die een duur is en vermenigvuldigt u die met de kosten per uur in uw lokale valuta van het **VM-knooppunt type** dat u hebt geselecteerd. Het resultaat bevat niet de tijd van het knoop punt dat nodig is om de knoop punten actief en werkend te krijgen. Als u bijvoorbeeld **Standard_F8s_v2** selecteert voor het knooppunt type, dat een kost prijs van $0.40/uur heeft en de geschatte reken kosten 3 uur en 57 minuten zijn, is de geschatte kosten voor het uitvoeren van de taak $0,40 * ~ 4 uur = ~ $1,60. De werkelijke kosten zijn waarschijnlijk iets hoger als gevolg van de extra tijd voor het starten van de knoop punten. U kunt de kosten van het knoop punt per uur vinden op de pagina met [Azure batch prijzen](https://azure.microsoft.com/pricing/details/virtual-machines/linux) (Selecteer ' verwerkte optimalisatie ' of ' high performance Compute ' voor de categorie).

## <a name="Local-bake"></a>Maken uw scène op uw PC
U kunt uw scène op uw eigen PC maken. Dit kan handig zijn om te experimenteren met geluids met kleine scènes voordat u een Azure Batch-account maakt. Houd er rekening mee dat de akoestische simulatie veel tijd kan duren, afhankelijk van de grootte van de scène.

### <a name="minimum-hardware-requirements"></a>Minimale hardwarevereisten
* Een x86-64-processor met ten minste 8 kernen en 32 GB aan RAM-geheugen

Een voor beeld: in onze tests op een 8-core computer met Intel Xeon E5-1660 @ 3 GHz en 32 GB RAM-
* Een kleine scène met 100 tests kan ongeveer 2 uur duren voor een ruwe maken of 32 uur voor een fijne maken.
* Een middel grote scène met 1000 tests kan ongeveer 20 uur duren voor een ruwe maken of 21 dagen voor een fijne maken.

### <a name="setup-docker"></a>Docker instellen
Installeer en configureer docker op de PC waarop de simulatie wordt uitgevoerd-
1. Installeer de [docker](https://www.docker.com/products/docker-desktop)-toolset.
2. Start docker-instellingen, navigeer naar de opties Geavanceerd en configureer bronnen voor ten minste 8 GB RAM-geheugen. Hoe meer Cpu's u aan docker kunt toewijzen, hoe sneller de maken wordt voltooid. ![Scherm afbeelding van voor beeld van docker-instellingen](media/docker-settings.png)
3. Navigeer naar gedeelde stations en schakel delen in voor het station dat wordt gebruikt voor de verwerking.![Scherm opname van opties voor gedeeld docker-station](media/docker-shared-drives.png)

### <a name="run-local-bake"></a>Lokale maken uitvoeren
1. Klik op de knop ' lokale maken voorbereiden ' op het tabblad **maken** en selecteer een map waarin de invoer bestanden en uitvoerings scripts worden opgeslagen. U kunt de maken vervolgens op elke computer uitvoeren, zolang deze voldoet aan de minimale hardwarevereisten en docker heeft geïnstalleerd door de map naar die computer te kopiëren.
2. Start de simulatie met het script ' runlocalbake. bat '. Met dit script wordt de project akoestische docker-installatie kopie opgehaald met de hulpprogram ma's die nodig zijn voor simulatie verwerking en de simulatie starten. 
3. Zodra de simulatie is voltooid, kopieert u het resulterende. Ace-bestand terug naar uw Unity-project. Om ervoor te zorgen dat unit deze als een binair bestand herkent, voegt u '. bytes ' toe aan de bestands extensie (bijvoorbeeld ' Scene1. ace. bytes '). De gedetailleerde logboeken voor de simulatie worden opgeslagen in ' AcousticsLog. txt '. Als u problemen ondervindt, deelt u dit bestand om te helpen met de diagnose.

## <a name="Data-Files"></a>Gegevens bestanden die worden toegevoegd door het maken-proces

Er zijn vier gegevens bestanden gemaakt tijdens het maken-proces. De ene bevat de simulatie resultaten en wordt geleverd met uw titel. De overige opslag eenheid editor-gerelateerde gegevens.

Simulatie resultaat:
* **Assets/AcousticsData/akoestische\_[scènenaam]. ace. bytes**: Dit is de opzoek tabel van de runtime en bevat simulatie resultaten en de voxelized akoestische scène-elementen. De locatie en naam van dit bestand kunnen worden gewijzigd met behulp van de velden op het tabblad **tests** .

Zorg ervoor dat u het simulatie resultaten bestand niet verwijdert. Het kan niet worden hersteld, behalve door de scène te rebakingen.

Editor-gegevens bestanden:
* **Assets/Editor/[SceneName]\_AcousticsParameters.asset**: In dit bestand worden de gegevens opgeslagen die u invoert in velden in de gebruikers interface van de akoestische. De locatie en naam van dit bestand kunnen niet worden gewijzigd.
* **Assets/AcousticsData/Editor/Acoustics_[SceneName].vox**: In dit bestand worden de voxelized akoestische-geometrie en de eigenschappen van het materiaal die worden berekend met behulp van de knop **berekenen...** op het tabblad tests opgeslagen. De locatie en naam van dit bestand kunnen worden gewijzigd met behulp van de velden op het tabblad **tests** .
* **Assets/AcousticsData/Editor/Acoustics\_[SceneName]\_config.xml**: In dit bestand worden simulatie parameters opgeslagen die worden berekend met behulp van de knop **berekenen...** op het tabblad **tests** . De locatie en naam van dit bestand kunnen worden gewijzigd met behulp van de velden op het tabblad **tests** .

## <a name="set-up-the-acoustics-lookup-table"></a>De zoek tabel voor akoestische instellingen instellen
Sleep de **project akoestische** prefab uit het deel venster Project naar uw scène:

![Scherm opname van akoestische prefab in eenheid](media/acoustics-prefab.png)

Klik op het spel object **ProjectAcoustics** en ga naar het deel venster Inspector. Geef de locatie van uw maken-resultaat op (de. ACE-bestand, in **assets/AcousticsData**) door deze te slepen en neer te zetten in het akoestische-beheer script, of door te klikken op de knop Circle naast het tekstvak.

![Scherm afbeelding van akoestische prefab in eenheid](media/acoustics-manager.png)  

## <a name="next-steps"></a>Volgende stappen
* De [ontwerp besturings elementen voor eenheid](unity-workflow.md) verkennen
* Bekijk de [ontwerp concepten van het project akoestische ontwerpen](design-process.md)

