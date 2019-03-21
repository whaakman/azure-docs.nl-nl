---
title: Doorlopend integreren en ontwikkelen met hulpprogramma's voor Stream Analytics
description: Dit artikel wordt beschreven hoe u Visual Studio-hulpprogramma's voor Azure Stream Analytics gebruikt voor het instellen van een continue integratie en implementatie.
services: stream-analytics
author: su-jie
ms.author: sujie
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 09/27/2017
ms.openlocfilehash: 641254be37ac0019ee6a256fc99f96fc3bfb75a2
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58122770"
---
# <a name="continuously-integrate-and-develop-with-stream-analytics-tools"></a>Doorlopend integreren en ontwikkelen met hulpprogramma's voor Stream Analytics
In dit artikel wordt beschreven hoe u de Azure Stream Analytics-hulpprogramma's voor Visual Studio gebruiken voor het instellen van een continue integratie en implementatie-proces.

Gebruik versie 2.3.0000.0 of hoger van [Stream Analytics-hulpprogramma's voor Visual Studio](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-tools-for-visual-studio) ondersteuning voor MSBuild krijgen.

Een NuGet-pakket is beschikbaar: [Microsoft.Azure.Stream Analytics.CICD](https://www.nuget.org/packages/Microsoft.Azure.StreamAnalytics.CICD/). Dit bevat de tools voor MSBuild, lokaal uitvoeren en implementatie die ondersteuning bieden voor het continue integratie- en implementatieproces van Stream Analytics Visual Studio-projecten. 
> [!NOTE]
> Het NuGet-pakket kan worden gebruikt alleen met de 2.3.0000.0 of hoger versie van Stream Analytics-hulpprogramma's voor Visual Studio. Hebt u projecten die zijn gemaakt in eerdere versies van Visual Studio-hulpprogramma's, alleen openen met de 2.3.0000.0 of hoger versie en opslaan. De nieuwe mogelijkheden zijn ingeschakeld. 

Zie voor meer informatie, [Stream Analytics-hulpprogramma's voor Visual Studio](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-tools-for-visual-studio).

## <a name="msbuild"></a>MSBuild
Als de standaard Visual Studio MSBuild-ervaring voor het bouwen van een project hebt u twee opties. U kunt met de rechtermuisknop op het project en kies vervolgens **bouwen**. U kunt ook gebruiken **MSBuild** in het NuGet-pakket vanaf de opdrachtregel.
```
./build/msbuild /t:build [Your Project Full Path] /p:CompilerTaskAssemblyFile=Microsoft.WindowsAzure.StreamAnalytics.Common.CompileService.dll  /p:ASATargetsFilePath="[NuGet Package Local Path]\build\StreamAnalytics.targets"

```

Wanneer een Stream Analytics Visual Studio-project met succes wordt gemaakt, genereert de volgende twee Azure Resource Manager sjabloonbestanden onder de **bin / [foutopsporing of stenen] / implementeren** map: 

*  Resource Manager-sjabloonbestand

       [ProjectName].JobTemplate.json 

*  Resource Manager-parameterbestand

       [ProjectName].JobTemplate.parameters.json   

Er zijn de standaardparameters in het parameters.json-bestand van de instellingen in Visual Studio-project. Als u implementeren naar een andere omgeving wilt, vervang u de parameters daarvan.

> [!NOTE]
> De referenties voor de standaardwaarden zijn ingesteld op null. U bent *vereist* de waarden instellen voordat u naar de cloud implementeren.

```json
"Input_EntryStream_sharedAccessPolicyKey": {
      "value": null
    },
```
Meer informatie over het [implementeren met een Resource Manager-sjabloonbestand en Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy). Meer informatie over het [een object gebruiken als een parameter in Resource Manager-sjabloon](https://docs.microsoft.com/azure/architecture/building-blocks/extending-templates/objects-as-parameters).

Voor het gebruik van beheerde identiteit voor Azure Data Lake Store Gen1 als uitvoer-sink, moet u toegang verlenen tot de service-principal met behulp van PowerShell voordat u Azure implementeert. Meer informatie over het [ADLS Gen1 met beheerde identiteit implementeren met Resource Manager-sjabloon](stream-analytics-managed-identities-adls.md#resource-manager-template-deployment).


## <a name="command-line-tool"></a>Opdrachtregel-hulpprogramma

### <a name="build-the-project"></a>Het project bouwen
Het NuGet-pakket is een opdrachtregelprogramma met de naam **SA.exe**. Het ondersteunt project bouwen en lokaal testen op een willekeurige computer, wat u in uw continue integratie en continue leveringsproces gebruiken kunt. 

De implementatiebestanden worden standaard in de huidige map geplaatst. U kunt het uitvoerpad opgeven met behulp van de volgende OutputPath - parameter:

```
./tools/SA.exe build -Project [Your Project Full Path] [-OutputPath <outputPath>] 
```

### <a name="test-the-script-locally"></a>Het script lokaal testen

Als uw project, lokale invoerbestanden in Visual Studio opgegeven heeft, kunt u een geautomatiseerd script test uitvoeren met behulp van de *localrun* opdracht. Het resultaat van de uitvoer is geplaatst in de huidige map.
 
```
localrun -Project [ProjectFullPath]
```

### <a name="generate-a-job-definition-file-to-use-with-the-stream-analytics-powershell-api"></a>Genereren van een taakdefinitiebestand voor gebruik met de PowerShell-API van Stream Analytics

De *arm* opdracht gebruikt u de taaksjabloon en de taak sjabloon parameterbestanden die worden gegenereerd door build als invoer. Vervolgens combineert het ze in een taak definitie JSON-bestand dat kan worden gebruikt met de PowerShell-API van Stream Analytics.

```powershell
arm -JobTemplate <templateFilePath> -JobParameterFile <jobParameterFilePath> [-OutputFile <asaArmFilePath>]
```
Voorbeeld:
```powershell
./tools/SA.exe arm -JobTemplate "ProjectA.JobTemplate.json" -JobParameterFile "ProjectA.JobTemplate.parameters.json" -OutputFile "JobDefinition.json" 
```


