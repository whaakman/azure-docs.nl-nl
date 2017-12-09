---
title: Stream Analytics Visual Studio-hulpprogramma's gebruiken voor het instellen van het continue integratie en implementatie-proces | Microsoft Docs
description: Zelfstudie voor het gebruik van Stream Analytics Visual Studio-hulpprogramma's voor het instellen van een continu proces voor de integratie en implementatie
keywords: Visual studio NuGet, DevOps, CI/CD
documentationcenter: 
services: stream-analytics
author: su-jie
manager: 
editor: 
ms.assetid: a473ea0a-3eaa-4e5b-aaa1-fec7e9069f20
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 9/27/2017
ms.author: sujie
ms.openlocfilehash: 0fdc7bd1a98e147bf6c7f7c290f03a878c83a1a9
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="use-stream-analytics-visual-studio-tools-to-set-up-a-continuous-integration-and-deployment-process"></a>Stream Analytics Visual Studio-hulpprogramma's gebruiken voor het instellen van een continu proces voor de integratie en implementatie
In deze zelfstudie leert u hoe u de Azure Stream Analytics Visual Studio-hulpprogramma's voor het instellen van een continue integratie en implementatieproces.

De meest recente versie (2.3.0000.0 of hoger) van [Stream Analytics-tools voor Visual Studio](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-tools-for-visual-studio) voegt ondersteuning toe voor MSBuild.

Er is ook een nieuw uitgebrachte NuGet-pakket [Microsoft.Azure.Stream Analytics.CICD](https://www.nuget.org/packages/Microsoft.Azure.StreamAnalytics.CICD/). Het biedt de MSBuild, lokale uitvoering en implementatiehulpmiddelen die ondersteuning bieden voor het continue proces voor de integratie en implementatie van Stream Analytics Visual Studio-projecten. 
> [!NOTE] 
Het NuGet-pakket kan alleen met de 2.3.0000.0 of hoger dan de versie van Stream Analytics-hulpprogramma's voor Visual Studio worden gebruikt. Als u gemaakt in eerdere versies van Visual Studio tools projecten hebt, alleen openen met de 2.3.0000.0 of hoger dan versie en opslaan. Vervolgens wordt de nieuwe mogelijkheden zijn ingeschakeld. 

Meer informatie over het gebruik van de [Stream Analytics-tools voor Visual Studio](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-tools-for-visual-studio).

## <a name="msbuild"></a>MSBuild
Als de standaard Visual Studio MSBuild-ervaring als u wilt maken van een project hebt u twee opties. U kunt met de rechtermuisknop op het project en kies vervolgens **bouwen**. U kunt ook gebruiken **MSBuild** in het NuGet-pakket vanaf de opdrachtregel.
```
./build/msbuild /t:build [Your Project Full Path] /p:CompilerTaskAssemblyFile=Microsoft.WindowsAzure.StreamAnalytics.Common.CompileService.dll  /p:ASATargetsFilePath="[NuGet Package Local Path]\build\StreamAnalytics.targets"

```

Wanneer een Stream Analytics Visual Studio-project is gebaseerd, worden er gegenereerd met de volgende twee Azure Resource Manager sjabloonbestanden onder de **bin / [foutopsporing/Retail] / implementeren** map: 

*  Resource Manager-sjabloonbestand

       [ProjectName].JobTemplate.json 

*  Resource Manager-parameterbestand

       [ProjectName].JobTemplate.parameters.json   

De standaardparameters in het bestand parameters.json zijn uit de instellingen in Visual Studio-project. Als u implementeren op een andere omgeving wilt, vervangt u de parameters dienovereenkomstig in.

> [!NOTE] 
De referenties voor de standaardwaarden zijn ingesteld op null. U bent *vereist* de waarden instellen voordat u naar de cloud implementeert.

```json
"Input_EntryStream_sharedAccessPolicyKey": {
      "value": null
    },
```
Meer informatie over het [implementeren met een Resource Manager-sjabloonbestand en Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy). Meer informatie over het [gebruik van een object als parameter voor Resource Manager-sjabloon](https://docs.microsoft.com/azure/architecture/building-blocks/extending-templates/objects-as-parameters).


## <a name="command-line-tool"></a>Opdrachtregelprogramma

### <a name="build-the-project"></a>Het project bouwen
Het NuGet-pakket is een opdrachtregelprogramma aangeroepen **SA.exe**. Het ondersteunt project bouwen en lokaal testen op een willekeurige machine, kunt u in uw continue integratie en continue levering proces. 

De implementatie-bestanden worden standaard in de huidige map geplaatst. U kunt het uitvoerpad opgeven met behulp van de volgende - OutputPath-parameter:

```
./tools/SA.exe build -Project [Your Project Full Path] [-OutputPath <outputPath>] 
```

### <a name="test-the-script-locally"></a>Het script lokaal testen

Als uw project, lokale invoerbestanden in Visual Studio opgegeven heeft, kunt u een test geautomatiseerde scripts uitvoeren met behulp van de *localrun* opdracht. Het uitvoerresultaat wordt geplaatst in de huidige map.
 
```
localrun -Project [ProjectFullPath]
```

### <a name="generate-a-job-definition-file-to-use-with-the-stream-analytics-powershell-api"></a>Een taakdefinitiebestand voor gebruik met de Stream Analytics PowerShell API genereren

De *arm* opdracht gebruikt u de sjabloon en de taak parameter sjabloonbestanden gegenereerd via build als invoer. Vervolgens worden gecombineerd ze naar een taak definitie JSON-bestand dat kan worden gebruikt met de Stream Analytics PowerShell API.

```
arm -JobTemplate <templateFilePath> -JobParameterFile <jobParameterFilePath> [-OutputFile <asaArmFilePath>]
```
Voorbeeld:
```
./tools/SA.exe arm -JobTemplate "ProjectA.JobTemplate.json" -JobParameterFile "ProjectA.JobTemplate.parameters.json" -OutputFile "JobDefinition.json" 
```


