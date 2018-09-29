---
title: Akoestische-walkthrough voor voorbeeld - Cognitive Services-project
description: In dit scenario beschrijft de scène Unity-voorbeeld voor Project akoestische, met inbegrip van de implementatie van het bureaublad en VR.
services: cognitive-services
author: kegodin
manager: noelc
ms.service: cognitive-services
ms.component: acoustics
ms.topic: article
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: eaf7ff9f7f791fd6d04e6b76d256b4987c50cd13
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47434089"
---
# <a name="unity-sample-walkthrough"></a>Walkthrough voor voorbeeld van Unity
Dit is een overzicht van het Project akoestische-voorbeeld. Voor meer informatie over welke akoestische Project is, bekijk de [Inleiding tot Project akoestische](what-is-acoustics.md). Als u hulp nodig hebt in het Project akoestische-pakket toe te voegen aan een bestaande Unity-project, gebruikt de [introductiehandleiding](getting-started.md).

## <a name="requirements-for-running-the-sample-project"></a>Vereisten voor het uitvoeren van het voorbeeldproject
* Unity 2018.2 +, met behulp van .NET 4.x scripting runtime-versie
* Windows 64-bits Unity Editor
* Het voorbeeld biedt ondersteuning voor Windows-bureaublad, UWP en Android-doelen, met inbegrip van beeldschermen head gekoppeld (HMDs)
* Azure Batch-abonnement nodig voor bake proces

## <a name="sample-project-setup"></a>Instellen van het project
Download en importeer het **MicrosoftAcoustics.Sample.unitypackage**. Tijdens het importeren, projectinstellingen inclusief **ruimtelijk** en **runtimeversie Scripting** worden bijgewerkt om te voldoen aan de vereisten van de invoegtoepassing. Wanneer dit voltooid is, ziet u een fout in de Unity-console van **AcousticsGeometry.cs** over het wijzigen van de Runtime-versie uitvoeren van scripts op **.NET 4.x Equivalent**. Deze wijziging van de instellingen wordt uitgevoerd als onderdeel van het importeren van het pakket, maar vereist een herstart Unity moet zijn van kracht. Unity nu opnieuw opstarten.

## <a name="running-the-sample"></a>Het voorbeeld uitvoeren
Het voorbeeld bevat een scène demo **Assets/AcousticsDemo/ProjectAcousticsDemo.unity**. Deze scène heeft drie geluid bronnen. Standaard, slechts één bron van geluid wordt afgespeeld en de andere twee zijn onderbroken. Deze bevinden zich in **geluid bronnen** in de **hiërarchie**. Als u een algemene navigatie-script, wordt de Camera van het hoofdvenster een onderliggend element van het object CameraHolder. 

![Hiërarchie weergeven](media/SampleHierarchyView.png)

De scène al is sparen en een ACE-bestand is gekoppeld aan de **MicrosoftAcoustics** prefab in de **hiërarchie**. 

Luister naar hoe de scène geluiden door te klikken op de knop afspelen in de Unity-editor. Gebruik op het bureaublad, W, A, S, D en de muis om te navigeren. Als u wilt vergelijken hoe de scène met en zonder akoestische klinkt, drukt u op de **R** knop totdat de overlaytekst verandert in rood en zegt "akoestische: uitgeschakeld." Als u wilt zien sneltoetsen voor meer besturingselementen, drukt u op **F1**. Alle besturingselementen zijn ook bruikbaar met de rechtermuisknop te klikken om te selecteren van de actie uit te voeren, vervolgens links te klikken op als de bewerking wilt uitvoeren.

## <a name="targeting-other-platforms"></a>Die gericht is op andere platforms
Het voorbeeld bevat de instellingen uit te voeren op Windows-bureaublad-, UWP-, Windows Mixed Reality-, Android- en Oculus Go. Het project is standaard geconfigureerd voor Windows-bureaublad. Als u wilt een VR-platform als doel, gaat u naar de player-instellingen (**bewerken > Projectinstellingen > Player**), vinden de **XR instellingen**, en controleer de **Virtual Reality ondersteund** selectievakje.

![VR inschakelen](media/VRSupport.png)  

Verbinding maken met een hoofdtelefoon VR tot uw PC. Ga naar **bestand > Build Settings**, en klikt u op **bouwen en uitvoeren** het voorbeeld implementeren in uw hoofdtelefoon VR. Navigeer door de scène met behulp van de bewegingscontrollers voor uw hoofdtelefoon of gebruik W, A-Z, D op het toetsenbord.    
Als u wilt richten op Android- en Oculus Go, kiest u Android van de **Build Settings** menu. Klik op **overschakelen doel**, klikt u vervolgens **bouwen en uitvoeren**. Hierdoor wordt de voorbeeld-scène geïmplementeerd naar uw verbonden Android-apparaat. Zie voor meer informatie over het ontwikkelen van Unity voor Android [documentatie voor Unity](https://docs.unity3d.com/Manual/android-GettingStarted.html).

![Doel-Android](media/TargetAndroid.png)  

## <a name="next-steps"></a>Volgende stappen
* [Maak een Azure-account](create-azure-account.md) voor uw eigen gebouwd
* Verken de [proces ontwerpen](design-process.md)

