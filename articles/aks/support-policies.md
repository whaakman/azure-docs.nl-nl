---
title: Ondersteuningsbeleid voor Azure Kubernetes Service (AKS)
description: Meer informatie over Azure Kubernetes Service (AKS) ondersteuningsbeleid, gedeelde verantwoordelijkheid en functies die zich in de Preview-versie (of alpha of bèta).
services: container-service
author: jnoller
ms.service: container-service
ms.topic: article
ms.date: 04/01/2019
ms.author: jenoller
ms.openlocfilehash: 0d2c080be727d2ae13d6d9e5274f17cadffbe640
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65786462"
---
# <a name="support-policies-for-azure-kubernetes-service"></a>Ondersteuningsbeleid voor Azure Kubernetes Service

In dit artikel bevat informatie over beleid voor technische ondersteuning en beperkingen voor Azure Kubernetes Service (AKS). Het artikel wordt ook uitgelegd worker-knooppunt management, beheerde besturingselement vlak onderdelen, open-source-onderdelen van derden en beveiligings- of patch management.

## <a name="service-updates-and-releases"></a>Service-updates en versies

* Release-informatie, Zie [AKS opmerkingen bij de release](https://github.com/Azure/AKS/releases).
* Zie voor meer informatie over functies in preview [AKS preview-functies en verwante projecten](https://github.com/Azure/AKS/blob/master/previews.md).

## <a name="managed-features-in-aks"></a>Functies in AKS beheerd

Basis-infrastructuur als een service (IaaS)-cloud-onderdelen, zoals rekencapaciteit of netwerkonderdelen, geeft gebruikers toegang tot op laag niveau besturingselementen en opties voor het aanpassen. AKS biedt daarentegen een kant-en Kubernetes-implementatie waarmee klanten de algemene set configuraties en mogelijkheden die ze nodig hebben. AKS-klanten hebben beperkte aanpassing-, implementatie- en andere opties. Deze klanten hoeft te bekommeren of Kubernetes-clusters rechtstreeks beheren.

Met AKS, wordt de klant een volledig beheerde *controlelaag*. Het besturingselement vlak bevat alle van de onderdelen en services die de klant moet werken en Kubernetes-clusters bieden aan eindgebruikers. Alle Kubernetes-onderdelen worden onderhouden en beheerd door Microsoft.

Microsoft beheert en bewaakt de volgende onderdelen via het deelvenster Beheer:

* Kubelet- of Kubernetes API-servers
* Etcd of een compatibel sleutel / waarde-archief, Quality of Service (QoS), schaalbaarheid en runtime te leveren
* DNS-services (bijvoorbeeld kube-DNS- of CoreDNS)
* Kubernetes-proxy of netwerken

AKS is een volledig beheerde oplossing niet. Sommige onderdelen, zoals worker-knooppunten, *gedeelde verantwoordelijkheid*, waar gebruikers moeten ten behoeve van het AKS-cluster. Invoer van de gebruiker is vereist, bijvoorbeeld een patch worker-knooppunt besturingssysteem (OS) beveiliging toe te passen.

De services zijn *beheerd* in de zin dat Microsoft en het AKS-team wordt geïmplementeerd, werkt, en is verantwoordelijk voor de servicebeschikbaarheid en -functionaliteit. Klanten kunnen deze beheerde onderdelen niet wijzigen. Microsoft beperkt aanpassen om te controleren of een consistent en schaalbaar gebruikerservaring. Zie voor een volledig aanpasbare oplossing [AKS Engine](https://github.com/Azure/aks-engine).

> [!NOTE]
> AKS worker-knooppunten worden in de Azure-portal weergegeven als gewone Azure IaaS-resources. Maar deze virtuele machines worden geïmplementeerd in een aangepaste Azure-resourcegroep (voorafgegaan door MC\\*). Het is mogelijk om te wijzigen van AKS worker-knooppunten. Bijvoorbeeld, u kunt Secure Shell (SSH) wijzigen AKS worker-knooppunten van de manier waarop u normale virtuele machines wijzigen (u kunt echter niet, wijzigen de OS-basisinstallatiekopie, en wijzigingen kunnen niet behouden tijdens een update of opnieuw opstarten), en kunt u andere Azure-resources koppelen aan AKS worker-knooppunten. Maar wanneer u wijzigingen aanbrengt *buiten-band-beheer en aanpassingen,* het AKS-cluster kan worden hebben. Vermijd worker-knooppunten wijzigen, tenzij Microsoft Support zorgt ervoor dat u wijzigingen kunt aanbrengen.

## <a name="shared-responsibility"></a>Gedeelde verantwoordelijkheid

Als u een cluster maakt, definieert de klant de worker-knooppunten voor Kubernetes AKS maakt. Klant-workloads worden uitgevoerd op deze knooppunten. Klanten kunnen eigenaar en weergeven of wijzigen van de worker-knooppunten.

Omdat de klant clusterknooppunten persoonlijke code uitvoeren en opslaan van gevoelige gegevens, kunnen ze in beperkte mate Microsoft Support openen. Microsoft Support kan niet aanmelden bij, voert u opdrachten in of weergeven van Logboeken voor deze knooppunten zonder toestemming van de snelle klant of hulp.

Omdat de worker-knooppunten zijn gevoelig, neemt Microsoft uiterst nauwkeurig beheer van hun achtergrond beperken. In veel gevallen blijft uw werkbelasting uitvoeren, zelfs als de Kubernetes-master knooppunten, etcd en andere Microsoft-beheerde onderdelen is mislukt. Ondoordacht gewijzigde worker-knooppunten kunnen leiden tot verlies van gegevens en workloads en het cluster hebben kunnen weergeven.

## <a name="aks-support-coverage"></a>AKS ondersteuningsdekking

Microsoft biedt technische ondersteuning voor het volgende:

* Connectiviteit met alle Kubernetes-onderdelen die de Kubernetes-service biedt en ondersteunt, zoals de API-server.
* Beheer, actieve tijdsduur van QoS en bewerkingen van Kubernetes besturingselement vlak services (Kubernetes-hoofdknooppunten, API-server, etcd en kube-dns).
* Etcd. Ondersteuning biedt automatische, transparante back-ups van gegevens van alle etcd elke 30 minuten na noodgevallen plannen en cluster herstel van de gebruikersstatus. Deze back-ups zijn niet rechtstreeks beschikbaar aan klanten of gebruikers. Ze Zorg ervoor dat de betrouwbaarheid van gegevens en consistentie.
* Alle integratiepunten in het stuurprogramma van de Azure-cloud-provider voor Kubernetes. Het gaat hierbij om integratie in andere Azure-services zoals netwerktaakverdelers, permanente volumes of netwerken (Kubernetes en Azure CNI).
* Vragen of problemen met het aanpassen van het besturingselement vlak-onderdelen, zoals de Kubernetes API-server, etcd en kube-dns.
* Onderwerpen over netwerken, zoals Azure CNI, kubenet, of andere toegang tot het netwerk en de functionaliteit van problemen. Problemen kunnen omvatten DNS-resolutie, pakketverlies, routering, enzovoort. Microsoft biedt ondersteuning voor verschillende scenario's voor netwerken:
  * Kubenet (basis) en geavanceerde netwerken (Azure CNI) binnen het cluster en onderdelen die zijn gekoppeld
  * Connectiviteit met andere Azure-services en toepassingen
  * Ingress-controllers en inkomend verkeer of load balancer-configuraties
  * Prestaties van het netwerk en latentie

Microsoft biedt geen technische ondersteuning voor het volgende:

* Vragen over het gebruik van Kubernetes. Microsoft Support biedt geen advies over het maken van aangepaste inkomend domeincontrollers, gebruiken werkbelastingen van toepassingen of software van derden of open-source-pakketten of hulpprogramma's toe te passen.
  > [!NOTE]
  > Microsoft Support kunt advies geven over AKS clusterfunctionaliteit, aanpassen en afstemmen (voor bijvoorbeeld problemen met de bewerkingen van Kubernetes en procedures).
* Van derden open-source-projecten die niet zijn opgegeven als onderdeel van de Kubernetes besturingselement vlak of geïmplementeerd met AKS-clusters. Deze projecten advies inwinnen Istio, Helm, Envoy of anderen.
  > [!NOTE]
  > Microsoft kan best-effort ondersteuning bieden voor open source-projecten van derden, zoals Helm en Kured. Wanneer de open-source-hulpprogramma van derden kan worden geïntegreerd met de Kubernetes Azure-cloud-provider of andere fouten AKS-specifieke, biedt Microsoft ondersteuning voor voorbeelden en toepassingen van Microsoft-documentatie.
* Gesloten-source-software van derden. Deze software kunt opnemen security scanprogramma's en netwerken apparaten of software.
* Problemen met het multicloud of productondersteuning build-outs. Microsoft biedt geen bijvoorbeeld ondersteuning voor problemen met betrekking tot het uitvoeren van een federatieve multipublic leverancier cloudoplossing.
* Aanpassingen dan die zijn vermeld in het netwerk de [AKS-documentatie](https://docs.microsoft.com/azure/aks/).
  > [!NOTE]
  > Microsoft biedt ondersteuning voor problemen en fouten met betrekking tot netwerkbeveiligingsgroepen (nsg's). Microsoft Support kunnen bijvoorbeeld beantwoord vragen over een NSG niet bijwerken of een onverwacht NSG of load balancer-gedrag.

## <a name="aks-support-coverage-for-worker-nodes"></a>AKS ondersteuningsdekking voor worker-knooppunten

### <a name="microsoft-responsibilities-for-aks-worker-nodes"></a>De verantwoordelijkheden van Microsoft voor AKS worker-knooppunten

Microsoft en klanten vertellen over verantwoordelijk is voor Kubernetes worker-knooppunten waar:

* De OS-basisinstallatiekopie vereist toevoegingen (zoals controle- en netwerkinfrastructuur van agents).
* De worker-knooppunten ontvangen automatisch patches voor het besturingssysteem.
* Problemen met het Kubernetes besturingselement vlak onderdelen die worden uitgevoerd op de worker-knooppunten automatisch worden hersteld. Onderdelen zijn onder andere het volgende:
  * Kube-proxy
  * VPN-tunnels die communicatiepaden aan de Kubernetes-master onderdelen
  * Kubelet
  * Docker of Moby daemon

> [!NOTE]
> Op een worker-knooppunt, als een besturingselement vlak van het onderdeel niet operationeel is, het AKS-team moet mogelijk opnieuw opstarten van het gehele worker-knooppunt. Vanwege hun beperkte toegang tot de actieve werkbelasting van de klant en gegevens, het AKS-team opnieuw wordt opgestart een worker-knooppunt alleen als de klant het probleem escaleert. Waar mogelijk, werkt het AKS-team om te voorkomen dat een opnieuw opstarten vereist die betrekking hebben op de toepassing.

### <a name="customer-responsibilities-for-aks-worker-nodes"></a>De verantwoordelijkheden van de klant voor AKS worker-knooppunten

Microsoft, niet automatisch opnieuw opgestart worker-knooppunten om toe te passen op serverniveau OS-patches. Hoewel patches voor het besturingssysteem worden geleverd aan de worker-knooppunten, de *klant* is verantwoordelijk voor het opnieuw opstarten van de worker-knooppunten om de wijzigingen te laten. Gedeelde tapewisselaars daemons zoals solid-state hybride-station (SSHD) en andere onderdelen op het niveau van het systeem of het besturingssysteem wordt automatisch gevuld.

Klanten zijn verantwoordelijk voor het uitvoeren van upgrades voor Kubernetes. Ze kunnen upgrades via het Configuratiescherm van Azure of Azure CLI uitvoeren. Dit geldt voor updates die beveiligings-of functionaliteit verbeteringen in Kubernetes.

> [!NOTE]
> Omdat AKS is een *beheerde service*, de end-doelen verwijdert de verantwoordelijkheid voor updates, patches en verzamelen om de service management meer voltooid en zonder. Als de capaciteit van de service voor het beheer van end-to-end toeneemt, kunnen sommige functies (bijvoorbeeld knooppunt opnieuw wordt opgestart en automatische toepassing van patches) weglaten in toekomstige releases.

### <a name="security-issues-and-patching"></a>Beveiligingsproblemen met zich mee en patching uit handen

Als een beveiligingsfout in een of meer onderdelen van AKS wordt gevonden, wordt het AKS-team patch uitvoeren voor alle betrokken clusters om het probleem te beperken. U kunt ook krijgt het team gebruikers richtlijnen voor upgrades.

Voor worker-knooppunten dat een security fout is van invloed op, als een patch zonder enige uitvaltijd beschikbaar is, wordt het AKS-team dat patch toe te passen en meldingen verzenden naar gebruikers van de wijziging.

Wanneer een beveiligingspatch worker-knooppunt opnieuw wordt opgestart vereist, stuurt Microsoft de klanten van deze vereiste. De klant is verantwoordelijk voor het opnieuw opstarten of bij te werken om op te halen van de cluster-patch. Als gebruikers zijn niet van toepassing de patches op basis van AKS richtlijnen, blijft het cluster kwetsbaar voor beveiligingsproblemen.

### <a name="node-maintenance-and-access"></a>Onderhoud van knooppunt- en toegangsbeheer

Worker-knooppunten zijn van een gedeelde verantwoordelijkheid en zijn eigendom van klanten. Als gevolg hiervan hebben klanten de mogelijkheid om zich aanmelden bij hun worker-knooppunten en potentieel schadelijke wijzigingen zoals het kernel-updates en het installeren of verwijderen van pakketten.

Als klanten destructieve wijzigingen aanbrengen of ervoor zorgen dat besturingselement vlak onderdelen gaat offline of niet meer functioneert, AKS detecteert van deze fout en het worker-knooppunt naar de vorige werkende status wordt automatisch hersteld.

Hoewel klanten kunnen zich aanmelden bij en worker-knooppunten wijzigen, wordt dit afgeraden, omdat wijzigingen een cluster hebben aanbrengen kunnen.

## <a name="network-ports-access-and-nsgs"></a>Netwerk-poorten, toegang en nsg 's

Als een beheerde service heeft AKS specifieke vereisten voor netwerken en connectiviteit. Deze vereisten zijn minder flexibel dan de vereisten voor normale IaaS-onderdelen. In AKS, kunnen bewerkingen, zoals het aanpassen van NSG-regels, blokkeren van een specifieke poort (bijvoorbeeld met behulp van firewallregels waarmee uitgaande poort 443), en in de whitelist URL's uw cluster hebben.

> [!NOTE]
> Op dit moment kunt AKS u niet volledig vergrendelen uitgaande verkeer van uw cluster (bijvoorbeeld expliciete domein of in de whitelist aan poort). De lijst met URL's en poorten is kan worden gewijzigd zonder waarschuwing. U kunt de bijgewerkte lijst krijgen door het maken van een Azure-ondersteuningsticket. De lijst is alleen voor klanten die u bereid bent te accepteren dat de beschikbaarheid van het cluster kan worden getroffen *op elk gewenst moment.*

## <a name="unsupported-alpha-and-beta-kubernetes-features"></a>Niet-ondersteunde functies voor alpha- en Bètaservices Kubernetes

AKS ondersteunt alleen stabiel functies binnen de upstream Kubernetes-project. Tenzij anders wordt gedocumenteerd, biedt geen ondersteuning alpha- en Bètaservices functies die beschikbaar in de upstream Kubernetes-project zijn voor AKS.

In twee scenario's, kunnen alpha of bèta-functies worden geïmplementeerd voordat ze algemeen beschikbaar:

* Klanten hebben voldoet aan de AKS-product, ondersteuning of IT-teams en wordt gevraagd om deze nieuwe functies te proberen.
* Deze functies zijn [ingeschakeld door een functievlag](https://github.com/Azure/AKS/blob/master/previews.md). Klanten moeten expliciet meldt zich aan deze functies gebruiken.

## <a name="preview-features-or-feature-flags"></a>Preview-functies of functie vlaggen

Microsoft brengt voor de functies en functionaliteit waarvoor uitgebreide testen en feedback van gebruikers, nieuwe preview-functies of onderdelen achter een functievlag. Houd rekening met deze functies als voorlopige versie of bèta-functies.

Preview-functies of onderdelen van de functie-vlag zijn niet bedoeld voor productie. Doorlopende wijzigingen in API's en gedrag, oplossingen voor problemen en andere wijzigingen kunnen resulteren in instabiel clusters en uitvaltijd.

Functies in public preview zijn vallen onder 'best effort'-ondersteuning als deze functies beschikbaar als preview zijn en niet bedoeld voor productie en worden ondersteund door de technische ondersteuning AKS teams alleen tijdens kantooruren. Raadpleeg voor meer informatie:

* [Veelgestelde vragen over ondersteuning van Azure](https://azure.microsoft.com/support/faq/)

> [!NOTE]
> Preview-functies van kracht vanaf de Azure *abonnement* niveau. Installeer geen preview-functies op een productie-abonnement. Een productie-abonnement kunt preview-functies API standaardgedrag wijzigen en van invloed zijn op normale bewerkingen.

## <a name="upstream-bugs-and-issues"></a>Upstream bugs en problemen

Gezien de snelheid van de ontwikkeling in de upstream Kubernetes-project, ontstaan fouten altijd. Sommige van deze fouten kan niet worden hersteld of gewerkt om binnen het AKS-systeem. Oplossingen voor problemen moeten in plaats daarvan grotere patches upstream projecten (zoals Kubernetes, knooppunt- of werkrollen besturingssystemen en kernels). Voor de onderdelen die Microsoft eigenaar is van (zoals de Azure-cloud-provider), zijn AKS en Azure personeel doorgevoerd naar de upstream-problemen oplossen in de community.

Wanneer een probleem met de technische ondersteuning is de hoofdmap wordt veroorzaakt door een of meer upstream bugs, wordt de AKS-ondersteuning en -engineering-teams:

* Identificeer en koppeling van de upstream-fouten met een ondersteunende informatie op basis waarvan wordt uitgelegd waarom dit probleem is van invloed op uw cluster of de werkbelasting. Klanten ontvangen koppelingen naar de vereiste opslagplaatsen, zodat ze kunnen bekijken van de problemen en zien wanneer een nieuwe release oplossingen biedt.
* Mogelijke tijdelijke oplossingen of oplossingen bieden. Als het probleem kan grotendeels worden opgevangen, een [bekende probleem](https://github.com/Azure/AKS/issues?q=is%3Aissue+is%3Aopen+label%3Aknown-issue) wordt ingediend in de AKS-opslagplaats. De indiening bekend probleem wordt beschreven:
  * Het probleem, inclusief koppelingen naar upstream bugs.
  * De tijdelijke oplossing en de details over een upgrade of een andere persistentie van de oplossing.
  * Ruwe tijdlijnen voor de opname van het probleem, op basis van de upstream worden uitgerold.
