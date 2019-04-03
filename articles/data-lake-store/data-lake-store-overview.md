---
title: Overzicht van Azure Data Lake Storage Gen1 | Microsoft Docs
description: Inzicht in welke Data Lake Storage Gen1 is (voorheen bekend als Azure Data Lake Store) en de waarde die het biedt ten opzichte van andere gegevensarchieven
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: twooley
ms.openlocfilehash: 438eab091fac103b66f0789beca0098b87ee44cd
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58885652"
---
# <a name="overview-of-azure-data-lake-storage-gen1"></a>Overzicht van Azure Data Lake Storage Gen1

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)]

Azure Data Lake Storage Gen1 is een bedrijfsbrede zeer grote opslagruimte voor big data-analysewerklasten. Met Azure Data Lake kunt u gegevens van elke grootte, type en opnamesnelheid vastleggen op één enkele locatie voor operationele en experimentele analyses.

> [!TIP]
> Gebruik de [leertraject voor Data Lake Storage Gen1](https://azure.microsoft.com/documentation/learning-paths/data-lake-store-self-guided-training/) om te beginnen met het verkennen van de Data Lake Storage Gen1-service.
> 
> 

Data Lake Storage Gen1 kan worden geopend via Hadoop (beschikbaar met HDInsight-cluster) met de WebHDFS compatibele REST-API. Het is speciaal ontworpen om de opgeslagen gegevens te kunnen analyseren en het is afgestemd op prestaties voor scenario's met gegevensanalyses. Het bevat out-of-the-box alle mogelijkheden op bedrijfsniveau: beveiliging, beheerbaarheid, schaalbaarheid, betrouwbaarheid en beschikbaarheid. Dit zijn allemaal essentiële factoren voor zakelijke gebruiksvoorbeelden.

![Azure Data Lake](./media/data-lake-store-overview/data-lake-store-concept.png)

Enkele van de belangrijkste mogelijkheden van Data Lake Storage Gen1 zijn de volgende.

### <a name="built-for-hadoop"></a>Gebouwd voor Hadoop
Data Lake Storage Gen1 is een Apache Hadoop-bestandssysteem dat compatibel is met Hadoop Distributed File System (HDFS) en werkt met het Hadoop-ecosysteem.  Uw bestaande HDInsight-toepassingen of services die gebruikmaken van de API WebHDFS kunnen eenvoudig integreren met Data Lake Storage Gen1. Data Lake Storage Gen1 wordt ook aangegeven dat een WebHDFS compatibele REST-interface voor toepassingen

Gegevens die zijn opgeslagen in Data Lake Storage Gen1 kunnen eenvoudig worden geanalyseerd met analytische frameworks van Hadoop zoals MapReduce of Hive. Microsoft Azure HDInsight-clusters kunnen worden ingericht en geconfigureerd voor rechtstreekse toegang tot gegevens die zijn opgeslagen in Data Lake Storage Gen1.

### <a name="unlimited-storage-petabyte-files"></a>Onbeperkte opslag, bestanden ter grootte van petabytes
Data Lake Storage Gen1 biedt onbeperkte opslag en is geschikt voor het opslaan van een verscheidenheid aan gegevens voor analyse. Het legt geen limieten op voor de grootte van accounts of bestanden, of de hoeveelheid gegevens die kunnen worden opgeslagen in een Data Lake. De grootte van afzonderlijke bestanden kan uiteenlopen van een kilobyte tot petabytes, waardoor het een uitstekende keuze is voor het opslaan van elk type gegevens. Gegevens worden blijvend opgeslagen door meerdere kopieën te maken en er is geen limiet voor de tijdsduur waarin de gegevens kunnen worden opgeslagen in de Data Lake.

### <a name="performance-tuned-for-big-data-analytics"></a>Prestaties zijn afgestemd op big data-analyses
Data Lake Storage Gen1 is gebouwd voor het uitvoeren van grootschalige analytische systemen waarvoor grote doorvoer doorzoeken en analyseren van grote hoeveelheden gegevens vereist. De Data Lake verspreidt delen van een bestand over een aantal afzonderlijke opslagservers. Hiermee verbetert u de doorvoer wanneer het bestand in parallel wordt gelezen voor het uitvoeren van gegevensanalyse.

### <a name="enterprise-ready-highly-available-and-secure"></a>Enterprise-ready: Maximaal beschikbare en veilige
Data Lake Storage Gen1 biedt industriestandaard beschikbaarheid en betrouwbaarheid. Uw gegevensassets worden blijvend opgeslagen door het maken van redundante exemplaren ter bescherming tegen onverwachte fouten. Ondernemingen kunnen Data Lake Storage Gen1 in hun oplossingen gebruiken als een belangrijk onderdeel van hun bestaande gegevensplatform.

Data Lake Storage Gen1 biedt ook geavanceerde beveiliging voor de opgeslagen gegevens. Zie voor meer informatie, [gegevens beveiligen in Azure Data Lake Storage Gen1](#DataLakeStoreSecurity).

### <a name="all-data"></a>Alle gegevens
Data Lake Storage Gen1 kunnen alle gegevens worden opgeslagen in de oorspronkelijke indeling, zoals ze gemaakt zijn, zonder eerst om te zetten. Data Lake Storage Gen1 is niet een schema worden gedefinieerd voordat de gegevens zijn geladen, vereist daardoor kan het afzonderlijke analytische framework de gegevens interpreteren en een schema definiëren op het moment van de analyse. De mogelijkheid voor het opslaan van bestanden van verschillende groottes en indelingen, maakt het mogelijk voor Data Lake Storage Gen1 verwerking van gestructureerde, semi-gestructureerde en ongestructureerde gegevens.

Data Lake Storage Gen1 containers voor gegevens zijn eigenlijk mappen en bestanden. U werkt met de opgeslagen gegevens met behulp van SDK's, Azure Portal en Azure Powershell. Als u uw gegevens in het archief plaats via deze interfaces en de juiste containers gebruikt, kunt u allerlei soorten gegevens opslaan. Data Lake Storage Gen1 voert geen speciale verwerking van gegevens op basis van het type gegevens dat wordt opgeslagen.

## <a name="DataLakeStoreSecurity"></a>Het beveiligen van gegevens in Data Lake Storage Gen1
Data Lake Storage Gen1 gebruikt Azure Active Directory voor verificatie en toegangsbeheerlijsten (ACL's) voor het beheren van toegang tot uw gegevens.

| Functie | Description |
| --- | --- |
| Authentication |Data Lake Storage Gen1 kan worden geïntegreerd met Azure Active Directory (AAD) voor identiteits- en toegangsbeheer voor alle gegevens die zijn opgeslagen in Data Lake Storage Gen1. Als gevolg van de integratie, Data Lake Storage Gen1 voordelen van alle AAD-functies zoals multi-factor authentication, voorwaardelijke toegang, op rollen gebaseerd toegangsbeheer, bewaking van toepassingsgebruik, beveiligingsbewaking en waarschuwingen, enzovoort. Data Lake Storage Gen1 ondersteunt het OAuth 2.0-protocol voor verificatie in de REST-interface. Zie [Data Lake Storage Gen1 verificatie](data-lakes-store-authentication-using-azure-active-directory.md)|
| Toegangsbeheer |Data Lake Storage Gen1 biedt toegangsbeheer door ondersteuning POSIX-machtigingen die worden weergegeven door het protocol WebHDFS. ACL's kunnen worden ingeschakeld voor de hoofdmap, submappen en afzonderlijke bestanden. Zie voor meer informatie over de werking van ACL's in de context van Data Lake Storage Gen1 [toegangsbeheer in Data Lake Storage Gen1](data-lake-store-access-control.md). |
| Versleuteling |Data Lake Storage Gen1 biedt ook versleuteling voor gegevens die zijn opgeslagen in het account. U geeft de versleutelingsinstellingen op tijdens het maken van een Data Lake Storage Gen1-account. U kunt ervoor kiezen de gegevens te versleutelen of niet te versleutelen. Zie voor meer informatie, [versleuteling in Data Lake Storage Gen1](data-lake-store-encryption.md). Zie voor instructies over het bieden van versleuteling-gerelateerde configuratie [aan de slag met Azure Data Lake Storage Gen1 met behulp van de Azure-Portal](data-lake-store-get-started-portal.md). |

Wilt u meer informatie over het beveiligen van gegevens in Data Lake Storage Gen1? Volg dan de onderstaande links.

* Zie voor instructies over het beveiligen van gegevens in Data Lake Storage Gen1 [gegevens beveiligen in Azure Data Lake Storage Gen1](data-lake-store-secure-data.md).
* Kijkt u liever naar video’s? [Bekijk deze video](https://mix.office.com/watch/1q2mgzh9nn5lx) over het beveiligen van gegevens die zijn opgeslagen in Data Lake Storage Gen1.

## <a name="applications-compatible-with-data-lake-storage-gen1"></a>Toepassingen die compatibel zijn met Data Lake Storage Gen1
Data Lake Storage Gen1 is compatibel met de meeste open source-onderdelen in het Hadoop-ecosysteem. Het kan ook goed worden geïntegreerd in andere Azure-services. Hierdoor kunt u Data Lake Storage Gen1 een perfecte optie voor uw opslagbehoeften. Volg de onderstaande koppelingen voor meer informatie over hoe Data Lake Storage Gen1 zowel met open source-onderdelen, evenals andere Azure-services kunnen worden gebruikt.

* Zie [toepassingen en services die compatibel zijn met Azure Data Lake Storage Gen1](data-lake-store-compatible-oss-other-applications.md) voor een lijst van open source-toepassingen die compatibel met Data Lake Storage Gen1.
* Zie [integreren met andere Azure-services](data-lake-store-integrate-with-other-services.md) om te begrijpen hoe Data Lake Storage Gen1 met andere Azure-services kan worden gebruikt om in te schakelen van een breder scala aan scenario's.
* Zie [scenario's voor het gebruik van Data Lake Storage Gen1](data-lake-store-data-scenarios.md) voor meer informatie over het gebruik van Data Lake Storage Gen1 in scenario's zoals het ophalen van gegevens, gegevens verwerken, downloaden van gegevens en gegevens te visualiseren.

## <a name="what-is-data-lake-storage-gen1-file-system-adl"></a>Wat is Data Lake Storage Gen1-bestandssysteem (adl: / /)?
Data Lake Storage Gen1 kunnen worden geopend via het nieuwe bestandssysteem, het AzureDataLakeFilesystem (adl: / /), in Hadoop-omgevingen (beschikbaar met HDInsight-cluster). Toepassingen en services die gebruikmaken van adl:// kunnen profiteren van verdere optimalisatie van prestaties die op dit moment niet beschikbaar is in WebHDFS. Als gevolg hiervan, Data Lake Storage Gen1 biedt u de flexibiliteit om te behalen de beste prestaties met de aanbevolen optie adl: / / of bestaande code door u verdergaat met het gebruik van de API WebHDFS rechtstreeks te beheren. Azure HDInsight maakt volledig gebruik van AzureDataLakeFilesystem om de beste prestaties bieden op Data Lake Storage Gen1.

U kunt toegang tot uw gegevens in met behulp van Data Lake Storage Gen1 `adl://<data_lake_storage_gen1_name>.azuredatalakestore.net`. Zie voor meer informatie over hoe u toegang tot de gegevens in Data Lake Storage Gen1 [eigenschappen van de opgeslagen gegevens weergeven](data-lake-store-get-started-portal.md#properties)

## <a name="next-steps"></a>Volgende stappen

* [Aan de slag met Data Lake Storage Gen1 met behulp van de Azure-Portal](data-lake-store-get-started-portal.md)
* [Aan de slag met Azure Data Lake Storage Gen1 met .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Azure HDInsight gebruiken met Data Lake Storage Gen1](data-lake-store-hdinsight-hadoop-use-portal.md)