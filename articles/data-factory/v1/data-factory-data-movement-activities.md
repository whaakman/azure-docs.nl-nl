---
title: Verplaatsen van gegevens met behulp van de Kopieeractiviteit | Microsoft Docs
description: 'Meer informatie over de gegevensverplaatsing in Data Factory-pijplijnen: gegevensmigratie tussen winkels cloud en tussen een winkel lokale en een cloud-opslag. Gebruik de kopieerbewerking.'
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 67543a20-b7d5-4d19-8b5e-af4c1fd7bc75
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: jingwang
robots: noindex
ms.openlocfilehash: dff87d41df2bdb5439785846c9653f2f0be2b40d
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="move-data-by-using-copy-activity"></a>Verplaatsen van gegevens met behulp van kopieerbewerking
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](data-factory-data-movement-activities.md)
> * [Versie 2 - Preview](../copy-activity-overview.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [Kopieeractiviteit in V2](../copy-activity-overview.md).

## <a name="overview"></a>Overzicht
In Azure Data Factory, kunt u Kopieeractiviteit gebruiken voor het kopiëren van gegevens tussen on-premises en cloud gegevensarchieven. Nadat de gegevens worden gekopieerd, worden deze verder getransformeerd en geanalyseerd. U kunt ook Kopieeractiviteit gebruiken om de transformatie en analyseresultaten voor business intelligence (BI) en het verbruik van de toepassing te publiceren.

![Rol van kopieerbewerking](media/data-factory-data-movement-activities/copy-activity.png)

Kopieeractiviteit wordt mogelijk gemaakt door een veilige, betrouwbare en schaalbare, en [wereldwijd beschikbare service](#global). In dit artikel biedt details over de gegevensverplaatsing in Data Factory en Kopieeractiviteit.

Eerst gaan we kijken hoe gegevens migreren tussen twee gegevensarchieven voor cloud en tussen een on-premises gegevensopslag en cloud gegevensopslag plaatsvindt.

> [!NOTE]
> Zie voor meer informatie over activiteiten in het algemeen, [Understanding pijplijnen en activiteiten](data-factory-create-pipelines.md).
>
>

### <a name="copy-data-between-two-cloud-data-stores"></a>Kopiëren van gegevens tussen twee gegevensarchieven voor cloud
Wanneer de bron- en sink gegevensarchieven zich in de cloud, gaat u Kopieeractiviteit via de volgende fasen om gegevens te kopiëren van de bron voor de sink. De service die ook door de Kopieeractiviteit:

1. Leest de gegevens uit de gegevensopslag van de bron.
2. Serialisatie/deserialisatie, compressie/decompressie, toewijzingskolommen uitvoert en typ conversie. Deze bewerkingen op basis van de configuraties van de invoergegevensset, uitvoergegevensset en Kopieeractiviteit gebeurt.
3. Schrijft gegevens naar het gegevensarchief van de bestemming.

De service kiest automatisch het optimale regio uit te voeren van de verplaatsing van gegevens. Deze regio is meestal een die het dichtst bij het sink-gegevensarchief.

![Exemplaar van cloud-naar-cloud](./media/data-factory-data-movement-activities/cloud-to-cloud.png)

### <a name="copy-data-between-an-on-premises-data-store-and-a-cloud-data-store"></a>Kopiëren van gegevens tussen een on-premises gegevensopslag en cloud-gegevensopslag
Veilig om gegevens te verplaatsen tussen een on-premises gegevensopslag en cloud gegevensopslag, Data Management Gateway te installeren op uw on-premises machine. Data Management Gateway is een agent waarmee de verplaatsing van gegevens voor hybride en verwerking. U kunt deze installeren op dezelfde computer als de gegevens zelf opslaan of op een afzonderlijke computer die toegang tot het gegevensarchief heeft.

In dit scenario Data Management Gateway voert de serialisatie/deserialisatie, compressie/decompressie, toewijzingskolommen en typ conversie. Gegevens wordt niet uitgebreid via de Azure Data Factory-service. Data Management Gateway schrijft de gegevens in plaats daarvan rechtstreeks naar het doelarchief.

![On-premises-naar-cloudtoepassing kopiëren](./media/data-factory-data-movement-activities/onprem-to-cloud.png)

Zie [gegevens verplaatsen tussen on-premises en cloud gegevensarchieven](data-factory-move-data-between-onprem-and-cloud.md) voor een inleiding en stapsgewijze instructies. Zie [Data Management Gateway](data-factory-data-management-gateway.md) voor gedetailleerde informatie over deze agent.

U kunt ook gegevens die worden ondersteund van/naar gegevensarchieven die worden gehost op Azure IaaS virtuele machines (VM's) met behulp van Data Management Gateway verplaatsen. In dit geval kunt u Data Management Gateway installeren op dezelfde virtuele machine als de gegevens zelf opslaan of op een afzonderlijke virtuele machine die toegang tot het gegevensarchief heeft.

## <a name="supported-data-stores-and-formats"></a>Ondersteunde gegevensarchieven en indelingen
De kopieeractiviteit in Data Factory kopieert gegevens van een brongegevensarchief naar een sinkgegevensarchief. Data Factory ondersteunt de volgende gegevensarchieven. Gegevens vanuit elke willekeurige bron kunnen naar een sink worden geschreven. Klik op een gegevensarchief voor informatie over het kopiëren van gegevens naar en van dat archief.

> [!NOTE] 
> Als u gegevens wilt verplaatsen naar/van een gegevensarchief dat niet wordt ondersteund door de kopieeractiviteit, gebruikt u een **aangepaste activiteit** in Data Factory met uw eigen logica voor het kopiëren/verplaatsen van gegevens. Zie [Aangepaste activiteiten gebruiken in een Azure Data Factory-pijplijn](data-factory-use-custom-activities.md) voor meer informatie over het maken en gebruiken van een aangepaste activiteit.

[!INCLUDE [data-factory-supported-data-stores](../../../includes/data-factory-supported-data-stores.md)]

> [!NOTE]
> Gegevensarchieven met een * kunnen zich on-premises of op Azure IaaS bevinden. Hiervoor moet u [Data Management Gateway](data-factory-data-management-gateway.md) installeren op een on-premises/Azure IaaS-computer.

### <a name="supported-file-formats"></a>Ondersteunde bestandsindelingen
U kunt een Kopieeractiviteit om te **kopiëren van bestanden als-is** tussen twee bestandsgebaseerde gegevensarchieven, kunt u overslaan de [sectie opgemaakt](data-factory-create-datasets.md) in zowel de definities van de invoer en uitvoer gegevensset. De gegevens efficiënt gekopieerd zonder een serialisatie/deserialisatie.

Kopieeractiviteit ook leest uit en schrijft naar bestanden in de opgegeven indelingen: **tekst, JSON Avro, ORC en parketvloeren**, en compressiecodec **GZip, Deflate, BZip2 en ZipDeflate** worden ondersteund. Zie [ondersteunde indelingen voor bestands- en compressie](data-factory-supported-file-and-compression-formats.md) met details.

U kunt bijvoorbeeld de volgende kopie activiteiten doen:

* Gegevens kopiëren in on-premises SQL-Server en schrijven naar Azure Data Lake Store ORC-indeling.
* Kopieer de bestanden in tekstindeling (CSV) van de on-premises bestandssysteem en schrijven naar Azure Blob Avro-indeling.
* ZIP-bestanden kopiëren van on-premises bestandssysteem en decomprimeren vervolgens land naar Azure Data Lake Store.
* Kopiëren van gegevens in de GZip gecomprimeerde tekst (CSV)-indeling van Azure Blob en schrijven naar Azure SQL Database.

## <a name="global"></a>Algemeen beschikbaar gegevensverplaatsing
Azure Data Factory is alleen beschikbaar in de regio's VS-West, VS-Oost en Noord-Europa. De service die ook door de kopieerbewerking is echter beschikbaar globaal in de volgende regio's en -locaties. De topologie wereldwijd beschikbare zorgt ervoor dat efficiënt gegevensverplaatsing die meestal regio-overschrijdende hops voorkomt. Zie [-Services per regio](https://azure.microsoft.com/regions/#services) voor beschikbaarheid van de Gegevensfactory en de verplaatsing van gegevens in een regio.

### <a name="copy-data-between-cloud-data-stores"></a>Gegevens kopiëren tussen gegevensarchieven cloud
Wanneer zowel bron- en sink gegevensarchieven in de cloud zijn, Data Factory maakt gebruik van een service-implementatie in de regio die het dichtst bij de sink in het hetzelfde Geografie om de gegevens te verplaatsen. Raadpleeg de volgende tabel voor toewijzing:

| Geografie van de bestemming gegevensarchieven | De regio van het gegevensarchief bestemming | Regio gebruikt voor gegevensverplaatsing |
|:--- |:--- |:--- |
| Verenigde Staten | VS - oost | VS - oost |
| &nbsp; | VS - oost 2 | VS - oost 2 |
| &nbsp; | VS - midden | VS - midden |
| &nbsp; | Noord-centraal VS | Noord-centraal VS |
| &nbsp; | Zuid-centraal VS | Zuid-centraal VS |
| &nbsp; | West-centraal VS | West-centraal VS |
| &nbsp; | VS - west | VS - west |
| &nbsp; | VS - west 2 | VS - west 2 |
| Canada | Canada - oost | Canada - midden |
| &nbsp; | Canada - midden | Canada - midden |
| Brazilië | Brazilië - zuid | Brazilië - zuid |
| Europa | Noord-Europa | Noord-Europa |
| &nbsp; | West-Europa | West-Europa |
| Verenigd Koninkrijk | Verenigd Koninkrijk West | Verenigd Koninkrijk Zuid |
| &nbsp; | Verenigd Koninkrijk Zuid | Verenigd Koninkrijk Zuid |
| Azië en Stille Oceaan | Zuidoost-Azië | Zuidoost-Azië |
| &nbsp; | Oost-Azië | Zuidoost-Azië |
| Australië | Australië - oost | Australië - oost |
| &nbsp; | Australië - zuidoost | Australië - zuidoost |
| India | Centraal-India | Centraal-India |
| &nbsp; | West-India | Centraal-India |
| &nbsp; | Zuid-India | Centraal-India |
| Japan | Japan - oost | Japan - oost |
| &nbsp; | Japan - west | Japan - oost |
| Korea | Korea - centraal | Korea - centraal |
| &nbsp; | Korea - zuid | Korea - centraal |

U kunt ook kunt u expliciet de regio van de Data Factory-service moet worden gebruikt voor het uitvoeren van de kopie door te geven aangeven `executionLocation` eigenschap onder Kopieeractiviteit `typeProperties`. Ondersteunde waarden voor deze eigenschap hierboven worden vermeld **regio gebruikt voor gegevensverplaatsing** kolom. Opmerking: uw gegevens die regio via de kabel tijdens het kopiëren doorloopt. Bijvoorbeeld: voor het kopiëren tussen Azure worden opgeslagen in Zuid-Korea, kunt u opgeven `"executionLocation": "Japan East"` voor doorsturen via Japan regio (Zie [steekproef JSON](#by-using-json-scripts) als verwijzing).

> [!NOTE]
> Als de regio van het gegevensarchief bestemming niet in de voorgaande lijst is of kan niet worden getraceerd, standaard Kopieeractiviteit niet worden uitgevoerd in plaats van via een andere regio, tenzij `executionLocation` is opgegeven. De lijst met ondersteunde regio zal worden uitgebreid gedurende een bepaalde periode.
>

### <a name="copy-data-between-an-on-premises-data-store-and-a-cloud-data-store"></a>Kopiëren van gegevens tussen een on-premises gegevensopslag en cloud-gegevensopslag
Wanneer gegevens worden gekopieerd tussen on-premises (of Azure virtuele machines/IaaS) en in de archieven, cloud [Data Management Gateway](data-factory-data-management-gateway.md) verplaatsing van gegevens op een lokale machine of virtuele machine uitvoert. De gegevens wordt niet uitgebreid via de service in de cloud, tenzij u de [kopie gefaseerde](data-factory-copy-activity-performance.md#staged-copy) mogelijkheid. In dit geval loopt gegevens via de fasering Azure Blob-opslag voordat deze worden geschreven naar het gegevensarchief sink.

## <a name="create-a-pipeline-with-copy-activity"></a>Een pijplijn maken met de kopieerbewerking
U kunt een pijplijn maken met de Kopieeractiviteit in een aantal manieren:

### <a name="by-using-the-copy-wizard"></a>Met behulp van de Wizard kopiëren
De Data Factory-Wizard kopiëren kunt u een pijplijn maken met de Kopieeractiviteit. Deze pijplijn, kunt u gegevens uit ondersteunde bronnen kopiëren naar bestemmingen *zonder te schrijven JSON* definities voor de gekoppelde services, gegevenssets en pijplijnen. Zie [Data Factory-Wizard kopiëren](data-factory-copy-wizard.md) voor meer informatie over de wizard.  

### <a name="by-using-json-scripts"></a>Met behulp van JSON-scripts
U kunt Data Factory-Editor in de Azure-portal, Visual Studio of Azure PowerShell gebruiken voor het maken van een JSON-definitie voor een pijplijn (via Kopieeractiviteit). Vervolgens implementeert u deze voor het maken van de pijplijn in Data Factory. Zie [zelfstudie: gebruik Kopieeractiviteit in een Azure Data Factory-pijplijn](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor een zelfstudie met stapsgewijze instructies.    

JSON-eigenschappen (zoals naam, beschrijving, invoer en uitvoer tabellen en -beleid) zijn beschikbaar voor alle typen activiteiten. Eigenschappen die beschikbaar zijn in de `typeProperties` sectie van de activiteit variëren met elk activiteitstype.

Voor de Kopieeractiviteit, de `typeProperties` sectie varieert afhankelijk van de soorten gegevensbronnen en de sinks. Klik op een bron/sink in de [ondersteunde gegevensbronnen en put](#supported-data-stores-and-formats) sectie voor informatie over de type-eigenschappen die ondersteuning biedt voor Kopieeractiviteit voor die gegevensopslag.

Hier volgt een voorbeeld JSON-definitie:

```json
{
  "name": "ADFTutorialPipeline",
  "properties": {
    "description": "Copy data from Azure blob to Azure SQL table",
    "activities": [
      {
        "name": "CopyFromBlobToSQL",
        "type": "Copy",
        "inputs": [
          {
            "name": "InputBlobTable"
          }
        ],
        "outputs": [
          {
            "name": "OutputSQLTable"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "SqlSink"
          },
          "executionLocation": "Japan East"          
        },
        "Policy": {
          "concurrency": 1,
          "executionPriorityOrder": "NewestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
    ],
    "start": "2016-07-12T00:00:00Z",
    "end": "2016-07-13T00:00:00Z"
  }
}
```
Het schema dat is gedefinieerd in de uitvoergegevensset die bepaalt wanneer de activiteit wordt uitgevoerd (bijvoorbeeld: **dagelijkse**, frequentie als **dag**, en het interval als **1**). De activiteit gegevens worden gekopieerd van een invoergegevensset (**bron**) naar een uitvoergegevensset (**sink**).

U kunt meer dan één invoergegevensset naar Kopieeractiviteit opgeven. Ze worden gebruikt om te controleren of de afhankelijkheden voordat de activiteit wordt uitgevoerd. Echter wordt alleen de gegevens uit de eerste gegevensset gekopieerd naar de doel-dataset. Zie voor meer informatie [planning en uitvoering](data-factory-scheduling-and-execution.md).  

## <a name="performance-and-tuning"></a>Prestaties en afstemmen
Zie de [Kopieeractiviteit prestaties en prestatieafstemming handleiding](data-factory-copy-activity-performance.md), waarin belangrijke factoren die van invloed op de prestaties van de verplaatsing van gegevens (Kopieeractiviteit) in Azure Data Factory wordt beschreven. Ook hier worden de waargenomen prestaties tijdens interne tests en worden verschillende manieren om te optimaliseren van de prestaties van de Kopieeractiviteit besproken.

## <a name="fault-tolerance"></a>Fouttolerantie
Standaard kopieeractiviteit wordt gestopt met het kopiëren van gegevens in en keer terug fout optreden wanneer niet-compatibele gegevens tussen de bron- en sink; terwijl u expliciet configureren kunt voor het overslaan en meld u de niet-compatibele rijen en alleen kopie wordt deze compatibel gegevens om de kopie is voltooid. Zie de [Kopieeractiviteit fouttolerantie](data-factory-copy-activity-fault-tolerance.md) op meer informatie.

## <a name="security-considerations"></a>Beveiligingsoverwegingen
Zie de [beveiligingsoverwegingen](data-factory-data-movement-security-considerations.md), waarin wordt beschreven beveiligingsinfrastructuur die services voor gegevensverplaatsing in Azure Data Factory gebruiken om uw gegevens te beveiligen.

## <a name="scheduling-and-sequential-copy"></a>Planning en sequentiële kopiëren
Zie [planning en uitvoering](data-factory-scheduling-and-execution.md) voor gedetailleerde informatie over hoe plannen en uitvoeren in de Data Factory werkt. Het is mogelijk meerdere kopieerbewerkingen na elkaar uitgevoerd op een manier opeenvolgende/gerangschikt. Zie de [sequentieel kopiëren](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) sectie.

## <a name="type-conversions"></a>Typeconversies
Andere gegevensarchieven hebben native type van verschillende systemen. Kopieeractiviteit voert automatische typeconversies van brontypen opvangen typen met de volgende benadering voor in twee stappen:

1. Systeemeigen brontypen naar een .NET-type converteren.
2. Converteren van een .NET-type naar een systeemeigen sink-type.

De toewijzing van een typesysteem systeemeigen is een .NET-type voor een gegevensopslag is in het desbetreffende data store artikel. (Klik op de specifieke koppeling in de [ondersteunde gegevensarchieven](#supported-data-stores) tabel). U kunt deze toewijzingen gebruiken om te bepalen juiste typen tijdens het maken van tabellen, zodat de kopieerbewerking wordt de juiste conversies uitgevoerd.

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over de Kopieeractiviteit, [gegevens kopiëren van Azure Blob-opslag naar Azure SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
* Zie voor meer informatie over het verplaatsen van gegevens vanuit een on-premises gegevensopslag in een cloud-gegevensarchief, [gegevens verplaatsen van on-premises naar cloud gegevensarchieven](data-factory-move-data-between-onprem-and-cloud.md).
