---
title: De resultaten of Logboeken van de voltooide taken en taken met een gegevensarchief - Azure Batch behouden | Microsoft Docs
description: Meer informatie over verschillende opties voor persistent maken uitvoergegevens van Batch-taken en taken. U kunt deze persistent maken van gegevens naar Azure Storage of naar een ander gegevensarchief.
services: batch
author: tamram
manager: timlt
editor: 
ms.assetid: 16e12d0e-958c-46c2-a6b8-7843835d830e
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 06/16/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3ca93e823f02b1483ed290cf89de191937d1e2c3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="persist-job-and-task-output"></a>Taken persistent maken

[!INCLUDE [batch-task-output-include](../../includes/batch-task-output-include.md)]

Algemene voorbeelden van de taakuitvoer zijn:

- Bestanden die zijn gemaakt wanneer de taak processen invoergegevens.
- De logboekbestanden die zijn gekoppeld aan de uitvoering van de taak. 

In dit artikel beschrijft de verschillende opties voor het persistent maken van de taakuitvoer en de scenario's waarvoor elke optie is het meest geschikt.   

## <a name="about-the-batch-file-conventions-standard"></a>Over de standaard conventies van Batch-bestand

Batch definieert een optionele set conventies voor het benoemen van bestanden van de taak uitvoer in Azure Storage. De [Batch bestand conventies standaard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) beschrijft deze overeenkomsten. De bestanden standaard bepaalt de namen van de container en blob doelpad in Azure Storage voor een bepaald op basis van de namen van de taak en -uitvoerbestand.

Het is aan u of u wilt gebruikmaken van de bestanden standaard voor de naamgeving van uw gegevens uitvoerbestanden. U kunt ook de doelcontainer een naam en blob echter kunt u. Als u de bestanden standaard gebruikt voor het benoemen van uitvoerbestanden, wordt de uitvoerbestanden zijn beschikbaar voor weergave in de [Azure-portal][portal].

Er zijn een aantal verschillende manieren waarmee u de standaard bestand conventies kunt:

- Als u de API van Batch-service gebruikt om vast te leggen van de uitvoerbestanden, kunt u de naam van doelbestand containers en blobs volgens de conventies bestand standaard. De API van Batch-service kunt u uitvoerbestanden van clientcode, zonder te wijzigen van uw taaktoepassing behouden.
- Als u met .NET ontwikkelt, kunt u de [bestand conventies van Azure Batch-bibliotheek voor .NET][nuget_package]. Een voordeel van het gebruik van deze bibliotheek is dat deze ondersteuning biedt voor het opvragen van de uitvoerbestanden volgens hun-ID of het doel. De ingebouwde query functionaliteit kunt eenvoudig toegang tot de uitvoerbestanden vanuit een clienttoepassing of van andere taken. Uw taaktoepassing moet echter worden gewijzigd om aan te roepen bestand Conventions-bibliotheek. Zie voor meer informatie de documentatie voor de [bestand Conventions-bibliotheek voor .NET](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.aspx).
- Als u met een andere taal dan .NET ontwikkelt, kunt u de bestanden standaard implementeren in uw toepassing.

## <a name="design-considerations-for-persisting-output"></a>Ontwerpoverwegingen voor het persistent maken uitvoer 

Overweeg de volgende factoren die betrekking hebben op de taak en uitvoer bij het ontwerpen van uw Batch-oplossing.

* **Levensduur van COMPUTE**: Compute knooppunten zijn vaak tijdelijke, met name in groepen voor automatisch schalen die zijn ingeschakeld. De uitvoer van een taak die wordt uitgevoerd op een knooppunt is alleen beschikbaar als het knooppunt bestaat en alleen binnen de bewaarperiode voor het bestand die u hebt ingesteld voor de taak. Als een taak wordt de uitvoer die u moet uitvoeren mogelijk nadat de taak voltooid is, kunnen de taak moet de uitvoerbestanden uploaden naar een duurzame store, zoals Azure Storage.

* **Opslag uitvoer**: Azure Storage wordt aanbevolen als gegevensopslag voor uitvoer van de taak, maar u kunt een duurzame opslag gebruiken. Uitvoer van de taak schrijven naar Azure Storage is geïntegreerd in de API van Batch-service. Als u een andere vorm van duurzame opslag gebruikt, moet u de toepassingslogica om te blijven behouden taak uitvoer zelf schrijven.   

* **Ophalen van de uitvoer**: U kunt uitvoer van de taak ophalen rechtstreeks vanuit de rekenknooppunten in uw pool, of van Azure Storage of een ander gegevensarchief als u taakuitvoer persistent hebt gemaakt. Voor het ophalen van de taak uitvoer rechtstreeks vanuit een rekenknooppunt, moet u de bestandsnaam en de uitvoerlocatie op het knooppunt. Als u de uitvoer van de taak naar Azure Storage zich blijven voordoen, moet u het volledige pad naar het bestand in Azure Storage voor het downloaden van de uitvoerbestanden met de Azure-opslag-SDK.

* **Uitvoer weergeven**: wanneer u naar een batchtaak in de Azure portal en selecteert u een navigeert **bestanden op knooppunt**, krijgt u alle bestanden die zijn gekoppeld aan de taak, niet alleen de uitvoerbestanden u geïnteresseerd bent in. Bestanden op rekenknooppunten zijn ook beschikbaar terwijl het knooppunt bestaat en alleen binnen de bewaartijd voor bestanden die u hebt ingesteld voor de taak. Als u wilt weergeven in de uitvoer van de taak die u hebt opgeslagen in Azure Storage, kunt u de Azure-portal of een Azure Storage client-toepassing, zoals de [Azure Opslagverkenner][storage_explorer]. Als u wilt weergeven uitvoergegevens in Azure Storage met de portal of een ander hulpprogramma, moet u weten wat de locatie van het bestand en navigeer naar het rechtstreeks.

## <a name="options-for-persisting-output"></a>Opties voor het persistent maken uitvoer

Afhankelijk van uw scenario moet u er een aantal verschillende manieren om vast te leggen van de uitvoer van de taak zijn:

- Gebruik de API van Batch-service.  
- Gebruik de Batch-bestand Conventions-bibliotheek voor .NET.  
- De Batch-bestand Conventions-standaard implementeren in uw toepassing.
- Een aangepast bestand gegevensverplaatsing oplossing implementeren.

De volgende secties wordt elke methode in meer detail beschreven.

### <a name="use-the-batch-service-api"></a>Gebruik de API van Batch-service

Met versie 2017-05-01, de Batch-service wordt ondersteuning toegevoegd voor uitvoerbestanden opgeven in Azure Storage voor taakgegevens wanneer u [een taak toevoegen aan een taak](https://docs.microsoft.com/rest/api/batchservice/add-a-task-to-a-job) of [een verzameling taken toevoegen aan een taak](https://docs.microsoft.com/rest/api/batchservice/add-a-collection-of-tasks-to-a-job).

De API van Batch-service ondersteunt vastleggen van taakgegevens naar een Azure Storage-account van groepen die zijn gemaakt met de configuratie van de virtuele machine. Met de API van Batch-service, kunt u taakgegevens behouden zonder te wijzigen van de toepassing die de taak wordt uitgevoerd. U kunt eventueel voldoen aan de [Batch bestand conventies standaard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) voor het benoemen van bestanden die u deze persistent naar Azure Storage maken. 

Gebruik de API van Batch-service om vast te leggen van de taak uitvoer wanneer:

- Wilt u gegevens van Batch-taken en jobbeheertaken in groepen die zijn gemaakt met de configuratie van de virtuele machine persistent maken.
- U wilt behouden van gegevens naar een Azure Storage-container met een willekeurige naam.
- U wilt behouden van gegevens naar een Azure Storage-container met de naam volgens de [Batch bestand conventies standaard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions). 

> [!NOTE]
> De API van Batch-service biedt geen ondersteuning voor vastleggen van gegevens van de taken die worden uitgevoerd in groepen die zijn gemaakt met de configuratie van de cloud-service. Zie voor meer informatie over persistent maken taak de uitvoer van groepen met de configuratie van de cloud-services [taak en gegevens naar Azure Storage met de Batch-bestand Conventions-bibliotheek voor .NET voor het persistent maken](batch-task-output-file-conventions.md)
> 
> 

Zie voor meer informatie over de taakuitvoer persistent maken met de API van Batch-service [Persist taakgegevens naar Azure Storage met de Batch-service API](batch-task-output-files.md). Zie ook het voorbeeldproject [PersistOutputs] [github_persistoutputs] op GitHub, die laat zien hoe u de Batch-clientbibliotheek voor .NET om vast te leggen van de uitvoer van de taak naar duurzame opslag.

### <a name="use-the-batch-file-conventions-library-for-net"></a>Gebruik de Batch-bestand Conventions-bibliotheek voor .NET

Batch-oplossingen met C# en .NET-ontwikkelaars kunnen gebruiken de [bestand Conventions-bibliotheek voor .NET] [ nuget_package] om vast te leggen taakgegevens naar een Azure Storage-account, volgens de [batchbestand Standaard conventies](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions). De bibliotheek bestand conventies verwerkt zwevend uitvoerbestanden naar Azure Storage en de namen van de bestemming containers en blobs in een bekende manier.

De bibliotheek bestand conventies biedt ondersteuning voor het opvragen van de uitvoerbestanden met ID of doel, zodat u gemakkelijk te vinden zonder het volledige bestand URI's. 

Gebruik de Batch-bestand Conventions-bibliotheek voor .NET om vast te leggen van de taak uitvoer wanneer:

- Wilt u van stroomgegevens naar Azure Storage terwijl de taak nog actief.
- Wilt u gegevens uit toepassingen die zijn gemaakt met de configuratie van de cloud-service of de configuratie van de virtuele machine persistent maken.
- Uw clienttoepassing of andere taken in de taak moet om te zoeken en downloaden van de taak uitvoerbestanden ID of met het doel. 
- U wilt uitvoeren van de controle te wijzen of vroeg uploaden van de eerste resultaten.
- U wilt weergeven van uitvoer van de taak in de Azure portal.

Zie voor meer informatie over persistent maken uitvoer van de taak met de bestand Conventions-bibliotheek voor .NET [taak en gegevens naar Azure Storage met de Batch-bestand Conventions-bibliotheek voor .NET om vast te leggen ](batch-task-output-file-conventions.md). Zie ook het voorbeeldproject [PersistOutputs] [github_persistoutputs] op GitHub, die laat zien hoe u het bestand Conventions-bibliotheek voor .NET om vast te leggen van de uitvoer van de taak naar duurzame opslag.

Het voorbeeldproject [PersistOutputs] [github_persistoutputs] op GitHub laat zien hoe u de Batch-clientbibliotheek voor .NET gebruikt om vast te leggen van de uitvoer van de taak naar duurzame opslag.

### <a name="implement-the-batch-file-conventions-standard"></a>De Batch-bestand Conventions-standaard implementeren

Als u een andere taal dan .NET gebruikt, kunt u implementeren de [Batch bestand conventies standaard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) in uw eigen toepassing. 

U kunt voor het implementeren van het bestand Conventions-naamgevingsnorm zelf wanneer u wilt dat een schematische naam beproefde of wanneer u wilt weergeven van uitvoer van de taak in de Azure portal.

### <a name="implement-a-custom-file-movement-solution"></a>Een aangepast bestand gegevensverplaatsing oplossing implementeren

U kunt ook uw eigen volledig bestand gegevensverplaatsing oplossing implementeren. Gebruik deze benadering wanneer:

- U wilt behouden taakgegevens naar een ander gegevensarchief dan Azure Storage. Om bestanden te uploaden naar een gegevensopslag zoals Azure SQL- of Azure DataLake, kunt u een aangepast script of uitvoerbaar bestand te uploaden naar die locatie. U kunt deze vervolgens aanroepen op de opdrachtregel na het uitvoeren van uw primaire uitvoerbaar bestand. Bijvoorbeeld op een knooppunt Windows kunt u deze twee opdrachten aanroepen:`doMyWork.exe && uploadMyFilesToSql.exe`
- U wilt uitvoeren van de controle te wijzen of vroeg uploaden van de eerste resultaten.
- Wilt u gedetailleerde controle over het afhandelen van fouten. U wilt bijvoorbeeld uw eigen oplossing implementeren als u wilt gebruiken van afhankelijkheid Taakacties bepaald op basis van specifieke taak afsluitcodes upload-acties te ondernemen. Zie voor meer informatie over afhankelijkheid Taakacties [Maak taakafhankelijkheden taken die afhankelijk van andere taken zijn uitvoeren](batch-task-dependencies.md). 

## <a name="next-steps"></a>Volgende stappen

- Verkennen met behulp van de nieuwe functies in de API van Batch-service voor het persistent maken van taakgegevens in [Persist taakgegevens naar Azure Storage met de Batch-service API](batch-task-output-files.md).
- Meer informatie over het gebruik van de Batch-bestand Conventions-bibliotheek voor .NET in [taak en gegevens naar Azure Storage met de Batch-bestand Conventions-bibliotheek voor .NET om vast te leggen ](batch-task-output-file-conventions.md).
- Zie het voorbeeldproject [PersistOutputs] [github_persistoutputs] op GitHub, die laat zien hoe u zowel de Batch-clientbibliotheek voor .NET en het bestand Conventions-bibliotheek voor .NET om vast te leggen van de uitvoer van de taak naar duurzame opslag.

[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[portal]: https://portal.azure.com
[storage_explorer]: http://storageexplorer.com/
