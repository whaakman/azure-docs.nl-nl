---
title: Concepten - beveiliging in Azure Kubernetes-Services (AKS)
description: Meer informatie over beveiliging in Azure Kubernetes Service (AKS), met inbegrip van hoofd- en knooppunt-communicatie, netwerkbeleid en Kubernetes-geheimen.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: iainfou
ms.openlocfilehash: df95329128c93f326b6f2c75fb7faef1a46029cc
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56456500"
---
# <a name="security-concepts-for-applications-and-clusters-in-azure-kubernetes-service-aks"></a>Beveiligingsconcepten voor toepassingen en -clusters in Azure Kubernetes Service (AKS)

Ter bescherming van uw klantgegevens tijdens het uitvoeren van werkbelastingen van toepassingen in Azure Kubernetes Service (AKS), is de beveiliging van uw cluster een van de belangrijkste factoren. Kubernetes bevat beveiligingsonderdelen, zoals *netwerkbeleidsregels* en *geheimen*. Azure wordt vervolgens toegevoegd in onderdelen, zoals netwerkbeveiligingsgroepen en upgraden van clusters die worden georganiseerd. Deze beveiligingsonderdelen worden gecombineerd om te voorkomen dat uw AKS-cluster met de meest recente beveiligingsupdates van OS en Kubernetes-versies en beveiligen met pod verkeer en toegang tot gevoelige referenties.

In dit artikel worden de belangrijkste concepten die beveiligen van uw toepassingen in AKS geïntroduceerd:

- [Beveiliging van de master-onderdelen](#master-security)
- [Beveiliging van knooppunt](#node-security)
- [Upgraden van clusters](#cluster-upgrades)
- [Netwerkbeveiliging](#network-security)
- [Kubernetes Secrets](#kubernetes-secrets)

## <a name="master-security"></a>Beveiliging van master

In AKS uitmaken de Kubernetes-hoofdcomponenten deel van de beheerde service die is geleverd door Microsoft. Elk AKS-cluster is hun eigen Kubernetes één tenants, speciaal model voor de API-Server, Scheduler, enzovoort. Dit model wordt beheerd en onderhouden door Microsoft

Standaard is de Kubernetes API-server maakt gebruik van een openbaar IP-adres, en met FQDN-naam (Fully Qualified Domain Name). U kunt toegang tot de API-server met behulp van Kubernetes op rollen gebaseerd toegangsbeheer en Azure Active Directory beheren. Zie voor meer informatie, [Azure AD-integratie met AKS][aks-aad].

## <a name="node-security"></a>Beveiliging van knooppunt

AKS-knooppunten zijn Azure virtuele machines die u beheren en onderhouden. De knooppunten draait een geoptimaliseerde Ubuntu Linux-distributie met de Docker container-runtime. Wanneer een AKS-cluster wordt gemaakt of uitgebreid, worden automatisch de knooppunten geïmplementeerd met de meest recente beveiligingsupdates OS en configuraties.

OS-beveiligingspatches het Azure-platform automatisch toegepast op de knooppunten op basis van elke nacht. Als een update van het besturingssysteem beveiliging een host opnieuw worden opgestart vereist, wordt dat opnieuw opstarten niet automatisch uitgevoerd. U kunt handmatig opnieuw opstarten de knooppunten of een algemene aanpak is het gebruik [Kured][kured], een open-source opnieuw opstarten-daemon voor Kubernetes. Kured wordt uitgevoerd als een [DaemonSet] [ aks-daemonsets] en bewaakt elk knooppunt op de aanwezigheid van een bestand dat aangeeft dat een herstart vereist is. Opnieuw opstarten worden beheerd in het cluster met behulp van dezelfde [cordon en proces leegmaken](#cordon-and-drain) als de clusterupgrade van een.

Knooppunten worden geïmplementeerd in een subnet privé virtueel netwerk met geen openbare IP-adressen die zijn toegewezen. SSH is standaard ingeschakeld voor probleemoplossing en management-toepassing. Deze SSH-toegang is alleen beschikbaar via het interne IP-adres.

De knooppunten gebruiken voor opslag, Azure Managed Disks. Dit zijn Premium-schijven ondersteund door hoogwaardige SSD's voor de meeste VM-grootten in knooppunt. De gegevens die zijn opgeslagen op beheerde schijven worden automatisch versleuteld in rust in het Azure-platform. Voor een betere redundantie, worden deze schijven ook veilig gerepliceerd binnen de Azure-datacenter.

Kubernetes-omgevingen in AKS of ergens anders, die momenteel zijn niet volledig veilig voor onveilig multitenant gebruik. Aanvullende beveiligingsfuncties zoals *Pod beveiligingsbeleid* of meer fijnmazig op rollen gebaseerd toegangsbeheer (RBAC) voor knooppunten moeilijker aanvallen. Voor de waarde true beveiliging bij het uitvoeren van workloads voor onveilig multitenant, is een hypervisor echter de enige niveau van beveiliging die u moet vertrouwen. Het beveiligingsdomein voor Kubernetes wordt het hele cluster, niet een afzonderlijke knooppunten. Voor deze typen werkbelastingen voor onveilig multitenant, moet u fysiek geïsoleerd clusters. Zie voor meer informatie over manieren om workloads te isoleren [aanbevolen procedures voor het isoleren van de cluster in AKS][cluster-isolation],

## <a name="cluster-upgrades"></a>Upgraden van clusters

Voor beveiliging en naleving, of de nieuwste functies te gebruiken, biedt Azure hulpprogramma's voor de organisatie van de upgrade van een AKS-cluster en -onderdelen. Deze upgrade orchestration bevat zowel de Kubernetes-hoofd- en agent de onderdelen. U vindt een [lijst met beschikbare Kubernetes-versies](supported-kubernetes-versions.md) voor uw AKS-cluster. Als u wilt het upgradeproces start, u een van deze versies beschikbaar. Azure en vervolgens veilig cordons en verkeer naar elk knooppunt AKS en de upgrade uitvoert.

### <a name="cordon-and-drain"></a>Cordon en clusterbesturingssysteem

Tijdens het upgradeproces worden AKS-knooppunten afzonderlijk afgebakend uit het cluster, zodat nieuwe pods niet op deze worden gepland. De knooppunten zijn vervolgens geleegd en bijgewerkt als volgt:

- Bestaande schillen zijn zonder problemen beëindigd en gepland voor de resterende knooppunten.
- Het knooppunt opnieuw wordt opgestart, het upgradeproces is voltooid en wordt terug naar het AKS-cluster.
- Schillen zijn gepland om uit te voeren op deze opnieuw.
- Het volgende knooppunt in het cluster wordt afgebakend en geleegd hetzelfde proces gebruiken totdat alle knooppunten worden bijgewerkt.

Zie voor meer informatie, [een AKS-cluster upgraden][aks-upgrade-cluster].

## <a name="network-security"></a>Netwerkbeveiliging

Voor de connectiviteit en beveiliging met on-premises netwerken, kunt u uw AKS-cluster in bestaande subnetten van virtuele Azure-netwerk implementeren. Deze virtuele netwerken kunnen een Azure-Site-naar-Site VPN of Express Route-verbinding naar uw on-premises netwerk te hebben. Kubernetes inkomend domeincontrollers kunnen worden gedefinieerd met persoonlijke, interne IP-adressen, zodat de services zijn alleen toegankelijk via deze verbinding van het interne netwerk.

### <a name="azure-network-security-groups"></a>Azure-netwerk-beveiligingsgroepen

Als u wilt de stroom van verkeer in virtuele netwerken filteren, gebruikt Azure de regels voor netwerkbeveiligingsgroepen. Deze regels definiëren de bron en doel-IP-adresbereiken, poorten en protocollen die worden toegestaan of geweigerd toegang tot bronnen. Standaardregels worden voor TLS-verkeer naar de Kubernetes API-server en waarbij voor SSH-toegang tot de knooppunten gemaakt. Als u services met load balancers, poorttoewijzingen of routes voor inkomend verkeer maken, Hiermee wijzigt u AKS automatisch de netwerkbeveiligingsgroep voor verkeer voor flow op de juiste wijze.

## <a name="kubernetes-secrets"></a>Kubernetes Secrets

Een Kubernetes *geheim* wordt gebruikt voor het invoeren van gevoelige gegevens in schillen, zoals toegang tot referenties of sleutels. U maakt eerst een geheim met behulp van de Kubernetes-API. Wanneer u uw schil of de implementatie definieert, kan een specifiek geheim worden aangevraagd. Geheimen zijn alleen bedoeld als knooppunten met een geplande schil dat vereist is en het geheim is opgeslagen in *tmpfs*niet geschreven naar schijf. Wanneer de laatste schil op een knooppunt dat is vereist een geheim is verwijderd, wordt het geheim uit van het knooppunt tmpfs verwijderd. Geheimen worden opgeslagen in een bepaalde naamruimte en kunnen alleen worden geopend door schillen in de dezelfde naamruimte.

Het gebruik van geheimen vermindert de gevoelige informatie die is gedefinieerd in de schil of service manifest YAML. In plaats daarvan kunt u het geheim die is opgeslagen in Kubernetes API-Server als onderdeel van uw YAML-manifest aanvragen. Deze benadering biedt alleen de specifieke pod-toegang tot de geheime sleutel.

## <a name="next-steps"></a>Volgende stappen

Als u wilt beginnen met het beveiligen van uw AKS-clusters, Zie [een AKS-cluster upgraden][aks-upgrade-cluster].

Zie de volgende artikelen voor meer informatie over core Kubernetes en concepten voor AKS:

- [Kubernetes / AKS-clusters en workloads][aks-concepts-clusters-workloads]
- [Kubernetes / AKS-identiteit][aks-concepts-identity]
- [Kubernetes / AKS virtuele netwerken][aks-concepts-network]
- [Kubernetes / AKS-opslag][aks-concepts-storage]
- [Kubernetes / AKS schalen][aks-concepts-scale]

<!-- LINKS - External -->
[kured]: https://github.com/weaveworks/kured
[kubernetes-network-policies]: https://kubernetes.io/docs/concepts/services-networking/network-policies/

<!-- LINKS - Internal -->
[aks-daemonsets]: concepts-clusters-workloads.md#daemonsets
[aks-upgrade-cluster]: upgrade-cluster.md
[aks-aad]: aad-integration.md
[aks-concepts-clusters-workloads]: concepts-clusters-workloads.md
[aks-concepts-identity]: concepts-identity.md
[aks-concepts-scale]: concepts-scale.md
[aks-concepts-storage]: concepts-storage.md
[aks-concepts-network]: concepts-network.md
[cluster-isolation]: operator-best-practices-cluster-isolation.md
