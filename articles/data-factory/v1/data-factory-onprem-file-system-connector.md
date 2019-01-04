---
title: Gegevens kopiëren naar/van een bestandssysteem met behulp van Azure Data Factory | Microsoft Docs
description: Informatie over het kopiëren van gegevens naar en van een on-premises bestandssysteem met behulp van Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: ce19f1ae-358e-4ffc-8a80-d802505c9c84
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/13/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 1cd7e504a614203218cb06b337becf36b992cf1d
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54018225"
---
# <a name="copy-data-to-and-from-an-on-premises-file-system-by-using-azure-data-factory"></a>Gegevens kopiëren naar en van een on-premises bestandssysteem met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](data-factory-onprem-file-system-connector.md)
> * [Versie 2 (huidige versie)](../connector-file-system.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [File System-connector in V2](../connector-file-system.md).


In dit artikel wordt uitgelegd hoe u met de Kopieeractiviteit in Azure Data Factory gebruiken om te kopiëren van gegevens naar/vanuit een on-premises bestandssysteem. Dit is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit geeft.

## <a name="supported-scenarios"></a>Ondersteunde scenario's
U kunt gegevens kopiëren **uit een on-premises bestandssysteem** opgeslagen in de volgende gegevens:

[!INCLUDE [data-factory-supported-sink](../../../includes/data-factory-supported-sinks.md)]

U kunt gegevens kopiëren van de volgende gegevensarchieven **naar een on-premises bestandssysteem**:

[!INCLUDE [data-factory-supported-sources](../../../includes/data-factory-supported-sources.md)]

> [!NOTE]
> Kopieeractiviteit worden het bronbestand niet worden verwijderd nadat deze is gekopieerd naar de bestemming. Als u verwijderen van het bronbestand na een geslaagde kopieerbewerking wilt, maakt u een aangepaste activiteit wilt verwijderen van het bestand en het gebruik van de activiteit in de pijplijn. 

## <a name="enabling-connectivity"></a>Verbindingen inschakelen
Data Factory ondersteunt verbindingen van en naar een on-premises bestandssysteem via **Data Management Gateway**. U moet de Data Management Gateway installeren in uw on-premises omgeving voor de Data Factory-service verbinding maken met een ondersteunde on-premises gegevensarchief met inbegrip van het bestandssysteem. Zie voor meer informatie over Data Management Gateway en voor stapsgewijze instructies over het instellen van de gateway, [gegevens verplaatsen tussen on-premises bronnen en de cloud met Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md). Naast de Data Management Gateway moeten er worden geen binaire bestanden worden geïnstalleerd om te communiceren en naar een on-premises bestandssysteem. U moet installeren en gebruiken van Data Management Gateway, zelfs als het bestandssysteem in Azure IaaS-VM. Zie voor gedetailleerde informatie over de gateway, [Data Management Gateway](data-factory-data-management-gateway.md).

Voor het gebruik van een bestandsshare voor Linux installeren [Samba](https://www.samba.org/) op uw Linux-server, en installeer Data Management Gateway op een Windows-server. Data Management Gateway installeert op een Linux-server wordt niet ondersteund.

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit die gegevens naar/van een bestandssysteem verplaatst met behulp van verschillende hulpprogramma's / API's.

De eenvoudigste manier om een pijplijn te maken is met de **Kopieerwizard**. Zie [zelfstudie: Een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht van het maken van een pijplijn met behulp van de wizard kopiëren.

U kunt ook de volgende hulpprogramma's gebruiken om een pijplijn te maken: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en  **REST-API**. Zie [zelfstudie Kopieeractiviteit](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor het maken van een pijplijn met een kopieeractiviteit.

Of u de hulpprogramma's of API's gebruikt, kunt u de volgende stappen uit voor het maken van een pijplijn die gegevens van een brongegevensarchief naar een sink-gegevensopslag verplaatst uitvoeren:

1. Maak een **gegevensfactory**. Een data factory kan één of meer pijplijnen bevatten. 
2. Maak **gekoppelde services** opgeslagen om invoer- en gegevens te koppelen aan uw data factory. Bijvoorbeeld, als u gegevens uit Azure blob storage naar een on-premises bestandssysteem kopieert, u twee gekoppelde services om uw on-premises bestandssysteem en de Azure storage-account koppelen aan uw data factory. Zie voor de gekoppelde service-eigenschappen die specifiek voor een on-premises bestandssysteem zijn, [gekoppelde service-eigenschappen](#linked-service-properties) sectie.
3. Maak **gegevenssets** te vertegenwoordigen invoer- en uitvoergegevens voor de kopieerbewerking. In het voorbeeld dat wordt vermeld in de vorige stap, maakt u een gegevensset om op te geven van de blob-container en map die de invoergegevens bevat. En u maakt een andere gegevensset om op te geven van de map en bestandsnaam (optioneel) in uw bestandssysteem. Zie voor de gegevensseteigenschappen die specifiek voor on-premises bestandssysteem zijn, [gegevensseteigenschappen](#dataset-properties) sectie.
4. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. In het voorbeeld eerder vermeld, gebruikt u BlobSource als bron- en FileSystemSink als een sink voor de kopieeractiviteit. Op dezelfde manier als u vanuit on-premises bestandssysteem naar Azure Blob Storage kopieert, gebruikt u FileSystemSource en BlobSink in de kopieeractiviteit. Zie voor kopiëren-activiteitseigenschappen die specifiek voor on-premises bestandssysteem zijn, [eigenschappen van de kopieeractiviteit](#copy-activity-properties) sectie. Klik op de koppeling in de vorige sectie voor de gegevensopslag voor meer informatie over het gebruik van een gegevensarchief als een bron of een sink.

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en de pijplijn) automatisch voor u gemaakt. Wanneer u hulpprogramma's / API's (met uitzondering van de .NET API), kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling.  Zie voor voorbeelden met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren naar/van een bestandssysteem [JSON voorbeelden](#json-examples-for-copying-data-to-and-from-file-system) sectie van dit artikel.

De volgende secties bevatten meer informatie over JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar bestandssysteem:

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
U kunt een on-premises bestandssysteem koppelen aan een Azure-gegevensfactory met de **On-Premises bestandsserver** gekoppelde service. De volgende tabel bevat beschrijvingen van JSON-elementen die specifiek voor de bestandsserver van On-Premises gekoppelde service zijn.

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| type |Zorg ervoor dat de eigenschap type wordt ingesteld op **OnPremisesFileServer**. |Ja |
| host |Hiermee geeft u het pad naar de hoofdmap van de map die u wilt kopiëren. Gebruik het escape-teken ' \ ' voor speciale tekens in de tekenreeks. Zie [voorbeeld gekoppelde service en de gegevensset definities](#sample-linked-service-and-dataset-definitions) voor voorbeelden. |Ja |
| gebruikers-id |Geef de ID van de gebruiker die toegang tot de server heeft. |Nee (als u ervoor encryptedCredential kiest) |
| wachtwoord |Geef het wachtwoord voor de gebruiker (gebruikersnaam). |Nee (als u ervoor encryptedCredential kiest |
| encryptedCredential |Geef de versleutelde referenties die u krijgen kunt door de cmdlet New-AzureRmDataFactoryEncryptValue uit te voeren. |Nee (als u ervoor kiest om op te geven van gebruikers-id en wachtwoord in tekst zonder opmaak) |
| gatewayName |Hiermee geeft u de naam van de gateway die Data Factory moet worden gebruikt verbinding maken met de on-premises bestandsserver. |Ja |


### <a name="sample-linked-service-and-dataset-definitions"></a>Voorbeeld van een gekoppelde service en definities van de gegevensset
| Scenario | Hosten in de definitie van de gekoppelde service | Mappad in de definitie van de gegevensset |
| --- | --- | --- |
| Lokale map op de machine Data Management Gateway: <br/><br/>Voorbeelden: D:\\ \* of D:\folder\subfolder\\* |D:\\ \\ (voor Data Management Gateway 2.0 en hoger) <br/><br/> localhost (voor oudere versies dan Data Management Gateway 2.0) |. \\ \\ of de map\\\\submap (voor Data Management Gateway 2.0 en hoger) <br/><br/>D:\\ \\ of D:\\\\map\\\\submap (voor de gatewayversie lager dan 2.0) |
| Externe gedeelde map: <br/><br/>Voorbeelden: \\ \\MijnServer\\delen\\ \* of \\ \\MijnServer\\delen\\map\\submap\\* |\\\\\\\\myserver\\\\share |. \\ \\ of de map\\\\submap |

>[!NOTE]
>Wanneer u via de gebruikersinterface, moet u niet voor het invoeren van twee backslashes (`\\`) opgeven om te escapen, zoals u ook via JSON, één backslash.

### <a name="example-using-username-and-password-in-plain-text"></a>Voorbeeld: Met behulp van gebruikersnaam en wachtwoord in tekst zonder opmaak

```JSON
{
  "Name": "OnPremisesFileServerLinkedService",
  "properties": {
    "type": "OnPremisesFileServer",
    "typeProperties": {
      "host": "\\\\Contosogame-Asia",
      "userid": "Admin",
      "password": "123456",
      "gatewayName": "mygateway"
    }
  }
}
```

### <a name="example-using-encryptedcredential"></a>Voorbeeld: Met behulp van encryptedcredential

```JSON
{
  "Name": " OnPremisesFileServerLinkedService ",
  "properties": {
    "type": "OnPremisesFileServer",
    "typeProperties": {
      "host": "D:\\",
      "encryptedCredential": "WFuIGlzIGRpc3Rpbmd1aXNoZWQsIG5vdCBvbmx5IGJ5xxxxxxxxxxxxxxxxx",
      "gatewayName": "mygateway"
    }
  }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset
Zie voor een volledige lijst van eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets en secties, [gegevenssets maken](data-factory-create-datasets.md). Secties, zoals de structuur, beschikbaarheid en het beleid van een gegevensset JSON zijn vergelijkbaar voor alle typen van de gegevensset.

De sectie typeProperties verschilt voor elk type gegevensset. Het biedt informatie zoals de locatie en de indeling van de gegevens in het gegevensarchief. De typeProperties sectie voor de gegevensset van het type **bestandsshare** heeft de volgende eigenschappen:

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| folderPath |Hiermee geeft u het subpad naar de map. Gebruik het escape-teken '\' voor speciale tekens in de tekenreeks. Filteren op jokerteken wordt niet ondersteund. Zie [voorbeeld gekoppelde service en de gegevensset definities](#sample-linked-service-and-dataset-definitions) voor voorbeelden.<br/><br/>U kunt deze eigenschap combineren met **partitionBy** naar de map paden op basis van het segment de status begin/einde en tijden. |Ja |
| fileName |Geef de naam van het bestand in de **folderPath** als u wilt dat de tabel om te verwijzen naar een specifiek bestand in de map. Als u een waarde voor deze eigenschap niet opgeeft, wordt de tabel verwijst naar alle bestanden in de map.<br/><br/>Wanneer **fileName** is niet opgegeven voor een uitvoergegevensset en **preserveHierarchy** niet is opgegeven in de activiteit-sink, de naam van het gegenereerde bestand is in de volgende indeling: <br/><br/>`Data.<Guid>.txt` (Voorbeeld: Data.0a405f8a-93ff-4C6F-b3be-f69616f1df7a.txt) |Nee |
| fileFilter |Geef een filter op dat moet worden gebruikt voor het selecteren van een subset van de bestanden in het mappad in plaats van alle bestanden. <br/><br/>Toegestane waarden zijn: `*` (meerdere tekens) en `?` (Eén teken).<br/><br/>Voorbeeld 1: "fileFilter": "* .log"<br/>Voorbeeld 2: "fileFilter': -1 - 2014?. txt"<br/><br/>Houd er rekening mee dat fileFilter is van toepassing voor een invoergegevensset van de bestandsshare. |Nee |
| partitionedBy |U kunt partitionedBy gebruiken om op te geven van een dynamische folderPath/bestandsnaam voor time series-gegevens. Een voorbeeld is folderPath geparametriseerde voor elk uur gegevens. |Nee |
| Indeling | De volgende bestandsindelingen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie, [tekstindeling](data-factory-supported-file-and-compression-formats.md#text-format), [Json-indeling](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-indeling](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc-indeling](data-factory-supported-file-and-compression-formats.md#orc-format), en [Parquet-indeling](data-factory-supported-file-and-compression-formats.md#parquet-format) secties. <br><br> Als u wilt **bestanden als kopiëren-is** overslaan tussen op basis van bestanden (binaire kopie), het gedeelte indeling in beide definities van de gegevensset voor invoer en uitvoer. |Nee |
| Compressie | Geef het type en het niveau van compressie voor de gegevens. Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**. Ondersteunde niveaus zijn: **Optimale** en **snelste**. Zie [bestands- en compressie indelingen in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nee |

> [!NOTE]
> U kunt geen bestandsnaam en fileFilter tegelijkertijd gebruiken.

### <a name="using-partitionedby-property"></a>Met behulp van de eigenschap partitionedBy
Zoals vermeld in de vorige sectie, kunt u een dynamische folderPath en de bestandsnaam voor time series-gegevens met de **partitionedBy** eigenschap [Data Factory-functies en de systeemvariabelen](data-factory-functions-variables.md).

Zie voor meer informatie over meer informatie over time series-gegevenssets, planning en segmenten, [gegevenssets maken](data-factory-create-datasets.md), [planning en uitvoering](data-factory-scheduling-and-execution.md), en [het maken van pijplijnen](data-factory-create-pipelines.md).

#### <a name="sample-1"></a>Voorbeeld 1:

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

In dit voorbeeld {segment} wordt vervangen door de waarde van de Data Factory-systeemvariabele slicestart-waarde in de notatie (YYYYMMDDHH). Slicestart-waarde verwijst naar de begintijd van het segment. FolderPath verschilt voor elk segment. Bijvoorbeeld: wikidatagateway/wikisampledataout/2014100103 of wikidatagateway/wikisampledataout/2014100104.

#### <a name="sample-2"></a>Voorbeeld 2:

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
 [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```

Jaar, maand, dag en tijd van de slicestart-waarde in dit voorbeeld zijn uitgepakt naar afzonderlijke variabelen die gebruikmaken van de eigenschappen folderPath en de bestandsnaam.

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Zie voor een volledige lijst van de secties & eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen zoals naam, beschrijving, invoer en uitvoer gegevenssets en beleidsregels zijn beschikbaar voor alle soorten activiteiten. Dat eigenschappen die beschikbaar zijn in de **typeProperties** sectie van de activiteit variëren met elk activiteitstype.

Ze verschillen voor de kopieeractiviteit, afhankelijk van de typen van bronnen en sinks. Als u gegevens uit een on-premises bestandssysteem verplaatst, u het brontype instellen in de kopieeractiviteit naar **FileSystemSource**. Als u gegevens naar een on-premises bestandssysteem verplaatst, u stelt het sink-type in de kopieeractiviteit naar **FileSystemSink**. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door FileSystemSource en FileSystemSink.

**FileSystemSource** ondersteunt de volgende eigenschappen:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| recursieve |Geeft aan of de gegevens recursief worden gelezen uit de submappen of alleen voor de opgegeven map. |True, False (standaard) |Nee |

**FileSystemSink** ondersteunt de volgende eigenschappen:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| copyBehavior |Definieert het gedrag kopiëren wanneer de bron BlobSource of bestandssysteem is. |**PreserveHierarchy:** Hiermee behoudt u de bestandshiërarchie in de doelmap. Dat wil zeggen, is het relatieve pad van het bronbestand voor de bronmap hetzelfde als het relatieve pad van de doel-bestand naar de doelmap.<br/><br/>**FlattenHierarchy:** Alle bestanden uit de bronmap worden gemaakt in het eerste niveau van de doelmap. De doelbestanden worden gemaakt met een automatisch gegenereerde naam.<br/><br/>**MergeFiles:** Hiermee worden alle bestanden uit de bronmap naar één bestand samengevoegd. Als de naam/blob-naam van het bestand is opgegeven, is de naam van het samengevoegde de opgegeven naam. Anders is de bestandsnaam van een automatisch gegenereerde. |Nee |

### <a name="recursive-and-copybehavior-examples"></a>recursieve en copyBehavior voorbeelden
Deze sectie beschrijft het resulterende gedrag van de kopieerbewerking voor de verschillende combinaties van waarden voor de recursieve en copyBehavior-eigenschappen.

| recursieve waarde | copyBehavior waarde | Resulterende gedrag |
| --- | --- | --- |
| true |preserveHierarchy |Voor een bronmap Map1 met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>de doelmap Map1 is gemaakt met dezelfde structuur als de bron:<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 |
| true |flattenHierarchy |Voor een bronmap Map1 met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Het doel Map1 is gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch gegenereerde naam voor File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand3<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File5 |
| true |mergeFiles |Voor een bronmap Map1 met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Het doel Map1 is gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand1 bestand2 + bestand3 + File4 + 5-bestand inhoud worden samengevoegd in één bestand met een automatisch gegenereerde naam. |
| false |preserveHierarchy |Voor een bronmap Map1 met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>de doelmap Map1 is gemaakt met de volgende structuur:<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/><br/>Subfolder1 bestand3 File4 en File5 is niet opgehaald. |
| false |flattenHierarchy |Voor een bronmap Map1 met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>de doelmap Map1 is gemaakt met de volgende structuur:<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch gegenereerde naam voor File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand2<br/><br/>Subfolder1 bestand3 File4 en File5 is niet opgehaald. |
| false |mergeFiles |Voor een bronmap Map1 met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>de doelmap Map1 is gemaakt met de volgende structuur:<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand1 + bestand2 inhoud worden samengevoegd in één bestand met een automatisch gegenereerde naam.<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch gegenereerde naam voor File1<br/><br/>Subfolder1 bestand3 File4 en File5 is niet opgehaald. |

## <a name="supported-file-and-compression-formats"></a>Ondersteunde indelingen voor bestanden en compressie
Zie [bestands- en compressie indelingen in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) artikel voor meer informatie.

## <a name="json-examples-for-copying-data-to-and-from-file-system"></a>JSON-voorbeelden voor het kopiëren van gegevens naar en van bestandssysteem
De volgende voorbeelden geven een voorbeeld van JSON-definities die u gebruiken kunt voor het maken van een pijplijn met behulp van de [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ze laten zien hoe het kopiëren van gegevens naar en van een on-premises bestandssysteem en een Azure Blob-opslag. U kunt echter gegevens kopiëren *rechtstreeks* uit een van de bronnen aan een van de sinks die worden vermeld in [ondersteunde bronnen en sinks](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory.

### <a name="example-copy-data-from-an-on-premises-file-system-to-azure-blob-storage"></a>Voorbeeld: Gegevens kopiëren van een on-premises bestandssysteem naar Azure Blob-opslag
Dit voorbeeld laat zien hoe u gegevens kopiëren van een on-premises bestandssysteem naar Azure Blob-opslag. Het voorbeeld heeft de volgende Data Factory-entiteiten:

* Een gekoppelde service van het type [OnPremisesFileServer](#linked-service-properties).
* Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Invoer [gegevensset](data-factory-create-datasets.md) van het type [bestandsshare](#dataset-properties).
* Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* Een [pijplijn](data-factory-create-pipelines.md) met de Kopieeractiviteit die gebruikmaakt van [FileSystemSource](#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Het volgende voorbeeld tijdreeksen worden gegevens gekopieerd van een on-premises bestandssysteem naar Azure Blob storage per uur. De JSON-eigenschappen die worden gebruikt in deze voorbeelden worden beschreven in de secties na de voorbeelden.

Instellen als eerste stap van Data Management Gateway aan de hand van de instructies in [gegevens verplaatsen tussen on-premises bronnen en de cloud met Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md).

**Bestandsserver voor on-Premises gekoppelde service:**

```JSON
{
  "Name": "OnPremisesFileServerLinkedService",
  "properties": {
    "type": "OnPremisesFileServer",
    "typeProperties": {
      "host": "\\\\Contosogame-Asia.<region>.corp.<company>.com",
      "userid": "Admin",
      "password": "123456",
      "gatewayName": "mygateway"
    }
  }
}
```

Wordt u aangeraden de **encryptedCredential** eigenschap in plaats daarvan de **userid** en **wachtwoord** eigenschappen. Zie [File Server gekoppelde service](#linked-service-properties) voor meer informatie over deze gekoppelde service.

**Gekoppelde Azure Storage-service:**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```

**On-premises bestand system invoergegevensset:**

Gegevens wordt opgehaald uit een nieuw bestand om het uur. De eigenschappen folderPath en de bestandsnaam worden bepaald op basis van de begintijd van het segment.  

Instellen van `"external": "true"` Data Factory informeert dat de dataset bevindt zich buiten de data factory en niet door een activiteit in de data factory gemaakt wordt.

```JSON
{
  "name": "OnpremisesFileSystemInput",
  "properties": {
    "type": " FileShare",
    "linkedServiceName": " OnPremisesFileServerLinkedService ",
    "typeProperties": {
      "folderPath": "mysharedfolder/yearno={Year}/monthno={Month}/dayno={Day}",
      "fileName": "{Hour}.csv",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ]
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```

**Azure Blob storage-uitvoer gegevensset:**

Gegevens worden geschreven naar een nieuwe blob elk uur (frequentie: uur en interval: 1). Het pad naar de map voor de blob wordt dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Pad naar de map maakt gebruik van het jaar, maand, dag en uur onderdelen van de begintijd.

```JSON
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": "\t",
        "rowDelimiter": "\n"
      }
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

**Een kopieeractiviteit in een pijplijn met File System-bron- en Blob:**

De pijplijn bevat een kopieeractiviteit die is geconfigureerd voor het gebruik van de invoer- en uitvoergegevenssets en is gepland voor elk uur uitgevoerd. In de pijplijn-JSON-definitie heeft de **bron** type is ingesteld op **FileSystemSource**, en **sink** type is ingesteld op **BlobSink**.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2015-06-01T18:00:00",
    "end":"2015-06-01T19:00:00",
    "description":"Pipeline for copy activity",
    "activities":[  
      {
        "name": "OnpremisesFileSystemtoBlob",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "OnpremisesFileSystemInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "FileSystemSource"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
     ]
   }
}
```

### <a name="example-copy-data-from-azure-sql-database-to-an-on-premises-file-system"></a>Voorbeeld: Gegevens kopiëren van Azure SQL Database naar een on-premises bestandssysteem
Het volgende voorbeeld laat zien:

* Een gekoppelde service van het type [AzureSqlDatabase.](data-factory-azure-sql-connector.md#linked-service-properties)
* Een gekoppelde service van het type [OnPremisesFileServer](#linked-service-properties).
* Een invoergegevensset van het type [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).
* Een uitvoergegevensset van het type [bestandsshare](#dataset-properties).
* Een pijplijn met een kopieeractiviteit die gebruikmaakt van [SqlSource](data-factory-azure-sql-connector.md##copy-activity-properties) en [FileSystemSink](#copy-activity-properties).

Het voorbeeld kopieert time series-gegevens uit een Azure SQL-tabel naar een on-premises bestandssysteem elk uur. De JSON-eigenschappen die worden gebruikt in deze voorbeelden worden beschreven in de secties na de voorbeelden.

**Gekoppelde service Azure SQL Database:**

```JSON
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```

**Bestandsserver voor on-Premises gekoppelde service:**

```JSON
{
  "Name": "OnPremisesFileServerLinkedService",
  "properties": {
    "type": "OnPremisesFileServer",
    "typeProperties": {
      "host": "\\\\Contosogame-Asia.<region>.corp.<company>.com",
      "userid": "Admin",
      "password": "123456",
      "gatewayName": "mygateway"
    }
  }
}
```

Wordt u aangeraden de **encryptedCredential** eigenschap in plaats van de **userid** en **wachtwoord** eigenschappen. Zie [bestandssysteem gekoppelde service](#linked-service-properties) voor meer informatie over deze gekoppelde service.

**Azure SQL-invoer gegevensset:**

Het voorbeeld wordt ervan uitgegaan dat u een tabel 'MyTable' in Azure SQL gemaakt hebt en een kolom met de naam 'timestampcolumn' voor time series-gegevens bevat.

Instellen van ``“external”: ”true”`` Data Factory informeert dat de dataset bevindt zich buiten de data factory en niet door een activiteit in de data factory gemaakt wordt.

```JSON
{
  "name": "AzureSqlInput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
    "typeProperties": {
      "tableName": "MyTable"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```

**On-premises bestand system uitvoergegevensset:**

Gegevens worden gekopieerd naar een nieuw bestand om het uur. Het mappad en de bestandsnaam voor de blob worden bepaald op basis van de begintijd van het segment.

```JSON
{
  "name": "OnpremisesFileSystemOutput",
  "properties": {
    "type": "FileShare",
    "linkedServiceName": " OnPremisesFileServerLinkedService ",
    "typeProperties": {
      "folderPath": "mysharedfolder/yearno={Year}/monthno={Month}/dayno={Day}",
      "fileName": "{Hour}.csv",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ]
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```

**Een kopieeractiviteit in een pijplijn met de SQL-bron en sink bestandssysteem:**

De pijplijn bevat een kopieeractiviteit die is geconfigureerd voor het gebruik van de invoer- en uitvoergegevenssets en is gepland voor elk uur uitgevoerd. In de pijplijn-JSON-definitie heeft de **bron** type is ingesteld op **SqlSource**, en de **sink** type is ingesteld op **FileSystemSink**. De SQL-query die is opgegeven voor de **SqlReaderQuery** eigenschap selecteert u de gegevens in het afgelopen uur te kopiëren.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2015-06-01T18:00:00",
    "end":"2015-06-01T20:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "AzureSQLtoOnPremisesFile",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureSQLInput"
          }
        ],
        "outputs": [
          {
            "name": "OnpremisesFileSystemOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd}\\'', WindowStart, WindowEnd)"
          },
          "sink": {
            "type": "FileSystemSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 3,
          "timeout": "01:00:00"
        }
      }
     ]
   }
}
```


Ook kunt u kolommen uit de brongegevensset op kolommen uit de sink-gegevensset in het definitie van de activiteit kopiëren toewijzen. Zie voor meer informatie, [toewijzing van kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Prestaties en afstemmen
 Zie voor meer informatie over belangrijke factoren die invloed hebben op de prestaties van de verplaatsing van gegevens (Kopieeractiviteit) in Azure Data Factory en de verschillende manieren om te optimaliseren, de [Kopieeractiviteit prestatie- en afstemmingshandleiding](data-factory-copy-activity-performance.md).
