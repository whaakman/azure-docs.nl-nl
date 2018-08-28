---
title: Fouten opsporen in Azure Data Lake Analytics code lokaal
description: Informatie over het gebruik van Azure Data Lake Tools voor Visual Studio fouten opsporen in U-SQL-taken op uw lokale werkstation.
services: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: jasonwhowell
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.topic: conceptual
ms.workload: big-data
ms.date: 07/03/2018
ms.openlocfilehash: 9e73fc4db1404842e6d3878d88d8f02511587bc9
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43044974"
---
# <a name="debug-azure-data-lake-analytics-code-locally"></a>Fouten opsporen in Azure Data Lake Analytics code lokaal

Net zoals u in de Azure Data Lake Analytics-service kunt, kunt u Azure Data Lake Tools voor Visual Studio voor het uitvoeren en fouten opsporen in Azure Data Lake Analytics-code op uw lokale werkstation.

Meer informatie over het [U-SQL-script uitvoeren op uw lokale computer](data-lake-analytics-data-lake-tools-local-run.md).

## <a name="debug-scripts-and-c-assemblies-locally"></a>Lokaal fouten opsporen in scripts en C#-assembly's

U kunt fouten opsporen C#-assembly's zonder te verzenden en ze naar de Azure Data Lake Analytics-service te registreren. U kunt onderbrekingspunten instellen in zowel de code-behind-bestand en een waarnaar wordt verwezen, C#-project.

### <a name="debug-local-code-in-a-code-behind-file"></a>Fouten opsporen in lokale code in een code-behind-bestand

1. Onderbrekingspunten instellen in de code-behind-bestand.
2. Selecteer **F5** fouten opsporen in het script lokaal.

> [!NOTE]
   > De volgende procedure werkt alleen in Visual Studio 2015. In oudere versies van Visual Studio, u moet wellicht handmatig toe te voegen de **PDB** bestanden.  
   >
   >

### <a name="debug-local-code-in-a-referenced-c-project"></a>Fouten opsporen in lokale code in een waarnaar wordt verwezen, C#-project

1. Maak een C#-assemblyproject en bouw het voor het genereren van de uitvoer **DLL** bestand.
2. Registreert de **DLL** bestand met behulp van een U-SQL-instructie:

        CREATE ASSEMBLY assemblyname FROM @"..\..\path\to\output\.dll";
        
3. Stel onderbrekingspunten in in de C#-code.
4. Selecteer **F5** fouten opsporen in het script door te verwijzen naar de C# **DLL** lokaal bestand.


## <a name="next-steps"></a>Volgende stappen

- Zie voor een voorbeeld van een complexere query [websitelogboeken analyseren met Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
- Taakdetails Zie [gebruik Job Browser en Job View voor Azure Data Lake Analytics-taken](data-lake-analytics-data-lake-tools-view-jobs.md).
- Als u wilt de vertex execution view gebruiken, Zie [de Vertex Execution View gebruiken in Data Lake Tools voor Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).
