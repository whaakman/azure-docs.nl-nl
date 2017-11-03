Lokaal redundante opslag (LRS) repliceert uw gegevens driemaal binnen een schaaleenheid opslag die wordt gehost in een datacentrum in de regio waarin u uw opslagaccount hebt gemaakt. Een schrijfaanvraag retourneert is slechts eenmaal in het geschreven op alle drie replica's. Deze drie replica's elke zich bevinden in domeinen met afzonderlijke fouten en upgradedomeinen binnen één opslag schaaleenheid.

Een schaaleenheid storage is een verzameling van rekken opslagknooppunten. Een domein met fouten (FD) is een groep van knooppunten die kunnen worden beschouwd als knooppunten van hetzelfde fysieke rek en een fysieke eenheid van de fout vertegenwoordigen. Een upgradedomein (UD) is een groep van knooppunten die samen zijn bijgewerkt tijdens het proces van een service-upgrade (implementatie). De drie replica's zijn verdeeld over UDs en FDs binnen één opslag-schaaleenheid om ervoor te zorgen dat gegevens beschikbaar is, zelfs als hardwarefout van invloed is op één rack of wanneer knooppunten zijn bijgewerkt tijdens een implementatie.

LRS is de laagste kosten-optie en biedt minimaal duurzaamheid ten opzichte van andere opties. In het geval van een datacenter niveau noodgeval (fire, die het overspoelen enz.) kunnen alle drie replica's verloren gaan of onherstelbaar zijn. Om dit risico te beperken, wordt geografisch redundante opslag (GRS) aanbevolen voor de meeste toepassingen.

Lokaal redundante opslag nog steeds wenselijk in bepaalde scenario's:

* Biedt de hoogste maximale bandbreedte van Azure Storage-replicatieopties.
* Als uw toepassing gegevens die u kunt eenvoudig opnieuw worden samengesteld opgeslagen, kunt u kiezen voor LRS.
* Sommige toepassingen zijn beperkt tot het repliceren van gegevens alleen binnen een land vanwege beheervereisten gegevens. Een gekoppelde gebied kan worden in een ander land. Zie voor meer informatie over regio paren [Azure-regio's](https://azure.microsoft.com/regions/).