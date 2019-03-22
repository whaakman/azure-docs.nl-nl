---
title: Project akoestische Unreal Bake zelfstudie
titlesuffix: Azure Cognitive Services
description: Dit document beschrijft het proces voor het indienen van een akoestische bake met de extensie Unreal-editor.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: michem
ms.openlocfilehash: 544de5a3ac48c12d75f05a1c9adb56f48bb540f4
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58311550"
---
# <a name="project-acoustics-unreal-bake-tutorial"></a>Project akoestische Unreal Bake zelfstudie
Dit document beschrijft het proces voor het indienen van een akoestische bake met de extensie Unreal-editor.

Er zijn vijf stappen tot het uitvoeren van een bake:

1. Maken of uw player navigatie mesh taggen
2. Tag akoestische geometrie
3. Akoestische materiaal eigenschappen toewijzen aan geometrie
4. Plaatsing van de Preview-test
5. Bake

## <a name="open-the-project-acoustics-editor-mode"></a>De bewerkingsmodus akoestische Project openen

Importeer het Project akoestische invoegtoepassing pakket aan uw project. Zie voor hulp bij dit, de [Unreal integratie](unreal-integration.md) onderwerp. Zodra de invoegtoepassing is geïntegreerd, opent u de gebruikersinterface akoestische door te klikken op het pictogram Nieuw akoestische modus.

![Schermopname van Editor Unreal akoestische Mode option](media/acoustics-mode.png)

## <a name="tag-actors-for-acoustics"></a>Tag actoren voor akoestische

Het tabblad objecten is het eerste tabblad dat wordt weergegeven wanneer u de modus akoestische opent. Gebruik dit tabblad om tag actoren in uw niveau, voegt de **AcousticsGeometry** of **AcousticsNavigation** tags aan de actoren.

Selecteer een of meer objecten in de hele wereld Outliner of gebruik de **selectie** sectie voor hulp bij alle objecten van een specifieke categorie selecteren. Zodra de objecten zijn geselecteerd, gebruikt u de **taggen** sectie toe te passen van de gewenste tag op de geselecteerde objecten.

### <a name="for-reference-the-objects-tab-parts"></a>Ter referentie: De objecten tabblad delen

![Schermafbeelding van akoestische objecten tabblad in Unreal](media/unreal-objects-tab-details.png)

1. Selectie knoppen op het tabblad (**objecten** tabblad is geselecteerd). Gebruik deze knoppen om te zien hoe de verschillende stappen een bake akoestische van boven naar beneden te doen.
2. Een korte beschrijving van wat u moet doen met behulp van deze pagina.
3. Beschikbare kiezers voor actoren op het niveau.
4. Te klikken op **Selecteer** selecteert u alle objecten op het niveau dat overeenkomt met ten minste een van de gecontroleerde actor-typen.
5. Te klikken op **alle selecties opheffen** , de huidige selectie worden gewist. Dit is hetzelfde als de ESC-toets te maken.
6. Gebruik deze keuzerondjes te kiezen of ze de geometrie- of navigatie-label toepassen op de geselecteerde actoren.
7. Te klikken op **Tag** voegt het geselecteerde label toe aan alle geselecteerde actoren.
8. Te klikken op **label verwijderen** , het geselecteerde label wordt verwijderd uit alle geselecteerde actoren.
9. Te klikken op **Selecteer gelabeld** wordt de huidige selectie wissen en alle actoren met het geselecteerde label selecteren.
10. Deze statistieken laten zien hoeveel actors zijn gelabeld met elk type tag.

### <a name="tag-acoustics-occlusion-and-reflection-geometry"></a>Tag akoestische bedekking en reflectie geometrie

Open het tabblad objecten van het venster akoestische. Alle objecten markeren als akoestische geometrie, als ze moeten occlude, weerspiegelen, of geluid absorberen. Akoestische geometry kan omvatten zaken zoals volledig is ontwikkeld, wanden daken, windows & glazen venster, tapijt en grote meubels. U kunt elke willekeurige niveau van complexiteit voor deze objecten gebruiken. Omdat de scène voxelized voordat de simulatie is, zijn zeer gedetailleerde netten, zoals structuren met veel kleine bladeren, niet meer kostbaar zijn om u te maken dan vereenvoudigde objecten.

Dingen die al dan niet mogen invloed hebben op de akoestische, zoals onzichtbaar botsing netten zijn niet opgenomen.

De transformatie van een object op het moment van de berekening van de test (via het tabblad tests hieronder) in de resultaten bake is opgelost. Een van de gemarkeerde objecten in de scène wordt verplaatst, moet de berekening van de test opnieuw uitvoeren en de scène rebaking.

## <a name="create-or-tag-a-navigation-mesh"></a>Maken of een navigatie-NET taggen

Een navigatie-NET wordt gebruikt voor test-punten voor simulatie plaatsen. U kunt de Unreal [Nav NET grenzen Volume](https://api.unrealengine.com/INT/Engine/AI/BehaviorTrees/QuickStart/2/index.html), of u kunt uw eigen mesh navigatie opgeven. U moet ten minste één object als label **akoestische navigatie**.

## <a name="select-acoustic-materials"></a>Akoestische materiaal selecteren

Nadat uw objecten zijn gemarkeerd, klikt u op de **materiaal** knop om te gaan naar het tabblad materialen. Op dit tabblad wordt gebruikt om op te geven van eigenschappen voor het materiaal op het niveau. Voordat een actors zijn gemarkeerd, wordt deze niet leeg zijn.

Het akoestisch materiaal beheren geluid energieverbruik weergegeven van elke surface. Het standaard akoestische materiaal is vergelijkbaar met concreet opname. Project akoestische stelt materialen op basis van de naam van de scène materiaal.

De tijd weerklank van een bepaald materiaal in een kamer is omgekeerd gerelateerd aan de coëfficiënt opname met de meeste materiaal met waarden voor opname in het bereik 0,01-0.20. Materialen met opname coëfficiënten boven in dit bereik zijn zeer absorberend. Bijvoorbeeld als een kamer geluiden te reverberant, wijzigt u het akoestisch materiaal van de wanden, basis of maximum in een van de hogere absorptivity. De toewijzing van akoestische materiaal geldt voor alle actoren die gebruikmaken van dit materiaal scène.

![Grafiek met negatieve correlatie weerklank tijd met opname coëfficiënt](media/reverb-time-graph.png)

### <a name="for-reference-parts-of-the-materials-tab"></a>Ter referentie: Onderdelen van het tabblad materiaal

![Schermafbeelding van akoestische objecten tabblad in Unreal](media/unreal-materials-tab-details.png)

1. De **materiaal** tabblad knop, die wordt gebruikt om deze pagina.
2. Een korte beschrijving van wat u moet doen met behulp van deze pagina.
3. De lijst van materiaal dat wordt gebruikt op het niveau, die is overgenomen uit de actoren gelabeld als **AcousticsGeometry**. Op een materiaal hier te klikken om alle objecten in de scène die gebruikmaken van dit materiaal te selecteren.
4. Geeft het akoestisch materiaal dat het scène-materiaal aan is toegewezen. Klik op een vervolgkeuzelijst als u wilt toewijzen van een scène materiaal op een andere akoestische materiaal.
5. De coëfficiënt akoestische opname van het materiaal dat is geselecteerd in de vorige kolom bevat. Een waarde van nul betekent precies bij reflectieve (geen opname), terwijl een waarde van 1 betekent precies absorptie (geen weerspiegeling). Het materiaal akoestische (stap #4) te wijzigen van deze waarde wordt bijgewerkt **aangepaste**.


## <a name="calculate-and-review-listener-probe-locations"></a>Berekenen en listener test locaties bekijken

Na het toewijzen van het materiaal, Ga naar de **tests** tabblad.

### <a name="for-reference-parts-of-the-probes-tab"></a>Ter referentie: Onderdelen van het tabblad tests

![Schermafbeelding van akoestische tests tabblad in Unreal](media/unreal-probes-tab-details.png)

1. De **tests** tabblad-knop die wordt gebruikt om deze pagina
2. Een korte beschrijving van wat u moet doen met behulp van deze pagina
3. Met deze optie kiest u een resolutie grof of fijn simulatie. Grof is sneller, maar heeft bepaalde compromissen. Zie [grof vs fijn resolutie](#Coarse-vs-Fine-Resolution) hieronder voor meer informatie.
4. Kies de locatie waar de gegevensbestanden akoestische moeten worden geplaatst met behulp van dit veld. Klik op de knop met '...' voor het gebruik van een map kiezen. Zie voor meer informatie over gegevensbestanden [gegevensbestanden](#Data-Files) hieronder.
5. De gegevensbestanden voor deze scène wordt de naam hier met behulp van het voorvoegsel dat is opgegeven. De standaardwaarde is '_AcousticsData [naam]'.
6. Klik op de **berekenen** knop voxelize de scène en de test-distributiepuntlocaties te berekenen. Dit wordt dan niet lokaal op uw computer en moet worden uitgevoerd vóór het uitvoeren van een bake. Nadat de tests zijn berekend, de besturingselementen die hierboven wordt uitgeschakeld en deze knop wordt gewijzigd, zodat het zegt **wissen**. Klik op de **wissen** om te wissen van de berekeningen en de besturingselementen inschakelen zodat u kunt opnieuw berekenen met behulp van de nieuwe instellingen.

Tests moeten worden geplaatst door het geautomatiseerde proces dat is opgegeven de **tests** tabblad.


### <a name="what-the-calculate-button-calculates"></a>Wat de knop 'Berekenen' wordt berekend

De **berekenen** knop neemt alle gegevens die u tot nu toe hebt opgegeven (geometrie, navigatie, materialen en de instelling grof/fijn) en verschillende stappen doorlopen:

1. Het duurt de geometrie van het scène-netten en een volume voxel berekent. Het volume voxel is een 3-dimensionale volume die uw gehele scène omsluit en bestaat uit kleine kubieke 'voxels'. De grootte van de voxels wordt bepaald door de frequentie simulatie, die is ingesteld door de **simulatie resolutie** instelling. Elke voxel is gemarkeerd als een 'geopend air' of scène geometry bevatten. Als een voxel geometrie bevat wordt de voxel gelabeld met de coëfficiënt voor opname van het materiaal dat is toegewezen aan die geometrie.
2. Vervolgens wordt de navigatie-gegevens voor het berekenen van acoustically interessante locaties waar de speler kunt gaan. Probeert het vinden van een redelijk klein aantal deze locaties met kleinere gebieden zoals inbegrepen en elkaar tegenkwamen, en vervolgens naar ruimten spaties openen. Voor kleine schermen doorgaans is dit minder dan 100 locaties, terwijl grote schermen kunnen maximaal duizend hebben.
3. Voor elk van de laatste listener locaties berekent, bepaalt dat een aantal parameters, zoals hoe 'geopend' is de ruimte, de grootte van de ruimte die deel uitmaakt, enzovoort.
4. De resultaten van deze berekeningen worden opgeslagen in bestanden op de locatie die u opgeeft (Zie [gegevensbestanden](#Data-Files) hieronder)

Deze berekeningen kunnen enkele minuten duren, afhankelijk van de grootte van uw scène en de snelheid van uw computer.

Zodra deze berekeningen voltooid zijn, kunt u zowel de voxel-gegevens en de test-distributiepuntlocaties om ervoor te zorgen dat de bake u goede resultaten krijgt bekijken. Een ongeldige navigatie net zoals of ontbrekende/extra geometrie doorgaans snel zichtbaar zijn in de Preview-versie, zodat u deze kunt oplossen.


## <a name="debug-display"></a>Fouten opsporen in weergeven

Nadat de berekening van de test is voltooid, een nieuwe actor wordt weergegeven in de hele wereld Outliner met de naam **AcousticsDebugRenderer**. Controle van de **renderen tests** en **Voxels renderen** selectievakjes schakelt u de weergave fouten opsporen in de viewport van de editor.

![Schermopname van akoestische fouten opsporen in Renderer actor in Unreal-Editor](media/acoustics-debug-renderer.png)

Als u niet alle voxels of tests overlay bekijken op het niveau van ziet, zorg er dan voor dat realtime rendering is ingeschakeld in de viewport.

![Schermafbeelding van de renderingoptie realtime-in Unreal](media/unreal-real-time-rendering.png)

### <a name="voxels"></a>Voxels

Voxels worden weergegeven in het scène-venster als groen kubussen rond deelnemende geometry. Voxels die alleen lucht bevatten, worden niet weergegeven. Er is een grote groene vakje rond uw gehele scène die het volledige voxel volume dat wordt gebruikt in de simulatie wordt aangeduid.
Een scène verplaatsen en controleer of de geometrie acoustically occluding voxels heeft. Controleer ook dat niet-akoestische objecten zoals botsing netten voxelized nog niet zijn. De camera van het scène heeft binnen ongeveer 5 meters van het object voor de voxels om weer te geven.

Als u de voxels die zijn gemaakt met behulp van abrupte resolutie vs fijn resolutie vergelijkt, ziet u dat de grof voxels tweemaal zo groot zijn.

![Schermafbeelding van akoestische voxels preview in Unreal-editor](media/unreal-voxel-preview.png)

### <a name="probe-points"></a>Test-punten

Test punten zijn gelijk aan mogelijke player (listener)-locaties. Wanneer bakken, berekent de simulatie de locaties van alle mogelijke bron verbinding te maken met elk punt test akoestische. Tijdens runtime, wordt de locatie van de listener geïnterpoleerde onder in de buurt test punten.

Het is belangrijk om te controleren dat test punten overal die in de speler wordt verwacht dat in de scène reizen bestaan. Test-punten worden geplaatst op het net navigatie door de engine voor het Project akoestische en kunnen niet worden verplaatst of bewerkt, dus zorg ervoor de navigatie mesh-omvat alle mogelijke player locaties door te inspecteren van de test-punten.

![Schermafbeelding van akoestische tests Preview-versie in Unreal](media/unreal-probes-preview.png)

### <a name="Coarse-vs-Fine-Resolution"></a>Fijn Grof Visual Studio-oplossing

Het enige verschil tussen de instellingen voor het omzetten van grove en goed is de frequentie waarmee de simulatie wordt uitgevoerd. Fijn maakt gebruik van een twee keer zo hoog als grof frequentie.
Terwijl dit misschien eenvoudig lijken, heeft een aantal gevolgen voor de akoestische simulatie:

* De golflengte voor grof twee keer zo lang als fijn is, en daarom de voxels tweemaal zo groot zijn.
* De tijd van de simulatie is direct gerelateerd aan de voxel grootte, waardoor een grof bake ongeveer 16 keer sneller dan een prima bake.
* Portals (bijvoorbeeld deuren of windows) kleiner is dan de grootte van de voxel niet kunnen worden gesimuleerd. De grof instelling kan ertoe leiden dat sommige van deze kleinere portals voor het niet worden gesimuleerd; Daarom geeft ze niet geluid via tijdens runtime. U kunt zien als dit gebeurt door het voxels weer te geven.
* De frequentie van de lagere simulatie resulteert in minder diffraction rond hoeken en randen.
* Geluid bronnen kunnen zich niet in "gevuld" voxels die voxels die geometry bevatten: dit resulteert in geen geluid. Het is moeilijker geluid bronnen plaatsen, zodat ze niet binnen de grotere voxels van grove vallen dan wanneer u het fijn instelling.
* De grotere voxels wordt inbreuk maken meer bij portals, zoals hieronder wordt weergegeven. De eerste afbeelding is gemaakt met behulp van abrupte, terwijl de tweede is de dezelfde poort met fijn resolutie. Zoals aangegeven door de rode markeringen, moet u er veel minder inbreuk in de poort die met behulp van de instelling van de orde is. De blauwe lijn is de poort, zoals gedefinieerd door de geometrie, terwijl de rode lijn de effectieve akoestische portal gedefinieerd door de grootte van de voxel wordt. Hoe deze indringers wordt afgespeeld uit in een bepaalde situatie is afhankelijk van de volledig op hoe de voxels uitgelijnd met de geometrie van de portal, die wordt bepaald door de grootte en de locaties van de objecten in de scène.

![Schermafbeelding van grove voxels invullen van een poort in Unreal](media/unreal-coarse-bake.png)

![Schermafbeelding van fijn voxels in een poort in Unreal](media/unreal-fine-bake.png)

## <a name="bake-your-level-using-azure-batch"></a>Maken van uw niveau met behulp van Azure Batch

U kunt maken van een scène met een rekencluster in de cloud met behulp van de Azure Batch-service. De Project akoestische Unreal invoegtoepassing maakt rechtstreeks verbinding met Azure Batch te maken, beheren en een Azure Batch-cluster voor elke bake afbreken. Op het tabblad Bake, Voer uw Azure-referenties, selecteer een clustertype van de machine en de grootte en Bake op.

### <a name="for-reference-parts-of-the-bake-tab"></a>Ter referentie: Onderdelen van het tabblad bake

![Schermafbeelding van akoestische verdient tabblad in Unreal](media/unreal-bake-tab-details.png)

1. De knop Tab verdient is gebruikt om deze pagina.
2. Een korte beschrijving van wat te doen op deze pagina.
3. Velden in te voeren van uw Azure-referenties wanneer uw Azure-account is gemaakt. Zie voor meer informatie, [Azure Batch-Account maken](create-azure-account.md).
4. Start de Azure-portal voor het beheren van uw abonnementen, gebruik controleren en factureringsgegevens enzovoort weergeven. 
5. Azure batch compute-knooppunt dat moet worden gebruikt voor de berekening. Het knooppunttype moet worden ondersteund door de locatie van uw Azure data center. Als niet weet, laat u op **Standard_F8s_v2**.
6. Het aantal knooppunten moet worden gebruikt voor deze berekening. Het getal dat u hier opgeeft, is van invloed op de tijd voor het voltooien van de bake en wordt beperkt door de toewijzing van uw Azure Batch-core. De standaard-toewijzing alleen kunt u twee 8-core knooppunten of het knooppunt van een 16-core, maar kan worden uitgebreid. Zie voor meer informatie over core toewijzing beperkingen [Azure Batch-Account maken](create-azure-account.md).
7. Schakel dit selectievakje in om te configureren van uw compute-toepassingen wilt gebruiken [knooppunten met lage prioriteit](https://docs.microsoft.com/azure/batch/batch-low-pri-vms). Rekenknooppunten met lage prioriteit hebben veel lagere kosten, maar ze mogelijk niet altijd beschikbaar of op elk gewenst moment kunnen worden verschoven.
8. De hoeveelheid tijd die is het waarschijnlijk moet worden uitgevoerd voor de taak wordt uitgevoerd in de cloud. Dit omvat geen opstarttijd knooppunt. Zodra de taak uitgevoerd wordt, is dit over hoe lang moet zijn voordat u de resultaten teruggaan. Houd er rekening mee dat dit slechts een schatting te maken is.
9. De totale hoeveelheid computertijd die nodig zijn voor de simulaties uitvoeren. Dit is de totale hoeveelheid tijd knooppunt dat wordt gebruikt in Azure. Zie [Estimating bake kosten](#Estimating-bake-cost) hieronder voor meer informatie over het gebruik van deze waarde.
10. Klik op de knop Bake om in te dienen de bake naar de cloud. Terwijl een taak wordt uitgevoerd, geeft dit **taak annuleren** in plaats daarvan. Als er fouten zijn op dit tabblad, of als de werkstroom op de **tests** tabblad is niet voltooid, wordt deze knop wordt uitgeschakeld.
11. Het aantal test voor uw scène berekend op de **tests** tabblad. Het aantal tests bepaalt het aantal simulaties die moeten worden uitgevoerd in de cloud. U kunt meer knooppunten dan er tests zijn niet opgeven.
12. Dit bericht ziet u de huidige status van de taak, of als er fouten op dit tabblad zijn, wat deze fouten zijn.

U krijgt altijd volledige informatie over actieve taken, pools compute en opslag op de [Azure-portal](https://portal.azure.com).

Terwijl een taak wordt uitgevoerd de **verdient** knop verandert **taak annuleren**. Gebruik deze knop om te annuleren van de taak wordt uitgevoerd. Annuleren van een taak kan niet ongedaan worden en er zijn geen resultaten zijn beschikbaar voor alle Azure-rekenen keer gebruikt voor de annulering wordt er nog steeds kosten in rekening gebracht.

Nadat u een bake hebt gestart, kunt u Unreal sluiten. Afhankelijk van het project, knooppunttype en het aantal knooppunten, kan een cloud-bake enkele uren duren. De taakstatus bake wordt bijgewerkt wanneer u het project opnieuw laden en het venster akoestische open. Als de taak is voltooid, wordt het uitvoerbestand worden gedownload.

De Azure-referenties zijn veilig opgeslagen op uw lokale computer en die zijn gekoppeld aan uw Unreal project. Ze worden gebruikt uitsluitend tot stand brengen van een beveiligde verbinding met Azure.

### <a name="Estimating-bake-cost"></a> Azure bake kosten schatten

Voor een schatting van wat een bepaalde bake kost, nemen de waarde die wordt weergegeven voor **geschatte kosten voor Compute**, dit is een duur en vermenigvuldigen die door het elk uur kosten in uw lokale valuta van de **VM knooppunttype** u hebt geselecteerd. Het resultaat bevat niet de knooppunt tijd die nodig zijn aan de slag van de knooppunten en wordt uitgevoerd. Als u bijvoorbeeld **Standard_F8s_v2** voor uw knooppunttype heeft een prijs van $ 0,40/uur, en de geschatte kosten voor Compute 3 uur en 57 minuten, wordt de geschatte kosten voor het uitvoeren van de taak is $0,40 * ~ 4 uur = ~ $1,60. De werkelijke kosten wordt waarschijnlijk een enigszins hoger vanwege de extra tijd om op te halen van de knooppunten gestart. U vindt het uurtarief knooppunt kosten op de [Azure Batch-prijzen](https://azure.microsoft.com/pricing/details/virtual-machines/linux) pagina (Selecteer 'Compute geoptimaliseerd' of 'High performance computing' voor de categorie).

### <a name="reviewing-the-bake-results"></a>De bake resultaten te bekijken

Nadat de bake is voltooid, moet u controleren of de punten voxels en test in de verwachte locaties zijn door het uitvoeren van de runtime-invoegtoepassing.

## <a name="Data-Files"></a>Gegevensbestanden

Er zijn vier gegevensbestanden die zijn gemaakt door deze invoegtoepassing op verschillende tijdstippen. Slechts één van deze is nodig tijdens runtime en wordt geplaatst in de map inhoud/akoestische van uw project, die wordt automatisch toegevoegd aan het pad van de verpakking van uw project. De andere drie zijn in de map akoestische gegevens en niet zijn verpakt.

* **[Project]/Config/ProjectAcoustics.cfg**: Dit bestand slaat de gegevens die u in de velden in de gebruikersinterface van de modus akoestische invoeren. De locatie en naam van dit bestand kunnen niet worden gewijzigd. Er zijn andere waarden die zijn opgeslagen in dit bestand die invloed hebben op de bake, maar ze zijn bedoeld voor ervaren gebruikers en mag niet worden gewijzigd.
* **[Project]/Content/Acoustics/[LevelName]\_AcousticsData.ace**: Dit bestand is wat wordt gemaakt tijdens de simulatie bake en bevat de opzoekgegevens gebruikt door de runtime voor de akoestische van uw scène renderen. De locatie en naam van dit bestand kunnen worden gewijzigd met de velden op de **tests** tabblad.
* **[Project]/Plugins/ProjectAcoustics/AcousticsData/[LevelName]\_AcousticsData.vox**: Dit bestand wordt opgeslagen de voxelized akoestische geometrie en eigenschappen van het materiaal. Berekend op basis van de **berekenen** knop op de **tests** tabblad. De locatie en naam van dit bestand kunnen worden gewijzigd met de velden op de **tests** tabblad.
* **[Project]/Plugins/ProjectAcoustics/AcousticsData/[LevelName]\_AcousticsData\_config.xml**: Dit bestand wordt opgeslagen parameters berekend op basis van de **berekenen** knop op de **tests** tabblad. De locatie en naam van dit bestand kunnen worden gewijzigd met de velden op de **tests** tabblad.

Let erop dat niet als u wilt verwijderen van het bestand *.ace is gedownload van Azure. Dit bestand is niet hersteld, met uitzondering van de scène rebaking.

## <a name="next-steps"></a>Volgende stappen
* Verken de [besturingselementen voor Unreal ontwerpen](unreal-workflow.md)
* Verken de [Project akoestische ontwerpconcepten](design-process.md)

