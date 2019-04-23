---
title: Migreren naar gedetailleerde, op rollen gebaseerde toegang voor configuraties van clusters - Azure HDInsight
description: Meer informatie over de wijzigingen die nodig zijn om te migreren naar gedetailleerde, op rollen gebaseerde toegang voor configuraties van clusters.
author: tylerfox
ms.author: tyfox
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/19/2019
ms.openlocfilehash: 0422d848ccdf9ba82e68813de64eec863ee4ad29
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/22/2019
ms.locfileid: "60006230"
---
# <a name="migrate-to-granular-role-based-access-for-cluster-configurations"></a>Migreren naar gedetailleerde, op rollen gebaseerde toegang voor configuraties van clusters

We introduceren belangrijke wijzigingen meer fijnmazig op basis van rollen om toegang te ondersteunen om gevoelige informatie te verkrijgen. Als onderdeel van deze, sommige wijzigingen **actie is mogelijk vereist** als u een van de [betrokken entiteiten /-scenario's](#am-i-affected-by-these-changes).

## <a name="what-is-changing"></a>Wat is gewijzigd?

Voorheen geheimen kunnen worden verkregen via de HDInsight-API door cluster gebruikers beschikken de eigenaar, bijdrager of lezer [RBAC-rollen](https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles).
Voortaan kunt langer deze geheime gegevens niet toegankelijk voor gebruikers met de rol van lezer. We zijn ook een nieuwe 'Hdinsight-Cluster Operator'-rol die is mogelijk om op te halen van geheimen zonder de beheerdersmachtigingen van inzender of eigenaar wordt verleend geïntroduceerd. Samenvatting:

| Rol                                  | Eerder                                                                                       | Nu       |
|---------------------------------------|--------------------------------------------------------------------------------------------------|-----------|
| Lezer                                | -Lezen toegang, met inbegrip van geheimen                                                                   | -Read-access, **met uitzondering van** geheimen |           |   |   |
| HDInsight-Cluster-Operator<br>(Nieuwe rol) | N/A                                                                                              | / Lezen/schrijven-toegang, met inbegrip van geheimen         |   |   |
| Inzender                           | / Lezen/schrijven-toegang, met inbegrip van geheimen<br>-Maken en beheren van alle soorten Azure-resources.     | Geen wijziging |
| Eigenaar                                 | Lees-/ schrijftoegang inclusief geheimen<br>-Volledige toegang tot alle resources<br>-Toegang aan anderen delegeren | Geen wijziging |

Voor meer informatie over de toewijzing van de rol Operator voor HDInsight-Cluster toevoegen aan een gebruiker om lees-/ schrijftoegang tot de geheimen van de cluster, raadpleegt u de onderstaande sectie [de toewijzing van de rol Operator voor HDInsight-Cluster toevoegen aan een gebruiker](#add-the-hdinsight-cluster-operator-role-assignment-to-a-user).

## <a name="am-i-affected-by-these-changes"></a>Ben ik beïnvloed door deze wijzigingen?

De volgende scenario's en entiteiten zijn hierbij betrokken:

- [API](#api): Gebruikers met behulp van de `/configurations` of `/configurations/{configurationName}` eindpunten.
- [Azure HDInsight-hulpprogramma's voor Visual Studio Code](#azure-hdinsight-tools-for-visual-studio-code) versie ___ en lager.
- [Azure Toolkit voor IntelliJ](#azure-toolkit-for-intellij) versie __ en lager.
- [Azure Toolkit voor Eclipse](#azure-toolkit-for-eclipse) versie __ en lager.
- [SDK voor .NET](#sdk-for-net)
    - [versie 1.x of 2.x](#versions-1x-and-2x): Gebruikers met behulp van de `GetClusterConfigurations`, `GetConnectivitySettings`, `ConfigureHttpSettings`, `EnableHttp` of `DisableHttp` methoden van de klasse ConfigurationsOperationsExtensions.
    - [versies 3.x en maximaal](#versions-3x-and-up): Gebruikers met behulp van de `EnableHttp`, `DisableHttp`, `Update`, of `Get` methoden van de `ConfigurationsOperationsExtensions` klasse.
- [SDK voor Python](#sdk-for-python): Gebruikers met behulp van de `get` of `update` methoden van de klasse ConfigurationsOperations.
- [SDK voor Java](#sdk-for-java): Gebruikers met behulp van de `update` of `get` methoden van de klasse ConfigurationsInner.
- [SDK voor Go](#sdk-for-go): Gebruikers met behulp van de `Get` of `Update` methoden van de struct ConfigurationsClient.

Zie de onderstaande secties (of gebruik de bovenstaande koppelingen) om te zien van de migratie van de stappen voor uw scenario.

### <a name="api"></a>API

De volgende API's worden gewijzigd of afgeschaft:

- [**GET-/configurations/ {configurationName}** ](https://docs.microsoft.com/rest/api/hdinsight/hdinsight-cluster#get-configuration) (gevoelige gegevens verwijderd) https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/configurations/{configurationName}?api-version={api-version}
    - Eerder hebt gebruikt om op te halen van afzonderlijke configuratie-typen (met inbegrip van geheimen).
    - Deze API-aanroep retourneert nu afzonderlijke configuratie typen met geheimen weggelaten. Om op te halen van alle configuraties, met inbegrip van geheimen, gebruikt u de nieuwe [POST /configurations]() aanroepen. Als u alleen de instellingen van de gateway, gebruikt u de nieuwe [POST /getGatewaySettings]() aanroepen.
- [**GET-/configurations** ](https://docs.microsoft.com/rest/api/hdinsight/hdinsight-cluster#get-configurations) (afgeschaft) https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/configurations?api-version={api-version}
    - Eerder gebruikt voor het ophalen van alle configuraties (met inbegrip van geheimen)
    - Deze API-aanroep wordt niet meer ondersteund. Voor alle configuraties voortaan gebruikmaken van de nieuwe [POST /configurations]() aanroepen. Voor configuraties met gevoelige parameters weggelaten verkrijgen, gebruikt u de [GET /configurations/ {configurationName}]() aanroepen.
- [**POST /configurations/ {configurationName}** ](https://docs.microsoft.com/rest/api/hdinsight/hdinsight-cluster#change-connectivity-settings) (afgeschaft) https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/configurations/{configurationName}?api-version={api-version}
    - Eerder gebruikt voor het bijwerken van gatewayreferenties.
    - Deze API-aanroep wordt afgeschaft en wordt niet meer ondersteund. Gebruik de nieuwe [POST /updateGatewaySettings]() in plaats daarvan.

De vervanging van de volgende die API 's zijn toegevoegd:</span>

- **POST /configurations** https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/configurations?api-version={api-version}
    - Gebruik deze API om op te halen van alle configuraties, met inbegrip van geheimen.
- **POST /getGatewaySettings** https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/getGatewaySettings?api-version={api-version}
    - Gebruik deze API om op te halen van de gateway-instellingen.
- **POST /updateGatewaySettings** https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/updateGatewaySettings?api-version={api-version}
    - Deze API gebruiken voor het bijwerken van de gateway-instellingen (gebruikersnaam en/of wachtwoord).

### <a name="azure-hdinsight-tools-for-visual-studio-code"></a>Azure HDInsight-hulpprogramma's voor Visual Studio Code

Als u versie 1.1.1 of ouder, werk bij naar de [meest recente versie van Azure HDInsight Tools voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=mshdinsight.azure-hdinsight&ssr=false) om onderbrekingen te voorkomen.

### <a name="azure-toolkit-for-intellij"></a>Azure Toolkit voor IntelliJ

Als u versie 3.21.0 of ouder bent, werkt u aan de [meest recente versie van de Azure Toolkit voor IntelliJ-invoegtoepassing](https://plugins.jetbrains.com/plugin/8053-azure-toolkit-for-intellij) om onderbrekingen te voorkomen.

### <a name="azure-toolkit-for-eclipse"></a>Azure Toolkit voor Eclipse

Als u de release 2019-03-29 of ouder bent, werkt u naar de nieuwste versie van de Azure Toolkit voor Eclipse om onderbrekingen te voorkomen.

### <a name="sdk-for-net"></a>SDK voor .NET

#### <a name="versions-1x-and-2x"></a>Versie 1.x en 2.x

Voer een update naar [versie 2.1.0](https://www.nuget.org/packages/Microsoft.Azure.Management.HDInsight/2.1.0) van de HDInsight-SDK voor .NET. Minimale codewijzigingen mogelijk zijn vereist als u een methode beïnvloed door deze wijzigingen:

- `ClusterOperationsExtensions.GetClusterConfigurations` wordt **retourneren niet langer gevoelige parameters** , zoals opslagsleutels (core-site) of HTTP-referenties (gateway).
    - Gebruiken om op te halen van alle configuraties, met inbegrip van gevoelige parameters `ClusterOperationsExtensions.ListConfigurations` voortaan.  Houd er rekening mee dat gebruikers met de rol 'Lezer' is niet mogelijk om deze methode te gebruiken. Hierdoor nauwkeurige controle over welke gebruikers toegang gevoelige gegevens voor een cluster tot.
    - Als u wilt alleen HTTP-gateway-referenties ophalen, gebruikt u `ClusterOperationsExtensions.GetGatewaySettings`.

- `ClusterOperationsExtensions.GetConnectivitySettings` nu is afgeschaft en vervangen door `ClusterOperationsExtensions.GetGatewaySettings`.

- `ClusterOperationsExtensions.ConfigureHttpSettings` nu is afgeschaft en vervangen door `ClusterOperationsExtensions.UpdateGatewaySettings`.

- `ConfigurationsOperationsExtensions.EnableHttp` en `DisableHttp` zijn afgeschaft. HTTP is nu altijd ingeschakeld, zodat deze methoden zijn niet meer nodig hebt.

#### <a name="versions-3x-and-up"></a>Versies 3.x en hoger

Voer een update naar [versie 5.0.0](https://www.nuget.org/packages/Microsoft.Azure.Management.HDInsight/5.0.0) van de HDInsight-SDK voor .NET. Minimale codewijzigingen mogelijk zijn vereist als u een methode beïnvloed door deze wijzigingen:

- [`ConfigurationOperationsExtensions.Get`](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.get?view=azure-dotnet) wordt **retourneren niet langer gevoelige parameters** , zoals opslagsleutels (core-site) of HTTP-referenties (gateway).
    - Gebruiken om op te halen van alle configuraties, met inbegrip van gevoelige parameters `ConfigurationOperationsExtensions.List` voortaan.  Houd er rekening mee dat gebruikers met de rol 'Lezer' is niet mogelijk om deze methode te gebruiken. Hierdoor nauwkeurige controle over welke gebruikers toegang gevoelige gegevens voor een cluster tot. 
    - Als u wilt alleen HTTP-gateway-referenties ophalen, gebruikt u `ClusterOperationsExtensions.GetGatewaySettings`. 
- [`ConfigurationsOperationsExtensions.Update`](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.update?view=azure-dotnet) nu is afgeschaft en vervangen door `ClusterOperationsExtensions.UpdateGatewaySettings`. 
- [`ConfigurationsOperationsExtensions.EnableHttp`](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.enablehttp?view=azure-dotnet) en [ `DisableHttp` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.disablehttp?view=azure-dotnet) zijn afgeschaft. HTTP is nu altijd ingeschakeld, zodat deze methoden zijn niet meer nodig hebt.

### <a name="sdk-for-python"></a>SDK voor Python

Voer een update naar [versie 1.0.0](https://pypi.org/project/azure-mgmt-hdinsight/1.0.0/) van de HDInsight-SDK voor Python. Minimale codewijzigingen mogelijk zijn vereist als u een methode beïnvloed door deze wijzigingen:

- [`ConfigurationsOperations.get`](https://docs.microsoft.com/python/api/azure-mgmt-hdinsight/azure.mgmt.hdinsight.operations.configurations_operations.configurationsoperations?view=azure-python#get-resource-group-name--cluster-name--configuration-name--custom-headers-none--raw-false----operation-config-) wordt **retourneren niet langer gevoelige parameters** , zoals opslagsleutels (core-site) of HTTP-referenties (gateway).
    - Gebruiken om op te halen van alle configuraties, met inbegrip van gevoelige parameters [ `ConfigurationsOperations.list` ](https://docs.microsoft.com/python/api/azure-mgmt-hdinsight/azure.mgmt.hdinsight.operations.configurations_operations.configurationsoperations?view=azure-python#list-resource-group-name--cluster-name--custom-headers-none--raw-false----operation-config-) voortaan.  Houd er rekening mee dat gebruikers met de rol 'Lezer' is niet mogelijk om deze methode te gebruiken. Hierdoor nauwkeurige controle over welke gebruikers toegang gevoelige gegevens voor een cluster tot. 
    - Als u wilt alleen HTTP-gateway-referenties ophalen, gebruikt u [ `ConfigurationsOperations.get_gateway_settings` ](https://docs.microsoft.com/python/api/azure-mgmt-hdinsight/azure.mgmt.hdinsight.operations.clusters_operations.clustersoperations?view=azure-python#get-gateway-settings-resource-group-name--cluster-name--custom-headers-none--raw-false----operation-config-).
- [`ConfigurationsOperations.update`](https://docs.microsoft.com/python/api/azure-mgmt-hdinsight/azure.mgmt.hdinsight.operations.clusters_operations.clustersoperations?view=azure-python#update-resource-group-name--cluster-name--tags-none--custom-headers-none--raw-false----operation-config-) nu is afgeschaft en vervangen door [ `ClusterOperationsExtensions.update_gateway_settings` ](https://docs.microsoft.com/python/api/azure-mgmt-hdinsight/azure.mgmt.hdinsight.operations.clusters_operations.clustersoperations?view=azure-python#update-gateway-settings-resource-group-name--cluster-name--parameters--custom-headers-none--raw-false--polling-true----operation-config-).

### <a name="sdk-for-java"></a>SDK voor Java

Voer een update naar [versie 1.0.0](https://search.maven.org/artifact/com.microsoft.azure.hdinsight.v2018_06_01_preview/azure-mgmt-hdinsight/) van de HDInsight-SDK voor Java. Minimale codewijzigingen mogelijk zijn vereist als u een methode beïnvloed door deze wijzigingen:

- [`ConfigurationsInner.get`](https://docs.microsoft.com/java/api/com.microsoft.azure.management.hdinsight.v2018__06__01__preview.implementation._configurations_inner.get) wordt **retourneren niet langer gevoelige parameters** , zoals opslagsleutels (core-site) of HTTP-referenties (gateway).
    - Gebruiken om op te halen van alle configuraties, met inbegrip van gevoelige parameters `ConfigurationsInner.list` voortaan.  Houd er rekening mee dat gebruikers met de rol 'Lezer' is niet mogelijk om deze methode te gebruiken. Hierdoor nauwkeurige controle over welke gebruikers toegang gevoelige gegevens voor een cluster tot. 
    - Als u wilt alleen HTTP-gateway-referenties ophalen, gebruikt u `ConfigurationsOperations.get_gateway_settings`.
- [`ConfigurationsInner.update`](https://docs.microsoft.com/java/api/com.microsoft.azure.management.hdinsight.v2018__06__01__preview.implementation._configurations_inner.update) nu is afgeschaft en vervangen door `ClusterOperationsExtensions.update_gateway_settings`.

### <a name="sdk-for-go"></a>SDK voor Go

Voer een update naar [versie 27.1.0](https://github.com/Azure/azure-sdk-for-go/tree/master/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight) van de HDInsight-SDK voor Go. Minimale codewijzigingen mogelijk zijn vereist als u een methode beïnvloed door deze wijzigingen:

- [`ConfigurationsClient.get`](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight#ConfigurationsClient.Get) wordt **retourneren niet langer gevoelige parameters** , zoals opslagsleutels (core-site) of HTTP-referenties (gateway).
    - Gebruiken om op te halen van alle configuraties, met inbegrip van gevoelige parameters [ `ConfigurationsClient.list` ](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight#ConfigurationsClient.List) voortaan.  Houd er rekening mee dat gebruikers met de rol 'Lezer' is niet mogelijk om deze methode te gebruiken. Hierdoor nauwkeurige controle over welke gebruikers toegang gevoelige gegevens voor een cluster tot. 
    - Als u wilt alleen HTTP-gateway-referenties ophalen, gebruikt u [ `ClustersClient.get_gateway_settings` ](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight#ClustersClient.GetGatewaySettings).
- [`ConfigurationsClient.update`](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight#ConfigurationsClient.Update) nu is afgeschaft en vervangen door [ `ClustersClient.update_gateway_settings` ](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight#ClustersClient.UpdateGatewaySettings).

## <a name="add-the-hdinsight-cluster-operator-role-assignment-to-a-user"></a>De toewijzing van de rol Operator voor HDInsight-Cluster toevoegen aan een gebruiker

Een gebruiker met de [Inzender](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#contributor) of [eigenaar](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner) rol kunt verlenen van de rol Operator voor HDInsight-Cluster voor gebruikers die u lezen/schrijven toegang hebben tot HDInsight-cluster geheimen wilt, zoals de gatewayreferenties-cluster en opslagaccountsleutels.

### <a name="using-the-azure-cli"></a>Azure CLI gebruiken

Er is de eenvoudigste manier om deze roltoewijzing toevoegen met behulp van de volgende opdracht in de Azure CLI:

```azurecli-interactive
az role assignment create --role "HDInsight Cluster Operator" --assignee user@domain.com
```

> [!NOTE]
> Met deze opdracht moet worden uitgevoerd door een gebruiker met de rollen Inzender of eigenaar, zoals alleen ze deze machtigingen kunnen verlenen. De `--assignee` is het e-mailadres van de gebruiker aan wie u wilt toewijzen van de rol van Operator van HDInsight-Cluster.

De bovenstaande opdracht verleent deze rol op het abonnementsniveau. Als u wilt alleen verlenen in plaats daarvan deze rol op het niveau van de resourcegroep, kunt u de opdracht als volgt te werk:

```azurecli-interactive
az role assignment create --role "HDInsight Cluster Operator" --assignee user@domain.com -g <ResourceGroupName>
```

### <a name="using-the-azure-portal"></a>Azure Portal gebruiken

U kunt ook de Azure-portal de toewijzing van de rol Operator voor HDInsight-Cluster toevoegen aan een gebruiker. Zie de documentatie, [beheren van toegang tot Azure-resources met behulp van RBAC en de Azure-portal: een roltoewijzing toevoegen](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal#add-a-role-assignment).
