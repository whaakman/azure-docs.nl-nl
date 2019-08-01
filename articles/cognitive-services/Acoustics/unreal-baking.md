---
title: Zelf studie voor project akoestische Unreal maken
titlesuffix: Azure Cognitive Services
description: In dit document wordt het proces voor het verzenden van een akoestische maken met behulp van de Unreal-Editor-extensie beschreven.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: michem
ROBOTS: NOINDEX
ms.openlocfilehash: 47946570db305ff3d54dfed9ea6f698e5deb7b72
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68704790"
---
# <a name="project-acoustics-unreal-bake-tutorial"></a>Zelf studie voor project akoestische Unreal maken
In dit document wordt het proces voor het verzenden van een akoestische maken met behulp van de Unreal-Editor-extensie beschreven.

Er zijn vijf stappen voor het uitvoeren van een maken:

1. Uw speler navigatie-net maken of coderen
2. Geometrie van label akoestische
3. Eigenschappen van akoestische materialen toewijzen aan geometrie
4. Test plaatsing preview
5. Maken

## <a name="open-the-project-acoustics-editor-mode"></a>De modus voor het project akoestische editor openen

Importeer het invoeg pakket voor project akoestische conversies aan uw project. Zie het onderwerp [Unreal-integratie](unreal-integration.md) voor meer informatie. Zodra de invoeg toepassing is geïntegreerd, opent u de gebruikers interface voor akoestische door te klikken op het pictogram nieuwe akoestische modus.

![Scherm opname van de optie voor de akoestische modus van de Unreal-editor](media/acoustics-mode.png)

## <a name="tag-actors-for-acoustics"></a>Actors voor akoestische Codeer

Het tabblad objecten is het eerste tabblad dat wordt weer gegeven wanneer u de akoestische modus opent. Gebruik dit tabblad om actors op uw niveau te labelen, waarmee de **AcousticsGeometry** -of **AcousticsNavigation** -Tags aan de actors worden toegevoegd.

Selecteer een of meer objecten in de wereld-contourie of gebruik de sectie **bulksgewijze selectie** om alle objecten van een specifieke categorie te selecteren. Als objecten eenmaal zijn geselecteerd, gebruikt u de sectie labelen om de gewenste tag toe te passen op de geselecteerde objecten.

Als iets geen **AcousticsGeometry** of **AcousticsNavigation** -tag heeft, wordt deze in de simulatie genegeerd. Alleen statische mazen, NAV-netten en landschappen worden ondersteund. Als u iets anders labelt, wordt dit genegeerd.

### <a name="for-reference-the-objects-tab-parts"></a>Ter referentie: De onderdelen op het tabblad objecten

![Scherm afbeelding van het tabblad akoestische objecten in Unreal](media/unreal-objects-tab-details.png)

1. De knoppen voor het tabblad selectie (tabblad**objecten** geselecteerd). Gebruik deze knoppen om de verschillende stappen voor het uitvoeren van een akoestische maken van boven naar beneden door te lopen.
2. Een korte beschrijving van wat u moet doen met deze pagina.
3. Beschik bare selecters voor actors op het niveau.
4. Als u op **selecteren** klikt, worden alle objecten geselecteerd op het niveau die overeenkomen met ten minste één van de ingeschakelde actor typen.
5. Als **u op alles opheffen** klikt, wordt de huidige selectie gewist. Dit komt overeen met de Escape-toets.
6. Gebruik deze keuze rondjes om te kiezen of u de geometrie of navigatie code op de geselecteerde actors wilt Toep assen.
7. Als u op **tag** klikt, wordt de geselecteerde tag toegevoegd aan alle geselecteerde actors.
8. Als u op **tag** klikt, wordt de geselecteerde tag verwijderd uit alle geselecteerde actors.
9. Als **u op gelabeld selecteren** klikt, wordt de huidige selectie gewist en worden alle actors met de momenteel geselecteerde tag geselecteerd.
10. Deze statistieken laten zien hoeveel Actors zijn gelabeld met elk type code.

### <a name="tag-acoustics-occlusion-and-reflection-geometry"></a>Label akoestische bedekking en reflectie geometrie

Open het tabblad objecten van het akoestische venster. Markeer objecten als akoestische geometrie als ze occlude, reflectie of absorberende klank moeten hebben. De geometrie van de akoestische kan zaken bevatten zoals grond, wanden, daken, Windows & glas, vloeren en grote meubelen. U kunt elk wille keurig niveau van complexiteit voor deze objecten gebruiken. Omdat de scène voxelized is vóór simulatie, zijn zeer gedetailleerde netten, zoals structuren met veel kleine bladeren, niet meer kostbaar voor maken dan vereenvoudigde objecten.

Neem geen dingen op die niet van invloed zijn op de akoestische, zoals onzichtbare botsingen.

De trans formatie van een object op het moment van de berekening van de test (via het tabblad tests hieronder) wordt opgelost in de maken-resultaten. Als u een van de gemarkeerde objecten in de scène verplaatst, moet u de test berekening opnieuw uitvoeren en de scène rebakingen.

### <a name="create-or-tag-a-navigation-mesh"></a>Een navigatie-net maken of coderen

Er wordt een navigatie-net gebruikt voor het plaatsen van test punten voor simulatie. U kunt het Unreal- [volume van de NAV mesh](https://api.unrealengine.com/INT/Engine/AI/BehaviorTrees/QuickStart/2/index.html)gebruiken of u kunt uw eigen navigatie-net opgeven. U moet ten minste één object markeren als **akoestische navigatie**. Als u het navigatie-net van Unreal gebruikt, zorg er dan voor dat u het eerst hebt gemaakt.

### <a name="acoustics-volumes"></a>Geluids volumes ###

Er zijn nog meer geavanceerde aanpassingen die u kunt aanbrengen in uw navigatie gebieden met **geluids volumes**. **Geluids volumes** zijn actors die u aan uw scène kunt toevoegen, zodat u gebieden die u wilt insluiten en negeren in het navigatie-net moet kunnen selecteren. De actor beschrijft een eigenschap die kan worden overgeschakeld tussen ' include ' en ' exclude '. ' Include '-volumes zorgen ervoor dat alleen de gebieden van het navigatie-net in de domeinen worden beschouwd en ' exclude ' volumes markeren die gebieden moeten worden genegeerd. ' Exclude ' volumes worden altijd toegepast na ' include ' volumes. Zorg ervoor dat **geluids volumes** worden gelabeld als **akoestische navigatie** via het gebruikelijke proces op het tabblad objecten. Deze actors worden ***niet*** automatisch gelabeld.

![Scherm afbeelding van de geluids eigenschappen van het volume in Unreal](media/unreal-acoustics-volume-properties.png)

' Exclude ' volumes zijn voornamelijk bedoeld om nauw keurige controle te geven over waar het niet plaatsen van tests voor het verhogen van het resource gebruik.

![Scherm opname van geluids volume met uitsluiting in Unreal](media/unreal-acoustics-volume-exclude.png)

' Include '-volumes zijn handig voor het maken van hand matige secties van een scène, bijvoorbeeld als u uw scène wilt opdelen in meerdere akoestische zones. Als u bijvoorbeeld een grote scène hebt, zijn veel kilo meters in vier kant en hebt u twee belang rijke gebieden waarop u maken wilt. U kunt twee grote ' include '-volumes in de scène tekenen en voor elk van deze een ACE-bestand maken. In het spel kunt u vervolgens met behulp van activerings volumes gecombineerd met blauw drukken-aanroepen het juiste ACE-bestand laden wanneer de speler elke tegel nadert.

**Geluids volumes** beperken alleen de navigatie en ***niet*** de geometrie. Elke test in een ' include '- **geluids volume** kan nog steeds alle benodigde geometrie buiten het volume halen bij het uitvoeren van Golf simulaties. Daarom mag er geen sprake zijn van bedekking of andere akoestische, ten gevolge van de speler van de ene sectie naar de andere.

## <a name="select-acoustic-materials"></a>Akoestische materialen selecteren

Als uw objecten zijn gelabeld, klikt u op de knop **materialen** om naar het tabblad materialen te gaan. Dit tabblad wordt gebruikt om de eigenschappen van het materiaal voor elk materiaal op het niveau op te geven. Voordat een actor is gelabeld, is deze leeg.

De akoestische materialen bepalen de hoeveelheid geluids energie die van elk Opper vlak weer gegeven wordt. Het standaard geluids materiaal heeft een absorptie die vergelijkbaar is met beton. In Project akoestische worden materialen voorgesteld op basis van de naam van het scène materiaal.

De reverberation tijd van een bepaald materiaal in een kamer is inkerend gerelateerd aan de absorptie coëfficiënt, waarbij de meeste materialen met absorptie waarden in het 0,01 tot 0,20 bereik. Materialen met absorptie coëfficiënten boven dit bereik zijn zeer absorberen. Als bijvoorbeeld een ruimte klinkt te reverberant, wijzigt u het akoestische materiaal van de wanden, de vloer of het plafond in een hogere absorptivity. De toewijzing van akoestische materialen is van toepassing op alle Actors die gebruikmaken van dat scène materiaal.

![Grafiek met een negatieve correlatie tussen reverberation tijd en absorptie coëfficiënt](media/reverb-time-graph.png)

### <a name="for-reference-parts-of-the-materials-tab"></a>Ter referentie: Onderdelen van het tabblad materialen

![Scherm afbeelding van het tabblad akoestische objecten in Unreal](media/unreal-materials-tab-details.png)

1. De knop met het tabblad **materialen** wordt gebruikt om deze pagina te openen.
2. Een korte beschrijving van wat u moet doen met deze pagina.
3. De lijst met materialen die worden gebruikt op het niveau, afkomstig van de Actors die zijn gelabeld als **AcousticsGeometry**. Als u hier op een materiaal klikt, worden alle objecten in de scène geselecteerd die dit materiaal gebruiken.
4. Toont het akoestische materiaal waaraan het scène materiaal is toegewezen. Klik op een vervolg keuzelijst om een scène materiaal opnieuw toe te wijzen aan een ander akoestisch materiaal.
5. Toont de akoestische absorptie coëfficiënt van het materiaal dat is geselecteerd in de vorige kolom. Een waarde van nul betekent perfect reflectie (geen absorptie), terwijl een waarde van 1 betekent perfect absorptive (geen reflectie). Als u deze waarde wijzigt, wordt het akoestische materiaal (stap #4) bijgewerkt naar **aangepast**.

Als u wijzigingen aanbrengt in de materialen in uw scène, moet u scha kelen tussen tabbladen in de invoeg toepassing project akoestische om de wijzigingen weer te geven die op het tabblad **materialen** worden weer gegeven.

## <a name="calculate-and-review-listener-probe-locations"></a>Test locaties voor listeners berekenen en controleren

Nadat u de materialen hebt toegewezen, gaat u naar het tabblad **tests** .

### <a name="for-reference-parts-of-the-probes-tab"></a>Ter referentie: Onderdelen van het tabblad tests

![Scherm afbeelding van het tabblad akoestische tests in Unreal](media/unreal-probes-tab-details.png)

1. De knop met het tabblad probe die wordt gebruikt om deze pagina te openen
2. Een korte beschrijving van wat u met deze pagina moet doen
3. Gebruik deze optie om een grof of fijn te simuleren. Grof is sneller, maar heeft bepaalde afwegingen. Zie de onderstaande [maken-oplossing](bake-resolution.md) voor meer informatie.
4. Kies de locatie waar de akoestische gegevens bestanden moeten worden geplaatst met dit veld. Klik op de knop met '... ' een Mapkiezer gebruiken. Zie [gegevens bestanden](#Data-Files) hieronder voor meer informatie over gegevens bestanden.
5. De gegevens bestanden voor deze scène worden aangeduid met het voor voegsel dat u hier opgeeft. De standaard waarde is [niveau naam] _AcousticsData.
6. Klik op de knop **berekenen** om de scène te voxelize en de locaties van het test punt te berekenen. Dit wordt lokaal op uw computer uitgevoerd en moet worden uitgevoerd voordat u een maken kunt uitvoeren. Nadat de tests zijn berekend, worden de bovenstaande besturings elementen uitgeschakeld en wordt deze knop gewijzigd in **duidelijk**. Klik op de knop **wissen** om de berekeningen te wissen en de besturings elementen in te scha kelen, zodat u opnieuw kunt berekenen met nieuwe instellingen.

Tests moeten worden geplaatst via het geautomatiseerde proces dat op het tabblad **tests** wordt vermeld.


### <a name="what-the-calculate-button-calculates"></a>Wat wordt berekend met de knop berekenen

De knop **berekenen** neemt alle gegevens die u tot nu toe hebt opgegeven (geometrie, navigatie, materialen en de instelling grof/fijn) en doorloopt verschillende stappen:

1. Het neemt de geometrie van de scène netten en berekent een Voxel-volume. Het Voxel-volume is een driedimensionaal volume dat uw hele scène omsluit en bestaat uit kleine kubieke ' voxels '. De grootte van de voxels wordt bepaald door de simulatie frequentie, die wordt ingesteld door de instelling **simulatie oplossing** . Elke Voxel is gemarkeerd als ' lucht openen ' of met een scène geometrie. Als een Voxel Geometry bevat, wordt de Voxel gelabeld met de absorptie coëfficiënt van het materiaal dat aan die geometrie is toegewezen.
2. Vervolgens worden de navigatie gegevens gebruikt voor het berekenen van akoestische interessante locaties waar de speler mogelijk naartoe gaat. Er wordt geprobeerd een redelijk kleine set van deze locaties te vinden die kleinere gebieden bevatten, zoals kantjes en hal, en vervolgens naar kamers om ruimten te openen. Voor kleine scènes is dit doorgaans minder dan 100 locaties, terwijl grote scènes kunnen tot 1000.
3. Voor elk van de laatste listeners die worden berekend, bepaalt het een aantal para meters, zoals hoe ' open ' de ruimte is, de grootte van de ruimte waarin deze zich bevindt, enzovoort.
4. De resultaten van deze berekeningen worden opgeslagen in bestanden op de locatie die u opgeeft (zie onderstaande [gegevens bestanden](#Data-Files) )

Afhankelijk van de grootte van uw scène en de snelheid van uw computer, kan het enkele minuten duren voordat deze berekeningen worden uitgevoerd.

Zodra deze berekeningen zijn voltooid, kunt u de Voxel-gegevens en de locatie van het test punt bekijken om ervoor te zorgen dat de maken u goede resultaten krijgt. Dingen zoals een onjuist navigatie-net of ontbrekende/extra geometrie worden doorgaans snel zichtbaar in de preview-versie, zodat u deze kunt corrigeren.


## <a name="debug-display"></a>Fout opsporing weer geven

Nadat de test berekening is voltooid, wordt er een nieuwe actor weer gegeven in de wereld contourer met de naam **AcousticsDebugRenderer**. Door de selectie vakjes test **tests** te controleren en **voxels** in te scha kelen, wordt de weer gave van fout opsporing in de View Port van de redacteur ingeschakeld.

![Scherm opname van akoestische weer gave van fout opsporing renderer in Unreal editor](media/acoustics-debug-renderer.png)

Als er geen voxels of tests op uw niveau worden weer gegeven, moet u ervoor zorgen dat realtime-rendering is ingeschakeld in de View Port.

![Scherm opname van de optie voor het weer geven van real-time in Unreal](media/unreal-real-time-rendering.png)

### <a name="voxels"></a>Voxels

Voxels worden in het scène venster weer gegeven als groene kubussen rond de deel geometrie. Voxels die alleen lucht bevatten, worden niet weer gegeven. Er is een grote groene doos rond uw hele scène die het volledige Voxel volume aanduidt dat wordt gebruikt in de simulatie.
Ga door uw scène en controleer of de occluding geometrie voxels heeft. Controleer ook of er niet-akoestische objecten, zoals botsings netten, niet zijn voxelized. De scène camera moet binnen ongeveer 5 meters van het object liggen om de voxels weer te geven.

Als u de voxels die is gemaakt met ruwe resolutie en een nauw keurige resolutie vergelijkt, ziet u dat de ruwe voxels twee maal zo groot zijn.

![Scherm opname van akoestische voxels preview in Unreal-editor](media/unreal-voxel-preview.png)

### <a name="probe-points"></a>Test punten

Test punten zijn synoniemen met mogelijke Player-locaties (listener). Wanneer Baking wordt uitgevoerd, berekent de simulatie de geluids bronnen die alle mogelijke bron locaties aan elk test punt verbinden. Tijdens runtime wordt de locatie van de listener geinterpolatie tussen test punten in de buurt.

Het is belang rijk om te controleren of er test punten bestaan waar de speler naar verwachting in de scène gaat. Test punten worden op het navigatie-net door de project akoestische engine geplaatst en kunnen niet worden verplaatst of bewerkt. Zorg er dus voor dat het navigatie net alle mogelijke locaties van de speler bedekt door de test punten te controleren.

![Scherm opname van akoestische tests voor beeld in Unreal](media/unreal-probes-preview.png)

Zie [maken-oplossing](bake-resolution.md) voor meer informatie over grove nauw keurigheid.

## <a name="bake-your-level-using-azure-batch"></a>Maken uw niveau met behulp van Azure Batch

U kunt uw scène maken met een berekenings cluster in de Cloud met behulp van de Azure Batch-service. De Unreal-invoeg toepassing voor project akoestische maakt rechtstreeks verbinding met Azure Batch om een Azure Batch cluster voor elke maken te instantiëren, te beheren en te verbreken. Voer uw Azure-referenties in op het tabblad maken, selecteer een type en grootte voor de cluster machine en klik op maken.

### <a name="for-reference-parts-of-the-bake-tab"></a>Ter referentie: Delen van het tabblad maken

![Scherm afbeelding van het tabblad akoestische maken in Unreal](media/unreal-bake-tab-details.png)

1. De knop met het tabblad maken wordt gebruikt om deze pagina te openen.
2. Een korte beschrijving van wat u op deze pagina moet doen.
3. Velden om uw Azure-referenties in te voeren nadat uw Azure-account is gemaakt. Zie [een Azure batch-account maken](create-azure-account.md)voor meer informatie.
4. Start Azure Portal voor het beheren van uw abonnementen, het controleren van het gebruik en het weer geven van facturerings gegevens, enzovoort. 
5. Type Azure batch Compute-knoop punt dat moet worden gebruikt voor de berekening. Het knooppunt type moet worden ondersteund door de Azure Data Center-locatie. Als dat niet het geval is, moet u op **Standard_F8s_v2**.
6. Het aantal knoop punten dat moet worden gebruikt voor deze berekening. Het nummer dat u hier invoert, is van invloed op de tijd voor het volt ooien van de maken en wordt beperkt door uw Azure Batch core-toewijzing. De standaard toewijzing biedt alleen ondersteuning voor twee 8 kern knooppunten of een 1 16-kern knooppunt, maar kan worden uitgebreid. Zie [een Azure batch-account maken](create-azure-account.md)voor meer informatie over kern toewijzings beperkingen.
7. Schakel dit selectie vakje in om uw reken groep te configureren voor het gebruik van [knoop punten met een lage prioriteit](https://docs.microsoft.com/azure/batch/batch-low-pri-vms). Reken knooppunten met lage prioriteit hebben veel lagere kosten, maar zijn mogelijk niet altijd beschikbaar of kunnen op elk gewenst moment worden voorranglen.
8. De hoeveelheid tijd die nodig is om uw taak uit te voeren in de Cloud. Dit geldt niet voor de start tijd van het knoop punt. Zodra de taak is gestart, weet u hoe lang deze moet zijn voordat u de resultaten terugkrijgt. Houd er rekening mee dat dit slechts een schatting is.
9. De totale hoeveelheid computer tijd die nodig is om de simulaties uit te voeren. Dit is de totale hoeveelheid reken tijd van het knoop punt die wordt gebruikt in Azure. Zie [schatting](#Estimating-bake-cost) van de kosten voor maken hieronder voor meer informatie over het gebruik van deze waarde.
10. Klik op de knop maken om de maken naar de cloud te verzenden. Terwijl een taak wordt uitgevoerd, wordt in plaats daarvan de **taak annuleren** weer gegeven. Als er fouten zijn opgetreden op dit tabblad of als de werk stroom op het tabblad **tests** niet is voltooid, wordt deze knop uitgeschakeld.
11. Het aantal tests voor uw scène zoals berekend op het tabblad **tests** . Het aantal tests bepaalt het aantal simulaties dat moet worden uitgevoerd in de Cloud. U kunt niet meer knoop punten opgeven dan er tests zijn.
12. Dit bericht geeft u de huidige status van de taak of als er fouten op dit tabblad staan, wat deze fouten zijn.

U kunt altijd volledige informatie krijgen over actieve taken, reken Pools en opslag op het [Azure Portal](https://portal.azure.com).

Terwijl een taak wordt uitgevoerd, wordt de **maken** -knop gewijzigd in **taak annuleren**. Gebruik deze knop om de taak in voortgang te annuleren. Het annuleren van een taak kan niet ongedaan worden gemaakt, er zijn geen resultaten meer beschikbaar en er worden nog steeds kosten in rekening gebracht voor elke Azure-reken tijd die vóór de annulering wordt gebruikt.

Nadat u een maken hebt gestart, kunt u Unreal sluiten. Afhankelijk van het project, het knooppunt type en het aantal knoop punten kan een Cloud-maken enkele uren duren. De taak status maken wordt bijgewerkt wanneer u het project opnieuw laadt en het akoestische venster opent. Als de taak is voltooid, wordt het uitvoer bestand gedownload.

De Azure-referenties worden veilig opgeslagen op uw lokale machine en gekoppeld aan uw Unreal-project. Ze worden alleen gebruikt om een beveiligde verbinding met Azure tot stand te brengen.

### <a name="Estimating-bake-cost"></a>Kosten voor Azure maken schatten

Als u wilt schatten wat een bepaalde maken kost, neemt u de waarde voor **geschatte reken kosten**op, die een duur is en vermenigvuldigt u die met de kosten per uur in uw lokale valuta van het **VM-knooppunt type** dat u hebt geselecteerd. Het resultaat bevat niet de tijd van het knoop punt dat nodig is om de knoop punten actief en werkend te krijgen. Als u bijvoorbeeld **Standard_F8s_v2** selecteert voor het knooppunt type, dat een kost prijs van $0.40/uur heeft en de geschatte reken kosten 3 uur en 57 minuten zijn, is de geschatte kosten voor het uitvoeren van de taak $0,40 * ~ 4 uur = ~ $1,60. De werkelijke kosten zijn waarschijnlijk iets hoger als gevolg van de extra tijd voor het starten van de knoop punten. U kunt de kosten van het knoop punt per uur vinden op de pagina met [Azure batch prijzen](https://azure.microsoft.com/pricing/details/virtual-machines/linux) (Selecteer ' verwerkte optimalisatie ' of ' high performance Compute ' voor de categorie).

### <a name="reviewing-the-bake-results"></a>De maken-resultaten controleren

Nadat de maken is voltooid, controleert u of de voxels en test punten zich op de verwachte locaties bevinden door de runtime-invoeg toepassing uit te voeren.

## <a name="Data-Files"></a>Gegevens bestanden

Er zijn op verschillende momenten vier gegevens bestanden die door deze invoeg toepassing zijn gemaakt. Er is slechts één van beide nodig tijdens de runtime en deze wordt geplaatst in de map Content/akoestische van het project, die automatisch wordt toegevoegd aan het pakketmap van het project. De andere drie bevinden zich in de map met akoestische gegevens en zijn niet verpakt.

* **[Project]/Config/ProjectAcoustics.cfg**: In dit bestand worden de gegevens opgeslagen die u invoert in velden in de gebruikers interface van de akoestische modus. De locatie en naam van dit bestand kunnen niet worden gewijzigd. Er zijn andere waarden opgeslagen in dit bestand die van invloed zijn op de maken, maar ze zijn voor ervaren gebruikers en mogen niet worden gewijzigd.
* **[Project]/Content/Acoustics/[LevelName]\_AcousticsData.ace**: Dit bestand is wat wordt gemaakt tijdens de maken-simulatie en bevat de opzoek gegevens die door de runtime worden gebruikt om de akoestische beelden van uw scène weer te geven. De locatie en naam van dit bestand kunnen worden gewijzigd met behulp van de velden op het tabblad **tests** . Als u de naam van dit bestand wilt wijzigen nadat het is gemaakt, verwijdert u de UAsset uit uw Unreal-project, wijzigt u de naam van het bestand buiten Unreal in Verkenner en importeert u dit bestand opnieuw in Unreal om een nieuwe UAsset te maken. Het is niet mogelijk om de naam van de UAsset zelf te wijzigen.
* **[Project]/Plugins/ProjectAcoustics/AcousticsData/[LevelName]\_AcousticsData.vox**: In dit bestand worden de voxelized-akoestische geometrie en de materiaal eigenschappen opgeslagen. Berekend met behulp van de knop **berekenen** op het tabblad **tests** . De locatie en naam van dit bestand kunnen worden gewijzigd met behulp van de velden op het tabblad **tests** .
* **[Project]/Plugins/ProjectAcoustics/AcousticsData/[LevelName]\_AcousticsData\_config.xml**: In dit bestand worden de para meters die worden berekend met behulp van de knop **berekenen** op het tabblad **tests** . De locatie en naam van dit bestand kunnen worden gewijzigd met behulp van de velden op het tabblad **tests** .

Zorg ervoor dat u het bestand *. Ace dat is gedownload van Azure niet verwijdert. Dit bestand kan niet worden hersteld, behalve door de scène te rebakingen.

## <a name="next-steps"></a>Volgende stappen
* De [ontwerp besturings elementen voor Unreal](unreal-workflow.md) verkennen
* Bekijk de [ontwerp concepten van het project akoestische ontwerpen](design-process.md)

