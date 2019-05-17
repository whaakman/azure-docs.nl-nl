---
title: Doorlopend integreren en ontwikkelen met Azure Stream Analytics CI/CD npm-pakket
description: In dit artikel wordt beschreven hoe u van Azure Stream Analytics CI/CD npm-pakket voor het instellen van een continue integratie en implementatie.
services: stream-analytics
author: su-jie
ms.author: sujie
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/15/2019
ms.openlocfilehash: 7fe46712d610d881c21653461d12e4f8efecb468
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2019
ms.locfileid: "65827873"
---
# <a name="continuously-integrate-and-develop-with-stream-analytics-cicd-npm-package"></a>Doorlopend integreren en ontwikkelen met Stream Analytics CI/CD npm-pakket
In dit artikel wordt beschreven hoe u de Azure Stream Analytics CI/CD npm-pakket gebruiken voor het instellen van een continue integratie en implementatie.

## <a name="build-the-vs-code-project"></a>Bouw het project VS Code

U kunt inschakelen met continue integratie en implementatie voor Azure Stream Analytics-taken met behulp van de **asa-streamanalytics-cicd** npm-pakket. De npm-pakket biedt de hulpprogramma's voor het genereren van Azure Resource Manager-sjablonen van [Stream Analytics Visual Studio Code-projecten](quick-create-vs-code.md). Het kan worden gebruikt in Windows, macOS en Linux zonder dat Visual Studio Code wordt geïnstalleerd.

Als u eenmaal hebt [het pakket hebt gedownload](https://www.npmjs.com/package/azure-streamanalytics-cicd), gebruik de volgende opdracht om uit te voeren van de Azure Resource Manager-sjablonen. Als de **outputPath** niet is opgegeven, de sjablonen worden geplaatst in de **implementeren** map onder van het project **bin** map.

```powershell
asa-cicd build -scriptPath <scriptFullPath> -outputPath <outputPath>
```

Wanneer een Stream Analytics Visual Studio Code-project met succes wordt gemaakt, genereert de volgende twee Azure Resource Manager sjabloonbestanden onder de **bin / [foutopsporing of stenen] / implementeren** map: 

*  Resource Manager-sjabloonbestand

       [ProjectName].JobTemplate.json 

*  Resource Manager-parameterbestand

       [ProjectName].JobTemplate.parameters.json   

Er zijn de standaardparameters in het parameters.json-bestand van de instellingen in Visual Studio-project. Als u implementeren naar een andere omgeving wilt, vervang u de parameters daarvan.

> [!NOTE]
> De referenties voor de standaardwaarden zijn ingesteld op null. U bent **vereist** de waarden instellen voordat u naar de cloud implementeren.

```json
"Input_EntryStream_sharedAccessPolicyKey": {
      "value": null
    },
```
Meer informatie over het [implementeren met een Resource Manager-sjabloonbestand en Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy). Meer informatie over het [een object gebruiken als een parameter in Resource Manager-sjabloon](https://docs.microsoft.com/azure/architecture/building-blocks/extending-templates/objects-as-parameters).

Voor het gebruik van beheerde identiteit voor Azure Data Lake Store Gen1 als uitvoer-sink, moet u toegang verlenen tot de service-principal met behulp van PowerShell voordat u Azure implementeert. Meer informatie over het [ADLS Gen1 met beheerde identiteit implementeren met Resource Manager-sjabloon](stream-analytics-managed-identities-adls.md#resource-manager-template-deployment).
## <a name="next-steps"></a>Volgende stappen

* [Snelstart: Een taak in de cloud Azure Stream Analytics maken in Visual Studio Code (Preview)](quick-create-vs-code.md)
* [Stream Analytics-query's met Visual Studio Code (Preview) lokaal testen](vscode-local-run.md)
* [Verken Azure Stream Analytics met Visual Studio Code (Preview)](vscode-explore-jobs.md)
