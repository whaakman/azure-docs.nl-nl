---
title: Akoestische met Project akoestische - Cognitive Services maken
description: Dit document beschrijft het proces voor het indienen van een akoestische bake met de extensie van Unity-editor.
services: cognitive-services
author: kegodin
manager: noelc
ms.service: cognitive-services
ms.component: acoustics
ms.topic: article
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: 0e16ec765ae3cbef8a941f43a149428ffdf5bd8d
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/17/2018
ms.locfileid: "40181577"
---
# <a name="bake-acoustics"></a>Akoestische maken

Dit document beschrijft het proces voor het indienen van een akoestische bake met de extensie van Unity-editor. Zie voor meer achtergrondinformatie over akoestische [wat is er akoestische](what-is-acoustics.md). Zie voor een snelstartgids [aan de slag](getting-started.md).

## <a name="import-the-plugin"></a>De invoegtoepassing te importeren

Importeer het akoestische invoegtoepassing pakket aan uw project. Open vervolgens de gebruikersinterface akoestische door te kiezen **venster > akoestische** in het menu Unity:

![Open akoestische venster](media/WindowAcoustics.png)

## <a name="principles"></a>Principes

Het venster akoestische hulpprogramma haalt de informatie de akoestische engine moet de akoestische voor uw scène berekenen. Er zijn vijf stappen tot het uitvoeren van een bake:

1. Maken of uw player navigatie mesh markeren
2. Akoestische geometrie markeren
3. Akoestische materiaal eigenschappen toewijzen aan geometrie
4. Plaatsing van de Preview-test
5. Bake

Wanneer de bake voltooid is, ziet [overzicht van het ontwerp voor akoestische](design-process.md) voor optionele na bake ontwerp stappen.

## <a name="create-or-mark-a-navigation-mesh"></a>Maken of een navigatie-NET markeren

Een navigatie-NET wordt gebruikt voor test-punten voor simulatie plaatsen. U kunt de Unity [navigatie mesh-werkstroom](https://docs.unity3d.com/Manual/nav-BuildingNavMesh.html), of kunt u uw eigen mesh navigatie. Navigatie-netten die zijn gemaakt met de Unity-werkstroom wordt door het systeem akoestische worden opgehaald. Voor het gebruik van uw eigen netten, markeer ze uit de **objecten** tabblad zoals beschreven in de volgende stap.

## <a name="objects-tab"></a>Tabblad objecten

Open de **objecten** tabblad van de **akoestische** venster. Dit tabblad gebruiken om objecten in uw scène, voegt gewoon toe markeren de **AcousticsGeometry** of **AcousticsNavigation** onderdelen voor het object. U kunt ook de [standaard Unity onderdeel werkstroom](https://docs.unity3d.com/Manual/UsingComponents.html) te markeren of markering opheffen objecten.

Een of meer objecten in het venster van de scène of de hiërarchie selecteren en vervolgens markeren of markering opheffen voor **AcousticsGeometry** of **AcousticsNavigation** zoals hieronder wordt beschreven. Als er niets is geselecteerd, kunt u markeren of markering alles in de scène in één keer verwijderen.

Alleen NET Renderers en terreinen kunnen worden gemarkeerd. Alle objecttypen die worden genegeerd. De selectievakjes wordt markeren of markering opheffen van alle betrokken objecten.

Als u er niets is geselecteerd in de scène hebt, lijkt het alsof de volgende afbeelding:

![Objecten tabblad geen selectie](media/ObjectsTabNoSelectionDetail.png)

Als u iets hebt geselecteerd in het venster scène of hiërarchie hebt, kunt u het eruitziet zoals in de volgende afbeelding:

![Objecten tabblad geen selectie](media/ObjectsTabWithSelectionDetail.png)

### <a name="objects-tab-parts"></a>Tabblad delen van objecten

De onderdelen van de tabbladpagina zijn:

1. Selectie knoppen op het tabblad (**objecten** tabblad is geselecteerd). Gebruik deze knoppen om te zien hoe de verschillende stappen voor het uitvoeren van een bake akoestische van links naar rechts.
2. Een korte beschrijving van wat u moet doen met behulp van deze pagina.
3. Beschikbare filters voor de hiërarchie-venster. Gebruik dit voor het filteren van het venster van de hiërarchie op objecten van het opgegeven type, zodat u ze eenvoudiger kunt markeren. Als u dit niet hebt gedaan, nog iets voor akoestische gemarkeerd, selecteer de laatste twee opties u niets ziet. Ze kunnen echter nuttig is gemarkeerd om objecten te vinden zodra u hebt gedaan, zijn.
4. Als er geen objecten zijn geselecteerd, ziet de status van alle objecten in de scène:
    * Totaal - het totale aantal actieve, niet-verborgen objecten in de scène.
    * Genegeerd - het aantal objecten die niet net Renderers of terreinen zijn.
    * NET - het aantal NET-Renderer objecten in de scène
    * Terrein - het aantal terrein objecten in de scène
    * Geometrie - het aantal net of terrein objecten in de scène gemarkeerd als "Akoestische geometrie"
    * Navigatie - het aantal Mesh of terrein objecten in de scène gemarkeerd als 'Akoestische navigatie'. Dit nummer bevat geen NavMesh van Unity.
5. Geeft het totale aantal 'kunnen markeren' objecten in de scène, dit alleen NET Renderers en terreinen is. Geeft de selectievakjes die u gebruiken kunt om te markeren (het juiste onderdeel toevoegen) die objecten als geometrie of navigatie voor akoestische
6. Als er niets is geselecteerd, wordt deze opmerking herinnert u om objecten te selecteren voor het markeren van indien nodig. U kunt ook controleren op een of beide selectievakjes om alle objecten in de scène markeren zonder iets te selecteren.
7. Wanneer u objecten zijn geselecteerd, ziet de status van alleen de geselecteerde objecten.
8. Geeft het totale aantal 'kunnen markeren' geselecteerde objecten. Controleren of de selectievakjes uitschakelt wordt markeren of markering opheffen van alleen de geselecteerde objecten.

Als sommige objecten zijn gemarkeerd en sommige zijn niet, wordt een "gemengde" waarde weergegeven in het bijbehorende selectievakje:

![Selectievakje met gemengde waarde](media/MixedObjectSelectionDetail.png)

Alle objecten zijn gemarkeerd te klikken op het selectievakje forceert en te klikken op opnieuw alle objecten wordt markering.

Objecten kunnen worden gemarkeerd voor zowel geometry en navigatie.

### <a name="guidelines-for-marking-objects"></a>Richtlijnen voor het markeren van objecten

Zorg ervoor dat u alle objecten markeren als **akoestische geometrie** als ze occlude moeten, weerspiegelen, of geluid absorberen. Akoestische geometry kan omvatten zaken zoals volledig is ontwikkeld, wanden daken, windows & glazen venster, tapijt en grote meubels. Het is goed dat kleinere objecten bevatten zoals lichten, decoratieve elementen, verlichtingsarmaturen, als ze niet de bake kosten aanzienlijk verbeteren. Het is belangrijk niet missen belangrijke elementen zoals het begin of een maximum. Daarnaast bevatten geen zaken die de akoestische, zoals het aantal conflicten netten geen gevolgen mag hebben.

De transformatie van een object op het moment van de berekening van de test (via de **tests** tabblad hieronder) in de resultaten bake is opgelost. Een van de gemarkeerde objecten in de scène wordt verplaatst, moet de berekening van de test opnieuw uitvoeren en de scène rebaking.

## <a name="materials-tab"></a>Tabblad materiaal

Nadat uw objecten zijn gemarkeerd, klikt u op de **materiaal** knop om te gaan naar het tabblad materialen.

### <a name="parts-of-the-materials-tab"></a>Onderdelen van het tabblad materiaal

![Details van het tabblad materiaal](media/MaterialsTabDetail.png)

1. De **materiaal** tabblad knop, die wordt gebruikt om deze pagina.
2. Een korte beschrijving van wat u moet doen met behulp van deze pagina.
3. Als dit selectievakje inschakelt, wordt alleen materiaal dat wordt gebruikt door objecten gemarkeerd als **akoestische geometrie** wordt weergegeven. Anders wordt al het materiaal gebruikt in de scène worden weergegeven.
4. Gebruik deze opties om te wijzigen van de volgorde van de vervolgkeuzelijst die wordt weergegeven wanneer u een vervolgkeuzelijst in de kolom akoestische hieronder (6). **Naam** het akoestisch materiaal sorteren op naam. "Absorptivity" in de volgorde van absorptivity van laag naar hoog gesorteerd.
5. De lijst van materiaal dat wordt gebruikt in de scène, alfabetisch gesorteerd. Als de **gemarkeerd alleen weergeven** selectievakje is ingeschakeld (#3), alleen materialen die worden gebruikt door de objecten die zijn gemarkeerd als **akoestische geometrie** worden weergegeven. Op een materiaal hier te klikken om alle objecten in de scène die gebruikmaken van dit materiaal te selecteren.
6. Geeft het akoestisch materiaal dat het scène-materiaal aan is toegewezen. Klik op een vervolgkeuzelijst als u wilt toewijzen van een scène materiaal op een andere akoestische materiaal. Kunt u de sorteervolgorde van het menu dat wordt weergegeven wanneer u klikt op een item volgen met behulp van de **akoestische sorteren op:** opties hierboven (4).
7. De coëfficiënt akoestische opname van het materiaal dat is geselecteerd in de vorige kolom bevat. Een waarde van nul betekent precies bij reflectieve (geen opname), terwijl een waarde van 1 betekent precies absorptie (geen weerspiegeling). De coëfficiënt voor de opname kan niet worden gewijzigd, tenzij het geselecteerde materiaal is ingesteld op 'Aangepast'.
8. Voor een materiaal dat is toegewezen aan 'Aangepaste', de schuifregelaar is niet meer uitgeschakeld en u kunt ervoor kiezen de opname coëfficiënt met behulp van de schuifregelaar of typ een waarde in. Zie voor meer informatie over materiaaleigenschappen [overzicht van het ontwerp voor akoestische](design-process.md).

### <a name="guidelines-for-assigning-materials-or-absorption-values"></a>Richtlijnen voor het toewijzen van materialen (of een opname waarden)

Op dit tabblad probeert te achterhalen wat de opname van het materiaal dat is gebaseerd op de naam. Bijvoorbeeld, als de naam van het materiaal scène LivingRoom_WoodTable is, is het eerste akoestische materiaal dat is toegewezen aan het "hout". Materiaal waar een bekende materiaal kan niet worden bepaald, het materiaal 'Standaard', waarvoor opname die vergelijkbaar is met concreet worden toegewezen.

U kunt het scène-materiaal akoestische materialen opnieuw toewijzen. Bijvoorbeeld als een kamer geluiden te reverberant, wijzigt u het akoestisch materiaal van de wanden, basis of maximum in een van de hogere absorptivity. De akoestische materiaal toewijzing is van toepassing op alle objecten die gebruikmaken van dit materiaal scène.

## <a name="probes-tab"></a>Tabblad tests

Na het toewijzen van het materiaal, Ga naar de **tests** tabblad.

### <a name="parts-of-the-probes-tab"></a>Onderdelen van het tabblad tests

![Details van het tabblad tests](media/ProbesTabDetail.png)

1. De **tests** tabblad-knop die wordt gebruikt om deze pagina
2. Een korte beschrijving van wat u moet doen met behulp van deze pagina
3. Gebruik deze om een oplossing grof of fijn simulatie. Grof is sneller, maar heeft bepaalde compromissen. Zie ["grof vs fijn resolutie"](#Coarse-vs-Fine-Resolution) hieronder voor meer informatie.
4. Kies de locatie waar de gegevensbestanden akoestische moeten worden geplaatst met behulp van dit veld. Klik op de knop met '...' voor het gebruik van een map kiezen. De standaardwaarde is **activa/AcousticsData**. Een **Editor** submap ook worden gemaakt onder deze locatie. Zie voor meer informatie over gegevensbestanden ["Gegevensbestanden"](#Data-Files) hieronder.
5. De gegevensbestanden voor deze scène wordt de naam hier met behulp van het voorvoegsel dat is opgegeven. De standaardwaarde is 'Acoustics_ [scènenaam van de]'.
6. Nadat de tests zijn berekend, kunt u de bovenstaande besturingselementen wordt uitgeschakeld. Klik op de **wissen** om te wissen van de berekeningen en de besturingselementen inschakelen zodat u kunt opnieuw berekenen met behulp van de nieuwe instellingen.
7. Klik op de **berekenen...**  knop voxelize de scène en de test-distributiepuntlocaties te berekenen. Dit wordt dan niet lokaal op uw computer en moet worden uitgevoerd vóór het uitvoeren van een bake.

In deze versie van het Project akoestische tests handmatig kan niet worden geplaatst en moeten worden geplaatst door het geautomatiseerde proces dat is opgegeven de **tests** tabblad.

### <a name="what-the-calculate-button-calculates"></a>Wat de knop 'Berekenen...' wordt berekend

De **berekenen...**  knop neemt alle gegevens die u tot nu toe hebt opgegeven (geometrie, navigatie, materialen en de instelling grof/fijn) en verschillende stappen doorlopen:

1. Het duurt de geometrie van het scène-netten en een volume voxel berekent. Het volume voxel is een 3-dimensionale volume die uw gehele scène omsluit en bestaat uit kleine kubieke 'voxels'. De grootte van de voxels wordt bepaald door de frequentie simulatie, die is ingesteld door de **simulatie resolutie** instelling. Elke voxel is gemarkeerd als een 'geopend air' of scène geometry bevatten. Als een voxel geometrie bevat wordt de voxel gelabeld met de coëfficiënt voor opname van het materiaal dat is toegewezen aan die geometrie.
2. Vervolgens wordt de navigatie-gegevens voor het berekenen van acoustically interessante locaties waar de speler kunt gaan. Probeert het vinden van een redelijk klein aantal deze locaties met kleinere gebieden zoals inbegrepen en elkaar tegenkwamen, en vervolgens naar ruimten spaties openen. Voor kleine schermen doorgaans is dit minder dan 100 locaties, terwijl grote schermen kunnen maximaal duizend hebben.
3. Voor elk van de laatste listener locaties berekent, bepaalt dat een aantal parameters, zoals hoe 'geopend' is de ruimte, de grootte van de ruimte die deel uitmaakt, enzovoort.
4. De resultaten van deze berekeningen worden opgeslagen in bestanden op de locatie die u opgeeft (Zie ["Gegevensbestanden"](#Data-Files) hieronder)

Deze berekeningen kunnen enkele minuten duren, afhankelijk van de grootte van uw scène en de snelheid van uw computer.

Zodra deze berekeningen voltooid zijn, kunt u zowel de voxel-gegevens en de test-distributiepuntlocaties om ervoor te zorgen dat de bake u goede resultaten krijgt bekijken. Een ongeldige navigatie net zoals of ontbrekende/extra geometrie doorgaans snel zichtbaar zijn in de Preview-versie, zodat u deze kunt oplossen.

### <a name="scene-rename"></a>Scène wijzigen

Naam van de scène wordt gebruikt om de verbinding van de scène met opslaan van de test punt plaatsing en voxelization-bestanden. Als de naam van de scène is gewijzigd nadat de test-punten worden berekend, is het materiaal en plaatsing toewijzing gegevens verloren en moet opnieuw worden gestart.

## <a name="debug-display-through-gizmos"></a>Fouten opsporen in weergeven via Gizmos

Standaard, zowel de **tests** en **Voxels** gizmos zijn ingeschakeld. Deze ziet u dat de voxels en test wijst locaties die zijn berekend. Ze kunnen worden ingeschakeld of uitgeschakeld met behulp van het menu Gizmos:

![Gizmos Menu](media/GizmosMenu.png)

### <a name="voxels"></a>Voxels

Voxels worden weergegeven in het scène-venster als groen kubussen rond deelnemende geometry. Voxels die alleen lucht bevatten, worden niet weergegeven. Er is een grote groene vakje rond uw gehele scène die het volledige voxel volume dat wordt gebruikt in de simulatie wordt aangeduid.
Een scène verplaatsen en controleer of alles wat u de geometrie moet voxels heeft. De camera van het scène heeft binnen ongeveer 5 meters van het object voor de voxels om weer te geven.

Als u de voxels die zijn gemaakt met behulp van abrupte resolutie vs fijn resolutie vergelijkt, ziet u dat de grof voxels tweemaal zo groot zijn.

![Voxel-Preview](media/VoxelCubesPreview.png)

### <a name="probe-points"></a>Test-punten

Test punten zijn gelijk aan mogelijke player (listener)-locaties. Wanneer u een bake uitvoert, wordt de akoestische voor een goede bron overal in de scène voor elk van deze test punten berekend. Als de speler niet rechtstreeks op de locatie van een test, worden de gegevens van de test-punten die het dichtst bij de speler gebruikt geïnterpoleerd van de parameters op die locatie.

Daarom is het belangrijk om ervoor te zorgen dat test punten overal bestaan de speler wordt verwacht dat in de scène reizen en kleine gebieden en pictogrammen kunt u goed worden weergegeven. Test-punten kunnen niet worden geplaatst door de engine voor het Project akoestische op basis van de interpretatie van van uw scène geometrie en worden verplaatst of bewerkt in deze Preview Designer. In plaats daarvan gebruiken ze om te controleren of de juistheid van uw geometrie-markering en de gegevens van de hint navigatie.

![Tests-Preview](media/ProbesPreview.png)

### <a name="Coarse-vs-Fine-Resolution"></a>Fijn Grof Visual Studio-oplossing

Het enige verschil tussen de resolutie-instellingen grof en fijn is de frequentie waarmee de simulatie wordt uitgevoerd. Fijn maakt gebruik van een frequentie twee keer zo hoog als grof.
Terwijl dit misschien eenvoudig lijken, heeft een aantal gevolgen voor de akoestische simulatie:

* De golflengte voor grof is twee keer lang zo goed, en daarom de voxels tweemaal zo groot zijn.
* De tijd van de simulatie is direct gerelateerd aan de voxel grootte, waardoor een grof maken van ongeveer 16 keer sneller dan een prima bake.
* Portals (bijvoorbeeld deuren of windows) kleiner is dan de grootte van de voxel niet kunnen worden gesimuleerd. De grof instelling kan ertoe leiden dat sommige van deze kleinere portals voor het niet worden gesimuleerd; Daarom geeft ze niet geluid via tijdens runtime. U kunt zien als dit gebeurt door het voxels weer te geven.
* De frequentie van de lagere simulatie resulteert in minder diffraction rond hoeken en randen.
* Geluid bronnen kunnen zich niet in "gevuld" voxels die voxels die geometry bevatten: dit resulteert in geen geluid. Het is moeilijker te vinden van geluid bronnen, zodat ze niet binnen de grotere voxels van grof zijn dan de instelling van de fijn wordt gebruikt.
* De grotere voxels wordt inbreuk maken meer bij portals, zoals hieronder wordt weergegeven. De eerste afbeelding is gemaakt met behulp van grof, terwijl de tweede is de dezelfde poort met fijn resolutie. Zoals aangegeven door de rode markeringen, moet u er veel minder inbreuk in de poort die met behulp van de instelling van de orde is. De blauwe lijn is de poort, zoals gedefinieerd door de geometrie, terwijl de rode lijn de effectieve akoestische portal gedefinieerd door de grootte van de voxel wordt. Hoe deze indringers wordt afgespeeld uit in een bepaalde situatie is afhankelijk van de volledig op hoe de voxels uitgelijnd met de geometrie van de portal, die wordt bepaald door de grootte en de locaties van de objecten in de scène.

![Grof poort](media/CoarseVoxelDoorway.png)

![Fijn poort](media/FineVoxelDoorway.png)

## <a name="bake-tab"></a>Tabblad maken

Zodra u tevreden met de preview-gegevens bent, gebruikt u de **verdient** tabblad om in te dienen uw bake naar de cloud.

### <a name="parts-of-the-bake-tab"></a>Onderdelen van het tabblad bake

![Details van het tabblad maken](media/BakeTabDetails.png)

1. De knop Tab verdient is gebruikt om deze pagina.
2. Een korte beschrijving van wat te doen op deze pagina.
3. Velden in te voeren van uw Azure-referenties wanneer uw Azure-account is gemaakt. Zie voor meer informatie, [Azure Batch-Account maken](create-azure-account.md).
4. Azure batch compute-knooppunt dat moet worden gebruikt voor de berekening. Het knooppunttype moet worden ondersteund door de locatie van uw Azure data center. Als niet weet, laat u op **Standard_F8**.
5. Het aantal knooppunten moet worden gebruikt voor deze berekening. Het getal dat u hier opgeeft, is van invloed op de tijd voor het voltooien van de bake en wordt beperkt door de toewijzing van uw Azure Batch-core. De standaard-toewijzing alleen kunt u twee 8-core knooppunten of het knooppunt van een 16-core, maar kan worden uitgebreid. Zie voor meer informatie over core toewijzing beperkingen [Azure Batch-Account maken](create-azure-account.md).
6. Het aantal test voor uw scène berekend op de **tests** tabblad. Het aantal tests bepaalt het aantal simulaties die moeten worden uitgevoerd in de cloud. U kunt meer knooppunten dan er tests zijn niet opgeven.
7. De hoeveelheid tijd die is het waarschijnlijk moet worden uitgevoerd voor de taak wordt uitgevoerd in de cloud. Dit omvat geen opstarttijd knooppunt. Zodra de taak uitgevoerd wordt, is dit over hoe lang moet zijn voordat u de resultaten teruggaan. Houd er rekening mee dat dit slechts een schatting te maken is.
8. De totale hoeveelheid computertijd die nodig zijn voor de simulaties uitvoeren. Dit is de totale hoeveelheid tijd knooppunt dat wordt gebruikt in Azure. Zie [Estimating bake kosten](#Estimating-bake-cost) hieronder voor meer informatie over het gebruik van deze waarde.
9. Dit bericht geeft aan waar de resultaten van de bake wordt opgeslagen als de taak is voltooid.
10. (Alleen bij Geavanceerd gebruik) Als u om een bepaalde reden die u nodig hebt om af te dwingen van Unity tot te maken over een bake verzonden (bijv. u hebt gedownload de resultaten met behulp van een andere computer), klikt u op de **status wissen** knop om u te maken over de taak die is ingediend. Houd er rekening mee dat dit betekent dat het bestand met resultaten, wanneer u klaar bent, wordt **niet** worden gedownload, en **dit is niet hetzelfde als de taak annuleren**. De taak blijft als actief is, om uit te voeren in de cloud.
11. Klik op de knop Bake om in te dienen de bake naar de cloud. Terwijl een taak wordt uitgevoerd, geeft dit **taak annuleren** in plaats daarvan.
12. Dit gebied toont de status van de bake. Wanneer dit is voltooid, moet overeenkomen met **gedownloade**.

U krijgt altijd volledige informatie over actieve taken, pools compute en opslag op de [Azure Portal](https://portal.azure.com).

Terwijl een taak wordt uitgevoerd de **verdient** knop verandert **taak annuleren**. Gebruik deze knop om te annuleren van de taak wordt uitgevoerd. U wordt gevraagd te bevestigen voordat de taak is geannuleerd. Annuleren van een taak kan niet ongedaan worden gemaakt, er zijn geen resultaten zijn beschikbaar en u nog steeds betalen voor de gebruikte tijd voor alle Azure-rekenen.

Nadat u een bake hebt gestart, kunt u de Unity kunt sluiten. Afhankelijk van het project, knooppunttype en het aantal knooppunten, kan een cloud-bake enkele uren duren. De taakstatus bake wordt bijgewerkt wanneer u het project opnieuw laden en het venster akoestische open. Als de taak is voltooid, wordt het uitvoerbestand worden gedownload.

De Azure-referenties zijn veilig opgeslagen op uw lokale computer en die zijn gekoppeld aan uw Unity-editor. Ze worden gebruikt uitsluitend tot stand brengen van een beveiligde verbinding met Azure.

### <a name="Estimating-bake-cost"></a> Bake kosten schatten

Voor een schatting van wat een bepaalde bake kost, nemen de waarde die wordt weergegeven voor **geschatte kosten voor Compute**, dit is een duur en vermenigvuldigen die door het elk uur kosten in uw lokale valuta van de **VM knooppunttype** u hebt geselecteerd. Het resultaat bevat niet de knooppunt tijd die nodig zijn aan de slag van de knooppunten en wordt uitgevoerd. Als u bijvoorbeeld **Standard_F8** voor uw knooppunttype heeft een prijs van $ 0,75/uur, en de geschatte kosten voor Compute 3 uur en 57 minuten, wordt de geschatte kosten voor het uitvoeren van de taak is $0,75 * ~ 4 uur = ~ $3,00. De werkelijke kosten wordt waarschijnlijk een enigszins hoger vanwege de extra tijd om op te halen van de knooppunten gestart. U vindt het uurtarief knooppunt kosten op de [Azure Batch-prijzen](https://azure.microsoft.com/pricing/details/virtual-machines/linux) pagina (Selecteer 'Compute geoptimaliseerd' of 'High performance computing' voor de categorie).

### <a name="reviewing-the-bake-results"></a>De bake resultaten te bekijken

Nadat de bake is voltooid, moet u controleren of de punten voxels en test in de verwachte locaties zijn door het uitvoeren van de runtime-invoegtoepassing. Meer informatie vindt u in [overzicht van het ontwerp voor akoestische](design-process.md).

## <a name="Data-Files"></a>Gegevensbestanden

Er zijn vier gegevensbestanden die zijn gemaakt door deze invoegtoepassing op verschillende tijdstippen. Slechts één van deze is nodig tijdens runtime, de andere drie zijn daarom in mappen met de naam 'Editor', zodat ze in uw project wordt niet worden gecompileerd.

* **Activa-Editor / / [SceneName]\_AcousticsParameters.asset**: dit bestand slaat de gegevens die u in de velden in de gebruikersinterface van akoestische invoeren. De locatie en naam van dit bestand kunnen niet worden gewijzigd. Er zijn andere waarden die zijn opgeslagen in dit bestand die invloed hebben op de bake, maar ze zijn bedoeld voor ervaren gebruikers en mag niet worden gewijzigd.
* **Activa/AcousticsData/akoestische\_[SceneName].ace.bytes**: dit bestand is wat wordt gemaakt tijdens de simulatie bake en bevat de opzoekgegevens gebruikt door de runtime voor de akoestische van uw scène renderen. De locatie en naam van dit bestand kunnen worden gewijzigd met de velden op de **tests** tabblad.
* **Assets/AcousticsData/Editor/Acoustics_[SceneName].vox**: dit bestand wordt opgeslagen de voxelized akoestische geometrie en eigenschappen van het materiaal. Berekend op basis van de **berekenen...**  knop op het tabblad tests. De locatie en naam van dit bestand kunnen worden gewijzigd met de velden op de **tests** tabblad.
* **Activa/AcousticsData /-Editor/akoestische\_[SceneName]\_config.xml**: dit bestand wordt opgeslagen parameters berekend op basis van de **berekenen...**  knop op de **tests** tabblad. De locatie en naam van dit bestand kunnen worden gewijzigd met de velden op de **tests** tabblad.

Let erop dat niet als u wilt verwijderen de *. ace.bytes bestand gedownload uit de bake. Dit bestand is niet hersteld, met uitzondering van de scène rebaking.

## <a name="next-steps"></a>Volgende stappen
* Bake resultaten toepassen op geluid bronnen in [overzicht van het ontwerp voor akoestische](design-process.md).

