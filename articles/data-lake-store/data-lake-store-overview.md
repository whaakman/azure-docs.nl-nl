---
title: Wat is Azure Data Lake Storage Gen1? | Microsoft Docs
description: Overzicht van Data Lake Storage Gen1 (voorheen bekend als Azure Data Lake Store) en de waarde die het biedt ten opzichte van andere gegevensarchieven
services: data-lake-store
author: twooley
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: twooley
ms.openlocfilehash: 99384374226fd89cfd672c6b4f851a1743db0764
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67118806"
---
# <a name="what-is-azure-data-lake-storage-gen1"></a>Wat is Azure Data Lake Storage Gen1?

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)]

Azure Data Lake Storage Gen1 is een bedrijfsbrede zeer grote opslagruimte voor big data-analysewerklasten. Met Azure Data Lake kunt u gegevens van elke grootte, type en opnamesnelheid vastleggen op één enkele locatie voor operationele en experimentele analyses.

Data Lake Storage Gen1 kan worden geopend via Hadoop (beschikbaar met HDInsight-cluster) met de WebHDFS compatibele REST-API. Het is ontworpen om te kunnen analyseren voor de opgeslagen gegevens en is afgestemd op prestaties voor scenario's met gegevensanalyses. Data Lake Storage Gen1 bevat alle mogelijkheden op bedrijfsniveau: beveiliging, beheerbaarheid, schaalbaarheid, betrouwbaarheid en beschikbaarheid.

![Azure Data Lake](./media/data-lake-store-overview/data-lake-store-concept.png)

## <a name="key-capabilities"></a>Belangrijkste mogelijkheden

Enkele van de belangrijkste mogelijkheden van Data Lake Storage Gen1 zijn de volgende.

### <a name="built-for-hadoop"></a>Gebouwd voor Hadoop

Data Lake Storage Gen1 is een Apache Hadoop-bestandssysteem dat compatibel is met Hadoop Distributed File System (HDFS) en werkt met het Hadoop-ecosysteem. Uw bestaande HDInsight-toepassingen of services die gebruikmaken van de API WebHDFS kunnen eenvoudig integreren met Data Lake Storage Gen1. Data Lake Storage Gen1 wordt ook aangegeven dat een WebHDFS compatibele REST-interface voor toepassingen.

U kunt eenvoudig analyseren gegevens die zijn opgeslagen in Data Lake Storage Gen1 met analytische frameworks van Hadoop zoals MapReduce of Hive. U kunt Azure HDInsight-clusters inrichten en deze voor rechtstreekse toegang tot gegevens die zijn opgeslagen in Data Lake Storage Gen1 configureren.

### <a name="unlimited-storage-petabyte-files"></a>Onbeperkte opslag, bestanden ter grootte van petabytes

Data Lake Storage Gen1 kan biedt onbeperkte opslag en een verscheidenheid aan gegevens voor analyse. Deze opleggen beperkingen met betrekking tot de grootte van accounts, bestanden of de hoeveelheid gegevens die kunnen worden opgeslagen in een data lake niet. Afzonderlijke bestanden kan uiteenlopen van een kilobyte tot petabytes in grootte. Gegevens worden blijvend opgeslagen door meerdere kopieën te maken. Er is geen limiet voor de tijdsduur waarvoor de gegevens kunnen worden opgeslagen in de data lake.

### <a name="performance-tuned-for-big-data-analytics"></a>Prestaties zijn afgestemd op big data-analyses

Data Lake Storage Gen1 is gebouwd voor het uitvoeren van grootschalige analytische systemen waarvoor grote doorvoer doorzoeken en analyseren van grote hoeveelheden gegevens vereist. De Data Lake verspreidt delen van een bestand over een aantal afzonderlijke opslagservers. Hiermee verbetert u de doorvoer wanneer het bestand in parallel wordt gelezen voor het uitvoeren van gegevensanalyse.

### <a name="enterprise-ready-highly-available-and-secure"></a>Geschikt voor bedrijven: Maximaal beschikbare en veilige

Data Lake Storage Gen1 biedt industriestandaard beschikbaarheid en betrouwbaarheid. Uw gegevensassets worden blijvend opgeslagen door het maken van redundante exemplaren ter bescherming tegen onverwachte fouten.

Data Lake Storage Gen1 biedt ook geavanceerde beveiliging voor de opgeslagen gegevens. Zie voor meer informatie, [gegevens beveiligen in Azure Data Lake Storage Gen1](#DataLakeStoreSecurity).

### <a name="all-data"></a>Alle gegevens

Data Lake Storage Gen1 kunt gegevens opslaan in de oorspronkelijke indeling zonder eerst om te zetten. Data Lake Storage Gen1 is niet een schema worden gedefinieerd voordat de gegevens zijn geladen, vereist daardoor kan het afzonderlijke analytische framework de gegevens interpreteren en een schema definiëren op het moment van de analyse. De mogelijkheid voor het opslaan van bestanden van verschillende groottes en indelingen is het mogelijk dat Data Lake Storage Gen1 verwerking van gestructureerde, semi-gestructureerde en ongestructureerde gegevens.

Data Lake Storage Gen1 containers voor gegevens zijn eigenlijk mappen en bestanden. U werkt met de opgeslagen gegevens met behulp van SDK's, de Azure-portal en Azure Powershell. Als u uw gegevens in het archief met behulp van deze interfaces en de juiste containers, kunt u elk type gegevens kunt opslaan. Data Lake Storage Gen1 voert geen speciale verwerking van gegevens op basis van het type gegevens dat wordt opgeslagen.

## <a name="DataLakeStoreSecurity"></a>Het beveiligen van gegevens

Data Lake Storage Gen1 gebruikt Azure Active Directory (Azure AD) voor verificatie en toegang toegangsbeheerlijsten (ACL's) voor het beheren van toegang tot uw gegevens.

| Functie | Description |
| --- | --- |
| Verificatie |Data Lake Storage Gen1 kan worden geïntegreerd met Azure AD voor identiteits-en toegangsbeheer voor alle gegevens die zijn opgeslagen in Data Lake Storage Gen1. Vanwege de integratie, Data Lake Storage Gen1 voordelen van alle Azure AD functie, zoals multi-factor authentication, voorwaardelijke toegang, op rollen gebaseerd toegangsbeheer, bewaking van toepassingsgebruik, beveiligingsbewaking en waarschuwingen, enzovoort. Data Lake Storage Gen1 ondersteunt het OAuth 2.0-protocol voor verificatie in de REST-interface. Zie [Data Lake Storage Gen1 verificatie](data-lakes-store-authentication-using-azure-active-directory.md).|
| Toegangsbeheer |Data Lake Storage Gen1 biedt toegangsbeheer door ondersteuning POSIX-machtigingen die worden weergegeven door het protocol WebHDFS. U kunt de ACL's voor de hoofdmap, submappen en afzonderlijke bestanden inschakelen. Zie voor meer informatie over de werking van ACL's in de context van Data Lake Storage Gen1 [toegangsbeheer in Data Lake Storage Gen1](data-lake-store-access-control.md). |
| Versleuteling |Data Lake Storage Gen1 biedt ook versleuteling voor gegevens die zijn opgeslagen in het account. U geeft de versleutelingsinstellingen op tijdens het maken van een Data Lake Storage Gen1-account. U kunt de gegevens te versleutelen of opt niet te versleutelen. Zie voor meer informatie, [versleuteling in Data Lake Storage Gen1](data-lake-store-encryption.md). Zie voor instructies over het bieden van versleuteling-gerelateerde configuratie [aan de slag met Data Lake Storage Gen1 met behulp van de Azure-portal](data-lake-store-get-started-portal.md). |

Zie voor instructies over het beveiligen van gegevens in Data Lake Storage Gen1 [gegevens beveiligen in Azure Data Lake Storage Gen1](data-lake-store-secure-data.md).

## <a name="application-compatibility"></a>Compatibiliteit van toepassingen

Data Lake Storage Gen1 is compatibel met de meeste open source-componenten in het Hadoop-ecosysteem. Het is ook geïntegreerd met andere Azure-services. Voor meer informatie over hoe u Data Lake Storage Gen1 met open source-componenten kunt gebruiken en andere Azure-services, gebruik de volgende koppelingen:

- Zie [toepassingen en services die compatibel zijn met Azure Data Lake Storage Gen1](data-lake-store-compatible-oss-other-applications.md) voor een lijst van open-sourcetoepassingen die compatibel met Data Lake Storage Gen1.
- Zie [integreren met andere Azure-services](data-lake-store-integrate-with-other-services.md) om te begrijpen hoe u Data Lake Storage Gen1 met andere Azure-services gebruikt om in te schakelen van een breder scala aan scenario's.
- Zie [scenario's voor het gebruik van Data Lake Storage Gen1](data-lake-store-data-scenarios.md) voor meer informatie over het gebruik van Data Lake Storage Gen1 in scenario's zoals het ophalen van gegevens, gegevens verwerken, downloaden van gegevens en gegevens te visualiseren.

## <a name="data-lake-storage-gen1-file-system"></a>Data Lake Storage Gen1 bestandssysteem

Data Lake Storage Gen1 kunnen worden geopend via het bestandssysteem AzureDataLakeFilesystem (adl: / /) in de Hadoop-omgevingen (beschikbaar met HDInsight-cluster). Toepassingen en services die gebruikmaken van adl: / / kunnen profiteren van verdere optimalisaties die niet op dit moment beschikbaar is in WebHDFS. Als gevolg hiervan Data Lake Storage Gen1 biedt u de flexibiliteit om een maken gebruik van de beste prestaties met de aanbevolen optie adl: / / of bestaande code door u verdergaat met het gebruik van de API WebHDFS rechtstreeks te beheren. Azure HDInsight maakt volledig gebruik van AzureDataLakeFilesystem om de beste prestaties bieden op Data Lake Storage Gen1.

U kunt toegang tot uw gegevens in met behulp van Data Lake Storage Gen1 `adl://<data_lake_storage_gen1_name>.azuredatalakestore.net`. Zie voor meer informatie over hoe u toegang tot de gegevens in Data Lake Storage Gen1 [eigenschappen van de opgeslagen gegevens weergeven](data-lake-store-get-started-portal.md#properties).

## <a name="next-steps"></a>Volgende stappen

- [Aan de slag met Data Lake Storage Gen1 met behulp van de Azure portal](data-lake-store-get-started-portal.md)
- [Aan de slag met Data Lake Storage Gen1 met .NET SDK](data-lake-store-get-started-net-sdk.md)
- [Azure HDInsight gebruiken met Data Lake Storage Gen1](data-lake-store-hdinsight-hadoop-use-portal.md)