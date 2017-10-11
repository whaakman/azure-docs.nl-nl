De volgende tabel worden beschreven van de belangrijkste quota, limieten, standaardwaarden en vertragingen in Azure Scheduler.

| Resource | Beschrijving van de limiet |
| --- | --- |
| **De grootte van de taak** |Maximale grootte is 16 kB. Als een plaatsen of een PATCH in een taak die groter is dan deze limiet resulteert, wordt een 400 Ongeldige aanvraag statuscode geretourneerd. |
| **De grootte van de aanvraag-URL** |Maximale grootte van de aanvraag-URL is 2048 tekens. |
| **Cumulatieve header-grootte** |Maximale cumulatieve header-grootte is 4096 tekens. |
| **Header-telling** |Maximale header aantal is 50 headers. |
| **Grootte van standaardberichthoofdtekst** |Maximale grootte is 8192 tekens. |
| **Terugkeerpatroon in beslag neemt** |Maximale terugkeerpatroon span is 18 maanden. |
| **Tijd tot tijd starten** |Maximaal 'keer in de begintijd' is 18 maanden. |
| **Jobgeschiedenis** |Maximale antwoordtekst die zijn opgeslagen in de taakgeschiedenis is 2048 bytes. |
| **Frequentie** |Het quotum van de maximale frequentie standaard is 1 uur in een gratis taakverzameling en 1 minuut in een standaard taakverzameling. De maximale frequentie kan worden geconfigureerd op een taakverzameling moet lager zijn dan het maximum. Alle taken in de taakverzameling zijn beperkt de waarde voor de taakverzameling is ingesteld. Als u probeert te maken van een taak met een hogere frequentie dan de maximale frequentie voor de taakverzameling vervolgens mislukt aanvraag met statuscode 409 Conflict. |
| **Taken** |Het quotum van de maximale taken standaard is 5 taken in een gratis taakverzameling en 50 taken in een standaard taakverzameling. Het maximum aantal taken kan worden geconfigureerd op een taakverzameling. Alle taken in de taakverzameling zijn beperkt de waarde voor de taakverzameling is ingesteld. Als u probeert meer taken dan het maximum aantal taken quotum maken, klikt u vervolgens mislukt de aanvraag met statuscode 409 Conflict. |
| **Taakverzamelingen** |Maximum aantal taakverzameling per abonnement is 200.000. |
| **Geschiedenis vasthouden** |Taakgeschiedenis worden bewaard gedurende 2 maanden of tot de laatste 1000 uitvoeringen. |
| **Voltooide en mislukte taak bewaren** |Voltooide en mislukte jobs worden bewaard gedurende 60 dagen. |
| **Time-out** |Er is een statische (niet-configureerbare) aanvraag time-out van 60 seconden voor HTTP-acties. Volg voor actieve bewerkingen langer, asynchrone HTTP-protocollen. bijvoorbeeld, een 202 onmiddellijk geretourneerd, maar blijven werken op de achtergrond. |

