De opslag wordt beperkt door de schijfruimte of door de vaste limiet voor het *maximale aantal* indexen of documenten (wat zich als eerste voordoet).

| Resource | Gratis | Basic | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| Service Level Agreement (SLA) |Nee <sup>1</sup> |Ja |Ja |Ja |Ja |Ja |
| Opslag per partitie |50 MB |2 GB |25 GB |100 GB |200 GB |200 GB |
| Partities per service |N.v.t. |1 |12 |12 |12 |3 <sup>2</sup> |
| Partitiegrootte |N.v.t. |2 GB |25 GB |100 GB |200 GB |200 GB |
| Replica's |N.v.t. |3 |12 |12 |12 |12 |
| Maximale aantal indexen |3 |5 |50 |200 |200 |1000 per partitie of 3000 per service |
| Maximale aantal indexeerfuncties |3 |5 |50 |200 |200 |Geen ondersteuning voor indexeerfuncties |
| Maximale aantal gegevensbronnen |3 |5 |50 |200 |200 |Geen ondersteuning voor indexeerfuncties |
| Maximale aantal documenten |10.000 |1 miljoen |15 miljoen per partitie of 180 miljoen per service |60 miljoen per partitie of 720 miljoen per service |120 miljoen per partitie of 1,4 miljard per service |1 miljoen per index of 200 miljoen per partitie |

<sup>1</sup> gratis laag en de preview-functies niet bij service level agreements (Sla's) worden geleverd. Voor alle factureerbare lagen, sla's van kracht als u voldoende redundantie voor uw service inricht. Twee of meer replica's zijn vereist voor de SLA voor query (gelezen). Drie of meer replica's zijn vereist voor query's en indexering SLA (lezen / schrijven). Het aantal partities is niet een SLA-overweging. 

<sup>2</sup> S3 HD heeft een vaste limiet van 3 partities. Dit is lager dan de partitielimiet voor S3. De partitielimiet is lager doordat S3 HD aanzienlijk meer indexen heeft. Omdat er servicelimieten bestaan voor zowel rekenresources (opslag en verwerking) als inhoud (indexen en documenten), wordt de limiet voor de inhoud het eerst bereikt.
