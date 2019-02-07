---
title: Gegevens kopiëren naar en van Azure Data Lake Storage Gen1 | Microsoft Docs
description: Meer informatie over het kopiëren van gegevens naar en van Data Lake Store met behulp van Azure Data Factory
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: 25b1ff3c-b2fd-48e5-b759-bb2112122e30
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 8731857d133e60cad4ecdca21874916949e05ff3
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55813514"
---
# <a name="copy-data-to-and-from-data-lake-storage-gen1-by-using-data-factory"></a>Gegevens kopiëren van en naar Data Lake Storage Gen1 met behulp van Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](data-factory-azure-datalake-connector.md)
> * [Versie 2 (huidige versie)](../connector-azure-data-lake-store.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [Azure Data Lake Storage Gen1-connector in V2](../connector-azure-data-lake-store.md).

In dit artikel wordt uitgelegd hoe u gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te verplaatsen naar en van Azure Data Lake Storage Gen1 (voorheen bekend als Azure Data Lake Store). Dit is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel, een overzicht van de verplaatsing van gegevens met de Kopieeractiviteit.

## <a name="supported-scenarios"></a>Ondersteunde scenario's
U kunt gegevens kopiëren **van Azure Data Lake Store** opgeslagen in de volgende gegevens:

[!INCLUDE [data-factory-supported-sinks](../../../includes/data-factory-supported-sinks.md)]

U kunt gegevens kopiëren van de volgende gegevensarchieven **naar Azure Data Lake Store**:

[!INCLUDE [data-factory-supported-sources](../../../includes/data-factory-supported-sources.md)]

> [!NOTE]
> Een Data Lake Store-account maken voordat u het maken van een pijplijn met Copy Activity in. Zie voor meer informatie, [aan de slag met Azure Data Lake Store](../../data-lake-store/data-lake-store-get-started-portal.md).

## <a name="supported-authentication-types"></a>Ondersteunde verificatietypen
De Data Lake Store-connector ondersteunt de volgende verificatietypen:
* Verificatie van service-principal
* Verificatie van de gebruiker referenties (OAuth)

U wordt aangeraden gebruik te maken van service-principal verificatie, met name voor het kopiëren van een geplande gegevens. Geldigheidsduur van het token gedrag kan optreden met verificatie van gebruikersreferenties. Zie voor configuratiedetails de [gekoppelde service-eigenschappen](#linked-service-properties) sectie.

## <a name="get-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit die gegevens naar/van een Azure Data Lake Store verplaatst met behulp van verschillende hulpprogramma's / API's.

De eenvoudigste manier om een pijplijn om gegevens te kopiëren is met de **Kopieerwizard**. Zie voor een zelfstudie over het maken van een pijplijn met behulp van de Wizard kopiëren [zelfstudie: Een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md).

U kunt ook de volgende hulpprogramma's gebruiken om een pijplijn te maken: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en  **REST-API**. Zie [zelfstudie Kopieeractiviteit](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor het maken van een pijplijn met een kopieeractiviteit.

Of u de hulpprogramma's of API's gebruikt, kunt u de volgende stappen uit voor het maken van een pijplijn die gegevens van een brongegevensarchief naar een sink-gegevensopslag verplaatst uitvoeren:

1. Maak een **gegevensfactory**. Een data factory kan één of meer pijplijnen bevatten.
2. Maak **gekoppelde services** opgeslagen om invoer- en gegevens te koppelen aan uw data factory. Bijvoorbeeld, als u gegevens uit Azure blob storage naar een Azure Data Lake Store kopieert, u twee gekoppelde services om uw Azure storage-account en Azure Data Lake store koppelen aan uw data factory. Zie voor de gekoppelde service-eigenschappen die specifiek voor Azure Data Lake Store zijn, [gekoppelde service-eigenschappen](#linked-service-properties) sectie.
2. Maak **gegevenssets** te vertegenwoordigen invoer- en uitvoergegevens voor de kopieerbewerking. In het voorbeeld dat wordt vermeld in de vorige stap, maakt u een gegevensset om op te geven van de blob-container en map die de invoergegevens bevat. En u maakt een andere gegevensset om op te geven van de map en het bestandspad in de Data Lake store met de gegevens die zijn gekopieerd uit de blob-opslag. Zie voor de gegevensseteigenschappen die specifiek voor Azure Data Lake Store zijn, [gegevensseteigenschappen](#dataset-properties) sectie.
3. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. In het voorbeeld eerder vermeld, gebruikt u BlobSource als bron- en AzureDataLakeStoreSink als een sink voor de kopieeractiviteit. Op dezelfde manier als u van Azure Data Lake Store met Azure Blob Storage kopiëren wilt, gebruikt u AzureDataLakeStoreSource en BlobSink in de kopieeractiviteit. Zie voor kopiëren-activiteitseigenschappen die specifiek voor Azure Data Lake Store zijn, [eigenschappen van de kopieeractiviteit](#copy-activity-properties) sectie. Klik op de koppeling in de vorige sectie voor de gegevensopslag voor meer informatie over het gebruik van een gegevensarchief als een bron of een sink.

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en de pijplijn) automatisch voor u gemaakt. Wanneer u hulpprogramma's / API's (met uitzondering van de .NET API), kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling. Zie voor voorbeelden met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren naar/van een Azure Data Lake Store [JSON voorbeelden](#json-examples-for-copying-data-to-and-from-data-lake-store) sectie van dit artikel.

De volgende secties bevatten meer informatie over JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar Data Lake Store.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
Een gekoppelde service verbindt een gegevensopslag op een data factory. U maakt een gekoppelde service van het type **AzureDataLakeStore** uw Data Lake Store om gegevens te koppelen aan uw data factory. De volgende tabel beschrijft de JSON-elementen die specifiek zijn voor Data Lake Store gekoppelde services. U kunt kiezen tussen service-principal en verificatie van gebruikersreferenties.

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| **type** | De eigenschap type moet worden ingesteld op **AzureDataLakeStore**. | Ja |
| **dataLakeStoreUri** | Informatie over de Azure Data Lake Store-account. Deze informatie wordt een van de volgende indelingen: `https://[accountname].azuredatalakestore.net/webhdfs/v1` of `adl://[accountname].azuredatalakestore.net/`. | Ja |
| **subscriptionId** | Azure-abonnement-ID die de Data Lake Store-account behoort. | Vereist voor sink |
| **resourceGroupName** | Naam van Azure-resource die de Data Lake Store-account behoort. | Vereist voor sink |

### <a name="service-principal-authentication-recommended"></a>Service-principal verificatie (aanbevolen)
Voor het gebruik van service-principal verificatie, de Toepassingsentiteit van een registreren in Azure Active Directory (Azure AD) en verleen deze de toegang tot Data Lake Store. Zie voor gedetailleerde stappen [Service-naar-serviceverificatie](../../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Noteer de volgende waarden, die u gebruikt voor het definiëren van de gekoppelde service:
* Toepassings-id
* Toepassingssleutel
* Tenant-id

> [!IMPORTANT]
> Zorg ervoor dat u verleent de service principal juiste machtiging in Azure Data Lake Store:
>- **Data Lake Store gebruiken als bron**, ten minste verlenen **lees- en uitvoeringsmachtigingen** data access-machtiging voor het weergeven en kopieer de inhoud van een map of **lezen** machtiging voor het kopiëren van een enkel bestand. Er is geen vereiste voor beheer van toegang.
>- **Data Lake Store gebruiken als sink**, ten minste verlenen **schrijven + uitvoeren** data access-machtiging voor het maken van onderliggende items in de map. En als u Azure IR gebruiken om te kopiëren (bron en sink zijn in de cloud), als u wilt dat de Data Factory, Data Lake Store regio detecteren, verlenen ten minste **lezer** rol bij het account toegangsbeheer (IAM). Als u wilt voorkomen dat deze rol IAM [Geef een waarde executionLocation](data-factory-data-movement-activities.md#global) door de locatie van uw Data Lake Store in de kopieeractiviteit.
>- Als u **Kopieerwizard gebruiken pijplijnen ontwerpen**, ten minste verlenen **lezer** rol bij het account toegangsbeheer (IAM). Bovendien verlenen ten minste **lees- en uitvoeringsmachtigingen** machtiging voor de hoofdmap van uw Data Lake Store ('/') en alle onderliggende items. Anders ziet u mogelijk het bericht "de opgegeven referenties zijn ongeldig."

Gebruik verificatie van service-principal door de volgende eigenschappen op te geven:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| **servicePrincipalId** | Opgeven van de toepassing client-ID. | Ja |
| **servicePrincipalKey** | Geef de sleutel van de toepassing. | Ja |
| **tenant** | De tenantgegevens (domain name of tenant-ID) opgeven in uw toepassing zich bevindt. U kunt het ophalen van de muis in de rechterbovenhoek van de Azure-portal. | Ja |

**Voorbeeld: Service-principal verificatie**
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

### <a name="user-credential-authentication"></a>Verificatie van gebruikersreferenties
U kunt ook kunt u gebruikersverificatie referentie om te kopiëren van en naar Data Lake Store door de volgende eigenschappen op te geven:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| **Autorisatie** | Klik op de **autoriseren** knop in de Data Factory-Editor en voer uw referenties op waarmee de automatisch gegenereerde autorisatie-URL worden toegewezen aan deze eigenschap. | Ja |
| **sessionId** | OAuth-sessie-ID van de OAuth-autorisatie-sessie. Elke sessie-ID is uniek en kan slechts één keer worden gebruikt. Deze instelling wordt automatisch gegenereerd wanneer u de Data Factory-Editor gebruiken. | Ja |

> [!IMPORTANT]
> Zorg ervoor dat u de juiste machtiging van de gebruiker in Azure Data Lake Store verlenen:
>- **Data Lake Store gebruiken als bron**, ten minste verlenen **lees- en uitvoeringsmachtigingen** data access-machtiging voor het weergeven en kopieer de inhoud van een map of **lezen** machtiging voor het kopiëren van een enkel bestand. Er is geen vereiste voor beheer van toegang.
>- **Data Lake Store gebruiken als sink**, ten minste verlenen **schrijven + uitvoeren** data access-machtiging voor het maken van onderliggende items in de map. En als u Azure IR gebruiken om te kopiëren (bron en sink zijn in de cloud), als u wilt dat de Data Factory, Data Lake Store regio detecteren, verlenen ten minste **lezer** rol bij het account toegangsbeheer (IAM). Als u wilt voorkomen dat deze rol IAM [Geef een waarde executionLocation](data-factory-data-movement-activities.md#global) door de locatie van uw Data Lake Store in de kopieeractiviteit.
>- Als u **Kopieerwizard gebruiken pijplijnen ontwerpen**, ten minste verlenen **lezer** rol bij het account toegangsbeheer (IAM). Bovendien verlenen ten minste **lees- en uitvoeringsmachtigingen** machtiging voor de hoofdmap van uw Data Lake Store ('/') en alle onderliggende items. Anders ziet u mogelijk het bericht "de opgegeven referenties zijn ongeldig."

**Voorbeeld: Verificatie van gebruikersreferenties**
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<session ID>",
            "authorization": "<authorization URL>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

#### <a name="token-expiration"></a>Geldigheidsduur van het token
De autorisatiecode die u met behulp van genereert de **autoriseren** knop verloopt na een bepaalde hoeveelheid tijd. Het volgende bericht betekent dat het verificatietoken is verlopen:

Referentie-bewerkingsfout: invalid_grant - AADSTS70002: Fout bij het valideren van referenties. AADSTS70008: De opgegeven toegang verlenen, is verlopen of ingetrokken. Traceer-ID: d18629e8-af88-43c5-88e3-d8419eb1fca1 correlatie-ID: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 tijdstempel: 2015-12-15 21-09-31Z.

De volgende tabel ziet u de vervaldatum tijden van verschillende typen gebruikersaccounts:

| Gebruikerstype | Verloopt na |
|:--- |:--- |
| Gebruikersaccounts *niet* beheerd door Azure Active Directory (bijvoorbeeld @hotmail.com of @live.com) |12 uur |
| Gebruikersaccounts die worden beheerd door Azure Active Directory |14 dagen na het laatste segment uitvoeren <br/><br/>Als een segment op basis van een gekoppelde OAuth-service wordt uitgevoerd ten minste eenmaal per 14 dagen na 90 dagen |

Als u uw wachtwoord voordat u de verlooptijd van de token wijzigen, verloopt het token onmiddellijk. Hier ziet u het bericht die eerder in deze sectie.

U kunt het account opnieuw autoriseren met behulp van de **autoriseren** wanneer het token verloopt om de gekoppelde service opnieuw te implementeren. U kunt ook genereren waarden voor de **sessionId** en **autorisatie** eigenschappen programmatisch met behulp van de volgende code:


```csharp
if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
    linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
{
    AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

    WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
    string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

    AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
    if (azureDataLakeStoreProperties != null)
    {
        azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeStoreProperties.Authorization = authorization;
    }

    AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
    if (azureDataLakeAnalyticsProperties != null)
    {
        azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeAnalyticsProperties.Authorization = authorization;
    }
}
```
Zie voor meer informatie over de Data Factory-klassen in de code gebruikt de [AzureDataLakeStoreLinkedService klasse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService klasse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), en [ AuthorizationSessionGetResponse klasse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) onderwerpen. Voeg een verwijzing naar versie `2.9.10826.1824` van `Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll` voor de `WindowsFormsWebAuthenticationDialog` klasse die wordt gebruikt in de code.

## <a name="troubleshooting-tips"></a>Tips voor probleemoplossing

**Probleem:** Bij het kopiëren van gegevens **in** Azure Data Lake Store, als de kopieerbewerking is mislukt met de volgende fout:

  ```
  Failed to detect the region for Azure Data Lake account {your account name}. Please make sure that the Resource Group name: {resource group name} and subscription ID: {subscription ID} of this Azure Data Lake Store resource are correct.
  ```

**Hoofdoorzaak:** Er zijn 2 mogelijke redenen:

1. De `resourceGroupName` en/of `subscriptionId` opgegeven in Azure Data Lake Store gekoppelde service is onjuist.
2. De service-principal of de gebruiker beschikt niet over de benodigde machtiging.

**Oplossing:**

1. Zorg ervoor dat de `subscriptionId` en `resourceGroupName` u opgeeft in de gekoppelde service `typeProperties` zijn inderdaad degene die de data lake-account behoort.

2. Zorg ervoor dat u ten minste verlenen **lezer** rol aan de gebruiker of service-principal op het data lake-account. Dit is het maken van:

    1. Ga naar Azure Portal -> uw Data Lake Store-account
    2. Klik op **toegangsbeheer (IAM)** op de blade van de Data Lake Store
    3. Klik op **roltoewijzing toevoegen**
    4. Stel **rol** als **lezer**, en selecteer de gebruiker of de service-principal die u voor het kopiëren gebruiken om toegang te verlenen

3. Als u niet wilt verlenen **lezer** rol aan de gebruiker of service-principal, alternatief is het [expliciet opgeven de uitvoeringslocatie van een](data-factory-data-movement-activities.md#global) in kopie activitywith de locatie van uw Data Lake Store. Voorbeeld:

    ```json
    {
      "name": "CopyToADLS",
      "type": "Copy",
      ......
      "typeProperties": {
        "source": {
          "type": "<source type>"
        },
        "sink": {
          "type": "AzureDataLakeStoreSink"
        },
        "exeuctionLocation": "West US"
      }
    }
    ```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset
Als u wilt een gegevensset die de invoergegevens in een Data Lake Store opgeven, stelt u de **type** eigenschap van de gegevensset in **AzureDataLakeStore**. Stel de **linkedServiceName** eigenschap van de gegevensset in de naam van de Data Lake Store gekoppelde service. Zie voor een volledige lijst van JSON-secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties van een gegevensset in JSON, zoals **structuur**, **beschikbaarheid**, en **beleid**, zijn vergelijkbaar voor alle typen van de gegevensset (Azure SQL-database, Azure-blob en Azure-tabel voor voorbeeld). De **typeProperties** sectie verschilt voor elk type gegevensset en bevat informatie zoals de locatie en indeling van de gegevens in het gegevensarchief.

De **typeProperties** sectie voor een gegevensset van het type **AzureDataLakeStore** bevat de volgende eigenschappen:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| **folderPath** |Pad naar de container en map in Data Lake Store. |Ja |
| **fileName** |De naam van het bestand in Azure Data Lake Store. De **fileName** eigenschap is optioneel en is hoofdlettergevoelig. <br/><br/>Als u opgeeft **fileName**, de activiteit (inclusief kopie) werkt op het specifieke bestand.<br/><br/>Wanneer **fileName** niet is opgegeven, kopie bevat alle bestanden in **folderPath** in de invoergegevensset.<br/><br/>Wanneer **fileName** is niet opgegeven voor een uitvoergegevensset en **preserveHierarchy** niet is opgegeven in de activiteit-sink, de naam van het gegenereerde bestand is in de indeling van gegevens. _GUID_.txt'. Bijvoorbeeld: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt. | Nee |
| **partitionedBy** |De **partitionedBy** eigenschap is optioneel. U kunt deze gebruiken om op te geven van een dynamische pad en bestandsnaam op voor time series-gegevens. Bijvoorbeeld, **folderPath** kunnen als parameters worden gebruikt voor elk uur gegevens. Zie voor meer informatie en voorbeelden, de eigenschap partitionedBy. |Nee |
| **Indeling** | De volgende bestandsindelingen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, en **ParquetFormat**. Stel de **type** eigenschap onder **indeling** op een van deze waarden. Zie voor meer informatie de [tekstindeling](data-factory-supported-file-and-compression-formats.md#text-format), [JSON-indeling](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-indeling](data-factory-supported-file-and-compression-formats.md#avro-format), [ORC-indeling](data-factory-supported-file-and-compression-formats.md#orc-format), en [Parquet-indeling ](data-factory-supported-file-and-compression-formats.md#parquet-format) secties in het [bestands- en compressie indelingen die worden ondersteund door Azure Data Factory](data-factory-supported-file-and-compression-formats.md) artikel. <br><br> Als u wilt kopiëren van bestanden ' als-is "tussen op basis van bestanden (binaire kopie), gaat de `format` sectie in beide definities van de gegevensset voor invoer en uitvoer. |Nee |
| **Compressie** | Geef het type en het niveau van compressie voor de gegevens. Ondersteunde typen zijn **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**. Ondersteunde niveaus zijn **optimale** en **snelst**. Zie voor meer informatie, [bestands- en compressie indelingen die worden ondersteund door Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nee |

### <a name="the-partitionedby-property"></a>De eigenschap partitionedBy
U kunt opgeven als dynamisch **folderPath** en **fileName** eigenschappen voor time series-gegevens met de **partitionedBy** eigenschap, Data Factory-functies en systeemvariabelen. Zie voor meer informatie, de [Azure Data Factory - functies en systeemvariabelen](data-factory-functions-variables.md) artikel.


In het volgende voorbeeld `{Slice}` wordt vervangen door de waarde van de Data Factory-systeemvariabele `SliceStart` in de opgegeven indeling (`yyyyMMddHH`). De naam van de `SliceStart` verwijst naar de begintijd van het segment. De `folderPath` eigenschap verschilt voor elk segment, zoals in `wikidatagateway/wikisampledataout/2014100103` of `wikidatagateway/wikisampledataout/2014100104`.

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

In het volgende voorbeeld, het jaar, maand, dag en tijd van `SliceStart` zijn uitgepakt naar afzonderlijke variabelen die worden gebruikt door de `folderPath` en `fileName` eigenschappen:
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
Zie voor meer informatie over time series-gegevenssets, planning en segmenten de [gegevenssets in Azure Data Factory](data-factory-create-datasets.md) en [Data Factory plannen en uitvoeren](data-factory-scheduling-and-execution.md) artikelen.


## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [het maken van pijplijnen](data-factory-create-pipelines.md) artikel. Eigenschappen zoals naam, beschrijving, invoer en uitvoer tabellen en -beleid zijn beschikbaar voor alle soorten activiteiten.

De eigenschappen die beschikbaar zijn in de **typeProperties** gedeelte van een activiteit variëren met elk activiteitstype. Ze verschillen, afhankelijk van de typen van bronnen en sinks voor een kopieeractiviteit.

**AzureDataLakeStoreSource** ondersteunt de volgende eigenschap in de **typeProperties** sectie:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| **recursive** |Geeft aan of de gegevens recursief worden gelezen uit de submappen of alleen voor de opgegeven map. |True (standaardwaarde), False |Nee |

**AzureDataLakeStoreSink** ondersteunt de volgende eigenschappen in de **typeProperties** sectie:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| **copyBehavior** |Hiermee geeft u het gedrag van de kopie. |<b>PreserveHierarchy</b>: Hiermee behoudt u de bestandshiërarchie in de doelmap. Het relatieve pad van het bronbestand voor bronmap is identiek aan het relatieve pad van doelbestand naar doelmap.<br/><br/><b>FlattenHierarchy</b>: Alle bestanden uit de bronmap worden gemaakt in het eerste niveau van de doelmap. De doelbestanden worden gemaakt met automatisch gegenereerde naam.<br/><br/><b>MergeFiles</b>: Hiermee worden alle bestanden uit de bronmap naar één bestand samengevoegd. Als de naam van het bestand of de blob is opgegeven, is de naam van het samengevoegde de opgegeven naam. Anders is de naam van het automatisch gegenereerde. |Nee |

### <a name="recursive-and-copybehavior-examples"></a>recursieve en copyBehavior voorbeelden
Deze sectie beschrijft het resulterende gedrag van de kopieerbewerking voor de verschillende combinaties van recursieve en copyBehavior waarden.

| recursieve | copyBehavior | Resulterende gedrag |
| --- | --- | --- |
| true |preserveHierarchy |Voor een bronmap Map1 met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>de doelmap Map1 wordt gemaakt met dezelfde structuur als de bron<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. |
| true |flattenHierarchy |Voor een bronmap Map1 met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Het doel Map1 is gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch gegenereerde naam voor File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand3<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File5 |
| true |mergeFiles |Voor een bronmap Map1 met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Het doel Map1 is gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand1 bestand2 + bestand3 + File4 + 5-bestand inhoud worden samengevoegd in één bestand met automatisch gegenereerde naam |
| false |preserveHierarchy |Voor een bronmap Map1 met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>De doelmap Map1 wordt gemaakt met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/><br/><br/>Subfolder1 bestand3 File4 en File5 worden niet doorgevoerd. |
| false |flattenHierarchy |Voor een bronmap Map1 met de volgende structuur:<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>De doelmap Map1 wordt gemaakt met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch gegenereerde naam voor File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand2<br/><br/><br/>Subfolder1 bestand3 File4 en File5 worden niet doorgevoerd. |
| false |mergeFiles |Voor een bronmap Map1 met de volgende structuur:<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>De doelmap Map1 wordt gemaakt met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand1 + bestand2 inhoud worden samengevoegd in één bestand met automatisch gegenereerde naam. Automatisch gegenereerde naam voor File1<br/><br/>Subfolder1 bestand3 File4 en File5 worden niet doorgevoerd. |

## <a name="supported-file-and-compression-formats"></a>Ondersteunde indelingen voor bestanden en compressie
Zie voor meer informatie, de [bestands- en compressie indelingen in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) artikel.

## <a name="json-examples-for-copying-data-to-and-from-data-lake-store"></a>JSON-voorbeelden voor het kopiëren van gegevens naar en van Data Lake Store
De volgende voorbeelden geven een voorbeeld van JSON-definities. U kunt deze voorbeeld-definities voor het maken van een pijplijn met behulp van de [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). De voorbeelden laten zien hoe het kopiëren van gegevens naar en van Data Lake Store en Azure Blob-opslag. Echter, de gegevens kunnen worden gekopieerd _rechtstreeks_ uit een van de bronnen aan een van de ondersteunde Put. Zie voor meer informatie de sectie 'ondersteunde gegevensarchieven en indelingen' in de [gegevens verplaatsen met behulp van Kopieeractiviteit](data-factory-data-movement-activities.md) artikel.

### <a name="example-copy-data-from-azure-blob-storage-to-azure-data-lake-store"></a>Voorbeeld: Gegevens kopiëren van Azure Blob Storage naar Azure Data Lake Store
De voorbeeldcode in deze sectie bevat:

* Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Een gekoppelde service van het type [AzureDataLakeStore](#linked-service-properties).
* Invoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureDataLakeStore](#dataset-properties).
* Een [pijplijn](data-factory-create-pipelines.md) met een kopieeractiviteit die gebruikmaakt van [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) en [AzureDataLakeStoreSink](#copy-activity-properties).

De voorbeelden laten zien hoe time series gegevens uit Azure Blob Storage wordt elk uur naar Data Lake Store wordt gekopieerd.

**Een gekoppelde Azure Storage-service**

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

**Azure Data Lake Store gekoppelde service**

```JSON
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

> [!NOTE]
> Zie voor configuratiedetails de [gekoppelde service-eigenschappen](#linked-service-properties) sectie.
>

**De Azure Blob-invoergegevensset**

In het volgende voorbeeld, gegevens wordt opgehaald uit een nieuwe blob elk uur (`"frequency": "Hour", "interval": 1`). Pad en naam van de map voor de blob worden dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Pad naar de map maakt gebruik van het jaar, maand en dag deel van de begintijd. Naam van het bestand maakt gebruik van het uurgedeelte van de begintijd. De `"external": true` instelling van de Data Factory-service wordt geïnformeerd dat de tabel bevindt zich buiten de data factory en niet door een activiteit in de data factory wordt geproduceerd.

```JSON
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
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

**Azure Data Lake Store-uitvoergegevensset**

Het volgende voorbeeld worden gegevens gekopieerd naar Data Lake Store. Nieuwe gegevens worden gekopieerd naar Data Lake Store om het uur.

```JSON
{
    "name": "AzureDataLakeStoreOutput",
    "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/output/"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**Kopieeractiviteit in een pijplijn met een blob-bron- en een Data Lake Store**

In het volgende voorbeeld bevat deze pijplijn maar een kopieeractiviteit die is geconfigureerd voor gebruik van de invoer- en uitvoergegevenssets. De kopieeractiviteit is gepland voor elk uur uitgevoerd. In de pijplijn-JSON-definitie heeft de `source` type is ingesteld op `BlobSource`, en de `sink` type is ingesteld op `AzureDataLakeStoreSink`.

```json
{
    "name":"SamplePipeline",
    "properties":
    {
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":
        [
            {
                "name": "AzureBlobtoDataLake",
                "description": "Copy Activity",
                "type": "Copy",
                "inputs": [
                    {
                        "name": "AzureBlobInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureDataLakeStoreOutput"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "AzureDataLakeStoreSink"
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

### <a name="example-copy-data-from-azure-data-lake-store-to-an-azure-blob"></a>Voorbeeld: Gegevens kopiëren van Azure Data Lake Store met een Azure-blob
De voorbeeldcode in deze sectie bevat:

* Een gekoppelde service van het type [AzureDataLakeStore](#linked-service-properties).
* Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Invoer [gegevensset](data-factory-create-datasets.md) van het type [AzureDataLakeStore](#dataset-properties).
* Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* Een [pijplijn](data-factory-create-pipelines.md) met een kopieeractiviteit die gebruikmaakt van [AzureDataLakeStoreSource](#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

De code tijdreeksen worden gegevens gekopieerd van Data Lake Store met een Azure-blob elk uur.

**Azure Data Lake Store gekoppelde service**

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>"
        }
    }
}
```

> [!NOTE]
> Zie voor configuratiedetails de [gekoppelde service-eigenschappen](#linked-service-properties) sectie.
>

**Een gekoppelde Azure Storage-service**

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
**Azure Data Lake-invoergegevensset**

In dit voorbeeld stellen `"external"` naar `true` informeert de Data Factory-service dat de tabel bevindt zich buiten de data factory en niet door een activiteit in de data factory wordt geproduceerd.

```json
{
    "name": "AzureDataLakeStoreInput",
    "properties":
    {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/input/",
            "fileName": "SearchLog.tsv",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            }
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
**De Azure Blob-uitvoergegevensset**

In het volgende voorbeeld, gegevens worden geschreven naar een nieuwe blob elk uur (`"frequency": "Hour", "interval": 1`). Het pad naar de map voor de blob wordt dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Pad naar de map maakt gebruik van het jaar, maand, dag en uur deel van de begintijd.

```JSON
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Een kopieeractiviteit in een pijplijn met een Azure Data Lake Store-bron- en een blob-sink**

In het volgende voorbeeld bevat deze pijplijn maar een kopieeractiviteit die is geconfigureerd voor gebruik van de invoer- en uitvoergegevenssets. De kopieeractiviteit is gepland voor elk uur uitgevoerd. In de pijplijn-JSON-definitie heeft de `source` type is ingesteld op `AzureDataLakeStoreSource`, en de `sink` type is ingesteld op `BlobSink`.

```json
{
    "name":"SamplePipeline",
    "properties":{
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[
            {
                "name": "AzureDakeLaketoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                    {
                        "name": "AzureDataLakeStoreInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "AzureDataLakeStoreSource",
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

U kunt ook de kolommen uit de brongegevensset naar kolommen in de sink-gegevensset toewijzen in het definitie van de activiteit kopiëren. Zie voor meer informatie, [toewijzing van kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Prestaties en afstemmen
Zie voor meer informatie over de factoren die invloed hebben op prestaties van de Kopieeractiviteit en over het optimaliseren van het, de [Kopieeractiviteit prestatie- en afstemmingshandleiding](data-factory-copy-activity-performance.md) artikel.
