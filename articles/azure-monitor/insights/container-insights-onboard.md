---
title: Azure Monitor voor containers inschakelen | Microsoft Docs
description: In dit artikel wordt beschreven hoe u Azure Monitor voor containers inschakelt en configureert, zodat u kunt begrijpen hoe uw container presteert en welke prestatie problemen zijn geïdentificeerd.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/12/2019
ms.author: magoedte
ms.openlocfilehash: 25be8f166fec8a311fdc2ed1fa3fca6339185e94
ms.sourcegitcommit: 10251d2a134c37c00f0ec10e0da4a3dffa436fb3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2019
ms.locfileid: "67867519"
---
# <a name="how-to-enable-azure-monitor-for-containers"></a>Azure Monitor inschakelen voor containers

In dit artikel vindt u een overzicht van de beschik bare opties voor het instellen van Azure Monitor voor containers om de prestaties te bewaken van werk belastingen die in Kubernetes-omgevingen zijn geïmplementeerd en die worden gehost op de [Azure Kubernetes-service](https://docs.microsoft.com/azure/aks/).

Azure Monitor voor containers kan worden ingeschakeld voor nieuwe of bestaande implementaties van een of meer van AKS met behulp van de volgende ondersteunde methoden:

* Van de Azure Portal, Azure PowerShell of met Azure CLI
* Met behulp van [Terraform en AKS](../../terraform/terraform-create-k8s-cluster-with-tf-and-aks.md)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Vereisten
Voordat u begint, zorg ervoor dat u het volgende hebt:

* **Een Log Analytics-werk ruimte.**

    Azure Monitor voor containers ondersteunt een Log Analytics-werk ruimte in de regio's die worden vermeld in azure- [producten per regio, met](https://azure.microsoft.com/global-infrastructure/services/?regions=all&products=monitor)uitzonde ring van de regio **US gov-Virginia**.

    U kunt een werk ruimte maken wanneer u de bewaking van uw nieuwe AKS-cluster inschakelt of de voorbereidings ervaring een standaard werkruimte maakt in de standaard resource groep van het AKS-cluster abonnement. Als u wilt deze zelf maken, kunt u het maken via [Azure Resource Manager](../platform/template-workspace-configuration.md), tot en met [PowerShell](../scripts/powershell-sample-create-workspace.md?toc=%2fpowershell%2fmodule%2ftoc.json), of in de [Azure-portal](../learn/quick-create-workspace.md). Zie [regio toewijzing voor Azure monitor voor containers](container-insights-region-mapping.md)voor een lijst met de ondersteunde toewijzings paren die worden gebruikt voor de standaardwerk ruimte.

* U bent lid van de **rol log Analytics Inzender** om container bewaking in te scha kelen. Zie voor meer informatie over het beheren van toegang tot een Log Analytics-werkruimte [werkruimten beheren](../platform/manage-access.md).

* U bent lid van de rol **[eigenaar](../../role-based-access-control/built-in-roles.md#owner)** op de cluster bron AKS.

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

* Prometheus-metrische gegevens worden standaard niet verzameld. Voordat u [de agent configureert](container-insights-agent-config.md) om deze te verzamelen, is het belang rijk dat u de Prometheus- [documentatie](https://prometheus.io/) bekijkt om te begrijpen wat u kunt definiëren.

## <a name="components"></a>Onderdelen

De mogelijkheid om prestaties te bewaken, is afhankelijk van een beperkte Log Analytics-agent voor Linux speciaal ontwikkeld voor Azure Monitor voor containers. Deze gespecialiseerde agent verzamelt prestaties en gebeurtenisgegevens van alle knooppunten in het cluster en de agent wordt automatisch geregistreerd en geïmplementeerd met de opgegeven Log Analytics-werkruimte tijdens de implementatie. De versie van de agent wordt door microsoft / oms:ciprod04202018 of hoger, en wordt vertegenwoordigd door een datum in de volgende indeling: *mmddyyyy*.

>[!NOTE]
>Met de preview-versie van Windows Server-ondersteuning voor AKS is er voor een AKS-cluster met Windows Server-knoop punten geen agent geïnstalleerd om gegevens te verzamelen en door te sturen naar Azure Monitor. In plaats daarvan wordt een Linux-knoop punt dat automatisch in het cluster wordt geïmplementeerd als onderdeel van de standaard implementatie, worden de gegevens verzameld en doorgestuurd naar Azure Monitor voor alle Windows-knoop punten in het cluster.  
>

Wanneer een nieuwe versie van de agent wordt uitgebracht, wordt deze automatisch bijgewerkt op uw beheerde Kubernetes-clusters die worden gehost in Azure Kubernetes Service (AKS). Als u wilt volgen de versies die zijn uitgebracht, Zie [agent aankondigingen](https://github.com/microsoft/docker-provider/tree/ci_feature_prod).

>[!NOTE]
>Als u al een AKS-cluster hebt geïmplementeerd, kunt u inschakelen bewaking met behulp van Azure CLI of een opgegeven Azure Resource Manager-sjabloon, zoals verderop in dit artikel wordt gedemonstreerd. U kunt geen gebruiken `kubectl` als u wilt bijwerken, verwijderen, opnieuw implementeren of implementeren van de agent.
>De sjabloon opnieuw moet worden geïmplementeerd in dezelfde resourcegroep bevinden als het cluster.

U schakelt Azure Monitor voor containers in met behulp van een van de volgende methoden die worden beschreven in de volgende tabel.

| Implementatie status | Methode | Description |
|------------------|--------|-------------|
| Nieuw AKS-cluster | [Een cluster maken met Azure CLI](../../aks/kubernetes-walkthrough.md#create-aks-cluster)| U kunt de bewaking inschakelen van een nieuw AKS-cluster dat u maakt met Azure CLI. |
| | [Een cluster maken met behulp van terraform](container-insights-enable-new-cluster.md#enable-using-terraform)| U kunt de bewaking inschakelen van een nieuw AKS-cluster dat u maakt met behulp van de open source-terraform. |
| Bestaand AKS-cluster | [Inschakelen met behulp van Azure CLI](container-insights-enable-existing-clusters.md#enable-using-azure-cli) | U kunt de bewaking van een AKS-cluster inschakelen dat al is geïmplementeerd met behulp van Azure CLI. |
| |[Inschakelen met behulp van terraform](container-insights-enable-existing-clusters.md#enable-using-terraform) | U kunt de bewaking van een AKS-cluster inschakelen dat al is geïmplementeerd met het open source-hulp programma terraform. |
| | [Inschakelen vanaf Azure Monitor](container-insights-enable-existing-clusters.md#enable-from-azure-monitor-in-the-portal)| U kunt de bewaking inschakelen van een of meer AKS-clusters die al zijn geïmplementeerd op de pagina met meerdere clusters van AKS in Azure Monitor. |
| | [Inschakelen vanuit AKS-cluster](container-insights-enable-existing-clusters.md#enable-directly-from-aks-cluster-in-the-portal)| U kunt de bewaking rechtstreeks vanuit een AKS-cluster inschakelen in de Azure Portal. |
| | [Inschakelen met behulp van een Azure Resource Manager sjabloon](container-insights-enable-existing-clusters.md#enable-using-an-azure-resource-manager-template)| U kunt de bewaking van een AKS-cluster inschakelen met een vooraf geconfigureerde Azure Resource Manager-sjabloon. |

## <a name="next-steps"></a>Volgende stappen

* Deze metrische gegevens voor servicestatus zijn met bewaking ingeschakeld om vast te leggen van de gezondheid van metrische gegevens voor de AKS-clusterknooppunten en de schillen, beschikbaar in de Azure-portal. Zie voor meer informatie over het gebruik van Azure Monitor voor containers, [weergave Azure Kubernetes Service health](container-insights-analyze.md).
