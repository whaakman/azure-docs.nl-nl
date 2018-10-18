---
title: 'Voorbeeld: Project Acoustics'
titlesuffix: Azure Cognitive Services
description: In deze handleiding wordt de Unity-voorbeeldscène voor Project Acoustics beschreven, met inbegrip van de implementatie naar het bureaublad en VR.
services: cognitive-services
author: kegodin
manager: cgronlun
ms.service: cognitive-services
ms.component: acoustics
ms.topic: sample
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: f5ea565e68579dfad601d1037daeb4113e3daa43
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48901155"
---
# <a name="unity-sample-walkthrough"></a>Handleiding voor het Unity-voorbeeld
Dit is een handleiding voor het Project Acoustics-voorbeeld. Voor meer informatie over wat Acoustics Project is, bekijkt u de [Inleiding op Project Acoustics](what-is-acoustics.md). Als u hulp nodig hebt bij het toevoegen van het Project Acoustics-pakket aan een bestaand Unity-project, bekijkt u de handleiding [Aan de slag](getting-started.md).

## <a name="requirements-for-running-the-sample-project"></a>Vereisten voor het uitvoeren van het voorbeeldproject
* Unity 2018.2+, met .NET 4.x Scripting Runtime Version
* Windows 64-bits Unity Editor
* Het voorbeeld biedt ondersteuning voor Windows-bureaublad en UWP- en Android-doelen, met inbegrip van head-mounted displays (HMD‘s)
* Azure Batch-abonnement vereist voor het proces

## <a name="sample-project-setup"></a>Het voorbeeldproject instellen
Download en importeer **MicrosoftAcoustics.Sample.unitypackage**. Tijdens het importeren worden de projectinstellingen, inclusief **Spatializer** en **Scripting Runtime Version** bijgewerkt om te voldoen aan de vereisten van de invoegtoepassing. Wanneer dit is voltooid, ziet u in de Unity-console een fout van **AcousticsGeometry.cs** over het wijzigen van de Scripting Runtime Version in **.NET 4.x Equivalent**. Deze wijziging van de instellingen wordt uitgevoerd als onderdeel van het importeren van het pakket, maar er is een herstart nodig voordat Unity effectief is. Start Unity nu opnieuw op.

## <a name="running-the-sample"></a>Het voorbeeld uitvoeren
Het voorbeeld bevat een voorbeeldscène **Assets/AcousticsDemo/ProjectAcousticsDemo.unity**. Deze scène heeft drie geluidsbronnen. Standaard wordt slechts één geluidsbron afgespeeld en de andere twee zijn onderbroken. Deze bevinden zich in **Geluidsbronnen** in de **hiërarchie**. Om u te helpen een algemeen navigatiescript te maken, is de hoofdcamera een onderliggend element van het object CameraHolder. 

![Hiërarchieweergave](media/SampleHierarchyView.png)

Dit scène is al voltooid en er is een ACE-bestand gekoppeld aan de **MicrosoftAcoustics**-prefab in de **hiërarchie**. 

Luister naar het geluid van de scène door te klikken op de knop Afspelen in de Unity-editor. Gebruik op het bureaublad W, A, S, D en de muis om te navigeren. Als u wilt vergelijken hoe de scène met en zonder Acoustics klinkt, drukt u op **R** totdat de overlaytekst rood wordt en zegt "Acoustics: Disabled". Als u sneltoetsen wilt zien voor meer besturingselementen, drukt u op **F1**. Alle besturingselementen zijn ook bruikbaar door met de rechtermuisknop te klikken om de actie te selecteren die u wilt uitvoeren. Vervolgens klikt u met de linkermuisknop om de actie uit te voeren.

## <a name="targeting-other-platforms"></a>Scenario voor andere platformen
Het voorbeeld bevat instellingen om uit te voeren op Windows-bureaublad, UWP, Windows Mixed Reality, Android en Oculus Go. Het project is standaard geconfigureerd voor Windows-bureaublad. Als u een VR-platform als doel hebt, gaat u naar de player-instellingen (**Bewerken > Projectinstellingen > Player**), zoekt u **XR-instellingen** en schakelt u het selectievakje **Virtual Reality ondersteund** in.

![VR inschakelen](media/VRSupport.png)  

Sluit een VR-headset aan op uw pc. Ga naar **Bestand > Build-instellingen** en klik op **Bouwen en uitvoeren** om het voorbeeld te implementeren in uw VR-headset. Navigeer door de scène met behulp van de bewegingscontrollers voor uw headset of gebruik W, A, S, D op het toetsenbord.    
Als u Android en Oculus Go als doel hebt, kiest u Android in het menu **Build-instellingen**. Klik op **Doel wisselen** en klik vervolgens op **Bouwen en uitvoeren**. Hierdoor wordt de voorbeeldscène geïmplementeerd in uw verbonden Android-apparaat. Zie de [Unity-documentatie](https://docs.unity3d.com/Manual/android-GettingStarted.html) voor meer informatie over Unity-ontwikkeling voor Android.

![Android als doel](media/TargetAndroid.png)  

## <a name="next-steps"></a>Volgende stappen
* [Een Azure-account maken](create-azure-account.md) voor uw eigen processen
* Het [ontwerpproces](design-process.md) verkennen

