---
title: Kubernetes op Azure Stack implementeren | Microsoft Docs
description: Informatie over het implementeren van Kubernetes op Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 01/16/2019
ms.openlocfilehash: 9ce21a6b01dc4f4a99e5c70faf1e004bf7918dad
ms.sourcegitcommit: a8948ddcbaaa22bccbb6f187b20720eba7a17edc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56594114"
---
# <a name="deploy-kubernetes-to-azure-stack"></a>Kubernetes op Azure Stack implementeren

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

> [!Note]  
> Kubernetes in Azure Stack is in preview. Azure Stack-niet-verbonden scenario wordt momenteel niet ondersteund door de Preview-versie.

U kunt de stappen in dit artikel om te implementeren en instellen van de resources voor Kubernetes in een enkele, gecoördineerde bewerking. De stappen gebruikt een Azure Resource Manager-oplossingssjabloon. U moet de vereiste gegevens verzamelen over uw Azure Stack-installatie genereren van de sjabloon, en vervolgens naar de cloud kunt implementeren. De Azure Stack-sjabloon gebruikt niet de dezelfde beheerde AKS-service aangeboden in de globale Azure.

## <a name="kubernetes-and-containers"></a>Kubernetes en containers

U kunt installeren met behulp van Azure Resource Manager-sjablonen die zijn gegenereerd door de ACS-Engine in Azure Stack Kubernetes. [Kubernetes](https://kubernetes.io) is een open-source systeem voor het automatiseren van implementatie, schalen en beheren van toepassingen in containers. Een [container](https://www.docker.com/what-container) is in een afbeelding. De containerinstallatiekopie is vergelijkbaar met een virtuele machine, maar in tegenstelling tot een virtuele machine, de container bevat alleen de benodigde resources beschikken om uit te voeren van een toepassing, zoals de code, runtime voor het uitvoeren van de code, specifieke bibliotheken en -instellingen.

U kunt Kubernetes te gebruiken:

- Ontwikkel toepassingen met grote aantallen schaalbare en kan worden bijgewerkt, die kunnen worden geïmplementeerd in een paar seconden. 
- Vereenvoudig het ontwerp van uw toepassing en de betrouwbaarheid verbeteren door verschillende Helm-toepassingen. [Helm](https://github.com/kubernetes/helm) is een open-source verpakking-hulpprogramma dat helpt u bij het installeren en beheren van de levenscyclus van toepassingen met Kubernetes.
- Eenvoudig bewaken en diagnoses uitvoeren van de status van uw toepassingen.

U wordt enkel gefactureerd voor de compute-gebruik door de ondersteuning van uw cluster knooppunten vereist. Zie voor meer informatie, [gebruik en facturering in Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-billing-and-chargeback).

## <a name="deploy-kubernetes"></a>Implementeren van Kubernetes

De stappen voor het implementeren van een Kubernetes-cluster in Azure Stack is afhankelijk van uw identity management-service. Controleer of de oplossing voor identiteitsbeheer die worden gebruikt door de installatie van Azure Stack. Neem contact op met uw Azure Stack-beheerder om te controleren of uw identity management-service.

- **Azure Active Directory (Azure AD)**  
Zie voor instructies over het installeren van het cluster bij het gebruik van Azure AD [Kubernetes implementeren naar Azure Stack met Azure Active Directory (Azure AD)](azure-stack-solution-template-kubernetes-azuread.md).

- **Active Directory Federated Services (AD FS)**  
Zie voor instructies over het installeren van het cluster wanneer u AD FS [Kubernetes implementeren naar Azure Stack met behulp van Active Directory Federated Services (AD FS)](azure-stack-solution-template-kubernetes-adfs.md).

## <a name="connect-to-your-cluster"></a>Verbinding maken met uw cluster

U kunt nu verbinding maken met uw cluster. Het model kunt u vinden in de resourcegroep van uw cluster en de naam `k8s-master-<sequence-of-numbers>`. Gebruik een SSH-client verbinding maken met de master. In het model, kunt u **kubectl**, de Kubernetes-opdrachtregelclient voor het beheren van uw cluster. Zie voor instructies [Kubernetes.io](https://kubernetes.io/docs/reference/kubectl/overview).

Mogelijk merkt u ook de **Helm** Pakketbeheer nuttig voor het installeren en implementeren van apps met uw cluster. Zie voor instructies over het installeren en gebruik van Helm met uw cluster [helm.sh](https://helm.sh/).

## <a name="next-steps"></a>Volgende stappen

[Een Kubernetes (voor de Azure Stack-operator) toevoegen aan de Marketplace](../azure-stack-solution-template-kubernetes-cluster-add.md)

[Kubernetes op Azure Stack met Azure Active Directory (Azure AD) implementeren](azure-stack-solution-template-kubernetes-azuread.md)

[Kubernetes op Azure Stack met behulp van Active Directory Federated Services (AD FS) implementeren](azure-stack-solution-template-kubernetes-adfs.md)

[Kubernetes op Azure](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)
