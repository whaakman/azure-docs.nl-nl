De volgende tabel beschrijft elke van de belangrijkste quota's, limieten, standaardwaarden en vertragingen in Azure Scheduler.

| Resource | Beschrijving van de limiet |
| --- | --- |
| **Grootte van de taak** |Maximale grootte is 16 kB. Als een PUT- of een PATCH in een taak die groter zijn dan deze limieten resulteert, wordt een statuscode 400 Ongeldige aanvraag geretourneerd. |
| **Grootte van de aanvraag-URL** |Maximale grootte van de aanvraag-URL is 2048 tekens. |
| **Cumulatieve header-grootte** |Maximale totale koptekstgrootte is 4096 tekens. |
| **Header-aantal** |Maximale header-count is 50 headers. |
| **Grootte van standaardberichthoofdtekst** |Hoofdtekst van de maximale grootte is 8192 tekens. |
| **Bereik van terugkeerpatroon** |Maximale terugkeerpatroon span is 18 maanden. |
| **Tijd tot tijd** |Maximaal aantal 'time begintijd' is 18 maanden. |
| **Jobgeschiedenis** |Maximale antwoordtekst die zijn opgeslagen in de taakgeschiedenis is 2048 bytes. |
| **Frequentie** |De maximale frequentie standaardquotum is een gratis taakverzameling 1 uur en 1 minuut in een standard taakcollectie. De maximale frequentie kan worden geconfigureerd op een taakverzameling moet lager zijn dan het maximum. Alle taken in de taakverzameling zijn beperkt de waarde voor de taakverzameling is ingesteld. Als u probeert te maken van een taak met een hogere frequentie dan de maximale frequentie voor de taakverzameling vervolgens mislukt aanvraag met statuscode 409 Conflict. |
| **Taken** |De maximale taken standaardquotum is 5 taken in een gratis taakverzameling en 50 taken in een standard taakcollectie. Het maximale aantal taken kan worden geconfigureerd op een taakverzameling. Alle taken in de taakverzameling zijn beperkt de waarde voor de taakverzameling is ingesteld. Als u probeert te maken van meer taken dan het maximum aantal taken quotum, klikt u vervolgens mislukt de aanvraag met statuscode 409 Conflict. |
| **Taakcollecties** |Maximum aantal taakverzameling per abonnement is 200.000. |
| **Geschiedenis vasthouden** |Taakgeschiedenis wordt bewaard tot maximaal 2 maanden of tot de laatste 1000 uitvoeringen. |
| **Voltooide en mislukte taak bewaren** |Voltooide en mislukte jobs worden gedurende 60 dagen bewaard. |
| **Timeout** |Er is een statische (niet configureerbaar) time-out voor 60 seconden voor HTTP-acties. Volg voor meer actieve bewerkingen, asynchrone HTTP-protocollen. bijvoorbeeld, een 202 onmiddellijk resultaten retourneerde, maar blijven werken op de achtergrond. |

