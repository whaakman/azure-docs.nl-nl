| Resource | Standaardlimiet |
| --- | :--- |
| Maximum aantal knooppunten per cluster | 100 |
| Maximaal aantal pods per knooppunt ([basisnetwerken met Kubenet][basic-networking]) | 110 |
| Maximaal aantal pods per knooppunt ([geavanceerde netwerken met Azure CNI][advanced-networking]) | 30<sup>1</sup> |
| Maximaal aantal clusters per abonnement | 20<sup>2</sup> |

<sup>1</sup> Deze waarde kan worden aangepast door een ARM-sjabloon te implementeren. Voorbeelden hiervan vindt u [hier][arm-deployment-example].<br />
<sup>2</sup> Verstuur een [Azure-ondersteuningsaanvraag][azure-support] om een hogere limiet aan te vragen.<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/networking-overview.md#basic-networking
[advanced-networking]: ../articles/aks/networking-overview.md#advanced-networking

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
[arm-deployment-example]: https://github.com/Azure/AKS/blob/master/examples/vnet/02-aks-custom-vnet.json#L64-L69
