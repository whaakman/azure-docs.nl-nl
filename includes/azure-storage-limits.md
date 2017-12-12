| Resource | Standaardlimiet |
| --- | --- |
| Aantal opslagaccounts per abonnement | 200<sup>1</sup> |
| Maximum aantal opslagaccountcapaciteit | 500 TiB<sup>2</sup> |
| Maximumaantal blob-containers, blobs, bestandsshares, tabellen, wachtrijen, entiteiten of berichten per storage-account | Geen limiet |
| Percentage maximum aantal aanvragen per storage-account | aanvragen per seconde voor 20.000<sup>2</sup> |
| Maximale inkomende<sup>3</sup> per storage-account (ons regio's) | 10 Gbps als GRS/ZRS<sup>4</sup> ingeschakeld, 20 Gbps voor LRS<sup>2</sup> |
| Maximum aantal uitgaande<sup>3</sup> per storage-account (ons regio's) | 20 Gbps als RA-GRS/GRS/ZRS<sup>4</sup> ingeschakeld, 30 Gbps voor LRS<sup>2</sup> |
| Maximale inkomende<sup>3</sup> per storage-account (buiten de Verenigde Staten regio's) | 5 Gbps als GRS/ZRS<sup>4</sup> ingeschakeld, 10 Gbps voor LRS<sup>2</sup> |
| Maximum aantal uitgaande<sup>3</sup> per storage-account (buiten de Verenigde Staten regio's) | 10 Gbps als RA-GRS/GRS/ZRS<sup>4</sup> ingeschakeld, 15 Gbps voor LRS<sup>2</sup> |

<sup>1</sup>bevat standaard- en Premium storage-accounts. Als u meer dan 200 opslagaccounts nodig hebt, dient u een aanvraag in te dienen via de [ondersteuning van Azure](https://azure.microsoft.com/support/faq/). Het Azure Storage-team bekijkt dan uw bedrijfsscenario en kan tot 250 opslagaccounts goedkeuren. 

<sup>2</sup> maken om uw accounts standaardopslag groeien voorbij de grenzen aangekondigd in de frequentie van capaciteit, inkomend en uitgaand en aanvraag, een aanvraag via [ondersteuning van Azure](https://azure.microsoft.com/support/faq/). Het Azure Storage-team worden de aanvragen controleren en hogere limieten op basis van geval tot geval kan goedkeuren.

<sup>3</sup> beperkt alleen door het account inkomend en uitgaand limieten. *Inkomend* verwijst naar alle gegevens (aanvragen) verstuurd naar een opslagaccount. *Uitgaand* verwijst naar alle gegevens (reacties) die worden ontvangen van een opslagaccount.  

<sup>4</sup>azure Storage redundantieopties zijn onder andere:
* **RA-GRS**: geografisch redundante opslag met leestoegang. Als RA-GRS is ingeschakeld, zijn uitgaande doelen voor de secundaire locatie identiek aan die voor de primaire locatie.
* **GRS**: geografisch redundante opslag. 
* **ZRS**: Zone-redundante opslag. Alleen beschikbaar voor blok-blobs. 
* **LRS**: lokaal redundante opslag. 