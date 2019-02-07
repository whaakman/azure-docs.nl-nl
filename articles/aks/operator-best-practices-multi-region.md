---
title: Operator aanbevolen procedures - hoge beschikbaarheid en herstel na noodgevallen in Azure Kubernetes Services (AKS)
description: Meer over de best practices uit de cluster-operator voor maximale beschikbaarheid voor uw toepassingen om te zorgen voor hoge beschikbaarheid en herstel noodsituaties in Azure Kubernetes Services (AKS) voorbereiden
services: container-service
author: lastcoolnameleft
ms.service: container-service
ms.topic: conceptual
ms.date: 11/28/2018
ms.author: lastcoolnameleft
ms.openlocfilehash: 9958f5f0f1435af231c1426a249c745f4a2352c5
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55816608"
---
# <a name="best-practices-for-business-continuity-and-disaster-recovery-in-azure-kubernetes-service-aks"></a>Aanbevolen procedures voor zakelijke continuïteit en herstel na noodgevallen in Azure Kubernetes Service (AKS)

Wanneer u clusters in Azure Kubernetes Service (AKS) beheert, wordt het belangrijker actieve tijdsduur van toepassingen. AKS biedt hoge beschikbaarheid met behulp van meerdere knooppunten in een beschikbaarheidsset. Deze meerdere knooppunten voorkomen niet dat u een regio-fout. Als u wilt de actieve tijdsduur maximaliseren, bepaalde zakelijke continuïteit en herstelfuncties worden geïmplementeerd.

Deze aanbevolen procedures voor artikel richt zich op overwegingen waarmee u van plan bent voor zakelijke continuïteit en herstel na noodgevallen in AKS. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
* Plan voor AKS-clusters in meerdere regio 's
* Verkeer doorverbinden tussen meerdere clusters met Azure Traffic Manager
* Gebruik van geo-replicatie voor uw installatiekopie-containerregisters
* Plan voor de status van de toepassing in meerdere clusters
* Opslag in meerdere regio's repliceren

## <a name="plan-for-multi-region-deployment"></a>Plan voor implementatie in meerdere regio 's

**Aanbevolen procedurerichtlijn** : wanneer u meerdere AKS clusters implementeert kiest u regioparen waar AKS beschikbaar is en het gebruik van gekoppelde regio's.

Een AKS-cluster wordt geïmplementeerd in een enkele regio. Om uzelf te beschermen tegen storingen van de regio, implementeert u uw toepassing in meerdere AKS-clusters in verschillende regio's. Wanneer u van plan in welke regio bent naar uw AKS-cluster implementeren, de volgende overwegingen zijn van toepassing:

* [Beschikbaarheid in regio's AKS](https://docs.microsoft.com/azure/aks/container-service-quotas#region-availability)
  * Kiest u regioparen dicht bij uw gebruikers. AKS is voortdurend uitbreiden naar nieuwe regio's.
* [Azure gekoppelde regio 's](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)
  * Kies voor uw geografische gebied twee regio's die zijn gekoppeld aan elkaar worden verbonden. Deze regio's platform updates coördineren en herstel richten waar nodig.
* Niveau van beschikbaarheid van de service (' hot ' / ' hot ', ' hot ' / warme, warme/koude)
  * Wilt u beide regio's worden uitgevoerd op hetzelfde moment, met één regio *gereed* om te beginnen met het leveren van verkeer of een regio die u moet de tijd om aan de slag met het leveren van verkeer.

Beschikbaarheid in regio's AKS en gekoppelde regio's zijn gezamenlijke overweging. Implementeer uw AKS-clusters in gekoppelde regio's die zijn ontworpen voor het beheren van noodherstel regio tegelijk. Bijvoorbeeld, AKS is beschikbaar in *VS-Oost* en *VS-West*. Deze regio's zijn ook gekoppeld. Deze twee regio's zou worden aanbevolen bij het maken van een AKS-BC/DR-strategie.

Wanneer u uw toepassing implementeert, moet u ook een andere stap toevoegen aan uw CI/CD-pijplijn te implementeren op deze clusters met meerdere AKS. Als u uw implementatie pijplijnen niet bijwerkt, kunnen alleen implementaties van toepassingen worden geïmplementeerd in een van uw regio's en de AKS-clusters. Klantenverkeer dat wordt omgeleid naar een secundaire regio wordt niet de meest recente code-updates ontvangt.

## <a name="use-azure-traffic-manager-to-route-traffic"></a>Azure Traffic Manager gebruiken om verkeer te routeren

**Aanbevolen procedurerichtlijn** -klanten kunnen hun dichtstbijzijnde AKS-cluster en de toepassing exemplaar kunt instellen dat Azure Traffic Manager. Voor de beste prestaties en redundantie, instellen dat alle toepassingsverkeer via Traffic Manager voordat u doorgaat naar uw AKS-cluster.

Met meerdere AKS-clusters in verschillende regio's moet u bepalen hoe verkeer wordt omgeleid naar de toepassingen die worden uitgevoerd in elk cluster. [Met Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/) is een DNS-verkeer load balancer die verkeer tussen regio's kunt distribueren. U kunt gebruikers op basis van de reactietijd van de cluster, of op basis van Geografie versturen.

![AKS met Azure Traffic Manager](media/operator-best-practices-bc-dr/aks-azure-traffic-manager.png)

Met een AKS-cluster van één klanten maken doorgaans verbinding met de *Service IP* of DNS-naam van een bepaalde toepassing. Klanten moeten in een implementatie met meerdere cluster verbinding maken met een Traffic Manager-DNS-naam die naar de services op elke AKS-cluster verwijst. Deze services worden gedefinieerd met behulp van Traffic Manager-eindpunten. Elk eindpunt is de *Service Load Balancer IP*. Deze configuratie kunt u direct van netwerkverkeer van het Traffic Manager-eindpunt in één regio naar het eindpunt in een andere regio.

![Met Azure Traffic Manager de geografische routering](media/operator-best-practices-bc-dr/traffic-manager-geographic-routing.png)

Traffic Manager wordt gebruikt voor de DNS-zoekacties uitvoeren en retourneren van de meest geschikte eindpunt voor een gebruiker. Geneste profielen kunnen worden gebruikt met prioriteit gegeven voor een primaire locatie. Een gebruiker moet bijvoorbeeld voornamelijk verbinding maken met de dichtstbijzijnde geografische regio. Als deze regio een probleem is, Traffic Manager in plaats daarvan zorgt ervoor dat ze naar een secundaire regio. Deze aanpak zorgt ervoor klanten kunnen altijd verbinding gemaakt met het exemplaar van een toepassing, zelfs als de dichtstbijzijnde geografische regio niet beschikbaar is.

Voor instructies over het instellen van deze eindpunten en routering, Zie [configureren van de geografische verkeersrouteringsmethode met Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-configure-geographic-routing-method).

### <a name="layer-7-application-routing-with-azure-front-door"></a>Laag 7 voordeur met Azure-toepassingsroutering

Met Azure Traffic Manager gebruikt DNS (laag 3) voor verkeer van de vorm. [Azure voordeur (preview)](https://docs.microsoft.com/azure/frontdoor/front-door-overview) biedt een optie voor het routeren van HTTP/HTTPS (layer 7). Extra functies van de voordeur zijn SSL-beëindiging, aangepaste domein, Web Application Firewall, herschrijven van URL's en sessie-affiniteit.

Bekijk de behoeften van uw toepassingsverkeer om te begrijpen welke oplossing het meest geschikt is.

## <a name="enable-geo-replication-for-container-images"></a>Actieve geo-replicatie voor installatiekopieën van containers

**Aanbevolen procedurerichtlijn** -Store van uw containerinstallatiekopieën in Azure Container Registry en het register voor elke regio AKS geo-replicatie.

Als u wilt implementeren en uw toepassingen in AKS uitvoert, moet u een manier voor het opslaan en ophalen van de containerinstallatiekopieën. Azure Container Registry (ACR) kunt integreren met AKS voor het veilig opslaan van uw containerinstallatiekopieën of Helm-grafieken. ACR biedt ondersteuning voor meerdere masters geo-replicatie om uw afbeeldingen automatisch repliceren naar Azure-regio's over de hele wereld. Ter verbetering van prestaties en beschikbaarheid, gebruikt u ACR geo-replicatie naar een register maken in elke regio waarin u een AKS-cluster. Een AKS-cluster haalt vervolgens containerinstallatiekopieën uit het lokale ACR-register in dezelfde regio:

![Azure Container Registry geo-replicatie voor installatiekopieën van containers](media/operator-best-practices-bc-dr/acr-geo-replication.png)

De voordelen van het gebruik van geo-replicatie van de ACR omvatten het volgende:

* **Installatiekopieën binnenhaalt uit dezelfde regio is veel sneller.** U pull afbeeldingen van hoge snelheid en lage latentie netwerkverbindingen binnen dezelfde Azure-regio.
* **Installatiekopieën binnenhaalt uit dezelfde regio is betrouwbaarder.** Als een regio niet beschikbaar is, haalt uw AKS-cluster de installatiekopie van een andere ACR-register beschikbaar blijft.
* **Installatiekopieën binnenhaalt uit dezelfde regio is goedkoper.** Er zijn geen kosten van de uitgaande netwerk tussen datacenters.

Geo-replicatie is een functie van *Premium* SKU ACR registers. Zie voor instructies over het configureren van replicatie, [geo-replicatie van Azure Container Registry](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication)

## <a name="remove-service-state-from-inside-containers"></a>Verwijder service state van in containers

**Aanbevolen procedurerichtlijn** - waar mogelijk, de status van de service in de container niet opslaan. Gebruik in plaats daarvan Azure PaaS-services die ondersteuning bieden voor replicatie voor meerdere regio's.

De status van service verwijst naar de gegevens in het geheugen of op de schijf die een service nodig voor de functie heeft. Status bevat de gegevensstructuren en variabelen die de service leest en schrijft. Afhankelijk van hoe de service is ontworpen, zijn de status van de bestanden of andere bronnen die zijn opgeslagen op schijf. Bijvoorbeeld, de bestanden een database wilt gebruiken voor het opslaan van gegevens en transactielogboeken.

Status kan worden externalized of geplaatst met de code die de status is bewerken. Bedrijfsprocessen van status wordt meestal gedaan met behulp van een database of ander gegevensarchief die wordt uitgevoerd op verschillende computers via het netwerk of buiten het proces op dezelfde computer.

Containers en microservices zijn meest flexibele als de processen die worden uitgevoerd in deze status niet vasthouden. Als uw toepassingen bijna altijd een status bevatten, gebruikt u een Platform als een Service-oplossing, zoals Azure Database voor MySQL/Postgres of Azure SQL.

Zie voor meer informatie over voor informatie over het bouwen van toepassingen die meer overdraagbaar zijn, de volgende richtlijnen:

* [De basis van meerdere factoren twaalf App methodologie](https://12factor.net/).
* [Een webtoepassing uitvoeren in meerdere Azure-regio 's](https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/multi-region)

## <a name="create-a-storage-migration-plan"></a>Stel een migratieplan voor opslag

**Aanbevolen procedurerichtlijn** : als u gebruik van Azure Storage, voorbereiden en testen van uw opslag migreren van de primaire naar de back-regio.

Uw toepassingen kunnen Azure Storage gebruiken voor hun gegevens. Als uw toepassingen worden verdeeld over meerdere AKS-clusters in verschillende regio's, moet u de opslag die is gesynchroniseerd houden. Twee algemene manieren om opslag te repliceren zijn onder andere de volgende methoden:

* Asynchrone replicatie op basis van een toepassing
* Asynchrone replicatie op basis van infrastructuur

### <a name="infrastructure-based-asynchronous-replication"></a>Asynchrone replicatie op basis van infrastructuur

Uw toepassingen, permanente opslag vereist, zelfs nadat een schil is verwijderd. In Kubernetes, kunt u permanente Volumes gebruiken om vast te leggen van de opslag van gegevens. Deze permanente volumes zijn gekoppeld aan VM-knooppunt en klikt u vervolgens beschikbaar gemaakt voor het gehele product. Permanente volumes Volg schillen, zelfs als de schil wordt verplaatst naar een ander knooppunt binnen hetzelfde cluster.

Afhankelijk van het gebruik van de oplossing voor opslag kunnen replicatiestrategieën afwijken. Algemene oplossingen voor opslag, zoals [Gluster](https://docs.gluster.org/en/latest/Administrator%20Guide/Geo%20Replication/), [CEPH](http://docs.ceph.com/docs/master/cephfs/disaster-recovery/), [toren](https://rook.io/docs/rook/master/disaster-recovery.html), en [Portworx](https://docs.portworx.com/scheduler/kubernetes/going-production-with-k8s.html#disaster-recovery-with-cloudsnaps) hebben hun eigen richtlijnen.

De centrale methode is een gemeenschappelijk tijdstip van de opslag voor toepassingen om hun gegevens te schrijven. Deze gegevens worden vervolgens gerepliceerd in regio's en vervolgens lokaal toegankelijk zijn.

![Asynchrone replicatie op basis van infrastructuur](media/operator-best-practices-bc-dr/aks-infra-based-async-repl.png)

Als u Azure Managed Disks gebruikt, zijn beschikbaar replicatie en DR-oplossingen met behulp van een van de volgende methoden:

* [ARK op Azure](https://github.com/heptio/ark/blob/master/docs/azure-config.md)
* [Azure Site Recovery](https://azure.microsoft.com/blog/asr-managed-disks-between-azure-regions/)

### <a name="application-based-asynchronous-replication"></a>Asynchrone replicatie op basis van een toepassing

Er is momenteel geen Kubernetes-native-implementatie voor asynchrone replicatie op basis van een toepassing. Met de losjes gekoppelde aard van containers en Kubernetes, een traditionele aanpak van de toepassing of taal kunnen worden gebruikt. De centrale benadering is voor de toepassingen zelf voor het repliceren van de opslag-aanvragen die vervolgens worden geschreven naar van elk cluster onderliggende opslag van gegevens.

![Asynchrone replicatie op basis van een toepassing](media/operator-best-practices-bc-dr/aks-app-based-async-repl.png)

## <a name="next-steps"></a>Volgende stappen

In dit artikel is gericht op de zakelijke continuïteit en noodherstel overwegingen voor herstel in AKS-clusters. Zie voor meer informatie over de bewerkingen voor een cluster in AKS, de volgende aanbevolen procedures:

* [Multitenancy en cluster-isolatie][aks-best-practices-cluster-isolation]
* [Kubernetes scheduler basisfuncties][aks-best-practices-scheduler]

<!-- INTERNAL LINKS -->
[aks-best-practices-scheduler]: operator-best-practices-scheduler.md
[aks-best-practices-cluster-isolation]: operator-best-practices-cluster-isolation.md
