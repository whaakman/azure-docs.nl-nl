---
title: Opdrachtregelprogramma build voor Azure | Microsoft Docs
description: Opdrachtregelprogramma build voor Azure
services: visual-studio-online
author: ghogen
manager: douge
assetId: 94b35d0d-0d35-48b6-b48b-3641377867fd
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 03/05/2017
ms.author: ghogen
ms.openlocfilehash: 7d0138abb07aea46ad8d0069c87964b393347dcf
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
ms.locfileid: "31791423"
---
# <a name="building-azure-projects-from-the-command-line"></a>Het bouwen van Azure projecten vanaf de opdrachtregel
Met de Engine van Microsoft bouwen (MSBuild), kunt u producten in de build-omgevingen waarin Visual Studio niet is geïnstalleerd. MSBuild maakt gebruik van een XML-indeling voor bestanden in dat de uitbreidbare en volledig wordt ondersteund door Microsoft. Met de MSBuild-bestandsindeling, kunt u aangeven welke items moeten worden gebouwd voor een of meer platforms en configuraties.

U kunt ook MSBuild uitvoeren op de opdrachtregel en dit onderwerp wordt beschreven die benadering. U kunt specifieke configuraties van een project maken door het instellen van eigenschappen op de opdrachtregel. Op deze manier kunt u ook de doelen die MSBuild voortbouwt definiëren. Zie voor meer informatie over opdrachtregelparameters en MSBuild [MSBuild-Naslaggids](https://msdn.microsoft.com/library/ms164311.aspx).

## <a name="msbuild-parameters"></a>MSBuild-parameters
De eenvoudigste manier om een pakket te maken is om uit te voeren MSBuild met de `/t:Publish` optie. Met deze opdracht maakt standaard een map ten opzichte van de hoofdmap voor het project, zoals `<ProjectDirectory>\bin\Configuration\app.publish\`. Wanneer u een Azure-project maakt, twee bestanden worden gegenereerd: het pakket van het bestand zelf en het bijbehorende configuratiebestand:

* Bestand van het pakket (`project.cspkg`)
* Configuratiebestand (`ServiceConfiguration.TargetProfile.cscfg`)

Elke Azure-project bevat standaard één service-configuratiebestand voor de lokale (foutopsporing) builds en een andere voor builds cloud (fasering of productie). U kunt echter toevoegen of verwijderen van bestanden van de configuratie van de service zo nodig. Wanneer u een pakket vanuit Visual Studio maakt, wordt u gevraagd welke service-configuratiebestand om op te nemen samen met het pakket. Als u een pakket met behulp van MSBuild bouwen, wordt het lokale bestand voor de configuratie van de service standaard opgenomen. Instellen als u wilt opnemen in een ander bestand in de configuratie van de service, de `TargetProfile` eigenschap van de MSBuild-opdracht (`MSBuild /t:Publish /p:TargetProfile=ProfileName`).

Als u gebruiken van een alternatieve map voor het pakket opgeslagen en configuratiebestanden wilt, het pad worden ingesteld met behulp van de `/p:PublishDir=Directory\` optie, met inbegrip van het afsluitende backslash scheidingsteken.

## <a name="next-steps"></a>Volgende stappen
Nadat het pakket is gebouwd, kunt u deze kunt implementeren naar Azure.