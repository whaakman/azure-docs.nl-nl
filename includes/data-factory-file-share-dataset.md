## <a name="fileshare-dataset-type-properties"></a>Eigenschappen van gegevensset bestandsshare
Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets van de [gegevenssets maken](../articles/data-factory/v1/data-factory-create-datasets.md) artikel. Secties zoals structuur, beschikbaarheid en beleid van een gegevensset JSON zijn identiek voor alle typen van de gegevensset.

De **typeProperties** sectie verschilt voor elk type dataset. Het levert informatie die specifiek is voor het type dataset. De typeProperties sectie voor een gegevensset van het type **FileShare** gegevensset heeft de volgende eigenschappen:

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| folderPath |Subpad naar de map. Gebruik van escape-teken ' \ ' voor speciale tekens in de tekenreeks. Zie [voorbeeld gekoppelde service en gegevensset definities](#sample-linked-service-and-dataset-definitions) voor voorbeelden.<br/><br/>U kunt deze eigenschap combineren met **partitionBy** map hebben paden op basis van het segment beginnen of eindigen-datums en tijden. |Ja |
| fileName |Geef de naam van het bestand in de **folderPath** als u wilt dat de tabel om te verwijzen naar een bepaald bestand in de map. Als u geen waarde voor deze eigenschap niet opgeeft, wordt de tabel verwijst naar alle bestanden in de map.<br/><br/>Wanneer fileName niet voor een uitvoergegevensset opgegeven is, de naam van het gegenereerde bestand zou worden in de volgende indeling: <br/><br/>Gegevens. <Guid>.txt (voorbeeld: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Nee |
| fileFilter |Hiermee geeft u een filter moet worden gebruikt om een subset van de bestanden in het mappad in plaats van alle bestanden te selecteren.<br/><br/>Toegestane waarden zijn: `*` (meerdere tekens) en `?` (willekeurig teken).<br/><br/>Voorbeelden 1:`"fileFilter": "*.log"`<br/>Voorbeeld 2:`"fileFilter": 2014-1-?.txt"`<br/><br/> fileFilter geldt voor een invoergegevensset bestandsshare. Deze eigenschap wordt niet ondersteund met HDFS. |Nee |
| partitionedBy |partitionedBy kan worden gebruikt om op te geven van een dynamische folderPath, filename voor tijd reeksgegevens. Bijvoorbeeld, folderPath geparametriseerde voor elk uur van gegevens. |Nee |
| Indeling | De volgende indelingstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie [tekstindeling](#specifying-textformat), [Json-indeling](#specifying-jsonformat), [Avro-indeling](#specifying-avroformat), [Orc indeling](#specifying-orcformat), en [parketvloeren indeling](#specifying-parquetformat) secties. <br><br> Als u wilt **kopiëren van bestanden als-is** overslaan tussen bestandsgebaseerde winkels (binaire kopiëren), de sectie indeling in de definities van beide invoer en uitvoer gegevensset. |Nee |
| Compressie | Geef het type en de compressie van de gegevens. Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**; en ondersteunde niveaus: **optimale** en **snelst**. Zie voor meer informatie [geven compressie](#specifying-compression) sectie. |Nee |
| useBinaryTransfer |Geef binaire overdrachtsmodus of gebruiken. De waarde True voor binaire modus en ONWAAR ASCII. Standaardwaarde: True. Deze eigenschap kan alleen worden gebruikt wanneer de bijbehorende gekoppelde-servicetype is van het type: FtpServer. |Nee |

> [!NOTE]
> bestandsnaam en fileFilter worden niet gelijktijdig gebruikt.
>
>

### <a name="using-partionedby-property"></a>Gebruik de eigenschap partionedBy
Zoals vermeld in de vorige sectie, kunt u een dynamische folderPath, filename voor time series-gegevens met partitionedBy opgeven. U kunt dit doen met de Data Factory-macro's en de systeemvariabele SliceStart, SliceEnd die wijzen op de logische periode voor een bepaalde gegevenssegment.

Zie voor meer informatie over tijd reeks gegevenssets, planning en segmenten, [gegevenssets maken](../articles/data-factory/v1/data-factory-create-datasets.md), [planning en uitvoering](../articles/data-factory/v1/data-factory-scheduling-and-execution.md), en [pijplijnen maken](../articles/data-factory/v1/data-factory-create-pipelines.md) artikelen.

#### <a name="sample-1"></a>Voorbeeld 1:

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
In dit voorbeeld {segment} is vervangen door de opgegeven waarde van de Data Factory systeemvariabele SliceStart in de notatie (YYYYMMDDHH). De SliceStart verwijst voor het starten van de tijd van het segment. FolderPath verschilt voor elk segment. Voorbeeld: wikidatagateway/wikisampledataout/2014100103 of wikidatagateway/wikisampledataout/2014100104.

#### <a name="sample-2"></a>Voorbeeld 2:

```json
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
In dit voorbeeld worden jaar, maand, dag en tijd van de SliceStart uitgepakt in verschillende variabelen die worden gebruikt door de eigenschappen voor folderPath en de bestandsnaam.
