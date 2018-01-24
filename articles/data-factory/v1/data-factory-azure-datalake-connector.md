---
title: "Gegevens kopiëren naar en van Azure Data Lake Store | Microsoft Docs"
description: "Informatie over het kopiëren van gegevens naar en van Data Lake Store met behulp van Azure Data Factory"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 25b1ff3c-b2fd-48e5-b759-bb2112122e30
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: f4ba8288c1efd443310b4efc305c6f397c8163a0
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="copy-data-to-and-from-data-lake-store-by-using-data-factory"></a>Gegevens kopiëren naar en van Data Lake Store via Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](data-factory-azure-datalake-connector.md)
> * [Versie 2 - Preview](../connector-azure-data-lake-store.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [Azure Data Lake Store-connector in V2](../connector-azure-data-lake-store.md).

In dit artikel wordt uitgelegd hoe gebruiken Kopieeractiviteit in Azure Data Factory om gegevens te verplaatsen naar en van Azure Data Lake Store. Dit is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel, een overzicht van de verplaatsing van gegevens met de Kopieeractiviteit.

## <a name="supported-scenarios"></a>Ondersteunde scenario's
U kunt gegevens kopiëren **van Azure Data Lake Store** opslaat in de volgende gegevens:

[!INCLUDE [data-factory-supported-sinks](../../../includes/data-factory-supported-sinks.md)]

U kunt gegevens kopiëren van de volgende gegevensarchieven **met Azure Data Lake Store**:

[!INCLUDE [data-factory-supported-sources](../../../includes/data-factory-supported-sources.md)]

> [!NOTE]
> Een Data Lake Store-account maken voordat u een pijplijn maakt met de Kopieeractiviteit. Zie voor meer informatie [aan de slag met Azure Data Lake Store](../../data-lake-store/data-lake-store-get-started-portal.md).

## <a name="supported-authentication-types"></a>Ondersteunde verificatietypen
De Data Lake Store-connector ondersteunt de volgende verificatietypen:
* Verificatie van service-principal
* Verificatie van gebruikersreferenties (OAuth) 

Het is raadzaam dat u service-principal verificatie, met name voor een geplande gegevens opnieuw te kopiëren. Verlopen van het token kan gebeuren met verificatie van gebruikersreferenties. Zie voor configuratiedetails de [gekoppelde service-eigenschappen](#linked-service-properties) sectie.

## <a name="get-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit waarmee gegevens worden verplaatst van een Azure Data Lake Store met verschillende hulpprogramma's voor API's.

De eenvoudigste manier om u te maken van een pijplijn om gegevens te kopiëren is met de **Wizard kopiëren**. Zie voor een zelfstudie over het maken van een pijplijn met behulp van de Wizard kopiëren [zelfstudie: een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md).

U kunt ook de volgende hulpprogramma's gebruiken voor het maken van een pijplijn: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en **REST-API**. Zie [kopie activiteit zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor een pijplijn maken met een kopieeractiviteit.

Of u de hulpprogramma's of API's gebruiken, moet u de volgende stappen voor het maken van een pijplijn die de gegevens vanuit een brongegevensarchief naar een gegevensarchief sink verplaatst uitvoeren:

1. Maak een **gegevensfactory**. Een gegevensfactory kan één of meer pijplijnen bevatten. 
2. Maak **gekoppelde services** slaat om invoer- en gegevens te koppelen aan uw gegevensfactory. Bijvoorbeeld, als u gegevens uit een Azure blob-opslag met een Azure Data Lake Store kopieert, maakt u twee gekoppelde services als u wilt uw Azure storage-account en de Azure Data Lake store koppelen aan uw gegevensfactory. Zie voor de gekoppelde service-eigenschappen die specifiek voor Azure Data Lake Store zijn, [gekoppelde service-eigenschappen](#linked-service-properties) sectie. 
2. Maak **gegevenssets** vertegenwoordigt de invoer- en -gegevens voor de kopieerbewerking. In het voorbeeld in de laatste stap wordt vermeld, maakt u een gegevensset om op te geven van de blob-container en de map waarin de invoergegevens. En maken van een andere gegevensset opgeven van de map en het bestandspad in de Data Lake store de gegevens gekopieerd van de blob-opslag bevat. Zie voor eigenschappen van gegevensset die specifiek voor Azure Data Lake Store zijn, [eigenschappen van gegevensset](#dataset-properties) sectie.
3. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. In het voorbeeld eerder vermeld, gebruikt u BlobSource als een bron- en AzureDataLakeStoreSink als een sink voor de kopieeractiviteit. Op dezelfde manier als u van Azure Data Lake Store naar Azure Blob Storage kopiëren wilt, gebruikt u AzureDataLakeStoreSource en BlobSink in de kopieerbewerking. Zie voor activiteitseigenschappen kopiëren die specifiek voor Azure Data Lake Store zijn, [activiteitseigenschappen kopiëren](#copy-activity-properties) sectie. Klik op de koppeling in de vorige sectie voor de gegevensopslag voor meer informatie over het gebruik van een gegevensarchief als een bron of een sink.  

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en pijplijn) automatisch voor u gemaakt. Wanneer u extra/API's (met uitzondering van de .NET API) gebruikt, kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling.  Zie voor voorbeelden met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren naar/van een Azure Data Lake Store, [JSON voorbeelden](#json-examples-for-copying-data-to-and-from-data-lake-store) sectie van dit artikel.

De volgende secties bevatten informatie over de JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar Data Lake Store.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
Een gekoppelde service gegevensopslag is gekoppeld aan een gegevensfactory. Maken van een gekoppelde service van het type **AzureDataLakeStore** uw Data Lake Store om gegevens te koppelen aan uw gegevensfactory. De volgende tabel beschrijft de JSON-elementen die specifiek zijn voor Data Lake Store gekoppelde services. U kunt kiezen tussen service-principal en verificatie van gebruikersreferenties.

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| **type** | De eigenschap type moet worden ingesteld op **AzureDataLakeStore**. | Ja |
| **dataLakeStoreUri** | Informatie over het Azure Data Lake Store-account. Deze informatie heeft een van de volgende indelingen: `https://[accountname].azuredatalakestore.net/webhdfs/v1` of `adl://[accountname].azuredatalakestore.net/`. | Ja |
| **abonnements-id** | Azure-abonnement-ID waartoe het Data Lake Store-account behoort. | Vereist voor sink |
| **resourceGroupName** | Naam Azure resourcegroep waartoe het Data Lake Store-account behoort. | Vereist voor sink |

### <a name="service-principal-authentication-recommended"></a>Verificatie van de service principal (aanbevolen)
Registreren van een Toepassingsentiteit in Azure Active Directory (Azure AD) voor het gebruik van verificatie van de service-principal en wordt de toegang verlenen tot Data Lake Store. Zie voor gedetailleerde stappen [authentication Service-naar-serviceconnector](../../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Noteer de volgende waarden die u gebruikt voor het definiëren van de gekoppelde service:
* Toepassings-id
* Sleutel van toepassing 
* Tenant-id

> [!IMPORTANT]
> Zorg ervoor dat u service principal juiste toestemming geven in Azure Data Lake Store:
>- **Data Lake Store te gebruiken als bron**, ten minste verlenen **lezen + Execute** data access-machtiging voor het weergeven en kopieer de inhoud van een map of **lezen** machtiging voor het kopiëren van één bestand. Er is geen vereiste voor toegangsbeheer op account.
>- **Gebruik van Data Lake Store als sink**, ten minste verlenen **schrijven + uitvoeren** data access-machtiging voor het maken van onderliggende items in de map. En als u Azure IR gebruiken om te zorgen dat kopiëren (bron- en sink zijn in de cloud), om te kunnen laten Data Factory Data Lake Store regio detecteren, verlenen ten minste **lezer** rol in account toegangsbeheer (IAM). Als u wilt voorkomen dat deze rol IAM [executionLocation opgeven](data-factory-data-movement-activities.md#global) met de locatie van uw Data Lake Store in de kopieerbewerking.
>- Als u **Wizard kopiëren gebruiken voor het ontwerpen van pijplijnen**, ten minste verlenen **lezer** rol in account toegangsbeheer (IAM). Bovendien verlenen ten minste **lezen + Execute** machtiging voor de hoofdmap van uw Data Lake Store ('/') en de onderliggende items. Anders mogelijk ziet u het bericht "de opgegeven referenties zijn ongeldig."

Verificatie van de service-principal gebruiken door te geven van de volgende eigenschappen:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| **servicePrincipalId** | Geef de toepassing client-ID. | Ja |
| **servicePrincipalKey** | De sleutel van de toepassing opgeven. | Ja |
| **tenant** | De tenant-gegevens (domain name of tenant-ID) opgeven onder uw toepassing zich bevindt. U kunt deze ophalen door de muis in de rechterbovenhoek van de Azure portal. | Ja |

**Voorbeeld: Service-principal-verificatie**
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
U kunt ook kunt u verificatie van gebruikersreferenties voor het kopiëren van of naar Data Lake Store door te geven van de volgende eigenschappen:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| **autorisatie** | Klik op de **autoriseren** in de Data Factory-Editor en voer uw referenties op waarmee de automatisch gegenereerde autorisatie-URL worden toegewezen aan deze eigenschap. | Ja |
| **sessie-id** | OAuth-sessie-ID van de OAuth-autorisatie-sessie. Elke sessie-ID is uniek en kan slechts één keer worden gebruikt. Deze instelling wordt automatisch gegenereerd wanneer u de Data Factory-Editor. | Ja |

> [!IMPORTANT]
> Zorg ervoor dat u de juiste machtiging van de gebruiker in Azure Data Lake Store verlenen:
>- **Data Lake Store te gebruiken als bron**, ten minste verlenen **lezen + Execute** data access-machtiging voor het weergeven en kopieer de inhoud van een map of **lezen** machtiging voor het kopiëren van één bestand. Er is geen vereiste voor toegangsbeheer op account.
>- **Gebruik van Data Lake Store als sink**, ten minste verlenen **schrijven + uitvoeren** data access-machtiging voor het maken van onderliggende items in de map. En als u Azure IR gebruiken om te zorgen dat kopiëren (bron- en sink zijn in de cloud), om te kunnen laten Data Factory Data Lake Store regio detecteren, verlenen ten minste **lezer** rol in account toegangsbeheer (IAM). Als u wilt voorkomen dat deze rol IAM [executionLocation opgeven](data-factory-data-movement-activities.md#global) met de locatie van uw Data Lake Store in de kopieerbewerking.
>- Als u **Wizard kopiëren gebruiken voor het ontwerpen van pijplijnen**, ten minste verlenen **lezer** rol in account toegangsbeheer (IAM). Bovendien verlenen ten minste **lezen + Execute** machtiging voor de hoofdmap van uw Data Lake Store ('/') en de onderliggende items. Anders mogelijk ziet u het bericht "de opgegeven referenties zijn ongeldig."

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

#### <a name="token-expiration"></a>Verlopen van het token
De autorisatiecode die u met behulp van genereren de **autoriseren** knop verloopt na een bepaalde hoeveelheid tijd. Het volgende bericht betekent dat de verificatietoken is verlopen:

Referentie-bewerkingsfout: invalid_grant - AADSTS70002: fout bij het valideren van referenties. AADSTS70008: De toegewezen toegangsmachtiging is verlopen of ingetrokken. Traceer-ID: d18629e8-af88-43c5-88e3-d8419eb1fca1 correlatie-ID: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 tijdstempel: 2015-12-15 21-09-31Z.

De volgende tabel ziet u de vervaldatum tijden van verschillende typen gebruikersaccounts:

| Gebruikerstype | Verloopt na |
|:--- |:--- |
| Gebruikersaccounts *niet* beheerd door Azure Active Directory (bijvoorbeeld @hotmail.com of @live.com) |12 uur |
| Gebruikersaccounts worden beheerd door Azure Active Directory |het is 14 dagen na het laatste segment uitvoeren <br/><br/>Als een segment op basis van een gekoppelde op basis van het OAuth-service wordt uitgevoerd in de 14 dagen ten minste eenmaal na 90 dagen |

Als u uw wachtwoord voordat de verlooptijd van de token wijzigen, is het token verloopt onmiddellijk. U ziet het bericht dat eerder is vermeld in deze sectie.

U kunt het account opnieuw autoriseren met behulp van de **autoriseren** knop wanneer het token is verlopen opnieuw te implementeren, de gekoppelde service. U kunt ook genereren waarden voor de **sessionId** en **autorisatie** eigenschappen programmatisch met behulp van de volgende code:


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
Zie voor meer informatie over de Data Factory-klassen gebruikt in de code de [AzureDataLakeStoreLinkedService klasse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService klasse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), en [ Klasse AuthorizationSessionGetResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) onderwerpen. Voeg een verwijzing naar versie `2.9.10826.1824` van `Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll` voor de `WindowsFormsWebAuthenticationDialog` klasse die in de code wordt gebruikt.

## <a name="troubleshooting-tips"></a>Tips voor probleemoplossing

**Symptoom:** bij het kopiëren van gegevens **in** Azure Data Lake Store, als uw kopieeractiviteit met de volgende fout mislukken:

  ```
  Failed to detect the region for Azure Data Lake account {your account name}. Please make sure that the Resource Group name: {resource group name} and subscription ID: {subscription ID} of this Azure Data Lake Store resource are correct.
  ```

**Hoofdoorzaak:** er 2 mogelijke redenen zijn:

1. De `resourceGroupName` en/of `subscriptionId` opgegeven in Azure Data Lake Store gekoppelde service is onjuist.
2. De gebruiker of de service-principal hebt niet de benodigde machtiging.

**Oplossing:**

1. Zorg ervoor dat de `subscriptionId` en `resourceGroupName` u opgeeft in de gekoppelde service `typeProperties` zijn inderdaad degene die uw data lake-account behoort.

2. Zorg ervoor dat u ten minste verlenen '**lezer**' rol aan de gebruiker of service-principal op het data lake-account. Dit is het maken van:

    1. Ga naar de Azure Portal -> uw Data Lake Store-account
    2. Klik op ' Access Control (IAM) ' in de blade van de Data Lake Store
    3. Klik op "Toevoegen" in de blade van ' Access Control (IAM) "
    4. Stel 'Rol' als 'Lezer' en selecteer de gebruiker of de service-principal die u voor kopie gebruiken om toegang te verlenen

3. Als u niet dat de rol 'Lezer' verlenen aan de gebruiker of service-principal wilt, alternatieve gebruikersinvoer is het [expliciet een uitvoeren-locatie opgeeft](data-factory-data-movement-activities.md#global) in kopiëren activitywith de locatie van uw Data Lake Store. Voorbeeld:

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
Als een dataset staat voor invoergegevens in een Data Lake Store opgeven, die u stelt de **type** eigenschap van de gegevensset **AzureDataLakeStore**. Stel de **linkedServiceName** eigenschap van de gegevensset op de naam van de Data Lake Store gekoppelde service. Zie voor een volledige lijst van JSON-secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties van een gegevensset in JSON, zoals **structuur**, **beschikbaarheid**, en **beleid**, zijn identiek voor alle typen van de gegevensset (Azure SQL-database, blob van Azure en Azure-tabel voor voorbeeld). De **typeProperties** sectie verschilt voor elk type gegevensset en bevat informatie zoals de locatie en indeling van de gegevens in het gegevensarchief. 

De **typeProperties** sectie voor een gegevensset van het type **AzureDataLakeStore** bevat de volgende eigenschappen:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| **folderPath** |Pad naar de container en map in Data Lake Store. |Ja |
| **fileName** |Naam van het bestand in Azure Data Lake Store. De **fileName** eigenschap is optioneel en is hoofdlettergevoelig. <br/><br/>Als u opgeeft **fileName**, de activiteit (inclusief kopiëren) werkt op het specifieke bestand.<br/><br/>Wanneer **fileName** niet is opgegeven, kopie bevat alle bestanden in **folderPath** in de invoer gegevensset.<br/><br/>Wanneer **fileName** is niet opgegeven voor een uitvoergegevensset en **preserveHierarchy** niet is opgegeven in activiteit sink, de naam van het gegenereerde bestand is in de indeling van gegevens. _GUID_.txt'. Voorbeeld: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt. |Nee |
| **partitionedBy** |De **partitionedBy** eigenschap is optioneel. U kunt deze gebruiken om op te geven van een dynamische pad en bestandsnaam op voor timeseries gegevens. Bijvoorbeeld: **folderPath** kunnen als parameters worden gebruikt voor elk uur van gegevens. Zie voor meer informatie en voorbeelden [de eigenschap partitionedBy](#using-partitionedby-property). |Nee |
| **indeling** | De volgende indelingstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, en  **ParquetFormat**. Stel de **type** eigenschap onder **indeling** op een van deze waarden. Zie voor meer informatie de [tekstindeling](data-factory-supported-file-and-compression-formats.md#text-format), [JSON-indeling](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-indeling](data-factory-supported-file-and-compression-formats.md#avro-format), [ORC indeling](data-factory-supported-file-and-compression-formats.md#orc-format), en [parketvloeren-indeling ](data-factory-supported-file-and-compression-formats.md#parquet-format) secties in de [bestands- en compressie indelingen die worden ondersteund door Azure Data Factory](data-factory-supported-file-and-compression-formats.md) artikel. <br><br> Als u wilt kopiëren van bestanden ' als-is ' overslaan tussen bestandsgebaseerde winkels (binaire kopiëren), de `format` sectie in beide definities invoer en uitvoer gegevensset. |Nee |
| **compressie** | Geef het type en de compressie van de gegevens. Ondersteunde typen zijn **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**. Ondersteunde niveaus zijn **optimale** en **snelst**. Zie voor meer informatie [bestands- en compressie indelingen die worden ondersteund door Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nee |

### <a name="the-partitionedby-property"></a>De eigenschap partitionedBy
Kunt u dynamische **folderPath** en **fileName** eigenschappen voor timeseries gegevens met de **partitionedBy** eigenschap, Data Factory-functies en systeemvariabelen. Zie voor meer informatie de [Azure Data Factory - functies en systeemvariabelen](data-factory-functions-variables.md) artikel.


In het volgende voorbeeld `{Slice}` is vervangen door de waarde van de Data Factory systeemvariabele `SliceStart` in de opgegeven indeling (`yyyyMMddHH`). De naam van de `SliceStart` verwijst naar de begintijd van het segment. De `folderPath` eigenschap verschilt voor elk segment, zoals in `wikidatagateway/wikisampledataout/2014100103` of `wikidatagateway/wikisampledataout/2014100104`.

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

In het volgende voorbeeld, het jaar, maand, dag en tijd van `SliceStart` worden uitgepakt in verschillende variabelen die worden gebruikt door de `folderPath` en `fileName` eigenschappen:
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
Zie voor meer informatie over tijdreeks gegevenssets, planning en segmenten de [gegevenssets in Azure Data Factory](data-factory-create-datasets.md) en [Data Factory plannen en uitvoeren](data-factory-scheduling-and-execution.md) artikelen. 


## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen op, zoals naam, beschrijving, invoer en uitvoer tabellen en -beleid zijn beschikbaar voor alle typen activiteiten.

De eigenschappen die beschikbaar zijn in de **typeProperties** gedeelte van een activiteit variëren met elk activiteitstype. Voor een kopieeractiviteit variëren ze, afhankelijk van de typen van bronnen en Put.

**AzureDataLakeStoreSource** ondersteunt de volgende eigenschap in de **typeProperties** sectie:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| **recursive** |Hiermee wordt aangegeven of de gegevens recursief is gelezen uit de submappen of alleen uit de opgegeven map. |True (standaardwaarde), False |Nee |


**AzureDataLakeStoreSink** ondersteunt de volgende eigenschappen in de **typeProperties** sectie:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| **copyBehavior** |Hiermee geeft u het gedrag van de kopie. |<b>PreserveHierarchy</b>: behoudt de bestandshiërarchie in de doelmap. Het relatieve pad van het bronbestand naar de bronmap is identiek aan het relatieve pad van doelbestand naar doelmap.<br/><br/><b>FlattenHierarchy</b>: alle bestanden uit de bronmap worden gemaakt in het eerste niveau van de doelmap. De doelbestanden worden gemaakt met de automatisch gegenereerde namen.<br/><br/><b>MergeFiles</b>: alle bestanden uit de bronmap op één bestand worden samengevoegd. Als de naam van het bestand of blob is opgegeven, is de samengevoegde bestandsnaam de opgegeven naam. De bestandsnaam is anders wordt automatisch gegenereerd. |Nee |

### <a name="recursive-and-copybehavior-examples"></a>Voorbeelden van recursieve en copyBehavior
Deze sectie beschrijft het resulterende gedrag van de kopieerbewerking voor verschillende combinaties van recursieve en copyBehavior waarden.

| Recursieve | copyBehavior | Resulterende gedrag |
| --- | --- | --- |
| waar |preserveHierarchy |Voor een bronmap Map1 met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>de doelmap Map1 wordt gemaakt met dezelfde structuur als de bron<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. |
| waar |flattenHierarchy |Voor een bronmap Map1 met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>het doel Map1 wordt gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand3<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File5 |
| waar |mergeFiles |Voor een bronmap Map1 met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>het doel Map1 wordt gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 bestand2 + bestand3 + File4 + bestand 5 inhoud worden samengevoegd in één bestand met automatisch gegenereerde naam |
| onwaar |preserveHierarchy |Voor een bronmap Map1 met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>de doelmap Map1 wordt gemaakt met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/><br/><br/>Subfolder1 bestand3 File4 en File5 zijn niet opgenomen. |
| onwaar |flattenHierarchy |Voor een bronmap Map1 met de volgende structuur:<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>de doelmap Map1 wordt gemaakt met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand2<br/><br/><br/>Subfolder1 bestand3 File4 en File5 zijn niet opgenomen. |
| onwaar |mergeFiles |Voor een bronmap Map1 met de volgende structuur:<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>de doelmap Map1 wordt gemaakt met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + bestand2 inhoud worden samengevoegd in één bestand met automatisch gegenereerde naam. automatisch gegenereerde naam voor File1<br/><br/>Subfolder1 bestand3 File4 en File5 zijn niet opgenomen. |

## <a name="supported-file-and-compression-formats"></a>Ondersteunde indelingen voor bestands- en compressie
Zie voor meer informatie de [bestands- en compressie-notaties in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) artikel.

## <a name="json-examples-for-copying-data-to-and-from-data-lake-store"></a>JSON-voorbeelden voor het kopiëren van gegevens naar en van Data Lake Store
De volgende voorbeelden geven voorbeeld JSON definities. U kunt deze definities voorbeeld een pijplijn maken met behulp van de [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). De voorbeelden laten zien hoe om gegevens te kopiëren naar en van Data Lake Store en Azure Blob storage. Echter, de gegevens kunnen worden gekopieerd _rechtstreeks_ uit een van de bronnen aan een van de ondersteunde Put. Zie voor meer informatie de sectie 'ondersteunde gegevensarchieven en indelingen' in de [verplaatsen van gegevens met behulp van de Kopieeractiviteit](data-factory-data-movement-activities.md) artikel.  

### <a name="example-copy-data-from-azure-blob-storage-to-azure-data-lake-store"></a>Voorbeeld: Gegevens kopiëren van Azure Blob-opslag naar Azure Data Lake Store
De voorbeeldcode in dit gedeelte ziet:

* Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Een gekoppelde service van het type [AzureDataLakeStore](#linked-service-properties).
* Invoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureDataLakeStore](#dataset-properties).
* Een [pijplijn](data-factory-create-pipelines.md) met een kopieeractiviteit die gebruikmaakt van [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) en [AzureDataLakeStoreSink](#copy-activity-properties).

De voorbeelden laten zien hoe timeseries gegevens uit Azure Blob Storage is gekopieerd naar Data Lake Store om het uur. 

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

**Gekoppelde service van Azure Data Lake Store**

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

In het volgende voorbeeld gegevens wordt opgehaald uit een nieuwe blob elk uur (`"frequency": "Hour", "interval": 1`). Pad en naam van de map voor de blob worden dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Het mappad maakt gebruik van het jaar, maand en daggedeelte van de begintijd. De bestandsnaam wordt gebruikt voor het uurgedeelte van de begintijd. De `"external": true` instelling informeert de Data Factory-service dat de tabel aan de gegevensfactory extern en niet wordt geproduceerd door een activiteit in de gegevensfactory.

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

**Azure Data Lake Store uitvoergegevensset**

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


**Kopieeractiviteit in een pijplijn met een blob-bron- en een Data Lake Store-sink**

De pijplijn bevat in het volgende voorbeeld wordt een kopieeractiviteit die is geconfigureerd voor gebruik van de invoer- en uitvoergegevenssets. De kopieerbewerking is gepland voor elk uur uitgevoerd. In de pijplijn-JSON-definitie de `source` type is ingesteld op `BlobSource`, en de `sink` type is ingesteld op `AzureDataLakeStoreSink`.

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

### <a name="example-copy-data-from-azure-data-lake-store-to-an-azure-blob"></a>Voorbeeld: Gegevens kopiëren van Azure Data Lake Store naar een Azure-blob
De voorbeeldcode in dit gedeelte ziet:

* Een gekoppelde service van het type [AzureDataLakeStore](#linked-service-properties).
* Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Invoer [gegevensset](data-factory-create-datasets.md) van het type [AzureDataLakeStore](#dataset-properties).
* Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* Een [pijplijn](data-factory-create-pipelines.md) met een kopieeractiviteit die gebruikmaakt van [AzureDataLakeStoreSource](#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

De code opgehaald-timeseries-gegevens in Data Lake Store met een Azure-blob elk uur. 

**Gekoppelde service van Azure Data Lake Store**

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
**Azure Data Lake invoergegevensset**

In dit voorbeeld instelling `"external"` naar `true` informeert de Data Factory-service dat de tabel aan de gegevensfactory extern en niet wordt geproduceerd door een activiteit in de gegevensfactory.

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

In het volgende voorbeeld gegevens worden geschreven naar een nieuwe blob elk uur (`"frequency": "Hour", "interval": 1`). Het pad naar de blob wordt dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Het mappad maakt gebruik van het jaar, maand, dag en uur gedeelte van de begintijd.

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

De pijplijn bevat in het volgende voorbeeld wordt een kopieeractiviteit die is geconfigureerd voor gebruik van de invoer- en uitvoergegevenssets. De kopieerbewerking is gepland voor elk uur uitgevoerd. In de pijplijn-JSON-definitie de `source` type is ingesteld op `AzureDataLakeStoreSource`, en de `sink` type is ingesteld op `BlobSink`.

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

U kunt ook kolommen uit de bron-gegevensset naar kolommen in de gegevensset sink toewijzen in de definitie van de activiteit kopiëren. Zie voor meer informatie [toewijzing gegevensset kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Prestaties en afstemmen
Zie voor meer informatie over de factoren die invloed hebben op prestaties van de Kopieeractiviteit en optimaliseren het, de [Kopieeractiviteit prestaties en prestatieafstemming handleiding](data-factory-copy-activity-performance.md) artikel.
