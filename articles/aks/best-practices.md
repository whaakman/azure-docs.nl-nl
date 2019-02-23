---
title: Aanbevolen procedures voor Azure Kubernetes Service (AKS)
description: Verzameling van de cluster-operator en developer best practices om te bouwen en beheren van toepassingen in Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 12/07/2018
ms.author: iainfou
ms.openlocfilehash: c73f2fea808944847fab6a675914ee57e537560b
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56731324"
---
# <a name="cluster-operator-and-developer-best-practices-to-build-and-manage-applications-on-azure-kubernetes-service-aks"></a>Cluster-operator en developer aanbevolen procedures voor het bouwen en beheren van toepassingen in Azure Kubernetes Service (AKS)

Als u wilt bouwen en uitvoeren van toepassingen is in Azure Kubernetes Service (AKS), zijn er enkele belangrijke overwegingen om te begrijpen en te implementeren. Deze gebieden zijn multitenancy en scheduler-functies, cluster en pod beveiliging of zakelijke continuïteit en herstel na noodgevallen. De volgende aanbevolen procedures worden gegroepeerd zodat clusteroperators en ontwikkelaars begrijpen van de overwegingen voor elk van deze gebieden en implementeren van de desbetreffende functies.

Deze aanbevolen procedures en conceptuele artikelen zijn geschreven in combinatie met de AKS-productgroep, engineering-teams en veld teams, met inbegrip van globale zwart gordels (GBBs).

## <a name="cluster-operator-best-practices"></a>Aanbevolen procedures voor cluster-operator

Als een cluster-operator, werkt u samen met toepassingseigenaren van en ontwikkelaars aan de hand van hun behoeften. U kunt vervolgens de volgende best practices naar uw AKS-clusters configureren indien nodig.

**Multitenancy**

* [Aanbevolen procedures voor het cluster-isolatie](operator-best-practices-cluster-isolation.md)
    * Bevat kernonderdelen van multitenancy en logische isolatie met naamruimten.
* [Aanbevolen procedures voor basic scheduler-functies](operator-best-practices-scheduler.md)
    * Omvat het gebruik van resourcequota en pod onderbreking budgetten.
* [Aanbevolen procedures voor geavanceerde scheduler-functies](operator-best-practices-advanced-scheduler.md)
    * Omvat het gebruik van taints en tolerations, knooppunt selectoren en affiniteit, en de affiniteit tussen pod en anti-affiniteit.
* [Aanbevolen procedures voor verificatie en autorisatie](operator-best-practices-identity.md)
    * Integratie met Azure Active Directory, met behulp van op rollen gebaseerd toegangsbeheer (RBAC) en pod-id's bevat.

**Beveiliging**

* [Aanbevolen procedures voor beveiliging van clusters en upgrades](operator-best-practices-cluster-security.md)
    * Bevat het beveiligen van toegang tot de API-server, beperken van toegang tot de container en beheren van upgrades en knooppunt opnieuw wordt opgestart.
* [Aanbevolen procedures voor het beheer van container-installatiekopie en beveiliging](operator-best-practices-container-image-management.md)
    * Bevat de installatiekopie en verbeteren, met behulp van vertrouwde registers en geautomatiseerde builds basisinstallatiekopie updates beveiligen...
* [Best practices voor beveiliging van de schil](developer-best-practices-pod-security.md)
    * Beveiligen van toegang tot resources, referentieblootstelling te beperken en het gebruik van de schil identiteiten en digitale sleutelkluizen bevat.

**Netwerk en opslag**

* [Aanbevolen procedures voor verbinding met het netwerk](operator-best-practices-network.md)
    * Bevat verschillende modellen, met behulp van de ingangs- en web application Firewall (WAF) en de beveiliging van knooppunt SSH-toegang.
* [Aanbevolen procedures voor opslag en back-ups](operator-best-practices-storage.md)
    * Omvat het kiezen van het juiste type en knooppunt opslaggrootte, dynamisch inrichten van volumes en back-ups van gegevens.

**Enterprise-ready-workloads uitvoeren**

* [Aanbevolen procedures voor zakelijke continuïteit en herstel na noodgevallen](operator-best-practices-multi-region.md)
    * Omvat het gebruik van regioparen, meerdere clusters met Azure Traffic Manager, en geo-replicatie van containerinstallatiekopieën.

## <a name="developer-best-practices"></a>Aanbevolen procedures voor ontwikkelaars

Als ontwikkelaar of de eigenaar van de toepassing, kunt u uw ontwikkeling te vereenvoudigen en definiëren prestatiebehoeften van de toepassing vereist.

* [Aanbevolen procedures voor toepassingsontwikkelaars om resources te beheren](developer-best-practices-resource-management.md)
    * Bevat de schil resourceaanvragen en -limieten, ontwikkelingsprogramma's configureren en controleren op problemen met toepassingen te definiëren.
* [Best practices voor beveiliging van de schil](developer-best-practices-pod-security.md)
    * Beveiligen van toegang tot resources, referentieblootstelling te beperken en het gebruik van de schil identiteiten en digitale sleutelkluizen bevat.

## <a name="kubernetes--aks-concepts"></a>Kubernetes / AKS-concepten

Om te begrijpen van enkele van de functies en onderdelen van deze aanbevolen procedures, ziet u ook de volgende conceptuele artikelen voor clusters in Azure Kubernetes Service (AKS):

* [Belangrijkste concepten van Kubernetes](concepts-clusters-workloads.md)
* [Toegang en identiteit](concepts-identity.md)
* [Beveiligingsconcepten](concepts-security.md)
* [Netwerkconcepten](concepts-network.md)
* [Opslagopties](concepts-storage.md)
* [Opties voor schalen](concepts-scale.md)

## <a name="next-steps"></a>Volgende stappen

Als u nodig hebt om aan de slag met AKS, volgt u een van de Quick starts voor het implementeren van een Azure Kubernetes Service (AKS)-cluster met de [Azure CLI](kubernetes-walkthrough.md) of [Azure-portal](kubernetes-walkthrough-portal.md).
