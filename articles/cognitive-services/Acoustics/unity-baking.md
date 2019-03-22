---
title: Project akoestische Unity Bake zelfstudie
titlesuffix: Azure Cognitive Services
description: Deze zelfstudie beschrijft akoestische bakken met akoestische in Unity-Project.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: kegodin
ms.openlocfilehash: f44b6f9ed42770fe830346de08058e33ed68a249
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58309637"
---
# <a name="project-acoustics-unity-bake-tutorial"></a>Project akoestische Unity Bake zelfstudie
Deze zelfstudie beschrijft akoestische bakken met akoestische in Unity-Project.

Softwarevereisten:
* [Unity 2018.2 +](http://unity3d.com) voor Windows
* [Project akoestische invoegtoepassing geïntegreerd in uw Unity-project](unity-integration.md) of de [Project akoestische Unity voorbeelden voor inhoud](unity-quickstart.md)
* Optioneel: Een [Azure Batch-account](create-azure-account.md) te versnellen gebouwd met behulp van cloud computing

## <a name="open-the-project-acoustics-bake-window"></a>Open het Project akoestische verdient venster
Kies **venster > akoestische** in het menu Unity:

![Schermafbeelding van Unity-editor met de menu-optie voor akoestische venster is gemarkeerd](media/window-acoustics.png)

## <a name="create-a-navigation-mesh"></a>Een navigatie-net maken
Project akoestische maakt gebruik van een navigatie-NET listener test punten voor simulatie plaatsen. U kunt de Unity [navigatie mesh werkstroom](https://docs.unity3d.com/Manual/nav-BuildingNavMesh.html), of gebruik een ander pakket van de 3D-modellen voor het ontwerpen van uw eigen mesh. 

## <a name="mark-acoustic-scene-objects"></a>Akoestische scène objecten markeren
Project akoestische vertrouwt op twee soorten objecten voor de simulatie scène: de objecten die wordt weergegeven en occlude geluid in de simulatie en de speler navigatie mesh die listener test punten in de simulatie wordt beperkt. Beide objecttypen zijn gemarkeerd met behulp van de **objecten** tabblad. 

Omdat alleen objecten markeren voegt de **AcousticsGeometry** of **AcousticsNavigation** onderdelen op het object, u kunt ook de [standaard Unity onderdeel werkstroom](https://docs.unity3d.com/Manual/UsingComponents.html)te markeren of markering opheffen objecten. Alleen NET Renderers en terreinen kunnen worden gemarkeerd. Alle objecttypen die worden genegeerd. De selectievakjes wordt markeren of markering opheffen van alle betrokken objecten.

### <a name="mark-acoustic-occlusion-and-reflection-geometry"></a>Akoestische bedekking en reflectie geometrie markeren
Open de **objecten** tabblad van de **akoestische** venster. Alle objecten markeren als **akoestische geometrie** als ze occlude moeten, weerspiegelen, of geluid absorberen. Akoestische geometry kan omvatten zaken zoals volledig is ontwikkeld, wanden daken, windows & glazen venster, tapijt en grote meubels. U kunt elke willekeurige niveau van complexiteit voor deze objecten gebruiken. Omdat de scène voxelized voordat de simulatie is, zijn zeer gedetailleerde netten, zoals structuren met veel kleine bladeren, niet meer kostbaar zijn om u te maken dan vereenvoudigde objecten.

Dingen die al dan niet mogen invloed hebben op de akoestische, zoals onzichtbaar botsing netten zijn niet opgenomen.

De transformatie van een object op het moment van de berekening van de test (via de **tests** tabblad hieronder) in de resultaten bake is opgelost. Een van de gemarkeerde objecten in de scène wordt verplaatst, moet de berekening van de test opnieuw uitvoeren en de scène rebaking.

### <a name="mark-the-navigation-mesh"></a>De navigatie-NET is ingeschakeld
Navigatie-netten die zijn gemaakt met de Unity-werkstroom wordt door het systeem akoestische worden opgehaald. Voor het gebruik van uw eigen netten, markeer ze uit de **objecten** tabblad.

### <a name="for-reference-the-objects-tab-parts"></a>Ter referentie: De objecten tabblad delen
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

Als u er niets is geselecteerd in de scène hebt, ziet het tabblad objecten, zoals in de volgende afbeelding:

![Schermafbeelding van akoestische objecten tabblad met geen selectie](media/objects-tab-no-selection-detail.png)

Als u iets hebt geselecteerd in het venster scène of hiërarchie hebt, kunt u het eruitziet zoals in de volgende afbeelding:

![Schermafbeelding van akoestische objecten tabblad met de selectie wordt weergegeven](media/objects-tab-selection-detail.png)

Als sommige objecten zijn gemarkeerd en sommige zijn niet, wordt een "gemengde" waarde weergegeven in het bijbehorende selectievakje:

![Schermafbeelding van akoestische objecten tabblad met gemengde selectie pictogram gemarkeerd](media/mixed-object-selection-detail.png)

Alle objecten zijn gemarkeerd te klikken op het selectievakje forceert en te klikken op opnieuw alle objecten wordt markering.

Objecten kunnen worden gemarkeerd voor zowel geometry en navigatie.

## <a name="select-acoustic-materials"></a>Akoestische materiaal selecteren
Nadat uw objecten zijn gemarkeerd, klikt u op de **materiaal** knop en akoestische materiaal toe te wijzen. Het Project akoestische materiaal-systeem is gekoppeld aan het systeem van de visual materiaal Unity: twee objecten afzonderlijk akoestische materialen hebben ze afzonderlijke visual materiaal moeten hebben.

Het akoestisch materiaal beheren geluid energieverbruik weergegeven van elke surface. Het standaard akoestische materiaal is vergelijkbaar met concreet opname. Project akoestische stelt materialen op basis van de naam van het visuele element materiaal. U kunt het akoestisch materiaal 'Aangepaste' toewijzen aan een materiaal om in te schakelen van een opname coëfficiënt schuifregelaar.

De tijd weerklank van een bepaald materiaal in een kamer is omgekeerd gerelateerd aan de coëfficiënt opname met de meeste materiaal met waarden voor opname in het bereik 0,01-0.20. Materialen met opname coëfficiënten buiten dit bereik zijn zeer absorberend.

![Grafiek met negatieve correlatie weerklank tijd met opname coëfficiënt](media/reverb-time-graph.png)

### <a name="for-reference-parts-of-the-materials-tab"></a>Ter referentie: Onderdelen van het tabblad materiaal
![Schermafbeelding van akoestische materialen tabblad in Unity](media/materials-tab-detail.png)

1. De **materiaal** tabblad knop, die wordt gebruikt om deze pagina.
2. Een korte beschrijving van wat u moet doen met behulp van deze pagina.
3. Als dit selectievakje inschakelt, wordt alleen materiaal dat wordt gebruikt door objecten gemarkeerd als **akoestische geometrie** wordt weergegeven. Anders wordt al het materiaal gebruikt in de scène worden weergegeven.
4. Gebruik deze opties om te wijzigen van de volgorde van de vervolgkeuzelijst die wordt weergegeven wanneer u een vervolgkeuzelijst in de kolom akoestische hieronder (6). **Naam** het akoestisch materiaal sorteren op naam. "Absorptivity" in de volgorde van absorptivity van laag naar hoog gesorteerd.
5. De lijst van materiaal dat wordt gebruikt in de scène, alfabetisch gesorteerd. Als de **gemarkeerd alleen weergeven** selectievakje is ingeschakeld (#3), alleen materialen die worden gebruikt door de objecten die zijn gemarkeerd als **akoestische geometrie** worden weergegeven. Op een materiaal hier te klikken om alle objecten in de scène die gebruikmaken van dit materiaal te selecteren.
6. Geeft het akoestisch materiaal dat het scène-materiaal aan is toegewezen. Klik op een vervolgkeuzelijst als u wilt toewijzen van een scène materiaal op een andere akoestische materiaal. Kunt u de sorteervolgorde van het menu dat wordt weergegeven wanneer u klikt op een item volgen met behulp van de **akoestische sorteren op:** opties hierboven (4).
7. De coëfficiënt akoestische opname van het materiaal dat is geselecteerd in de vorige kolom bevat. Een waarde van nul betekent precies bij reflectieve (geen opname), terwijl een waarde van 1 betekent precies absorptie (geen weerspiegeling). De coëfficiënt voor de opname kan niet worden gewijzigd, tenzij het geselecteerde materiaal is ingesteld op 'Aangepast'.
8. Voor een materiaal dat is toegewezen aan 'Aangepaste', de schuifregelaar is niet meer uitgeschakeld en u kunt ervoor kiezen de opname coëfficiënt met behulp van de schuifregelaar of typ een waarde in.

## <a name="calculate-and-review-listener-probe-locations"></a>Berekenen en listener test locaties bekijken
Na het toewijzen van het materiaal, Ga naar de **tests** tabblad en klik op **berekenen** listener test punten voor simulatie plaatsen.

### <a name="what-the-calculate-button-calculates"></a>Wat de knop 'Berekenen...' wordt berekend
De **berekenen...**  knop maakt gebruik van uw geselecteerde akoestische scène geometrie uw scène voorbereiden voor simulatie:

1. Het duurt de geometrie van het scène-netten en een volume voxel berekent. Het volume voxel is een 3-dimensionale volume die uw gehele scène omsluit en bestaat uit kleine kubieke 'voxels'. De grootte van de voxels wordt bepaald door de frequentie simulatie, die is ingesteld door de **simulatie resolutie** instelling. Elke voxel is gemarkeerd als een 'geopend air' of scène geometry bevatten. Als een voxel geometrie bevat wordt de voxel gelabeld met de coëfficiënt voor opname van het materiaal dat is toegewezen aan die geometrie.
2. Het maakt gebruik van de navigatie-mesh(es) listener test punten plaatsen. Het algoritme geeft een balans tussen de concurrerende problemen van ruimtelijke dekking en simulatie tijd en de bestandsgrootte, en tegelijkertijd het die het beperken van gangen en altijd een bepaalde hoeveelheid dekking kleine spaties. Typische test punt telt bereik van 100 voor kleine schermen tot een paar duizend voor grote schermen.

Deze berekeningen kunnen enkele minuten duren, afhankelijk van de grootte van uw scène en de snelheid van uw computer.

### <a name="review-voxel-and-probe-placement"></a>Controleer voxel en test de plaatsing
Zowel de voxel-gegevens en de locaties van de punt test om te controleren of dat u bent klaar om u te maken van uw scène bekijken. Een onvolledige navigatie mesh of ontbreekt of is extra akoestische geometrie meestal snel zichtbaar zijn in de Preview-versie. Voxel en test de plaatsing kan worden ingeschakeld of uitgeschakeld met behulp van het menu Gizmos:

![Schermafbeelding van Gizmos in het menu in Unity](media/gizmos-menu.png)

Voxels akoestische geometrie die worden weergegeven als groen kubussen. Verken uw scène en controleer of alles wat u de geometrie moet voxels heeft. De camera van het scène heeft binnen ongeveer 5 meters van het object voor de voxels om weer te geven.

Als u de voxels die zijn gemaakt met behulp van abrupte resolutie vs fijn resolutie vergelijkt, ziet u dat de grof voxels tweemaal zo groot zijn.

![Schermafbeelding van grove voxels preview in de editor voor Unity](media/voxel-cubes-preview.png)

Simulatieresultaten worden geïnterpoleerd tussen listener test distributiepuntlocaties tijdens runtime. Controleer de punten van de test in de buurt van waar dat de speler wordt verwacht dat in de scène worden uitgewisseld.

![Schermafbeelding van de tests preview in de editor voor Unity](media/probes-preview.png)

### <a name="take-care-with-scene-renames"></a>Let erop dat met de naam van de scène
Naam van de scène wordt gebruikt om de verbinding van de scène met opslaan van de test punt plaatsing en voxelization-bestanden. Als de naam van de scène is gewijzigd nadat de test-punten worden berekend, is het materiaal en plaatsing toewijzing gegevens verloren en moet opnieuw worden gestart.

### <a name="for-reference-parts-of-the-probes-tab"></a>Ter referentie: Onderdelen van het tabblad tests
![Schermafbeelding van akoestische tests tabblad in Unity](media/probes-tab-detail.png)

1. De **tests** tabblad-knop die wordt gebruikt om deze pagina
2. Een korte beschrijving van wat u moet doen met behulp van deze pagina
3. Gebruik deze om een oplossing grof of fijn simulatie. Grof is sneller, maar heeft bepaalde compromissen. Zie [kiezen grof vs prima oplossing](#Coarse-vs-Fine-Resolution) hieronder voor meer informatie.
4. Kies de locatie waar de gegevensbestanden akoestische moeten worden geplaatst met behulp van dit veld. Klik op de knop met '...' voor het gebruik van een map kiezen. De standaardwaarde is **activa/AcousticsData**. Een **Editor** submap ook worden gemaakt onder deze locatie. Zie voor meer informatie over gegevensbestanden [gegevensbestanden](#Data-Files) hieronder.
5. De gegevensbestanden voor deze scène wordt de naam hier met behulp van het voorvoegsel dat is opgegeven. De standaardwaarde is 'Acoustics_ [scènenaam van de]'.
6. Nadat de tests zijn berekend, kunt u de bovenstaande besturingselementen wordt uitgeschakeld. Klik op de **wissen** om te wissen van de berekeningen en de besturingselementen inschakelen zodat u kunt opnieuw berekenen met behulp van de nieuwe instellingen.
7. Klik op de **berekenen...**  knop voxelize de scène en de test-distributiepuntlocaties te berekenen. Dit wordt dan niet lokaal op uw computer en moet worden uitgevoerd vóór het uitvoeren van een bake.

In deze versie van het Project akoestische tests handmatig kan niet worden geplaatst en moeten worden geplaatst door het geautomatiseerde proces dat is opgegeven de **tests** tabblad.

### <a name="Coarse-vs-Fine-Resolution"></a>Grof versus prima oplossing kiezen

Het enige verschil tussen de instellingen voor het omzetten van grove en goed is de frequentie waarmee de simulatie wordt uitgevoerd. Fijn maakt gebruik van een twee keer zo hoog als grof frequentie.
Terwijl dit misschien eenvoudig lijken, heeft een aantal gevolgen voor de akoestische simulatie:

* De golflengte voor grof twee keer zo lang als fijn is, en daarom de voxels tweemaal zo groot zijn.
* De tijd van de simulatie is direct gerelateerd aan de voxel grootte, waardoor een grof bake ongeveer 16 keer sneller dan een prima bake.
* Portals (bijvoorbeeld deuren of windows) kleiner is dan de grootte van de voxel niet kunnen worden gesimuleerd. De grof instelling kan ertoe leiden dat sommige van deze kleinere portals voor het niet worden gesimuleerd; Daarom geeft ze niet geluid via tijdens runtime. U kunt zien als dit gebeurt door het voxels weer te geven.
* De frequentie van de lagere simulatie resulteert in minder diffraction rond hoeken en randen.
* Geluid bronnen kunnen zich niet in "gevuld" voxels die voxels die geometry bevatten: dit resulteert in geen geluid. Het is moeilijker te vinden van geluid bronnen, zodat ze niet binnen de grotere voxels van grove vallen dan de instelling van de fijn wordt gebruikt.
* De grotere voxels wordt inbreuk maken meer bij portals, zoals hieronder wordt weergegeven. De eerste afbeelding is gemaakt met behulp van abrupte, terwijl de tweede is de dezelfde poort met fijn resolutie. Zoals aangegeven door de rode markeringen, moet u er veel minder inbreuk in de poort die met behulp van de instelling van de orde is. De blauwe lijn is de poort, zoals gedefinieerd door de geometrie, terwijl de rode lijn de effectieve akoestische portal gedefinieerd door de grootte van de voxel wordt. Hoe deze indringers wordt afgespeeld uit in een bepaalde situatie is afhankelijk van de volledig op hoe de voxels uitgelijnd met de geometrie van de portal, die wordt bepaald door de grootte en de locaties van de objecten in de scène.

![Schermafbeelding van grove voxels in poort](media/coarse-voxel-doorway.png)

![Schermafbeelding van fijn voxels in poort](media/fine-voxel-doorway.png)

## <a name="bake-your-scene-using-azure-batch"></a>Maken van een scène met behulp van Azure Batch
U kunt maken van een scène met een rekencluster in de cloud met behulp van de Azure Batch-service. De Project akoestische Unity-invoegtoepassing maakt rechtstreeks verbinding met Azure Batch te maken, beheren en een Azure Batch-cluster voor elke bake afbreken. Op de **verdient** tabblad, Voer uw Azure-referenties, selecteer een clustertype van de machine en de grootte en klikt u op **verdient**.

### <a name="for-reference-parts-of-the-bake-tab"></a>Ter referentie: Onderdelen van het tabblad Bake
![Schermafbeelding van akoestische verdient tabblad in Unity](media/bake-tab-details.png)

1. De knop Tab verdient is gebruikt om deze pagina.
2. Een korte beschrijving van wat te doen op deze pagina.
3. Velden in te voeren van uw Azure-referenties wanneer uw Azure-account is gemaakt. Zie voor meer informatie, [Azure Batch-Account maken](create-azure-account.md).
4. Docker-installatiekopie tag voor de toolset akoestische.
5. Start de Azure-portal voor het beheren van uw abonnementen, gebruik controleren en factureringsgegevens enzovoort weergeven. 
6. Azure batch compute-knooppunt dat moet worden gebruikt voor de berekening. Het knooppunttype moet worden ondersteund door de locatie van uw Azure data center. Als niet weet, laat u op **Standard_F8s_v2**.
7. Het aantal knooppunten moet worden gebruikt voor deze berekening. Het getal dat u hier opgeeft, is van invloed op de tijd voor het voltooien van de bake en wordt beperkt door de toewijzing van uw Azure Batch-core. De standaard-toewijzing alleen kunt u twee 8-core knooppunten of het knooppunt van een 16-core, maar kan worden uitgebreid. Zie voor meer informatie over core toewijzing beperkingen [Azure Batch-Account maken](create-azure-account.md).
8. Schakel dit selectievakje in om te configureren van uw compute-toepassingen wilt gebruiken [knooppunten met lage prioriteit](https://docs.microsoft.com/azure/batch/batch-low-pri-vms). Rekenknooppunten met lage prioriteit hebben veel lagere kosten, maar ze mogelijk niet altijd beschikbaar of op elk gewenst moment kunnen worden verschoven.
9. Het aantal test voor uw scène berekend op de **tests** tabblad. Het aantal tests bepaalt het aantal simulaties die moeten worden uitgevoerd in de cloud. U kunt meer knooppunten dan er tests zijn niet opgeven.
10. De hoeveelheid tijd die moet worden uitgevoerd voor de taak wordt uitgevoerd in de cloud naar verwachting. Dit omvat geen opstarttijd knooppunt. Zodra de taak uitgevoerd wordt, is dit over hoe lang moet zijn voordat u de resultaten teruggaan. Houd er rekening mee dat dit slechts een schatting te maken is.
11. De totale hoeveelheid computertijd die nodig zijn voor de simulaties uitvoeren. Dit is de totale hoeveelheid tijd knooppunt dat wordt gebruikt in Azure. Zie [Estimating bake kosten](#Estimating-bake-cost) hieronder voor meer informatie over het gebruik van deze waarde.
12. Dit bericht geeft aan waar de resultaten van de bake wordt opgeslagen als de taak is voltooid.
13. (Alleen bij Geavanceerd gebruik) Als u om een bepaalde reden die u nodig hebt om af te dwingen van Unity tot te maken over een bake verzonden (bijv. u hebt gedownload de resultaten met behulp van een andere computer), klikt u op de **status wissen** knop om u te maken over de taak die is ingediend. Houd er rekening mee dat dit betekent dat het bestand met resultaten, wanneer u klaar bent, wordt **niet** worden gedownload, en **dit is niet hetzelfde als de taak annuleren**. De taak blijft als actief is, om uit te voeren in de cloud.
14. Klik op de **verdient** knop om in te dienen de bake naar de cloud. Terwijl een taak wordt uitgevoerd, geeft dit **taak annuleren** in plaats daarvan.
15. Wordt voorbereid voor de verwerking van [akoestische simulatie op uw PC](#Local-bake).
16. Dit gebied toont de status van de bake. Wanneer dit is voltooid, moet overeenkomen met **gedownloade**.

U krijgt altijd volledige informatie over actieve taken, pools compute en opslag op de [Azure-portal](https://portal.azure.com).

Terwijl een taak wordt uitgevoerd de **verdient** knop verandert **taak annuleren**. Gebruik deze knop om te annuleren van de taak wordt uitgevoerd. U wordt gevraagd te bevestigen voordat de taak is geannuleerd. Annuleren van een taak kan niet ongedaan worden gemaakt, er zijn geen resultaten zijn beschikbaar en u nog steeds betalen voor de gebruikte tijd voor alle Azure-rekenen.

Nadat u een bake hebt gestart, kunt u de Unity kunt sluiten. Afhankelijk van het project, knooppunttype en het aantal knooppunten, kan een cloud-bake enkele uren duren. De taakstatus bake wordt bijgewerkt wanneer u het project opnieuw laden en het venster akoestische open. Als de taak is voltooid, wordt het uitvoerbestand worden gedownload.

De Azure-referenties zijn veilig opgeslagen op uw lokale computer en die zijn gekoppeld aan uw Unity-editor. Ze worden gebruikt uitsluitend tot stand brengen van een beveiligde verbinding met Azure.

### <a name="Estimating-bake-cost"></a> Azure bake kosten schatten

Voor een schatting van wat een bepaalde bake kost, nemen de waarde die wordt weergegeven voor **geschatte kosten voor Compute**, dit is een duur en vermenigvuldigen die door het elk uur kosten in uw lokale valuta van de **VM knooppunttype** u hebt geselecteerd. Het resultaat bevat niet de knooppunt tijd die nodig zijn aan de slag van de knooppunten en wordt uitgevoerd. Als u bijvoorbeeld **Standard_F8s_v2** voor uw knooppunttype heeft een prijs van $ 0,40/uur, en de geschatte kosten voor Compute 3 uur en 57 minuten, wordt de geschatte kosten voor het uitvoeren van de taak is $0,40 * ~ 4 uur = ~ $1,60. De werkelijke kosten wordt waarschijnlijk een enigszins hoger vanwege de extra tijd om op te halen van de knooppunten gestart. U vindt het uurtarief knooppunt kosten op de [Azure Batch-prijzen](https://azure.microsoft.com/pricing/details/virtual-machines/linux) pagina (Selecteer 'Compute geoptimaliseerd' of 'High performance computing' voor de categorie).

## <a name="Local-bake"></a> Maken van uw scène op uw PC
U kunt uw scène maken op uw eigen PC. Dit kan nuttig zijn om te experimenteren met akoestische met kleine schermen voordat het maken van een Azure Batch-account zijn. Houd er rekening mee de simulatie akoestische kan erg lang duren, afhankelijk van de grootte van de scène.

### <a name="minimum-hardware-requirements"></a>Minimale hardwarevereisten
* Een x86-64-processor met ten minste 8 kerngeheugens en 32 GB aan RAM-geheugen

Een voorbeeld: bij onze tests op een 8-core-machine met Intel Xeon E5-1660 @ 3 GHz en 32 GB RAM-
* Een kleine scène met 100 tests kan ongeveer 2 uur voor een grof bake of 32 uur voor een goed bake duren.
* Een scène middelgrote met 1000 tests duurt ongeveer 20 uur voor een grof bake of 21 dagen voor een goed bake.

### <a name="setup-docker"></a>Docker instellen
Installeren en configureren van Docker op de computer die de simulatie - verwerkt
1. Installeer de [Docker toolset](https://www.docker.com/products/docker-desktop).
2. Start de Docker-instellingen, gaat u naar de opties 'Geavanceerd' en configureren van resources op ten minste 8 GB RAM-geheugen. De meer CPU's die u kunt toewijzen aan Docker, hoe sneller de bake wordt voltooid. ![Schermopname van voorbeeld van Docker-instellingen](media/docker-settings.png)
3. Ga naar "Gedeelde stations" en delen van het station gebruikt voor de verwerking van inschakelen.![Opties voor Screnshot van Docker gedeelde stations](media/docker-shared-drives.png)

### <a name="run-local-bake"></a>Lokale bake uitvoeren
1. Klik op de knop 'Voorbereiden lokale verdient' op de **verdient** tabblad en selecteer een map waarin de invoerbestanden en de uitvoering van scripts worden opgeslagen. U kunt vervolgens de bake uitvoeren op elke computer, zolang deze voldoet aan de minimale hardwarevereisten en waarop Docker is geïnstalleerd door te kopiëren van de map op de computer.
2. Start de simulatie met behulp van het script 'runlocalbake.bat'. Dit script wordt het Project akoestische Docker-installatiekopie met de toolset die nodig zijn voor de verwerking van de simulatie ophalen en de simulatie starten. 
3. Nadat de simulatie is voltooid, moet u het resulterende .ace-bestand kopiëren naar uw Unity-project. Om ervoor te zorgen dat Unity herkent deze als een binair bestand, voegt u ".bytes" aan de bestandsextensie (bijvoorbeeld ' Scene1.ace.bytes'). De gedetailleerde logboeken voor de simulatie worden opgeslagen in "AcousticsLog.txt." Als u problemen ondervindt, kunt u dit bestand om te helpen bij diagnose delen.

## <a name="Data-Files"></a> Toegevoegd door het proces bake gegevensbestanden

Er zijn vier gegevensbestanden tijdens de bake gemaakt. Een bevat de simulatieresultaten en wordt geleverd met de titel. De andere opslaan van gegevens met betrekking tot de Unity-Editor.

Simulatieresultaat:
* **Assets/AcousticsData/Acoustics\_[SceneName].ace.bytes**: Dit is de runtime-opzoektabel en simulatieresultaten en de voxelized akoestische scène elementen bevat. De locatie en naam van dit bestand kunnen worden gewijzigd met de velden op de **tests** tabblad.

Let erop dat niet als u wilt verwijderen van de simulatie-bestand met resultaten. Het niet hersteld, met uitzondering van de scène rebaking.

Gegevensbestanden van de editor:
* **Assets/Editor/[SceneName]\_AcousticsParameters.asset**: Dit bestand slaat de gegevens die u in de velden in de gebruikersinterface van akoestische invoeren. De locatie en naam van dit bestand kunnen niet worden gewijzigd.
* **Assets/AcousticsData/Editor/Acoustics_[SceneName].vox**: Dit bestand wordt opgeslagen de voxelized akoestische geometrie en de eigenschappen van het materiaal die worden berekend met behulp van de **berekenen...**  knop op het tabblad tests. De locatie en naam van dit bestand kunnen worden gewijzigd met de velden op de **tests** tabblad.
* **Assets/AcousticsData/Editor/Acoustics\_[SceneName]\_config.xml**: Dit bestand wordt opgeslagen simulatie parameters berekend op basis van de **berekenen...**  knop op de **tests** tabblad. De locatie en naam van dit bestand kunnen worden gewijzigd met de velden op de **tests** tabblad.

## <a name="set-up-the-acoustics-lookup-table"></a>Instellen van de opzoektabel akoestische
Sleep en zet de **Project akoestische** prefab uit het deelvenster project in uw scène:

![Schermafbeelding van akoestische prefab in Unity](media/acoustics-prefab.png)

Klik op de **ProjectAcoustics** Game-Object en Ga naar het deelvenster inspector. Geef de locatie van uw bake resultaat die (de. Bestand optimaal **activa/AcousticsData**) door slepen en neerzetten in het script akoestische Manager of door te klikken op de knop cirkel naast het tekstvak.

![Schermafbeelding van de Manager van de akoestische prefab in Unity](media/acoustics-manager.png)  

## <a name="next-steps"></a>Volgende stappen
* Verken de [besturingselementen voor Unity ontwerpen](unity-workflow.md)
* Verken de [Project akoestische ontwerpconcepten](design-process.md)

