Geografisch redundante opslag (GRS) worden uw gegevens gerepliceerd naar een secundaire regio die honderden mijl weg van de primaire regio. Als uw storage-account GRS ingeschakeld heeft, zijn uw gegevens is duurzame zelfs het geval van een volledige regionale uitval of een ramp optreedt waarbij de primaire regio kan niet worden hersteld.

Voor een opslagaccount met GRS ingeschakeld, is een update eerst toegewijd aan de primaire regio, waar deze drie keer is gerepliceerd. De update wordt vervolgens asynchroon gerepliceerd naar de secundaire regio, waar deze ook driemaal gerepliceerd.

Met GRS de gebieden primaire en secundaire replica's in afzonderlijke foutdomeinen beheren en domeinen binnen een schaaleenheid opslag zoals beschreven met LRS upgraden.

Overwegingen:

* Aangezien asynchrone replicatie moet worden van een vertraging, er in een noodsituatie regionale is het mogelijk dat wijzigingen die nog niet is gerepliceerd naar de secundaire regio verbroken worden als de gegevens van de primaire regio kan niet worden hersteld.
* De replica is niet beschikbaar, tenzij Microsoft failover naar de secundaire regio initieert. Als Microsoft een failover naar de secundaire regio starten, u hebt leesmachtigingen en schrijftoegang tot die gegevens na de failover is voltooid. Zie voor meer informatie [Disaster Recovery richtlijnen](../articles/storage/common/storage-disaster-recovery-guidance.md). 
* Als een toepassing wil lezen van de secundaire regio, moet de gebruiker RA-GRS inschakelen.

Als u een opslagaccount maakt, selecteert u de primaire regio voor het account. De secundaire regio wordt bepaald op basis van de primaire regio en kan niet worden gewijzigd. De volgende tabel toont de koppelingen van de primaire en secundaire regio.

| Primair | Secundair |
| --- | --- |
| Noord-centraal VS | Zuid-centraal VS |
| Zuid-centraal VS | Noord-centraal VS |
| VS - oost | VS - west |
| VS - west | VS - oost |
| Verenigde Staten (oost 2) | VS - midden |
| VS - midden | Verenigde Staten (oost 2) |
| Noord-Europa | West-Europa |
| West-Europa | Noord-Europa |
| Zuidoost-Azië | Oost-Azië |
| Oost-Azië | Zuidoost-Azië |
| China - oost | China - noord |
| China - noord | China - oost |
| Japan - oost | Japan - west |
| Japan - west | Japan - oost |
| Brazilië - zuid | Zuid-centraal VS |
| Australië - oost | Australië - zuidoost |
| Australië - zuidoost | Australië - oost |
| India - zuid | India - midden |
| India - midden | India - zuid |
| India - west | India - zuid |
| VS (overheid) - Iowa | VS (overheid) - Virginia |
| VS (overheid) - Virginia | VS (overheid) - Texas |
| VS (overheid) - Texas | VS (overheid) - Arizona |
| VS (overheid) - Arizona | VS (overheid) - Texas |
| Canada - midden | Canada - oost |
| Canada - oost | Canada - midden |
| Verenigd Koninkrijk West | Verenigd Koninkrijk Zuid |
| Verenigd Koninkrijk Zuid | Verenigd Koninkrijk West |
| Duitsland - centraal | Duitsland - noordoost |
| Duitsland - noordoost | Duitsland - centraal |
| VS - west 2 | West-centraal VS |
| West-centraal VS | VS - west 2 |

Zie voor actuele informatie over regio's die worden ondersteund door Azure [Azure-regio's](https://azure.microsoft.com/regions/).

>[!NOTE]  
> VS Gov Virginia secundaire regio is Gov ons Texas. Voorheen gebruikt Gov ons Virginia Gov ons Iowa als een secundaire regio. Storage-accounts nog Gov ons Iowa als een secundaire regio worden gemigreerd naar Gov ons Texas als een secundaire regio. 
> 
> 