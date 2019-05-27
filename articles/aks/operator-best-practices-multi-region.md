---
title: Hoge beschikbaarheid en herstel na noodgevallen in Azure Kubernetes Service (AKS)
description: Informatie over aanbevolen procedures voor het bereiken van maximale beschikbaarheid voor uw toepassingen, hoge beschikbaarheid en herstel na noodgevallen in Azure Kubernetes Service (AKS) voorbereiden van een cluster-operator.
services: container-service
author: lastcoolnameleft
ms.service: container-service
ms.topic: conceptual
ms.date: 11/28/2018
ms.author: lastcoolnameleft
ms.openlocfilehash: 93af2e4c373701383a674c694f7799ba890414dd
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2019
ms.locfileid: "65887438"
---
# <a name="best-practices-for-business-continuity-and-disaster-recovery-in-azure-kubernetes-service-aks"></a>Aanbevolen procedures voor zakelijke continuïteit en herstel na noodgevallen in Azure Kubernetes Service (AKS)

Wanneer u clusters in Azure Kubernetes Service (AKS) beheert, wordt het belangrijker actieve tijdsduur van toepassingen. AKS biedt hoge beschikbaarheid met behulp van meerdere knooppunten in een beschikbaarheidsset. Maar deze meerdere knooppunten niet uw systeem te beschermen tegen het uitvallen van een regio. Als u wilt de actieve tijdsduur maximaliseren, plan vooruit om zakelijke continuïteit en voorbereiden voor herstel na noodgevallen.

In dit artikel richt zich op het plan voor bedrijfscontinuïteit en herstel na noodgevallen in AKS. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Plan voor AKS-clusters in meerdere regio's.
> * Verkeer routeren voor meerdere clusters met behulp van Azure Traffic Manager.
> * Gebruik geo-replicatie voor uw container-installatiekopieregisters.
> * Plan voor de status van de toepassing in meerdere clusters.
> * Opslag in meerdere regio's repliceren.

## <a name="plan-for-multiregion-deployment"></a>Multiregion implementatie plannen

**Beste**: Wanneer u meerdere AKS clusters implementeert, kiest u regioparen waar AKS beschikbaar is en gebruikt u twee gekoppelde regio's.

Een AKS-cluster wordt geïmplementeerd in een enkele regio. Als u wilt uw systeem te beschermen tegen storingen van de regio, uw toepassing in meerdere AKS-clusters te implementeren in verschillende regio's. Wanneer u van plan bent om uw AKS-cluster te implementeren, houd rekening met:

* [**Beschikbaarheid in regio's AKS**](https://docs.microsoft.com/azure/aks/quotas-skus-regions#region-availability): Kiest u regioparen dicht bij uw gebruikers. AKS breidt voortdurend naar nieuwe regio's.
* [**Gekoppelde Azure-regio's**](https://docs.microsoft.com/azure/best-practices-availability-paired-regions): Kies voor uw geografische gebied twee regio's die zijn gekoppeld aan elkaar worden verbonden. Gekoppelde regio's platform updates coördineren en herstel richten waar nodig.
* **Beschikbaarheid van de service**: Bepalen of uw gekoppelde regio's ' hot ' / ' hot ', ' hot ' / warme of warme/koude moeten zijn. Wilt u beide regio's worden uitgevoerd op hetzelfde moment, met één regio *gereed* verkeer starten? Of wilt u de ene regio naar de tijd hebt om aan de slag met het leveren van verkeer?

Beschikbaarheid in regio's AKS en gekoppelde regio's zijn een gezamenlijke overweging. Implementeer uw AKS-clusters in gekoppelde regio's die zijn ontworpen voor het beheren van noodherstel regio tegelijk. AKS is bijvoorbeeld beschikbaar in VS-Oost en VS-West. Deze regio's zijn gekoppeld. Wanneer u een AKS-BC/DR-strategie maakt, kiest u deze twee regio's.

Wanneer u uw toepassing implementeert, moet u een andere stap toevoegen aan uw CI/CD-pijplijn te implementeren op deze clusters met meerdere AKS. Als u uw implementatie pijplijnen niet bijwerkt, kunnen toepassingen worden geïmplementeerd in slechts één van de regio's en de AKS-clusters. Klantenverkeer dat wordt omgeleid naar een secundaire regio wordt niet de meest recente code-updates ontvangt.

## <a name="use-azure-traffic-manager-to-route-traffic"></a>Azure Traffic Manager gebruiken om verkeer te routeren

**Beste**: Met Azure Traffic Manager kan direct-klanten naar de dichtstbijzijnde AKS-cluster en de toepassing instantie. Voor de beste prestaties en redundantie, instellen dat alle toepassingsverkeer via Traffic Manager voordat u deze gaat u naar uw AKS-cluster.

Als u meerdere clusters van AKS in verschillende regio's hebt, gebruikt u Traffic Manager om te bepalen hoe verkeer stroomt naar de toepassingen die worden uitgevoerd in elk cluster. [Met Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/) is een DNS-verkeer load balancer die verkeer tussen regio's kunt distribueren. Met Traffic Manager kunt route-gebruikers op basis van de reactietijd van de cluster of op basis van Geografie.

![AKS met Traffic Manager](media/operator-best-practices-bc-dr/aks-azure-traffic-manager.png)

Klanten met een AKS-cluster meestal verbinden met de service-IP-adres of de DNS-naam van een bepaalde toepassing. Klanten moeten in een implementatie van multicluster verbinding maken met een Traffic Manager-DNS-naam die naar de services op elke AKS-cluster verwijst. Deze services definiëren met behulp van Traffic Manager-eindpunten. Elk eindpunt is de *load balancer IP-service*. Deze configuratie gebruiken om netwerkverkeer van het Traffic Manager-eindpunt in één regio naar het eindpunt in een andere regio te regelen.

![Geografische routering via Traffic Manager](media/operator-best-practices-bc-dr/traffic-manager-geographic-routing.png)

Traffic Manager voert van DNS-zoekacties uit en retourneert de meest geschikte eindpunt van een gebruiker. Geneste profielen kunnen prioriteiten aanbrengen van een primaire locatie. Gebruikers moeten bijvoorbeeld in het algemeen verbinding maken met de dichtstbijzijnde geografische regio. Als deze regio een probleem is, stuurt Traffic Manager in plaats daarvan de gebruikers naar een secundaire regio. Deze aanpak zorgt ervoor dat klanten verbinding met een exemplaar van een toepassing maken kunnen, zelfs als de dichtstbijzijnde geografische regio niet beschikbaar is.

Zie voor meer informatie over het instellen van eindpunten en routering [de geografische verkeersrouteringsmethode configureren met behulp van Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-configure-geographic-routing-method).

### <a name="layer-7-application-routing-with-azure-front-door-service"></a>Routering van laag 7 toepassing met Azure voordeur Service

Traffic Manager gebruikt DNS (laag 3) voor verkeer van de vorm. [Azure voordeur Service](https://docs.microsoft.com/azure/frontdoor/front-door-overview) biedt een optie voor het routeren van HTTP/HTTPS (layer 7). Extra functies van Azure voordeur Service zijn SSL-beëindiging, aangepaste domein, de web application firewall, herschrijven van URL's en sessie-affiniteit. Bekijk de behoeften van uw toepassingsverkeer om te begrijpen welke oplossing het meest geschikt is.

## <a name="enable-geo-replication-for-container-images"></a>Actieve geo-replicatie voor installatiekopieën van containers

**Beste**: Store van uw containerinstallatiekopieën in Azure Container Registry en het register voor elke regio AKS geo-replicatie.

Als u wilt implementeren en uw toepassingen in AKS uitvoert, moet u een manier voor het opslaan en ophalen van de containerinstallatiekopieën. Container Registry is geïntegreerd met AKS, zodat deze worden veilig uw containerinstallatiekopieën of Helm-grafieken opgeslagen kan. Container Registry biedt ondersteuning voor multimaster geo-replicatie om uw afbeeldingen automatisch repliceren naar Azure-regio's over de hele wereld. 

Gebruik ter verbetering van prestaties en beschikbaarheid, geo-replicatie voor Container Registry een register maken in elke regio waarin u een AKS-cluster. Een AKS-cluster haalt vervolgens containerinstallatiekopieën uit het lokale containerregister in dezelfde regio:

![Geo-replicatie voor container Registry voor installatiekopieën van containers](media/operator-best-practices-bc-dr/acr-geo-replication.png)

Wanneer u met geo-replicatie voor Container Registry pull installatiekopieën uit dezelfde regio bevinden, worden de resultaten zijn:

* **Sneller**: U afbeeldingen van hoge en lage latentie netwerkverbindingen binnen dezelfde Azure-regio op te halen.
* **Betrouwbaarder**: Als een regio niet beschikbaar is, haalt uw AKS-cluster de installatiekopieën van een beschikbare container registry.
* **Goedkoper**: Er zijn geen kosten van de uitgaande netwerk tussen datacenters.

Geo-replicatie is een functie van *Premium* containerregisters SKU. Zie voor meer informatie over het configureren van geo-replicatie [geo-replicatie voor Container Registry](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication).

## <a name="remove-service-state-from-inside-containers"></a>Verwijder service state van in containers

**Beste**: Waar mogelijk, niet de status van de service in de container opgeslagen. In plaats daarvan gebruik een Azure-platform as a service (PaaS) die ondersteuning biedt voor regioreplicatie.

*Status service* verwijst naar de gegevens in het geheugen of op de schijf die een service nodig voor de functie heeft. Status bevat de gegevensstructuren en variabelen die de service leest en schrijft. Afhankelijk van hoe de service is ontworpen, de staat ook advies inwinnen bij bestanden of andere bronnen die zijn opgeslagen op de schijf. De status kan bijvoorbeeld de bestanden die een database worden gebruikt voor het opslaan van gegevens en transactielogboeken.

Status kan worden externalized of geplaatst met de code die de status wordt bewerkt. Normaal gesproken extern u status maken met behulp van een database of ander gegevensarchief die wordt uitgevoerd op verschillende computers via het netwerk of die wordt uitgevoerd buiten het proces op dezelfde computer.

Containers en microservices zijn meest flexibele als de processen die worden uitgevoerd in deze status niet behouden. Omdat toepassingen bijna altijd een status bevatten, gebruikt u een PaaS-oplossing, zoals Azure Database voor MySQL, Azure Database voor PostgreSQL of Azure SQL Database.

Draagbare om toepassingen te bouwen, Zie de volgende richtlijnen:

* [De methodologie 12-factor-app](https://12factor.net/)
* [Een webtoepassing uitvoeren in meerdere Azure-regio 's](https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/multi-region)

## <a name="create-a-storage-migration-plan"></a>Stel een migratieplan voor opslag

**Beste**: Als u Azure Storage, voorbereiden en testen van uw opslag migreren van de primaire regio naar de back-regio.

Uw toepassingen, bijvoorbeeld Azure Storage gebruiken voor hun gegevens. Omdat uw toepassingen worden verdeeld over meerdere AKS-clusters in verschillende regio's, moet u de opslag die is gesynchroniseerd houden. Hier volgen twee algemene manieren om opslag te repliceren:

* Asynchrone replicatie op basis van infrastructuur
* Asynchrone replicatie op basis van een toepassing

### <a name="infrastructure-based-asynchronous-replication"></a>Asynchrone replicatie op basis van infrastructuur

Uw toepassingen mogelijk permanente opslag, zelfs nadat een schil wordt verwijderd. In Kubernetes, kunt u permanente volumes gebruiken om vast te leggen van de opslag van gegevens. Permanente volumes zijn gekoppeld aan een VM-knooppunt en klikt u vervolgens beschikbaar gemaakt voor het gehele product. Permanente volumes Volg schillen, zelfs als de schillen worden verplaatst naar een ander knooppunt binnen hetzelfde cluster.

De replicatiestrategie die u gebruikt, is afhankelijk van uw oplossing. Algemene oplossingen voor opslag, zoals [Gluster](https://docs.gluster.org/en/latest/Administrator%20Guide/Geo%20Replication/), [Ceph](http://docs.ceph.com/docs/master/cephfs/disaster-recovery/), [toren](https://rook.io/docs/rook/master/disaster-recovery.html), en [Portworx](https://docs.portworx.com/scheduler/kubernetes/going-production-with-k8s.html#disaster-recovery-with-cloudsnaps) bieden hun eigen richtlijnen voor herstel na noodgevallen en replicatie.

De typische strategie is het bieden van een algemene opslag punt waar toepassingen hun gegevens kunnen schrijven. Deze gegevens worden vervolgens gerepliceerd in regio's en vervolgens lokaal toegankelijk zijn.

![Asynchrone replicatie op basis van infrastructuur](media/operator-best-practices-bc-dr/aks-infra-based-async-repl.png)

Als u Azure Managed Disks gebruikt, kunt u replicatie en DR-oplossingen, zoals deze:

* [ARK op Azure](https://github.com/heptio/ark/blob/master/docs/azure-config.md)
* [Azure Site Recovery](https://azure.microsoft.com/blog/asr-managed-disks-between-azure-regions/)

### <a name="application-based-asynchronous-replication"></a>Asynchrone replicatie op basis van een toepassing

Kubernetes biedt momenteel geen systeemeigen implementatie voor asynchrone replicatie op basis van een toepassing. Aangezien containers en Kubernetes zijn los gekoppeld en moet een traditionele aanpak van de toepassing of taal werken. De toepassingen zelf repliceren normaal gesproken de storage-aanvragen, die vervolgens worden weggeschreven naar de onderliggende opslag van gegevens van elke cluster.

![Asynchrone replicatie op basis van een toepassing](media/operator-best-practices-bc-dr/aks-app-based-async-repl.png)

## <a name="next-steps"></a>Volgende stappen

In dit artikel is gericht op zakelijke continuïteit en overwegingen van herstel na noodgevallen voor AKS-clusters. Zie voor meer informatie over de bewerkingen voor een cluster in AKS, deze artikelen over aanbevolen procedures:

* [Multitenancy en cluster-isolatie][aks-best-practices-cluster-isolation]
* [Kubernetes scheduler basisfuncties][aks-best-practices-scheduler]

<!-- INTERNAL LINKS -->
[aks-best-practices-scheduler]: operator-best-practices-scheduler.md
[aks-best-practices-cluster-isolation]: operator-best-practices-cluster-isolation.md
