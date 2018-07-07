---
title: Fouten opsporen in Azure Data Lake Analytics code lokaal | Microsoft Docs
description: Informatie over het gebruik van Azure Data Lake Tools voor Visual Studio fouten opsporen in U-SQL-taken op uw lokale werkstation.
services: data-lake-analytics
documentationcenter: ''
author: yanancai
manager: jhubbard
editor: cgronlun
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/03/2018
ms.author: yanacai
ms.openlocfilehash: 181512c12c1e72e6aa8205aabd5ea22816d4c5df
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37889540"
---
# <a name="debug-azure-data-lake-analytics-code-locally"></a>Fouten opsporen in Azure Data Lake Analytics code lokaal

Net zoals u in de Azure Data Lake-service kunt, kunt u Azure Data Lake Tools voor Visual Studio voor het uitvoeren en fouten opsporen in uw Azure Data Lake Analytics-code op uw werkstation.

Informatie over [U-SQL-script uitvoeren op uw lokale computer](data-lake-analytics-data-lake-tools-local-run.md).

## <a name="debug-scripts-and-c-assemblies-locally"></a>Lokaal fouten opsporen in scripts en C#-assembly's

U kunt fouten opsporen C#-assembly's zonder te verzenden en registreren in Azure Data Lake Analytics-Service. U kunt onderbrekingspunten instellen in zowel het onderliggende-codebestand als een C#-project waarnaar wordt verwezen.

### <a name="to-debug-local-code-in-code-behind-file"></a>Foutopsporing in lokale code in onderliggende-codebestand

1. Onderbrekingspunten instellen in het onderliggende-codebestand.
2. Druk op F5 om het script lokaal fouten opsporen in.

> [!NOTE]
   > De volgende procedure werkt alleen in Visual Studio 2015. In oudere Visual Studio-versies moet u mogelijk de PDB-bestanden handmatig toevoegen.  
   >
   >

### <a name="to-debug-local-code-in-a-referenced-c-project"></a>Fouten opsporen in lokale code in een C#-project waarnaar wordt verwezen

1. Maak een C#-assemblyproject en bouw het zo dat het de DLL-uitvoer genereert.
2. Registreer het DLL-bestand met een U-SQL-instructie:

        CREATE ASSEMBLY assemblyname FROM @"..\..\path\to\output\.dll";
        
3. Stel onderbrekingspunten in in de C#-code.
4. Druk op F5 om het script dat verwijst naar de C#-dll lokaal fouten opsporen in.


## <a name="next-steps"></a>Volgende stappen

- Zie voor een complexere query [websitelogboeken analyseren met Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
- Taakdetails Zie [gebruik Job Browser en Job View voor Azure Data Lake Analytics-taken](data-lake-analytics-data-lake-tools-view-jobs.md).
- Als u wilt de vertex execution view gebruiken, Zie [de Vertex Execution View gebruiken in Data Lake Tools voor Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).
