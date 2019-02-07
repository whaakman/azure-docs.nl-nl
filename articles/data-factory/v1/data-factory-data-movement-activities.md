---
title: Gegevens verplaatsen met behulp van de Kopieeractiviteit | Microsoft Docs
description: 'Meer informatie over gegevensverplaatsing in Data Factory-pijplijnen: migratie van gegevens tussen cloud en tussen een on-premises gegevensopslag en een archief in de cloud. Kopieeractiviteit gebruiken.'
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: 67543a20-b7d5-4d19-8b5e-af4c1fd7bc75
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/05/2017
ms.author: jingwang
robots: noindex
ms.openlocfilehash: c188c23f87715b6ba5b90f6015b59f2a347ec0cf
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55821317"
---
# <a name="move-data-by-using-copy-activity"></a>Gegevens verplaatsen met behulp van Kopieeractiviteit
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](data-factory-data-movement-activities.md)
> * [Versie 2 (huidige versie)](../copy-activity-overview.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [Copy-activiteit in V2](../copy-activity-overview.md).

## <a name="overview"></a>Overzicht
In Azure Data Factory, kunt u Kopieeractiviteit gebruiken om te kopiëren van gegevens tussen on-premises en cloud gegevensarchieven. Nadat de gegevens worden gekopieerd, kan worden verder verwerkt en geanalyseerd. U kunt ook een Kopieeractiviteit gebruiken om transformatie en analyseresultaten voor business intelligence (BI) en het verbruik van de toepassing te publiceren.

![Rol van Kopieeractiviteit](media/data-factory-data-movement-activities/copy-activity.png)

Kopieeractiviteit wordt aangestuurd door een veilige, betrouwbare, schaalbare, en [wereldwijd beschikbare service](#global). Dit artikel bevat informatie over gegevensverplaatsing in Data Factory en activiteit kopiëren.

Eerst gaan we kijken hoe gegevens migreren tussen twee cloudlocaties voor gegevensopslag, en tussen een on-premises gegevensopslag en een gegevensarchief cloud plaatsvindt.

> [!NOTE]
> Zie voor meer informatie over activiteiten in het algemeen, [inzicht in pijplijnen en activiteiten](data-factory-create-pipelines.md).
>
>

### <a name="copy-data-between-two-cloud-data-stores"></a>Kopiëren van gegevens tussen twee cloudlocaties voor gegevensopslag
Als bron en sink-gegevensopslag in de cloud, doorloopt Copy Activity in de volgende fasen om gegevens te kopiëren van de bron voor de sink. De service die wordt gebruikt door de Kopieeractiviteit:

1. Leest gegevens uit de brongegevensopslag.
2. Voert serialisatie/deserialisatie, compressie/decompressie, kolom toewijzen, en typ conversie. Dit gebeurt deze bewerkingen op basis van de configuraties van de invoergegevensset, de uitvoergegevensset en de Kopieeractiviteit.
3. Schrijft gegevens naar het beoogde gegevensarchief.

De service kiest automatisch de optimale regio om uit te voeren van de verplaatsing van gegevens. Deze regio is meestal een die het dichtst bij de sink-gegevensopslag.

![Cloud-naar-cloud kopiëren](./media/data-factory-data-movement-activities/cloud-to-cloud.png)

### <a name="copy-data-between-an-on-premises-data-store-and-a-cloud-data-store"></a>Kopiëren van gegevens tussen een on-premises gegevensopslag en een gegevensarchief cloud
Veilig om gegevens te verplaatsen tussen een on-premises gegevensopslag en een gegevensarchief voor de cloud, installeer Data Management Gateway op uw on-premises computer. Data Management Gateway is een agent waarmee hybride gegevensverplaatsing en -verwerking. U kunt deze installeren op dezelfde computer als de gegevens zelf opslaan, of op een afzonderlijke virtuele machine die toegang tot het gegevensarchief heeft.

In dit scenario, Data Management Gateway voert de serialisatie/deserialisatie, compressie/decompressie, kolom toewijzen en typ conversie. Geen gegevensstroom via de Azure Data Factory-service. Data Management Gateway schrijft de gegevens in plaats daarvan rechtstreeks naar de doel-store.

![On-premises-naar-cloud kopiëren](./media/data-factory-data-movement-activities/onprem-to-cloud.png)

Zie [gegevens verplaatsen tussen on-premises en cloud gegevensarchieven](data-factory-move-data-between-onprem-and-cloud.md) voor een inleiding en stapsgewijze instructies. Zie [Data Management Gateway](data-factory-data-management-gateway.md) voor gedetailleerde informatie over deze agent.

U kunt ook gegevens van/naar ondersteunde gegevensarchieven die worden gehost op Azure IaaS virtuele machines (VM's) met behulp van Data Management Gateway verplaatsen. U kunt in dit geval Data Management Gateway installeren op dezelfde virtuele machine, zoals het opslaan van de gegevens zelf, of op een afzonderlijke virtuele machine die toegang heeft tot het gegevensarchief.

## <a name="supported-data-stores-and-formats"></a>Ondersteunde gegevensarchieven en indelingen
De kopieeractiviteit in Data Factory kopieert gegevens van een brongegevensarchief naar een sinkgegevensarchief. Data Factory ondersteunt de volgende gegevensarchieven. Gegevens vanuit elke willekeurige bron kunnen naar een sink worden geschreven. Klik op een gegevensarchief voor informatie over het kopiëren van gegevens naar en van dat archief.

> [!NOTE] 
> Als u gegevens wilt verplaatsen naar/van een gegevensarchief dat niet wordt ondersteund door de kopieeractiviteit, gebruikt u een **aangepaste activiteit** in Data Factory met uw eigen logica voor het kopiëren/verplaatsen van gegevens. Zie [Aangepaste activiteiten gebruiken in een Azure Data Factory-pijplijn](data-factory-use-custom-activities.md) voor meer informatie over het maken en gebruiken van een aangepaste activiteit.

[!INCLUDE [data-factory-supported-data-stores](../../../includes/data-factory-supported-data-stores.md)]

> [!NOTE]
> Gegevensarchieven met een * kunnen zich on-premises of op Azure IaaS bevinden. Hiervoor moet u [Data Management Gateway](data-factory-data-management-gateway.md) installeren op een on-premises/Azure IaaS-computer.

### <a name="supported-file-formats"></a>Ondersteunde bestandsindelingen
Kunt u Copy-activiteit naar **kopiëren van bestanden als-is** tussen twee bestanden zijn gebaseerd, kunt u overslaan de [sectie opgemaakt](data-factory-create-datasets.md) in zowel de gegevenssetdefinities voor invoer en uitvoer. De gegevens efficiënt gekopieerd zonder een serialisatie/deserialisatie.

Kopieeractiviteit ook leest uit en schrijft naar bestanden in een opgegeven indeling: **Tekst, JSON, Avro, ORC en Parquet**, en compressiecodec **GZip, Deflate, BZip2 en ZipDeflate** worden ondersteund. Zie [ondersteunde indelingen voor bestanden en compressie](data-factory-supported-file-and-compression-formats.md) met details.

U kunt bijvoorbeeld de volgende kopieeractiviteiten doen:

* Kopiëren van gegevens in on-premises SQL Server en schrijven naar Azure Data Lake Store in ORC-indeling.
* Bestanden kopiëren in tekstindeling (CSV) van on-premises bestandssysteem en naar Azure-Blob in Avro-indeling wilt schrijven.
* ZIP-bestanden kopiëren van on-premises bestandssysteem en decomprimeren vervolgens land Azure Data Lake Store.
* Kopiëren van gegevens in de indeling van GZip gecomprimeerde tekstbestand (CSV) van Azure Blob en schrijven naar Azure SQL Database.

## <a name="global"></a>Wereldwijd beschikbaar gegevensverplaatsing
Azure Data Factory is alleen beschikbaar in de regio's VS-West, VS-Oost en Noord-Europa. Echter, de service die wordt gebruikt door de Kopieeractiviteit is wereldwijd beschikbaar in de volgende regio's en regio's. De algemeen beschikbare topologie zorgt u ervoor efficiënt gegevensverplaatsing die meestal regio-overschrijdende hops voorkomt. Zie [-Services per regio](https://azure.microsoft.com/regions/#services) voor beschikbaarheid van Data Factory en de verplaatsing van gegevens in een regio.

### <a name="copy-data-between-cloud-data-stores"></a>Gegevens kopiëren tussen gegevensarchieven in cloud
Als bron en sink-gegevensopslag in de cloud, Data Factory maakt gebruik van een service-implementatie in de regio die zich het dichtst bij de sink in dezelfde Geografie bevindt om de gegevens te verplaatsen. Raadpleeg de volgende tabel voor toewijzing:

| Geografische locatie van de doel-gegevensarchieven | De regio van het beoogde gegevensarchief | Regio die wordt gebruikt voor gegevensverplaatsing |
|:--- |:--- |:--- |
| Verenigde Staten | US - oost | US - oost |
| &nbsp; | US - oost 2 | US - oost 2 |
| &nbsp; | US - centraal | US - centraal |
| &nbsp; | US - noord-centraal | US - noord-centraal |
| &nbsp; | US - zuid-centraal | US - zuid-centraal |
| &nbsp; | US - west-centraal | US - west-centraal |
| &nbsp; | US - west | US - west |
| &nbsp; | US - west 2 | US - west 2 |
| Canada | Canada - oost | Canada - midden |
| &nbsp; | Canada - midden | Canada - midden |
| Brazilië | Brazilië - zuid | Brazilië - zuid |
| Europa | Europa - noord | Europa - noord |
| &nbsp; | Europa -west | Europa -west |
| Verenigd Koninkrijk | Verenigd Koninkrijk West | Verenigd Koninkrijk Zuid |
| &nbsp; | Verenigd Koninkrijk Zuid | Verenigd Koninkrijk Zuid |
| Azië en Stille Oceaan | Azië - zuidoost | Azië - zuidoost |
| &nbsp; | Azië - oost | Azië - zuidoost |
| Australië | Australië - oost | Australië - oost |
| &nbsp; | Australië - zuidoost | Australië - zuidoost |
| India | India - centraal | India - centraal |
| &nbsp; | India - west | India - centraal |
| &nbsp; | India - zuid | India - centraal |
| Japan | Japan - oost | Japan - oost |
| &nbsp; | Japan - west | Japan - oost |
| Korea | Korea - centraal | Korea - centraal |
| &nbsp; | Korea - zuid | Korea - centraal |

U kunt ook u kunt de regio van Data Factory-service moet worden gebruikt voor het uitvoeren van de kopieeractiviteit door op te geven expliciet aangeven `executionLocation` eigenschap onder Kopieeractiviteit `typeProperties`. Ondersteunde waarden voor deze eigenschap hierboven worden vermeld **regio die wordt gebruikt voor gegevensverplaatsing** kolom. Houd er rekening mee dat uw gegevens via deze regio loopt via de kabel tijdens het kopiëren. Bijvoorbeeld, te kopiëren tussen Azure worden opgeslagen in Korea, kunt u opgeven `"executionLocation": "Japan East"` om te worden gerouteerd via de regio Japan (Zie [voorbeeld-JSON](#by-using-json-scripts) als verwijzing).

> [!NOTE]
> Als de regio van het beoogde gegevensarchief niet in de voorgaande lijst is of kan niet worden getraceerd, standaard kopieerbewerking is mislukt in plaats van via een andere regio, tenzij `executionLocation` is opgegeven. De lijst met ondersteunde regio zal worden uitgebreid na verloop van tijd.
>

### <a name="copy-data-between-an-on-premises-data-store-and-a-cloud-data-store"></a>Kopiëren van gegevens tussen een on-premises gegevensopslag en een gegevensarchief cloud
Wanneer gegevens worden gekopieerd tussen on-premises (of Azure virtuele machines/IaaS) en cloud-winkels, [Data Management Gateway](data-factory-data-management-gateway.md) verplaatsing van gegevens uitvoert op een on-premises computer of virtuele machine. De gegevens niet door de service in de cloud, stromen, tenzij u de [gefaseerd kopiëren](data-factory-copy-activity-performance.md#staged-copy) mogelijkheid. In dit geval stromen gegevens via de Azure Blob-opslag staging voordat ze worden geschreven naar het sink-gegevensopslag.

## <a name="create-a-pipeline-with-copy-activity"></a>Een pijplijn maken met Copy Activity
U kunt een pijplijn maken met de Kopieeractiviteit in een aantal manieren:

### <a name="by-using-the-copy-wizard"></a>Met behulp van de Wizard kopiëren
De Kopieerwizard van Data Factory kunt u een pijplijn maken met de Kopieeractiviteit. Deze pijplijn kunt u gegevens uit ondersteunde bronnen naar bestemmingen kopiëren *zonder te hoeven schrijven JSON* definities voor de gekoppelde services, gegevenssets en pijplijnen. Zie [Kopieerwizard van Data Factory](data-factory-copy-wizard.md) voor meer informatie over de wizard.  

### <a name="by-using-json-scripts"></a>Met behulp van JSON-scripts
U kunt Data Factory-Editor in de Azure portal, Visual Studio of Azure PowerShell gebruiken om te maken van een JSON-definitie van een pijplijn (met behulp van activiteit kopiëren). Vervolgens kunt u deze voor het maken van de pijplijn in Data Factory implementeren. Zie [zelfstudie: Kopieeractiviteit gebruiken in een Azure Data Factory-pijplijn](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor een zelfstudie met stapsgewijze instructies.    

JSON-eigenschappen (zoals naam, beschrijving, invoer en uitvoer tabellen en -beleid) zijn beschikbaar voor alle soorten activiteiten. Eigenschappen die beschikbaar zijn in de `typeProperties` sectie van de activiteit variëren met elk activiteitstype.

Voor de Kopieeractiviteit, de `typeProperties` sectie is afhankelijk van de typen van bronnen en sinks. Klik op een bron/sink in de [ondersteunde bronnen en sinks](#supported-data-stores-and-formats) sectie voor meer informatie over de type-eigenschappen die Kopieeractiviteit voor het betreffende gegevensarchief ondersteunt.

Hier volgt een voorbeeld-JSON-definitie:

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
Het schema dat is gedefinieerd in de uitvoergegevensset bepaalt wanneer de activiteit wordt uitgevoerd (bijvoorbeeld: **dagelijkse**, frequentie als **dag**, en het interval als **1**). De activiteit worden gegevens gekopieerd van een invoergegevensset (**bron**) op een uitvoergegevensset (**sink**).

U kunt meer dan één invoergegevensset aan Copy-activiteit opgeven. Ze worden gebruikt om te controleren of de afhankelijkheden voordat de activiteit wordt uitgevoerd. Echter is alleen de gegevens uit de eerste gegevensset gekopieerd naar de doelgegevensset. Zie voor meer informatie, [planning en uitvoering](data-factory-scheduling-and-execution.md).  

## <a name="performance-and-tuning"></a>Prestaties en afstemmen
Zie de [Kopieeractiviteit prestatie- en afstemmingshandleiding](data-factory-copy-activity-performance.md), waarin belangrijke factoren die invloed hebben op de prestaties van de verplaatsing van gegevens (Kopieeractiviteit) in Azure Data Factory wordt beschreven. Ook geeft een lijst van de waargenomen prestaties tijdens interne tests van en worden verschillende manieren om te optimaliseren de prestaties van Kopieeractiviteit besproken.

## <a name="fault-tolerance"></a>Fouttolerantie
Standaard kopieeractiviteit wordt gestopt met het kopiëren van gegevens in en keer terug fout optreden als niet-compatibele gegevens tussen de bron en sink; terwijl u expliciet configureren kunt voor het overslaan en meldt u zich de niet-compatibele rijen en slechts kopie wordt deze compatibel gegevens naar de kopie is voltooid. Zie de [Kopieeractiviteit fouttolerantie](data-factory-copy-activity-fault-tolerance.md) op meer informatie.

## <a name="security-considerations"></a>Beveiligingsoverwegingen
Zie de [beveiligingsoverwegingen](data-factory-data-movement-security-considerations.md), waarin wordt beschreven beveiligingsinfrastructuur aanwezig zijn die gebruikmaken van services voor gegevensverplaatsing in Azure Data Factory om uw gegevens te beveiligen.

## <a name="scheduling-and-sequential-copy"></a>Planning en opeenvolgende kopiëren
Zie [planning en uitvoering](data-factory-scheduling-and-execution.md) voor gedetailleerde informatie over de plannings- en werking in Data Factory. Het is mogelijk meerdere kopieerbewerkingen na elkaar uitgevoerd op een manier opeenvolgende/gerangschikt. Zie de [kopiëren sequentieel](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) sectie.

## <a name="type-conversions"></a>Typeconversies
Andere gegevensarchieven hebben verschillende systeemeigen type systemen. Kopieeractiviteit voert automatische conversie van de typen gegevensbronnen met sink-type met de volgende methode voor verificatie in twee stappen uit:

1. Converteren van systeemeigen bronquery typen naar een .NET-type.
2. Converteren van een .NET-type naar een systeemeigen sink-type.

De toewijzing van een systeemeigen typesysteem met een .NET-type voor een gegevensopslag is in de respectieve data store artikel. (Klik op de specifieke koppeling in de tabel stores ondersteund). U kunt deze toewijzingen gebruiken om te bepalen juiste typen tijdens het maken van tabellen, zodat de Kopieeractiviteit wordt de juiste conversies uitgevoerd.

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over de Kopieeractiviteit, [gegevens kopiëren van Azure Blob-opslag naar Azure SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
* Zie voor meer informatie over het verplaatsen van gegevens vanuit een on-premises gegevensarchief naar een cloud-gegevensarchief, [gegevens verplaatsen van on-premises naar de cloud gegevensarchieven](data-factory-move-data-between-onprem-and-cloud.md).
