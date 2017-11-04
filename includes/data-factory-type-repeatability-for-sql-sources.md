## <a name="repeatability-during-copy"></a>Herhaalbaarheid tijdens het kopiëren
Bij het kopiëren van gegevens naar Azure SQL/SQL-Server van andere gegevens worden opgeslagen, moet één rekening moet houden herhaalbaarheid om te voorkomen dat ongewenste resultaten. 

Bij het kopiëren van gegevens naar Azure SQL/SQL Server-Database, wordt de kopieerbewerking standaard APPEND de gegevensset naar de tabel sink standaard. Bijvoorbeeld, als u gegevens uit een CSV (door komma's gescheiden waarden) bestand gegevensbron met twee records met Azure SQL/SQL Server-Database, is dit de tabel ziet er als:

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
```

Stel dat u fouten gevonden in de bron-bestand en de hoeveelheid omlaag buis uit 2 tot 4 in het bronbestand bijgewerkt. Als u het gegevenssegment opnieuw voor deze periode uitvoeren, vindt u twee nieuwe records die zijn toegevoegd aan Azure SQL/SQL Server-Database. De hieronder wordt ervan uitgegaan dat geen van de kolommen in de tabel de primary key-beperking heeft.

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
> Een segment kan automatisch worden opnieuw uitgevoerd in Azure Data Factory volgens het opgegeven beleid voor opnieuw proberen.
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

Het script voor opschoning zou worden uitgevoerd eerste tijdens het kopiëren van een bepaald segment dat de gegevens in de SQL-tabel overeenkomt met dat segment wilt verwijderen. De activiteit wordt vervolgens de gegevens in de SQL-tabel invoegen. 

Als het segment is nu opnieuw uitvoeren, dan hebt u dat de hoeveelheid wordt bijgewerkt vindt als gewenst.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

Stel dat de platte was is verwijderd uit de oorspronkelijke csv. Het segment opnieuw uit te voeren, zou het volgende resultaat opleveren: 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
7     Down Tube    4            2015-05-01 00:00:00
```
Niets nieuwe moest worden uitgevoerd. Het script voor opschoning van voor het verwijderen van de bijbehorende gegevens voor dat segment hebt uitgevoerd met de kopieerbewerking. Vervolgens wordt de invoer uit de csv lezen (inhoudt vervolgens slechts 1 record) en het ingevoegd in de tabel. 

### <a name="mechanism-2"></a>Mechanisme 2
> [!IMPORTANT]
> sliceIdentifierColumnName wordt niet ondersteund voor Azure SQL Data Warehouse op dit moment. 

Een ander mechanisme herhaalbaarheid bereiken is door een specifieke kolom (**sliceIdentifierColumnName**) in de doel-tabel. In deze kolom zou worden gebruikt door Azure Data Factory om te controleren of dat de bron- en doelserver gesynchroniseerd blijven. Deze methode werkt wanneer er flexibiliteit bij het definiëren van het schema van de SQL-tabel doel of wijzigen. 

In deze kolom wordt gebruikt door Azure Data Factory voor herhaalbaarheid doeleinden en in het proces Azure Data Factory wordt geen schemawijzigingen aanbrengen aan de tabel. Manier voor het gebruik van deze benadering:

1. Definieer een kolom van het type binaire (32) in de doel-SQL-tabel. Er mag geen beperkingen voor deze kolom. Laten we in deze kolom als 'ColumnForADFuseOnly' naam voor dit voorbeeld.
2. Gebruik deze als volgt in de kopieeractiviteit:
   
    ```json
    "sink":  
    { 
   
        "type": "SqlSink", 
        "sliceIdentifierColumnName": "ColumnForADFuseOnly"
    }
    ```

Azure Data Factory wordt gevuld in deze kolom aan de hand van de noodzaak om te controleren of dat de bron- en doelserver gesynchroniseerd blijven. De waarden van deze kolom mag niet buiten deze context worden gebruikt door de gebruiker. 

Net als bij mechanisme 1, Kopieeractiviteit wordt automatisch opschonen eerst de gegevens voor het opgegeven segment van de doel-SQL-tabel en voer vervolgens de kopieeractiviteit om de normale invoegen van de gegevens van bron naar doel voor dat segment. 

