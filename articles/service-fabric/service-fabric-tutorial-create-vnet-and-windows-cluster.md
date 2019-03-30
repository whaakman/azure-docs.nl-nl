---
title: Een Service Fabric-cluster waarop Windows wordt uitgevoerd in Azure maken | Microsoft Docs
description: In deze zelfstudie leert u hoe u een Windows Service Fabric-cluster implementeren in een virtueel Azure-netwerk en een netwerkbeveiligingsgroep met behulp van PowerShell.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/13/2019
ms.author: aljo
ms.custom: mvc
ms.openlocfilehash: 5ef143fe2021a9f705bf61b579e8251b2946b042
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58668089"
---
# <a name="tutorial-deploy-a-service-fabric-cluster-running-windows-into-an-azure-virtual-network"></a>Zelfstudie: Een Service Fabric-cluster waarop Windows wordt uitgevoerd in een Azure-netwerk implementeren

Deze zelfstudie is deel één van een serie. U informatie over het implementeren van een Azure Service Fabric-cluster met Windows in een [virtueel Azure-netwerk](../virtual-network/virtual-networks-overview.md) en [netwerkbeveiligingsgroep](../virtual-network/virtual-networks-nsg.md) met behulp van PowerShell en een sjabloon. Wanneer u klaar bent, hebt u een cluster wordt uitgevoerd in de cloud waarmee u toepassingen kunt implementeren. Zie voor het maken van een Linux-cluster dat gebruik maakt van de Azure CLI, [een beveiligd Linux-cluster maken in Azure](service-fabric-tutorial-create-vnet-and-linux-cluster.md).

In deze zelfstudie wordt een productiescenario beschreven. Als u wilt maken van een kleiner cluster aangemaakt voor testdoeleinden kunt u vinden [een testcluster maken](./scripts/service-fabric-powershell-create-secure-cluster-cert.md).

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een VNET maken in Azure met behulp van PowerShell
> * Een sleutelkluis maken en een certificaat uploaden
> * Azure Active Directory-verificatie instellen
> * Verzamelen van diagnostische gegevens configureren
> * De EventStore-service instellen
> * Instellen van Azure Monitor-Logboeken
> * Een beveiligd Service Fabric-cluster maken in Azure PowerShell
> * Het cluster beveiligen met een X.509-certificaat
> * Verbinding maken met het cluster met behulp van PowerShell
> * Een cluster verwijderen

In deze zelfstudiereeks leert u het volgende:
> [!div class="checklist"]
> * Een beveiligd cluster maken in Azure
> * [Een cluster bewaken](service-fabric-tutorial-monitor-cluster.md)
> * [Een cluster in- of uitschalen](service-fabric-tutorial-scale-cluster.md)
> * [De runtime van een cluster upgraden](service-fabric-tutorial-upgrade-cluster.md)
> * [Een cluster verwijderen](service-fabric-tutorial-delete-cluster.md)

## <a name="prerequisites"></a>Vereisten

Voor u met deze zelfstudie begint:

* Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan.
* Installeer de [Service Fabric SDK en PowerShell-module](service-fabric-get-started.md).
* Installeer de [Azure Powershell, moduleversie 4.1 of hoger](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps).
* Bekijk de belangrijkste concepten van [Azure clusters](service-fabric-azure-clusters-overview.md).
* [Plan en bereid](service-fabric-cluster-azure-deployment-preparation.md) voor de implementatie van een productie-cluster.

Met de volgende procedures wordt er een Service Fabric-cluster met zeven knooppunten gemaakt. Gebruik de [Azure-Prijscalculator](https://azure.microsoft.com/pricing/calculator/) voor het berekenen van de kosten die worden gemaakt door het uitvoeren van een Service Fabric-cluster in Azure.

## <a name="download-and-explore-the-template"></a>De sjabloon downloaden en verkennen

De volgende sjabloonbestanden van de Azure Resource Manager-downloaden:

* [azuredeploy.json][template]
* [azuredeploy.parameters.json][parameters]

Met deze sjabloon wordt een veilig cluster van zeven virtuele machines en drie knooppunttypen in een virtueel netwerk en een netwerkbeveiligingsgroep geïmplementeerd.  Andere voorbeeldsjablonen zijn te vinden op [GitHub](https://github.com/Azure-Samples/service-fabric-cluster-templates). De [azuredeploy.json] [ template] implementeert een aantal resources, waaronder de volgende.

### <a name="service-fabric-cluster"></a>Service Fabric-cluster

In de resource **Microsoft.ServiceFabric/clusters** wordt een Windows-cluster geconfigureerd met de volgende kenmerken:

* Drie typen.
* Vijf knooppunten in het primaire knooppunttype (configureerbaar in de Sjabloonparameters) en één knooppunt in elk van de andere twee knooppunten te typen.
* Besturingssysteem: Windows Server 2016 Datacenter met Containers (configureerbaar in de Sjabloonparameters).
* (Configureerbaar in de Sjabloonparameters) beveiligd met een certificaat.
* [Omgekeerde proxy](service-fabric-reverseproxy.md) is ingeschakeld.
* [DNS-service](service-fabric-dnsservice.md) is ingeschakeld.
* [Duurzaamheidsniveau](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) Brons (configureerbaar in de Sjabloonparameters).
* [Betrouwbaarheidsniveau](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) zilver (configureerbaar in de Sjabloonparameters).
* eindpunt van de clientverbinding: 19000 (configureerbaar in de Sjabloonparameters).
* het eindpunt van de HTTP-gateway: 19080 (configureerbaar in de Sjabloonparameters).

### <a name="azure-load-balancer"></a>Azure Load Balancer

In de **Microsoft.Network/loadBalancers** resource, een load balancer is geconfigureerd. Tests en regels zijn ingesteld voor de volgende poorten:

* eindpunt van de clientverbinding: 19000
* het eindpunt van de HTTP-gateway: 19080
* toepassingspoort: 80
* toepassingspoort: 443
* omgekeerde proxy van Service Fabric: 19081

Als er andere toepassingspoorten nodig zijn, moet u om aan te passen de **Microsoft.Network/loadBalancers** resource en de **Microsoft.Network/networkSecurityGroups** resource die u wilt het verkeer is toegestaan.

### <a name="virtual-network-subnet-and-network-security-group"></a>Virtueel netwerk, subnet en netwerkbeveiligingsgroep

De namen van het virtuele netwerk, het subnet en de netwerkbeveiligingsgroep zijn gedefinieerd in de sjabloonparameters. Adresruimten van het virtuele netwerk en subnet worden ook gedeclareerd in de sjabloonparameters en geconfigureerd in de resource **Microsoft.Network/virtualNetworks**:

* Virtuele netwerkadresruimte: 172.16.0.0/20
* Service Fabric-subnetadresruimte: 172.16.2.0/23

De volgende regels voor binnenkomend verkeer worden ingeschakeld in de resource **Microsoft.Network/networkSecurityGroups**. U kunt de poortwaarden wijzigen door de sjabloonvariabelen te wijzigen.

* ClientConnectionEndpoint (TCP): 19000
* HttpGatewayEndpoint (HTTP/TCP): 19080
* SMB: 445
* Internodecommunication: 1025, 1026, 1027
* Bereik kortstondige poorten: 49152 tot 65534 (minimaal 256 poorten).
* Poorten voor toepassingsgebruik: 80 en 443
* Poortbereik voor toepassingen: 49152 tot 65534 (gebruikt voor communicatie. Andere poorten niet worden geopend op de Load balancer).
* Blokkeer alle andere poorten

Als er andere toepassingspoorten nodig zijn, moet u om aan te passen de **Microsoft.Network/loadBalancers** resource en de **Microsoft.Network/networkSecurityGroups** resource die u wilt het verkeer is toegestaan.

### <a name="windows-defender"></a>Windows Defender
Standaard de [Windows Defender antivirus programma](/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016) is geïnstalleerd en functioneert op Windows Server 2016. De gebruikersinterface wordt standaard geïnstalleerd op een aantal SKU's, maar is niet vereist. Voor elk knooppunttype dat/elke VM-schaalset die is aangegeven in de sjabloon, wordt de [Antimalware voor Azure-VM-extensie](/azure/virtual-machines/extensions/iaas-antimalware-windows) gebruikt voor het uitsluiten van de Service Fabric-mappen en processen:

```json
{
"name": "[concat('VMIaaSAntimalware','_vmNodeType0Name')]",
"properties": {
        "publisher": "Microsoft.Azure.Security",
        "type": "IaaSAntimalware",
        "typeHandlerVersion": "1.5",
        "settings": {
        "AntimalwareEnabled": "true",
        "Exclusions": {
                "Paths": "D:\\SvcFab;D:\\SvcFab\\Log;C:\\Program Files\\Microsoft Service Fabric",
                "Processes": "Fabric.exe;FabricHost.exe;FabricInstallerService.exe;FabricSetup.exe;FabricDeployer.exe;ImageBuilder.exe;FabricGateway.exe;FabricDCA.exe;FabricFAS.exe;FabricUOS.exe;FabricRM.exe;FileStoreService.exe"
        },
        "RealtimeProtectionEnabled": "true",
        "ScheduledScanSettings": {
                "isEnabled": "true",
                "scanType": "Quick",
                "day": "7",
                "time": "120"
        }
        },
        "protectedSettings": null
}
}
```

## <a name="set-template-parameters"></a>De sjabloonparameters instellen

Het parameterbestand [azuredeploy.parameters.json][parameters] bepaalt veel waarden die worden gebruikt om het cluster en de bijbehorende resources te implementeren. Hier volgen de parameters voor uw implementatie te wijzigen:

**Parameter** | **Voorbeeldwaarde** | **Opmerkingen** 
|---|---|---|
|adminUserName|vmadmin| De gebruikersnaam van de beheerder van de cluster-VM's. [Vereisten voor virtuele machine gebruikersnaam](https://docs.microsoft.com/azure/virtual-machines/windows/faq#what-are-the-username-requirements-when-creating-a-vm). |
|adminPassword|Password#1234| Het wachtwoord van de beheerder van de cluster-VM's. [Vereisten voor wachtwoorden voor de virtuele machine](https://docs.microsoft.com/azure/virtual-machines/windows/faq#what-are-the-password-requirements-when-creating-a-vm).|
|clusterName|mysfcluster123| De naam van het cluster. Mag alleen letters en cijfers bevatten. De lengte kan minimaal 3 en maximaal 23 tekens zijn.|
|location|southcentralus| De locatie van het cluster. |
|certificateThumbprint|| <p>De waarde moet leeg zijn als u een zelfondertekend certificaat maakt of als u een certificaatbestand opgeeft.</p><p>Als u een bestaand certificaat wilt gebruiken dat u eerder hebt geüpload naar een sleutelkluis, vult u de SHA1-waarde van de certificaatvingerafdruk in. Bijvoorbeeld 6190390162C988701DB5676EB81083EA608DCCF3.</p> |
|certificateUrlValue|| <p>De waarde moet leeg zijn als u een zelfondertekend certificaat maakt of als u een certificaatbestand opgeeft. </p><p>Als u een bestaand certificaat wilt gebruiken dat u eerder hebt geüpload naar een sleutelkluis, vult u de URL van het certificaat in. Bijvoorbeeld, ' https:\//mykeyvault.vault.azure.net:443/secrets/mycertificate/02bea722c9ef4009a76c5052bcbf8346 '.</p>|
|sourceVaultValue||<p>De waarde moet leeg zijn als u een zelfondertekend certificaat maakt of als u een certificaatbestand opgeeft.</p><p>Als u een bestaand certificaat wilt gebruiken dat u eerder hebt geüpload naar een sleutelkluis, vult u de waarde van de bronkluis in. Bijvoorbeeld /subscriptions/333cc2c84-12fa-5778-bd71-c71c07bf873f/resourceGroups/MyTestRG/providers/Microsoft.KeyVault/vaults/MYKEYVAULT.</p>|

## <a name="set-up-azure-active-directory-client-authentication"></a>Clientverificatie via Azure Active Directory instellen
Voor Service Fabric-clusters die zijn geïmplementeerd in een openbaar netwerk dat wordt gehost op Azure, is de aanbeveling voor wederzijdse verificatie van client-naar-knooppunt:
* Gebruik Azure Active Directory voor de identiteit van de client.
* Een certificaat voor de identiteit van server- en SSL-codering van HTTP-communicatie gebruiken.

Instellen van Azure Active Directory (Azure AD) voor het verifiëren van clients voor een Service Fabric-cluster moet worden uitgevoerd voordat u [het maken van het cluster](#createvaultandcert). Azure Active Directory maakt het beheren van toegang tot toepassingen door organisaties (bekend als tenants) mogelijk. 

Een Service Fabric-cluster biedt verschillende toegangspunten bij de management-functionaliteit, met inbegrip van de webconsoles [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) en [Visual Studio](service-fabric-manage-application-in-visual-studio.md). Als gevolg hiervan, maakt u twee Azure Active Directory-toepassingen voor het beheren van toegang tot het cluster: een webtoepassing en een systeemeigen toepassing.  Nadat de toepassingen zijn gemaakt, wijst u gebruikers toe aan de rollen alleen-lezen en beheerder.

> [!NOTE]
> U moet de volgende stappen uitvoeren voordat u het cluster maakt. Omdat de scripts clusternamen en eindpunten verwachten, moeten de waarden worden gepland en geen waarden zijn die u al hebt gemaakt.

In dit artikel nemen we aan dat u al een tenant hebt gemaakt. Als u nog niet hebt, start u door te lezen [hoe u een Azure Active Directory-tenant verkrijgen](../active-directory/develop/quickstart-create-new-tenant.md).

We hebben een set Windows PowerShell-scripts gemaakt ter vereenvoudiging van de stappen voor het configureren van Azure AD met een Service Fabric-cluster. [Download de scripts](https://github.com/robotechredmond/Azure-PowerShell-Snippets/tree/master/MicrosoftAzureServiceFabric-AADHelpers/AADTool) op uw computer.

### <a name="create-azure-ad-applications-and-assign-users-to-roles"></a>Azure Active Directory-toepassingen maken en gebruikers toewijzen aan rollen
Maak twee Azure Active Directory-toepassingen voor het beheren van toegang tot het cluster: een webtoepassing en een systeemeigen toepassing. Nadat u de toepassingen voor uw cluster hebt gemaakt, uw gebruikers toewijzen aan de [rollen die worden ondersteund door Service Fabric](service-fabric-cluster-security-roles.md): alleen-lezen en -beheerder.

Voer `SetupApplications.ps1` uit, en geef de tenant-ID, de naam van het cluster en de antwoord-URL van de webtoepassing op als parameters. Geef de gebruikersnamen en wachtwoorden voor de gebruikers. Bijvoorbeeld:

```powershell
$Configobj = .\SetupApplications.ps1 -TenantId '<MyTenantID>' -ClusterName 'mysfcluster123' -WebApplicationReplyUrl 'https://mysfcluster123.eastus.cloudapp.azure.com:19080/Explorer/index.html' -AddResourceAccess
.\SetupUser.ps1 -ConfigObj $Configobj -UserName 'TestUser' -Password 'P@ssword!123'
.\SetupUser.ps1 -ConfigObj $Configobj -UserName 'TestAdmin' -Password 'P@ssword!123' -IsAdmin
```

> [!NOTE]
> Geef voor nationale clouds (bijvoorbeeld Azure Government, China, Azure, Azure Duitsland), de `-Location` parameter.

U vindt uw *TenantId* ofwel de map-id in de [Azure-portal](https://portal.azure.com). Selecteer **Azure Active Directory** > **eigenschappen** en kopieer de **map-ID** waarde.

De *Clusternaam* wordt gebruikt voor als prefix aan de Azure Active Directory-toepassingen die zijn gemaakt door het script. Het hoeft niet exact overeenkomen met de naam van het daadwerkelijke cluster. Dit is het eenvoudiger aan Azure AD-artefacten worden toegewezen aan de Service Fabric-cluster in gebruik.

*WebApplicationReplyUrl* wordt het standaardeindpunt dat Azure Active Directory retourneert naar uw gebruikers nadat ze klaar zijn aanmelden. Stel dit eindpunt in als het Service Fabric Explorer-eindpunt voor uw cluster, dit is standaard:

https://&lt;cluster_domain&gt;:19080/Explorer

U wordt gevraagd of u zich aanmeldt bij een account dat beheerdersrechten voor de Azure AD-tenant heeft. Nadat u zich hebt aangemeld, maakt het script de web- en systeemeigen toepassingen voor uw Service Fabric-cluster. In de toepassingen van de tenant van de [Azure-portal](https://portal.azure.com), ziet u twee nieuwe vermeldingen:

   * *ClusterName*\_Cluster
   * *ClusterName*\_Client

Het script af te drukken de JSON die is vereist voor de Resource Manager-sjabloon bij het maken van het cluster, dus is het een goed idee om Houd het PowerShell-venster geopend.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

### <a name="add-azure-ad-configuration-to-use-azure-ad-for-client-access"></a>Azure Active Directory-configuratie toevoegen voor het gebruik van Azure Active Directory voor clienttoegang
Configureer in [azuredeploy.json][template] Azure Active Directory in de sectie **Microsoft.ServiceFabric/clusters**. Voeg parameters toe voor de tenant-id, de toepassings-id van het cluster en de clienttoepassings-id.  

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
  "parameters": {
    ...

    "aadTenantId": {
      "type": "string",
      "defaultValue": "0e3d2646-78b3-4711-b8be-74a381d9890c"
    },
    "aadClusterApplicationId": {
      "type": "string",
      "defaultValue": "cb147d34-b0b9-4e77-81d6-420fef0c4180"
    },
    "aadClientApplicationId": {
      "type": "string",
      "defaultValue": "7a8f3b37-cc40-45cc-9b8f-57b8919ea461"
    }
  },

...

{
  "apiVersion": "2018-02-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  ...
  "properties": {
    ...
    "azureActiveDirectory": {
      "tenantId": "[parameters('aadTenantId')]",
      "clusterApplication": "[parameters('aadClusterApplicationId')]",
      "clientApplication": "[parameters('aadClientApplicationId')]"
    },
    ...
  }
}
```

Voeg de parameterwaarden toe in het parameterbestand [azuredeploy.parameters.json][parameters]. Bijvoorbeeld:

```json
"aadTenantId": {
"value": "0e3d2646-78b3-4711-b8be-74a381d9890c"
},
"aadClusterApplicationId": {
"value": "cb147d34-b0b9-4e77-81d6-420fef0c4180"
},
"aadClientApplicationId": {
"value": "7a8f3b37-cc40-45cc-9b8f-57b8919ea461"
}
```
<a id="configurediagnostics" name="configurediagnostics_anchor"></a>

## <a name="configure-diagnostics-collection-on-the-cluster"></a>Verzamelen van diagnostische gegevens configureren op het cluster
Wanneer u een Service Fabric-cluster uitvoert, is het een goed idee om de logboeken te verzamelen van alle knooppunten in een centrale locatie. De logboeken die in een centrale locatie, helpt u bij het analyseren en oplossen van problemen in uw cluster, of problemen in de toepassingen en services die worden uitgevoerd in het cluster.

Eén manier om te uploaden en verzamelen van Logboeken is het gebruik van de extensie Azure Diagnostics (WAD), die wordt logboeken geüpload naar Azure Storage en bevat ook een optie om Logboeken te verzenden naar Azure Application Insights of Eventhub. U kunt ook een extern proces gebruiken voor de gebeurtenissen kan lezen uit de opslag en plaats deze in een analyse platform product, zoals Azure Monitor-Logboeken of een andere oplossing voor het parseren van Logboeken.

Als u deze zelfstudie volgt, diagnostische verzameling al is geconfigureerd in de [sjabloon][template].

Hebt u een bestaand cluster die geen diagnostische gegevens die zijn geïmplementeerd, kunt u deze kunt toevoegen of bijwerken via de clustersjabloon. De Resource Manager-sjabloon die wordt gebruikt voor het maken van het bestaande cluster of de sjabloon downloaden via de portal aanpassen. Wijzig het template.json-bestand door het uitvoeren van de volgende taken:

Een nieuwe opslagresource toevoegen aan de sectie met resources in de sjabloon:
```json
"resources": [
...
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[parameters('applicationDiagnosticsStorageAccountName')]",
  "location": "[parameters('computeLocation')]",
  "sku": {
    "accountType": "[parameters('applicationDiagnosticsStorageAccountType')]"
  },
  "tags": {
    "resourceType": "Service Fabric",
    "clusterName": "[parameters('clusterName')]"
  }
},
...
]
```

Parameters voor de storage-accountnaam en typ vervolgens toevoegen aan de parametersectie van de sjabloon. Vervang die de tijdelijke aanduiding voor tekst storage-accountnaam komt dat hier met de naam van de storage-account u wilt.

```json
"parameters": {
...
"applicationDiagnosticsStorageAccountType": {
    "type": "string",
    "allowedValues": [
    "Standard_LRS",
    "Standard_GRS"
    ],
    "defaultValue": "Standard_LRS",
    "metadata": {
    "description": "Replication option for the application diagnostics storage account"
    }
},
"applicationDiagnosticsStorageAccountName": {
    "type": "string",
    "defaultValue": "**STORAGE ACCOUNT NAME GOES HERE**",
    "metadata": {
    "description": "Name for the storage account that contains application diagnostics data from the cluster"
    }
},
...
}
```

Voeg vervolgens de **IaaSDiagnostics** -extensie voor de matrix extensies van de **VirtualMachineProfile** eigenschap van elk **Microsoft.Compute/virtualMachineScaleSets** resource in het cluster.  Als u de [voorbeeldsjabloon][template], er zijn drie virtuele-machineschaalsets (één voor elk knooppunttype in het cluster).

```json
"apiVersion": "2018-10-01",
"type": "Microsoft.Compute/virtualMachineScaleSets",
"name": "[variables('vmNodeType1Name')]",
"properties": {
    ...
    "virtualMachineProfile": {
        "extensionProfile": {
            "extensions": [
                {
                    "name": "[concat(parameters('vmNodeType0Name'),'_Microsoft.Insights.VMDiagnosticsSettings')]",
                    "properties": {
                        "type": "IaaSDiagnostics",
                        "autoUpgradeMinorVersion": true,
                        "protectedSettings": {
                        "storageAccountName": "[parameters('applicationDiagnosticsStorageAccountName')]",
                        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-05-01-preview').key1]",
                        "storageAccountEndPoint": "https://core.windows.net/"
                        },
                        "publisher": "Microsoft.Azure.Diagnostics",
                        "settings": {
                        "WadCfg": {
                            "DiagnosticMonitorConfiguration": {
                            "overallQuotaInMB": "50000",
                            "EtwProviders": {
                                "EtwEventSourceProviderConfiguration": [
                                {
                                    "provider": "Microsoft-ServiceFabric-Actors",
                                    "scheduledTransferKeywordFilter": "1",
                                    "scheduledTransferPeriod": "PT5M",
                                    "DefaultEvents": {
                                    "eventDestination": "ServiceFabricReliableActorEventTable"
                                    }
                                },
                                {
                                    "provider": "Microsoft-ServiceFabric-Services",
                                    "scheduledTransferPeriod": "PT5M",
                                    "DefaultEvents": {
                                    "eventDestination": "ServiceFabricReliableServiceEventTable"
                                    }
                                }
                                ],
                                "EtwManifestProviderConfiguration": [
                                {
                                    "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                                    "scheduledTransferLogLevelFilter": "Information",
                                    "scheduledTransferKeywordFilter": "4611686018427387904",
                                    "scheduledTransferPeriod": "PT5M",
                                    "DefaultEvents": {
                                    "eventDestination": "ServiceFabricSystemEventTable"
                                    }
                                }
                                ]
                            }
                            }
                        },
                        "StorageAccount": "[parameters('applicationDiagnosticsStorageAccountName')]"
                        },
                        "typeHandlerVersion": "1.5"
                    }
                }
            ...
            ]
        }
    }
}
```
<a id="configureeventstore" name="configureeventstore_anchor"></a>

## <a name="configure-the-eventstore-service"></a>De EventStore-service configureren
De EventStore-service is een optie voor de bewaking in Service Fabric. EventStore biedt een manier om te beoordelen wat de status van uw cluster of de werkbelastingen op een bepaald tijdstip. De EventStore is een stateful Service Fabric-service die wordt onderhouden door gebeurtenissen uit het cluster. De gebeurtenis worden weergegeven via de Service Fabric Explorer, REST en API's. EventStore query's van het cluster rechtstreeks gebruikt om toegang te krijgen van diagnostische gegevens voor elke entiteit in het cluster en moet worden gebruikt om u te helpen:

* Problemen vaststellen op de ontwikkeling en testen, of waar mogelijk gebruikt u een pijplijn voor bewaking
* Bevestig dat beheeracties die u op het cluster onderneemt correct worden verwerkt
* Een 'snapshot' van de manier waarop Service Fabric is interactie met een bepaalde entiteit ophalen



Schakel de EventStore-service op het cluster, het volgende toevoegen aan de **instelling fabricSettings** eigenschap van de **Microsoft.ServiceFabric/clusters** resource:

```json
"apiVersion": "2018-02-01",
"type": "Microsoft.ServiceFabric/clusters",
"name": "[parameters('clusterName')]",
"properties": {
    ...
    "fabricSettings": [
        ...
        {
            "name": "EventStoreService",
            "parameters": [
                {
                "name": "TargetReplicaSetSize",
                "value": "3"
                },
                {
                "name": "MinReplicaSetSize",
                "value": "1"
                }
            ]
        }
    ]
}
```
<a id="configureloganalytics" name="configureloganalytics_anchor"></a>

## <a name="set-up-azure-monitor-logs-for-the-cluster"></a>Instellen van Azure Monitor-logboeken voor het cluster

Logboeken in Azure Monitor is onze aanbeveling om te controleren op gebeurtenissen op cluster. Als u logboeken van Azure Monitor voor het bewaken van uw cluster instelt, moet u beschikken over [diagnostische gegevens zijn ingeschakeld op het niveau van het cluster gebeurtenissen weergeven](#configure-diagnostics-collection-on-the-cluster).  

De werkruimte moet worden verbonden met de diagnostische gegevens die afkomstig zijn van uw cluster.  In dit logboekgegevens worden opgeslagen in de *applicationDiagnosticsStorageAccountName* storage-account, in de WADServiceFabric * EventTable WADWindowsEventLogsTable en WADETWEventTable tabellen.

Voeg de Azure Log Analytics-werkruimte en de oplossing toevoegt aan de werkruimte:

```json
"resources": [
    ...
    {
        "apiVersion": "2015-11-01-preview",
        "location": "[parameters('omsRegion')]",
        "name": "[parameters('omsWorkspacename')]",
        "type": "Microsoft.OperationalInsights/workspaces",
        "properties": {
            "sku": {
                "name": "Free"
            }
        },
        "resources": [
            {
                "apiVersion": "2015-11-01-preview",
                "name": "[concat(variables('applicationDiagnosticsStorageAccountName'),parameters('omsWorkspacename'))]",
                "type": "storageinsightconfigs",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]",
                    "[concat('Microsoft.Storage/storageAccounts/', variables('applicationDiagnosticsStorageAccountName'))]"
                ],
                "properties": {
                    "containers": [],
                    "tables": [
                        "WADServiceFabric*EventTable",
                        "WADWindowsEventLogsTable",
                        "WADETWEventTable"
                    ],
                    "storageAccount": {
                        "id": "[resourceId('Microsoft.Storage/storageaccounts/', variables('applicationDiagnosticsStorageAccountName'))]",
                        "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('applicationDiagnosticsStorageAccountName')),'2015-06-15').key1]"
                    }
                }
            },
            {
                "apiVersion": "2015-11-01-preview",
                "type": "datasources",
                "name": "sampleWindowsPerfCounter",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]"
                ],
                "kind": "WindowsPerformanceCounter",
                "properties": {
                    "objectName": "Memory",
                    "instanceName": "*",
                    "intervalSeconds": 10,
                    "counterName": "Available MBytes"
                }
            },
            {
                "apiVersion": "2015-11-01-preview",
                "type": "datasources",
                "name": "sampleWindowsPerfCounter2",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]"
                ],
                "kind": "WindowsPerformanceCounter",
                "properties": {
                    "objectName": "Service Fabric Service",
                    "instanceName": "*",
                    "intervalSeconds": 10,
                    "counterName": "Average milliseconds per request"
                }
            }
        ]
    },
    {
        "apiVersion": "2015-11-01-preview",
        "location": "[parameters('omsRegion')]",
        "name": "[variables('solution')]",
        "type": "Microsoft.OperationsManagement/solutions",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]"
        ],
        "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]"
        },
        "plan": {
            "name": "[variables('solution')]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('solutionName'))]",
            "promotionCode": ""
        }
    }
]
```

Vervolgens voegt u parameters toe
```json
"parameters": {
    ...
    "omsWorkspacename": {
        "type": "string",
        "defaultValue": "mysfomsworkspace",
        "metadata": {
            "description": "Name of your OMS Log Analytics Workspace"
        }
    },
    "omsRegion": {
        "type": "string",
        "defaultValue": "West Europe",
        "allowedValues": [
            "West Europe",
            "East US",
            "Southeast Asia"
        ],
        "metadata": {
            "description": "Specify the Azure Region for your OMS workspace"
        }
    }
}
```

Vervolgens voegt u variabelen toe:
```json
"variables": {
    ...
    "solution": "[Concat('ServiceFabric', '(', parameters('omsWorkspacename'), ')')]",
    "solutionName": "ServiceFabric"
}
```

Voeg de Log Analytics-agent-extensie op de schaal van elke virtuele machine instellen in het cluster en de agent verbinden met Log Analytics-werkruimte. Hierdoor kunnen verzamelen van diagnostische gegevens over containers, toepassingen en bewaking van toepassingsprestaties. Door deze toe te voegen als een uitbreiding op de virtuele machine scale set-resource, Azure Resource Manager zorgt ervoor dat deze wordt geïnstalleerd op elk knooppunt, zelfs wanneer schalen van het cluster.

```json
"apiVersion": "2018-10-01",
"type": "Microsoft.Compute/virtualMachineScaleSets",
"name": "[variables('vmNodeType1Name')]",
"properties": {
    ...
    "virtualMachineProfile": {
        "extensionProfile": {
            "extensions": [
                {
                    "name": "[concat(variables('vmNodeType0Name'),'OMS')]",
                    "properties": {
                        "publisher": "Microsoft.EnterpriseCloud.Monitoring",
                        "type": "MicrosoftMonitoringAgent",
                        "typeHandlerVersion": "1.0",
                        "autoUpgradeMinorVersion": true,
                        "settings": {
                            "workspaceId": "[reference(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename')), '2015-11-01-preview').customerId]"
                        },
                        "protectedSettings": {
                            "workspaceKey": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename')),'2015-11-01-preview').primarySharedKey]"
                        }
                    }
                }
            ...
            ]
        }
    }
}
```

<a id="createvaultandcert" name="createvaultandcert_anchor"></a>

## <a name="deploy-the-virtual-network-and-cluster"></a>Het virtuele netwerk en het cluster implementeren

Stel vervolgens de netwerktopologie in en implementeer het Service Fabric-cluster. De [azuredeploy.json] [ template] Resource Manager-sjabloon maakt u een virtueel netwerk, subnet en netwerkbeveiligingsgroep voor Service Fabric. De sjabloon implementeert ook een cluster met certificaatbeveiliging ingeschakeld. Gebruik voor productieclusters een certificaat van een certificeringsinstantie als het clustercertificaat. Een zelfondertekend certificaat kan worden gebruikt om testclusters te beveiligen.

De sjabloon in dit artikel implementeert een cluster dat gebruik maakt van de certificaatvingerafdruk voor het identificeren van het clustercertificaat. Geen twee certificaten kunnen dezelfde vingerafdruk hebben, waardoor certificaatbeheer moeilijker wordt. Schakelen tussen een geïmplementeerde cluster op basis van vingerafdrukken van het certificaat op de algemene naam van het certificaat vereenvoudigt het beheer van certificaten. Lees voor informatie over het bijwerken van het cluster voor het gebruik van certificaat algemene namen voor het beheren van certificaten, [cluster wijzigen de algemene naam Certificaatbeheer](service-fabric-cluster-change-cert-thumbprint-to-cn.md).

### <a name="create-a-cluster-by-using-an-existing-certificate"></a>Een cluster maken met behulp van een bestaand certificaat

Het volgende script maakt gebruik van de cmdlet [New-AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) en een sjabloon om een nieuw cluster in Azure te implementeren. De cmdlet maakt een nieuwe sleutelkluis in Azure en uploadt uw certificaat.

```powershell
# Variables.
$groupname = "sfclustertutorialgroup"
$clusterloc="southcentralus"  # Must match the location parameter in the template
$templatepath="C:\temp\cluster"

$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
$clustername = "mysfcluster123"  # Must match the clustername parameter in the template
$vaultname = "clusterkeyvault123"
$vaultgroupname="clusterkeyvaultgroup123"
$subname="$clustername.$clusterloc.cloudapp.azure.com"

# Sign in to your Azure account and select your subscription
Connect-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>

# Create a new resource group for your deployment, and give it a name and a location.
New-AzureRmResourceGroup -Name $groupname -Location $clusterloc

# Create the Service Fabric cluster.
New-AzureRmServiceFabricCluster  -ResourceGroupName $groupname -TemplateFile "$templatepath\azuredeploy.json" `
-ParameterFile "$templatepath\azuredeploy.parameters.json" -CertificatePassword $certpwd `
-KeyVaultName $vaultname -KeyVaultResourceGroupName $vaultgroupname -CertificateFile $certpath
```

### <a name="create-a-cluster-by-using-a-new-self-signed-certificate"></a>Een cluster met behulp van een nieuw, zelfondertekend certificaat maken

Het volgende script maakt gebruik van de cmdlet [New-AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) en een sjabloon om een nieuw cluster in Azure te implementeren. De cmdlet maakt een nieuwe sleutelkluis in Azure, voegt u een nieuw zelfondertekend certificaat toe aan de sleutelkluis en downloadt het certificaatbestand lokaal.

```powershell
# Variables.
$groupname = "sfclustertutorialgroup"
$clusterloc="southcentralus"  # Must match the location parameter in the template
$templatepath="C:\temp\cluster"

$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
$certfolder="c:\mycertificates\"
$clustername = "mysfcluster123"
$vaultname = "clusterkeyvault123"
$vaultgroupname="clusterkeyvaultgroup123"
$subname="$clustername.$clusterloc.cloudapp.azure.com"

# Sign in to your Azure account and select your subscription
Connect-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>

# Create a new resource group for your deployment, and give it a name and a location.
New-AzureRmResourceGroup -Name $groupname -Location $clusterloc

# Create the Service Fabric cluster.
New-AzureRmServiceFabricCluster  -ResourceGroupName $groupname -TemplateFile "$templatepath\azuredeploy.json" `
-ParameterFile "$templatepath\azuredeploy.parameters.json" -CertificatePassword $certpwd `
-CertificateOutputFolder $certfolder -KeyVaultName $vaultname -KeyVaultResourceGroupName $vaultgroupname -CertificateSubjectName $subname

```

## <a name="connect-to-the-secure-cluster"></a>Verbinding maken met het beveiligde cluster

Verbinding maken met het cluster met behulp van de Service Fabric PowerShell-module geïnstalleerd met de Service Fabric SDK.  Installeer eerst het certificaat in de persoonlijke opslag (Mijn opslag) van de huidige gebruiker op uw computer. Voer de volgende PowerShell-opdracht uit:

```powershell
$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\mysfcluster20170531104310.pfx `
        -Password $certpwd
```

U kunt nu verbinding maken met uw beveiligd cluster.

De **Service Fabric** PowerShell-module biedt veel cmdlets voor het beheren van Service Fabric-clusters, toepassingen en services. Gebruik de cmdlet [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) om verbinding te maken met het beveiligde cluster. De SHA1-vingerafdruk van het certificaat en de eindpuntdetails van de verbinding vindt u in de uitvoer van de vorige stap.

Als u Azure AD-clientverificatie hebt ingesteld, voert u de volgende opdracht uit: 
```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster123.southcentralus.cloudapp.azure.com:19000 `
        -KeepAliveIntervalInSec 10 `
        -AzureActiveDirectory `
        -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10
```

Als u Azure AD-client-verificatie niet hebt ingesteld, voert u de volgende opdracht uit:
```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster123.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

Controleer of u verbonden bent en of het cluster in orde, met behulp van is de [Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth) cmdlet.

```powershell
Get-ServiceFabricClusterHealth
```

## <a name="clean-up-resources"></a>Resources opschonen

De andere artikelen in deze zelfstudie gebruikt u het cluster dat u hebt gemaakt. Als u niet meteen verdergaat met het volgende artikel, is het wellicht een goed idee om [het cluster te verwijderen](service-fabric-cluster-delete.md). U bespaart dan kosten.

## <a name="next-steps"></a>Volgende stappen

Ga naar de volgende zelfstudie voor informatie over het schalen van uw cluster.

> [!div class="checklist"]
> * Een VNET maken in Azure met behulp van PowerShell
> * Een sleutelkluis maken en een certificaat uploaden
> * Azure Active Directory-verificatie instellen
> * Verzamelen van diagnostische gegevens configureren
> * De EventStore-service instellen
> * Instellen van Azure Monitor-Logboeken
> * Een beveiligd Service Fabric-cluster maken in Azure PowerShell
> * Het cluster beveiligen met een X.509-certificaat
> * Verbinding maken met het cluster met behulp van PowerShell
> * Een cluster verwijderen

Ga vervolgens verder naar de volgende zelfstudie voor informatie over het bewaken van uw cluster.
> [!div class="nextstepaction"]
> [Een Cluster bewaken](service-fabric-tutorial-monitor-cluster.md)

[template]:https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/7-VM-Windows-3-NodeTypes-Secure-NSG/AzureDeploy.json
[parameters]:https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/7-VM-Windows-3-NodeTypes-Secure-NSG/AzureDeploy.Parameters.json
