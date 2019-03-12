---
title: Behouden van de resultaten of de logboeken van voltooide taken en taken aan een gegevensopslag - Azure Batch | Microsoft Docs
description: Meer informatie over verschillende opties voor permanente uitvoergegevens van de Batch-taken en taken. U kunt gegevens naar Azure Storage, of naar een ander gegevensarchief bewaard.
services: batch
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: 16e12d0e-958c-46c2-a6b8-7843835d830e
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 11/14/2018
ms.author: lahugh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bc579cd372616563b61e5ba04fe32612f3efb1c7
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57541244"
---
# <a name="persist-job-and-task-output"></a>Taken persistent maken

[!INCLUDE [batch-task-output-include](../../includes/batch-task-output-include.md)]

Enkele algemene voorbeelden van uitvoer van de taak zijn:

- Bestanden die zijn gemaakt wanneer de invoergegevens van de taak-processen.
- De logboekbestanden die zijn gekoppeld aan de uitvoering van de taak.

Dit artikel beschrijft de verschillende opties voor permanente taakuitvoer.

## <a name="options-for-persisting-output"></a>Opties voor permanente uitvoer

Afhankelijk van uw scenario, moet u er een aantal verschillende manieren om vast te leggen van de uitvoer van de taak zijn:

- [Gebruik de API voor Batch-service](batch-task-output-files.md).  
- [De Batch File Conventions-bibliotheek voor .NET gebruiken](batch-task-output-file-conventions.md).  
- De standaard Batch File Conventions implementeren in uw toepassing.
- Implementeer een aangepast bestand gegevensverplaatsing-oplossing.

De volgende secties korte beschrijving van elke methode, evenals algemene ontwerpoverwegingen voor permanente uitvoer.

### <a name="use-the-batch-service-api"></a>De Batch-service-API gebruiken

De Batch-service ondersteunt het opgeven van de uitvoerbestanden in Azure Storage voor de taakgegevens wanneer u [een taak toevoegen aan een taak](https://docs.microsoft.com/rest/api/batchservice/add-a-task-to-a-job) of [een verzameling taken toevoegen aan een job](https://docs.microsoft.com/rest/api/batchservice/add-a-collection-of-tasks-to-a-job).

Zie voor meer informatie over permanente uitvoer van de taak met de Batch-service-API, [Persist taakgegevens naar Azure Storage met de Batch-service API](batch-task-output-files.md).

### <a name="use-the-batch-file-conventions-library-for-net"></a>De Batch File Conventions-bibliotheek voor .NET gebruiken

Batch definieert een optionele set van conventies voor het benoemen van bestanden van de taak uitvoer in Azure Storage. De [Batch File Conventions standard](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/Batch/Support/FileConventions#conventions) beschrijving van deze overeenkomsten. De standaard File Conventions bepaalt de namen van de container- en blobnaam doelpad in Azure Storage voor een bepaalde uitvoerbestand op basis van de namen van de taak en de taak.

Het is aan u of u wilt gebruikmaken van de File Conventions-standaard voor het benoemen van uw gegevensbestanden uitvoer. U kunt ook de doelcontainer een naam en blob maar u wilt. Als u de standaard File Conventions gebruikt voor het benoemen van uitvoerbestanden, wordt de uitvoerbestanden zijn beschikbaar voor weergave in de [Azure-portal][portal].

Het bouwen van Batch-oplossingen met C# en .NET-ontwikkelaars kunnen gebruiken de [File Conventions-bibliotheek voor .NET] [ nuget_package] om vast te leggen gegevens van de taak met een Azure Storage-account volgens de [Batch-bestand Conventies standard](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/Batch/Support/FileConventions#conventions). De File Conventions-bibliotheek verplaatsen uitvoerbestanden naar Azure Storage en de naamgeving van bestemming containers en blobs in een bekende manier worden verwerkt.

Zie voor meer informatie over permanente uitvoer van de taak met de File Conventions-bibliotheek voor .NET, [behoud van de taak en gegevens naar Azure Storage met de Batch File Conventions-bibliotheek voor .NET](batch-task-output-file-conventions.md).

### <a name="implement-the-batch-file-conventions-standard"></a>De standaard Batch File Conventions implementeren

Als u van een andere taal dan .NET gebruikmaakt, kunt u implementeren de [Batch File Conventions standard](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/Batch/Support/FileConventions#conventions) in uw eigen toepassing.

U kunt voor het implementeren van de File Conventions-naamgevingsnorm zelf wanneer u een schematische naam bewezen, of wanneer u wilt weergeven van uitvoer van de taak in Azure portal.

### <a name="implement-a-custom-file-movement-solution"></a>Een aangepast bestand gegevensverplaatsing-oplossing implementeren

U kunt ook uw eigen volledig bestand gegevensverplaatsing-oplossing implementeren. Gebruik deze benadering wanneer:

- U wilt behouden van gegevens van de taak aan een ander gegevensarchief dan Azure Storage. Om bestanden te uploaden naar een gegevensarchief zoals Azure SQL of Azure Lake, kunt u een aangepast script of uitvoerbare bestand te uploaden naar die locatie. U kunt deze vervolgens aanroepen op de opdrachtregel na het uitvoeren van uw primaire uitvoerbaar bestand. Bijvoorbeeld op een Windows-knooppunt, kunt u deze twee opdrachten aanroepen: `doMyWork.exe && uploadMyFilesToSql.exe`
- U wilt uploaden gecontroleerd of vroeg van eerste resultaten uitvoeren.
- Wilt u nauwkeurige controle over foutafhandeling onderhouden. Bijvoorbeeld, kunt u voor het implementeren van uw eigen oplossing als u wilt gebruiken van afhankelijkheid Taakacties bepaalde uploaden maatregelen op basis van specifieke taakafsluitcodes. Zie voor meer informatie over afhankelijkheid Taakacties [taakafhankelijkheden voor het uitvoeren van taken die afhankelijk van andere taken zijn maken](batch-task-dependencies.md).

## <a name="design-considerations-for-persisting-output"></a>Overwegingen bij het ontwerp voor permanente uitvoer

Bij het ontwerpen van uw Batch-oplossing, houd rekening met de volgende factoren met betrekking tot de taak en uitvoer.

- **Levensduur van COMPUTE**: COMPUTE-knooppunten zijn vaak tijdelijk is, met name in groepen automatisch schalen is ingeschakeld. Uitvoer van een taak die wordt uitgevoerd op een knooppunt is alleen beschikbaar als het knooppunt bestaat, en alleen binnen de bewaarperiode van het bestand die u hebt ingesteld voor de taak. Als een taak geeft een resultaat die u moet uitvoeren mogelijk nadat de taak voltooid is, kunnen de taak moet de uitvoerbestanden uploaden naar een duurzame store, zoals Azure Storage.

- **Storage-uitvoer**: Azure-opslag als gegevensopslag voor uitvoer van de taak wordt aanbevolen, maar u kunt een duurzame opslag gebruiken. Uitvoer van de taak schrijven naar Azure Storage is geïntegreerd in de Batch-service-API. Als u een andere vorm van duurzame opslag gebruikt, moet u de toepassingslogica om persistent taak uitvoer zelf te schrijven.

- **Het ophalen van de uitvoer**: U kunt uitvoer van de taak ophalen rechtstreeks vanuit de rekenknooppunten in uw pool, of vanuit Azure Storage of een ander gegevensarchief als u de uitvoer van de taak hebt opgeslagen. Als u wilt ophalen van de taak uitvoer rechtstreeks vanuit een compute-knooppunt, moet u de bestandsnaam en de uitvoerlocatie op het knooppunt. Als u de uitvoer van de taak naar Azure Storage zich blijven voordoen, moet u het volledige pad naar het bestand in Azure Storage voor het downloaden van de uitvoerbestanden met de Azure Storage SDK.

- **Uitvoer weergeven**: Wanneer u gaat u naar een batchtaak in Azure portal en selecteer **bestanden op knooppunt**, krijgt u alle bestanden die zijn gekoppeld aan de taak, niet alleen de uitvoerbestanden u geïnteresseerd bent. Bestanden op de rekenknooppunten zijn weer beschikbaar terwijl het knooppunt bestaat en alleen in de bewaartijd voor bestanden die u hebt ingesteld voor de taak. Als u de uitvoer van de taak die u naar Azure Storage hebt opgeslagen, kunt u de Azure-portal of een Azure Storage client-toepassing, zoals de [Azure Storage Explorer][storage_explorer]. Als u wilt weergeven uitvoergegevens in Azure Storage met de portal of een ander hulpprogramma, moet u weet dat de locatie van het bestand en gaat u rechtstreeks naar deze.

## <a name="next-steps"></a>Volgende stappen

- Ontdek het gebruik van de nieuwe functies in de Batch-service-API om vast te leggen van de gegevens van de taak in [Persist taakgegevens naar Azure Storage met de Batch-service API](batch-task-output-files.md).
- Meer informatie over het gebruik van de Batch File Conventions-bibliotheek voor .NET in [behoud van de taak en gegevens naar Azure Storage met de Batch File Conventions-bibliotheek voor .NET](batch-task-output-file-conventions.md).
- Zie de [PersistOutputs] [ github_persistoutputs] voorbeeldproject op GitHub, die laat zien hoe u zowel de Batch-clientbibliotheek voor .NET en de File Conventions-bibliotheek voor .NET gebruiken om vast te leggen van de taakuitvoer naar duurzame opslag .

[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[portal]: https://portal.azure.com
[storage_explorer]: https://storageexplorer.com/
[github_persistoutputs]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/PersistOutputs 
