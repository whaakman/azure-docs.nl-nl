U kunt meerdere services binnen een abonnement, elk criterium ingericht op een specifieke laag, alleen beperkt door het aantal services dat is toegestaan op elke laag maken. U kunt bijvoorbeeld maximaal 12 services op de basisstaffel en een andere 12 services op de laag S1 binnen hetzelfde abonnement maken. Zie voor meer informatie over categorieën [een SKU of laag kiezen voor Azure Search](../articles/search/search-sku-tier.md).

Maximale Servicelimieten kunnen op verzoek worden verhoogd. Neem contact op met ondersteuning van Azure als u meer services die zich binnen hetzelfde abonnement nodig.

| Resource | Gratis | Basic | S1 | S2 | S3 | S3 HD <sup>1</sup> |
| --- | --- | --- | --- | --- | --- | --- |
| Maximale services |1 |12 |12 |6 |6 |6 |
| Maximale schaal in SU <sup>2</sup> |N.V.T. <sup>3</sup> |3 SU <sup>4</sup> |36 SU |36 SU |36 SU |36 SU |

<sup>1</sup> S3 HD biedt geen ondersteuning voor [indexeerfuncties](../articles/search/search-indexer-overview.md) op dit moment. 

<sup>2</sup> search-eenheden (SU) zijn facturering eenheden, toegewezen als hetzij een *replica* of een *partitie*. U moet beide resources voor opslag, indexeren en querybewerkingen. Zie voor meer informatie over hoe de search-eenheden worden berekend, plus een grafiek van geldige combinaties die onder de maximale limieten blijven, [schalen niveaus van de bron voor query's en indexen werkbelastingen](../articles/search/search-capacity-planning.md). 

<sup>3</sup> vrij is op basis van gedeelde resources gebruikt door meerdere abonnees. Er zijn geen specifieke bronnen voor een afzonderlijke abonnee op deze laag. Maximale schaal is daarom gemarkeerd als niet van toepassing.

<sup>4</sup> basic heeft een vaste partitie. Bij deze laag worden aanvullende SUs gebruikt voor het toewijzen van meer replica's voor query hogere werkbelastingen.

