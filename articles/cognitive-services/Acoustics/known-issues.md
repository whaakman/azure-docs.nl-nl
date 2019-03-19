---
title: Bekende problemen met Project akoestische-invoegtoepassing
titlesuffix: Azure Cognitive Services
description: U kunt de volgende bekende problemen kan optreden bij het gebruik van de Designer-Preview voor Project akoestische.
services: cognitive-services
author: kylestorck
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: resources
ms.date: 08/17/2018
ms.author: kylestorck
ms.openlocfilehash: 4cbede768409596365bea0cdbbb451cc5195ac4b
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58136256"
---
# <a name="project-acoustics-known-issues"></a>Project akoestische bekende problemen
U kunt de volgende bekende problemen kan optreden bij het gebruik van de Designer-Preview voor Project akoestische.

## <a name="acoustic-parameters-are-lost-when-you-rename-a-scene"></a>Akoestische parameters, gaan verloren als u de naam van een scène

Als u de naam van een scène, worden alle de akoestische parameters die deel uitmaken van die scène wordt niet automatisch naar de nieuwe scène overdragen. Nog steeds wordt aanwezig in het oude assetbestand echter. Zoek de **SceneName_AcousticParameters.asset** bestand in de **Editor** map naast uw scène-bestand. Wijzig de naam van het bestand om de naam van de nieuwe scène weer te geven.

## <a name="unity-crashes-when-closing-project"></a>Unity loopt vast bij het sluiten van project

Op de meest recente versies van Unity (2018.2 +) is er een bekend probleem waarbij Unity loopt vast bij het sluiten van uw project. Dit wordt gevolgd door [dit probleem Unity](https://issuetracker.unity3d.com/issues/crash-on-assetdatabase-getassetimporterversions-when-closing-a-specific-unity-project).

## <a name="trouble-deploying-to-android"></a>Problemen met de implementatie op Android
Voor het gebruik van akoestische Project in Android, wijzigt u het doel-build in Android. Sommige versies van Unity hebben een bug met de implementatie van audio-invoegtoepassingen. Zorg ervoor dat u niet gebruikt een versie die wordt beïnvloed door [deze bug](https://issuetracker.unity3d.com/issues/android-ios-audiosource-playing-through-google-resonance-audio-sdk-with-spatializer-enabled-does-not-play-on-built-player).

## <a name="i-get-an-error-that-could-not-find-metadata-file-systemsecuritydll"></a>Ik krijg een foutmelding dat 'kan niet vinden-bestand met metagegevens System.Security.dll'

Zorg ervoor dat de Runtime-versie uitvoeren van scripts in de instellingen van de speler is ingesteld op **.NET 4.x Equivalent**, en Unity opnieuw te starten.

## <a name="im-having-authentication-problems-when-connecting-to-azure"></a>Ik ondervind verificatieproblemen met bij het verbinden met Azure

Controleer u de juiste referenties hebt gebruikt voor uw Azure-account, dat het type knooppunt in de bake aangevraagd ondersteuning biedt voor uw account en dat de systeemklok van uw nauwkeurig is.

## <a name="canceling-a-bake-leaves-the-bake-tab-in-deleting-state"></a>Het tabblad Bake status wordt 'verwijderd' verlaat annuleren van een bake
Project akoestische schonen vervolgens alle Azure-resources voor een taak voor de bewerking is voltooid of geannuleerd. Dit kan tot vijf minuten duren.

## <a name="next-steps"></a>Volgende stappen
* Probeer de [Unity](unity-quickstart.md) of [Unreal](unreal-quickstart.md) voorbeeld van inhoud

