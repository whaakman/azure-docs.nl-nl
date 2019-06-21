---
title: Migreren naar gedetailleerde, op rollen gebaseerde toegang voor configuraties van clusters - Azure HDInsight
description: Meer informatie over de wijzigingen die zijn vereist als onderdeel van de migratie naar gedetailleerde, op rollen gebaseerde toegang voor configuraties van clusters op HDInsight.
author: tylerfox
ms.author: tyfox
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 982c5dcc052f92afe381235db0bf066262fd82c6
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67304291"
---
# <a name="migrate-to-granular-role-based-access-for-cluster-configurations"></a>Migreren naar gedetailleerde, op rollen gebaseerde toegang voor clusterconfiguraties

We introduceren belangrijke wijzigingen meer fijnmazig op basis van rollen om toegang te ondersteunen om gevoelige informatie te verkrijgen. Als onderdeel van deze, sommige wijzigingen **actie is mogelijk vereist** als u een van de [betrokken entiteiten /-scenario's](#am-i-affected-by-these-changes).

## <a name="what-is-changing"></a>Wat is gewijzigd?

Voorheen geheimen kunnen worden verkregen via de HDInsight-API door cluster gebruikers beschikken de eigenaar, bijdrager of lezer [RBAC-rollen](https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles), zoals ze beschikbaar voor iedereen met zijn de `*/read` machtiging.
Voortaan kunt verkrijgen van toegang tot deze geheime gegevens moet de `Microsoft.HDInsight/clusters/configurations/*` machtiging, wat betekent dat ze niet meer kunnen worden geopend door gebruikers met de rol van lezer. Geheimen zijn gedefinieerd als de waarden die kunnen worden gebruikt voor het verkrijgen van meer uitgebreide toegang dan een gebruikersrol moeten toestaan. Deze bevatten waarden zoals cluster-gateway HTTP-referenties en opslagsleutels voor account databasereferenties.

We introduceren een nieuwe [HDInsight-Cluster Operator](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#hdinsight-cluster-operator) rol die is mogelijk om op te halen van geheimen zonder de beheerdersmachtigingen van inzender of eigenaar wordt verleend. Samenvatting:

| Rol                                  | Eerder                                                                                       | Voortaan       |
|---------------------------------------|--------------------------------------------------------------------------------------------------|-----------|
| Lezer                                | -Lezen toegang, met inbegrip van geheimen                                                                   | -Read-access, **met uitzondering van** geheimen |           |   |   |
| HDInsight-Cluster-Operator<br>(Nieuwe rol) | N/A                                                                                              | / Lezen/schrijven-toegang, met inbegrip van geheimen         |   |   |
| Inzender                           | / Lezen/schrijven-toegang, met inbegrip van geheimen<br>-Maken en beheren van alle soorten Azure-resources.     | Geen wijziging |
| Eigenaar                                 | Lees-/ schrijftoegang inclusief geheimen<br>-Volledige toegang tot alle resources<br>-Toegang aan anderen delegeren | Geen wijziging |

Voor meer informatie over de toewijzing van de rol Operator voor HDInsight-Cluster toevoegen aan een gebruiker om lees-/ schrijftoegang tot de geheimen van de cluster, raadpleegt u de onderstaande sectie [de toewijzing van de rol Operator voor HDInsight-Cluster toevoegen aan een gebruiker](#add-the-hdinsight-cluster-operator-role-assignment-to-a-user).

## <a name="am-i-affected-by-these-changes"></a>Ben ik beïnvloed door deze wijzigingen?

De volgende scenario's en entiteiten zijn hierbij betrokken:

- [API](#api): Gebruikers met behulp van de `/configurations` of `/configurations/{configurationName}` eindpunten.
- [Azure HDInsight-hulpprogramma's voor Visual Studio Code](#azure-hdinsight-tools-for-visual-studio-code) versie 1.1.1 of lager.
- [Azure Toolkit voor IntelliJ](#azure-toolkit-for-intellij) versie 3.20.0 of lager.
- [Azure Data Lake en Stream Analytics-hulpprogramma's voor Visual Studio](#azure-data-lake-and-stream-analytics-tools-for-visual-studio) lagere versie dan versie 2.3.9000.1.
- [Azure Toolkit voor Eclipse](#azure-toolkit-for-eclipse) versie 3.15.0 of lager.
- [SDK voor .NET](#sdk-for-net)
    - [versie 1.x of 2.x](#versions-1x-and-2x): Gebruikers met behulp van de `GetClusterConfigurations`, `GetConnectivitySettings`, `ConfigureHttpSettings`, `EnableHttp` of `DisableHttp` methoden van de klasse ConfigurationsOperationsExtensions.
    - [versies 3.x en maximaal](#versions-3x-and-up): Gebruikers met behulp van de `Get`, `Update`, `EnableHttp`, of `DisableHttp` methoden van de `ConfigurationsOperationsExtensions` klasse.
- [SDK voor Python](#sdk-for-python): Gebruikers met behulp van de `get` of `update` methoden van de `ConfigurationsOperations` klasse.
- [SDK voor Java](#sdk-for-java): Gebruikers met behulp van de `update` of `get` methoden van de `ConfigurationsInner` klasse.
- [SDK voor Go](#sdk-for-go): Gebruikers met behulp van de `Get` of `Update` methoden van de `ConfigurationsClient` struct.
- [Az.HDInsight PowerShell](#azhdinsight-powershell) lagere versie dan versie 2.0.0.
Zie de onderstaande secties (of gebruik de bovenstaande koppelingen) om de migratiestappen voor uw scenario te bekijken.

### <a name="api"></a>API

De volgende API's worden gewijzigd of afgeschaft:

- [**GET-/configurations/ {configurationName}** ](https://docs.microsoft.com/rest/api/hdinsight/hdinsight-cluster#get-configuration) (gevoelige gegevens verwijderd)
    - Eerder hebt gebruikt om op te halen van afzonderlijke configuratie-typen (met inbegrip van geheimen).
    - Deze API-aanroep retourneert nu afzonderlijke configuratie typen met geheimen weggelaten. Gebruik om alle configuraties, met inbegrip van geheimen, de nieuwe POST /configurations-aanroep. Gebruik de nieuwe POST /getGatewaySettings-aanroep om alleen de instellingen van de gateway.
- [**GET-/configurations** ](https://docs.microsoft.com/rest/api/hdinsight/hdinsight-cluster#get-configuration) (afgeschaft)
    - Eerder gebruikt voor het ophalen van alle configuraties (met inbegrip van geheimen)
    - Deze API-aanroep wordt niet meer ondersteund. Gebruik de nieuwe POST /configurations-aanroep om alle configuraties voortaan. Gebruik de GET-aanroep /configurations/ {configurationName} om configuraties met gevoelige parameters weggelaten.
- [**POST /configurations/ {configurationName}** ](https://docs.microsoft.com/rest/api/hdinsight/hdinsight-cluster#update-gateway-settings) (afgeschaft)
    - Eerder gebruikt voor het bijwerken van gatewayreferenties.
    - Deze API-aanroep wordt afgeschaft en wordt niet meer ondersteund. Gebruik in plaats daarvan het nieuwe bericht /updateGatewaySettings.

De vervanging van de volgende die API 's zijn toegevoegd:</span>

- [**POST /configurations**](https://docs.microsoft.com/rest/api/hdinsight/hdinsight-cluster#list-configurations)
    - Gebruik deze API om op te halen van alle configuraties, met inbegrip van geheimen.
- [**POST /getGatewaySettings**](https://docs.microsoft.com/rest/api/hdinsight/hdinsight-cluster#get-gateway-settings)
    - Gebruik deze API om op te halen van de gateway-instellingen.
- [**POST /updateGatewaySettings**](https://docs.microsoft.com/rest/api/hdinsight/hdinsight-cluster#update-gateway-settings)
    - Deze API gebruiken voor het bijwerken van de gateway-instellingen (gebruikersnaam en/of wachtwoord).

### <a name="azure-hdinsight-tools-for-visual-studio-code"></a>Azure HDInsight-hulpprogramma's voor Visual Studio Code

Als u versie 1.1.1 of hieronder, naar bijwerken de [meest recente versie van Azure HDInsight Tools voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=mshdinsight.azure-hdinsight&ssr=false) om onderbrekingen te voorkomen.

### <a name="azure-toolkit-for-intellij"></a>Azure Toolkit voor IntelliJ

Als u versie 3.20.0 of hieronder, naar bijwerken de [meest recente versie van de Azure Toolkit voor IntelliJ-invoegtoepassing](https://plugins.jetbrains.com/plugin/8053-azure-toolkit-for-intellij) om onderbrekingen te voorkomen.

### <a name="azure-data-lake-and-stream-analytics-tools-for-visual-studio"></a>Azure Data Lake en Stream Analytics-hulpprogramma's voor Visual Studio

Bijwerken naar versie 2.3.9000.1 of hoger van [Azure Data Lake en Stream Analytics-hulpprogramma's voor Visual Studio](https://marketplace.visualstudio.com/items?itemName=ADLTools.AzureDataLakeandStreamAnalyticsTools&ssr=false#overview) om onderbrekingen te voorkomen.  Zie onze documentatie voor hulp bij het bijwerken, [Update Data Lake Tools voor Visual Studio](https://docs.microsoft.com/azure/hdinsight/hadoop/apache-hadoop-visual-studio-tools-get-started#update-data-lake-tools-for-visual-studio).

### <a name="azure-toolkit-for-eclipse"></a>Azure Toolkit voor Eclipse

Als u versie 3.15.0 of hieronder, naar bijwerken de [meest recente versie van de Azure Toolkit voor Eclipse](https://marketplace.eclipse.org/content/azure-toolkit-eclipse) om onderbrekingen te voorkomen.

### <a name="sdk-for-net"></a>SDK voor .NET

#### <a name="versions-1x-and-2x"></a>Versie 1.x en 2.x

Bijwerken naar [versie 2.1.0](https://www.nuget.org/packages/Microsoft.Azure.Management.HDInsight/2.1.0) van de HDInsight-SDK voor .NET. Minimale codewijzigingen mogelijk zijn vereist als u een methode beïnvloed door deze wijzigingen:

- `ClusterOperationsExtensions.GetClusterConfigurations` wordt **retourneren niet langer gevoelige parameters** , zoals opslagsleutels (core-site) of HTTP-referenties (gateway).
    - Gebruiken om op te halen van alle configuraties, met inbegrip van gevoelige parameters `ClusterOperationsExtensions.ListConfigurations` voortaan.  Houd er rekening mee dat gebruikers met de rol 'Lezer' is niet mogelijk om deze methode te gebruiken. Hierdoor nauwkeurige controle over welke gebruikers toegang gevoelige gegevens voor een cluster tot.
    - Als u wilt alleen HTTP-gateway-referenties ophalen, gebruikt u `ClusterOperationsExtensions.GetGatewaySettings`.

- `ClusterOperationsExtensions.GetConnectivitySettings` nu is afgeschaft en vervangen door `ClusterOperationsExtensions.GetGatewaySettings`.

- `ClusterOperationsExtensions.ConfigureHttpSettings` nu is afgeschaft en vervangen door `ClusterOperationsExtensions.UpdateGatewaySettings`.

- `ConfigurationsOperationsExtensions.EnableHttp` en `DisableHttp` zijn afgeschaft. HTTP is nu altijd ingeschakeld, zodat deze methoden zijn niet meer nodig hebt.

#### <a name="versions-3x-and-up"></a>Versies 3.x en hoger

Bijwerken naar [versie 5.0.0](https://www.nuget.org/packages/Microsoft.Azure.Management.HDInsight/5.0.0) of hoger van de HDInsight SDK voor .NET. Minimale codewijzigingen mogelijk zijn vereist als u een methode beïnvloed door deze wijzigingen:

- [`ConfigurationOperationsExtensions.Get`](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.get?view=azure-dotnet) wordt **retourneren niet langer gevoelige parameters** , zoals opslagsleutels (core-site) of HTTP-referenties (gateway).
    - Gebruiken om op te halen van alle configuraties, met inbegrip van gevoelige parameters [ `ConfigurationOperationsExtensions.List` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.list?view=azure-dotnet) voortaan.  Houd er rekening mee dat gebruikers met de rol 'Lezer' is niet mogelijk om deze methode te gebruiken. Hierdoor nauwkeurige controle over welke gebruikers toegang gevoelige gegevens voor een cluster tot. 
    - Als u wilt alleen HTTP-gateway-referenties ophalen, gebruikt u [ `ClusterOperationsExtensions.GetGatewaySettings` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.clustersoperationsextensions.getgatewaysettings?view=azure-dotnet). 
- [`ConfigurationsOperationsExtensions.Update`](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.update?view=azure-dotnet) nu is afgeschaft en vervangen door [ `ClusterOperationsExtensions.UpdateGatewaySettings` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.clustersoperationsextensions.updategatewaysettings?view=azure-dotnet). 
- [`ConfigurationsOperationsExtensions.EnableHttp`](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.enablehttp?view=azure-dotnet) en [ `DisableHttp` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.disablehttp?view=azure-dotnet) zijn afgeschaft. HTTP is nu altijd ingeschakeld, zodat deze methoden zijn niet meer nodig hebt.

### <a name="sdk-for-python"></a>SDK voor Python

Bijwerken naar [versie 1.0.0](https://pypi.org/project/azure-mgmt-hdinsight/1.0.0/) of hoger van de HDInsight-SDK voor Python. Minimale codewijzigingen mogelijk zijn vereist als u een methode beïnvloed door deze wijzigingen:

- [`ConfigurationsOperations.get`](https://docs.microsoft.com/python/api/azure-mgmt-hdinsight/azure.mgmt.hdinsight.operations.configurations_operations.configurationsoperations?view=azure-python#get-resource-group-name--cluster-name--configuration-name--custom-headers-none--raw-false----operation-config-) wordt **retourneren niet langer gevoelige parameters** , zoals opslagsleutels (core-site) of HTTP-referenties (gateway).
    - Gebruiken om op te halen van alle configuraties, met inbegrip van gevoelige parameters [ `ConfigurationsOperations.list` ](https://docs.microsoft.com/python/api/azure-mgmt-hdinsight/azure.mgmt.hdinsight.operations.configurations_operations.configurationsoperations?view=azure-python#list-resource-group-name--cluster-name--custom-headers-none--raw-false----operation-config-) voortaan.  Houd er rekening mee dat gebruikers met de rol 'Lezer' is niet mogelijk om deze methode te gebruiken. Hierdoor nauwkeurige controle over welke gebruikers toegang gevoelige gegevens voor een cluster tot. 
    - Als u wilt alleen HTTP-gateway-referenties ophalen, gebruikt u [ `ConfigurationsOperations.get_gateway_settings` ](https://docs.microsoft.com/python/api/azure-mgmt-hdinsight/azure.mgmt.hdinsight.operations.clusters_operations.clustersoperations?view=azure-python#get-gateway-settings-resource-group-name--cluster-name--custom-headers-none--raw-false----operation-config-).
- [`ConfigurationsOperations.update`](https://docs.microsoft.com/python/api/azure-mgmt-hdinsight/azure.mgmt.hdinsight.operations.clusters_operations.clustersoperations?view=azure-python#update-resource-group-name--cluster-name--tags-none--custom-headers-none--raw-false----operation-config-) nu is afgeschaft en vervangen door [ `ClusterOperations.update_gateway_settings` ](https://docs.microsoft.com/python/api/azure-mgmt-hdinsight/azure.mgmt.hdinsight.operations.clusters_operations.clustersoperations?view=azure-python#update-gateway-settings-resource-group-name--cluster-name--parameters--custom-headers-none--raw-false--polling-true----operation-config-).

### <a name="sdk-for-java"></a>SDK voor Java

Bijwerken naar [versie 1.0.0](https://search.maven.org/artifact/com.microsoft.azure.hdinsight.v2018_06_01_preview/azure-mgmt-hdinsight/) of hoger van de HDInsight SDK voor Java. Minimale codewijzigingen mogelijk zijn vereist als u een methode beïnvloed door deze wijzigingen:

- [`ConfigurationsInner.get`](https://docs.microsoft.com/java/api/com.microsoft.azure.management.hdinsight.v2018__06__01__preview.implementation._configurations_inner.get) wordt **retourneren niet langer gevoelige parameters** , zoals opslagsleutels (core-site) of HTTP-referenties (gateway).
    - Gebruiken om op te halen van alle configuraties, met inbegrip van gevoelige parameters [ `ConfigurationsInner.list` ](https://docs.microsoft.com/java/api/com.microsoft.azure.management.hdinsight.v2018_06_01_preview.implementation.configurationsinner.list?view=azure-java-stable) voortaan.  Houd er rekening mee dat gebruikers met de rol 'Lezer' is niet mogelijk om deze methode te gebruiken. Hierdoor nauwkeurige controle over welke gebruikers toegang gevoelige gegevens voor een cluster tot. 
    - Als u wilt alleen HTTP-gateway-referenties ophalen, gebruikt u [ `ClustersInner.getGatewaySettings` ](https://docs.microsoft.com/java/api/com.microsoft.azure.management.hdinsight.v2018_06_01_preview.implementation.clustersinner.getgatewaysettings?view=azure-java-stable).
- [`ConfigurationsInner.update`](https://docs.microsoft.com/java/api/com.microsoft.azure.management.hdinsight.v2018__06__01__preview.implementation._configurations_inner.update) nu is afgeschaft en vervangen door [ `ClustersInner.updateGatewaySettings` ](https://docs.microsoft.com/java/api/com.microsoft.azure.management.hdinsight.v2018_06_01_preview.implementation.clustersinner.updategatewaysettings?view=azure-java-stable).

### <a name="sdk-for-go"></a>SDK voor Go

Bijwerken naar [versie 27.1.0](https://github.com/Azure/azure-sdk-for-go/tree/master/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight) of hoger van de HDInsight SDK voor Go. Minimale codewijzigingen mogelijk zijn vereist als u een methode beïnvloed door deze wijzigingen:

- [`ConfigurationsClient.get`](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight#ConfigurationsClient.Get) wordt **retourneren niet langer gevoelige parameters** , zoals opslagsleutels (core-site) of HTTP-referenties (gateway).
    - Gebruiken om op te halen van alle configuraties, met inbegrip van gevoelige parameters [ `ConfigurationsClient.list` ](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight#ConfigurationsClient.List) voortaan.  Houd er rekening mee dat gebruikers met de rol 'Lezer' is niet mogelijk om deze methode te gebruiken. Hierdoor nauwkeurige controle over welke gebruikers toegang gevoelige gegevens voor een cluster tot. 
    - Als u wilt alleen HTTP-gateway-referenties ophalen, gebruikt u [ `ClustersClient.get_gateway_settings` ](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight#ClustersClient.GetGatewaySettings).
- [`ConfigurationsClient.update`](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight#ConfigurationsClient.Update) nu is afgeschaft en vervangen door [ `ClustersClient.update_gateway_settings` ](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight#ClustersClient.UpdateGatewaySettings).

### <a name="azhdinsight-powershell"></a>Az.HDInsight PowerShell
Bijwerken naar [Az PowerShell versie 2.0.0](https://www.powershellgallery.com/packages/Az) of later om te voorkomen dat onderbrekingen.  Minimale codewijzigingen mogelijk zijn vereist als u een methode beïnvloed door deze wijzigingen.
- `Grant-AzHDInsightHttpServicesAccess` nu is afgeschaft en vervangen door de nieuwe `Set-AzHDInsightGatewayCredential` cmdlet.
- `Get-AzHDInsightJobOutput` ter ondersteuning van gedetailleerde, op rollen gebaseerde toegang tot de opslagsleutel is bijgewerkt.
    - Gebruikers met de HDInsight Cluster-rollen Operator, Bijdrager of Eigenaar worden niet beïnvloed.
    - Gebruikers met alleen de rol van lezer moet opgeven de `DefaultStorageAccountKey` parameter expliciet.
- `Revoke-AzHDInsightHttpServicesAccess` nu is afgeschaft. HTTP is nu altijd ingeschakeld, zodat deze cmdlet is niet meer nodig.
 Zie de [az. HDInsight-Migratiehandleiding](https://github.com/Azure/azure-powershell/blob/master/documentation/migration-guides/Az.2.0.0-migration-guide.md#azhdinsight) voor meer informatie.

## <a name="add-the-hdinsight-cluster-operator-role-assignment-to-a-user"></a>De toewijzing van de rol Operator voor HDInsight-Cluster toevoegen aan een gebruiker

Een gebruiker met de [Inzender](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#contributor) of [eigenaar](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner) rol kunt toewijzen de [HDInsight-Cluster Operator](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#hdinsight-cluster-operator) rol aan gebruikers die u wilt lezen/schrijven toegang hebben tot gevoelige gegevens HDInsight-cluster-configuratiewaarden (zoals cluster gatewayreferenties en opslagsleutels voor account).

### <a name="using-the-azure-cli"></a>Azure CLI gebruiken

De eenvoudigste manier om toe te voegen deze roltoewijzing is met behulp van de `az role assignemnt create` opdracht in de Azure CLI.

> [!NOTE]
> Met deze opdracht moet worden uitgevoerd door een gebruiker met de rollen Inzender of eigenaar, zoals alleen ze deze machtigingen kunnen verlenen. De `--assignee` is het e-mailadres van de gebruiker aan wie u wilt toewijzen van de rol van Operator van HDInsight-Cluster.

#### <a name="grant-role-at-the-resource-cluster-level"></a>Verlenen van de rol op het resourceniveau van de (cluster)

```azurecli-interactive
az role assignment create --role "HDInsight Cluster Operator" --assignee <user@domain.com> --scope /subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroupName>/providers/Microsoft.HDInsight/clusters/<ClusterName>
```

#### <a name="grant-role-at-the-resource-group-level"></a>Rol op het niveau van de resource te verlenen

```azurecli-interactive
az role assignment create --role "HDInsight Cluster Operator" --assignee user@domain.com -g <ResourceGroupName>
```

#### <a name="grant-role-at-the-subscription-level"></a>Rol op het abonnementsniveau van het verlenen

```azurecli-interactive
az role assignment create --role "HDInsight Cluster Operator" --assignee user@domain.com
```

### <a name="using-the-azure-portal"></a>Azure Portal gebruiken

U kunt ook de Azure-portal de toewijzing van de rol Operator voor HDInsight-Cluster toevoegen aan een gebruiker. Zie de documentatie, [beheren van toegang tot Azure-resources met behulp van RBAC en de Azure-portal: een roltoewijzing toevoegen](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal#add-a-role-assignment).
