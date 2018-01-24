---
title: "Gegevens kopiëren naar/van een bestandssysteem met behulp van Azure Data Factory | Microsoft Docs"
description: "Informatie over het kopiëren van gegevens naar en van een on-premises bestandssysteem met behulp van Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: ce19f1ae-358e-4ffc-8a80-d802505c9c84
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: a717ea57642c0fb56eec176542ae401f83af6841
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="copy-data-to-and-from-an-on-premises-file-system-by-using-azure-data-factory"></a>Gegevens kopiëren naar en van een on-premises bestandssysteem met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](data-factory-onprem-file-system-connector.md)
> * [Versie 2 - Preview](../connector-file-system.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [File System-connector in V2](../connector-file-system.md).


In dit artikel wordt uitgelegd hoe de Kopieeractiviteit in Azure Data Factory gebruiken om gegevens uit een on-premises bestandssysteem kopiëren. Dit is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel, hetgeen een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit toont.

## <a name="supported-scenarios"></a>Ondersteunde scenario's
U kunt gegevens kopiëren **uit een on-premises bestandssysteem** opslaat in de volgende gegevens:

[!INCLUDE [data-factory-supported-sink](../../../includes/data-factory-supported-sinks.md)]

U kunt gegevens kopiëren van de volgende gegevensarchieven **naar het lokale bestandssysteem**:

[!INCLUDE [data-factory-supported-sources](../../../includes/data-factory-supported-sources.md)]

> [!NOTE]
> Kopieeractiviteit verwijdert het bronbestand niet nadat deze is gekopieerd naar de bestemming. Als u verwijderen van het bronbestand na een geslaagde kopie wilt, maakt u een aangepaste activiteit voor het bestand verwijderen en het gebruik van de activiteit in de pijplijn. 

## <a name="enabling-connectivity"></a>Connectiviteit inschakelen
Data Factory ondersteunt verbindingen van en naar een on-premises bestandssysteem via **Data Management Gateway**. U moet de Data Management Gateway installeren in uw on-premises omgeving voor de Data Factory-service voor het verbinding maken met een ondersteunde lokale gegevensarchief inclusief het bestandssysteem. Zie voor meer informatie over Data Management Gateway en voor stapsgewijze instructies over het instellen van de gateway [gegevens verplaatsen tussen lokale bronnen en de cloud met Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md). Afgezien van Data Management Gateway er hoeven geen binaire bestanden worden geïnstalleerd om te communiceren en naar een on-premises bestandssysteem. U moet installeren en gebruiken van Data Management Gateway, zelfs als het bestandssysteem in Azure IaaS VM. Zie voor gedetailleerde informatie over de gateway [Data Management Gateway](data-factory-data-management-gateway.md).

Voor het gebruik van een bestandsshare Linux installeren [Samba](https://www.samba.org/) op uw Linux-server en de Data Management Gateway installeren op een WindowsServer. Data Management Gateway installeren op een Linux-server wordt niet ondersteund.

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit waarmee gegevens van een bestandssysteem wordt verplaatst met behulp van verschillende hulpprogramma's voor API's.

De eenvoudigste manier om een pijplijn maken is met de **Wizard kopiëren**. Zie [zelfstudie: een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht over het maken van een pijplijn met de wizard kopiëren.

U kunt ook de volgende hulpprogramma's gebruiken voor het maken van een pijplijn: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en **REST-API**. Zie [kopie activiteit zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor een pijplijn maken met een kopieeractiviteit.

Of u de hulpprogramma's of API's gebruiken, moet u de volgende stappen voor het maken van een pijplijn die de gegevens vanuit een brongegevensarchief naar een gegevensarchief sink verplaatst uitvoeren:

1. Maak een **gegevensfactory**. Een gegevensfactory kan één of meer pijplijnen bevatten. 
2. Maak **gekoppelde services** slaat om invoer- en gegevens te koppelen aan uw gegevensfactory. Bijvoorbeeld, als u gegevens uit een Azure blob-opslag naar het lokale bestandssysteem kopieert, maakt u twee gekoppelde services om uw on-premises bestandssysteem en de Azure storage-account koppelen aan uw gegevensfactory. Zie voor de gekoppelde service-eigenschappen die specifiek voor een on-premises bestandssysteem zijn, [gekoppelde service-eigenschappen](#linked-service-properties) sectie.
3. Maak **gegevenssets** vertegenwoordigt de invoer- en -gegevens voor de kopieerbewerking. In het voorbeeld in de laatste stap wordt vermeld, maakt u een gegevensset om op te geven van de blob-container en de map waarin de invoergegevens. En maken van een andere dataset om op te geven van de map en bestandsnaam (optioneel) in het bestandssysteem. Zie voor eigenschappen van gegevensset die specifiek voor on-premises bestandssysteem zijn, [eigenschappen van gegevensset](#dataset-properties) sectie.
4. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. In het voorbeeld eerder vermeld, gebruikt u BlobSource als een bron- en FileSystemSink als een sink voor de kopieeractiviteit. Op dezelfde manier als u uit een on-premises bestandssysteem naar Azure Blob Storage kopiëren wilt, gebruikt u FileSystemSource en BlobSink in de kopieerbewerking. Zie voor activiteitseigenschappen kopiëren die specifiek voor on-premises bestandssysteem zijn, [activiteitseigenschappen kopiëren](#copy-activity-properties) sectie. Klik op de koppeling in de vorige sectie voor de gegevensopslag voor meer informatie over het gebruik van een gegevensarchief als een bron of een sink.

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en pijplijn) automatisch voor u gemaakt. Wanneer u extra/API's (met uitzondering van de .NET API) gebruikt, kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling.  Zie voor voorbeelden met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren naar/van een bestandssysteem, [JSON voorbeelden](#json-examples-for-copying-data-to-and-from-file-system) sectie van dit artikel.

De volgende secties bevatten informatie over de JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar bestandssysteem:

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
U kunt een on-premises bestandssysteem koppelen aan een Azure-gegevensfactory met de **On-Premises bestandsserver** gekoppelde service. De volgende tabel bevat beschrijvingen van JSON-elementen die specifiek voor de bestandsserver On-Premises gekoppelde service zijn.

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| type |Zorg ervoor dat de type-eigenschap is ingesteld op **OnPremisesFileServer**. |Ja |
| host |Hiermee geeft u het pad naar de hoofdmap van de map die u wilt kopiëren. Gebruik het escape-teken ' \ ' voor speciale tekens in de tekenreeks. Zie [voorbeeld gekoppelde service en gegevensset definities](#sample-linked-service-and-dataset-definitions) voor voorbeelden. |Ja |
| gebruikers-id |Geef de ID van de gebruiker die toegang tot de server heeft. |Nee (als u ervoor encryptedCredential kiest) |
| wachtwoord |Geef het wachtwoord voor de gebruiker (gebruikersnaam). |Nee (als u encryptedCredential kiezen |
| encryptedCredential |Geef de versleutelde referenties die u ophalen kunt door de cmdlet New-AzureRmDataFactoryEncryptValue. |Nee (als u ervoor kiest om op te geven van de gebruikersnaam en wachtwoord in tekst zonder opmaak) |
| gatewayName |Hiermee geeft u de naam van de gateway die voor Data Factory verbinding maken met de lokale bestandsserver wordt gebruikt. |Ja |


### <a name="sample-linked-service-and-dataset-definitions"></a>Voorbeeld van de gekoppelde service en de definities van de gegevensset
| Scenario | In de definitie van de gekoppelde service host | folderPath in de definitie van gegevensset |
| --- | --- | --- |
| Lokale map op de Data Management Gateway-apparaat: <br/><br/>Voorbeelden: D:\\ \* of D:\folder\subfolder\\* |D:\\ \\ (voor Data Management Gateway 2.0 en hoger) <br/><br/> localhost (voor oudere versies dan Data Management Gateway 2.0) |. \\ \\ of de map\\\\submap (voor Data Management Gateway 2.0 en hoger) <br/><br/>D:\\ \\ of D:\\\\map\\\\submap (voor gatewayversie lager dan 2.0) |
| Externe gedeelde map: <br/><br/>Voorbeelden: \\ \\MijnServer\\delen\\ \* of \\ \\MijnServer\\delen\\map\\submap\\* |\\\\\\\\myserver\\\\share |. \\ \\ of de map\\\\submap |


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

### <a name="example-using-encryptedcredential"></a>Voorbeeld: Encryptedcredential gebruiken

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
Zie voor een volledige lijst van de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets en secties [gegevenssets maken](data-factory-create-datasets.md). Secties zoals structuur, beschikbaarheid en beleid van een gegevensset JSON zijn identiek voor alle typen van de gegevensset.

De sectie typeProperties verschilt voor elk type dataset. Het levert informatie zoals de locatie en de indeling van de gegevens in het gegevensarchief. De typeProperties sectie voor de gegevensset van het type **FileShare** heeft de volgende eigenschappen:

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| folderPath |Hiermee geeft u het subpad naar de map. Gebruik het escape-teken ' \' voor speciale tekens in de tekenreeks. Zie [voorbeeld gekoppelde service en gegevensset definities](#sample-linked-service-and-dataset-definitions) voor voorbeelden.<br/><br/>U kunt deze eigenschap combineren met **partitionBy** map hebben paden op basis van het segment beginnen of eindigen-datums en tijden. |Ja |
| fileName |Geef de naam van het bestand in de **folderPath** als u wilt dat de tabel om te verwijzen naar een bepaald bestand in de map. Als u geen waarde voor deze eigenschap niet opgeeft, wordt de tabel verwijst naar alle bestanden in de map.<br/><br/>Wanneer **fileName** is niet opgegeven voor een uitvoergegevensset en **preserveHierarchy** niet is opgegeven in activiteit sink, de naam van het gegenereerde bestand is in de volgende indeling: <br/><br/>`Data.<Guid>.txt`(Voorbeeld: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt) |Nee |
| fileFilter |Hiermee geeft u een filter moet worden gebruikt om een subset van de bestanden in het mappad in plaats van alle bestanden te selecteren. <br/><br/>Toegestane waarden zijn: `*` (meerdere tekens) en `?` (willekeurig teken).<br/><br/>Voorbeeld 1: 'fileFilter":" * .log '<br/>Voorbeeld 2: 'fileFilter': 2014 - 1-?. txt'<br/><br/>Houd er rekening mee dat fileFilter is van toepassing op een bestandsshare-invoergegevensset. |Nee |
| partitionedBy |U kunt partitionedBy gebruiken om op te geven van een dynamische folderPath/bestandsnaam voor timeseries gegevens. Een voorbeeld is folderPath geparametriseerde voor elk uur van gegevens. |Nee |
| Indeling | De volgende indelingstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie [tekstindeling](data-factory-supported-file-and-compression-formats.md#text-format), [Json-indeling](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-indeling](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc indeling](data-factory-supported-file-and-compression-formats.md#orc-format), en [parketvloeren indeling](data-factory-supported-file-and-compression-formats.md#parquet-format) secties. <br><br> Als u wilt **kopiëren van bestanden als-is** overslaan tussen bestandsgebaseerde winkels (binaire kopiëren), de sectie indeling in de definities van beide invoer en uitvoer gegevensset. |Nee |
| Compressie | Geef het type en de compressie van de gegevens. Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**. Ondersteunde niveaus: **optimale** en **snelst**. Zie [bestands- en compressie-notaties in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nee |

> [!NOTE]
> U kunt geen bestandsnaam en fileFilter gelijktijdig gebruiken.

### <a name="using-partitionedby-property"></a>Gebruik de eigenschap partitionedBy
Zoals vermeld in de vorige sectie, kunt u een dynamische folderPath en de bestandsnaam voor de time series-gegevens met de **partitionedBy** -eigenschap [Data Factory-functies en de systeemvariabelen](data-factory-functions-variables.md).

Zie voor meer informatie over de tijdreeks gegevenssets, planning en segmenten inzicht, [gegevenssets maken](data-factory-create-datasets.md), [planning en uitvoering](data-factory-scheduling-and-execution.md), en [pijplijnen maken](data-factory-create-pipelines.md).

#### <a name="sample-1"></a>Voorbeeld 1:

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

In dit voorbeeld {segment} is vervangen door de waarde van de Data Factory systeemvariabele SliceStart in de notatie (YYYYMMDDHH). SliceStart verwijst voor het starten van de tijd van het segment. FolderPath verschilt voor elk segment. Bijvoorbeeld: wikidatagateway/wikisampledataout/2014100103 of wikidatagateway/wikisampledataout/2014100104.

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

In dit voorbeeld worden jaar, maand, dag en tijd van de SliceStart uitgepakt in verschillende variabelen die gebruikmaken van de eigenschappen folderPath en de bestandsnaam.

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten van de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen op, zoals naam, beschrijving, invoer en uitvoer gegevenssets en beleidsregels zijn beschikbaar voor alle typen activiteiten. Dat eigenschappen beschikbaar zijn in de **typeProperties** sectie van de activiteit variëren met elk activiteitstype.

Voor de kopieeractiviteit variëren ze, afhankelijk van de typen van bronnen en Put. Als u gegevens uit een on-premises bestandssysteem verplaatst, u het brontype instellen in de kopieerbewerking naar **FileSystemSource**. Als u gegevens naar een on-premises bestandssysteem verplaatst, u stelt het sink-type in de kopieerbewerking naar **FileSystemSink**. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door FileSystemSource en FileSystemSink.

**FileSystemSource** ondersteunt de volgende eigenschappen:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| Recursieve |Hiermee wordt aangegeven of de gegevens recursief is gelezen uit de submappen of alleen uit de opgegeven map. |True, False (standaard) |Nee |

**FileSystemSink** ondersteunt de volgende eigenschappen:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| copyBehavior |Definieert het gedrag kopiëren wanneer de bron BlobSource of het bestandssysteem. |**PreserveHierarchy:** behoudt de bestandshiërarchie in de doelmap. Dat wil zeggen, is het relatieve pad van het bronbestand voor de bronmap hetzelfde als het relatieve pad van het doel-bestand naar de doelmap.<br/><br/>**FlattenHierarchy:** alle bestanden uit de bronmap worden gemaakt in het eerste niveau van de doelmap. De doelbestanden worden gemaakt met een automatisch gegenereerde naam.<br/><br/>**MergeFiles:** alle bestanden uit de bronmap op één bestand worden samengevoegd. Als de naam/blob-naam van het bestand is opgegeven, is de samengevoegde bestandsnaam de opgegeven naam. Anders is het een automatisch gegenereerde naam. |Nee |

### <a name="recursive-and-copybehavior-examples"></a>Voorbeelden van recursieve en copyBehavior
Deze sectie beschrijft het resulterende gedrag van de kopieerbewerking voor verschillende combinaties van waarden voor de eigenschappen recursieve en copyBehavior.

| recursieve waarde | copyBehavior waarde | Resulterende gedrag |
| --- | --- | --- |
| waar |preserveHierarchy |Voor een bronmap Map1 met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>de doelmap Map1 is gemaakt met dezelfde structuur als de bron:<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 |
| waar |flattenHierarchy |Voor een bronmap Map1 met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>het doel Map1 wordt gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand3<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File5 |
| waar |mergeFiles |Voor een bronmap Map1 met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>het doel Map1 wordt gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 bestand2 + bestand3 + File4 + bestand 5 inhoud worden samengevoegd in één bestand met een automatisch gegenereerde naam. |
| onwaar |preserveHierarchy |Voor een bronmap Map1 met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>de doelmap Map1 is gemaakt met de volgende structuur:<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/><br/>Subfolder1 bestand3 File4 en File5 is niet opgenomen. |
| onwaar |flattenHierarchy |Voor een bronmap Map1 met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>de doelmap Map1 is gemaakt met de volgende structuur:<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand2<br/><br/>Subfolder1 bestand3 File4 en File5 is niet opgenomen. |
| onwaar |mergeFiles |Voor een bronmap Map1 met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>de doelmap Map1 is gemaakt met de volgende structuur:<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + bestand2 inhoud worden samengevoegd in één bestand met een automatisch gegenereerde naam.<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch gegenereerde naam voor File1<br/><br/>Subfolder1 bestand3 File4 en File5 is niet opgenomen. |

## <a name="supported-file-and-compression-formats"></a>Ondersteunde indelingen voor bestands- en compressie
Zie [bestands- en compressie-notaties in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) artikel voor meer informatie.

## <a name="json-examples-for-copying-data-to-and-from-file-system"></a>Voorbeelden voor het kopiëren van gegevens naar en van JSON-bestandssysteem
De volgende voorbeelden geven voorbeeld JSON definities die u een pijplijn maken kunt met behulp van de [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ze laten zien hoe om gegevens te kopiëren naar en van een on-premises bestandssysteem en de Azure-blobopslag. U kunt gegevens echter kopiëren *rechtstreeks* uit een van de bronnen aan een van de put die worden vermeld in [ondersteunde gegevensbronnen en put](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory.

### <a name="example-copy-data-from-an-on-premises-file-system-to-azure-blob-storage"></a>Voorbeeld: Gegevens kopiëren van een on-premises bestandssysteem naar Azure Blob-opslag
Dit voorbeeld laat zien hoe gegevens kopiëren van een on-premises bestandssysteem naar Azure Blob-opslag. Het voorbeeld heeft de volgende Data Factory-entiteiten:

* Een gekoppelde service van het type [OnPremisesFileServer](#linked-service-properties).
* Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Invoer [gegevensset](data-factory-create-datasets.md) van het type [FileShare](#dataset-properties).
* Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* Een [pijplijn](data-factory-create-pipelines.md) met de Kopieeractiviteit die gebruikmaakt van [FileSystemSource](#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Het volgende voorbeeld kopieert tijdreeks gegevens van een on-premises bestandssysteem naar Azure Blob storage elk uur. De JSON-eigenschappen die worden gebruikt in deze voorbeelden worden beschreven in de secties na de voorbeelden.

Instellen als eerste stap van Data Management Gateway volgens de instructies in [gegevens verplaatsen tussen lokale bronnen en de cloud met Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md).

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

Wordt u aangeraden de **encryptedCredential** eigenschap in plaats daarvan de **userid** en **wachtwoord** eigenschappen. Zie [bestandsserver gekoppelde service](#linked-service-properties) voor meer informatie over deze gekoppelde service.

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

**Lokale bestandsnaam systeem invoergegevensset:**

Gegevens wordt opgehaald uit een nieuw bestand om het uur. De eigenschappen folderPath en de bestandsnaam worden bepaald op basis van de begintijd van het segment.  

Instelling `"external": "true"` Data Factory informeert dat de dataset extern is aan de gegevensfactory en niet wordt geproduceerd door een activiteit in de gegevensfactory.

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

**Azure Blob storage-uitvoergegevensset:**

Gegevens worden geschreven naar een nieuwe blob elk uur (frequentie: uur, interval: 1). Het pad naar de blob wordt dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Het mappad maakt gebruik van het jaar, maand, dag en uur delen van de begintijd.

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

**Een kopieeractiviteit in een pijplijn met File System-bron en sink van Blob:**

De pijplijn bevat een kopieeractiviteit die is geconfigureerd voor gebruik van de invoer- en uitvoergegevenssets en elk uur is gepland. In de pijplijn-JSON-definitie de **bron** type is ingesteld op **FileSystemSource**, en **sink** type is ingesteld op **BlobSink**.

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
Het volgende voorbeeld toont:

* Een gekoppelde service van het type [AzureSqlDatabase.](data-factory-azure-sql-connector.md#linked-service-properties)
* Een gekoppelde service van het type [OnPremisesFileServer](#linked-service-properties).
* Een invoergegevensset van het type [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).
* Een uitvoergegevensset van het type [FileShare](#dataset-properties).
* Een pijplijn met een kopieeractiviteit die gebruikmaakt van [SqlSource](data-factory-azure-sql-connector.md##copy-activity-properties) en [FileSystemSink](#copy-activity-properties).

Het voorbeeld kopieert timeseries gegevens van een Azure SQL-tabel naar een on-premises bestandssysteem elk uur. De JSON-eigenschappen die worden gebruikt in deze voorbeelden worden beschreven in de secties na de voorbeelden.

**Azure SQL Database, gekoppelde service:**

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

Wordt u aangeraden de **encryptedCredential** eigenschap in plaats van de **userid** en **wachtwoord** eigenschappen. Zie [gekoppelde service van bestandssysteem](#linked-service-properties) voor meer informatie over deze gekoppelde service.

**Azure SQL invoergegevensset:**

Het voorbeeld wordt ervan uitgegaan dat u een tabel 'MijnTabel' in Azure SQL gemaakt hebt en een kolom met de naam 'timestampcolumn' voor timeseries gegevens bevat.

Instelling ``“external”: ”true”`` Data Factory informeert dat de dataset extern is aan de gegevensfactory en niet wordt geproduceerd door een activiteit in de gegevensfactory.

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

**Lokale bestandsnaam uitvoergegevensset systeem:**

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

**Een kopieeractiviteit in een pijplijn met de SQL-bron en sink van bestandssysteem:**

De pijplijn bevat een kopieeractiviteit die is geconfigureerd voor gebruik van de invoer- en uitvoergegevenssets en elk uur is gepland. In de pijplijn-JSON-definitie de **bron** type is ingesteld op **SqlSource**, en de **sink** type is ingesteld op **FileSystemSink**. De SQL-query die is opgegeven voor de **SqlReaderQuery** eigenschap selecteert u de gegevens in het afgelopen uur te kopiëren.

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


U kunt ook kolommen uit de bron-gegevensset naar kolommen uit sink gegevensset in de definitie van de activiteit kopiëren toewijzen. Zie voor meer informatie [toewijzing gegevensset kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Prestaties en afstemmen
 Zie voor meer informatie over de belangrijkste factoren die van invloed op de prestaties van de verplaatsing van gegevens (Kopieeractiviteit) in Azure Data Factory en verschillende manieren om te optimaliseren, de [Kopieeractiviteit prestaties en prestatieafstemming handleiding](data-factory-copy-activity-performance.md).
