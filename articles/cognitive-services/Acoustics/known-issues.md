---
title: Bekende problemen met de invoegtoepassing akoestische - Cognitive Services
description: U kunt de volgende bekende problemen kan optreden bij het gebruik van de Designer-Preview voor Project akoestische.
services: cognitive-services
author: kylestorck
manager: noelc
ms.service: cognitive-services
ms.component: acoustics
ms.topic: article
ms.date: 08/17/2018
ms.author: kylestorck
ms.openlocfilehash: b0dd5e0c365c65fc7b29752f7885acb71bdb3df8
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/17/2018
ms.locfileid: "40181575"
---
# <a name="known-issues"></a>Bekende problemen
U kunt de volgende bekende problemen kan optreden bij het gebruik van de Designer-Preview voor Project akoestische.

## <a name="acoustic-parameters-are-lost-when-you-rename-a-scene"></a>Akoestische parameters, gaan verloren als u de naam van een scène

Als u de naam van een scène, worden de akoestische parameters die deel uitmaken van die scène niet automatisch overgedragen naar de nieuwe scène. Ze nog steeds aanwezig in het oude assetbestand echter. Zoek de **SceneName_AcousticParameters.asset** bestand in de **Editor** map naast uw scène-bestand. Wijzig de naam van het bestand om de naam van de nieuwe scène weer te geven.

## <a name="the-default-path-for-the-acousticsdata-folder-in-probes-tab-is-an-absolute-path"></a>Het standaardpad voor de map AcousticsData in tests tabblad is een absoluut pad

Dit moet standaard terugvallen op een relatief pad naar het eenvoudiger om te delen tussen de deelnemers een project. Als tijdelijke oplossing, moet u het pad ten opzichte van de projectmap te wijzigen.

## <a name="runtime-voxels-are-a-different-size-than-design-time-voxels"></a>Runtime-voxels zijn een ander formaat dan moment van ontwerp voxels

Als u een **berekenen** op de **tests** tabblad en bekijk de voxels, voert u een voxels bake en de weergave tijdens runtime voor de dezelfde scène, de voxels zich verschillende formaten. De voxels vooraf bake weergegeven, zijn de voxels gebruikt in de simulatie. De voxels weergegeven tijdens runtime worden gebruikt voor interpolatie tussen de test-punten. Dit kan ertoe leiden dat een inconsistentie waar portals open tijdens runtime die daadwerkelijk open niet wordt weergegeven.

## <a name="uwp-builds-not-working"></a>UWP bouwt werkt niet

UWP-builds worden niet verwerkt op de meest recente versies van Unity (2018.2 +). De uitvoeringsfase van de build wordt achterstallige en krijgt u fouten 'Unity-extensies zijn nog niet geïnitialiseerd'. Dit wordt gevolgd door [dit probleem Unity](https://fogbugz.unity3d.com/default.asp?1070491_1rgf14bakv5u779d).

## <a name="unity-crashes-when-closing-project"></a>Unity loopt vast bij het sluiten van project

Op de meest recente versies van Unity (2018.2 +) is er een bekend probleem waarbij Unity loopt vast bij het sluiten van uw project. Dit wordt gevolgd door [dit probleem Unity](https://issuetracker.unity3d.com/issues/crash-on-assetdatabase-getassetimporterversions-when-closing-a-specific-unity-project).

## <a name="trouble-deploying-to-android"></a>Problemen met de implementatie op Android
Voor het gebruik van akoestische Project in Android, wijzigt u het doel-build in Android. Sommige versies van Unity bevatten een fout met de implementatie van audio-invoegtoepassingen--Zorg ervoor dat u niet gebruikt een versie die wordt beïnvloed door [deze bug](https://issuetracker.unity3d.com/issues/android-ios-audiosource-playing-through-google-resonance-audio-sdk-with-spatializer-enabled-does-not-play-on-built-player).

## <a name="i-get-an-error-that-could-not-find-metadata-file-systemsecuritydll"></a>Ik krijg een foutmelding dat 'kan niet vinden-bestand met metagegevens System.Security.dll'

Zorg ervoor dat de Runtime-versie uitvoeren van scripts in de instellingen van de speler is ingesteld op **.NET 4.x Equivalent**, en Unity opnieuw te starten.

## <a name="im-having-authentication-problems-when-connecting-to-azure"></a>Ik ondervind verificatieproblemen met bij het verbinden met Azure

Controleer u de juiste referenties hebt gebruikt voor uw Azure-account, dat het type knooppunt in de bake aangevraagd ondersteuning biedt voor uw account en dat de systeemklok van uw nauwkeurig is.

## <a name="next-steps"></a>Volgende stappen
* Aan de slag [integreren akoestische in uw Unity-project](getting-started.md)

