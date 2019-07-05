---
title: Meer informatie over het controleren van de inhoud van een virtuele machine
description: Meer informatie over hoe Azure Policy Rego en Open-beleidsagent gebruikt voor het beheren van clusters op Azure Kubernetes Service.
author: DCtheGeek
ms.author: dacoulte
ms.date: 06/24/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: fdb392533e28df1d50e90c842d0117385afb254b
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67453903"
---
# <a name="understand-azure-policy-for-azure-kubernetes-service"></a>Meer informatie over Azure Policy voor Azure Kubernetes Service

Azure-beleid kan worden geïntegreerd met de [Azure Kubernetes Service](../../../aks/intro-kubernetes.md) (AKS) om toe te passen op schaal enforcements en de beveiliging op uw clusters in een gecentraliseerd, consistente manier.
Door het gebruik van uitbreiden [GateKeeper](https://github.com/open-policy-agent/gatekeeper), een _toelating controller webhook_ voor [Open beleidsagent](https://www.openpolicyagent.org/) (OPA), Azure Policy maakt het mogelijk om te beheren en te rapporteren over de naleving status van uw Azure-resources en de AKS-clusters vanaf één locatie.

> [!NOTE]
> Azure Policy voor AKS is beperkte Preview-versie en biedt alleen ondersteuning voor ingebouwde beleidsdefinities.

## <a name="overview"></a>Overzicht

Als u wilt inschakelen en gebruiken van Azure Policy voor AKS met uw AKS-cluster, moet u de volgende acties uitvoeren:

- [Aanmelden voor preview-functies](#opt-in-for-preview)
- [De Azure Policy-invoegtoepassing installeren](#installation-steps)
- [Een beleidsdefinitie toewijzen voor AKS](#built-in-policies)
- [Wacht op validatie](#validation-and-reporting-frequency)

## <a name="opt-in-for-preview"></a>Aanmelden voor Preview-versie

Voordat u de Azure Policy-invoegtoepassing installeren of inschakelen van een van de service-functies, het abonnement moet inschakelen de **Microsoft.ContainerService** resourceprovider en de **Microsoft.PolicyInsights**resourceprovider, en vervolgens om de Preview-versie lid te worden goedgekeurd. Als u wilt deelnemen aan de Preview-versie, volg deze stappen in de Azure portal of met Azure CLI:

- Azure Portal:

  1. Registreert de **Microsoft.ContainerService** en **Microsoft.PolicyInsights** resourceproviders. Zie voor stappen [resourceproviders en typen](../../../azure-resource-manager/resource-manager-supported-services.md#azure-portal).

  1. Start de Azure Policy-service in Azure Portal door **Alle services** te selecteren en dan **Beleid** te zoeken en te selecteren.

     ![Zoeken naar beleid in alle Services](../media/rego-for-aks/search-policy.png)

  1. Selecteer **Join Preview** aan de linkerkant van de Azure Policy-pagina.

     ![Deelnemen aan het beleid voor het AKS-preview](../media/rego-for-aks/join-aks-preview.png)

  1. Selecteer de rij van het abonnement dat u wilt toevoegen aan de Preview-versie.

  1. Selecteer de **Opt-in voor** knop aan de bovenkant van de lijst met abonnementen.

- Azure CLI:

  ```azurecli-interactive
  # Log in first with az login if you're not using Cloud Shell

  # Provider register: Register the Azure Kubernetes Services provider
  az provider register --namespace Microsoft.ContainerService

  # Provider register: Register the Azure Policy provider
  az provider register --namespace Microsoft.PolicyInsights

  # Feature register: enables installing the add-on
  az feature register --namespace Microsoft.ContainerService --name AKS-AzurePolicyAutoApprove

  # Feature register: enables the add-on to call the Azure Policy resource provider
  az feature register --namespace Microsoft.PolicyInsights --name AKS-DataplaneAutoApprove
  ```

## <a name="azure-policy-add-on"></a>Azure Policy-invoegtoepassing

De _Azure Policy-invoegtoepassing_ voor Kubernetes de Azure Policy-service wordt verbonden met de GateKeeper toelating controller. De invoegtoepassing, die is geïnstalleerd in de _azure-beleid_ naamruimte enacts de volgende functies:

- Controle met Azure Policy voor toewijzingen aan het AKS-cluster
- Gedownload en in de cache opgeslagen beleid voor meer informatie, inclusief de _rego_ beleidsdefinitie, als **configmaps**
- Controle op naleving voor een volledige scan op het AKS-cluster wordt uitgevoerd
- Controle van rapporten en compatibiliteitsdetails terug naar Azure Policy

### <a name="installing-the-add-on"></a>De invoegtoepassing installeren

#### <a name="prerequisites"></a>Vereisten

Voordat u de invoegtoepassing in uw AKS-cluster installeert, moet de preview-extensie worden geïnstalleerd. Deze stap wordt uitgevoerd met Azure CLI:

1. U moet de Azure CLI versie 2.0.62 of later geïnstalleerd en geconfigureerd. Voer `az --version` uit om de versie te bekijken. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren](/cli/azure/install-azure-cli).

1. Het AKS-cluster moet versie _1.10_ of hoger. Gebruik het volgende script naar uw AKS-clusterversie valideren:

   ```azurecli-interactive
   # Log in first with az login if you're not using Cloud Shell

   # Look for the value in kubernetesVersion
   az aks list
   ```

1. Installeer versie _0.4.0_ preview van Azure CLI-extensie voor AKS, `aks-preview`:

   ```azurecli-interactive
   # Log in first with az login if you're not using Cloud Shell

   # Install/update the preview extension
   az extension add --name aks-preview

   # Validate the version of the preview extension
   az extension show --name aks-preview --query [version]
   ```

   > [!NOTE]
   > Als u eerder hebt geïnstalleerd het _aks-preview_ -extensie, moet u eerst een bijgewerkt met behulp van de `az extension update --name aks-preview` opdracht.

#### <a name="installation-steps"></a>Installatiestappen

Zodra de vereisten zijn voltooid, installeert u de invoegtoepassing Azure Policy in het AKS-cluster dat u wilt beheren.

- Azure Portal

  1. Start de AKS-service in Azure portal door te klikken op **alle services**, zoeken en te selecteren **Kubernetes-services**.

  1. Selecteer een van uw AKS-clusters.

  1. Selecteer **beleid (preview)** aan de linkerkant van de pagina van de Kubernetes-service.

     ![Beleid van het AKS-cluster](../media/rego-for-aks/policies-preview-from-aks-cluster.png)

  1. Selecteer in de hoofdpagina van de **invoegtoepassing inschakelen** knop.

     ![De Azure-beleid voor de AKS-invoegtoepassing inschakelen](../media/rego-for-aks/enable-policy-add-on.png)

     > [!NOTE]
     > Als de **invoegtoepassing inschakelen** knop is uitgeschakeld, het abonnement nog niet is toegevoegd aan de Preview-versie. Zie [Opt-in voor de Preview-versie](#opt-in-for-preview) voor de vereiste stappen.

- Azure-CLI

  ```azurecli-interactive
  # Log in first with az login if you're not using Cloud Shell

  az aks enable-addons --addons azure-policy --name MyAKSCluster --resource-group MyResourceGroup
  ```

### <a name="validation-and-reporting-frequency"></a>Validatie en rapportage van frequentie

De invoegtoepassing wordt gecontroleerd met Azure Policy voor wijzigingen in toewijzingen van beleid om de 5 minuten. Tijdens deze vernieuwingscyclus, verwijdert u de invoegtoepassing alle _configmaps_ in de _azure-beleid_ naamruimte stelt vervolgens de _configmaps_ voor gebruik van de GateKeeper.

> [!NOTE]
> Terwijl een _cluster admin_ gemachtigd is om te het _azure-beleid_ naamruimte, is het niet aanbevolen of ondersteund wijzigingen wilt aanbrengen in de naamruimte. Handmatige wijzigingen gaan verloren tijdens de vernieuwingscyclus.

Elke 5 minuten, aanroepen de invoegtoepassing voor een volledige scan van het cluster. Nadat het verzamelen van gegevens van de volledige scan en een realtime evaluaties door GateKeeper van wijzigingen aan het cluster, de invoegtoepassing rapporteert de resultaten terug naar Azure-beleid voor opname in [compatibiliteitsdetails](../how-to/get-compliance-data.md) , zoals een Azure-beleid toewijzing. Alleen resultaten voor actieve beleidstoewijzingen worden geretourneerd tijdens de controlecyclus.

## <a name="policy-language"></a>Beleid voor taal

De structuur van de Azure Policy taal voor het beheren van AKS volgt die van bestaande beleidsregels. Het effect _EnforceRegoPolicy_ wordt gebruikt voor het beheren van uw AKS-clusters en het duurt _details_ eigenschappen die specifiek zijn voor het werken met OPA en GateKeeper. Zie voor meer informatie en voorbeelden, de [EnforceRegoPolicy](effects.md#enforceregopolicy) effect.

Als onderdeel van de _details.policy_ eigenschap in de beleidsdefinitie, Azure Policy wordt de URI van een beleid rego doorgegeven aan de invoegtoepassing. Rego is de taal die OPA en GateKeeper wordt ondersteund als u wilt valideren of muteren een aanvraag voor het Kubernetes-cluster. Door een bestaande standaard ondersteuning voor het beheer van Kubernetes, maakt Azure Policy het mogelijk hergebruik bestaande regels en ze worden gekoppeld met Azure Policy een geïntegreerde cloud-naleving rapportervaring. Zie voor meer informatie, [wat is er Rego?](https://www.openpolicyagent.org/docs/how-do-i-write-policies.html#what-is-rego).

## <a name="built-in-policies"></a>Ingebouwd beleid

Als u zoekt het ingebouwde beleid voor het beheren van AKS met behulp van de Azure portal, de volgende stappen uit:

1. Start de Azure Policy-service in Azure portal. Selecteer **alle services** in het linkerdeelvenster en vervolgens zoeken naar en selecteer **beleid**.

1. Selecteer in het linkerdeelvenster van de pagina Azure Policy, **definities**.

1. Klik in de vervolgkeuzelijst met categorie gebruiken **Alles selecteren** schakelt u het filter en selecteer vervolgens **Kubernetes-service**.

1. Selecteer de beleidsdefinitie, en selecteer vervolgens de **toewijzen** knop.

> [!NOTE]
> Bij het toewijzen van het Azure-beleid voor AKS-definitie, de **bereik** de AKS-cluster-bron moet bevatten.

U kunt ook gebruik van de [een beleid toewijzen - Portal](../assign-policy-portal.md) Snelstartgids voor het zoeken en toewijzen van een AKS-beleid. Zoeken naar de definitie van een Kubernetes-beleid in plaats van het voorbeeld 'virtuele machines controleren'.

## <a name="logging"></a>Logboekregistratie

### <a name="azure-policy-add-on-logs"></a>Logboeken van de invoegtoepassing Azure Policy

Als een Kubernetes-controller/container houdt u de invoegtoepassing Azure Policy Logboeken in het AKS-cluster. De logboeken worden weergegeven in de **Insights** pagina van het AKS-cluster. Zie voor meer informatie, [begrijpen AKS-cluster prestaties met Azure Monitor voor containers](../../../azure-monitor/insights/container-insights-analyze.md).

### <a name="gatekeeper-logs"></a>GateKeeper-Logboeken

Volg de stappen in om in te schakelen GateKeeper-logboeken voor de nieuwe resourceaanvragen, [inschakelen en controleren van master knooppunt in AKS logboeken Kubernetes](../../../aks/view-master-logs.md).
Hier volgt een voorbeeld van een query om weer te geven geweigerde gebeurtenissen op de nieuwe resourceaanvragen:

```kusto
| where Category == "kube-audit"
| where log_s contains "admission webhook"
| limit 100
```

Als u logboeken van GateKeeper containers, volg de stappen in [inschakelen en controleren van master knooppunt in AKS logboeken Kubernetes](../../../aks/view-master-logs.md) en controleer de _kube-apiserver_ optie in de **diagnostische instellingen** deelvenster.

## <a name="remove-the-add-on"></a>Verwijderen van de invoegtoepassing

Als u wilt verwijderen van de invoegtoepassing Azure Policy van uw AKS-cluster, gebruikt de Azure portal of Azure CLI:

- Azure Portal

  1. Start de AKS-service in Azure portal door te klikken op **alle services**, zoeken en te selecteren **Kubernetes-services**.

  1. Selecteer uw AKS-cluster waar u de Azure Policy-invoegtoepassing wilt uitschakelen.

  1. Selecteer **beleid (preview)** aan de linkerkant van de pagina van de Kubernetes-service.

     ![Beleid van het AKS-cluster](../media/rego-for-aks/policies-preview-from-aks-cluster.png)

  1. Selecteer in de hoofdpagina van de **invoegtoepassing uit te schakelen** knop.

     ![De Azure-beleid voor de AKS-invoegtoepassing uitschakelen](../media/rego-for-aks/disable-policy-add-on.png)

- Azure-CLI

  ```azurecli-interactive
  # Log in first with az login if you're not using Cloud Shell

  az aks disable-addons --addons azure-policy --name MyAKSCluster --resource-group MyResourceGroup
  ```

## <a name="next-steps"></a>Volgende stappen

- Bekijk voorbeelden op [voorbeelden voor Azure Policy](../samples/index.md).
- Bekijk de [structuur van Azure Policy-definities](definition-structure.md).
- Lees [Informatie over de effecten van het beleid](effects.md).
- Begrijpen hoe u [programmatisch beleid maken](../how-to/programmatically-create.md).
- Meer informatie over het [ophalen compatibiliteitsgegevens](../how-to/getting-compliance-data.md).
- Meer informatie over het [herstellen van niet-compatibele resources](../how-to/remediate-resources.md).
- Lees wat een beheergroep met is [organiseren van uw resources met Azure-beheergroepen](../../management-groups/index.md).