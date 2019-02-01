---
title: Operator aanbevolen procedures - Cluster isolatie in Azure Kubernetes Services (AKS)
description: Meer over de best practices uit de cluster-operator voor isolatie in Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: iainfou
ms.openlocfilehash: 94aaa72497a8a5f171d6b42f59a3c5b507c71492
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55495000"
---
# <a name="best-practices-for-cluster-isolation-in-azure-kubernetes-service-aks"></a>Aanbevolen procedures voor het cluster isolatie in Azure Kubernetes Service (AKS)

Wanneer u clusters in Azure Kubernetes Service (AKS) beheert, moet u vaak teams en workloads te isoleren. AKS biedt flexibiliteit voor het uitvoeren van clusters met meerdere tenants en resources isoleren. Als u wilt uw investering in Kubernetes te maximaliseren, moeten deze functies en isolatie voor meerdere tenants worden begrepen en geïmplementeerd.

Deze aanbevolen procedures voor richt zich op de isolatie voor clusteroperators. In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Plan voor clusters met meerdere tenants en scheiding van resources
> * Logische of fysieke isolatie in uw AKS-clusters gebruiken

## <a name="design-clusters-for-multi-tenancy"></a>Clusters van ontwerp voor multitenancy

Kubernetes biedt functies waarmee u logisch kunt isoleren teams en werkbelastingen in hetzelfde cluster. Het doel moet worden voor het minste aantal bevoegdheden binnen het bereik van de resources die elk team nodig heeft. Een [Namespace] [ k8s-namespaces] in Kubernetes maakt u een grens van een logische isolatie. Aanvullende kubernetes-functies en overwegingen voor isolatie en multitenancy zijn onder andere de volgende gebieden:

* **Planning** omvat het gebruik van basisfuncties zoals resourcequota en pod onderbreking budgetten. Zie voor meer informatie over deze functies [aanbevolen procedures voor basic scheduler-functies in AKS][aks-best-practices-scheduler].
  * Meer geavanceerde functies van de scheduler bevatten taints en tolerations, knooppunt selectoren: en knooppunt- en pod affiniteit of anti-affiniteit. Zie voor meer informatie over deze functies [aanbevolen procedures voor geavanceerde scheduler-functies in AKS][aks-best-practices-advanced-scheduler].
* **Netwerken** omvat het gebruik van beleid voor het beheren van de verkeersstroom in-en uitchecken van schillen netwerken.
* **Verificatie en autorisatie** de gebruiker van op rollen gebaseerd toegangsbeheer (RBAC) en Azure Active Directory (AD)-integratie, pod identiteiten en geheimen op te nemen in Azure Key Vault. Zie voor meer informatie over deze functies [aanbevolen procedures voor verificatie en autorisatie in AKS][aks-best-practices-identity].
* **Containers** pod-beveiligingsbeleid, pod security contexten, scannen afbeeldingen en runtimes beveiligingsproblemen bevatten. Omvat ook het gebruik van App chassis of Seccomp (beveiligde computers) voor het beperken van toegang tot de container naar het onderliggende knooppunt.

## <a name="logically-isolate-clusters"></a>Logisch geïsoleerd clusters

**Aanbevolen procedurerichtlijn** -logische isolatie gebruikt om teams en projecten te scheiden. Probeer te minimaliseren van het aantal fysieke AKS-clusters die u implementeert voor teams of toepassingen isoleren.

Met logische isolatie, kan een AKS-cluster worden gebruikt voor meerdere werkbelastingen, teams en omgevingen. Kubernetes [naamruimten] [ k8s-namespaces] vormen de grens logische isolatie voor werkbelastingen en bronnen.

![Logische isolatie van een Kubernetes-cluster in AKS](media/operator-best-practices-cluster-isolation/logical-isolation.png)

Logische scheiding van clusters bevat meestal een hogere dichtheid pod dan fysiek geïsoleerd clusters. Er is minder overtollige computercapaciteit die bevindt zich niet in het cluster actief. In combinatie met het Kubernetes-cluster automatisch schalen, kunt u het aantal knooppunten schaal omhoog of omlaag voldoen aan eisen. Deze best practice-benadering voor automatisch schalen kunt u alleen het aantal knooppunten die nodig zijn uitgevoerd en minimaliseert de kosten.

Kubernetes-omgevingen in AKS of ergens anders, zijn niet volledig veilig voor onveilig multitenant gebruik. Aanvullende beveiligingsfuncties zoals *Pod beveiligingsbeleid* en meer fijnmazig op rollen gebaseerd toegangsbeheer (RBAC) voor knooppunten aanvallen lastiger zijn. Voor de waarde true beveiliging bij het uitvoeren van workloads voor onveilig multitenant, is een hypervisor echter de enige niveau van beveiliging die u moet vertrouwen. Het beveiligingsdomein voor Kubernetes wordt het hele cluster, niet een afzonderlijke knooppunten. Voor deze typen werkbelastingen voor onveilig multitenant, moet u fysiek geïsoleerd clusters.

## <a name="physically-isolate-clusters"></a>Fysiek geïsoleerd clusters

**Aanbevolen procedurerichtlijn** -het gebruik van fysieke isolatie voor elke afzonderlijke team of de implementatie van de toepassing minimaliseren. In plaats daarvan gebruik *logische* isolatie, zoals beschreven in de vorige sectie.

Er is een veelgebruikte manier voor het cluster isolatie met fysiek gescheiden AKS-clusters. In dit isolatiemodel teams of workloads toegewezen hun eigen AKS-cluster. Deze aanpak vaak ziet eruit als de eenvoudigste manier om te isoleren werkbelastingen of teams, maar voegt extra beheer en financiële overhead toe. U nu deze meerdere clusters beheren, en moet afzonderlijk toegang en machtigingen toe te wijzen. U wordt ook gefactureerd voor de afzonderlijke knooppunten.

![Fysieke isolatie van afzonderlijke Kubernetes-clusters in AKS](media/operator-best-practices-cluster-isolation/physical-isolation.png)

Fysiek afzonderlijke clusters hebben meestal een lage pod dichtheid. Omdat elk team of de werkbelasting hun eigen AKS-cluster, is het cluster vaak te veel met compute-resources worden ingericht. Vaak is een klein aantal schillen gepland op die knooppunten. Niet-gebruikte capaciteit op de knooppunten kan niet worden gebruikt voor toepassingen of services in de ontwikkeling door andere teams. Deze extra resources bijdragen aan de extra kosten in fysiek gescheiden clusters.

## <a name="next-steps"></a>Volgende stappen

In dit artikel gericht op het cluster-isolatie. Zie voor meer informatie over de bewerkingen voor een cluster in AKS, de volgende aanbevolen procedures:

* [Kubernetes scheduler basisfuncties][aks-best-practices-scheduler]
* [Geavanceerde functies voor Kubernetes-scheduler][aks-best-practices-advanced-scheduler]
* [Verificatie en autorisatie][aks-best-practices-identity]

<!-- EXTERNAL LINKS -->

<!-- INTERNAL LINKS -->
[k8s-namespaces]: concepts-clusters-workloads.md#namespaces
[aks-best-practices-scheduler]: operator-best-practices-scheduler.md
[aks-best-practices-advanced-scheduler]: operator-best-practices-advanced-scheduler.md
[aks-best-practices-identity]: operator-best-practices-identity.md
