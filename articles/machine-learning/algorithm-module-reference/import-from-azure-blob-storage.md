---
title: 'Importeren uit Azure Blob-opslag: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Meer informatie over dat dit onderwerp wordt beschreven hoe u de importeren uit Azure Blob Storage-module gebruiken in Azure Machine Learning-service gegevens lezen uit Azure blob-opslag, zodat u de gegevens in een machine learning-experiment kunt gebruiken.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 4ac98516c1a326e1ede09bbb9660113ffd0642a0
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029683"
---
# <a name="import-from-azure-blob-storage-module"></a>Importeren uit Azure Blob Storage-module

Dit artikel beschrijft een module van de visuele interface (preview) voor Azure Machine Learning-service.

Gebruik deze module gegevens lezen uit Azure blob-opslag, zodat u de gegevens in een machine learning-experiment kunt gebruiken.  

De Azure Blob-Service is voor het opslaan van grote hoeveelheden gegevens, inclusief binaire gegevens. Azure-blobs kunnen worden geopend vanaf elke locatie en met behulp van HTTP of HTTPS. Mogelijk is verificatie vereist, afhankelijk van het type van de blob-opslag. 

- Openbare blobs zijn toegankelijk voor iedereen, of door gebruikers met een SAS-URL.
- Persoonlijke blobs is vereist voor een aanmelding en referenties.

Importeren van blob-opslag is vereist dat gegevens worden opgeslagen in blobs die gebruikmaken van de **blok-blob** indeling. De bestanden die zijn opgeslagen in de blob moeten door komma's gescheiden (CSV) of door tabs gescheiden (TSV) indelingen gebruiken. Wanneer u het bestand lezen, zijn de records en -koppen kenmerk van toepassing als rijen in het geheugen geladen als een gegevensset.


Het is raadzaam dat u uw gegevens voordat u importeert, profileren om ervoor te zorgen dat het schema is zoals verwacht. Het importproces scant een aantal head rijen om te bepalen van het schema, maar hoger rijen bevat mogelijk extra kolommen of gegevens die fouten veroorzaken.



## <a name="manually-set-properties-in-the-import-data-module"></a>Handmatig de eigenschappen instellen in de module gegevens importeren

De volgende stappen wordt beschreven hoe u het handmatig configureren van de bron voor het importeren.

1. Voeg de **importgegevens** module naar het experimentcanvas. U kunt deze module vinden in de interface, in de **gegevensinvoer en uitvoer**

2. Voor **gegevensbron**, selecteer **Azure Blob Storage**.

3. Voor **verificatietype**, kiest u **openbare (SAS-URL)** als u weet dat de informatie die is opgegeven als een openbare gegevensbron. Een SAS-URL is een tijdelijke URL voor openbare toegang die u genereren kunt met behulp van een Azure storage-hulpprogramma.

    Kies anders **Account**.

4. Als uw gegevens zich in een **openbare** blob die toegankelijk zijn met behulp van een SAS-URL, u hoeft geen aanvullende referenties omdat de URL-reeks bevat alle informatie die nodig is voor de verificatie en downloaden.

    In de **URI** veld typt of plakt u de volledige URI die het account en de openbare blob worden gedefinieerd.



5. Als uw gegevens zich in een **persoonlijke** -account, moet u referenties met inbegrip van de accountnaam en de sleutel opgeven.

    - Voor **accountnaam**typt of plakt u de naam van het account met de blob die u wilt openen.

        Bijvoorbeeld, als de volledige URL van het opslagaccount dat is `http://myshared.blob.core.windows.net`, typt u `myshared`.

    - Voor **accountsleutel**, plakt u de toegangssleutel voor opslag die is gekoppeld aan het account.

        Als u de toegangssleutel niet weet, Zie de sectie, "Azure storage-accounts beheren' in dit artikel: [Over Azure Storage-Accounts](https://docs.microsoft.com/azure/storage/storage-create-storage-account).

6. Voor **pad naar de container, map of blob**, typ de naam van de specifieke blob die u wilt ophalen.

    Bijvoorbeeld, als u een bestand met de naam geüpload **data01.csv** naar de container **trainingdata** in een account met de naam **mymldata**, zou de volledige URL voor het bestand: `http://mymldata.blob.core.windows.net/trainingdata/data01.txt` .

    Daarom in het veld **pad naar de container, map of blob**, typt u: `trainingdata/data01.csv`

    U kunt de jokertekens gebruiken voor het importeren van meerdere bestanden, `*` (sterretje) of `?` (vraagteken).

    Bijvoorbeeld, ervan uitgaande dat de container `trainingdata` bevat meerdere bestanden van een indeling die compatibel is, kan u de volgende specificatie gebruiken om te lezen van alle bestanden die beginnen met `data`, en samengevoegd tot één gegevensset:

    `trainingdata/data*.csv`

    U kunt geen jokertekens gebruiken in namen van containers. Als u importeren van bestanden uit meerdere containers wilt, gebruikt u een afzonderlijk exemplaar van de **gegevens importeren** -module voor elke container en vervolgens samenvoegen met de gegevenssets met behulp van de [rijen toevoegen](./add-rows.md) module.

    > [!NOTE]
    > Als u de optie hebt geselecteerd **gebruiken in de cache opgeslagen resultaten**, eventuele wijzigingen die u in de bestanden in de container aanbrengt een vernieuwing van de gegevens in het experiment niet activeren.

7. Voor **Blob-bestandsindeling**, selecteer een optie die aangeeft de indeling van de gegevens die zijn opgeslagen in de blob, dat zodat Azure Machine Learning kan de gegevens op de juiste wijze worden verwerkt. De volgende indelingen worden ondersteund:

    - **CSV**: Door komma's gescheiden waarden (CSV) zijn de standaardindeling voor de opslag voor exporteren en importeren van bestanden in Azure Machine Learning. Als de gegevens al een rij met koppen bevat, moet u de optie selecteert, **bestand heeft de rij met koppen**, of de header zullen worden behandeld als een gegevensrij.

       

    - **TSV**: Door tabs gescheiden waarden (TSV) zijn een indeling die wordt gebruikt door veel hulpmiddelen voor machine learning. Als de gegevens al een rij met koppen bevat, moet u de optie selecteert, **bestand heeft de rij met koppen**, of de header zullen worden behandeld als een gegevensrij.

       

    - **ARFF**: Deze indeling biedt ondersteuning voor importeren van bestanden in de indeling die wordt gebruikt door de toolset Weka. 

   

8. Voer het experiment uit.


## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) met Azure Machine Learning-service. 