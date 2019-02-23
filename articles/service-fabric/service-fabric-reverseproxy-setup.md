---
title: Azure Service Fabric reverse proxy niet instellen | Microsoft Docs
description: Informatie over het instellen en de omgekeerde proxy van Service Fabric configureren.
services: service-fabric
documentationcenter: na
author: jimacoMS2
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 11/13/2018
ms.author: v-jamebr
ms.openlocfilehash: 52d6781f83262162f261d094d4818f803e5f3866
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2019
ms.locfileid: "56670210"
---
# <a name="set-up-and-configure-reverse-proxy-in-azure-service-fabric"></a>Instellen en configureren van omgekeerde proxy's in Azure Service Fabric
Omgekeerde proxy is een optionele Azure Service Fabric-service waarmee microservices in een Service Fabric-cluster uitgevoerd detecteren en te communiceren met andere services die http-eindpunten hebben. Zie voor meer informatie, [omgekeerde proxy in Azure Service Fabric](service-fabric-reverseproxy.md). Dit artikel ziet u hoe u kunt instellen en configureren van omgekeerde proxy in het cluster. 

## <a name="enable-reverse-proxy-using-azure-portal"></a>Inschakelen van omgekeerde proxy met behulp van Azure portal

Azure portal biedt een optie voor het inschakelen van omgekeerde proxy bij het maken van een nieuw Service Fabric-cluster. U kunt een bestaand cluster voor het gebruik van omgekeerde proxy via de portal niet bijwerken. 

Omgekeerde proxy configureren wanneer u [maken van een cluster met behulp van Azure portal](./service-fabric-cluster-creation-via-portal.md), zorg ervoor dat u het volgende doen:

1. In **stap 2: Clusterconfiguratie**onder **configuratie van knooppunttypen**, selecteer **omgekeerde proxy inschakelen**.

   ![Omgekeerde proxy op portal inschakelen](./media/service-fabric-reverseproxy-setup/enable-rp-portal.png)
2. (Optioneel) Als u wilt beveiligde omgekeerde proxy configureren, moet u een SSL-certificaat configureren. In **stap 3: Beveiliging**op **beveiligingsinstellingen voor het cluster configureren**onder **configuratietype**, selecteer **aangepaste**. Klik vervolgens onder **omgekeerde Proxy SSL-certificaat**, selecteer **bevatten een SSL-certificaat voor omgekeerde proxy** en voer de certificaatdetails van uw.

   ![Beveiligde omgekeerde proxy configureren op de portal](./media/service-fabric-reverseproxy-setup/configure-rp-certificate-portal.png)

   Als u niet de omgekeerde proxy configureren met een certificaat wanneer u het cluster maakt, kunt u dit later doen via het Resource Manager-sjabloon voor de resourcegroep van het cluster. Zie voor meer informatie, [omgekeerde proxy inschakelen via Azure Resource Manager-sjablonen](#enable-reverse-proxy-via-azure-resource-manager-templates).

## <a name="enable-reverse-proxy-via-azure-resource-manager-templates"></a>Inschakelen van omgekeerde proxy via Azure Resource Manager-sjablonen

Voor clusters op Azure, kunt u de Azure Resource Manager-sjabloon om in te schakelen van de omgekeerde proxy in Service Fabric. Wanneer u deze inschakelen door het cluster bij te werken op een later tijdstip of het cluster maakt, kunt u omgekeerde proxy inschakelen. 

Voor een nieuw cluster, kunt u [een aangepaste Resource Manager-sjabloon maken](service-fabric-cluster-creation-via-arm.md) of kunt u een voorbeeldsjabloon. 

U vindt een voorbeeld van Resource Manager-sjablonen waarmee u bij het configureren van beveiligde omgekeerde proxy voor een Azure-cluster in de [beveiligde Reverse Proxy-voorbeeldsjablonen](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/ReverseProxySecureSample) op GitHub. Raadpleeg [HTTPS Reverse Proxy configureren in een beveiligd cluster](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/ReverseProxySecureSample/README.md#configure-https-reverse-proxy-in-a-secure-cluster) in de Leesmij-bestand voor instructies en de sjablonen kunnen gebruiken voor veilige omgekeerde proxy configureren met een certificaat en om af te handelen rollover van certificaten.

Voor een bestaand cluster, kunt u de Resource Manager-sjabloon voor de resource van het cluster exporteren groeperen met behulp van de [Azure-portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template#export-the-template-from-resource-group), [PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template-powershell#export-resource-group-as-template), of de [Azure CLI](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template-cli#export-resource-group-as-template).

Nadat u een Resource Manager-sjabloon hebt, kunt u inschakelen de omgekeerde proxy met de volgende stappen uit:

1. Een poort definiëren voor de omgekeerde proxy in het [parametersectie](../azure-resource-manager/resource-group-authoring-templates.md) van de sjabloon.

    ```json
    "SFReverseProxyPort": {
        "type": "int",
        "defaultValue": 19081,
        "metadata": {
            "description": "Endpoint for Service Fabric Reverse proxy"
        }
    },
    ```
2. Geef de poort voor elk van de nodetype objecten in de [ **Microsoft.ServiceFabric/clusters** ](https://docs.microsoft.com/azure/templates/microsoft.servicefabric/clusters) [Resource type sectie](../azure-resource-manager/resource-group-authoring-templates.md).

    De poort wordt aangeduid met de parameternaam van de, reverseProxyEndpointPort.

    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
       "nodeTypes": [
          {
           ...
           "reverseProxyEndpointPort": "[parameters('SFReverseProxyPort')]",
           ...
          },
        ...
        ],
        ...
    }
    ```
3. Voor het configureren van SSL-certificaten op de poort voor de omgekeerde proxy, het certificaat toevoegen aan de ***reverseProxyCertificate*** eigenschap in de **Microsoft.ServiceFabric/clusters** [Resource Typ sectie](../resource-group-authoring-templates.md).

    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]"
        ],
        "properties": {
            ...
            "reverseProxyCertificate": {
                "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                "x509StoreName": "[parameters('sfReverseProxyCertificateStoreName')]"
            },
            ...
            "clusterState": "Default",
        }
    }
    ```

### <a name="supporting-a-reverse-proxy-certificate-thats-different-from-the-cluster-certificate"></a>Ondersteuning van een omgekeerde proxy-certificaat dat verschilt van het clustercertificaat
 Als de reverse proxy-certificaat van het certificaat dat voor beveiliging van het cluster verschilt, moet klikt u vervolgens het eerder opgegeven certificaat worden geïnstalleerd op de virtuele machine en toegevoegd aan de toegangsbeheerlijst (ACL) zodat deze toegankelijk in de Service Fabric. Dit kan worden gedaan de [ **Microsoft.Compute/virtualMachineScaleSets** ](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachinescalesets) [Resource type sectie](../resource-group-authoring-templates.md). Voor de installatie, moet u het certificaat toevoegen aan de osProfile. De extensie-sectie van de sjabloon kan het certificaat in de ACL bijwerken.

  ```json
  {
    "apiVersion": "[variables('vmssApiVersion')]",
    "type": "Microsoft.Compute/virtualMachineScaleSets",
    ....
      "osProfile": {
          "adminPassword": "[parameters('adminPassword')]",
          "adminUsername": "[parameters('adminUsername')]",
          "computernamePrefix": "[parameters('vmNodeType0Name')]",
          "secrets": [
            {
              "sourceVault": {
                "id": "[parameters('sfReverseProxySourceVaultValue')]"
              },
              "vaultCertificates": [
                {
                  "certificateStore": "[parameters('sfReverseProxyCertificateStoreValue')]",
                  "certificateUrl": "[parameters('sfReverseProxyCertificateUrlValue')]"
                }
              ]
            }
          ]
        }
   ....
   "extensions": [
          {
              "name": "[concat(parameters('vmNodeType0Name'),'_ServiceFabricNode')]",
              "properties": {
                      "type": "ServiceFabricNode",
                      "autoUpgradeMinorVersion": false,
                      ...
                      "publisher": "Microsoft.Azure.ServiceFabric",
                      "settings": {
                        "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
                        "nodeTypeRef": "[parameters('vmNodeType0Name')]",
                        "dataPath": "D:\\\\SvcFab",
                        "durabilityLevel": "Bronze",
                        "testExtension": true,
                        "reverseProxyCertificate": {
                          "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                          "x509StoreName": "[parameters('sfReverseProxyCertificateStoreValue')]"
                        },
                  },
                  "typeHandlerVersion": "1.0"
              }
          },
      ]
    }
  ```
> [!NOTE]
> Wanneer u certificaten die verschillen van de clustercertificaat voor het inschakelen van de omgekeerde proxy op een bestaand cluster gebruikt, de reverse proxycertificaat installeren en bijwerken van de ACL voor het cluster voordat u de omgekeerde proxy inschakelen. Voltooi de [Azure Resource Manager-sjabloon](service-fabric-cluster-creation-via-arm.md) implementatie met behulp van de instellingen die worden vermeld eerder voordat u begint met een implementatie voor de omgekeerde proxy inschakelen in de stappen 1-3.

## <a name="enable-reverse-proxy-on-standalone-clusters"></a>Inschakelen van omgekeerde proxy op zelfstandige clusters

Voor zelfstandige clusters, moet u omgekeerde proxy in het bestand ClusterConfig.json inschakelen. Hier kunt u omgekeerde proxy bij maken van een cluster of een upgrade van de configuratie voor een bestaand cluster. Zie voor meer informatie over de instellingen die beschikbaar zijn in ClusterConfig.json bestanden, [zelfstandige clusterinstellingen](./service-fabric-cluster-manifest.md).

De volgende stappen ziet u de instellingen te gebruiken voor het inschakelen van omgekeerde proxy en, optioneel, voor het beveiligen van de omgekeerde proxy met een X.509-certificaat. 

1. Voor het inschakelen van omgekeerde proxy instellen de **reverseProxyEndpointPort** waarde voor het knooppunttype onder **eigenschappen** in de configuratie van het cluster. De volgende JSON ziet u de omgekeerde proxy eindpuntpoort voor instellen naar 19081 voor knooppunten van het type 'NodeType0':

   ```json
       "properties": {
          ... 
           "nodeTypes": [
               {
                   "name": "NodeType0",
                   ...
                   "reverseProxyEndpointPort": "19081",
                   ...
               }
           ],
          ...
       }
   ```
2. (Optioneel) Voor een veilige reverse proxy configureren van een certificaat in de **security** sectie onder **eigenschappen**. 
   - Voor een ontwikkelings- of -omgeving, kunt u de **ReverseProxyCertificate** instelling:

      ```json
          "properties": {
              ...
              "security": {
                  ...
                  "CertificateInformation": {
                      ...
                      "ReverseProxyCertificate": {
                          "Thumbprint": "[Thumbprint]",
                          "ThumbprintSecondary": "[Thumbprint]",
                          "X509StoreName": "My"
                      },
                      ...
                  }
              },
              ...
          }
      ```
   - Voor een productie-omgeving, de **ReverseProxyCertificateCommonNames** instelling wordt aanbevolen:

      ```json
          "properties": {
              ...
              "security": {
                  ...
                  "CertificateInformation": {
                      ...
                      "ReverseProxyCertificateCommonNames": {
                        "CommonNames": [
                            {
                              "CertificateCommonName": "[CertificateCommonName]"
                            }
                          ],
                          "X509StoreName": "My"
                      },
                      ...
                  }
              },
              ...
          }
      ```

   Zie voor meer informatie over het configureren en beheren van certificaten voor een zelfstandige cluster, evenals meer details over het configureren van certificaten die worden gebruikt voor het beveiligen van omgekeerde proxy [X509 beveiliging op basis van certificaat](./service-fabric-windows-cluster-x509-security.md).

Nadat u uw ClusterConfig.json-bestand voor het inschakelen van omgekeerde proxy hebt gewijzigd, volgt u de instructies in [Upgrade van de clusterconfiguratie](service-fabric-cluster-config-upgrade-windows-server.md) push de wijzigingen in uw cluster.


## <a name="expose-reverse-proxy-on-a-public-port-through-azure-load-balancer"></a>Omgekeerde proxy voor een openbare poort via Azure Load Balancer beschikbaar

Om de omgekeerde proxy van buiten een Azure-cluster op te lossen, instellen van Azure Load Balancer-regels en een Azure-Statustest voor de omgekeerde-proxypoort. Deze stappen kunnen worden uitgevoerd met behulp van Azure portal of de Resource Manager-sjabloon op elk gewenst moment nadat u het cluster hebt gemaakt. 

> [!WARNING]
> Wanneer u de omgekeerde proxy poort in de Load Balancer configureert, kunnen alle microservices in het cluster die beschikbaar maken van een HTTP-eindpunt worden opgevraagd van buiten het cluster. Dit betekent dat de microservices is bedoeld voor intern mogelijk kunnen worden gedetecteerd door een bepaald kwaadwillende gebruiker. Dit geeft kunnen ernstige problemen die kunnen worden misbruikt; bijvoorbeeld:
>
> * Een kwaadwillende gebruiker mogelijk een denial of service-aanval starten door een interne service beschikt niet over een voldoende beperkte kwetsbaarheid voor aanvallen herhaaldelijk aan te roepen.
> * Een kwaadwillende gebruiker mogelijk onjuist ingedeelde pakketten leveren aan een interne service, wat resulteert in onbedoeld gedrag.
> * Een service die is bedoeld voor intern opleveren persoonlijke of gevoelige gegevens niet zijn bedoeld om te worden blootgesteld aan services buiten het cluster, dus wanneer deze gevoelige gegevens naar een kwaadwillende gebruiker beschikbaar komen. 
>
> Zorg ervoor dat u volledig begrijpen en oplossen van de mogelijke gevolgen voor de beveiliging voor uw cluster en de apps die zijn uitgevoerd, voordat u de omgekeerde-proxypoort openbaar maken. 
>

Als u wilt om omgekeerde proxy voor een zelfstandige cluster openbaar zichtbaar te maken, wordt de manier waarop u dit doen is afhankelijk van het systeem die als host fungeert voor het cluster en valt buiten het bereik van dit artikel. De voorgaande waarschuwing over het weergeven van omgekeerde proxy openbaar, maar nog steeds van toepassing.

### <a name="expose-the-reverse-proxy-using-azure-portal"></a>Beschikbaar maken van de omgekeerde proxy met behulp van Azure portal 

1. Op de Azure portal en klikt u op de resourcegroep voor uw cluster, en klik vervolgens op de load balancer voor uw cluster.
2. Een health test voor de omgekeerde-proxypoort onder in het linkerdeelvenster van het venster load balancer toevoegen **instellingen**, klikt u op **statuscontroles**. Klik vervolgens op **toevoegen** tests venster aan de bovenkant van de status en Geef details op voor de omgekeerde-proxypoort aan en klik vervolgens op **OK**. De omgekeerde-proxypoort is standaard 19081, tenzij u deze hebt gewijzigd tijdens het maken van het cluster.

   ![Omgekeerde proxy health test configureren](./media/service-fabric-reverseproxy-setup/lb-rp-probe.png)
3. Toevoegen van een Load Balancer-regel voor het beschikbaar te maken van de omgekeerde-proxypoort, in het linkerdeelvenster van de load balancer-venster onder **instellingen**, klikt u op **Taakverdelingsregels**. Klik vervolgens op **toevoegen** aan de bovenkant van de Load balancing regels venster en Voer details in voor de omgekeerde-proxypoort. Zorg ervoor dat u de **poort** waarde naar de poort die u wilt dat de omgekeerde proxy beschikbaar gesteld op, de **back-endpoort** waarde naar de poort die u hebt ingesteld toen u omgekeerde proxy ingeschakeld en de **statustest** waarde op de statustest die u in de vorige stap hebt geconfigureerd. Andere velden als de toepassing en klikt u op **OK**.

   ![Load balancer-regel voor omgekeerde proxy configureren](./media/service-fabric-reverseproxy-setup/lb-rp-rule.png)

### <a name="expose-the-reverse-proxy-via-resource-manager-templates"></a>De omgekeerde proxy via Resource Manager-sjablonen weergeven

De volgende JSON verwijst naar dezelfde sjabloon die wordt gebruikt in [omgekeerde proxy inschakelen via Azure Resource Manager-sjablonen](#enable-reverse-proxy-via-azure-resource-manager-templates). Verwijzen naar dat gedeelte van het document voor meer informatie over het maken van een Resource Manager-sjabloon of een sjabloon exporteert voor een bestaand cluster.  De wijzigingen zijn aangebracht aan de [ **Microsoft.Network/loadBalancers** ](https://docs.microsoft.com/azure/templates/microsoft.network/loadbalancers) [Resource type sectie](../resource-group-authoring-templates.md).

    ```json
    {
        "apiVersion": "[variables('lbApiVersion')]",
        "type": "Microsoft.Network/loadBalancers",
        ...
        ...
        "loadBalancingRules": [
            ...
            {
                "name": "LBSFReverseProxyRule",
                "properties": {
                    "backendAddressPool": {
                        "id": "[variables('lbPoolID0')]"
                    },
                    "backendPort": "[parameters('SFReverseProxyPort')]",
                    "enableFloatingIP": "false",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig0')]"
                    },
                    "frontendPort": "[parameters('SFReverseProxyPort')]",
                    "idleTimeoutInMinutes": "5",
                    "probe": {
                        "id": "[concat(variables('lbID0'),'/probes/SFReverseProxyProbe')]"
                    },
                    "protocol": "tcp"
                }
            }
        ],
        "probes": [
            ...
            {
                "name": "SFReverseProxyProbe",
                "properties": {
                    "intervalInSeconds": 5,
                    "numberOfProbes": 2,
                    "port":     "[parameters('SFReverseProxyPort')]",
                    "protocol": "tcp"
                }
            }  
        ]
    }
    ```


## <a name="customize-reverse-proxy-behavior-using-fabric-settings"></a>Met de instellingen van de fabric reverse proxy-gedrag aanpassen

U kunt het gedrag van omgekeerde proxy via fabric-instellingen in het Resource Manager-sjabloon voor clusters die worden gehost in Azure of in het bestand ClusterConfig.json voor zelfstandige clusters aanpassen. Instellingen voor omgekeerde proxy gedrag bevinden zich in de [ **ApplicationGateway/Http** ](./service-fabric-cluster-fabric-settings.md#applicationgatewayhttp) sectie de **instelling fabricSettings** sectie onder het cluster **eigenschappen** sectie. 

Bijvoorbeeld, stelt u de waarde van **DefaultHttpRequestTimeout** in te stellen de time-out voor aanvragen op de omgekeerde proxy voor 180 seconden zoals in de volgende JSON:

   ```json
   {
   "fabricSettings": [
             ...
             {
               "name": "ApplicationGateway/Http",
               "parameters": [
                 {
                   "name": "DefaultHttpRequestTimeout",
                   "value": "180"
                 }
               ]
             }
           ],
           ...
   }
   ``` 

Zie voor meer informatie over het bijwerken van de fabric-instellingen voor Azure-clusters [aanpassen met behulp van Resource Manager-sjablonen clusterinstellingen](service-fabric-cluster-config-upgrade-azure.md). Zie voor zelfstandige clusters, [clusterinstellingen voor zelfstandige clusters aanpassen](service-fabric-cluster-config-upgrade-windows-server.md). 

Verschillende infrastructuurinstellingen worden gebruikt voor het tot stand te brengen veilige communicatie tussen de reverse proxy- en -services. Zie voor gedetailleerde informatie over deze instellingen, [verbinding maken met een beveiligd service met de omgekeerde proxy](service-fabric-reverseproxy-configure-secure-communication.md).

## <a name="next-steps"></a>Volgende stappen
* [Doorsturen naar beveiligde HTTP-service met de omgekeerde proxy instellen](service-fabric-reverseproxy-configure-secure-communication.md)
* Zie voor omgekeerde proxy configuratieopties, [ApplicationGateway/Http-sectie in instellingen voor aanpassen van Service Fabric-cluster](service-fabric-cluster-fabric-settings.md#applicationgatewayhttp).
