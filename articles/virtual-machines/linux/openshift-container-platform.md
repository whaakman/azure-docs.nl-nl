---
title: OpenShift Containerplatform in Azure implementeren | Microsoft Docs
description: OpenShift Containerplatform in Azure implementeren.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: mdotson
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/18/2019
ms.author: haroldw
ms.openlocfilehash: 296bc42313ef80425004d3c9b43c6792cbaf97f4
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "62127001"
---
# <a name="deploy-openshift-container-platform-in-azure"></a>OpenShift Containerplatform in Azure implementeren

U kunt een van de verschillende methoden voor het implementeren van OpenShift Container Platform in Azure:

- U kunt handmatig implementeren van de onderdelen die nodig zijn Azure-infrastructuur en voer vervolgens de [documentatie voor OpenShift Container Platform](https://docs.openshift.com/container-platform).
- U kunt ook een bestaande [Resource Manager-sjabloon](https://github.com/Microsoft/openshift-container-platform/) die vereenvoudigt de implementatie van het cluster OpenShift Container Platform.
- Een andere optie is met de [Azure Marketplace-aanbieding](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.openshift-container-platform?tab=Overview).

Een Red Hat-abonnement is vereist voor alle opties. De Red Hat Enterprise Linux-exemplaar is tijdens de implementatie aan het Red Hat-abonnement is geregistreerd en die is gekoppeld aan de groeps-ID die de rechten voor OpenShift Container Platform bevat.
Zorg ervoor dat u hebt een geldige gebruikersnaam, wachtwoord en groep-ID in Red Hat abonnement Manager (RHSM) U kunt een activeringscode, organisatie-ID en groep-ID. U kunt deze informatie controleren door aan te melden bij https://access.redhat.com.


## <a name="deploy-using-the-openshift-container-platform-resource-manager-template"></a>Implementeren met behulp van de OpenShift Container Platform Resource Manager-sjabloon

### <a name="private-clusters"></a>Privé-Clusters

Persoonlijke OpenShift-clusters te implementeren vereist meer dan niet met een openbaar IP-adres dat is gekoppeld aan de master load balancer (web-console) of aan de netwerktaakverdeler infra (router).  Een persoonlijke cluster maakt in het algemeen gebruik van een aangepaste DNS-server (niet de standaard Azure DNS), een aangepaste domeinnaam op (zoals contoso.com) en vooraf gedefinieerde virtuele netwerken.  Voor privé-clusters moet u uw virtuele netwerk vooraf configureren met de juiste subnetten en DNS-serverinstellingen.  Gebruik vervolgens **existingMasterSubnetReference**, **existingInfraSubnetReference**, **existingCnsSubnetReference**, en  **existingNodeSubnetReference** om op te geven van het bestaande subnet voor gebruik door het cluster.

Als u persoonlijke master hebt geselecteerd (**masterClusterType**= persoonlijke), een statisch privé IP-adres moet worden opgegeven voor **masterPrivateClusterIp**.  Deze IP wordt toegewezen aan de front-end van de master load balancer.  Het IP-adres moet zich binnen de CIDR voor de master-subnet en niet in gebruik.  **masterClusterDnsType** moet worden ingesteld op 'aangepaste' en de DNS-naam moet worden opgegeven voor master **masterClusterDns**.  De DNS-naam moet worden toegewezen aan het statische privé-IP en wordt gebruikt voor toegang tot de console op de hoofdknooppunten.

Als u privé-router is geselecteerd (**routerClusterType**= persoonlijke), een statisch privé IP-adres moet worden opgegeven voor **routerPrivateClusterIp**.  Deze IP wordt toegewezen aan de front-end van de infra load balancer.  Het IP-adres moet zich binnen de CIDR voor de infra subnet en niet wordt gebruikt.  **routingSubDomainType** moet worden ingesteld op 'aangepaste' en de DNS-naam van jokertekens voor routering moet worden opgegeven voor **routingSubDomain**.  

Als persoonlijke modellen en persoonlijke router zijn geselecteerd, de aangepaste domeinnaam moet worden ingevoerd voor **domeinnaam**

Na een geslaagde implementatie is het knooppunt bastionhost het enige knooppunt met een openbaar IP-adres dat u kunt ssh in.  Zelfs als de hoofdknooppunten worden geconfigureerd voor openbare toegang, ze worden niet weergegeven voor ssh toegang.

Als u wilt implementeren met behulp van de Resource Manager-sjabloon, kunt u een parameterbestand gebruiken om op te geven van de invoerparameters. Voor het verder aanpassen van de implementatie, de GitHub-opslagplaats splitst en de juiste items wijzigen.

Sommige algemene opties voor het aanpassen bevatten, maar niet beperkt tot:

- Bastionhost VM-grootte (variabele in azuredeploy.json)
- Naamconventies (variabelen in azuredeploy.json)
- OpenShift-cluster-specifieke instellingen, worden gewijzigd via het hosts-bestand (deployOpenShift.sh)

### <a name="configure-the-parameters-file"></a>Het parameterbestand configureren

De [sjabloon van OpenShift Container Platform](https://github.com/Microsoft/openshift-container-platform) heeft meerdere branches die beschikbaar zijn voor verschillende versies van OpenShift Container Platform.  Op basis van uw behoeften, kunt u rechtstreeks vanuit de opslagplaats implementeren of u kunt de opslagplaats splitst en aangepaste wijzigingen aanbrengen in de sjablonen of scripts voordat u implementeert.

Gebruik de `appId` waarde van de service-principal die u eerder hebt gemaakt voor de `aadClientId` parameter.

Het volgende voorbeeld ziet een parameterbestand met de naam azuredeploy.parameters.json met de vereiste invoer.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "_artifactsLocation": {
            "value": "https://raw.githubusercontent.com/Microsoft/openshift-container-platform/master"
        },
        "location": {
            "value": "eastus"
        },
        "masterVmSize": {
            "value": "Standard_E2s_v3"
        },
        "infraVmSize": {
            "value": "Standard_D4s_v3"
        },
        "nodeVmSize": {
            "value": "Standard_D4s_v3"
        },
        "cnsVmSize": {
            "value": "Standard_E4s_v3"
        },
        "osImageType": {
            "value": "defaultgallery"
        },
        "marketplaceOsImage": {
            "value": {
                "publisher": "RedHat",
                "offer": "RHEL",
                "sku": "7-RAW",
                "version": "latest"
            }
        },
        "storageKind": {
            "value": "changeme"
        },
        "openshiftClusterPrefix": {
            "value": "changeme"
        },
        "minorVersion": {
            "value": "69"
        },
        "masterInstanceCount": {
            "value": 3
        },
        "infraInstanceCount": {
            "value": 3
        },
        "nodeInstanceCount": {
            "value": 3
        },
        "cnsInstanceCount": {
            "value": 3
        },
        "osDiskSize": {
            "value": 64
        },
        "dataDiskSize": {
            "value": 64
        },
        "cnsGlusterDiskSize": {
            "value": 128
        },
        "adminUsername": {
            "value": "changeme"
        },
        "enableMetrics": {
            "value": "false"
        },
        "enableLogging": {
            "value": "false"
        },
        "enableCNS": {
            "value": "false"
        },
        "rhsmUsernameOrOrgId": {
            "value": "changeme"
        },
        "rhsmPoolId": {
            "value": "changeme"
        },
        "rhsmBrokerPoolId": {
            "value": "changeme"
        },
        "sshPublicKey": {
            "value": "GEN-SSH-PUB-KEY"
        },
        "keyVaultSubscriptionId": {
            "value": "255a325e-8276-4ada-af8f-33af5658eb34"
        },
        "keyVaultResourceGroup": {
            "value": "changeme"
        },
        "keyVaultName": {
            "value": "changeme"
        },
        "enableAzure": {
            "value": "true"
        },
        "aadClientId": {
            "value": "changeme"
        },
        "domainName": {
            "value": "contoso.com"
        },
        "masterClusterDnsType": {
            "value": "default"
        },
        "masterClusterDns": {
            "value": "console.contoso.com"
        },
        "routingSubDomainType": {
            "value": "nipio"
        },
        "routingSubDomain": {
            "value": "apps.contoso.com"
        },
        "virtualNetworkNewOrExisting": {
            "value": "new"
        },
        "virtualNetworkName": {
            "value": "changeme"
        },
        "addressPrefixes": {
            "value": "10.0.0.0/14"
        },
        "masterSubnetName": {
            "value": "changeme"
        },
        "masterSubnetPrefix": {
            "value": "10.1.0.0/16"
        },
        "infraSubnetName": {
            "value": "changeme"
        },
        "infraSubnetPrefix": {
            "value": "10.2.0.0/16"
        },
        "nodeSubnetName": {
            "value": "changeme"
        },
        "nodeSubnetPrefix": {
            "value": "10.3.0.0/16"
        },
        "existingMasterSubnetReference": {
            "value": "/subscriptions/abc686f6-963b-4e64-bff4-99dc369ab1cd/resourceGroups/vnetresourcegroup/providers/Microsoft.Network/virtualNetworks/openshiftvnet/subnets/mastersubnet"
        },
        "existingInfraSubnetReference": {
            "value": "/subscriptions/abc686f6-963b-4e64-bff4-99dc369ab1cd/resourceGroups/vnetresourcegroup/providers/Microsoft.Network/virtualNetworks/openshiftvnet/subnets/infrasubnet"
        },
        "existingCnsSubnetReference": {
            "value": "/subscriptions/abc686f6-963b-4e64-bff4-99dc369ab1cd/resourceGroups/vnetresourcegroup/providers/Microsoft.Network/virtualNetworks/openshiftvnet/subnets/cnssubnet"
        },
        "existingNodeSubnetReference": {
            "value": "/subscriptions/abc686f6-963b-4e64-bff4-99dc369ab1cd/resourceGroups/vnetresourcegroup/providers/Microsoft.Network/virtualNetworks/openshiftvnet/subnets/nodesubnet"
        },
        "masterClusterType": {
            "value": "public"
        },
        "masterPrivateClusterIp": {
            "value": "10.1.0.200"
        },
        "routerClusterType": {
            "value": "public"
        },
        "routerPrivateClusterIp": {
            "value": "10.2.0.200"
        },
        "routingCertType": {
            "value": "selfsigned"
        },
        "masterCertType": {
            "value": "selfsigned"
        }
    }
}
```

Vervang de parameters door uw specifieke gegevens.

Andere versies mogelijk verschillende parameters, dus controleer of de vereiste parameters voor de vertakking die u gebruikt.

### <a name="azuredeployparametersjson-file-explained"></a>azuredeploy. Uitgelegd parameters.JSON-bestand

| Eigenschap | Description | Geldige opties | Standaardwaarde |
|----------|-------------|---------------|---------------|
| `_artifactsLocation`  | URL voor artefacten (json, scripts, enz.) |  |  https:\//raw.githubusercontent.com/Microsoft/openshift-container-platform/master  |
| `location` | Azure-regio resources wilt implementeren |  |  |
| `masterVmSize` | Grootte van de hoofd-VM. Selecteer een van de toegestane VM-grootten die worden vermeld in het bestand azuredeploy.json |  | Standard_E2s_v3 |
| `infraVmSize` | Grootte van de Infra VM. Selecteer een van de toegestane VM-grootten die worden vermeld in het bestand azuredeploy.json |  | Standard_D4s_v3 |
| `nodeVmSize` | Grootte van de virtuele machine van de App-knooppunt. Selecteer een van de toegestane VM-grootten die worden vermeld in het bestand azuredeploy.json |  | Standard_D4s_v3 |
| `cnsVmSize` | Grootte van het knooppunt voor systeemeigen opslag (CNS) in de Container VM. Selecteer een van de toegestane VM-grootten die worden vermeld in het bestand azuredeploy.json |  | Standard_E4s_v3 |
| `osImageType` | De RHEL-installatiekopie moet worden gebruikt. defaultgallery: On-Demand; Marketplace: externe afbeeldingen | defaultgallery <br> marketplace | defaultgallery |
| `marketplaceOsImage` | Als `osImageType` marketplace is, voert u de juiste waarden voor 'publisher', 'bieden', 'sku', 'versie' van de marketplace-aanbieding. Deze parameter is een objecttype |  |  |
| `storageKind` | Het type opslag moet worden gebruikt  | Beheerd<br> unmanaged | Beheerd |
| `openshiftClusterPrefix` | Voorvoegsel gebruikt voor het configureren van hostnamen voor alle knooppunten van het cluster.  Tussen 1 en 20 tekens |  | mijncluster |
| `minoVersion` | De secundaire versie van OpenShift Container Platform 3.11 implementeren |  | 69 |
| `masterInstanceCount` | Aantal Masters knooppunten implementeren | 1, 3, 5 | 3 |
| `infraInstanceCount` | Aantal infra knooppunten implementeren | 1, 2, 3 | 3 |
| `nodeInstanceCount` | Aantal knooppunten te implementeren | 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30 | 2 |
| `cnsInstanceCount` | Aantal CNS knooppunten implementeren | 3, 4 | 3 |
| `osDiskSize` | Grootte van de besturingssysteemschijf voor de virtuele machine (in GB) | 64, 128, 256, 512, 1024, 2048 | 64 |
| `dataDiskSize` | Grootte van de gegevensschijf koppelen aan de knooppunten voor Docker-volume (in GB) | 32, 64, 128, 256, 512, 1024, 2048 | 64 |
| `cnsGlusterDiskSize` | Grootte van de gegevensschijf koppelen aan CNS knooppunten voor gebruik door glusterfs (in GB | 32, 64, 128, 256, 512, 1024, 2048 | 128 |
| `adminUsername` | Gebruikersnaam van beheerder voor zowel besturingssysteem (VM) aanmelding als eerste OpenShift-gebruikersaccount |  | ocpadmin |
| `enableMetrics` | Metrische gegevens inschakelen. Metrische gegevens vereisen dat meer resources Selecteer dus juiste grootte voor infrastructuur-VM | true <br> false | false |
| `enableLogging` | Logboekregistratie inschakelen. elasticsearch pod moet 8 GB RAM-geheugen dus de juiste grootte selecteren voor infrastructuur-VM | true <br> false | false |
| `enableCNS` | Systeemeigen opslag Container inschakelen | true <br> false | false |
| `rhsmUsernameOrOrgId` | Red Hat Doelabonnementbeheerder gebruikersnaam of organisatie-ID |  |  |
| `rhsmPoolId` | De Red Hat Doelabonnementbeheerder groeps-ID die uw OpenShift-rechten voor de compute-knooppunten bevat |  |  |
| `rhsmBrokerPoolId` | De Red Hat Doelabonnementbeheerder groeps-ID die uw OpenShift-rechten voor masters en infra-knooppunten bevat. Als u geen andere pool-id's, voert u dezelfde groep-ID als 'rhsmPoolId' |  |
| `sshPublicKey` | Kopieer uw openbare SSH-sleutel hier |  |  |
| `keyVaultSubscriptionId` | De abonnements-ID van het abonnement waarin de Key Vault |  |  |
| `keyVaultResourceGroup` | De naam van de resourcegroep met de Key Vault |  |  |
| `keyVaultName` | De naam van de Sleutelkluis die u hebt gemaakt |  |  |
| `enableAzure` | Azure-Cloudprovider inschakelen | true <br> false | true |
| `aadClientId` | Azure Active Directory-Client-ID ook wel bekend als toepassings-ID voor de Service-Principal |  |  |
| `domainName` | De naam van de aangepaste domeinnaam te gebruiken (indien van toepassing). Ingesteld op 'none' als niet het geval is volledig privé-cluster implementeren |  | geen |
| `masterClusterDnsType` | Domeintype voor OpenShift-webconsole. 'default' gebruikt DNS-label van master infra openbaar IP-adres. 'custom' kunt u de naam van uw eigen definiëren | standaardinstelling <br> aangepast | standaardinstelling |
| `masterClusterDns` | De aangepaste DNS-naam gebruiken voor toegang tot de webconsole OpenShift als u aangepaste voor geselecteerd `masterClusterDnsType` |  | console.contoso.com |
| `routingSubDomainType` | Indien ingesteld op 'nipio' `routingSubDomain` nip.io wordt gebruikt.  'Aangepaste' gebruiken als u uw eigen domein dat u gebruiken wilt voor routering | nipio <br> aangepast | nipio |
| `routingSubDomain` | Het jokerteken DNS-naam die u gebruiken wilt voor de routering als u aangepaste voor geselecteerd `routingSubDomainType` |  | apps.contoso.com |
| `virtualNetworkNewOrExisting` | Selecteer of u een bestaand Virtueelnetwerk gebruiken of een nieuw Virtueelnetwerk maken | bestaande <br> nieuw | nieuw |
| `virtualNetworkResourceGroupName` | Naam van de resourcegroep voor het nieuwe Virtueelnetwerk als u 'Nieuw' voor geselecteerd `virtualNetworkNewOrExisting` |  | resourceGroup().name |
| `virtualNetworkName` | De naam van het nieuwe Virtueelnetwerk te maken als u 'Nieuw' voor geselecteerd `virtualNetworkNewOrExisting` |  | openshiftvnet |
| `addressPrefixes` | Adresvoorvoegsel van het nieuwe virtuele netwerk |  | 10.0.0.0/14 |
| `masterSubnetName` | De naam van de master subnet |  | mastersubnet |
| `masterSubnetPrefix` | Gebruikt voor het subnet master - CIDR moet een subset van de addressPrefix |  | 10.1.0.0/16 |
| `infraSubnetName` | De naam van de infra subnet |  | infrasubnet |
| `infraSubnetPrefix` | CIDR gebruikt voor de infrastructuur-subnet - moet een subset zijn van de addressPrefix |  | 10.2.0.0/16 |
| `nodeSubnetName` | De naam van het subnet van het knooppunt |  | nodesubnet |
| `nodeSubnetPrefix` | Gebruikt voor het subnet van knooppunt - CIDR moet een subset van de addressPrefix |  | 10.3.0.0/16 |
| `existingMasterSubnetReference` | Volledige verwijzing naar bestaande subnet voor de hoofdknooppunten. Niet nodig als het maken van nieuwe vNet / Subnet |  |  |
| `existingInfraSubnetReference` | Verwijzing naar bestaande subnet voor volledige infra knooppunten. Niet nodig als het maken van nieuwe vNet / Subnet |  |  |
| `existingCnsSubnetReference` | Volledige verwijzing naar een bestaand subnet voor CNS knooppunten. Niet nodig als het maken van nieuwe vNet / Subnet |  |  |
| `existingNodeSubnetReference` | Volledige verwijzing naar bestaande subnet voor de compute-knooppunten. Niet nodig als het maken van nieuwe vNet / Subnet |  |  |
| `masterClusterType` | Geef op of het cluster maakt gebruik van persoonlijke of openbare hoofdknooppunten. Als privé is gekozen, wordt niet de hoofdknooppunten worden blootgesteld aan Internet via een openbaar IP-adres. In plaats daarvan het gebruikt de privé IP-adres opgegeven in de `masterPrivateClusterIp` | openbaar <br> persoonlijk | openbaar |
| `masterPrivateClusterIp` | Als persoonlijke hoofdknooppunten zijn geselecteerd, moet klikt u vervolgens een privé IP-adres worden opgegeven voor gebruik door de interne load balancer voor hoofdknooppunten. Deze statische IP-adres moet binnen het CIDR-blok voor het master subnet en niet al in gebruik. Als openbare hoofdknooppunten zijn geselecteerd, deze waarde niet worden gebruikt, maar nog steeds moet worden opgegeven. |  | 10.1.0.200 |
| `routerClusterType` | Geef op of het cluster maakt gebruik van privé of openbaar infra knooppunten. Als particuliere kiest, worden de infra knooppunten wordt niet worden blootgesteld aan Internet via een openbaar IP-adres. In plaats daarvan het gebruikt de privé IP-adres opgegeven in de `routerPrivateClusterIp` | openbaar <br> persoonlijk | openbaar |
| `routerPrivateClusterIp` | Als persoonlijke infra knooppunten zijn ingeschakeld, wordt een privé IP-adres moet worden opgegeven voor gebruik door de interne load balancer voor infra knooppunten. Deze statische IP-adres moet binnen het CIDR-blok voor het master subnet en niet al in gebruik. Als een openbare infra knooppunten zijn geselecteerd, deze waarde niet worden gebruikt, maar nog steeds moet worden opgegeven |  | 10.2.0.200 |
| `routingCertType` | Aangepast certificaat gebruiken voor het routeringsdomein of de zelf-ondertekend certificaat van de standaard - Volg de instructies in **aangepaste certificaten** sectie | selfsigned <br> aangepast | selfsigned |
| `masterCertType` | Aangepast certificaat gebruiken voor het hoofddomein of de zelf-ondertekend certificaat van de standaard - Volg de instructies in **aangepaste certificaten** sectie | selfsigned <br> aangepast | selfsigned |

<br>

### <a name="deploy-using-azure-cli"></a>Implementeren met behulp van Azure CLI

> [!NOTE] 
> De volgende opdracht gebruikmaken van Azure CLI 2.0.8 of hoger. U kunt controleren of de CLI-versie met de `az --version` opdracht. Zie voor het bijwerken van de CLI-versie, [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latesti).

Het volgende voorbeeld wordt het cluster OpenShift en alle gerelateerde resources in een resourcegroep met de naam openshiftrg, met de naam van een implementatie van myOpenShiftCluster geïmplementeerd. De sjabloon wordt verwezen naar rechtstreeks vanuit de GitHub-opslagplaats en een lokale parameters bestand met de naam van bestand azuredeploy.parameters.json wordt gebruikt.

```azurecli 
az group deployment create -g openshiftrg --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-container-platform/master/azuredeploy.json \
      --parameters @./azuredeploy.parameters.json
```

De implementatie duurt minstens 60 minuten om te voltooien, op basis van het totale aantal knooppunten geïmplementeerd en opties die zijn geconfigureerd. De Bastionomgeving DNS FQDN-naam en de URL van de console OpenShift af te drukken naar de terminal wanneer de implementatie is voltooid.

```json
{
  "Bastion DNS FQDN": "bastiondns4hawllzaavu6g.eastus.cloudapp.azure.com",
  "OpenShift Console URL": "http://openshiftlb.eastus.cloudapp.azure.com/console"
}
```

Als u niet bezighouden met de opdrachtregel die wachten op de implementatie is voltooid wilt, toe te voegen `--no-wait` als een van de opties voor de implementatie van de groep. De uitvoer van de implementatie kan worden opgehaald uit Azure portal in de Implementatiesectie voor de resourcegroep.

## <a name="connect-to-the-openshift-cluster"></a>Verbinding maken met het cluster OpenShift

Als de implementatie is voltooid, haalt u de verbinding in het gedeelte van de uitvoer van de implementatie. Verbinding maken met de console OpenShift met uw browser met behulp van de **OpenShift-Console URL**. U kunt ook SSH naar de bastionhost. Hieronder volgt een voorbeeld waarbij de beheerdersgebruikersnaam clusteradmin en het openbare IP voor bastionhost DNS FQDN bastiondns4hawllzaavu6g.eastus.cloudapp.azure.com:

```bash
$ ssh clusteradmin@bastiondns4hawllzaavu6g.eastus.cloudapp.azure.com
```

## <a name="clean-up-resources"></a>Resources opschonen

Gebruik de [az group delete](/cli/azure/group) opdracht voor het verwijderen van de resourcegroep, OpenShift-cluster en alle gerelateerde resources wanneer ze niet meer nodig zijn.

```azurecli 
az group delete --name openshiftrg
```

## <a name="next-steps"></a>Volgende stappen

- [Taken na de implementatie](./openshift-post-deployment.md)
- [OpenShift-implementatie in Azure oplossen](./openshift-troubleshooting.md)
- [Aan de slag met OpenShift Container Platform](https://docs.openshift.com/container-platform)
