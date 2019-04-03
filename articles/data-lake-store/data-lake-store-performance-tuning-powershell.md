---
title: Richtlijnen voor het gebruik van Powershell met Azure Data Lake Storage Gen1 afstemmen van prestaties | Microsoft Docs
description: Tips over hoe u de prestaties verbeteren wanneer u Azure PowerShell met Azure Data Lake Storage Gen1
services: data-lake-store
documentationcenter: ''
author: stewu
manager: mtillman
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.date: 01/09/2018
ms.author: stewu
ms.openlocfilehash: 1c554b0eee844a632e6412b6f8a285c7a2573326
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58885312"
---
# <a name="performance-tuning-guidance-for-using-powershell-with-azure-data-lake-storage-gen1"></a>Richtlijnen voor het gebruik van PowerShell met Azure Data Lake Storage Gen1 afstemmen van prestaties

In dit artikel worden de eigenschappen die kunnen worden afgestemd om op te halen voor betere prestaties tijdens het werken met Azure Data Lake Storage Gen1 met behulp van PowerShell:

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="performance-related-properties"></a>Eigenschappen met betrekking tot prestaties

| Eigenschap            | Standaard | Beschrijving |
|---------------------|---------|-------------|
| PerFileThreadCount  | 10      | Met deze parameter kunt u het aantal parallelle threads voor het uploaden of downloaden van elk bestand kiezen. Dit getal staat voor het maximumaantal threads dat per bestand kunnen worden toegewezen, maar krijgt u mogelijk minder threads, afhankelijk van uw scenario (bijvoorbeeld, als u een bestand van 1 KB uploadt, krijgt u één thread, ook als u om 20 threads vraagt).  |
| ConcurrentFileCount | 10      | Deze parameter is specifiek bedoeld voor het uploaden en downloaden van mappen. De parameter bepaalt het aantal bestanden dat tegelijk kan worden geüpload of gedownload. Dit getal staat voor het maximum aantal bestanden dat kan worden geüpload of gedownload in één keer tegelijk, maar krijgt u mogelijk minder afhankelijk van uw scenario (bijvoorbeeld, als u twee bestanden uploadt, krijgt u twee bestanden tegelijk uploaden, zelfs als u vragen voor 15). |

**Voorbeeld**

Met deze opdracht downloadt bestanden van Data Lake Storage Gen1 naar de lokale schijf van de gebruiker met 20 threads per bestand en 100 bestanden tegelijk.

    Export-AzDataLakeStoreItem -AccountName <Data Lake Storage Gen1 account name> -PerFileThreadCount 20-ConcurrentFileCount 100 -Path /Powershell/100GB/ -Destination C:\Performance\ -Force -Recurse

## <a name="how-do-i-determine-the-value-for-these-properties"></a>Hoe bepaal ik welke waarden voor deze eigenschappen?

De volgende vraag die u mogelijk is het bepalen van welke waarde opgeven voor de eigenschappen die betrekking hebben op prestaties. Hier volgen een aantal richtlijnen.

* **Stap 1: Bepalen van het totale threadaantal** -u moet beginnen door het berekenen van het totale aantal threads te gebruiken. Als een algemene richtlijn, moet u zes threads voor elke fysieke kern.

        Total thread count = total physical cores * 6

    **Voorbeeld**

    We nemen hier aan dat u de PowerShell-opdrachten uitvoert vanaf een D14-VM met 16 kernen

        Total thread count = 16 cores * 6 = 96 threads


* **Stap 2: Bereken de PerFileThreadCount** -We berekenen de PerFileThreadCount op basis van de grootte van de bestanden. Voor bestanden die kleiner zijn dan 2,5 GB is er niet nodig voor deze parameter niet wijzigen omdat de standaardwaarde van 10 voldoende is. U moet voor bestanden die groter zijn dan 2,5 GB, gebruikt u 10 threads als basis voor de eerste 2,5 GB en 1 thread voor elke extra 256 MB toename in bestandsgrootte toevoegen. Als u een map kopieert met bestanden van zeer verschillende groottes, kunt u overwegen ze op vergelijkbare grootte te sorteren. Grote verschillen in bestandsgroottes kunnen tot slechtere prestaties leiden. Als het niet mogelijk is om bestanden op grootte te sorteren, dient u de PerFileThreadCount in te stellen op basis van het grootste bestand.

        PerFileThreadCount = 10 threads for the first 2.5 GB + 1 thread for each additional 256 MB increase in file size

    **Voorbeeld**

    Ervan uitgaande dat u 100 bestanden, variërend van 1 GB tot 10 GB hebt, gebruiken we de 10 GB als de maximale bestandsgrootte voor vergelijking, deze ziet er als volgt uit.

        PerFileThreadCount = 10 + ((10 GB - 2.5 GB) / 256 MB) = 40 threads

* **Stap 3: Bereken de ConcurrentFilecount** - gebruik van het totaalaantal threads en de PerFileThreadCount, ConcurrentFileCount te berekenen op basis van de volgende vergelijking:

        Total thread count = PerFileThreadCount * ConcurrentFileCount

    **Voorbeeld**

    We gaan uit van de gebruikte voorbeeldwaarden

        96 = 40 * ConcurrentFileCount

    De **ConcurrentFileCount** is **2,4**, wat we kunnen afronden naar **2**.

## <a name="further-tuning"></a>Verder afstemmen

Mogelijk is verder afstemmen nodig omdat u werkt met bestanden van uiteenlopende groottes. De bovenstaande berekening werkt goed als alle of de meeste van de bestanden ongeveer het bereik van 10 GB zijn. Als het echter bestanden van zeer uiteenlopende groottes betreft, waarbij veel bestanden kleiner zijn, kunt u de PerFileThreadCount verlagen. Als we de PerFileThreadCount verlagen, kunnen we de ConcurrentFileCount verhogen. Dus als gaan we ervan uit dat de meeste bestanden kleiner dan het bereik van 5 GB zijn, we onze berekening opnieuw uitvoeren kunnen:

    PerFileThreadCount = 10 + ((5 GB - 2.5 GB) / 256 MB) = 20

Dus **ConcurrentFileCount** 96/20, die 4.8 is, wordt afgerond op het **4**.

U kunt deze instellingen blijven aanpassen door de **PerFileThreadCount** te verhogen of te verlagen, afhankelijk van de verdeling van de bestandsgroottes.

### <a name="limitation"></a>Beperking

* **Aantal bestanden is lager dan de ConcurrentFileCount**: Als het aantal bestanden dat u uploadt kleiner dan is de **ConcurrentFileCount** dat u hebt berekend en minder **ConcurrentFileCount** moet gelijk zijn aan het aantal bestanden. U kunt eventuele overblijvende threads gebruiken om de **PerFileThreadCount** te verhogen.

* **Te veel threads**: Als u een threadaantal verhoogt te veel zonder de clustergrootte van uw verhogen, kunt u het risico van verminderde prestaties uitvoeren. Er treden mogelijk conflicten op wanneer u van context wisselt op de CPU.

* **Onvoldoende gelijktijdigheid**: Als de gelijktijdigheid onvoldoende is, is uw cluster mogelijk te klein. U kunt het aantal knooppunten in het cluster, waardoor u profiteert van meer gelijktijdigheid verhogen.

* **Beperkingsfouten**: U ziet u mogelijk beperkingsfouten optreden als uw gelijktijdigheid te hoog is. Als er beperkingsfouten optreden, dient u uw gelijktijdigheid te verkleinen of contact met ons op te nemen.

## <a name="next-steps"></a>Volgende stappen
* [Azure Data Lake Storage Gen1 gebruiken voor big data-vereisten](data-lake-store-data-scenarios.md) 
* [Bescherm uw gegevens in Data Lake Storage Gen1](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics gebruiken met Data Lake Storage Gen1](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight gebruiken met Data Lake Storage Gen1](data-lake-store-hdinsight-hadoop-use-portal.md)

