---
title: Hoe u ingebouwde Azure-Monitor voor containers | Microsoft Docs
description: Dit artikel wordt beschreven hoe u met het voorbereiden en configureren van Azure Monitor voor containers, zodat u begrijpt hoe de container wordt uitgevoerd en welke problemen met betrekking tot prestaties zijn geïdentificeerd.
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
ms.date: 04/25/2019
ms.author: magoedte
ms.openlocfilehash: 10b80a9749c5698195ac5d3493ac3b07fd6e24e1
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65073313"
---
# <a name="how-to-onboard-azure-monitor-for-containers"></a>Hoe u ingebouwde Azure-Monitor voor containers  

In dit artikel biedt een overzicht van de opties die beschikbaar zijn voor het instellen van Azure Monitor voor containers voor het controleren van de prestaties van workloads die worden geïmplementeerd in Kubernetes-omgevingen en die worden gehost op [Azure Kubernetes Service](https://docs.microsoft.com/azure/aks/).

Azure Monitor voor containers kan worden ingeschakeld voor nieuwe of bestaande implementaties van een of meer van AKS met behulp van de volgende ondersteunde methoden:

* In de Azure portal, Azure PowerShell, of met Azure CLI
* Met behulp van [Terraform en AKS](../../terraform/terraform-create-k8s-cluster-with-tf-and-aks.md)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Vereisten 
Voordat u begint, zorg ervoor dat u het volgende hebt:

- **Een Log Analytics-werkruimte.** Als u bewaking van uw nieuwe AKS-cluster of laat het onboarding-ervaring een standaardwerkruimte maken in de standaard-resourcegroep van het AKS-cluster-abonnement, kunt u deze maken. Als u wilt deze zelf maken, kunt u het maken via [Azure Resource Manager](../platform/template-workspace-configuration.md), tot en met [PowerShell](../scripts/powershell-sample-create-workspace.md?toc=%2fpowershell%2fmodule%2ftoc.json), of in de [Azure-portal](../learn/quick-create-workspace.md).
- U bent lid van de **rol van inzender van Log Analytics** containerbewaking inschakelen. Zie voor meer informatie over het beheren van toegang tot een Log Analytics-werkruimte [werkruimten beheren](../platform/manage-access.md).
- U bent lid van de **[eigenaar](../../role-based-access-control/built-in-roles.md#owner)** -rol op de AKS-cluster-bron. 

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

## <a name="components"></a>Onderdelen 

De mogelijkheid om prestaties te bewaken, is afhankelijk van een beperkte Log Analytics-agent voor Linux speciaal ontwikkeld voor Azure Monitor voor containers. Deze gespecialiseerde agent verzamelt prestaties en gebeurtenisgegevens van alle knooppunten in het cluster en de agent wordt automatisch geregistreerd en geïmplementeerd met de opgegeven Log Analytics-werkruimte tijdens de implementatie. De versie van de agent wordt door microsoft / oms:ciprod04202018 of hoger, en wordt vertegenwoordigd door een datum in de volgende indeling: *mmddyyyy*. 

>[!NOTE]
>Een AKS-cluster met Windows Server-knooppunten hebt met de preview-versie van Windows Server-ondersteuning voor AKS geen agent is geïnstalleerd voor het verzamelen van gegevens en deze doorsturen naar Azure Monitor. In plaats daarvan een Linux-knooppunt automatisch in het cluster is geïmplementeerd als onderdeel van de standaardimplementatie verzamelt en verzendt de gegevens naar Azure Monitor namens alle Windows-knooppunten in het cluster.  
>

Wanneer een nieuwe versie van de agent wordt uitgebracht, wordt deze automatisch bijgewerkt op uw beheerde Kubernetes-clusters die worden gehost in Azure Kubernetes Service (AKS). Als u wilt volgen de versies die zijn uitgebracht, Zie [agent aankondigingen](https://github.com/microsoft/docker-provider/tree/ci_feature_prod). 

>[!NOTE] 
>Als u al een AKS-cluster hebt geïmplementeerd, kunt u inschakelen bewaking met behulp van Azure CLI of een opgegeven Azure Resource Manager-sjabloon, zoals verderop in dit artikel wordt gedemonstreerd. U kunt geen gebruiken `kubectl` als u wilt bijwerken, verwijderen, opnieuw implementeren of implementeren van de agent. De sjabloon opnieuw moet worden geïmplementeerd in dezelfde resourcegroep bevinden als het cluster.

U kunt Azure Monitor inschakelen voor containers met behulp van een van de volgende methoden die worden beschreven in de volgende tabel.

| Implementatiestatus | Methode | Description | 
|------------------|--------|-------------| 
| Nieuw AKS-cluster | [Cluster maken met behulp van Azure CLI](../../aks/kubernetes-walkthrough.md#create-aks-cluster)| U kunt inschakelen bewaking van een nieuw AKS-cluster die u met Azure CLI maakt. | 
| | [Cluster maken met behulp van Terraform](container-insights-enable-new-cluster.md#enable-using-terraform)| U kunt inschakelen bewaking van een nieuw AKS-cluster die u maakt met de open-source hulpprogramma Terraform. | 
| Bestaand AKS-cluster | [Inschakelen met behulp van Azure CLI](container-insights-enable-existing-clusters.md#enable-using-azure-cli) | U kunt inschakelen bewaking van een AKS-cluster al geïmplementeerd met behulp van Azure CLI. | 
| |[Inschakelen met behulp van Terraform](container-insights-enable-existing-clusters.md#enable-using-terraform) | U kunt inschakelen bewaking van een AKS-cluster al geïmplementeerd met behulp van de open-source hulpprogramma Terraform. | 
| | [Inschakelen van Azure Monitor](container-insights-enable-existing-clusters.md#enable-from-azure-monitor-in-the-portal)| U kunt inschakelen bewaking van een of meer AKS clusters al geïmplementeerd vanaf de AKS-cluster met meerdere pagina in Azure Monitor. | 
| | [Inschakelen van AKS-cluster](container-insights-enable-existing-clusters.md#enable-directly-from-aks-cluster-in-the-portal)| U kunt rechtstreeks vanuit een AKS-cluster in Azure portal-controle inschakelen. | 
| | [Inschakelen met behulp van een Azure Resource Manager-sjabloon](container-insights-enable-existing-clusters.md#enable-using-an-azure-resource-manager-template)| U kunt inschakelen bewaking van een AKS-cluster met een vooraf geconfigureerde Azure Resource Manager-sjabloon. | 

## <a name="next-steps"></a>Volgende stappen

* Deze metrische gegevens voor servicestatus zijn met bewaking ingeschakeld om vast te leggen van de gezondheid van metrische gegevens voor de AKS-clusterknooppunten en de schillen, beschikbaar in de Azure-portal. Zie voor meer informatie over het gebruik van Azure Monitor voor containers, [weergave Azure Kubernetes Service health](container-insights-analyze.md).
