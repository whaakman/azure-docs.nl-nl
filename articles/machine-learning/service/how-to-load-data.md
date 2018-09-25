---
title: Gegevens laden met de Azure Machine Learning Data Prep SDK - Python
description: Meer informatie over het laden van gegevens met Azure Machine Learning Data Prep SDK. U kunt verschillende soorten gegevens laden, gegevens-bestandstypen en parameters opgeven of de slimme lezen SDK-functionaliteit gebruiken voor het automatisch detecteren bestandstype.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: cforbe
author: cforbe
manager: cgronlun
ms.reviewer: jmartens
ms.date: 09/24/2018
ms.openlocfilehash: 000870e3273799930f519ff32d6b072d8c2d1f10
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46989682"
---
#<a name="load-data-with-the-azure-machine-learning-data-prep-sdk"></a>Gegevens laden met de Azure Machine Learning Data Prep SDK

De [SDK van Azure Machine Learning Data Prep](https://docs.microsoft.com/python/api/overview/azure/dataprep?view=azure-dataprep-py) kunt u verschillende soorten invoergegevens. U kunt opgeven van het gegevenstype van het bestand en de bijbehorende parameters of de functionaliteit van de slimme lezen SDK gebruiken voor het automatisch detecteren van het type van een bestand.

## <a name="read-lines"></a>Regels lezen
Een van de eenvoudigste manieren om gegevens te laden is te lezen als tekstregels.

```
dataflow = dprep.read_lines(path='./data/text_lines.txt')
dataflow.head(5)
```
||Regel|
|----|-----|
|0|Datum \| \| Minimum temperatuur \| \| hoogste temperatuur|
|1|1-07-2015 \| \| -4.1 \| \| 10.0|
|2|2015-07-2 \| \| -0,8 \| \| 10.8|
|3|2015-07-3 \| \| -7.0 \| \| 10,5|
|4|2015-07-4 \| \| -5.5 \| \| 9.3|

Nadat de gegevens worden opgenomen, kunt u een pandas DataFrame voor de volledige gegevensset ophalen.

```
df = dataflow.to_pandas_dataframe()
df
```

||Regel|
|----|-----|
|0|Datum\| \| Minimum temperatuur\| \| hoogste temperatuur|
|1|1-07-2015\| \| 4.1\| \| 10.0|
|2|2015-07-2\| \| 0,8\| \| 10.8|
|3|2015-07-3\| \| 7.0\| \| 10,5|
|4|2015-07-4\| \| 5.5\| \| 9.3|

## <a name="read-csv"></a>Lezen van CSV
Bij het lezen van bestanden met scheidingstekens, kunt u de onderliggende runtime afleiden van de parameters bij het parseren (zoals een scheidingsteken, codering, of u wilt gebruiken, kopteksten, enzovoort) in plaats van ze bieden. In dit voorbeeld probeert een bestand lezen door alleen de locatie op te geven. 

```
# SAS expires June 16th, 2019
dataflow = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv?st=2018-06-15T23%3A01%3A42Z&se=2019-06-16T23%3A01%3A00Z&sp=r&sv=2017-04-17&sr=b&sig=ugQQCmeC2eBamm6ynM7wnI%2BI3TTDTM6z9RPKj4a%2FU6g%3D')
dataflow.head(5)
```

| |stnam|fipst|leaid|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|------|-----|
|0||stnam|fipst|leaid|leanm10|ncessch|MAM_MTH00numvalid_1011|
|1|ALABAMA|1|101710|Hale County|10171002158| |
|2|ALABAMA|1|101710|Hale County|10171002162| |
|3|ALABAMA|1|101710|Hale County|10171002156| |
|4|ALABAMA|1|101710|Hale County|10171000588|2|

Een van de parameters die u kunt opgeven is een aantal regels om over te slaan in de bestanden die u leest. Gebruik de volgende code uit de dubbele regel te filteren.
```
dataflow = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv',
                          skip_rows=1)
dataflow.head(5)
```

| |stnam|fipst|leaid|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|------|-----|
|0|ALABAMA|1|101710|Hale County|10171002158|29|
|1|ALABAMA|1|101710|Hale County|10171002162|40 |
|2|ALABAMA|1|101710|Hale County|10171002156| 43|
|3|ALABAMA|1|101710|Hale County|10171000588|2|
|4|ALABAMA|1|101710|Hale County|10171000589|23 |

Vervolgens kunt u de gegevenstypen van de kolommen bekijken.
```
dataflow.head(1).dtypes

stnam                     object
fipst                     object
leaid                     object
leanm10                   object
ncessch                   object
schnam10                  object
MAM_MTH00numvalid_1011    object
dtype: object
```
Helaas, alle van de kolommen zijn teruggekomen als tekenreeksen. Dit is omdat standaard de Azure Machine Learning Data Prep SDK niet de gegevens van het type wordt gewijzigd. We van lezen gegevensbron is een tekstbestand, zodat de SDK alle waarden als tekenreeksen worden gelezen. In dit voorbeeld, willen we numerieke kolommen as-nummers worden geparseerd. Om dit te doen, kunt u de parameter inference_arguments ingesteld op current_culture.

```
dataflow = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv',
                          skip_rows=1,
                          inference_arguments=dprep.InferenceArguments.current_culture())
dataflow.head(1).dtypes

stnam                      object
fipst                     float64
leaid                     float64
leanm10                    object
ncessch                   float64
schnam10                   object
ALL_MTH00numvalid_1011    float64
dtype: object
```
Aantal van de kolommen goed as-nummers zijn gedetecteerd en hun type is ingesteld op float64. Met gegevensopname gedaan, kunt u een pandas DataFrame voor de volledige gegevensset ophalen.

```
df = dataflow.to_pandas_dataframe()
df
```

| |stnam|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|
|0|ALABAMA|Hale County|1.017100e + 10|49,0|
|1|ALABAMA|Hale County|1.017100e + 10|40,0|
|2|ALABAMA|Hale County|1.017100e + 10|43.0|
|3|ALABAMA|Hale County|1.017100e + 10|2.0|
|4|ALABAMA|Hale County|1.017100e + 10|23.0|

## <a name="read-excel"></a>Alleen Excel
De Azure Machine Learning Data Prep SDK bevat een `read_excel` functie voor het laden van Excel-bestanden.
```
dataflow = dprep.read_excel(path='./data/excel.xlsx')
dataflow.head(5)
```
||Kolom1|Kolom2|Kolom3|Kolom4|Column5|Kolom6|Column7|Column8|
|------|------|------|-----|------|-----|-------|----|-----|
|0|HOBA|Ijzer, IVB|60000000.0|Gevonden|1920.0|http://www.lpi.usra.edu/meteor/metbull.php?cod... |-19.58333|17.91667|
|1|Cabo York|Ijzer, IIIAB|58200000.0|Gevonden|1818.0|http://www.lpi.usra.edu/meteor/metbull.php?cod... |76.13333|-64.93333|
|2|Campo del Cielo|Ijzer, IAB-MG|50000000.0|Gevonden|1576.0|http://www.lpi.usra.edu/meteor/metbull.php?cod... |-27.46667|-60.58333|
|3|Canyon Diablo|Ijzer, IAB-MG|30000000.0|Gevonden|1891.0|http://www.lpi.usra.edu/meteor/metbull.php?cod... |35.05000|-111.03333|
|4|Armanty|Ijzer, IIIE|28000000.0|Gevonden|1898.0|http://www.lpi.usra.edu/meteor/metbull.php?cod... |47.00000|88.00000|

U kunt het eerste blad van het Excel-bestand hebt geladen. U kunt hetzelfde resultaat bereiken door expliciet op te geven de naam van het spreadsheet dat u wilt laden. Als u het tweede blad in plaats daarvan worden geladen wilt, kunt u de naam opgeven als argument.
```
dataflow = dprep.read_excel(path='./data/excel.xlsx', sheet_name='Sheet2')
dataflow.head(5)
```

||Kolom1|Kolom2|Kolom3|Kolom4|Column5|Kolom6|Column7|Column8|
|------|------|------|-----|------|-----|-------|----|-----|
|0|Geen|Geen|Geen|Geen|Geen|Geen|Geen|Geen|Geen|
|1|Geen|Geen|Geen|Geen|Geen|Geen|Geen|Geen|Geen|
|2|Geen|Geen|Geen|Geen|Geen|Geen|Geen|Geen|Geen|
|3|positie|Titel|Studio|Wereldwijd|Binnenlandse / %|Kolom1|LGO / %|Kolom2|Jaar ^|
|4|1|Avatar|Fox|2788|760.5|0.273|2027.5|0.727|2009 ^|5|

Zoals u ziet, heeft de tabel in het tweede blad kopteksten en drie lege rijen. U moet de argumenten van de functie dienovereenkomstig wijzigen.
```
dataflow = dprep.read_excel(path='./data/excel.xlsx', sheet_name='Sheet2', use_header=True, skip_rows=3)
df = dataflow.to_pandas_dataframe()
df
```

||positie|Titel|Studio|Wereldwijd|Binnenlandse / %|Kolom1|LGO / %|Kolom2|Jaar ^|
|------|------|------|-----|------|-----|-------|----|-----|-----|
|0|1|Avatar|Fox|2788|760.5|0.273|2027.5|0.727|2009 ^|
|1|2|Titanic|Par.|2186.8|658.7|0.301|1528.1|0.699|1997 ^|
|2|3|De Marvel Avengers|BV|1518.6|623.4|0.41|895.2|0.59|2012|
|3|4|Harry Potter en de Deathly Hallows deel 2|WB GELIJK GEMAAKT|1341.5|381|0.284|960.5|0.716|2011|
|4|5|Geblokkeerd|BV|1274.2|400.7|0.314|873.5|0.686|2013|

## <a name="read-fixed-width-files"></a>Vaste breedte bestanden lezen
U kunt een lijst met verschuivingen opgeven voor vaste breedte-bestanden. De eerste kolom wordt altijd uitgegaan om te beginnen bij offset 0.

```
dataflow = dprep.read_fwf('./data/fixed_width_file.txt', offsets=[7, 13, 43, 46, 52, 58, 65, 73])
dataflow.head(5)
```
||010000|99999|ONJUISTE NOORWEGEN|NEE|NO_1|ENRS|Column7|Column8|Column9|
|------|------|------|-----|------|-----|-------|----|-----|----|
|0|010003|99999|ONJUISTE NOORWEGEN|NEE|NEE|ENSO||||
|1|010010|99999|JAN MAYEN|NEE|JN|ENJA|+70933|-008667|+00090|
|2|010013|99999|ROST|NEE|NEE|||||
|3|010014|99999|SOERSTOKKEN|NEE|NEE|ENSO|+59783|+005350|+00500|
|4|010015|99999|BRINGELAND|NEE|NEE|ENBL|+61383|+005867|+03270|


Als er geen kopteksten in de bestanden zijn, moet u de eerste rij als gegevens worden behandeld. Door door te geven `PromoteHeadersMode.NONE` aan het argument van het sleutelwoord header kunt u detectie van de koptekst te voorkomen en de juiste gegevens ophalen.

```
dataflow = dprep.read_fwf('./data/fixed_width_file.txt',
                          offsets=[7, 13, 43, 46, 52, 58, 65, 73],
                          header=dprep.PromoteHeadersMode.NONE)

df = dataflow.to_pandas_dataframe()
df
```
||Kolom1|Kolom2|Kolom3|Kolom4|Column5|Kolom6|Column7|Column8|Column9|
|------|------|------|-----|------|-----|-------|----|-----|----|
|0|010000|99999|ONJUISTE NOORWEGEN|NEE|NO_1|ENRS|Column7|Column8|Column9|
|1|010003|99999|ONJUISTE NOORWEGEN|NEE|NEE|ENSO||||
|2|010010|99999|JAN MAYEN|NEE|JN|ENJA|+70933|-008667|+00090|
|3|010013|99999|ROST|NEE|NEE|||||
|4|010014|99999|SOERSTOKKEN|NEE|NEE|ENSO|+59783|+005350|+00500|
|5|010015|99999|BRINGELAND|NEE|NEE|ENBL|+61383|+005867|+03270|

## <a name="read-sql"></a>SQL lezen
De SDK van Azure Machine Learning Data Prep kan ook gegevens uit SQL-servers laden. Op dit moment wordt alleen Microsoft SQL Server ondersteund.
Als u wilt lezen van gegevens uit een SQL-server maken object voor een gegevensbron die de verbindingsgegevens bevat.

```
secret = dprep.register_secret("[SECRET-USERNAME]", "[SECRET-PASSWORD]")

ds = dprep.MSSQLDataSource(server_name="[SERVER-NAME]",
                           database_name="[DATABASE-NAME]",
                           user_name="[DATABASE-USERNAME]",
                           password=[DATABASE-PASSWORD])
```
Zoals u kunt zien, de parameter password van `MSSQLDataSource` accepteert een geheime-object. U kunt een geheim object krijgen op twee manieren:
-   Registreren en de waarde van het geheim met de engine voor het uitvoeren. 
-   Het geheim met alleen-ID (dit is handig als de geheime waarde is al geregistreerd in de uitvoeringsomgeving) maken.

Nadat u een object voor een gegevensbron hebt gemaakt, kunt u doorgaan met het lezen van gegevens.
```
dataflow = dprep.read_sql(ds, "SELECT top 100 * FROM [SalesLT].[Product]")
dataflow.head(5)
```

||Product-id|Naam|ProductNumber|Kleur|StandardCost|ListPrice|Grootte|Gewicht|ProductCategoryID|ProductModelID|SellStartDate|SellEndDate|DiscontinuedDate|Thumbnailphoto wordt door|ThumbnailPhotoFileName|ROWGUID|ModifiedDate|
|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|
|0|680|HL-Fietsframe - zwart, 58|FR-R92B-58|Zwart|1059.3100|1431.50|58|1016.04|18|6|2002-06-01 00:00:00 + 00:00 uur|Geen|Geen|b'GIF89aP\x001\x00\xf7\x00\x00\x00\x00\x00\x80...|no_image_available_small.gif|43dd68d6-14a4-461F-9069-55309d90ea7e|2008-03-11 |0:01:36.827000 + 00:00 uur|
|1|706|HL-Fietsframe - rood, 58|FR-R92R-58|Rood|1059.3100|1431.50|58|1016.04|18|6|2002-06-01 00:00:00 + 00:00 uur|Geen|Geen|b'GIF89aP\x001\x00\xf7\x00\x00\x00\x00\x00\x80...|no_image_available_small.gif|9540ff17-2712-4c90-a3d1-8ce5568b2462|2008-03-11 |10:01:36.827000 + 00:00 uur|
|2|707|Sport-100-helm, rood|HL-U509-R|Rood|13.0863|34.99|Geen|Geen|35|33|2005-07-01 00:00:00 + 00:00 uur|Geen|Geen|b'GIF89aP\x001\x00\xf7\x00\x00\x00\x00\x00\x80...|no_image_available_small.gif|2e1ef41a-c08a-4ff6-8ada-bde58b64a712|2008-03-11 |10:01:36.827000 + 00:00 uur|
|3|708|Sport-100-helm, zwart|HL-U509|Zwart|13.0863|34.99|Geen|Geen|35|33|2005-07-01 00:00:00 + 00:00 uur|Geen|Geen|b'GIF89aP\x001\x00\xf7\x00\x00\x00\x00\x00\x80...|no_image_available_small.gif|a25a44fb-c2de-4268-958f-110b8d7621e2|2008-03-11 |10:01:36.827000 + 00:00 uur|
|4|709|Mountainbikesokken, M|DERGELIJKE-B909-M|Wit|3.3963|9,50|M|Geen|27|18|2005-07-01 00:00:00 + 00:00 uur|2006-06-30 00:00:00 + 00:00 uur|Geen|b'GIF89aP\x001\x00\xf7\x00\x00\x00\x00\x00\x80...|no_image_available_small.gif|18f95f47-1540-4e02-8f1f-cc1bcb6828d0|2008-03-11 |10:01:36.827000 + 00:00 uur|

```
df = dataflow.to_pandas_dataframe()
df.dtypes
```
```
ProductID                                     int64
Name                                         object
ProductNumber                                object
Color                                        object
StandardCost                                float64
ListPrice                                   float64
Size                                         object
Weight                                      float64
ProductCategoryID                             int64
ProductModelID                                int64
SellStartDate             datetime64[ns, UTC+00:00]
SellEndDate                                  object
DiscontinuedDate                             object
ThumbNailPhoto                               object
ThumbnailPhotoFileName                       object
rowguid                                      object
ModifiedDate              datetime64[ns, UTC+00:00]
dtype: object
```

## <a name="read-from-azure-data-lake-storage"></a>Lezen van Azure Data Lake-opslag
Er zijn twee manieren de SDK het OAuth-token nodig voor toegang tot Azure Data Lake-opslag kunt aanschaffen:
-   Het toegangstoken ophalen uit een recente aanmeldsessie van Azure CLI-aanmelding van de gebruiker
-   Een service-principal (SP) en een certificaat als geheim gebruiken

### <a name="use-an-access-token-from-a-recent-azure-cli-session"></a>Een toegangstoken van een recente sessie van de Azure CLI gebruiken
Voer de volgende opdracht op uw lokale computer:

> [!NOTE] 
> Als uw gebruikersaccount lid van meer dan één Azure-tenant is, moet u de tenant in de vorm van de hostnaam AAD-URL opgeven.


```
az login
az account show --query tenantId
dataflow = read_csv(path = DataLakeDataSource(path='adl://dpreptestfiles.azuredatalakestore.net/farmers-markets.csv', tenant='microsoft.onmicrosoft.com')) head = dataflow.head(5) head
```
### <a name="create-a-service-principal-with-the-azure-cli"></a>Een service-principal maken met de Azure-CLI
U kunt de Azure CLI een service-principal maken en het bijbehorende certificaat gebruiken. Deze specifieke service-principal is geconfigureerd als lezer, met het bereik dat is beperkt tot alleen de Azure Data Lake Storage-account 'dpreptestfiles'
```
az account set --subscription "Data Wrangling development"
az ad sp create-for-rbac -n "SP-ADLS-dpreptestfiles" --create-cert --role reader --scopes /subscriptions/35f16a99-532a-4a47-9e93-00305f6c40f2/resourceGroups/dpreptestfiles/providers/Microsoft.DataLakeStore/accounts/dpreptestfiles
```
Met deze opdracht verzendt de `appId` en het pad naar het certificaatbestand (meestal in de basismap). Het bestand .crt bevat zowel het openbare certificaat en de persoonlijke sleutel in de PEM-indeling.

De vingerafdruk van het uit te pakken.
```
openssl x509 -in adls-dpreptestfiles.crt -noout -fingerprint
```

##### <a name="configure-an-azure-data-lake-storage-account-for-the-service-principal"></a>Een Azure Data Lake Storage-account voor de service-principal configureren
Gebruik de object-id van de gebruiker of voor dit voorbeeld wordt de service-principal voor het configureren van de ACL voor het bestandssysteem van Azure Data Lake Storage:
```
az ad sp show --id "8dd38f34-1fcb-4ff9-accd-7cd60b757174" --query objectId
```
##### <a name="configure-read-and-execute-access-for-the-azure-data-lake-storage-file-system"></a>Configureer lezen en uitvoeren voor het bestandssysteem van Azure Data Lake Storage.
Omdat het onderliggende HDFS ACL-model biedt geen ondersteuning voor overname, moet u de ACL voor mappen en bestanden afzonderlijk te configureren.
```
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
#### <a name="acquire-an-oauth-access-token"></a>Een OAuth-toegangstoken verkrijgen
Gebruik de `adal` pakket (via: `pip install adal`) geen verificatiecontext op de MSFT-tenant maken en een OAuth-toegangstoken verkrijgen. ADLS, moet de resource in de aanvraag voor een toegangstoken voor 'https://datalake.azure.net', die verschilt van de meeste andere Azure-resources.

```
import adal
from azureml.dataprep.api.datasources import DataLakeDataSource

ctx = adal.AuthenticationContext('https://login.microsoftonline.com/microsoft.onmicrosoft.com')
token = ctx.acquire_token_with_client_certificate('https://datalake.azure.net/', servicePrincipalAppId, certificate, certThumbprint)
dataflow = dprep.read_csv(path = DataLakeDataSource(path='adl://dpreptestfiles.azuredatalakestore.net/farmers-markets.csv', accessToken=token['accessToken']))
dataflow.to_pandas_dataframe().head()
```

||FMID|MarketName|Website|Adres|city|District|
|----|------|-----|----|----|----|----|
|0|1012063|Caledonië landbouwers markt Association - Danville|https://sites.google.com/site/caledoniafarmers... ||Danville|Caledonië|
|1|1011871|Liefst in Stearns Homestead landbouwers ' markt|http://Stearnshomestead.com |6975 ridge weg|Parma|Cuyahoga|
|2|1011878|100 mijl markt|http://www.pfcmarkets.com |507 Harrison St|Verweggistan|Verweggistan|
|3|1009364|106 S. Main straat landbouwers markt|http://thetownofsixmile.wordpress.com/ |106 S. Main straat|Zes mijl|||
|4|1010691|10th Steet Community landbouwers markt|http://agrimissouri.com/mo-grown/grodetail.php... |10e straat en populieren|Lamar|Barton|

