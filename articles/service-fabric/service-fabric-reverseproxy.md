---
title: Azure Service Fabric omgekeerde proxy | Microsoft Docs
description: Service-Fabric reverse proxy gebruiken voor communicatie met microservices van binnen en buiten het cluster.
services: service-fabric
documentationcenter: .net
author: BharatNarasimman
manager: timlt
editor: vturecek
ms.assetid: 47f5c1c1-8fc8-4b80-a081-bc308f3655d3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 08/08/2017
ms.author: bharatn
ms.openlocfilehash: 3168a8129e2e73d7ab1de547679aabd10d8f7112
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="reverse-proxy-in-azure-service-fabric"></a>Azure Service Fabric-omgekeerde proxy
Omgekeerde proxy is ingebouwd in Azure Service Fabric helpt microservices uitgevoerd in een Service Fabric-cluster detecteren en te communiceren met andere services die HTTP-eindpunten hebben.

## <a name="microservices-communication-model"></a>Microservices communicatiemodel
Microservices in Service Fabric uitgevoerd op een subset van de knooppunten in het cluster en tussen de knooppunten om verschillende redenen kunt migreren. Als gevolg hiervan kunnen de eindpunten voor microservices dynamisch wijzigen. Microservice moet te ontdekken en te communiceren met andere services in het cluster, gaat u de volgende stappen:

1. Los de servicelocatie via de naming service.
2. Verbinding maken met de service.
3. De voorgaande stappen verpakken in een lus waarmee de omzetting van de service en probeer beleidsregels toepassen op verbindingsfouten

Zie voor meer informatie [Connect en communiceren met services](service-fabric-connect-and-communicate-with-services.md).

### <a name="communicating-by-using-the-reverse-proxy"></a>Communicatie met behulp van de omgekeerde proxy
Omgekeerde proxy is een service die wordt uitgevoerd op elk knooppunt en zorgt voor eindpuntomzetting, automatische nieuwe pogingen en andere verbindingsfouten namens de client-services. Omgekeerde proxy kan worden geconfigureerd voor verschillende beleidsregels van toepassing als het aanvragen van client-services worden verwerkt. Met behulp van een reverse proxy, kunt de client-service geen bibliotheken clientzijde HTTP-communicatie gebruiken en niet speciale oplossing vereisen en Pogingslogica in de service. 

Omgekeerde proxy beschrijft een of meer eindpunten op het lokale knooppunt voor clientservices moet worden gebruikt voor het verzenden van aanvragen naar de andere services.

![Interne communicatie][1]

> **Ondersteunde Platforms**
>
> Omgekeerde proxy in Service Fabric ondersteunt momenteel de volgende platforms
> * *Windows-Cluster*: Windows 8 en hoger of WindowsServer 2012 en hoger
> * *Linux-Cluster*: Reverse Proxy is momenteel niet beschikbaar voor Linux-clusters

## <a name="reaching-microservices-from-outside-the-cluster"></a>Microservices van buiten het cluster is bereikt
Het model van de externe communicatie standaard voor microservices is een opt-in-model waarbij elke service rechtstreeks van externe clients kan niet worden geopend. [Azure Load Balancer](../load-balancer/load-balancer-overview.md), die een netwerkgrens tussen microservices en externe clients voert netwerkadresomzetting en externe aanvragen voor het interne IP: poort eindpunten worden doorgestuurd. U kunt een microservice eindpunt rechtstreeks toegankelijk naar externe clients, moet u eerst de Load Balancer voor het doorsturen van verkeer naar elke poort die de service in het cluster gebruikt configureren. Bovendien worden de meeste microservices, met name stateful microservices niet live op alle knooppunten van het cluster. De microservices kunt verplaatsen tussen knooppunten op failover. In dergelijke gevallen Load Balancer kan effectief niet bepalen de locatie van het doelknooppunt van de replica's waarnaar verkeer moet worden doorgestuurd.

### <a name="reaching-microservices-via-the-reverse-proxy-from-outside-the-cluster"></a>Microservices via de omgekeerde proxy van buiten het cluster is bereikt
In plaats van de poort van een afzonderlijke service in de Load Balancer configureren, kunt u alleen de poort van de omgekeerde proxy configureren in de Load Balancer. Deze configuratie kiest, kunnen clients buiten het cluster te bereiken services binnen het cluster met behulp van de omgekeerde proxy zonder aanvullende configuratie.

![Externe communicatie][0]

> [!WARNING]
> Wanneer u de reverse proxy-poort in de Load Balancer configureert, zijn alle microservices in het cluster die een HTTP-eindpunt adresseerbare van buiten het cluster.
>
>


## <a name="uri-format-for-addressing-services-by-using-the-reverse-proxy"></a>URI-indeling voor het adresseren van services met behulp van de omgekeerde proxy
De omgekeerde proxy maakt gebruik van een specifieke uniform resource identifier (URI)-indeling voor het identificeren van de service-partitie waarnaar de binnenkomende aanvraag moet worden doorgestuurd:

```
http(s)://<Cluster FQDN | internal IP>:Port/<ServiceInstanceName>/<Suffix path>?PartitionKey=<key>&PartitionKind=<partitionkind>&ListenerName=<listenerName>&TargetReplicaSelector=<targetReplicaSelector>&Timeout=<timeout_in_seconds>
```

* **HTTP (s):** de omgekeerde proxy voor het accepteren van HTTP of HTTPS-verkeer kan worden geconfigureerd. Raadpleeg voor HTTPS doorsturen, [verbinding maken met een beveiligde service met de reverse proxy](service-fabric-reverseproxy-configure-secure-communication.md) zodra er een omgekeerde proxy-instellingen om te luisteren op HTTPS.
* **Cluster volledig gekwalificeerde domeinnaam (FQDN) | intern IP-adres:** voor externe clients, kunt u de omgekeerde proxy configureren zodat deze bereikbaar is via het clusterdomein zoals mycluster.eastus.cloudapp.azure.com. De omgekeerde proxy wordt standaard uitgevoerd op elk knooppunt. Voor interne verkeer worden de omgekeerde proxy bereikt op localhost of op een knooppunt van interne IP-adres bijvoorbeeld 10.0.0.1.
* **Poort:** dit is de poort, zoals 19081, die is opgegeven voor de omgekeerde proxy.
* **ServiceInstanceName:** dit is de volledig gekwalificeerde naam van het geïmplementeerde service-exemplaar dat u probeert te bereiken zonder de "fabric: / ' schema. Om bijvoorbeeld te bereiken de *fabric: / myapp/MijnService/* service, gebruikt u *myapp/MijnService*.

    De naam van de service-exemplaar is hoofdlettergevoelig. Gebruik een ander hoofdlettergebruik voor de naam van het service-exemplaar in de URL zorgt ervoor dat de aanvragen mislukt met 404 (niet gevonden).
* **Achtervoegsel-pad:** dit is het werkelijke URL-pad, zoals *myapi/waarden/toevoegen/3*, voor de service waarmee u verbinding wilt maken.
* **PartitionKey:** voor een gepartitioneerde service, is dit de berekende partitiesleutel van de partitie die u wilt bereiken. Dit is *niet* de partitie-ID GUID. Deze parameter is niet vereist voor services die gebruikmaken van het partitieschema singleton.
* **PartitionKind:** dit is het partitieschema van de service. Dit kan zijn 'Int64Range' of 'Met de naam'. Deze parameter is niet vereist voor services die gebruikmaken van het partitieschema singleton.
* **ListenerName** de eindpunten van de service van het formulier zijn {"Eindpunten": {'Listener1': '1', 'Listener2': 'Endpoint2'...}}. Wanneer de service meerdere eindpunten biedt, herkent hieraan het eindpunt dat de aanvraag van de client moet worden doorgestuurd. Dit kan worden weggelaten als de service slechts één listener heeft.
* **TargetReplicaSelector** Hiermee wordt aangegeven hoe de doelreplica of instantie moet worden geselecteerd.
  * Wanneer de doelservice stateful is, kan de TargetReplicaSelector zijn een van de volgende: 'PrimaryReplica', 'RandomSecondaryReplica' of 'RandomReplica'. Als deze parameter niet is opgegeven, is de standaardwaarde 'PrimaryReplica'.
  * Wanneer de doelservice staatloze, hervat omgekeerde proxy een willekeurig exemplaar van de partitie van de service voor het doorsturen van de aanvraag.
* **Time-out:** Hiermee geeft u de time-out voor de HTTP-aanvragen dat is gemaakt door de omgekeerde proxy voor de service namens de clientaanvraag. De standaardwaarde is 60 seconden. Dit is een optionele parameter.

### <a name="example-usage"></a>Voorbeeld van syntaxis
Als u bijvoorbeeld eens het *fabric: / MyApp/MijnService* service die Hiermee opent u een HTTP-listener op de volgende URL:

```
http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/
```

Hieronder vindt u de resources voor de service:

* `/index.html`
* `/api/users/<userId>`

Als de service gebruikt de singleton partitieschema, de *PartitionKey* en *PartitionKind* queryreeksparameters zijn niet vereist en de service kan worden bereikt door het gebruik van de gateway als:

* Extern:`http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService`
* Intern:`http://localhost:19081/MyApp/MyService`

Als de service het partitieschema Uniform Int64 gebruikt de *PartitionKey* en *PartitionKind* queryreeksparameters worden gebruikt voor het bereiken van een partitie van de service:

* Extern:`http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`
* Intern:`http://localhost:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`

Het bereiken van de resources die de service beschikbaar worden gesteld, plaatst u het bronpad achter de servicenaam in de URL:

* Extern:`http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService/index.html?PartitionKey=3&PartitionKind=Int64Range`
* Intern:`http://localhost:19081/MyApp/MyService/api/users/6?PartitionKey=3&PartitionKind=Int64Range`

De gateway stuurt vervolgens deze aanvragen naar de URL van de service:

* `http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/index.html`
* `http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/api/users/6`

## <a name="special-handling-for-port-sharing-services"></a>Speciale verwerking voor het delen van poort services
Azure Application Gateway probeert omzetten van een serviceadres opnieuw en probeer de aanvraag als een service kan niet worden bereikt. Dit is een belangrijk voordeel van Application Gateway omdat clientcode niet hoeft de eigen service-oplossing te implementeren en oplossen van de lus.

In het algemeen als een service kan niet worden bereikt, is de service-exemplaar of de replica verplaatst naar een ander knooppunt als onderdeel van de normale levenscyclus. Als dit gebeurt, kan een netwerkfout verbinding die wijzen op een eindpunt is niet meer openen op het adres van oorspronkelijk opgelost Application Gateway worden weergegeven.

Echter replica's of service-exemplaren kunnen delen een hostproces verstrekt en mogelijk ook delen van een poort wanneer deze wordt gehost door een op basis van het http.sys-webserver, met inbegrip van:

* [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener%28v=vs.110%29.aspx)
* [ASP.NET Core WebListener](https://docs.asp.net/latest/fundamentals/servers.html#weblistener)
* [Katana](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.OwinSelfHost/)

In dit geval is het waarschijnlijk dat de webserver is beschikbaar in het hostproces en reageren op aanvragen, maar de opgelost service-exemplaar of de replica niet meer beschikbaar op de host is. De gateway wordt in dit geval een HTTP 404-reactie ontvangen van de webserver. Een HTTP 404 is dus twee verschillende betekenis:

- Case #1: Het serviceadres juist is, maar de resource die de gebruiker heeft aangevraagd, bestaat niet.
- Case #2: Het serviceadres is onjuist en de resource die de gebruiker heeft aangegeven bestaat mogelijk op een ander knooppunt.

Het eerste geval is een normale HTTP 404, wordt beschouwd als een gebruikersfout opgetreden. De gebruiker heeft echter in het tweede geval verzocht een resource die bestaat. Application Gateway is niet vinden omdat de service zelf is verplaatst. Toepassingsgateway moet het omzetten van het adres opnieuw en probeer de aanvraag.

Toepassingsgateway moet dus een manier onderscheid maken tussen deze twee gevallen. Als u dit onderscheid, is een aanwijzing van de server vereist.

* Standaard Application Gateway wordt ervan uitgegaan dat het geval #2 en probeert oplossen en de aanvraag opnieuw uitgeven.
* Om aan te geven geval #1 aan de toepassingsgateway, moet de service de volgende HTTP-antwoordheader geretourneerd:

  `X-ServiceFabric : ResourceNotFound`

Deze HTTP-antwoordheader geeft aan dat een normale HTTP 404-situatie waarin de aangevraagde resource bestaat niet en Application Gateway niet probeert te zetten van het serviceadres opnieuw.

## <a name="setup-and-configuration"></a>Installatie en configuratie

### <a name="enable-reverse-proxy-via-azure-portal"></a>Inschakelen van omgekeerde proxy via Azure portal

Azure portal biedt een optie voor het inschakelen van omgekeerde proxy tijdens het maken van een nieuwe Service Fabric-cluster.
Onder **maken Service Fabric-cluster**, stap 2: clusterconfiguratie, configuratie van het type, schakel het selectievakje wilt inschakelen reverse proxy-'.
Voor het configureren van beveiligde omgekeerde proxy SSL-certificaat kan worden opgegeven in stap 3: beveiliging, beveiligingsinstellingen voor het cluster configureren, schakel het selectievakje ' een SSL-certificaat voor reverse proxy ' en voer de details van het certificaat.

### <a name="enable-reverse-proxy-via-azure-resource-manager-templates"></a>Inschakelen van omgekeerde proxy via Azure Resource Manager-sjablonen

U kunt de [Azure Resource Manager-sjabloon](service-fabric-cluster-creation-via-arm.md) de reverse proxy in Service Fabric voor het cluster in te schakelen.

Raadpleeg [HTTPS Reverse Proxy configureren in een beveiligde cluster](https://github.com/ChackDan/Service-Fabric/tree/master/ARM Templates/ReverseProxySecureSample#configure-https-reverse-proxy-in-a-secure-cluster) voor Azure Resource Manager voorbeelden van de sjabloon voor het configureren van beveiligde reverse proxy gebruikt met een certificaat en verwerking van certificaat rollover.

U krijgt eerst de sjabloon voor het cluster dat u wilt implementeren. U kunt de voorbeeldsjablonen gebruiken of een aangepaste Resource Manager-sjabloon maken. Vervolgens kunt u de omgekeerde proxy inschakelen met behulp van de volgende stappen uit:

1. Een poort definiëren voor de reverse proxy in de [gedeelte Parameters](../azure-resource-manager/resource-group-authoring-templates.md) van de sjabloon.

    ```json
    "SFReverseProxyPort": {
        "type": "int",
        "defaultValue": 19081,
        "metadata": {
            "description": "Endpoint for Service Fabric Reverse proxy"
        }
    },
    ```
2. Geef de poort voor elk van de nodetype objecten in de **Cluster** [resourcesectie type](../azure-resource-manager/resource-group-authoring-templates.md).

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
3. Om de omgekeerde proxy van buiten het Azure-cluster op te lossen, regels instellen om de Azure Load Balancer voor de poort die u in stap 1 hebt opgegeven.

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
4. Configureren van SSL-certificaten op de poort voor de omgekeerde proxy, het certificaat toevoegen aan de ***reverseProxyCertificate*** eigenschap in de **Cluster** [resourcesectie type](../resource-group-authoring-templates.md) .

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

### <a name="supporting-a-reverse-proxy-certificate-thats-different-from-the-cluster-certificate"></a>Ondersteuning van een reverse proxycertificaat dat afwijkt van de cluster-certificaat
 Als de reverse proxy-certificaat van het certificaat dat het cluster beveiligt verschilt, moet klikt u vervolgens het eerder opgegeven certificaat worden geïnstalleerd op de virtuele machine en toegevoegd aan de toegangsbeheerlijst (ACL) zodat de Service Fabric toegang hebt tot het. Dit kan worden uitgevoerd in de **virtualMachineScaleSets** [resourcesectie type](../resource-group-authoring-templates.md). Voor de installatie, moet u dat certificaat toevoegen aan de osProfile. De extensie-sectie van de sjabloon kan het certificaat in de ACL bijwerken.

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
> Wanneer u certificaten die afwijken van het certificaat van de cluster de reverse proxy op een bestaand cluster in te schakelen, de reverse proxy-certificaat installeren en bijwerken van de ACL voor het cluster voordat u de omgekeerde proxy inschakelt. Voltooi de [Azure Resource Manager-sjabloon](service-fabric-cluster-creation-via-arm.md) implementatie met behulp van de instellingen vermeld eerder voordat u begint met een implementatie voor de omgekeerde proxy inschakelen in stap 1-4.

## <a name="next-steps"></a>Volgende stappen
* Een voorbeeld bekijken van HTTP-communicatie tussen services in een [voorbeeldproject op GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started).
* [Met de reverse proxy worden doorgestuurd naar de beveiligde HTTP-service](service-fabric-reverseproxy-configure-secure-communication.md)
* [Externe procedureaanroepen weer dat met Reliable Services voor externe toegang](service-fabric-reliable-services-communication-remoting.md)
* [Web-API die gebruikmaakt van OWIN in Reliable Services](service-fabric-reliable-services-communication-webapi.md)
* [WCF-communicatie met behulp van Reliable Services](service-fabric-reliable-services-communication-wcf.md)
* Raadpleeg voor aanvullende reverse proxy-configuratie-opties ApplicationGateway/Http-sectie in [aanpassen Service Fabric-clusterinstellingen](service-fabric-cluster-fabric-settings.md).

[0]: ./media/service-fabric-reverseproxy/external-communication.png
[1]: ./media/service-fabric-reverseproxy/internal-communication.png
