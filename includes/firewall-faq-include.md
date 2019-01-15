---
title: bestand opnemen
description: bestand opnemen
services: firewall
author: vhorne
ms.service: ''
ms.topic: include
ms.date: 12/14/2018
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: 3d76f25fc4382c8f03fac682fa7286a4a329a1db
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54300624"
---
### <a name="what-is-azure-firewall"></a>Wat is Azure Firewall?

Azure Firewall is een beheerde, cloudgebaseerde netwerkbeveiligingsservice die uw Azure Virtual Network-resources beschermt. Het is een volledig stateful firewall-as-a-service met de ingebouwde hoge beschikbaarheid en cloudschaalbaarheid van de onbeperkte. U kunt beleid voor toepassings- en netwerkconnectiviteit centraal maken, afdwingen en registreren voor abonnementen en virtuele netwerken.

### <a name="what-capabilities-are-supported-in-azure-firewall"></a>Welke mogelijkheden worden ondersteund in de Firewall van Azure?

* Stateful firewall als een service
* Ingebouwde hoge beschikbaarheid met onbeperkte cloudschaalbaarheid
* Filteren op FQDN
* FQDN-tags
* Regels voor het filteren van netwerkverkeer
* Ondersteuning voor uitgaande SNAT
* Ondersteuning voor inkomende DNAT
* Centraal maken, afdwingen en meld u beleid van toepassing en netwerk connectiviteit tussen Azure-abonnementen en VNETs
* Volledig geïntegreerd met Azure Monitor voor registratie en analyses

### <a name="what-is-the-typical-deployment-model-for-azure-firewall"></a>Wat is het typische implementatiemodel voor de Firewall van Azure?

U kunt de Firewall van Azure implementeren op een virtueel netwerk, maar klanten doorgaans implementeren op een centrale virtueel netwerk en andere virtuele netwerken toe in een hub en spoke-model. Vervolgens stelt u de standaardroute uit de gekoppelde virtuele netwerken om te verwijzen naar dit centrale firewall virtuele netwerk.

### <a name="how-can-i-install-the-azure-firewall"></a>Hoe kan ik de Azure-Firewall installeren?

U kunt Azure Firewall instellen met behulp van de Azure portal, PowerShell, REST-API of met behulp van sjablonen. Zie [zelfstudie: Implementeren en configureren van de Firewall van Azure met behulp van de Azure-portal](../articles/firewall/tutorial-firewall-deploy-portal.md) voor stapsgewijze instructies.

### <a name="what-are-some-azure-firewall-concepts"></a>Wat zijn enkele concepten Firewall van Azure?

Firewall van Azure biedt ondersteuning voor regels en regelverzamelingen. Verzameling van een regel is een reeks regels die de dezelfde volgorde en prioriteit delen. Regelverzamelingen worden uitgevoerd in de volgorde van hun prioriteit. Netwerk regelverzamelingen hogere prioriteit dan de regelverzamelingen van toepassing zijn, en alle regels worden beëindigd.

Er zijn twee soorten regelverzamelingen:

* *Regels voor Application*: Hiermee kunt u de volledig gekwalificeerde domeinnamen (FQDN's) die kunnen worden benaderd vanaf een subnet configureren.
* *Regels voor*: Hiermee kunt u regels configureren die bronadressen, protocollen, doelpoorten en doeladressen bevatten.

### <a name="does-azure-firewall-support-inbound-traffic-filtering"></a>Biedt Azure Firewall ondersteuning voor binnenkomend verkeer filteren?

Firewall van Azure biedt ondersteuning voor binnenkomend en uitgaand filteren. Binnenkomende beveiliging is voor niet-HTTP/S-protocollen. Bijvoorbeeld RDP, SSH en FTP-protocollen.

### <a name="which-logging-and-analytics-services-are-supported-by-the-azure-firewall"></a>Welke services logboekregistratie en analyse worden ondersteund door de Firewall van Azure?

Firewall van Azure is geïntegreerd met Azure Monitor voor het weergeven en analyseren van de firewall-Logboeken. Logboeken kunnen worden verzonden naar Log Analytics, Azure Storage of Event Hubs. Ze kunnen worden geanalyseerd in Log Analytics of met verschillende hulpprogramma's zoals Excel en Power BI. Zie [Zelfstudie: Azure-Firewall-logboeken bewaken](../articles/firewall/tutorial-diagnostics.md).

### <a name="how-does-azure-firewall-work-differently-from-existing-services-such-as-nvas-in-the-marketplace"></a>Hoe Azure Firewall werkt anders van bestaande services zoals NVA's in de marketplace?

Firewall van Azure is een eenvoudige firewall-service die in bepaalde scenario's van klanten voorzien kan. Verwacht wordt dat u een combinatie van de NVA's van derden en de Firewall van Azure hebt. Samen beter is een prioriteit core.

### <a name="what-is-the-difference-between-application-gateway-waf-and-azure-firewall"></a>Wat is het verschil tussen de Application Gateway WAF- en firewallinstellingen van Azure?

De Web Application Firewall (WAF) is een functie van Application Gateway die gecentraliseerde binnenkomende beveiliging van uw webtoepassingen tegen algemene aanvallen en beveiligingsproblemen biedt. Firewall van Azure biedt binnenkomende beveiliging voor niet-HTTP/S-protocollen (bijvoorbeeld RDP, SSH, FTP), uitgaande op netwerkniveau beveiliging voor alle poorten en protocollen en de beveiliging op toepassingsniveau voor uitgaande HTTP/S.

### <a name="what-is-the-difference-between-network-security-groups-nsgs-and-azure-firewall"></a>Wat is het verschil tussen Netwerkbeveiligingsgroepen (nsg's) en de Firewall van Azure?

De Firewall van de Azure-service is een aanvulling op network security group functionaliteit. Samen bieden ze beter 'verdediging in de diepte' netwerkbeveiliging. Netwerkbeveiligingsgroepen bevatten gedistribueerde laag filteren van netwerkverkeer om te beperken van verkeer naar resources in virtuele netwerken in elk abonnement. Firewall van Azure is een volledig stateful, gecentraliseerde netwerk firewall as-a-service, waarmee de netwerk - en -beveiliging op toepassingsniveau in verschillende abonnementen en virtuele netwerken.

### <a name="how-do-i-set-up-azure-firewall-with-my-service-endpoints"></a>Hoe stel ik Azure-Firewall in met de service-eindpunten?

Voor beveiligde toegang tot PaaS-services raden wij service-eindpunten. U kunt de service-eindpunten in de Firewall van Azure-subnet inschakelen en uitschakelen verbonden spoke-netwerken. Op deze manier die u profiteren van functies, service-eindpunt beveiligings- en centrale logboekregistratie voor al het verkeer.

### <a name="what-is-the-pricing-for-azure-firewall"></a>Wat zijn de prijzen voor de Firewall van Azure?

Firewall van Azure heeft een vaste prijs + variabele kosten:

* Vaste kosten: $1.25/firewall/hour
* Variabel tarief: $ 0,03/GB verwerkt door de firewall (inkomend of uitgaand verkeer)

Er zijn geen kosten voor de firewall van een toewijzing ongedaan gemaakt.

Zie voor meer informatie, [prijzen van Azure-Firewall](https://azure.microsoft.com/pricing/details/azure-firewall/).

### <a name="how-can-i-stop-and-start-azure-firewall"></a>Hoe kan ik stoppen en starten van de Firewall van Azure?

U kunt Azure PowerShell gebruiken *toewijzing* en *toewijzen* methoden.

Bijvoorbeeld:

```azurepowershell
# Stop an exisitng firewall

$azfw = Get-AzureRmFirewall -Name "FW Name" -ResourceGroupName "RG Name"
$azfw.Deallocate()
Set-AzureRmFirewall -AzureFirewall $azfw
```

```azurepowershell
#Start a firewall

$azfw = Get-AzureRmFirewall -Name "FW Name" -ResourceGroupName "RG Name"
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName "RG Name" -Name "VNet Name"
$publicip = Get-AzureRmPublicIpAddress -Name "Public IP Name" -ResourceGroupName " RG Name"
$azfw.Allocate($vnet,$publicip)
Set-AzureRmFirewall -AzureFirewall $azfw
```

> [!NOTE]
> U moet een firewall en een openbaar IP-adres toewijzen aan de oorspronkelijke resourcegroep en het abonnement.

### <a name="what-are-the-known-service-limits"></a>Wat zijn de bekende servicebeperkingen?

Zie voor de Firewall van Azure-Servicelimieten, [Azure-abonnement en Servicelimieten, quotums en beperkingen](../articles/azure-subscription-service-limits.md#azure-firewall-limits)

### <a name="can-azure-firewall-in-a-hub-virtual-network-forward-and-filter-network-traffic-between-two-spoke-virtual-networks"></a>Firewall van Azure kunnen in een hub-netwerk doorsturen en filteren van netwerkverkeer tussen twee virtuele spoke-netwerken?

Ja, kunt u Azure-Firewall in een hub-netwerk te routeren en filteren verkeer tussen twee spoke virtueel netwerk. Subnetten in elk van de virtuele spoke-netwerken beschikken over UDR die verwijst naar de Firewall van Azure als een standaard-gateway voor dit scenario goed te laten werken.

### <a name="can-azure-firewall-forward-and-filter-network-traffic-between-subnets-in-the-same-virtual-network-or-peered-virtual-networks"></a>Kan Azure Firewall doorsturen en filteren van netwerkverkeer tussen subnetten in het hetzelfde virtuele netwerk of de gekoppelde virtuele netwerken?

Verkeer tussen subnetten in hetzelfde virtuele netwerk of in een rechtstreeks gekoppeld virtueel netwerk wordt rechtstreeks doorgestuurd, zelfs als de UDR verwijst naar de Firewall van Azure als de standaard-gateway. De aanbevolen methode voor de segmentering van het interne netwerk is het gebruik van Netwerkbeveiligingsgroepen. Als u in dit scenario subnet-naar-subnet-verkeer wilt verzenden, moet UDR het voorvoegsel van het doelsubnetwerk expliciet op beide subnetten bevatten.

### <a name="are-there-any-firewall-resource-group-restrictions"></a>Zijn er firewall beperkingen van de resource?

Ja. De firewall, subnet, VNet en het openbare IP-adres moeten zich in dezelfde resourcegroep bevinden.

### <a name="when-configuring-dnat-for-inbound-network-traffic-do-i-also-need-to-configure-a-corresponding-network-rule-to-allow-that-traffic"></a>Bij het configureren van DNAT voor binnenkomend netwerkverkeer, heb ik ook nodig om te configureren van een regel voor de bijbehorende als u wilt toestaan dat dit verkeer?

Nee. NAT-regels toevoegen impliciet een overeenkomende regel om de vertaalde verkeer te staan. U kunt dit gedrag overschrijven door expliciet een verzameling netwerkregels toe te voegen met regels voor weigeren die overeenkomen met het omgezette verkeer. Zie [Verwerkingslogica voor Azure Firewall-regels](../articles/firewall/rule-processing.md) voor meer informatie over de verwerkingslogica voor Azure Firewall-regels.
