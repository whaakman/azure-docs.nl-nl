---
title: 'Gegevens importeren: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Informatie over het gebruik van de module gegevens importeren in Azure Machine Learning-service gegevens laden in een machine learning-experiment uit gegevens van bestaande cloudservices.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: ed51c4e7b6c7d226c7827d1ba00bc96a7be1e6b0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65028303"
---
# <a name="import-data-module"></a>Data-module importeren

Dit artikel beschrijft een module van de visuele interface (preview) voor Azure Machine Learning-service.

Gebruik deze module gegevens laden in een machine learning-experiment uit gegevens van bestaande cloudservices.  
De module biedt nu een wizard waarmee u kunt een storage-optie kiezen en selecteren uit bestaande abonnementen en accounts snel alle opties configureren. Wilt u een bestaande verbinding bewerken? Er is geen probleem. de wizard worden alle vorige configuratiedetails geladen zodat u hoeft helemaal opnieuw. 
  
Nadat u de gegevens die u wilt gebruiken en verbinding maken met de bron gedefinieerd [importgegevens](./import-data.md) het gegevenstype van elke kolom op basis van de waarden die deze bevat en de gegevens worden geladen in uw Azure Machine Learning-werkruimte. De uitvoer van [importgegevens](./import-data.md) is een gegevensset die kan worden gebruikt met een experiment.

  
Als de brongegevens zijn gewijzigd, kunt u de gegevensset vernieuwen en nieuwe gegevens toevoegen door opnieuw uit te voeren [importgegevens](./import-data.md). Echter, als u niet dat opnieuw te lezen uit de gegevensbron telkens wanneer u het experiment uitvoert wilt, selecteert u de **gebruiken in de cache opgeslagen resultaten** optie op ' True '. Wanneer deze optie is geselecteerd, wordt de module wordt gecontroleerd of het experiment is uitgevoerd op de eerder met behulp van dezelfde bron- en dezelfde opties als invoer. Als een vorige uitvoering wordt gevonden, worden de gegevens in de cache wordt gebruikt, in plaats van het laden van de gegevens van de bron.
 

## <a name="data-sources"></a>Gegevensbronnen

De module gegevens importeren ondersteunt de volgende gegevensbronnen. Klik op de koppelingen voor gedetailleerde instructies en voorbeelden van het gebruik van elke gegevensbron. 
 
Als u niet zeker weet hoe of wanneer u gegevens moet opslaan, ziet u deze handleiding voor algemene scenario's voor gegevens in het data science process:  [Scenario's voor geavanceerde analyses in Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-plan-sample-scenarios). 


|Gegevensbron| Gebruiken met|
|-----------|-----------|  
|[Web-URL via HTTP](./import-from-web-url-via-http.md)|Gegevens die wordt gehost op een web-URL die gebruikmaakt van HTTP en die is opgegeven in de CSV, TSV, ARFF of SvmLight indelingen ophalen|  
|[Importeren uit Azure Blob-opslag](./import-from-azure-blob-storage.md) |Gegevens die zijn opgeslagen in de Azure blob-service ophalen|  

## <a name="how-to-use-import-data"></a>Het gebruik van gegevens importeren
 
1. Voeg de **importgegevens** module naar het experimentcanvas. U vindt deze module in de **gegevensinvoer en uitvoer** categorie in de interface.

2. Klik op **Start de Wizard gegevens importeren** het configureren van de gegevensbron met behulp van een wizard.

    De wizard haalt de accountnaam en de referenties en de help u andere opties configureren. Als u een bestaande configuratie bewerken wilt, laadt het de huidige waarden eerst.

3. Als u niet gebruiken van de wizard wilt, klikt u op **gegevensbron**, en kies het type cloud-gebaseerde opslag u leest uit. 

    Aanvullende instellingen zijn afhankelijk van het type opslag die u kiest, en of de opslag wordt beveiligd of niet. Mogelijk moet u de accountnaam, het bestandstype of de referenties. Sommige bronnen hoeven niet-authenticatie. voor andere resources moet u mogelijk kent de accountnaam, een sleutel of de containernaam.

4. Selecteer de **gebruiken in de cache opgeslagen resultaten** optie als u wilt de gegevensset gebruiken voor opeenvolgende wordt uitgevoerd in de cache.

    Ervan uitgaande dat er zijn geen andere wijzigingen aangebracht in de moduleparameters, laadt het experiment de gegevens alleen de eerste keer de module wordt uitgevoerd, en daarna maakt gebruik van de versie van een cache van de gegevensset.

    Schakel deze optie als u wilt de gegevens opnieuw laden telkens wanneer die u het experiment uitvoert.

5. Voer het experiment uit.

    Bij het importeren van gegevens de gegevens in de interface is geladen, wordt het gegevenstype van elke kolom op basis van de waarden die het bevat, numerieke of categorisch.

    - Als een header aanwezig is, wordt de header om de naam van de kolommen van de uitvoergegevensset gebruikt.

    - Als er geen bestaande kolomkoppen in de gegevens, worden nieuwe kolomnamen gegenereerd met behulp van de indeling col1, col2... , coln *.

## <a name="results"></a>Resultaten

Bij het importeren is voltooid, klikt u op de uitvoergegevensset en selecteer **Visualize** om te zien als de gegevens is geïmporteerd.

Als u opslaan van de gegevens opnieuw kunt gebruiken wilt, in plaats van het importeren van een nieuwe set gegevens telkens wanneer het experiment wordt uitgevoerd, met de rechtermuisknop op de uitvoer en selecteert u **opslaan als gegevensset**. Kies een naam voor de gegevensset. De opgeslagen gegevensset behoudt de gegevens op het moment van opslaan en gegevens worden niet bijgewerkt wanneer het experiment opnieuw wordt uitgevoerd, zelfs als de gegevensset in het experiment wordt gewijzigd. Dit kan zijn handig voor het maken van momentopnamen van gegevens.

Nadat de gegevens zijn geïmporteerd, moet het mogelijk enkele aanvullende voorbereidingen voor het maken van modellering en analyse:


- Gebruik [metagegevens bewerken](./edit-metadata.md) te wijzigen van kolomnamen, voor het afhandelen van een kolom als een ander gegevenstype, of om aan te geven dat sommige kolommen zijn labels of functies.

- Gebruik [Select Columns in Dataset](./select-columns-in-dataset.md) om te selecteren van een subset van kolommen te transformeren of in het model te gebruiken. De getransformeerde of verwijderde kolommen kunnen eenvoudig worden deze naar de oorspronkelijke gegevensset met behulp van de [kolommen toevoegen](./add-columns.md) module.  

- Gebruik [partitie en steekproef](./partition-and-sample.md) verdelen van de gegevensset, steekproeven, of de bovenste n rijen ophalen.

## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) met Azure Machine Learning-service. 