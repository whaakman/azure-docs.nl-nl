---
title: Bekende problemen met de invoeg toepassing voor project akoestische
titlesuffix: Azure Cognitive Services
description: U kunt de volgende bekende problemen tegen komen wanneer u de preview-versie van de ontwerp functie voor project akoestische gebruikt.
services: cognitive-services
author: kylestorck
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: kylsto
ROBOTS: NOINDEX
ms.openlocfilehash: df5a4ce62af0405ffab8f711fb40b3d92083a3a4
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68706651"
---
# <a name="project-acoustics-known-issues"></a>Bekende problemen in Project akoestische
U kunt de volgende bekende problemen tegen komen wanneer u de preview-versie van de ontwerp functie voor project akoestische gebruikt.

## <a name="acoustic-parameters-are-lost-when-you-rename-a-scene"></a>Akoestische para meters gaan verloren wanneer u de naam van een scène wijzigt

Als u de naam van een scène wijzigt, worden alle akoestische para meters die deel uitmaken van die scène, niet automatisch overgedragen naar de nieuwe scène. Ze blijven echter wel aanwezig in het oude activa bestand. Zoek naar het bestand **SceneName_AcousticParameters. Asset** in de map **Editor** naast uw scène bestand. Wijzig de naam van het bestand zodat dit overeenkomt met de nieuwe scène.

## <a name="unity-crashes-when-closing-project"></a>Eenheid loopt vast bij sluiten van project

Op de nieuwste versies van Unity (2018.2 +) is er sprake van een bekende fout waarbij unit vastloopt wanneer u het project sluit. Dit wordt bijgehouden door [Dit Unit-probleem](https://issuetracker.unity3d.com/issues/crash-on-assetdatabase-getassetimporterversions-when-closing-a-specific-unity-project).

## <a name="deploying-to-android-from-some-unity-versions"></a>Implementeren in Android vanuit een aantal units-versies

Sommige versies van unit eenheid hebben een bug bij het implementeren van audio-invoeg toepassingen voor Android. Zorg ervoor dat u geen versie gebruikt die door [deze bug](https://issuetracker.unity3d.com/issues/android-ios-audiosource-playing-through-google-resonance-audio-sdk-with-spatializer-enabled-does-not-play-on-built-player)wordt beïnvloed.

## <a name="i-get-an-error-that-could-not-find-metadata-file-systemsecuritydll"></a>Er verschijnt een fout bericht met de melding dat het bestands systeem van de meta gegevens niet kan worden gevonden. Security. dll

Zorg ervoor dat de runtime-versie van scripting in de instellingen van de speler is ingesteld op **.net 4. x equivalent**, en start eenheid opnieuw op.

## <a name="im-having-authentication-problems-when-connecting-to-azure"></a>Ik ondervind verificatie problemen bij het maken van verbinding met Azure

Controleer of u de juiste referenties voor uw Azure-account hebt gebruikt, of het account het type knoop punt ondersteunt dat is aangevraagd in de maken, en dat de systeem klok juist is.

## <a name="canceling-a-bake-leaves-the-bake-tab-in-deleting-state"></a>Als u een maken annuleert, wordt het tabblad maken in de status verwijderen verwijderd
Met de geluids bestanden van het project worden alle Azure-resources voor een taak opgeschoond na voltooiing of annulering. Dit kan vijf minuten duren.

## <a name="next-steps"></a>Volgende stappen
* De voorbeeld [](unity-quickstart.md) inhoud van Unity of [Unreal](unreal-quickstart.md) proberen

