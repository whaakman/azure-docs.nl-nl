---
title: Behouden van de resultaten of de logboeken van voltooide taken en taken aan een gegevensopslag - Azure Batch | Microsoft Docs
description: Meer informatie over verschillende opties voor permanente uitvoergegevens van de Batch-taken en taken. U kunt gegevens naar Azure Storage, of naar een ander gegevensarchief bewaard.
services: batch
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: 16e12d0e-958c-46c2-a6b8-7843835d830e
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 06/16/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b578abfa6fc0a10edc5daab40f8a0eea5e6653d9
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39115059"
---
# <a name="persist-job-and-task-output"></a>Taken persistent maken

[!INCLUDE [batch-task-output-include](../../includes/batch-task-output-include.md)]

Enkele algemene voorbeelden van uitvoer van de taak zijn:

- Bestanden die zijn gemaakt wanneer de invoergegevens van de taak-processen.
- De logboekbestanden die zijn gekoppeld aan de uitvoering van de taak. 

Dit artikel beschrijft de verschillende opties voor het persistent maken uitvoer van de taak en de scenario's waarvoor elke optie is het meest geschikt.   

## <a name="about-the-batch-file-conventions-standard"></a>Over de standaard Batch File Conventions

Batch definieert een optionele set van conventies voor het benoemen van bestanden van de taak uitvoer in Azure Storage. De [Batch File Conventions standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) beschrijving van deze overeenkomsten. De standaard File Conventions bepaalt de namen van de container- en blobnaam doelpad in Azure Storage voor een bepaalde uitvoerbestand op basis van de namen van de taak en de taak.

Het is aan u of u wilt gebruikmaken van de File Conventions-standaard voor het benoemen van uw gegevensbestanden uitvoer. U kunt ook de doelcontainer een naam en blob maar u wilt. Als u de standaard File Conventions gebruikt voor het benoemen van uitvoerbestanden, wordt de uitvoerbestanden zijn beschikbaar voor weergave in de [Azure-portal][portal].

Er zijn een aantal verschillende manieren waarop u de standaard File Conventions kunt gebruiken:

- Als u de API voor Batch-service gebruikt om vast te leggen van de uitvoerbestanden, kunt u de naam van bestemming containers en blobs op basis van de File Conventions-standaard. De Batch-service-API kunt u om vast te leggen van de uitvoerbestanden van clientcode, zonder te wijzigen van de taaktoepassing.
- Als u met .NET ontwikkelt, kunt u de [Azure Batch File Conventions-bibliotheek voor .NET][nuget_package]. Een voordeel van het gebruik van deze bibliotheek is dat het uitvoeren van query's de uitvoerbestanden op basis van hun-ID of het doel ondersteunt. De ingebouwde query functionaliteit kunt eenvoudig toegang tot de uitvoerbestanden van een clienttoepassing of van andere taken. Uw taaktoepassing moet echter worden gewijzigd om aan te roepen File Conventions-bibliotheek. Zie voor meer informatie, de referentie voor de [File Conventions-bibliotheek voor .NET](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.aspx).
- Als u met een andere taal dan .NET ontwikkelt, kunt u de standaard File Conventions implementeren in uw toepassing.

## <a name="design-considerations-for-persisting-output"></a>Overwegingen bij het ontwerp voor permanente uitvoer 

Bij het ontwerpen van uw Batch-oplossing, houd rekening met de volgende factoren met betrekking tot de taak en uitvoer.

* **Levensduur van COMPUTE**: Compute-knooppunten zijn vaak tijdelijk is, met name in groepen automatisch schalen is ingeschakeld. Uitvoer van een taak die wordt uitgevoerd op een knooppunt is alleen beschikbaar als het knooppunt bestaat, en alleen binnen de bewaarperiode van het bestand die u hebt ingesteld voor de taak. Als een taak geeft een resultaat die u moet uitvoeren mogelijk nadat de taak voltooid is, kunnen de taak moet de uitvoerbestanden uploaden naar een duurzame store, zoals Azure Storage.

* **Storage-uitvoer**: Azure Storage wordt aanbevolen als een gegevensarchief voor uitvoer van de taak, maar u kunt een duurzame opslag gebruiken. Uitvoer van de taak schrijven naar Azure Storage is geïntegreerd in de Batch-service-API. Als u een andere vorm van duurzame opslag gebruikt, moet u de toepassingslogica om persistent taak uitvoer zelf te schrijven.   

* **Het ophalen van de uitvoer**: U kunt uitvoer van de taak ophalen rechtstreeks vanuit de rekenknooppunten in uw pool, of vanuit Azure Storage of een ander gegevensarchief als u de uitvoer van de taak hebt opgeslagen. Als u wilt ophalen van de taak uitvoer rechtstreeks vanuit een compute-knooppunt, moet u de bestandsnaam en de uitvoerlocatie op het knooppunt. Als u de uitvoer van de taak naar Azure Storage zich blijven voordoen, moet u het volledige pad naar het bestand in Azure Storage voor het downloaden van de uitvoerbestanden met de Azure Storage SDK.

* **Uitvoer weergeven**: wanneer u gaat u naar een batchtaak in Azure portal en selecteer **bestanden op knooppunt**, krijgt u alle bestanden die zijn gekoppeld aan de taak, niet alleen de uitvoerbestanden u geïnteresseerd bent. Bestanden op de rekenknooppunten zijn weer beschikbaar terwijl het knooppunt bestaat en alleen in de bewaartijd voor bestanden die u hebt ingesteld voor de taak. Als u de uitvoer van de taak die u naar Azure Storage hebt opgeslagen, kunt u de Azure-portal of een Azure Storage client-toepassing, zoals de [Azure Storage Explorer][storage_explorer]. Als u wilt weergeven uitvoergegevens in Azure Storage met de portal of een ander hulpprogramma, moet u weet dat de locatie van het bestand en gaat u rechtstreeks naar deze.

## <a name="options-for-persisting-output"></a>Opties voor permanente uitvoer

Afhankelijk van uw scenario, moet u er een aantal verschillende manieren om vast te leggen van de uitvoer van de taak zijn:

- Gebruik de API voor Batch-service.  
- De Batch File Conventions-bibliotheek voor .NET gebruiken.  
- De standaard Batch File Conventions implementeren in uw toepassing.
- Implementeer een aangepast bestand gegevensverplaatsing-oplossing.

De volgende secties beschrijven elk benadering in meer detail.

### <a name="use-the-batch-service-api"></a>De Batch-service-API gebruiken

Met versie 2017-05-01, de Batch-service voegt ondersteuning toe voor uitvoerbestanden in Azure Storage voor de taakgegevens op te geven wanneer u [een taak toevoegen aan een taak](https://docs.microsoft.com/rest/api/batchservice/add-a-task-to-a-job) of [een verzameling taken toevoegen aan een job](https://docs.microsoft.com/rest/api/batchservice/add-a-collection-of-tasks-to-a-job).

De Batch-service-API biedt ondersteuning voor vastleggen van taakgegevens naar een Azure Storage-account van pools die zijn gemaakt met de configuratie van de virtuele machine. Met de Batch-service-API, kunt u taakgegevens behouden zonder te wijzigen van de toepassing die de taak wordt uitgevoerd. U kunt eventueel voldoen aan de [Batch File Conventions standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) voor het benoemen van bestanden die u bewaard in een Azure Storage. 

De Batch-service-API gebruiken om vast te leggen van de taak uitvoer wanneer:

- U wilt behouden van gegevens van Batch-taken en jobbeheertaken in pools die zijn gemaakt met de configuratie van de virtuele machine.
- U wilt dat het behoud van gegevens naar een Azure Storage-container met een willekeurige naam.
- U wilt dat het behoud van gegevens naar een Azure Storage-container met de naam volgens de [Batch File Conventions standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions). 

> [!NOTE]
> De API voor Batch-service biedt geen ondersteuning voor vastleggen van gegevens van de taken die worden uitgevoerd in pools die zijn gemaakt met de configuratie van de cloud-service. Zie voor meer informatie over permanente taak uitvoer van groepen met de cloud services-configuratie [taak en gegevens naar Azure Storage met de Batch File Conventions-bibliotheek voor .NET om vast te leggen behouden ](batch-task-output-file-conventions.md)
> 
> 

Zie voor meer informatie over permanente uitvoer van de taak met de Batch-service-API, [Persist taakgegevens naar Azure Storage met de Batch-service API](batch-task-output-files.md). Zie ook de [PersistOutputs] [ github_persistoutputs] voorbeeldproject op GitHub, die laat zien hoe u kunt de Batch-clientbibliotheek voor .NET gebruiken om vast te leggen van de taakuitvoer naar duurzame opslag.

### <a name="use-the-batch-file-conventions-library-for-net"></a>De Batch File Conventions-bibliotheek voor .NET gebruiken

Het bouwen van Batch-oplossingen met C# en .NET-ontwikkelaars kunnen gebruiken de [File Conventions-bibliotheek voor .NET] [ nuget_package] om vast te leggen gegevens van de taak met een Azure Storage-account volgens de [Batch-bestand Conventies standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions). De File Conventions-bibliotheek verplaatsen uitvoerbestanden naar Azure Storage en de naamgeving van bestemming containers en blobs in een bekende manier worden verwerkt.

De File Conventions-bibliotheek biedt ondersteuning voor een query uitvoeren op uitvoerbestanden op ID of doel, zodat u eenvoudig kunt terugvinden zonder het volledige bestand URI's. 

De Batch File Conventions-bibliotheek voor .NET gebruiken om vast te leggen van de taak uitvoer wanneer:

- Wilt u streaminggegevens naar Azure Storage terwijl de taak is nog steeds uitgevoerd.
- U wilt dat het behoud van gegevens van pools die zijn gemaakt met de cloudserviceconfiguratie of de configuratie van de virtuele machine.
- Uw clienttoepassing of andere taken in de taak moet om te zoeken en downloaden van de uitvoerbestanden van taak-ID of doel. 
- U wilt uploaden gecontroleerd of vroeg van eerste resultaten uitvoeren.
- U wilt weergeven van uitvoer van de taak in Azure portal.

Zie voor meer informatie over permanente uitvoer van de taak met de File Conventions-bibliotheek voor .NET, [taak en gegevens naar Azure Storage met de Batch File Conventions-bibliotheek voor .NET om vast te leggen persistent ](batch-task-output-file-conventions.md). Zie ook de [PersistOutputs] [ github_persistoutputs] voorbeeldproject op GitHub, die laat zien hoe u de File Conventions-bibliotheek voor .NET gebruikt om vast te leggen van de taakuitvoer naar duurzame opslag.

De [PersistOutputs] [ github_persistoutputs] voorbeeldproject op GitHub ziet u hoe u de Batch-clientbibliotheek voor .NET gebruiken om vast te leggen van de taakuitvoer naar duurzame opslag.

### <a name="implement-the-batch-file-conventions-standard"></a>De standaard Batch File Conventions implementeren

Als u van een andere taal dan .NET gebruikmaakt, kunt u implementeren de [Batch File Conventions standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) in uw eigen toepassing. 

U kunt voor het implementeren van de File Conventions-naamgevingsnorm zelf wanneer u een schematische naam bewezen, of wanneer u wilt weergeven van uitvoer van de taak in Azure portal.

### <a name="implement-a-custom-file-movement-solution"></a>Een aangepast bestand gegevensverplaatsing-oplossing implementeren

U kunt ook uw eigen volledig bestand gegevensverplaatsing-oplossing implementeren. Gebruik deze benadering wanneer:

- U wilt behouden van gegevens van de taak aan een ander gegevensarchief dan Azure Storage. Om bestanden te uploaden naar een gegevensarchief zoals Azure SQL of Azure Lake, kunt u een aangepast script of uitvoerbare bestand te uploaden naar die locatie. U kunt deze vervolgens aanroepen op de opdrachtregel na het uitvoeren van uw primaire uitvoerbaar bestand. Bijvoorbeeld op een Windows-knooppunt, kunt u deze twee opdrachten aanroepen: `doMyWork.exe && uploadMyFilesToSql.exe`
- U wilt uploaden gecontroleerd of vroeg van eerste resultaten uitvoeren.
- Wilt u nauwkeurige controle over foutafhandeling onderhouden. Bijvoorbeeld, kunt u voor het implementeren van uw eigen oplossing als u wilt gebruiken van afhankelijkheid Taakacties bepaalde uploaden maatregelen op basis van specifieke taakafsluitcodes. Zie voor meer informatie over afhankelijkheid Taakacties [taakafhankelijkheden voor het uitvoeren van taken die afhankelijk van andere taken zijn maken](batch-task-dependencies.md). 

## <a name="next-steps"></a>Volgende stappen

- Ontdek het gebruik van de nieuwe functies in de Batch-service-API om vast te leggen van de gegevens van de taak in [Persist taakgegevens naar Azure Storage met de Batch-service API](batch-task-output-files.md).
- Meer informatie over het gebruik van de Batch File Conventions-bibliotheek voor .NET in [behoud van de taak en gegevens naar Azure Storage met de Batch File Conventions-bibliotheek voor .NET](batch-task-output-file-conventions.md).
- Zie de [PersistOutputs] [ github_persistoutputs] voorbeeldproject op GitHub, die laat zien hoe u zowel de Batch-clientbibliotheek voor .NET en de File Conventions-bibliotheek voor .NET gebruiken om vast te leggen van de taakuitvoer naar duurzame opslag .

[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[portal]: https://portal.azure.com
[storage_explorer]: http://storageexplorer.com/
[github_persistoutputs]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/PersistOutputs 
