---
title: Automatisch schalen en zoneredundante toepassingsgateway v2
description: In dit artikel worden de Azure-toepassing Standard_v2 en WAF_v2 SKU, waaronder functies voor automatisch schalen en Zone-redundante geïntroduceerd.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 6/13/2019
ms.author: victorh
ms.openlocfilehash: 6aad0502b5739906d1fa8fa896f8d0af8cc38e30
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67205001"
---
# <a name="autoscaling-and-zone-redundant-application-gateway-v2"></a>Automatisch schalen en zoneredundante toepassingsgateway v2 

Application Gateway en Web Application Firewall (WAF) zijn ook beschikbaar onder een Standard_v2 en WAF_v2 SKU. De v2-SKU biedt verbeterde prestaties en voegt ondersteuning toe voor belangrijke nieuwe functies zoals automatisch schalen, zoneredundantie en ondersteuning voor statische VIP's. Bestaande functies onder de Standard- en WAF-Voorraadeenheid worden nog steeds ondersteund in de nieuwe v2-SKU, met een paar uitzonderingen die worden vermeld in [vergelijking](#differences-with-v1-sku) sectie.

De nieuwe v2-SKU bevat de volgende verbeteringen:

- **Automatische schaling**: Application Gateway of WAF-implementaties voor de automatische schaalaanpassing SKU kunnen omhoog of omlaag op basis van veranderende verkeerspatronen laden. Automatisch schalen heft ook de vereiste op om tijdens het inrichten een implementatiegrootte of het aantal instanties te kiezen. Deze SKU biedt de flexibiliteit van de waarde true. Application Gateway kan in de Standard_v2 en WAF_v2 SKU werken zowel in vaste capaciteit (automatisch schalen uitgeschakeld) en in de modus voor automatisch schalen ingeschakeld. Modus vaste capaciteit is handig voor scenario's met consistente en voorspelbare workloads. Modus voor automatisch schalen is het nuttig zijn in toepassingen die afwijking in het toepassingsverkeer te zien.
- **Zone redundantie**: Een Application Gateway of een WAF-implementatie kan omvatten meerdere Beschikbaarheidszones, verwijderen van de noodzaak voor het inrichten van afzonderlijke Application Gateway-instanties in elke zone met een Traffic Manager. U kunt een enkele zone of meerdere zones waarop Application Gateway-instanties zijn geïmplementeerd, waardoor het beter bestand is tegen bij een storing van de zone. De back-endpool voor toepassingen kan op dezelfde manier worden verdeeld in meerdere beschikbaarheidszones.

  Zoneredundantie is beschikbaar alleen waar Zones van Azure beschikbaar zijn. In andere regio's, worden alle andere functies ondersteund. Zie voor meer informatie, [wat zijn Beschikbaarheidszones in Azure?](../availability-zones/az-overview.md#services-support-by-region)
- **Statische VIP**: Application Gateway-v2 SKU ondersteunt uitsluitend de statische VIP-type. Dit zorgt ervoor dat het VIP-adres dat is gekoppeld aan de toepassingsgateway niet voor de levenscyclus van de implementatie, zelfs na het opnieuw opstarten wijzigen.  Is er een statische VIP-adres niet in v1, zodat u de URL van de application gateway in plaats van het IP-adres gebruiken moet voor het domein naam routering naar App Services via de application gateway.
- **Koptekst herschrijven**: Application Gateway kunt u toevoegen, verwijderen of bijwerken van HTTP-aanvraag- en reactieheaders met v2-SKU. Zie voor meer informatie, [Herschrijf de HTTP-headers met Application Gateway](rewrite-http-headers.md)
- **Key Vault-integratie (preview)** : Application Gateway v2 ondersteunt de integratie met Key Vault (in openbare preview) voor certificaten die zijn gekoppeld aan de HTTPS-functionaliteit listeners. Zie voor meer informatie, [SSL-beëindiging met Key Vault-certificaten](key-vault-certs.md).
- **Azure Kubernetes Service Controller voor binnenkomend verkeer (preview)** : De Controller voor binnenkomend verkeer van Application Gateway v2 kunt de Azure Application Gateway moet worden gebruikt als de ingang voor een Azure Kubernetes Service (AKS) wel AKS-Cluster. Zie voor meer informatie de [documentatiepagina](https://azure.github.io/application-gateway-kubernetes-ingress/).
- **Prestatieverbeteringen**: De v2-SKU biedt maximaal 5 X betere SSL-offload prestaties in vergelijking met de standaard/WAF-SKU.
- **Snellere implementatie en update** de v2-SKU biedt snellere implementatie en update in vergelijking met Standard/WAF-SKU. Dit omvat ook configuratiewijzigingen van WAF.

![](./media/application-gateway-autoscaling-zone-redundant/application-gateway-autoscaling-zone-redundant.png)

## <a name="supported-regions"></a>Ondersteunde regio’s

De Standard_v2 en WAF_v2 SKU is beschikbaar in de volgende regio's: Noord-centraal VS, Zuid-centraal VS, VS-West, VS-West 2, VS-Oost, VS-Oost 2, VS-midden, Noord-Europa, West-Europa, Zuidoost-Azië, Frankrijk-centraal, VK West, Japan-Oost, Japan-West. Extra regio's wordt in de toekomst worden toegevoegd.

## <a name="pricing"></a>Prijzen

Met de v2-SKU, het prijsmodel wordt aangedreven door verbruik en niet meer is gekoppeld aan het aantal instanties of grootten. De prijzen van v2 SKU bestaat uit twee onderdelen:

- **Vaste prijs** -dit is per uur (of een gedeelte van het uur) prijs voor het inrichten van een Standard_v2 of WAF_v2-Gateway.
- **Capaciteit eenheidsprijs** -dit is op basis van gebruik kosten in rekening naast de vaste kosten gebracht. Kosten voor capaciteitseenheden worden ook per uur of per deel van een uur berekend. Er zijn drie dimensies voor capaciteitseenheden: rekeneenheid, permanente verbinding en doorvoer. Rekeneenheid is een maateenheid voor de verbruikte processorcapaciteit. Factoren die invloed hebben op rekeneenheid zijn TLS-verbindingen per seconde, berekeningen herschrijven van URL's en verwerking van de WAF-regels. Permanente verbinding is een maateenheid tot stand gebrachte TCP-verbindingen met de application gateway in een bepaald interval van de facturering. Doorvoer is gemiddelde Megabits per seconde is verwerkt door het systeem in een bepaald interval van de facturering.

Elke capaciteitseenheid bestaat uit maximaal: 1 compute-eenheid of 2500 permanente verbindingen of 2,22 Mbps doorvoer.

COMPUTE-eenheid richtlijnen:

- **Standard_v2** -elke rekeneenheid ongeveer 50 verbindingen per seconde met RSA 2048-bits sleutel TLS-certificaat geschikt is.
- **WAF_v2** - elk compute-eenheid kan ongeveer 10 gelijktijdige aanvragen per seconde ondersteunen voor 70 tot 30% combinatie van het verkeer met 70% kleiner is aanvragen dan 2 KB ophalen/plaatsen en resterende hoger. Prestaties van de WAF wordt niet beïnvloed door grootte van antwoorden op dit moment.

> [!NOTE]
> Elke instantie kan op dit moment ongeveer 10 capaciteitseenheden ondersteunen.
> Het aantal aanvragen dat is een rekeneenheid kan worden verwerkt, is afhankelijk van verschillende criteria zoals TLS-certificaat-sleutelgrootte, algoritme voor sleuteluitwisseling, header regeneraties, en in het geval van WAF binnenkomende aanvraaggrootte. Het is raadzaam om dat u bij het uitvoeren van tests van de toepassing om te bepalen van de snelheid van aanvragen per rekeneenheid. Zowel capaciteitseenheid als rekeneenheid wordt beschikbaar gesteld als een metrische waarde voor de facturering wordt gestart.

De volgende tabel ziet u bijvoorbeeld prijzen en zijn alleen ter illustratie.

**Prijzen in VS Oost**:

|              SKU Name                             | Vaste prijs ($/ uur)  | Capaciteit prijs per eenheid ($/ CU-uur)   |
| ------------------------------------------------- | ------------------- | ------------------------------- |
| Standard_v2                                       |    0.20             | 0.0080                          |
| WAF_v2                                            |    0.36             | 0.0144                          |

Zie voor meer informatie prijzen, de [pagina met prijzen](https://azure.microsoft.com/pricing/details/application-gateway/). Facturering is gepland om te beginnen op 1 juli 2019.

**Voorbeeld 1**

Een Application Gateway Standard_v2 wordt ingericht zonder automatisch schalen in de modus voor handmatige vergroten/verkleinen met vaste capaciteit van vijf exemplaren.

Vaste prijs 744(hours) = * $0.20 $148.8 = <br>
Capaciteitseenheden 744 = (uren) * 10 capaciteitseenheid per exemplaar * 5 exemplaren * $0.008 per uur voor capaciteit eenheid = $297.6

Totale prijs = $148.8 + $297.6 $446.4 =

**Voorbeeld 2**

Een Application Gateway-standard_v2 gedurende een maand is ingericht en gedurende deze tijd ontvangt 25 nieuwe SSL-verbindingen/sec, gemiddelde van de gegevensoverdracht 8.88 Mbps. Ervan uitgaande dat verbindingen zijn korte heb gewoond, zou uw prijs zijn:

Vaste prijs 744(hours) = * $0.20 $148.8 =

Prijs per eenheid voor capaciteit 744(hours) = * Max (25/50 rekeneenheid voor verbindingen per seconde, 8.88/2.22 capaciteitseenheid voor doorvoer) * $0.008 = 744 * 4 * 0.008 $23.81 =

Totale prijs = $148. 23.81 8 + = $172.61

> [!NOTE]
> De functie Max retourneert de grootste waarde in een paar van de waarden.

**Voorbeeld 3**

Een Application Gateway WAF_v2 wordt ingericht voor een maand. Gedurende deze tijd het 25 nieuwe SSL-verbindingen/sec, gemiddelde 8.88 Mbps gegevensoverdracht ontvangt en 80 aanvragen per seconde heeft. Ervan uitgaande dat de verbindingen zijn korte duur en dat berekening van de compute-eenheid voor de toepassing biedt ondersteuning voor 10 RPS per compute-eenheid, de prijs zou zijn:

Vaste prijs 744(hours) = * $0.36 $267.84 =

Prijs per eenheid voor capaciteit 744(hours) = * Max (compute-eenheid Max(25/50 for connections/sec, 80/10 WAF RPS), 8.88/2.22 capaciteitseenheid voor doorvoer) * $0.0144 = 744 * 8 * 0.0144 $85.71 =

Totale prijs = $267.84 + $85.71 $353.55 =

> [!NOTE]
> De functie Max retourneert de grootste waarde in een paar van de waarden.

## <a name="scaling-application-gateway-and-waf-v2"></a>Application Gateway en WAF v2 schalen

Application Gateway en WAF kan worden geconfigureerd om te schalen in twee modi:

- **Automatisch schalen** - met automatisch schalen ingeschakeld, de toepassingsgateway en de WAF-v2 SKU's omhoog of omlaag op basis van verkeer toepassingsvereisten schalen. In deze modus biedt betere flexibiliteit voor uw toepassing en elimineert de noodzaak om te raden de application gateway-grootte of exemplaar aantal. In deze modus kunt u kosten kunt besparen door het niet vereist om uit te voeren van gateways piek ingerichte capaciteit voor de verwachte maximale te verdelen. Klanten moeten een minimum- en eventueel exemplaren opgeven. Minimale capaciteit zorgt ervoor dat Application Gateway en WAF v2 niet onder het minimum aantal exemplaren opgegeven vallen, zelfs in de afwezigheid van verkeer. U ontvangt een factuur voor deze Minimumcapaciteit zelfs in de afwezigheid van al het verkeer. U kunt eventueel ook een maximum aantal exemplaren, die zorgt ervoor dat de toepassingsgateway niet voorbij het opgegeven aantal exemplaren schalen opgeven. U gaat verder met de kosten in rekening gebracht voor de hoeveelheid verkeer die worden geleverd door de Gateway. Het aantal instanties kunnen variëren van 0 tot 125. De standaardwaarde voor het maximum aantal exemplaren is 20 indien niet opgegeven.
- **Handmatige** -u kunt ook handmatig modus waarbij de gateway wordt niet automatisch schalen. In deze modus als er meer verkeer dan wat Application Gateway of WAF verwerken kan, kan dit leiden tot verlies van verkeer. Met handmatige modus op te geven aantal exemplaren is verplicht. Aantal instanties kan variëren van 1 tot 125 exemplaren.

## <a name="feature-comparison-between-v1-sku-and-v2-sku"></a>Vergelijking van functies tussen SKU v1 en v2-SKU

De volgende tabel vergelijkt de functies die beschikbaar zijn met elke SKU.

|                                                   | V1-SKU   | v2-SKU   |
| ------------------------------------------------- | -------- | -------- |
| Automatisch schalen                                       |          | &#x2713; |
| Zoneredundantie                                   |          | &#x2713; |
| Statische VIP                                        |          | &#x2713; |
| Azure Kubernetes Service (AKS) ingangscontroller |          | &#x2713; |
| Integratie van Azure Sleutelkluis                       |          | &#x2713; |
| Herschrijf de headers van HTTP (S)                           |          | &#x2713; |
| URL-gebaseerde routering                                 | &#x2713; | &#x2713; |
| Hosting van meerdere sites                             | &#x2713; | &#x2713; |
| Omleiden van verkeer                               | &#x2713; | &#x2713; |
| Web Application Firewall (WAF)                    | &#x2713; | &#x2713; |
| SSL-beëindiging (Secure Sockets Layer)            | &#x2713; | &#x2713; |
| End-to-end SSL-versleuteling                         | &#x2713; | &#x2713; |
| Sessieaffiniteit                                  | &#x2713; | &#x2713; |
| Aangepaste foutpagina's                                | &#x2713; | &#x2713; |
| Ondersteuning voor WebSocket                                 | &#x2713; | &#x2713; |
| Ondersteuning voor HTTP/2                                    | &#x2713; | &#x2713; |
| Verwerkingsstop voor verbindingen                               | &#x2713; | &#x2713; |

> [!NOTE]
> De SKU biedt nu ondersteuning voor automatisch schalen-v2 [standaard statuscontroles](application-gateway-probe-overview.md#default-health-probe) automatisch de status van alle resources in de back-end-pool bewaken en markeer deze back-end-leden die worden beschouwd als niet in orde. De standaard-statustest wordt automatisch geconfigureerd voor back-ends die geen een aangepaste test-configuratie hebben. Zie voor meer informatie, [statuscontroles in application gateway](application-gateway-probe-overview.md).

## <a name="differences-with-v1-sku"></a>Verschillen met v1-SKU

|Verschil|Details|
|--|--|
|Certificaat voor clientverificatie|Wordt niet ondersteund.<br>Zie voor meer informatie, [overzicht van end-to-end SSL met Application Gateway](ssl-overview.md#end-to-end-ssl-with-the-v2-sku).|
|Met een combinatie van Standard_v2 en Standard Application Gateway in hetzelfde subnet|Niet ondersteund|
|Gebruiker gedefinieerde Route (UDR) op Application Gateway-subnet|Niet ondersteund|
|NSG voor binnenkomende poortbereik| -65200 tot en met 65535 voor Standard_v2 SKU<br>-65503 tot 65534 voor standaard-SKU.<br>Zie voor meer informatie de [Veelgestelde vragen over](application-gateway-faq.md#are-network-security-groups-supported-on-the-application-gateway-subnet).|
|Prestatielogboeken in Azure diagnostics|Wordt niet ondersteund.<br>Metrische gegevens van Azure moet worden gebruikt.|
|Billing|De facturering is gepland om te beginnen op 1 juli 2019.|
|FIPS-modus|Deze worden momenteel niet ondersteund.|
|Alleen ILB-modus|Dit wordt momenteel niet ondersteund. Openbare en samen ILB-modus wordt ondersteund.|
|Integratie van het netwerk controleren|Wordt niet ondersteund.|
|Integratie van Azure Security Center|Nog niet beschikbaar.

## <a name="migrate-from-v1-to-v2"></a>Migreren van v1 naar v2

Een Azure PowerShell-script is beschikbaar in de PowerShell gallery kunt u vanaf uw v1 Application Gateway/WAF migreren naar de v2-automatisch schalen SKU. Met dit script helpt u bij het kopiëren van de configuratie van uw gateway v1. Verkeer migratie is nog steeds uw eigen verantwoordelijkheid. Zie voor meer informatie, [migreren Azure Application Gateway van v1 in v2](migrate-v1-v2.md).

## <a name="next-steps"></a>Volgende stappen

- [Snelstart: Directe webverkeer met Azure Application Gateway - Azure portal](quick-create-portal.md)
- [Een automatisch schalen, zoneredundante toepassingsgateway maken met een gereserveerd virtueel IP-adres met behulp van Azure PowerShell](tutorial-autoscale-ps.md)
- Meer informatie over [Application Gateway](overview.md).
- Meer informatie over [Azure Firewall](../firewall/overview.md).
