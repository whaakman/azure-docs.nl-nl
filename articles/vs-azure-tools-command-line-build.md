---
title: Opdrachtregelprogramma build voor Azure | Microsoft Docs
description: Opdrachtregelprogramma build voor Azure
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 94b35d0d-0d35-48b6-b48b-3641377867fd
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/05/2017
ms.author: kraigb
ms.openlocfilehash: 5fe910e2757dd5ec783538e23e7f52e2f5725b39
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
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
Nadat het pakket is gebouwd, kunt u deze kunt implementeren naar Azure. Zie voor een zelfstudie die u laat zien hoe u kunt dit proces te automatiseren, [continue leveringsmethode voor Cloud-Services in Azure](./cloud-services/cloud-services-dotnet-continuous-delivery.md).

