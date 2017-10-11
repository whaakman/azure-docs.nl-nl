
>[!NOTE]
>Log Analytics werd voorheen Operational Insights genoemd.
>
>

De volgende limieten gelden voor Log Analytics-resources per abonnement:

| Resource | Standaardlimiet | Opmerkingen
| --- | --- | --- |
| Aantal gratis werkruimten per abonnement | 10 | Deze limiet kan niet worden verhoogd. |
| Aantal betaalde werkruimten per abonnement | N.v.t. | Er gelden limieten voor het aantal resources binnen een resourcegroep en voor het aantal resourcegroepen per abonnement | 


De volgende limieten zijn van toepassing op elke Log Analytics-werkruimte:

|  | Gratis | Standard | Premium | Zelfstandig | OMS |
| --- | --- | --- | --- | --- | --- |
| Gegevensvolume verzameld per dag |500 MB<sup>1</sup> |Geen |Geen | Geen | Geen
| Bewaarperiode van gegevens |7 dagen |1 maand |12 maanden | 1 maand<sup>2</sup> | 1 maand <sup>2</sup>|

<sup>1</sup> Als een klant de dagelijkse limiet van 500 MB voor gegevensoverdracht heeft bereikt, stopt het analyseren van gegevens. Dit wordt aan het begin van de volgende dag hervat. Een dag is gebaseerd op UTC.

<sup>2</sup> De bewaarperiode voor gegevens voor zelfstandige en OMS-prijsplannen kan worden verhoogd tot 730 dagen.

| Category | Limieten | Opmerkingen
| --- | --- | --- |
| Gegevensverzamelaar-API | Maximumgrootte voor een enkel bericht is 30 MB<br>Maximumgrootte voor veldwaarden is 32 KB | Grotere volumes splitsen in meerdere berichten<br>Velden die langer zijn dan 32 KB worden afgebroken. |
| API voor zoeken | 5000 records geretourneerd voor niet-samengevoegde gegevens<br>500.000 records voor samengevoegde gegevens | Samengevoegde gegevens is een zoekopdracht die de opdracht `measure` omvat
 
