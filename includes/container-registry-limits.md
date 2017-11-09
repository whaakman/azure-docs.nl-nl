| Bron | Basic | Standard | Premium |
|---|---|---|---|---|
| Storage | 10 giB | 100 giB| 500 giB |
| ReadOps per minuut<sup>1, 2</sup> | 1k | 300 kB | 10.000 k |
| WriteOps per minuut<sup>1, 3</sup> | 100 | 500 | 2 kB |
| Download MBps bandbreedte<sup>1</sup> | 30 | 60 | 100 |
| Uploaden van bandbreedte MBps<sup>1</sup> | 10 | 20 | 50 |
| Webhooks. | 2 | 10 | 100 |
| Geo-replicatie | N/A | N/A | [Ondersteund *(preview)*](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication) |

<sup>1</sup> *ReadOps*, *WriteOps*, en *bandbreedte* zijn minimale schattingen. ACR wil de prestaties verbeteren als gebruik vereist.

<sup>2</sup> [docker pull](https://docs.docker.com/registry/spec/api/#pulling-an-image) kan aan meerdere leesbewerkingen op basis van het aantal lagen in de afbeelding, plus het manifest ophalen.

<sup>3</sup> [docker push](https://docs.docker.com/registry/spec/api/#pushing-an-image) kan aan meerdere schrijfbewerkingen, op basis van het aantal lagen die moet worden geactiveerd. Een `docker push` bevat *ReadOps* voor het ophalen van een manifest voor een bestaande installatiekopie.