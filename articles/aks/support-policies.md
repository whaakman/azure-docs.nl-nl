---
title: Ondersteuning voor Azure Kubernetes Service (AKS)-beleid
description: Meer informatie over het ondersteuningsbeleid van Azure Kubernetes Service (AKS), gedeelde verantwoordelijkheid, Alpha-voorbeeld/bèta-functies.
services: container-service
author: jnoller
ms.service: container-service
ms.topic: article
ms.date: 04/01/2019
ms.author: jenoller
ms.openlocfilehash: f173fc7c794729eae8c60cceefa88d153800a816
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/11/2019
ms.locfileid: "59502171"
---
# <a name="azure-kubernetes-service-aks-support-policies"></a>Ondersteuning voor Azure Kubernetes Service (AKS)-beleid

Dit artikel bevat informatie over AKS technische ondersteuning van beleid, beperkingen en meer informatie, waaronder het beheer voor worker-knooppunt, beheerd besturingselement vlak onderdelen, open-source-onderdelen van derden en beveiliging / patch-beheer.

## <a name="service-updates--releases"></a>Service-updates en versies

* Zie voor release-informatie, de [AKS releaseopmerkingen][1]
* Zie voor functies in public preview [AKS-Preview-functies en verwante projecten][2]

## <a name="what-is-managed"></a>Wat wordt 'beheerd'

In tegenstelling tot IaaS cloud basisonderdelen zoals compute of netwerken, die beschikbaar maakt op laag niveau besturingselementen en opties voor het aanpassen voor gebruikers en biedt daardoor, de AKS-service biedt een gebruiksklare Kubernetes-implementatie die staat voor de algemene set configuraties en mogelijkheden die nodig zijn voor Kubernetes. Klanten die gebruikmaken van deze service hebben een beperkt aantal aanpassingen-, implementatie- en andere opties. Dit betekent ook klanten hoeven geen zorgen te maken of rechtstreeks beheren van de Kubernetes-cluster (s).

Met AKS, wordt de klant een volledig beheerde **controlelaag** : waar het besturingselement vlak bevat alle onderdelen en services die nodig zijn om te werken en Kubernetes-clusters bieden aan eindgebruikers. Alle Kubernetes-onderdelen worden onderhouden en beheerd door Microsoft.

Met de beheerde **controlelaag** de volgende onderdelen worden beheerd en bewaakt door Microsoft:

* Kubelet / Kubernetes API (s)
* Etcd of een compatibel sleutel/waarde-archief – zoals quality of service, schaalbaarheid en runtime
* DNS-services (bijvoorbeeld kube-DNS-/ CoreDNS)
* Kubernetes-Proxy/netwerken

AKS is niet een 100% beheerd **cluster** oplossing. Bepaalde onderdelen (zoals worker-knooppunten) zijn bepaalde **gedeelde verantwoordelijkheden** gevallen waarin bewerkingen voor het onderhouden van het AKS-cluster gebruikersinteractie is vereist. Bijvoorbeeld: worker-knooppunt OS security patch-toepassing.

 **Beheerde**, betekent dat Microsoft en het AKS-team heeft geïmplementeerd, werkt en is verantwoordelijk voor de beschikbaarheid en de functionaliteit van deze services. **Deze onderdelen kunnen niet worden gewijzigd door klanten**. Aanpassing is beperkt tot een consistent en schaalbaar gebruikerservaring te waarborgen. Zie voor een volledig aanpasbare oplossing [AKS-Engine](https://github.com/Azure/aks-engine).

> [!NOTE]
> Het is belangrijk te weten dat Azure Kubernetes Service worker-knooppunten in de Azure Portal als gewone Azure IaaS-Resources weergegeven, hoewel deze virtuele Machines worden geïmplementeerd in een aangepaste Azure-resourcegroep (voorafgegaan door MC\\*). Een gebruiker kan de bewerking alter, SSH naar ze net als bij normale virtuele machines (u kunt echter niet, wijzigen de OS-basisinstallatiekopie, en wijzigingen kunnen niet behouden tijdens een update of opnieuw opstarten), en u kunt aan andere Azure-resource zijn gekoppeld, of ze anders te wijzigen. **Hierdoor van buiten-band-beheer en aanpassingen betekent echter dat het AKS-cluster zelf hebben worden kan. Vermijd worker-knooppunt wijziging, tenzij geleid door Microsoft Support.**

## <a name="what-is-shared-responsibility"></a>Wat is er gedeelde verantwoordelijkheid

Tijdens de aanmaak van de cluster maakt AKS u een aantal worker-knooppunten van een Kubernetes gedefinieerd door de klant. Deze knooppunten, zoals aangegeven zijn waarin klanten workloads worden uitgevoerd. Klanten kunnen eigenaar en weergeven of wijzigen van deze worker-knooppunten.

Omdat deze knooppunten zijn persoonlijke code wordt uitgevoerd en gevoelige gegevens opslaan, Microsoft-ondersteuning heeft **beperkte toegang** aan de klant op alle clusterknooppunten. Microsoft-ondersteuning niet Meld u aan bij, opdrachten uit te voeren of weergeven van Logboeken voor deze knooppunten zonder uitdrukkelijke klant machtiging en/of hulp om foutopsporing opdrachten uitvoeren, zoals beschreven door het ondersteuningsteam.

Vanwege de gevoelige aard van deze worker-knooppunten neemt Microsoft uiterst voorzichtig te beperken van het beheer 'achter de schermen' van deze knooppunten. Zelfs als de Kubernetes-knooppunten, etcd en andere onderdelen is mislukt (beheerd door Microsoft) master blijft uw workload in veel gevallen worden uitgevoerd. Als worker-knooppunten worden gewijzigd zonder zorg, kan deze leiden tot verlies van gegevens en/of de werkbelasting en genereren van het cluster hebben.

## <a name="azure-kubernetes-service-support-coverage"></a>Ondersteuning van Azure Kubernetes Service

**Microsoft biedt technische ondersteuning voor het volgende:**

* Verbinding met alle onderdelen van Kubernetes geleverd en ondersteund door de Kubernetes-service (zoals de API-server)
* Beheer, actieve tijdsduur, QoS en bewerkingen van Kubernetes besturingselement vlak services (Kubernetes Master knooppunten, API-Server, etcd, kube-DNS-voorbeeld.
* Etcd ondersteuning biedt automatische, transparante back-ups van gegevens van alle etcd elke 30 minuten na noodgevallen plannen en cluster herstel van de gebruikersstatus. Deze back-ups zijn niet beschikbaar voor klanten/gebruikers rechtstreeks en worden gebruikt om te controleren of de betrouwbaarheid van gegevens en consistentie
* Alle integratiepunten in het Azure Cloud Provider-stuurprogramma voor Kubernetes, zoals de integratie met andere Azure-services zoals netwerktaakverdelers, permanente Volumes, Networking (Kubernetes en Azure CNI)
* Vragen of problemen rondom de aanpassing van besturingselement vlak-onderdelen, zoals de Kubernetes API-server, etcd en kube-dns.
* Problemen met over onderwerpen over netwerken, zoals Azure CNI, Kubenet of andere problemen met toegang en -functionaliteit (DNS-resolutie, pakketverlies, routering, enzovoort).
  * Netwerkscenario's ondersteund bevatten Kubenet (basis) en geavanceerde netwerken (Azure CNI) binnen het cluster en de bijbehorende onderdelen, de connectiviteit met andere Azure-services en toepassingen. Bovendien worden inkomend domeincontrollers en inkomend/load balancer-configuratie, prestaties van het netwerk en latentie ondersteund door Microsoft.

**Microsoft biedt geen technische ondersteuning voor het volgende:**

* Advies / 'Procedures' Gebruik Kubernetes vragen, bijvoorbeeld over het maken van aangepaste inkomend controllers, vragen over de werkbelasting van toepassing en andere-partij/OSS-pakketten of hulpprogramma's zijn niet inbegrepen.
  * Advies tickets voor AKS-cluster functionaliteit, aanpassing, afstemmingsacties – bijvoorbeeld Kubernetes problemen/How-to-tos zijn binnen het bereik.
* Van derden open-source-projecten niet is opgegeven als onderdeel van het Kubernetes vlak bepalen of geïmplementeerd met AKS-clusters, zoals Istio, Helm, Envoy en anderen.
  * Voor van derden open-source-projecten, zoals Helm en Kured, wordt aanbevolen inspanning ondersteuning geboden voor voorbeelden en toepassingen in Microsoft-documentatie en waar dat van derden open-source-hulpprogramma kan worden geïntegreerd met de Kubernetes Azure-cloud-provider of andere AKS-specifieke fouten.
* Gesloten-source-software van derden: dit kunnen bijvoorbeeld beveiligingsscans hulpprogramma's, netwerken, apparaten of software.
* Problemen met 'meerdere cloud' of de leverancier van meerdere build-outs worden niet ondersteund, bijvoorbeeld een leveranciersoplossing federatieve meerdere openbare cloud uitvoeren wordt niet ondersteund.
* Specifieke netwerk-aanpassingen, dan die zijn beschreven in de officiële [AKS-documentatie][3].
  > [!NOTE]
  > Problemen en fouten over Netwerkbeveiligingsgroepen wordt ondersteund. Ondersteuning kan bijvoorbeeld beantwoord vragen over nsg's niet kan worden bijgewerkt of onverwachte NSG of Load Balancer-gedrag.

## <a name="azure-kubernetes-service-support-coverage-worker-nodes"></a>Dekking voor Azure Kubernetes Service-ondersteuning (Worker-knooppunten)

### <a name="microsoft-responsibilities-for-azure-kubernetes-service-worker-nodes"></a>De verantwoordelijkheden van Microsoft voor Azure Kubernetes Service worker-knooppunten

Zoals aangegeven de `shared responsibility` sectie, Kubernetes worker-knooppunten kunnen worden onderverdeeld in een gemeenschappelijke verantwoordelijkheid-model, waarbij:

* Biedt de basisbesturingssysteemkopie met vereiste toevoegingen (zoals controle- en netwerkinfrastructuur van agents)
* Automatische patches voor besturingssystemen te leveren aan de worker-knooppunten
* Automatisch herstel van problemen met Kubernetes besturingselement vlak onderdelen die worden uitgevoerd op de worker-knooppunten, zoals:
  * kube-proxy
  * VPN-tunnels die communicatiepaden aan de Kubernetes-master onderdelen
  * kubelet
  * docker/moby daemon

> [!NOTE]
> Als een besturingselement vlak-component niet operationeel zijn op een worker-knooppunt is, moet het AKS-team mogelijk opnieuw opstarten van het gehele worker-knooppunt op te lossen. Dit is gedaan namens een gebruiker en niet wordt uitgevoerd, tenzij een uitbreiding van de klant wordt gemaakt (vervaldatum voor de toegang tot de actieve werkbelasting van klanten en de gegevens). Wanneer mogelijk AKS personeel werkt om alle vereiste opnieuw opstarten niet-toepassing die invloed hebben op.

### <a name="customer-responsibilities-for-azure-kubernetes-service-worker-nodes"></a>De verantwoordelijkheden van de klant voor Azure Kubernetes Service worker-knooppunten

**Microsoft is niet:**

- Automatisch opnieuw opstarten worker-knooppunten voor OS patches te laten treden niveau **(op dit moment Zie hieronder).**

Patches voor besturingssystemen worden geleverd aan de worker-knooppunten, maar het is de **verantwoordelijkheid van de klant** opnieuw op te starten van de worker-knooppunten voor de wijzigingen worden doorgevoerd. Deze automatische patches bevat gedeelde tapewisselaars daemons zoals SSHD en andere onderdelen van system/OS-niveau.

Voor upgrades van Kubernetes, **, zijn klanten verantwoordelijk voor het uitvoeren van de upgrade** via het Configuratiescherm van Azure of Azure CLI. Dit geldt voor updates die beveiligings- of functionaliteit verbeteringen in Kubernetes.

> [!NOTE]
> AKS is een `managed service` de doelstellingen van het einde van de service zijn verantwoordelijk voor patches, updates, logboekverzameling en meer zodat deze een meer volledig beheerd en zonder service verwijderen. Deze items (knooppunt opnieuw wordt opgestart, automatisch bijgewerkt) kunnen worden verwijderd in toekomstige releases vergroten voor onze mogelijkheden om u te end-to-end management.

### <a name="security-issues-and-patching"></a>Beveiligingsproblemen met zich mee en patching uit handen

In sommige gevallen (zoals CVEs), een beveiligingsfout vindt u in een of meer van de onderdelen van de AKS-service. In dergelijke scenario's wordt AKS patch uitvoeren voor alle betrokken clusters te indien mogelijk het probleem beperken of upgraderichtlijnen bieden aan gebruikers.

Voor worker-knooppunten beïnvloed door deze een beveiligingsrisico als een patch zonder enige uitvaltijd van het probleem beschikbaar is, de AKS-team zal deze correctie toepassen en meldingen verzenden naar gebruikers van de wijziging.

Als een beveiligingspatch is bedoeld voor worker-knooppunt opnieuw wordt opgestart, Microsoft ontvangt de klant van deze vereiste en de klant is verantwoordelijk voor het uitvoeren van het opnieuw opstarten of bijwerken voor het ophalen van de patch voor de cluster. Als gebruikers de patches op basis van de AKS-richtlijnen niet van toepassing, wordt het cluster blijft kwetsbaar voor de oorzaak van deze.

### <a name="node-maintenance-and-access"></a>Onderhoud van knooppunt- en toegangsbeheer

Omdat de worker-knooppunten zijn een gedeelde verantwoordelijkheid en onder het eigendom van klanten, klanten kunnen zich aanmelden bij deze werknemers en uitvoeren van mogelijk schadelijke wijzigingen, zoals kernel-updates verwijderen van pakketten en het installeren van nieuwe pakketten.

Als klanten destructieve acties of acties besturingselement vlak onderdelen om te gaan offline uitvoeren of anders worden niet-functionele, de AKS-service wordt deze fout detecteren en automatisch herstel voor het herstellen van het worker-knooppunt naar de vorige werkende uitvoeren status.

Klanten kunnen zich aanmelden bij en alter worker-knooppunten, maar het is *afgeraden* omdat hierdoor het cluster hebben.

## <a name="network-ports-access-and-network-security-groups"></a>Netwerk-poorten, toegang en Netwerkbeveiligingsgroepen

Als een beheerde service heeft AKS specifieke vereisten voor netwerken en connectiviteit. Deze vereisten zijn minder flexibel dan de normale IaaS-onderdelen. In tegenstelling tot andere IaaS-onderdelen, kunnen bepaalde bewerkingen (zoals het aanpassen van netwerkbeveiligingsgroepsregels, specifieke poorten blokkeren, URL in een whitelist opnemen, enzovoort) weergeven van uw cluster hebben (bijvoorbeeld firewallregels blokkeert uitgaande poort 443).

> [!NOTE]
> Volledig vergrendelen van uitgaande gegevens (bijvoorbeeld expliciete domein/poort in de whitelist) van het cluster is niet een ondersteund scenario is AKS op dit moment. De lijst met URL's en poorten kan worden gewijzigd zonder waarschuwing en kan worden opgegeven door het ondersteuningsteam van Azure via een ticket. De opgegeven lijst is alleen voor klanten die u bereid bent te accepteren die *de beschikbaarheid van uw cluster op elk gewenst moment kan worden beïnvloed.*

## <a name="alphabeta-kubernetes-features-not-supported"></a>Alpha/bèta Kubernetes-functies (niet ondersteund)

AKS ondersteunt alleen stabiel-functies binnen de upstream Kubernetes-project. Alpha/bèta-functies die beschikbaar zijn in upstream Kubernetes worden niet ondersteund, tenzij anders wordt gecommuniceerd en beschreven.

Er zijn twee mogelijke situaties waar Alpha of bèta-functies kunnen worden geïmplementeerd voordat de algemene beschikbaarheid:

* Klanten hebben voldoet aan de AKS-product, ondersteuning of IT-teams en expliciet wordt gevraagd om deze nieuwe functies te proberen.
* Deze functies zijn [ingeschakeld via een Functievlag] [ 2] (expliciete opt-in)

## <a name="preview-features--feature-flags"></a>Preview-functies / functie vlaggen

Voor functies en functionaliteit waarvoor uitgebreide tests, community en feedback van gebruikers, brengt Microsoft nieuwe preview-functies of onderdelen achter een functievlag. Deze functies moeten worden overwogen voorlopige versie / Bèta, en zijn blootgesteld aan gebruikers geven een kans om uit deze nieuwe functies te proberen.

Echter de volgende preview- / functievlag functies zijn niet bedoeld voor gebruik in productie-API's, gedragswijzigingen, oplossingen voor problemen en andere wijzigingen kunnen worden aangebracht die kunnen resulteren in instabiel clusters en uitvaltijd. Ondersteuning voor deze functies is beperkt tot fout/probleem melden. Schakel geen deze functies op productiesystemen of abonnementen.

> [!NOTE]
> Bij de Azure preview-functies inschakelen kracht **abonnement** niveau. Installeer preview-functies niet op de productie-abonnement, zoals het gedrag van standaard-API die invloed hebben op de normale werking kunt wijzigen.

## <a name="upstream-bugs-and-issues"></a>Upstream bugs en problemen

Gezien de snelheid van de ontwikkeling in de upstream Kubernetes-project, zijn er altijd fouten optreden die niet kunnen worden gevuld of gewerkt oplossing binnen het AKS-systeem en in plaats daarvan moeten groter patches upstream projecten (zoals Kubernetes, knooppunt/Worker besturingssystemen en Kernels). AKS/Azure personeel zijn onderdelen die we bezit hebt (zoals de Azure-Cloud-Provider), doorgegeven aan het verhelpen van het probleem upstream in de community.

Voor gevallen waarin een probleem met de technische ondersteuning hoofdmap veroorzaakt voor een of meer upstream bugs, ondersteuning voor AKS en engineering doet de volgende items:

* Identificeer en koppeling van de upstream-fouten met een ondersteunende informatie over waarom dit heeft gevolgen voor uw cluster en/of de werkbelasting. Klanten wordt geleverd met koppelingen naar de vereiste opslagplaatsen/problemen bekijken van de problemen en zien wanneer een nieuwe Kubernetes/andere release bevat de fix(es)
* Mogelijke tijdelijke oplossingen of oplossingen: In sommige gevallen is het mogelijk om te werken om het probleem: in dit geval een "[bekend probleem](https://github.com/Azure/AKS/issues?q=is%3Aissue+is%3Aopen+label%3Aknown-issue)" wordt ingediend voor de AKS-opslagplaats waarin wordt uitgelegd:
  * Het probleem en een koppeling naar een upstream bugs
  * De tijdelijke oplossing, en meer informatie over upgraden/andere persistentie van de oplossing
  * Ruwe tijdlijnen voor opname op basis van de upstream vrijgeeffrequentie

[1]: https://github.com/Azure/AKS/releases
[2]: https://github.com/Azure/AKS/blob/master/previews.md
[3]: https://docs.microsoft.com/azure/aks/
