---
title: Installeer de duurzame functies extensie en steekproeven - Azure
description: Informatie over het installeren van de extensie duurzame functies voor Azure Functions voor portal ontwikkeling of Visual Studio-ontwikkeling.
services: functions
author: cgillum
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: debadde78d937bcd4ec1df665aacfd1887fbcd02
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="install-the-durable-functions-extension-and-samples-azure-functions"></a>Installeer de duurzame functies extensie en voorbeelden (Azure-functies)

De [duurzame functies](durable-functions-overview.md) -extensie voor Azure Functions is opgegeven in het NuGet-pakket [Microsoft.Azure.WebJobs.Extensions.DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask). Dit artikel laat zien hoe u het pakket en een reeks voorbeelden voor het volgende ontwikkelomgevingen installeert:

* Visual Studio 2017 (aanbevolen) 
* Azure Portal

## <a name="visual-studio-2017"></a>Visual Studio 2017

Visual Studio biedt momenteel de beste ervaring voor het ontwikkelen van apps die gebruikmaken van duurzame functies.  Uw functies kunnen lokaal worden uitgevoerd en kunnen ook worden gepubliceerd naar Azure. U kunt beginnen met een leeg project of met een reeks Voorbeeldfuncties.

### <a name="prerequisites"></a>Vereisten

* Installeer de [meest recente versie van Visual Studio](https://www.visualstudio.com/downloads/) (versie 15.3 of hoger). De Azure-hulpprogramma's in uw installatieopties bevatten.

### <a name="start-with-sample-functions"></a>Beginnen met Voorbeeldfuncties

1. Download de [Voorbeeldapp ZIP-bestand voor Visual Studio](https://azure.github.io/azure-functions-durable-extension/files/VSDFSampleApp.zip). U hoeft niet de NuGet-verwijzing toevoegen omdat het voorbeeldproject het al heeft.
2. Installeren en uitvoeren [Azure-Opslagemulator](https://docs.microsoft.com/azure/storage/storage-use-emulator) versie 5.2 of hoger. U kunt ook bijwerken de *local.appsettings.json* bestand met echte Azure Storage-verbindingsreeksen.
3. Open het project in Visual Studio 2017. 
4. Voor instructies over het uitvoeren van de steekproef beginnen met [werken chaining - reeks voorbeeld Hallo](durable-functions-sequence.md). Het voorbeeld worden lokaal uitvoeren of gepubliceerd naar Azure.

### <a name="start-with-an-empty-project"></a>Beginnen met een leeg project
 
Volg de aanwijzingen dezelfde als voor het beginnen met het voorbeeld, maar de volgende stappen uit te voeren in plaats van gedownload de *.zip* bestand:

1. Een functie-App-project maken.
2. De volgende NuGet-pakket verwijzing toevoegen aan uw *.csproj* bestand:

   ```xml
   <PackageReference Include="Microsoft.Azure.WebJobs.Extensions.DurableTask" Version="1.0.0-beta" />
   ```

## <a name="azure-portal"></a>Azure Portal

Als u liever, kunt u de Azure-portal voor het ontwikkelen van duurzame functies.

### <a name="create-an-orchestrator-function"></a>Een orchestrator-functie maken

1. Maak een nieuwe functie-app op [functions.azure.com](https://functions.azure.com/signin).
2. Configureren van de functie app [versie 2.0 runtime](functions-versions.md).
3. Maak een nieuwe functie en selecteer de **duurzame functies Orchestrator - C#** sjabloon.
4. Onder **extensies is niet geïnstalleerd**, klikt u op **installeren** downloaden van de uitbreiding van NuGet.org.

### <a name="copy-sample-code-to-the-function-app"></a>Voorbeeldcode kopiëren naar de functie-app

1. Download de [DFSampleApp.zip](https://github.com/Azure/azure-functions-durable-extension/raw/master/docfx/files/DFSampleApp.zip) bestand.
2. Pak het voorbeeldbestand in `D:\home\site\wwwroot` in de functie-app met behulp van de Kudu- of FTP.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Voer de functie sample-koppeling](durable-functions-sequence.md)
