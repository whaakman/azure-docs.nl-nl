---
title: 'Zelfstudie: Stapsgewijze instructies voor het maken van een nieuwe HoloLens Unity-app met behulp van Azure ruimtelijke ankers | Microsoft Docs'
description: In deze zelfstudie leert u hoe u een nieuwe HoloLens Unity-app met behulp van Azure ruimtelijke ankers maakt.
author: julianparismorgan
manager: vriveras
services: azure-spatial-anchors
ms.author: pmorgan
ms.date: 07/05/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: 57244dd9f3365b3899bcc1dde6382cc3b51719d9
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67722925"
---
# <a name="tutorial-step-by-step-instructions-to-create-a-new-hololens-unity-app-using-azure-spatial-anchors"></a>Zelfstudie: Stapsgewijze instructies voor het maken van een nieuwe HoloLens Unity-app met behulp van Azure ruimtelijke ankers

Deze zelfstudie leert u hoe u een nieuwe HoloLens Unity-app maken met Azure ruimtelijke ankers.

## <a name="prerequisites"></a>Vereisten

Het volgende moet zijn geïnstalleerd om deze zelfstudie te voltooien:

1. Een Windows-machine met <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2017 +</a> geïnstalleerd met de **universele Windows-platformontwikkeling** werkbelasting en de **Windows 10 SDK (10.0.17763.0 of nieuwer)** onderdeel, en <a href="https://git-scm.com/download/win" target="_blank">Git voor Windows</a>.
2. De [C++/WinRT Visual Studio Extension (VSIX)](https://aka.ms/cppwinrt/vsix) voor Visual Studio moet worden geïnstalleerd vanuit de [Visual Studio Marketplace](https://marketplace.visualstudio.com/).
3. Een HoloLens-apparaat waarvoor de [ontwikkelaarsmodus](https://docs.microsoft.com/windows/mixed-reality/using-visual-studio) is ingeschakeld. Dit artikel vereist een HoloLens-apparaat met de [Update voor Windows van 10 oktober 2018](https://docs.microsoft.com/windows/mixed-reality/release-notes-october-2018 ) (ook wel bekend als RS5). Als u wilt bijwerken naar de nieuwste release op HoloLens, opent u de app **Instellingen**, gaat u naar **Bijwerken en beveiliging** en selecteert u vervolgens de knop **Controleren op updates**.

## <a name="getting-started"></a>Aan de slag

Eerst stellen we onze project en Unity scène:
1. Start Unity.
2. Selecteer **Nieuw**.
4. Zorg ervoor dat **3D** is geselecteerd.
5. Geef uw project de naam en voer een opslagbewerking **locatie**.
6. Klik op **project maken**.
7. Sla de scène leeg standaard een nieuw bestand met: **Bestand** > **opslaan als**.
8. Naam van de nieuwe scène **Main** en druk op de **opslaan** knop.

**De instellingen van het project instellen**

Nu stellen we de instellingen van sommige Unity-project die ons helpen bij de Windows Holographic-SDK voor de ontwikkeling van doel. 

Ten eerste kunt quality-instellingen voor de toepassing. 
1. Selecteer **bewerken** > **projectinstellingen** > **kwaliteit**
2. In de kolom met de **Windows Store** logo, klik op de pijl aan de **standaard** rij en selecteer **zeer laag**. Weet u de instelling correct wordt toegepast wanneer het vak in de **Windows Store** kolom en **zeer laag** rij is groen.

We moeten om te laten weten dat de app die we willen exporteren moet worden gebruikt voor het maken van een boeiende weergeven in plaats van een 2D-weergave Unity. We maken een boeiende weergave doordat Virtual Reality ondersteuning in Unity die gericht is op de Windows 10 SDK.

1. Ga naar **bewerken** > **projectinstellingen** > **Player**.
2. In de **deelvenster Inspector** voor **Player Settings**, selecteer de **Windows Store** pictogram.
3. Vouw de **XR instellingen** groep.
4. In de **Rendering** sectie, Controleer de **Virtual Reality ondersteund** selectievakje in om toe te voegen een nieuwe **Virtual Reality-SDK** lijst.
5. Controleer **Windows Mixed Reality** wordt weergegeven in de lijst. Als dit niet het geval is, selecteert u de **+** knop aan de onderkant van de lijst en kies **Windows Mixed Reality**.
 
> [!NOTE]
> Als u het pictogram Windows Store niet ziet, Controleer om te controleren of dat u de Windows Store .NET Scripting back-end voorafgaand aan installatie geselecteerd. Als dat niet het geval is, moet u mogelijk Unity opnieuw moet installeren met de juiste Windows-installatie.

**Controleer of de configuratie van .NET**
1. Ga naar **bewerken** > **projectinstellingen** > **Player** (u waarschijnlijk nog **Player** openen vanaf de vorige stap).
2. In de **deelvenster Inspector** voor **Player Settings**, selecteer de **Windows Store** pictogram.
3. In de **overige instellingen** configuratie sectie, zorg ervoor dat **Scripting back-end** is ingesteld op **.NET**.

**set-mogelijkheden**
1. Ga naar **bewerken** > **projectinstellingen** > **Player** (u waarschijnlijk nog **Player** openen vanaf de vorige stap).
2. In de **deelvenster Inspector** voor **Player Settings**, selecteer de **Windows Store** pictogram.
3. In de **publicatie-instellingen** configuratiesectie, selectievakje **InternetClientServer** en **SpatialPerception**.

**instellen van de belangrijkste virtuele camera**
1. In de **hiërarchie deelvenster**, selecteer **Main Camera**.
2. In de **Inspector**, stel de positie van de transformatie op **0,0,0**.
3. Zoeken de **wissen vlaggen** eigenschap, en wijzig de vervolgkeuzelijst met **Skybox** naar **effen kleur**.
4. Klik op de **achtergrond** veld om een kleurkiezer te openen.
5. Stel **R, G, B en A** naar **0**.
6. Selecteer **onderdeel toevoegen** en zoeken en toevoegen de **ruimtelijke toewijzing Collider**.

**ons script maken**
1. In de **Project** deelvenster, maak een nieuwe map **Scripts**onder de **activa** map. 
2. Klik met de rechtermuisknop op de map en selecteer vervolgens **maken >** ,  **C# Script**. Titel **AzureSpatialAnchorsScript**. 
3. Ga naar **GameObject** -> **maken leeg**. 
4. Selecteer deze, en klik in de **Inspector** Wijzig de naam van **GameObject** naar **MixedRealityCloud**. Selecteer **onderdeel toevoegen** en zoeken en toevoegen de **AzureSpatialAnchorsScript**.

**De prefab sphere maken**
1. Ga naar **GameObject** -> **3D-Object** -> **Sphere**.
2. In de **Inspector**, de schaal ingesteld op **0,25, 0,25, 0,25**.
3. Zoek de **Sphere** van het object in de **hiërarchie** deelvenster. Klik erop en sleep het naar de **activa** map in de **Project** deelvenster.
4. Klik met de rechtermuisknop en **verwijderen** de oorspronkelijke sphere u hebt gemaakt in de **hiërarchie** deelvenster.

U hebt nu een sphere prefab in uw **Project** deelvenster.

## <a name="trying-it-out"></a>De functies uitgeprobeerd
Als u wilt testen of alles werkt, kunt u uw app bouwen **Unity** en implementeren van **Visual Studio**. Ga als volgt hoofdstuk 6 van de [ **MR basisbeginselen 100: Aan de slag met Unity** cursus](https://docs.microsoft.com/windows/mixed-reality/holograms-100#chapter-6---build-and-deploy-to-device-from-visual-studio) om dit te doen. Hier ziet u de Unity beginnen met het scherm en klikt u vervolgens een duidelijk weer te geven.

## <a name="place-an-object-in-the-real-world"></a>Plaats een object in de praktijk
We gaan maken en plaatst u een object met behulp van uw app. Open de Visual Studio-oplossing die we hebben gemaakt wanneer we [onze app geïmplementeerd](#trying-it-out). 

Voeg eerst toe de volgende invoer in uw `Assembly-CSharp (Universal Windows)\Scripts\AzureSpatialAnchorsScript.cs`:

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=19-24)]

Voeg de volgende variabelen van de leden in uw `AzureSpatialAnchorsScript` klasse: 

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=26-42,48-52,60-79)]

Voordat u doorgaat, moet de sphere prefab dat we gemaakt op onze spherePrefab lidvariabele instellen. Ga terug naar **Unity**.
1. In **Unity**, selecteer de **MixedRealityCloud** van het object in de **hiërarchie** deelvenster.
2. Klik op de **Sphere** prefab die u hebt opgeslagen in de **Project** deelvenster. Sleep de **Sphere** u hebt geklikt op in de **Sphere Prefab** gebied onder **Azure ruimtelijke ankers Script (Script)** in de **Inspector** deelvenster .

U hebt nu de **Sphere** instellen als de prefab aan uw script. Samengesteld op basis van **Unity** en open vervolgens de resulterende **Visual Studio** oplossing weer, zoals u net heeft laten zien [uitprobeert](#trying-it-out). 

In **Visual Studio**, open `AzureSpatialAnchorsScript.cs` opnieuw. Voeg de volgende code in uw `Start()` methode. Deze code wordt koppelt `GestureRecognizer`, die wordt gedetecteerd wanneer er een Tik in de lucht en aanroep is `HandleTap`.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=81-90,93&highlight=4-10)]

We hebben nu om toe te voegen van de volgende `HandleTap()` methode onderstaande `Update()`. Er wordt een cast ray doen en een treffers waarop een sphere. 

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=267-277,299-300,304-312)]

Nu moeten we de bol maken. De bol wordt in eerste instantie zijn opgenomen, maar deze waarde later wordt aangepast. Voeg de volgende `CreateAndSaveSphere()` methode:

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=314-325,390)]

Uitvoeren van uw app uit **Visual Studio** zodra er meer valideren. Deze tijd, tik op het scherm voor het maken en uw wit sphere plaats op het vlak van uw keuze.

## <a name="set-up-the-dispatcher-pattern"></a>Instellen van de functie voor berichtverzending-patroon

Als u werkt met Unity, moet alle Unity API's, bijvoorbeeld API's die u gebruiken voor UI-updates worden uitgevoerd op de hoofdthread. In de code die er echter schrijft, krijgen we retouraanroepen voor andere threads. We willen UI in deze callbacks, bijwerken, zodat er een manier om te gaan van een thread aan clientzijde op de hoofdthread nodig. Voor het uitvoeren van code op de hoofdthread van een thread aan clientzijde, gebruiken we de functie voor berichtverzending-patroon. 

We gaan toevoegen een lidvariabele, dispatchQueue, dit is een wachtrij van acties. We acties push naar de wachtrij staan, en vervolgens uit de wachtrij verwijderen en de acties uitvoeren op de hoofdthread. 

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=38-51&highlight=6-9)]

Vervolgens gaan we een manier om een actie toevoegen aan de wachtrij toevoegen. Voeg `QueueOnUpdate()` direct na het `Update()` :

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=107-117)]

Laten we nu gebruiken de lus Update() om te controleren of er een actie is in de wachtrij. Zo ja, we uit de wachtrij verwijderen de actie en voer deze uit.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=95-105&highlight=4-10)]

## <a name="get-the-azure-spatial-anchors-sdk"></a>De Azure-ruimtelijke ankers SDK ophalen

We nu downloadt de Azure-SDK voor ruimtelijke ankers. Ga naar [Azure ruimtelijke ankers GitHub-releasepagina](https://github.com/Azure/azure-spatial-anchors-samples/releases). Onder activa, downloaden de **AzureSpatialAnchors.unitypackage** bestand. 

In Unity, gaat u naar **activa**, klikt u op **pakket importeren** > **Custom Package...** . Navigeer naar het pakket en selecteer **Open**.

In de nieuwe **Unity-pakket importeren** venster dat pop's, selecteer **geen** in de linkerbenedenhoek. Klik vervolgens onder **AzureSpatialAnchorsPlugin** > **invoegtoepassingen**, selecteer **algemene**, **Editor**, en  **HoloLens**. Klik op **importeren** in de rechterbenedenhoek.

Nu moeten we het herstellen van Nuget-pakketten om op te halen van ruimtelijke ankers-SDK van Azure. Samengesteld op basis van **Unity** en open vervolgens en bouwen de resulterende **Visual Studio** oplossing weer, zoals beschreven in de [uitprobeert](#trying-it-out). 

In uw **Visual Studio** oplossing, voeg de volgende import in uw `<ProjectName>\Assets\Scripts\AzureSpatialAnchorsScript.cs`:

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=23-26&highlight=1)]

Voeg de volgende variabelen in uw `AzureSpatialAnchorsScript` klasse:

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=48-63&highlight=6-11)]

## <a name="attach-a-local-azure-spatial-anchor-to-the-local-anchor"></a>Een lokale Azure ruimtelijke anker koppelen aan het lokale anker

Laten we Azure ruimtelijke anker CloudSpatialAnchorSession instellen. We beginnen door toe te voegen van de volgende `InitializeSession()` methode binnen uw `AzureSpatialAnchorsScript` klasse. Wanneer met de naam, zorgt het ervoor dat de sessie van een Azure ruimtelijke ankers is gemaakt en tijdens het opstarten van uw app de juiste wijze geïnitialiseerd.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=174-202,205-209)]

Nu moeten we schrijven van code voor het afhandelen van gemachtigde aanroepen. We voegt meer aan hen als we blijven.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=211-226)]

Nu gaan we een toepassing aansluiten uw `initializeSession()` methode in uw `Start()` methode.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=81-93&highlight=12)]

Voeg de volgende code in uw `CreateAndSaveSphere()` methode. Er wordt een lokale Azure ruimtelijke anker koppelen aan het gebied dat we in de praktijk wilt plaatsen.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=314-338,390&highlight=14-25)]

Voordat u verdergaat, moet u een Azure-ruimtelijke ankers te maken account-id en sleutel, als u deze nog niet hebt. Volg de volgende sectie om ze te verkrijgen.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="upload-your-local-anchor-into-the-cloud"></a>Uw lokale anker uploaden naar de cloud

Zodra u uw Azure ruimtelijke ankers account-id en -sleutel hebt, gaat u en plak de `Account Id` in `SpatialAnchorsAccountId` en de `Account Key` in `SpatialAnchorsAccountKey`.

Ten slotte gaan we een toepassing aansluiten alles bij elkaar. In uw `SpawnNewAnchoredObject()` methode, voeg de volgende code toe. Deze roept de `CreateAnchorAsync()` methode zodra uw sphere is gemaakt. Zodra de methode retourneert, worden de volgende code wordt uitgevoerd een laatste update van uw bol, wijzigt de kleur blauw.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=314-391&highlight=26-77)]

Uitvoeren van uw app uit **Visual Studio** zodra er meer. Uw hoofd verplaatsen en klik vervolgens in de lucht tikken om uw sphere. Zodra er voldoende frames, de sphere worden omgezet in geel en het uploaden van de cloud wordt gestart. Nadat het uploaden is voltooid, wordt uw sphere vervolgens blauw weergegeven. (Optioneel) u kunt ook gebruiken het uitvoervenster in **Visual Studio** voor het bewaken van de berichten voor het verzenden van uw app. U moet mogelijk zijn om te bekijken van de aanbevolen voor maken wordt uitgevoerd, evenals de anker-id die de cloud wordt geretourneerd als het uploaden is voltooid.

> [!NOTE]
> Als u ' DllNotFoundException: Kan DLL 'AzureSpatialAnchors' niet laden: De opgegeven module kan niet worden gevonden. ', moet u **schoon** en **bouwen** uw oplossing voor het opnieuw.

## <a name="locate-your-cloud-spatial-anchor"></a>Zoek uw cloud ruimtelijke anker

Een uw anker is geüpload naar de cloud, kunt u proberen deze opnieuw te zoeken. We gaan toevoegen de volgende code in uw `HandleTap()` methode. Deze code wordt:

* Bel `ResetSession()`, die stopt de `CloudSpatialAnchorSession` en onze bestaande blauw sphere verwijderen uit het scherm.
* Initialiseren `CloudSpatialAnchorSession` opnieuw. We doen dit zodat we zeker weet dat het anker gaan we zoeken afkomstig is van de cloud in plaats van het lokale anker die we hebben gemaakt.
* Maak een **Watcher** waarmee wordt gezocht naar het anker wordt geüpload naar Azure ruimtelijke ankers.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=267-305&highlight=13-31,35-36)]

Laten we nu toevoegen onze `ResetSession()` en `CleanupObjects()` methoden. U kunt deze onderstaande plaatsen `QueueOnUpdate()`

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=119-172)]

Nu moeten we koppelt de code die wordt aangeroepen wanneer het anker dat we query's voor zich bevindt. In `InitializeSession()`, de volgende callbacks toevoegen:

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=200-206&highlight=4-5)]

 
We gaan nu code toevoegen die wordt maken en een groene sphere plaatsen zodra de CloudSpatialAnchor zich bevindt. Deze ook inschakelen met scherm te tikken op opnieuw, zodat u kunt het volledige scenario zodra er meer herhalen: maken van een andere lokale anker, uploaden en zoek het opnieuw.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=228-265)]

Dat is alles. Uitvoeren van uw app uit **Visual Studio** een keer voor het uitproberen van het hele end-to-end-scenario. Verplaatsen van uw apparaat en uw wit sphere plaats. Vervolgens, blijven uw hoofd om vast te leggen van omgevingsgegevens totdat de bol geel wordt te verplaatsen. Uw lokale anker wordt geüpload en uw sphere wordt blauw inschakelen. Ten slotte, tikt u op uw scherm zodra er meer, zodat uw lokale anker wordt verwijderd en we vervolgens voor het bijbehorende equivalent van de cloud zoeken. Doorgaan met uw apparaat verplaatsen totdat uw ruimtelijke anker cloud bevindt. Een groene sphere moet worden weergegeven in de juiste locatie, en u kunt spoel en herhaal het hele scenario opnieuw.

[!INCLUDE [AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md)]