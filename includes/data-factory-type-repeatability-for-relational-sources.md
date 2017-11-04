## <a name="repeatability-during-copy"></a>Herhaalbaarheid tijdens het kopiëren
Wanneer het kopiëren van gegevens van en naar relationele opslaat, moet u rekening moet houden herhaalbaarheid om te voorkomen dat ongewenste resultaten. 

Een segment kan worden automatisch opnieuw uitvoeren in Azure Data Factory volgens het opgegeven beleid voor opnieuw proberen. Het is raadzaam dat u een beleid voor opnieuw proberen ter bescherming tegen tijdelijke fouten ingesteld. Daarom is herhaalbaarheid een belangrijk aspect te behandelen tijdens de verplaatsing van gegevens. 

**Als een bron:**

> [!NOTE]
> De volgende voorbeelden zijn voor Azure SQL, maar zijn van toepassing op een gegevensopslag die ondersteuning biedt voor rechthoekige gegevenssets. Wellicht hebt u om aan te passen de **type** van bron- en de **query** eigenschap (bijvoorbeeld: query in plaats van sqlReaderQuery) voor de gegevens opslaan.   
> 
> 

Normaal gesproken bij het lezen van relationele winkels, zou u alleen de gegevens die overeenkomt met dat segment lezen. Een manier om dit te doen zou met behulp van de variabelen WindowStart en WindowEnd beschikbaar in Azure Data Factory zijn. Meer informatie over de variabelen en functies in Azure Data Factory hier in de [planning en uitvoering](../articles/data-factory/v1/data-factory-scheduling-and-execution.md) artikel. Voorbeeld: 

```json
"source": {
"type": "SqlSource",
"sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm\\'', WindowStart, WindowEnd)"
},
```
Deze query leest gegevens van MyTable' die in het segment duur bereik valt. Opnieuw uitvoeren van dit segment zou dit gedrag ook altijd voor zorgen. 

In andere gevallen wilt u mogelijk de gehele tabel lezen (Stel voor één keer alleen verplaatsen) en de sqlReaderQuery kan als volgt definiëren:

```json
"source": 
{            
    "type": "SqlSource",
    "sqlReaderQuery": "select * from MyTable"
},
```
