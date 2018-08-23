---
title: Opdrachtregel bouwen voor Azure | Microsoft Docs
description: Opdrachtregel bouwen voor Azure
services: visual-studio-online
author: ghogen
manager: douge
assetId: 94b35d0d-0d35-48b6-b48b-3641377867fd
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/05/2017
ms.author: ghogen
ms.openlocfilehash: c47da12feeef2a1536cdbfbe0187fe8991b3257d
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42060585"
---
# <a name="building-azure-projects-from-the-command-line"></a>Het bouwen van projecten voor Azure vanaf de opdrachtregel
Met behulp van de Microsoft Build Engine (MSBuild), kunt u bouwen producten in de build-omgevingen waar Visual Studio nog niet is geïnstalleerd. MSBuild maakt gebruik van een XML-indeling voor projectbestanden die extensible en volledig wordt ondersteund door Microsoft. Met behulp van de MSBuild-bestandsindeling, kunt u aangeven welke items moeten worden gemaakt voor een of meer platformen en configuraties.

U kunt ook MSBuild uitvoeren op de opdrachtregel en in dit onderwerp beschrijft deze aanpak. Door het instellen van eigenschappen op de opdrachtregel, kunt u specifieke configuraties van een project maken. Op deze manier kunt u ook de doelen die MSBuild bouwt definiëren. Zie voor meer informatie over opdrachtregelparameters en MSBuild [MSBuild-Naslaggids](https://msdn.microsoft.com/library/ms164311.aspx).

## <a name="msbuild-parameters"></a>MSBuild-parameters
De eenvoudigste manier om een pakket te maken is om uit te voeren MSBuild met de `/t:Publish` optie. Met deze opdracht maakt standaard een map ten opzichte van de hoofdmap van het project, zoals `<ProjectDirectory>\bin\Configuration\app.publish\`. Wanneer u een Azure-project maakt, twee bestanden worden gegenereerd: het pakket van het bestand zelf en de bijbehorende configuratiebestand:

* Pakketbestand (`project.cspkg`)
* Configuratiebestand (`ServiceConfiguration.TargetProfile.cscfg`)

Elke Azure-project bevat standaard een service-configuratiebestand voor lokale (foutopsporing) builds en een andere voor builds in de cloud (fasering of productie). U kunt echter toevoegen of verwijderen van configuratie van de service bestanden indien nodig. Wanneer u een pakket vanuit Visual Studio maakt, wordt u gevraagd welke service-configuratiebestand om op te nemen samen met het pakket. Wanneer u een pakket met behulp van MSBuild bouwt, is het lokale service-configuratiebestand standaard opgenomen. Instellen als u wilt opnemen in een ander bestand in de configuratie van de service, de `TargetProfile` eigenschap van de opdracht MSBuild (`MSBuild /t:Publish /p:TargetProfile=ProfileName`).

Als u wilt met een alternatieve map voor de opgeslagen pakket en de configuratiebestanden, stelt u het pad met behulp van de `/p:PublishDir=Directory\` optie, met inbegrip van het afsluitende backslash-scheidingsteken.

## <a name="next-steps"></a>Volgende stappen
Nadat het pakket is gebouwd, kunt u deze implementeren naar Azure.