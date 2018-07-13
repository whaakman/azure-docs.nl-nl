## <a name="repeatability-during-copy"></a>Herhaalbaarheid tijdens het kopiëren van
Bij het kopiëren van gegevens naar Azure SQL/SQL-Server van andere gegevens worden opgeslagen moet een herhaalbaarheid Houd er rekening mee om te voorkomen dat ongewenste resultaten. 

Het kopiëren van gegevens met Azure SQL/SQL Server-Database, kopieeractiviteit, wordt standaard de gegevensset aan de sink-tabel toevoegen standaard. Bijvoorbeeld, het kopiëren van gegevens uit een CSV (door komma's gescheiden waarden) bestand gegevensbron met twee records met Azure SQL/SQL Server-Database, is dit de tabel eruit:

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
```

Stel dat u in het bronbestand zijn fouten gevonden en het aantal omlaag buis uit 2 tot 4 in het bronbestand bijgewerkt. Als u het gegevenssegment opnieuw voor deze periode uitvoeren, vindt u twee nieuwe records die zijn toegevoegd aan Azure SQL/SQL Server-Database. De hieronder wordt ervan uitgegaan dat geen van de kolommen in de tabel de primary key-beperking heeft.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

Om dit te voorkomen, moet u UPSERT semantiek opgeven door gebruik te maken van een van de onderstaande 2 mechanismen die hieronder vermeld.

> [!NOTE]
> Een segment kan automatisch worden opnieuw uitgevoerd in Azure Data Factory aan de hand van het beleid dat is opgegeven.
> 
> 

### <a name="mechanism-1"></a>Methode 1
U kunt gebruikmaken van **sqlWriterCleanupScript** eigenschap eerst opschonen actie uitvoeren wanneer een segment wordt uitgevoerd. 

```json
"sink":  
{ 
  "type": "SqlSink", 
  "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
}
```

Het script voor opschoning zou worden uitgevoerd tijdens het kopiëren van een bepaald segment die de gegevens wilt verwijderen uit de SQL-tabel die overeenkomt met dat segment eerste. De activiteit wordt vervolgens de gegevens invoegen in de SQL-tabel. 

Als het segment is nu opnieuw uitvoeren, dan hebt u dat de hoeveelheid wordt bijgewerkt vindt als gewenst.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

Stel dat de platte was-record wordt verwijderd uit de oorspronkelijke CSV-bestand. Het segment opnieuw uit te voeren, zou het volgende resultaat opleveren: 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
7     Down Tube    4            2015-05-01 00:00:00
```
Momenteel niks nieuws moest worden uitgevoerd. De kopieeractiviteit is het script voor opschoning als u wilt verwijderen van de bijbehorende gegevens voor dat segment. Vervolgens wordt de invoer uit het CSV-bestand lezen (die vervolgens bevat slechts 1 record) en in de tabel ingevoegd. 

### <a name="mechanism-2"></a>Mechanisme 2
> [!IMPORTANT]
> sliceIdentifierColumnName wordt niet ondersteund voor Azure SQL Data Warehouse op dit moment. 

Een ander mechanisme voor het bereiken van herhaalbaarheid is door een specifieke kolom (**sliceIdentifierColumnName**) in de doel-tabel. Deze kolom kan worden gebruikt door Azure Data Factory om te controleren of dat de bron- en gesynchroniseerd blijven. Deze methode werkt wanneer er flexibiliteit bij het wijzigen van of het doelschema van de SQL-tabel te definiëren. 

Deze kolom zou voor herhaalbaarheid doeleinden worden gebruikt door Azure Data Factory en in de Azure Data Factory wordt geen schemawijzigingen aanbrengen aan de tabel. Manier voor het gebruik van deze benadering:

1. Een kolom van het type binaire (32) in de doel-SQL-tabel definieert. Er mag geen beperkingen voor deze kolom. We noemen deze kolom als 'ColumnForADFuseOnly' in dit voorbeeld.
2. Gebruik deze als volgt in de kopieeractiviteit:
   
    ```json
    "sink":  
    { 
   
        "type": "SqlSink", 
        "sliceIdentifierColumnName": "ColumnForADFuseOnly"
    }
    ```

Azure Data Factory vult deze kolom aan de hand van de noodzaak om te controleren of dat de bron- en gesynchroniseerd blijven. De waarden van deze kolom mag niet buiten deze context worden gebruikt door de gebruiker. 

Net als bij mechanisme 1, Kopieeractiviteit wordt automatisch opschonen eerst de gegevens voor het opgegeven segment van de doel-SQL-tabel en voer vervolgens de kopieeractiviteit normaal gesproken voor het invoegen van de gegevens van bron naar bestemming voor dat segment. 

