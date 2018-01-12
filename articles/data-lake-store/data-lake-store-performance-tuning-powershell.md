---
title: Prestaties afstemmen richtlijnen voor het gebruik van Powershell met Data Lake Store | Microsoft Docs
description: Tips voor het verbeteren van de prestaties wanneer Azure PowerShell gebruiken met Data Lake Store
services: data-lake-store
documentationcenter: 
author: stewu
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/09/2018
ms.author: stewu
ms.openlocfilehash: 63e1114d49b7bcb8910e8cd8205f10d1e8587f61
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="performance-tuning-guidance-for-using-powershell-with-azure-data-lake-store"></a>Prestaties afstemmen richtlijnen voor het gebruik van PowerShell met Azure Data Lake Store

In dit artikel worden de eigenschappen die kunnen worden afgestemd voor een betere prestaties bij het werken met Data Lake Store met behulp van PowerShell:

## <a name="performance-related-properties"></a>Eigenschappen die betrekking hebben op prestaties

| Eigenschap            | Standaard | Beschrijving |
|---------------------|---------|-------------|
| PerFileThreadCount  | 10      | Met deze parameter kunt u het aantal parallelle threads voor het uploaden of downloaden van elk bestand kiezen. Dit nummer geeft de maximale threads dat kunnen worden verdeeld per bestand, maar krijgt u mogelijk minder threads, afhankelijk van uw scenario (bijvoorbeeld, als u een 1 KB-bestand uploadt, krijgt u één thread zelfs als u voor 20 threads vragen).  |
| ConcurrentFileCount | 10      | Deze parameter is specifiek bedoeld voor het uploaden en downloaden van mappen. De parameter bepaalt het aantal bestanden dat tegelijk kan worden geüpload of gedownload. Deze waarde staat voor het maximum aantal gelijktijdige bestanden die kunnen worden geüpload of in één keer worden gedownload, maar krijgt u mogelijk minder gelijktijdigheid, afhankelijk van uw scenario (bijvoorbeeld, als u twee bestanden uploaden wilt, krijgt u twee gelijktijdige bestanden uploads zelfs als u vragen voor 15). |

**Voorbeeld**

Met deze opdracht worden bestanden vanuit Azure Data Lake Store naar de lokale schijf van de gebruiker gedownload met 20 threads per bestand en 100 bestanden tegelijk.

    Export-AzureRmDataLakeStoreItem -AccountName <Data Lake Store account name> -PerFileThreadCount 20-ConcurrentFileCount 100 -Path /Powershell/100GB/ -Destination C:\Performance\ -Force -Recurse

## <a name="how-do-i-determine-the-value-for-these-properties"></a>Hoe bepaal ik de waarde voor deze eigenschappen

De volgende vraag die moet u wellicht is het bepalen welke waarde opgeven voor de eigenschappen die betrekking hebben op prestaties. Hier volgen een aantal richtlijnen.

* **Stap 1: bepaal het totale threadaantal** - begin door uit te rekenen wat het totale threadaantal is dat u wilt gebruiken. Als een algemene richtlijn, moet u zes threads voor elke fysieke kernen.

        Total thread count = total physical cores * 6

    **Voorbeeld**

    We nemen hier aan dat u de PowerShell-opdrachten uitvoert vanaf een D14-VM met 16 kernen

        Total thread count = 16 cores * 6 = 96 threads


* **Stap 2: bereken de PerFileThreadCount** - we berekenen de PerFileThreadCount op basis van de grootte van de bestanden. Voor bestanden die kleiner is dan 2,5 GB hoeft niet te wijzigen van deze parameter omdat de standaardwaarde van 10 voldoende is. Voor bestanden groter zijn dan 2,5 GB, moet u 10 threads gebruiken als basis voor de eerste 2,5 GB en 1-thread voor elke extra 256 MB toename bestandsgrootte toevoegen. Als u een map kopieert met bestanden van zeer verschillende groottes, kunt u overwegen ze op vergelijkbare grootte te sorteren. Grote verschillen in bestandsgroottes kunnen tot slechtere prestaties leiden. Als het niet mogelijk is om bestanden op grootte te sorteren, dient u de PerFileThreadCount in te stellen op basis van het grootste bestand.

        PerFileThreadCount = 10 threads for the first 2.5 GB + 1 thread for each additional 256 MB increase in file size

    **Voorbeeld**

    Als er 100 bestanden variëren van 1 GB tot 10 GB, we 10 GB gebruiken als de maximale bestandsgrootte voor vergelijking, welke als volgt lezen zou.

        PerFileThreadCount = 10 + ((10 GB - 2.5 GB) / 256 MB) = 40 threads

* **Stap 3: Berekenen ConcurrentFilecount** - gebruik van het totale aantal threads en PerFileThreadCount ConcurrentFileCount berekenen op basis van de volgende vergelijking:

        Total thread count = PerFileThreadCount * ConcurrentFileCount

    **Voorbeeld**

    We gaan uit van de gebruikte voorbeeldwaarden

        96 = 40 * ConcurrentFileCount

    De **ConcurrentFileCount** is **2,4**, wat we kunnen afronden naar **2**.

## <a name="further-tuning"></a>Verder afstemmen

Mogelijk is verder afstemmen nodig omdat u werkt met bestanden van uiteenlopende groottes. De voorgaande berekening werkt goed als alle of de meeste van de bestanden groter en dichter bij het bereik van 10 GB zijn. Als het echter bestanden van zeer uiteenlopende groottes betreft, waarbij veel bestanden kleiner zijn, kunt u de PerFileThreadCount verlagen. Als we de PerFileThreadCount verlagen, kunnen we de ConcurrentFileCount verhogen. Dus als gaan we ervan uit dat de meeste van de bestanden kleinere binnen het bereik van 5 GB zijn, we onze berekening opnieuw uitvoeren kunnen:

    PerFileThreadCount = 10 + ((5 GB - 2.5 GB) / 256 MB) = 20

Dus **ConcurrentFileCount** 96/20, namelijk 4.8 wordt afgerond op **4**.

U kunt deze instellingen blijven aanpassen door de **PerFileThreadCount** te verhogen of te verlagen, afhankelijk van de verdeling van de bestandsgroottes.

### <a name="limitation"></a>Beperking

* **Het aantal bestanden is lager dan de ConcurrentFileCount**: als het aantal bestanden dat u uploadt lager is dan de **ConcurrentFileCount** die u hebt berekend, dient u de **ConcurrentFileCount** te verlagen tot deze gelijk is met het aantal bestanden. U kunt eventuele overblijvende threads gebruiken om de **PerFileThreadCount** te verhogen.

* **Te veel threads**: als u het aantal threads te veel verhoogt zonder dat u de grootte van uw cluster verhoogt, kan dit tot slechtere prestaties leiden. Er treden mogelijk conflicten op wanneer u van context wisselt op de CPU.

* **Onvoldoende gelijktijdigheid**: als de gelijktijdigheid onvoldoende is, is uw cluster mogelijk te klein. U kunt het aantal knooppunten in het cluster, waarmee u meer gelijktijdige uitvoering verhogen.

* **Beperkingsfouten**: er treden mogelijk beperkingsfouten op als uw gelijktijdigheid te hoog is. Als er beperkingsfouten optreden, dient u uw gelijktijdigheid te verkleinen of contact met ons op te nemen.

## <a name="next-steps"></a>Volgende stappen
* [Azure Data Lake Store gebruiken voor big data-vereisten](data-lake-store-data-scenarios.md) 
* [Gegevens in Data Lake Store beveiligen](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics gebruiken met Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight gebruiken met Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)

