---
title: Project akoestische Unreal en Wwise-integratie
titlesuffix: Azure Cognitive Services
description: Deze procedures voor beschrijving van de integratie van het Project akoestische Unreal en Wwise invoegtoepassingen in uw project.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: how-to
ms.date: 03/20/2019
ms.author: kegodin
ms.openlocfilehash: 0baaf31386e1155dee6ca2bbfda6827ca3fc36fe
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58313444"
---
# <a name="project-acoustics-unreal-and-wwise-integration"></a>Project akoestische Unreal en Wwise-integratie
Deze instructies vindt u gedetailleerde integratiestappen van het Project akoestische invoegtoepassing-pakket in uw bestaande game Unreal en Wwise-project. 

Softwarevereisten:
* [Unreal Engine](https://www.unrealengine.com/) 4.21
* [AudioKinetic Wwise](https://www.audiokinetic.com/products/wwise/) 2018.1. +
* [Wwise-invoegtoepassing voor Unreal](https://www.audiokinetic.com/library/?source=UE4&id=index.html)
  * Als u een rechtstreekse integratie van de SDK Wwise gebruikt in plaats van de invoegtoepassingen Wwise Unreal, raadpleegt u het Project akoestische Unreal-invoegtoepassing en Wwise API-aanroepen.

Als u gebruiken van akoestische Project met een audio-engine dan Wwise wilt, contact met ons op de [Project akoestische forums](https://social.msdn.microsoft.com/Forums/en-US/home?forum=projectacoustics). De Project akoestische Unreal-invoegtoepassing kunt u akoestische gegevens op te vragen en breng vervolgens API-aanroepen naar de engine.

## <a name="download-project-acoustics"></a>Project akoestische downloaden
Als u niet hebt gedaan, downloadt u de [Project akoestische Unreal & Wwise invoegtoepassing pakket](https://www.microsoft.com/download/details.aspx?id=58090)). 

We hebben een invoegtoepassing Unreal Engine en een Wwise mixer-invoegtoepassing opgenomen in het pakket. De Unreal-invoegtoepassing biedt de editor en runtime-integratie. Tijdens de spelen berekent de Project akoestische Unreal invoegtoepassing parameters zoals bedekking voor elk object game elk frame. Deze parameters worden vertaald naar Wwise API-aanroepen.

## <a name="review-integration-steps"></a>Integratie van stappen

Er zijn deze belangrijkste stappen voor het installeren van het pakket en deze implementeren in uw game.
1. De Project akoestische Wwise mixer-invoegtoepassing installeren
2. (Opnieuw) Wwise op uw game te implementeren. Deze stap geeft de invoegtoepassing mixer in uw game project.
3. De Project akoestische Unreal-invoegtoepassing toevoegen aan uw game
4. De Wwise Unreal invoegtoepassing functionaliteit uitbreiden
5. Games bouwen en controleer dat Python is ingeschakeld
6. Uw project Wwise ingesteld voor het gebruik van Project akoestische
7. Audio-instellingen in Unreal

## <a name="1-install-the-project-acoustics-mixer-plugin"></a>1. De Project-akoestische mixer-invoegtoepassing installeren
* Wwise starten, klikt u vervolgens Open in de **invoegtoepassingen** tabblad onder **nieuwe invoegtoepassingen installeren**, selecteer **uit map toevoegen**. 

    ![Schermafbeelding van het installeren van een invoegtoepassing in Wwise starten](media/wwise-install-new-plugin.png)

* Kies de `AcousticsWwisePlugin\ProjectAcoustics` directory die is opgenomen in het pakket dat u hebt gedownload. Het bevat de bundel Wwise mixer-invoegtoepassing.

* Wwise wordt de invoegtoepassing installeren. Project akoestische moet nu weergegeven in de lijst met geïnstalleerde invoegtoepassingen in Wwise.
![Schermafbeelding van Wwise geïnstalleerd Plug-ins lijst na de installatie van Project akoestische](media/unreal-integration-post-mixer-plugin-install.png)

## <a name="2-redeploy-wwise-into-your-game"></a>2. Implementeren (opnieuw) Wwise in uw game
Opnieuw Wwise implementeren in uw spel, zelfs als u Wwise al hebt geïntegreerd. Dit neemt de invoegtoepassing Project akoestische Wwise in.

* **Engine-invoegtoepassing:** Als u Wwise geïnstalleerd als een game-invoegtoepassing in een Unreal C++-project hebt, kunt u deze stap overslaan. Als deze is geïnstalleerd in plaats daarvan als een invoegtoepassing engine voor het exemplaar omdat uw Unreal project blauwdruk alleen Wwise-implementatie met onze mixer-invoegtoepassing wat ingewikkelder is. Een Unreal C++ dummy, leeg project maken, sluit u dit als Unreal editor wordt geopend en volg de resterende procedure voor het implementeren van Wwise naar dit dummy-project. Kopieer vervolgens de geïmplementeerde Wwise-invoegtoepassing.
 
* Vanuit Wwise starten, klikt u op de **Unreal Engine** tabblad en klik vervolgens op het hamburgermenu naast **recente projecten van Unreal Engine** en selecteer **bladeren voor project**. Openen van uw game Unreal project `.uproject` bestand.

    ![Schermafbeelding van Wwise startprogramma voor apps van Unreal tabblad](media/wwise-unreal-tab.png)

* Klik vervolgens op **Wwise integreren in een Project** of **Wwise wijzigen in een Project**. Deze stap (opnieuw) integreert Wwise binaire bestanden in uw project, nu inclusief de Project-akoestische mixer-invoegtoepassing.

* **Engine-invoegtoepassing:** Als u van Wwise als een invoegtoepassing engine gebruikmaakt en project gemaakt de dummy als hierboven, Kopieer de map Wwise geïmplementeerd: `[DummyUProject]\Plugins\Wwise` en plak deze over `[UESource]\Engine\Plugins\Wwise`. `[DummyUProject]` het pad leeg Unreal C++-project en `[UESource]` is waar u de bronnen Unreal Engine is geïnstalleerd. Wanneer u klaar bent kopiëren, kunt u het project dummy verwijderen.

## <a name="3-add-the-project-acoustics-unreal-plugin-to-your-game"></a>3. De Project akoestische Unreal-invoegtoepassing toevoegen aan uw game
 
* Kopieer de `Unreal\ProjectAcoustics` map in de invoegtoepassing verpakt en maak een nieuwe map `[UProjectDir]\Plugins\ProjectAcoustics`, waarbij `UProjectDir` projectmap van uw game die is de `.uproject` bestand.
  * **Engine-invoegtoepassing**: Als u Wwise als een invoegtoepassing engine gebruikt, moet u Project akoestische gebruiken als een Unreal engine-invoegtoepassing. Gebruik in plaats van de bovenstaande doelmap: `[UESource]\Engine\Plugins\ProjectAcoustics`.

* Controleer of u ziet een `Wwise` map naast de `ProjectAcoustics` map. Het bevat de invoegtoepassing Wwise samen met de binaire bestanden voor de mixer-invoegtoepassing die u hebt (re-) in stap 2 hebt geïmplementeerd.

## <a name="4-extend-wwises-unreal-plugin-functionality"></a>4. De Wwise Unreal invoegtoepassing functionaliteit uitbreiden
* De Project akoestische Unreal invoegtoepassing vereist extra gedrag worden blootgesteld aan de invoegtoepassing Wwise Unreal API per [deze richtlijnen](https://www.audiokinetic.com/library/?source=UE4&id=using__initialsetup.html). We hebben een batch-bestand voor het automatiseren van de patch procedure opgenomen. 
* Binnen `Plugins\ProjectAcoustics\Resources`Voer `PatchWwise.bat`. Het voorbeeldproject van onze AcousticsGame maakt gebruik van een installatiekopie van het voorbeeld hieronder.

    ![Schermopname van Windows Explorer-venster markeren script voor het patchen van Wwise opgegeven](media/patch-wwise-script.png)

* Als u geen de DirectX-SDK geïnstalleerd, moet u de regel met DXSDK_DIR in commentaar `[UProject]\Plugins\Wwise\Source\AkAudio\AkAudio.Build.cs`

    ![Schermafbeelding van code-editor DXSDK als commentaar weergeven](media/directx-sdk-comment.png)

## <a name="5-build-game-and-check-python-is-enabled"></a>5. Games bouwen en controleer dat Python is ingeschakeld

* Compileer uw game en ervoor te zorgen dat deze correct bouwt. Anders controleert u de vorige stappen zorgvuldig door voordat u doorgaat. 
* Open het project in de Unreal-Editor. 
* **Engine-invoegtoepassing:** Als u ProjectAcoustics als engine-invoegtoepassing, zorg er ook voor dat deze ingeschakeld, die worden vermeld onder 'ingebouwde' invoegtoepassingen.
* Hier ziet u een nieuwe modus, waarmee wordt aangegeven dat project akoestische is geïntegreerd.

    ![Schermafbeelding van Unreal akoestische modus volledige weergeven](media/acoustics-mode-full.png)

* Controleer of u beschikt over de Python-invoegtoepassing voor Unreal ingeschakeld. Dit is vereist voor de integratie van de editor te laten functioneren.

    ![Schermafbeelding van het Python-extensies in de editor Unreal inschakelen](media/ensure-python.png)

## <a name="6-wwise-project-setup"></a>6. Instellen van het project Wwise

Een voorbeeldproject Wwise is inbegrepen bij het downloaden van de voorbeelden. We raden deze samen met deze instructies. De onderstaande schermafbeeldingen zijn afkomstig uit dit project.

### <a name="bus-setup"></a>Bus Setup
* De Project akoestische Unreal-invoegtoepassing wordt gezocht naar de bijbehorende mixer-invoegtoepassing op een bus met deze ***exacte*** naam: `Project Acoustics Bus`. Maak een nieuwe audio bus met deze naam. De mixer-invoegtoepassing kan worden gebruikt in verschillende configuraties, maar nu gaan we ervan uit dat dit te alleen weerklank verwerking wordt gebruikt. Deze bus hebben het signaal gemengde weerklank voor alle bronnen die akoestische gebruiken. Deze kunt upstream combineren in een structuur met een combinatie van bus, een voorbeeld hieronder ziet u, afkomstig uit onze Wwise-voorbeeldproject opgenomen in het voorbeeld downloaden.

    ![Schermafbeelding van Wwise bussen Project akoestische Bus weergeven](media/acoustics-bus.png)

* De kanaalconfiguratie van het op de bus moet worden ingesteld op een van: `1.0, 2.0, 4.0, 5.1 or 7.1`. Andere configuraties resulteert in geen uitvoer op deze bus.

    ![Schermopname van het kanaal configuratie-opties voor het Project akoestische Bus](media/acoustics-bus-channel-config.png)

* Nu gaat u in de Project-akoestische bus details en zorg ervoor dat u kunt zien dat de invoegtoepassing Mixer-tabblad

    ![Schermafbeelding van Wwise waarin wordt getoond hoe voor het inschakelen van de invoegtoepassing Mixer-tabblad voor het Project akoestische Bus](media/mixer-tab-enable.png)

* Vervolgens gaat u naar het tabblad van Mixer-invoegtoepassing en de invoegtoepassing project akoestische mixer toevoegen aan de bus

    ![Screenshow van Wwise bus waarin wordt getoond hoe het Project akoestische Mixer-invoegtoepassing toevoegen](media/add-mixer-plugin.png)

### <a name="actor-mixer-hierarchy-setup"></a>Actor-mixer hiërarchie-instellingen
* Uit prestatieoverwegingen geldt Project akoestische audio-DSP voor alle bronnen tegelijk. Hiervoor moet de invoegtoepassing functioneren als een mixer-invoegtoepassing. Wwise vereist mixer-invoegtoepassingen op de bus uitvoer, hoewel de bus uitvoer meestal het signaal droge uitvoer wordt. Project akoestische vereist het droge signaal worden gerouteerd via aux bussen terwijl het signaal dat NAT wordt uitgevoerd op de `Project Acoustics Bus`. Het volgende proces ondersteunt geleidelijke migratie naar deze signaalstroom.

* Stel dat hebt u een bestaand project met een actor-mixer-hiërarchie met aanpak, wapens en anderen op het hoogste niveau. Elk heeft bijbehorende uitvoer bus voor de Dr-mix. Hiermee kunt u dat u migreren van aanpak wilt voor het gebruik van akoestische. Maak eerst een bijbehorende aux-bus voor het uitvoeren van hun droge submix die een onderliggend element van de aanpak uitvoer-bus. We hebben bijvoorbeeld een "Droge" voorvoegsel gebruikt in onderstaande afbeelding te organiseren, hoewel de exacte naam is niet belangrijk. Alle meters of effecten u op de bus aanpak moest blijft werken als voorheen.

    ![Schermafbeelding van de aanbevolen Wwise droge Mix-installatie](media/wwise-dry-mix-setup.png)

* Wijzig vervolgens de structuur van de bus uitvoer voor de aanpak actor-mixer, met Project akoestische Bus instellen als uitvoer Bus en Dry_Footsteps instellen als een gebruiker gedefinieerde aux-bus.

    ![Schermafbeelding van de aanbevolen Wwise Actor Mixer Bus installatie](media/actor-mixer-bus-settings.png)

* Nu alle aanpak akoestische behandeld en hun weerklank op het Project akoestische-Bus uitvoeren. Het droge signaal wordt doorgestuurd via Dry_Footsteps en spatialized zoals gebruikelijk.

* Akoestische project is alleen van toepassing op geluiden waarvoor een 3D-locatie in de hele wereld. Volgende [Wwise documentatie](https://blog.audiokinetic.com/out-with-the-old-in-with-the-new-positioning-revamped-in-wwise-2018.1/), wordt de plaatsingseigenschappen moet worden ingesteld zoals wordt weergegeven. De instelling '3D Spatialization' is 'Plaats' of 'Positie + stand' indien nodig.

    ![Schermafbeelding van aanbevolen Wwise Actor plaatsing instellingen](media/wwise-positioning.png)

* Als u de uitvoer-Bus op andere bus die upstream combineert in **Project akoestische Bus** werkt niet. Wwise legt deze vereiste op mixer-invoegtoepassingen.

* Als u wilt dat een onderliggend element in de hiërarchie van de actor-mixer aanpak akoestische niet worden gebruikt, kunt u altijd 'bovenliggende overschrijven' op het gebruiken het afmelden.

* Als u game of gebruiker gedefinieerde verzendt voor weerklank op een actor-mixer in het spel, uitschakelen die op deze actor-mixer om te voorkomen dat twee keer weerklank toepassen.

### <a name="spatialization"></a>Spatialization
Standaard geldt de Project akoestische Wwise mixer-invoegtoepassing convolutiefilter weerklank, Wwise hiervoor panning spatialization verlaten. Wanneer u Project akoestische in deze standaardconfiguratie weerklank alleen-lezen, kunt u naar wens elk kanaal configuratie en spatialization methode gebruiken voor uw Dr-mix, zodat u kunt combineren en matchen bijna elk ruimtelijk met Project akoestische weerklank. Uw opties zijn onder andere [binaural spatializers Ambisonics gebaseerde](https://www.audiokinetic.com/products/ambisonics-in-wwise/) en [Windows Sonic](https://docs.microsoft.com/windows/desktop/CoreAudio/spatial-sound).
 
Akoestische project omvat een optionele ruimtelijk die ondersteuning biedt voor zowel op basis van een object met een hoge resolutie HRTF rendering, en pannen. Schakel het selectievakje 'Spatialization uitvoeren' aan de instellingen van de invoegtoepassing mixer en kiezen tussen HRTF of schuif en uitschakelen van de gebruiker gedefinieerde aux verzendt instellen boven aan het droge bussen om te voorkomen dat twee keer spatializing zowel met Project akoestische mixer-invoegtoepassing en Wwise. De modus spatialization kan niet worden gewijzigd in realtime, omdat hiervoor een geluid bank opnieuw genereren. U moet Unreal opnieuw opstarten en vervolgens opnieuw genereren soundbanks voordat play om op te halen mixer-invoegtoepassing wijzigingen in de configuratie, zoals het selectievakje 'Spatialization uitvoeren' bereikt.

![Schermafbeelding van Wwise Mixer-invoegtoepassing Spatialization-instellingen](media/mixer-spatial-settings.png)

Helaas, andere invoegtoepassingen ruimtelijk op basis van een object kan niet worden ondersteund op dit moment, terwijl ze worden geïmplementeerd als mixer-invoegtoepassingen en Wwise momenteel niet toegestaan om meerdere mixer invoegtoepassingen die zijn toegewezen aan een enkele actor-mixer.  

## <a name="7-audio-setup-in-unreal"></a>7. Audio-instellingen in Unreal
* Eerst moet u het niveau van uw games te produceren van een activum akoestische, dat wordt geplaatst in verdient `Content\Acoustics`. Raadpleeg de [Unreal verdient zelfstudie](unreal-baking.md) en hier hervatten. Bepaalde vooraf voltooide niveaus zijn opgenomen in het Voorbeeldpakket.
* Maak een actor akoestische ruimte in de scène. Alleen maken een van deze actoren in een niveau als het akoestische voor het niveau van de hele staat. 

    ![Schermafbeelding van Unreal editor met het maken van akoestische ruimte actor](media/create-acoustics-space.png)

* Nu de voltooide akoestische gegevensasset toewijzen aan de sleuf akoestische gegevens op de actor akoestische ruimte. Een scène heeft nu akoestische!

    ![Schermafbeelding van Unreal editor s howing akoestische Asset toewijzing](media/acoustics-asset-assign.png)

* Nu een lege actor toevoegen en doe het volgende:

    ![Schermafbeelding van Unreal editor akoestische onderdeel gebruik worden weergegeven in een lege actor](media/acoustics-component-usage.png)

1. Voeg een onderdeel akoestische Audio op de actor. Dit onderdeel kunt u het onderdeel Wwise audio uitbreiden met functionaliteit voor Project akoestische.
2. Het afspelen op Start vak wordt gecontroleerd door de standaardoptie, hierbij wordt de gebeurtenis gekoppeld Wwise op niveau opstarten.
3. Het selectievakje weergeven akoestische Parameters gebruiken om af te drukken op het scherm informatie over de bron voor foutopsporing.
    ![Schermafbeelding van Unreal deelvenster van editor akoestische op geluid bron met waarden van foutopsporing ingeschakeld](media/debug-values.png)
4. Toewijzen van een gebeurtenis Wwise per de gebruikelijke Wwise-werkstroom
5. Zorg ervoor dat de ruimtelijke Audio gebruiken is uitgeschakeld. Op dit moment als u akoestische Project voor een bepaald onderdeel van de audio u niet tegelijk gebruiken Wwise van ruimtelijke Audio-engine voor akoestische.

U bent nu klaar. Verplaatsen van de scène en Verken het akoestische effecten!

## <a name="next-steps"></a>Volgende stappen
* [Ontwerp](unreal-workflow.md) zelfstudie voor het Project akoestische in Unreal/Wwise
* [Meer informatie over het doen gebouwd](unreal-baking.md) voor uw game schermen 
