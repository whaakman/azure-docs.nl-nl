---
title: 'Belasting: dataprep Python-SDK'
titleSuffix: Azure Machine Learning service
description: Meer informatie over het laden van gegevens met Azure Machine Learning Data Prep SDK. U kunt verschillende soorten gegevens laden, gegevens-bestandstypen en parameters opgeven of de slimme lezen SDK-functionaliteit gebruiken voor het automatisch detecteren bestandstype.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: jmartens
ms.date: 02/22/2019
ms.custom: seodec18
ms.openlocfilehash: fef3281f1f4e727b58878439e3f6456fee3b6241
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66752931"
---
# <a name="load-and-read-data-with-the-azure-machine-learning-data-prep-sdk"></a>Laden en lezen van gegevens met de Azure Machine Learning Data Prep SDK
In dit artikel leert u verschillende methoden voor het laden van gegevens met behulp van de Azure Machine Learning Data Prep SDK.  De SDK biedt ondersteuning voor meerdere gegevensopname-functies, waaronder:

* Laden uit veel bestandstypen met het parseren van de parameter Deductie (codering, scheidingsteken, headers)
* Type converteren met behulp van Deductie tijdens het laden van bestand
* Ondersteuning voor MS SQL Server en Azure Data Lake Storage-verbindingen

> [!Important]
> Als u een nieuwe oplossing bouwt, kunt u de [Azure Machine Learning gegevenssets](how-to-explore-prepare-data.md) (preview) voor gegevens verkennen en de voorbereiding. Gegevenssets is de volgende versie van de SDK biedt uitgebreide functionaliteit voor het beheren van data-sets in AI-oplossingen voor gegevensvoorbereiding.
> Als u de `azureml-dataprep` pakket maken van een gegevensstroom met uw transformaties in plaats van de `azureml-datasets` pakket voor het maken van een gegevensset, kunt u zich niet voor later gebruik van momentopnamen of versioned gegevenssets.

De volgende tabel ziet u een selectie van functies die worden gebruikt voor het laden van gegevens uit algemene bestandstypen.

| Bestandstype | Function | Koppeling |
|-------|-------|-------|
|Alle|`auto_read_file()`|[Verwijzing](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep?view=azure-dataprep-py#auto-read-file-path--filepath--include-path--bool---false-----azureml-dataprep-api-dataflow-dataflow)|
|Text|`read_lines()`|[Verwijzing](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep#read-lines-path--filepath--header--azureml-dataprep-api-dataflow-promoteheadersmode----promoteheadersmode-none--0---encoding--azureml-dataprep-api-engineapi-typedefinitions-fileencoding----fileencoding-utf8--0---skip-rows--int---0--skip-mode--azureml-dataprep-api-dataflow-skipmode----skipmode-none--0---comment--str---none--include-path--bool---false--verify-exists--bool---true-----azureml-dataprep-api-dataflow-dataflow)|
|CSV|`read_csv()`|[Verwijzing](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep#read-csv-path--filepath--separator--str--------header--azureml-dataprep-api-dataflow-promoteheadersmode----promoteheadersmode-constantgrouped--3---encoding--azureml-dataprep-api-engineapi-typedefinitions-fileencoding----fileencoding-utf8--0---quoting--bool---false--inference-arguments--azureml-dataprep-api-builders-inferencearguments---none--skip-rows--int---0--skip-mode--azureml-dataprep-api-dataflow-skipmode----skipmode-none--0---comment--str---none--include-path--bool---false--archive-options--azureml-dataprep-api--archiveoption-archiveoptions---none--infer-column-types--bool---false--verify-exists--bool---true-----azureml-dataprep-api-dataflow-dataflow)|
|Excel|`read_excel()`|[Verwijzing](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep#read-excel-path--filepath--sheet-name--str---none--use-column-headers--bool---false--inference-arguments--azureml-dataprep-api-builders-inferencearguments---none--skip-rows--int---0--include-path--bool---false--infer-column-types--bool---false--verify-exists--bool---true-----azureml-dataprep-api-dataflow-dataflow)|
|Vaste breedte|`read_fwf()`|[Verwijzing](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep#read-fwf-path--filepath--offsets--typing-list-int---header--azureml-dataprep-api-dataflow-promoteheadersmode----promoteheadersmode-constantgrouped--3---encoding--azureml-dataprep-api-engineapi-typedefinitions-fileencoding----fileencoding-utf8--0---inference-arguments--azureml-dataprep-api-builders-inferencearguments---none--skip-rows--int---0--skip-mode--azureml-dataprep-api-dataflow-skipmode----skipmode-none--0---include-path--bool---false--infer-column-types--bool---false--verify-exists--bool---true-----azureml-dataprep-api-dataflow-dataflow)|
|JSON|`read_json()`|[Verwijzing](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep?view=azure-dataprep-py#read-json-path--filepath--encoding--azureml-dataprep-api-engineapi-typedefinitions-fileencoding----fileencoding-utf8--0---flatten-nested-arrays--bool---false--include-path--bool---false-----azureml-dataprep-api-dataflow-dataflow)|

## <a name="load-data-automatically"></a>Automatisch gegevens laden

Voor het laden van gegevens automatisch zonder het bestandstype op te geven, gebruikt u de `auto_read_file()` functie. Het type van het bestand en de argumenten die zijn vereist voor het lezen van deze automatisch worden afgeleid.

```python
import azureml.dataprep as dprep

dflow = dprep.auto_read_file(path='./data/any-file.txt')
```

Deze functie is handig voor het bestandstype, codering en andere parseren argumenten allemaal vanaf één handige invoerpunt automatisch te detecteren. De functie voert ook automatisch de volgende stappen uit die vaak worden uitgevoerd bij het laden van gegevens met scheidingstekens:

* Voorbeeldgrootte en het instellen van het scheidingsteken
* Lege records aan de bovenkant van het bestand wordt overgeslagen
* Voorbeeldgrootte en het instellen van de rij met koppen

Als u bekend bent met het bestand typt u vooraf en expliciet bepalen hoe die deze wordt geparseerd wilt, u kunt ook de bestand-specifieke functies gebruiken.

## <a name="load-text-line-data"></a>Laden van gegevens van de tekst

Als u wilt lezen eenvoudige tekst in een gegevensstroom, gebruikt u de `read_lines()` zonder optionele parameters op te geven.

```python
dflow = dprep.read_lines(path='./data/text_lines.txt')
dflow.head(5)
```

||Regel|
|----|-----|
|0|Datum \| \| Minimum temperatuur \| \| hoogste temperatuur|
|1|1-07-2015 \| \| -4.1 \| \| 10.0|
|2|2015-07-2 \| \| -0,8 \| \| 10.8|


Nadat de gegevens worden opgenomen, voert u de volgende code om te converteren van de gegevensstroom-object in een Pandas dataframe.

```python
pandas_df = dflow.to_pandas_dataframe()
```

## <a name="load-csv-data"></a>Laad CSV-gegevens

Bij het lezen van bestanden met scheidingstekens afleiden de onderliggende runtime de parseren parameters (scheidingsteken, codering, of u wilt gebruiken, kopteksten, enzovoort). Voer de volgende code om te lezen van een CSV-bestand door alleen de locatie op te geven.

```python
dflow = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv?st=2018-06-15T23%3A01%3A42Z&se=2019-06-16T23%3A01%3A00Z&sp=r&sv=2017-04-17&sr=b&sig=ugQQCmeC2eBamm6ynM7wnI%2BI3TTDTM6z9RPKj4a%2FU6g%3D')
dflow.head(5)
```

| |stnam|fipst|leaid|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|------|-----|
|0|stnam|fipst|leaid|leanm10|ncessch|MAM_MTH00numvalid_1011|
|1|ALABAMA|1|101710|Hale County|10171002158| |
|2|ALABAMA|1|101710|Hale County|10171002162| |


Als u wilt uitsluiten lijnen tijdens het laden, bepalen de `skip_rows` parameter. Deze parameter wordt geladen rijen aflopend in het CSV-bestand (met behulp van een index op basis van een) overgeslagen.

```python
dflow = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv',
                          skip_rows=1)
dflow.head(5)
```

| |stnam|fipst|leaid|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|------|
|0|ALABAMA|1|101710|Hale County|10171002158|29|
|1|ALABAMA|1|101710|Hale County|10171002162|40 |

Voer de volgende code om de kolom-gegevenstypen weer te geven.

```python
dflow.dtypes
```
Uitvoer:

    stnam                     object
    fipst                     object
    leaid                     object
    leanm10                   object
    ncessch                   object
    schnam10                  object
    MAM_MTH00numvalid_1011    object
    dtype: object

Standaard wordt de Azure Machine Learning Data Prep SDK de gegevens van het type niet gewijzigd. De gegevensbron die u leest is een tekstbestand, zodat de SDK alle waarden als tekenreeksen worden gelezen. In dit voorbeeld moeten de numerieke kolommen as-nummers worden geanalyseerd. Stel de `inference_arguments` parameter `InferenceArguments.current_culture()` automatisch afleiden en de kolommen van het type converteren tijdens lezen van het bestand.

```
dflow = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv',
                          skip_rows=1,
                          inference_arguments=dprep.InferenceArguments.current_culture())
dflow.dtypes
```
Uitvoer:

    stnam                      object
    fipst                     float64
    leaid                     float64
    leanm10                    object
    ncessch                   float64
    schnam10                   object
    ALL_MTH00numvalid_1011    float64
    dtype: object


Aantal van de kolommen goed zijn gedetecteerd als numerieke en hun type is ingesteld op `float64`.

## <a name="use-excel-data"></a>Excel-gegevens gebruiken

De SDK bevat een `read_excel()` functie voor het laden van Excel-bestanden. De functie wordt standaard het eerste blad in de werkmap worden geladen. Voor het definiëren van een bepaald blad te laden, bepalen de `sheet_name` parameter met de tekenreekswaarde van de naam van het blad.

```python
dflow = dprep.read_excel(path='./data/excel.xlsx', sheet_name='Sheet2')
dflow.head(5)
```

| |Kolom1|Kolom2|Kolom3|Kolom4|Column5|Kolom6|Column7|Column8| | |
|-|-------|-------|-------|-------|-------|-------|-------|-------|-|-|
|0|Geen|Geen|Geen|Geen|Geen|Geen|Geen|Geen|Geen| |
|1|Geen|Geen|Geen|Geen|Geen|Geen|Geen|Geen|Geen| |
|2|Geen|Geen|Geen|Geen|Geen|Geen|Geen|Geen|Geen| |
|3|positie|Titel|Studio|Wereldwijd|Binnenlandse / %|Kolom1|LGO / %|Kolom2|Jaar ^| |
|4|1|Avatar|Fox|2788|760.5|0.273|2027.5|0.727|2009 ^|5|

De uitvoer ziet u dat de gegevens in het tweede blad al drie lege rijen voordat u de headers. De `read_excel()` functie bevat de volgende optionele parameters voor het overslaan van rijen en met behulp van headers. Voer de volgende code om over te slaan van de eerste drie rijen uit en gebruik van de vierde rij als de headers.

```python
dflow = dprep.read_excel(path='./data/excel.xlsx', sheet_name='Sheet2', use_column_headers=True, skip_rows=3)
```

||positie|Titel|Studio|Wereldwijd|Binnenlandse / %|Kolom1|LGO / %|Kolom2|Jaar ^|
|------|------|------|-----|------|-----|-------|----|-----|-----|
|0|1|Avatar|Fox|2788|760.5|0.273|2027.5|0.727|2009 ^|
|1|2|Titanic|Par.|2186.8|658.7|0.301|1528.1|0.699|1997 ^|

## <a name="load-fixed-width-data-files"></a>Laden van bestanden met vast breedte

Als u wilt laden vaste breedte, moet u een lijst met verschuivingen teken opgeven. De eerste kolom wordt altijd uitgegaan om te beginnen bij verschuiving van nul.

```python
dflow = dprep.read_fwf('./data/fixed_width_file.txt', offsets=[7, 13, 43, 46, 52, 58, 65, 73])
dflow.head(5)
```

||010000|99999|ONJUISTE NOORWEGEN|NO|NO_1|ENRS|Column7|Column8|Column9|
|------|------|------|-----|------|-----|-------|----|-----|----|
|0|010003|99999|ONJUISTE NOORWEGEN|NO|NO|ENSO||||
|1|010010|99999|JAN MAYEN|NO|JN|ENJA|+70933|-008667|+00090|


Om te voorkomen van koptekst van detectie en de juiste gegevens parseren, doorgeven `PromoteHeadersMode.NONE` naar de `header` parameter.

```python
dflow = dprep.read_fwf('./data/fixed_width_file.txt',
                          offsets=[7, 13, 43, 46, 52, 58, 65, 73],
                          header=dprep.PromoteHeadersMode.NONE)
```

||Kolom1|Kolom2|Kolom3|Kolom4|Column5|Kolom6|Column7|Column8|Column9|
|------|------|------|-----|------|-----|-------|----|-----|----|
|0|010000|99999|ONJUISTE NOORWEGEN|NO|NO_1|ENRS|Column7|Column8|Column9|
|1|010003|99999|ONJUISTE NOORWEGEN|NO|NO|ENSO||||


## <a name="load-sql-data"></a>SQL-gegevens laden

De SDK kunt u ook gegevens laden van een SQL-bron. Op dit moment wordt alleen Microsoft SQL Server ondersteund. Als u wilt lezen van gegevens uit een SQL-server maken een [ `MSSQLDataSource` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.mssqldatasource?view=azure-dataprep-py) object met de verbindingsparameters. De parameter password van `MSSQLDataSource` accepteert een [ `Secret` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep?view=azure-dataprep-py#register-secret-value--str--id--str---none-----azureml-dataprep-api-engineapi-typedefinitions-secret) object. U kunt een geheim object op twee manieren maken:

* Registreren en de waarde van het geheim met de engine voor het uitvoeren.
* Het geheim te maken met slechts een `id` (of de geheime waarde al is geregistreerd in de uitvoeringsomgeving) met behulp van `dprep.create_secret("[SECRET-ID]")`.

```python
secret = dprep.register_secret(value="[SECRET-PASSWORD]", id="[SECRET-ID]")

ds = dprep.MSSQLDataSource(server_name="[SERVER-NAME]",
                           database_name="[DATABASE-NAME]",
                           user_name="[DATABASE-USERNAME]",
                           password=secret)
```

Nadat u een object voor een gegevensbron hebt gemaakt, kunt u doorgaan met het lezen van gegevens van de query-uitvoer.

```python
dflow = dprep.read_sql(ds, "SELECT top 100 * FROM [SalesLT].[Product]")
dflow.head(5)
```

| |Product-id|Name|ProductNumber|Kleur|StandardCost|ListPrice|Grootte|Gewicht|ProductCategoryID|ProductModelID|SellStartDate|SellEndDate|DiscontinuedDate|Thumbnailphoto wordt door|ThumbnailPhotoFileName|ROWGUID|ModifiedDate| |
|-|---------|----|-------------|-----|------------|---------|----|------|-----------------|--------------|-------------|-----------|----------------|--------------|----------------------|-------|------------|-|
|0|680|HL-Fietsframe - zwart, 58|FR-R92B-58|Zwart|1059.3100|1431.50|58|1016.04|18|6|2002-06-01 00:00:00 + 00:00 uur|Geen|Geen|b'GIF89aP\x001\x00\xf7\x00\x00\x00\x00\x00\x80...|no_image_available_small.gif|43dd68d6-14a4-461F-9069-55309d90ea7e|2008-03-11 |0:01:36.827000 + 00:00 uur|
|1|706|HL-Fietsframe - rood, 58|FR-R92R-58|Rood|1059.3100|1431.50|58|1016.04|18|6|2002-06-01 00:00:00 + 00:00 uur|Geen|Geen|b'GIF89aP\x001\x00\xf7\x00\x00\x00\x00\x00\x80...|no_image_available_small.gif|9540ff17-2712-4c90-a3d1-8ce5568b2462|2008-03-11 |10:01:36.827000 + 00:00 uur|
|2|707|Sport-100-helm, rood|HL-U509-R|Rood|13.0863|34.99|Geen|Geen|35|33|2005-07-01 00:00:00 + 00:00 uur|Geen|Geen|b'GIF89aP\x001\x00\xf7\x00\x00\x00\x00\x00\x80...|no_image_available_small.gif|2e1ef41a-c08a-4ff6-8ada-bde58b64a712|2008-03-11 |10:01:36.827000 + 00:00 uur|


## <a name="use-azure-data-lake-storage"></a>Azure Data Lake-opslag gebruiken

Er zijn twee manieren de SDK het OAuth-token nodig voor toegang tot Azure Data Lake-opslag kunt aanschaffen:

* Het toegangstoken ophalen uit een recente sessie van Azure CLI-aanmelding van de gebruiker
* Een service-principal (SP) en een certificaat als geheim gebruiken

### <a name="use-an-access-token-from-a-recent-azure-cli-session"></a>Een toegangstoken van een recente sessie van de Azure CLI gebruiken

Voer de volgende opdracht op uw lokale computer.

```azurecli
az login
az account show --query tenantId
dflow = read_csv(path = DataLakeDataSource(path='adl://dpreptestfiles.azuredatalakestore.net/farmers-markets.csv', tenant='microsoft.onmicrosoft.com')) head = dflow.head(5) head
```

> [!NOTE]
> Als uw gebruikersaccount lid van meer dan één Azure-tenant is, moet u de tenant in de vorm van de hostnaam AAD-URL opgeven.

### <a name="create-a-service-principal-with-the-azure-cli"></a>Een service-principal maken met de Azure-CLI

De Azure CLI gebruiken om een service-principal en het bijbehorende certificaat te maken. Deze specifieke service-principal is geconfigureerd met de `reader` rol, met het bereik dat is beperkt tot alleen de Azure Data Lake Storage-account 'dpreptestfiles'.

```azurecli
az account set --subscription "Data Wrangling development"
az ad sp create-for-rbac -n "SP-ADLS-dpreptestfiles" --create-cert --role reader --scopes /subscriptions/35f16a99-532a-4a47-9e93-00305f6c40f2/resourceGroups/dpreptestfiles/providers/Microsoft.DataLakeStore/accounts/dpreptestfiles
```

Met deze opdracht verzendt de `appId` en het pad naar het certificaatbestand (meestal in de basismap). Het bestand .crt bevat zowel het openbare certificaat en de persoonlijke sleutel in de PEM-indeling.

```
openssl x509 -in adls-dpreptestfiles.crt -noout -fingerprint
```

Gebruik de object-id van de gebruiker voor het configureren van de ACL voor het Azure Data Lake Storage-bestandssysteem. In dit voorbeeld wordt de service-principal gebruikt.

```azurecli
az ad sp show --id "8dd38f34-1fcb-4ff9-accd-7cd60b757174" --query objectId
```

Het configureren van `Read` en `Execute` toegang voor het Azure Data Lake Storage file system, u de ACL voor bestanden en mappen afzonderlijk configureren. Dit is vanwege het feit dat de onderliggende HDFS ACL-model biedt geen ondersteuning voor overname.

```azurecli
az dls fs access set-entry --account dpreptestfiles --acl-spec "user:e37b9b1f-6a5e-4bee-9def-402b956f4e6f:r-x" --path /
az dls fs access set-entry --account dpreptestfiles --acl-spec "user:e37b9b1f-6a5e-4bee-9def-402b956f4e6f:r--" --path /farmers-markets.csv
```

```
certThumbprint = 'C2:08:9D:9E:D1:74:FC:EB:E9:7E:63:96:37:1C:13:88:5E:B9:2C:84'
certificate = ''
with open('./data/adls-dpreptestfiles.crt', 'rt', encoding='utf-8') as crtFile:
    certificate = crtFile.read()

servicePrincipalAppId = "8dd38f34-1fcb-4ff9-accd-7cd60b757174"
```

### <a name="acquire-an-oauth-access-token"></a>Een OAuth-toegangstoken verkrijgen

Gebruik de `adal` pakket (`pip install adal`) geen verificatiecontext op de MSFT-tenant maken en een OAuth-toegangstoken verkrijgen. ADLS, moet de resource in de aanvraag voor een toegangstoken voor ' https:\//datalake.azure.net', die verschilt van de meeste andere Azure-resources.

```python
import adal
from azureml.dataprep.api.datasources import DataLakeDataSource

ctx = adal.AuthenticationContext('https://login.microsoftonline.com/microsoft.onmicrosoft.com')
token = ctx.acquire_token_with_client_certificate('https://datalake.azure.net/', servicePrincipalAppId, certificate, certThumbprint)
dflow = dprep.read_csv(path = DataLakeDataSource(path='adl://dpreptestfiles.azuredatalakestore.net/farmers-markets.csv', accessToken=token['accessToken']))
dflow.to_pandas_dataframe().head()
```

||FMID|MarketName|Website|Adres|city|District|
|----|------|-----|----|----|----|----|
|0|1012063|Caledonië landbouwers markt Association - Danville|https://sites.google.com/site/caledoniafarmers... ||Danville|Caledonië|
|1|1011871|Liefst in Stearns Homestead landbouwers ' markt|http://Stearnshomestead.com |6975 ridge weg|Parma|Cuyahoga|
|2|1011878|100 mijl markt|https://www.pfcmarkets.com |507 Harrison St|Verweggistan|Verweggistan|
|3|1009364|106 S. Main straat landbouwers markt|http://thetownofsixmile.wordpress.com/ |106 S. Main straat|Zes mijl|||
|4|1010691|10e straat Community landbouwers markt|https://agrimissouri.com/... |10e straat en populieren|Lamar|Barton|

## <a name="next-steps"></a>Volgende stappen

* Zie de SDK van Azure Machine Learning Data Prep [zelfstudie](tutorial-data-prep.md) voor een voorbeeld van een specifiek scenario oplossen
