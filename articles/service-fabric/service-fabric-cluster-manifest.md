---
title: Configureren van uw Azure Service Fabric-cluster zelfstandige | Microsoft Docs
description: Informatie over het configureren van uw zelfstandige of on-premises Azure Service Fabric-cluster.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 0c5ec720-8f70-40bd-9f86-cd07b84a219d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/06/2017
ms.author: dekapur
ms.openlocfilehash: bd6e5c1591d01329d95ccb168e5a14e436920baf
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="configuration-settings-for-a-standalone-windows-cluster"></a>Configuratie-instellingen voor een zelfstandige Windows-cluster
Dit artikel wordt beschreven hoe u een zelfstandige Azure Service Fabric-cluster configureren met behulp van het bestand ClusterConfig.json. U kunt dit bestand informatie opgeven over de knooppunten van het cluster, beveiligingsconfiguraties, evenals de netwerktopologie in termen van fouttolerantie en upgrade-domeinen.

Wanneer u [download het zelfstandige Service Fabric-pakket](service-fabric-cluster-creation-for-windows-server.md#downloadpackage), ClusterConfig.json voorbeelden zijn ook opgenomen. De voorbeelden die 'DevCluster' in hun naam hebben maken een cluster met alle drie knooppunten op dezelfde computer, met behulp van de logische knooppunten. Buiten deze knooppunten moet ten minste één worden gemarkeerd als een primaire knooppunt. Dit type van het cluster is nuttig voor ontwikkeling of tests omgevingen. Dit wordt niet ondersteund als een productiecluster. De voorbeelden die 'MultiMachine' in hun naam hebben helpen productie hoogwaardige-clusters maken met elk knooppunt op een afzonderlijke computer. Het aantal primaire knooppunten voor deze clusters is gebaseerd op het cluster [niveau van betrouwbaarheid](#reliability). In versie 5.7, API-versie 05-2017 verwijderd we de eigenschap van het niveau van betrouwbaarheid. Onze code berekent in plaats daarvan het meest geoptimaliseerde niveau van betrouwbaarheid voor uw cluster. Probeer niet een waarde op voor deze eigenschap instellen in versies 5.7 of hoger.


* ClusterConfig.Unsecure.DevCluster.json en ClusterConfig.Unsecure.MultiMachine.json ziet het maken van een niet-beveiligde testomgeving of een productiecluster, respectievelijk.

* ClusterConfig.Windows.DevCluster.json en ClusterConfig.Windows.MultiMachine.json laten zien hoe voor het maken van de test- of -clusters die zijn beveiligd met behulp van [Windows-beveiliging](service-fabric-windows-cluster-windows-security.md).

* ClusterConfig.X509.DevCluster.json en ClusterConfig.X509.MultiMachine.json laten zien hoe voor het maken van de test- of -clusters die zijn beveiligd met behulp van [X509 beveiliging op basis van certificaat](service-fabric-windows-cluster-x509-security.md).

Nu we bekijken de verschillende secties van een bestand ClusterConfig.json.

## <a name="general-cluster-configurations"></a>Algemene clusterconfiguraties
Algemene clusterconfiguraties betrekking hebben op de brede cluster-specifieke configuraties, zoals wordt weergegeven in de volgende JSON-fragment:

```json
    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "01-2017",
```

U kunt een beschrijvende naam geven tot uw Service Fabric-cluster toewijzen aan de naamvariabele. De clusterConfigurationVersion is het versienummer van het cluster. Vergroot het telkens wanneer u een upgrade uitvoert van Service Fabric-cluster. Laat apiVersion ingesteld op de standaardwaarde.

## <a name="nodes-on-the-cluster"></a>Knooppunten op het cluster

    <a id="clusternodes"></a>

U kunt de knooppunten op uw Service Fabric-cluster configureren met behulp van de sectie knooppunten als het volgende codefragment bevat:

    "nodes": [{
        "nodeName": "vm0",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
        "nodeName": "vm1",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType1",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType2",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],

Een Service Fabric-cluster moet ten minste drie knooppunten bevatten. U kunt meer knooppunten toevoegen aan deze sectie volgens uw instellingen. De volgende tabel beschrijft de configuratie-instellingen voor elk knooppunt:

| **Knooppuntconfiguratie** | **Beschrijving** |
| --- | --- |
| nodeName |U kunt een beschrijvende naam geven tot het knooppunt. |
| IP-adres |Het IP-adres van uw knooppunt vinden door een opdrachtpromptvenster te openen en te typen `ipconfig`. Let op het IPV4-adres en wijs het toe aan de variabele IP-adres. |
| nodeTypeRef |Elk knooppunt kan worden toegewezen als een ander knooppunt-type. De [knooppunttypen](#node-types) zijn gedefinieerd in de volgende sectie. |
| faultDomain |Domeinen met fouten inschakelen clusterbeheerders voor het definiëren van fysieke knooppunten die op hetzelfde moment als gevolg van gedeelde fysieke afhankelijkheden kunnen mislukken. |
| upgradeDomain |Upgradedomeinen beschrijven sets met knooppunten die Service Fabric-upgrades op rond dezelfde tijd worden afgesloten. U kunt welke knooppunten toewijzen aan welke upgradedomeinen omdat ze zijn niet door de fysieke vereisten beperkt. |

## <a name="cluster-properties"></a>Eigenschappen van cluster
De sectie met eigenschappen in de ClusterConfig.json wordt gebruikt voor het configureren van het cluster, zoals wordt weergegeven:

### <a name="reliability"></a>Betrouwbaarheid
Het concept van reliabilityLevel definieert het aantal replica's of exemplaren van de Service Fabric-systeemservices die kunnen worden uitgevoerd op de primaire knooppunten van het cluster. Het bepalen van de betrouwbaarheid van deze services en is daarom het cluster. De waarde wordt berekend door het systeem tijdens het cluster maken en de upgrade.

    <a id="reliability"></a>

### <a name="diagnostics"></a>Diagnostiek
U kunt de parameters voor het inschakelen van diagnostische gegevens en het oplossen van fouten in de knooppunt- of -cluster, zoals wordt weergegeven in het volgende fragment configureren in de sectie diagnosticsStore: 

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
    }

De metagegevens van een beschrijving van de diagnostische gegevens van uw cluster is en volgens de instellingen kan worden ingesteld. Deze variabelen helpen bij het verzamelen van Logboeken voor ETW-tracering en crashdumps en prestatiemeteritems. Zie voor meer informatie over ETW traceerlogboeken [Tracelog](https://msdn.microsoft.com/library/windows/hardware/ff552994.aspx) en [ETW-tracering](https://msdn.microsoft.com/library/ms751538.aspx). Alle logboeken, met inbegrip van [dumpbestanden crash](https://blogs.technet.microsoft.com/askperf/2008/01/08/understanding-crash-dump-files/) en [prestatiemeteritems](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx), kan worden omgeleid naar de map connectionString op uw computer. U kunt ook AzureStorage gebruiken voor het opslaan van diagnostische gegevens. Zie het volgende voorbeeld-fragment:

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "AzureStorage",
        "IsEncrypted": "false",
        "connectionstring": "xstore:DefaultEndpointsProtocol=https;AccountName=[AzureAccountName];AccountKey=[AzureAccountKey]"
    }

### <a name="security"></a>Beveiliging
De Beveiligingssectie is vereist voor een veilige zelfstandige Service Fabric-cluster. Het volgende fragment toont een deel van deze sectie:

    "security": {
        "metadata": "This cluster is secured using X509 certificates.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        . . .
    }

De metagegevens wordt een beschrijving van uw beveiligde cluster en volgens de instellingen kan worden ingesteld. De ClusterCredentialType en ServerCredentialType bepalen het type beveiliging die het cluster en de knooppunten implementeren. Ze kunnen worden ingesteld op *X509* voor een beveiliging op basis van certificaten of *Windows* voor beveiliging op basis van Azure Active Directory. De rest van de Beveiligingssectie is gebaseerd op het type beveiliging. Zie voor meer informatie over de om de rest van de Beveiligingssectie in te vullen [beveiliging op basis van certificaten in een zelfstandige cluster](service-fabric-windows-cluster-x509-security.md) of [Windows-beveiliging in een zelfstandige cluster](service-fabric-windows-cluster-windows-security.md).

### <a name="node-types"></a>Knooppunttypen

    <a id="nodetypes"></a>

De sectie nodeTypes beschrijft het type van de knooppunten die fungeren als uw cluster heeft. Ten minste één knooppunttype moet worden opgegeven voor een cluster, zoals wordt weergegeven in het volgende fragment: 

    "nodeTypes": [{
        "name": "NodeType0",
        "clientConnectionEndpointPort": "19000",
        "clusterConnectionEndpointPort": "19001",
        "leaseDriverEndpointPort": "19002"
        "serviceConnectionEndpointPort": "19003",
        "httpGatewayEndpointPort": "19080",
        "reverseProxyEndpointPort": "19081",
        "applicationPorts": {
            "startPort": "20575",
            "endPort": "20605"
        },
        "ephemeralPorts": {
            "startPort": "20606",
            "endPort": "20861"
        },
        "isPrimary": true
    }]

De naam is de beschrijvende naam voor dit knooppunttype. Wijs voor het maken van een knooppunt van dit knooppunttype de beschrijvende naam voor de variabele nodeTypeRef voor dat knooppunt als [genoemde](#nodes-on-the-cluster). Definieer de eindpunten van de verbinding die worden gebruikt voor elk knooppunttype. U kunt een ander poortnummer op voor deze verbindingseindpunten, zolang ze geen conflict met alle andere eindpunten in dit cluster veroorzaken. In een cluster met meerdere knooppunten zijn een of meer primaire knooppunten (dat wil zeggen isPrimary is ingesteld op *true*), afhankelijk van de [reliabilityLevel](#reliability). Zie voor meer informatie over de primaire en nonprimary knooppunttypen, [Service Fabric-cluster overwegingen bij capaciteitsplanning](service-fabric-cluster-capacity.md) voor informatie over nodeTypes en reliabilityLevel. 

#### <a name="endpoints-used-to-configure-the-node-types"></a>Eindpunten die worden gebruikt voor het configureren van de knooppunttypen
* clientConnectionEndpointPort is de poort die wordt gebruikt door de client verbinding maakt met het cluster bij client-API's worden gebruikt. 
* clusterConnectionEndpointPort is de poort waarop de knooppunten met elkaar communiceren.
* leaseDriverEndpointPort is de poort die wordt gebruikt door het stuurprogramma van de lease cluster om te controleren of de knooppunten nog steeds actief zijn. 
* serviceConnectionEndpointPort is de poort die wordt gebruikt voor de toepassingen en services die zijn geïmplementeerd op een knooppunt om te communiceren met de Service Fabric-client op dat bepaalde knooppunt.
* httpGatewayEndpointPort is de poort die wordt gebruikt door de Service Fabric Explorer verbinding maken met het cluster.
* ephemeralPorts overschrijven de [dynamische poorten gebruikt door het besturingssysteem](https://support.microsoft.com/kb/929851). Service Fabric gebruikt een deel van deze poorten als toepassing poorten en de resterende zijn beschikbaar voor het besturingssysteem. Er wordt ook aangegeven dit bereik aan het bestaande bereik aanwezig is in het besturingssysteem, zodat u de adresbereiken die zijn opgegeven in de JSON-voorbeeldbestanden voor alle doeleinden gebruiken kunt. Zorg ervoor dat het verschil tussen de begin- en end-poorten ten minste 255 is. U kunt uitvoeren in conflicten als dit verschil te laag, omdat dit bereik wordt gedeeld met het besturingssysteem. Als u wilt zien van het geconfigureerde dynamisch poortbereik, `netsh int ipv4 show dynamicport tcp`.
* applicationPorts zijn de poorten die worden gebruikt door de Service Fabric-toepassingen. Het poortbereik van toepassing moet groot genoeg is voor het eindpunt behoefte van uw toepassingen. Dit bereik moet exclusieve uit het bereik van de dynamische poort op de machine, dat wil zeggen, het bereik van ephemeralPorts zoals ingesteld in de configuratie. Service Fabric gebruikt deze poorten wanneer nieuwe poorten vereist zijn en zorgt voor de firewall voor deze poorten openen. 
* reverseProxyEndpointPort is een optionele reverse proxy-eindpunt. Zie voor meer informatie [Service Fabric omgekeerde proxy](service-fabric-reverseproxy.md). 

### <a name="log-settings"></a>Logboekinstellingen
U kunt de hoofdmappen voor de Service Fabric-gegevens en logboekbestanden instellen in de sectie fabricSettings. U kunt deze mappen alleen tijdens het maken van het oorspronkelijke cluster. Zie het volgende voorbeeld-fragment van dit gedeelte:

    "fabricSettings": [{
        "name": "Setup",
        "parameters": [{
            "name": "FabricDataRoot",
            "value": "C:\\ProgramData\\SF"
        }, {
            "name": "FabricLogRoot",
            "value": "C:\\ProgramData\\SF\\Log"
    }]

U wordt aangeraden dat u een niet-OS-station als de FabricDataRoot en FabricLogRoot gebruiken. Het biedt meer betrouwbaarheid in situaties voorkomen wanneer het besturingssysteem niet meer reageert. Als u alleen de gegevenshoofdmap hebt aanpast, wordt één niveau onder de gegevenshoofdmap in de hoofdmap van het logboek geplaatst.

### <a name="stateful-reliable-services-settings"></a>Instellingen voor stateful Reliable Services
U kunt de globale configuratie-instellingen voor Reliable Services instellen in de sectie KtlLogger. Zie voor meer informatie over deze instellingen [Stateful Reliable Services configureren](service-fabric-reliable-services-configuration.md). Het volgende voorbeeld ziet u hoe de gedeelde transactielogboek die wordt gemaakt een betrouwbare verzamelingen voor stateful services back wilt wijzigen:

    "fabricSettings": [{
        "name": "KtlLogger",
        "parameters": [{
            "name": "SharedLogSizeInMB",
            "value": "4096"
        }]
    }]

### <a name="add-on-features"></a>Extra functies
Voor het configureren van extra functies van de apiVersion configureren als 04 2017 of hoger en configureer de addonFeatures als volgt te werk:

    "apiVersion": "04-2017",
    "properties": {
      "addOnFeatures": [
          "DnsService",
          "RepairManager"
      ]
    }

### <a name="container-support"></a>Container-ondersteuning
Container als ondersteuning wilt inschakelen voor zowel Windows Server-containers en Hyper-V-containers voor zelfstandige clusters, moet de functie van de invoegtoepassing DnsService zijn ingeschakeld.


## <a name="next-steps"></a>Volgende stappen
Nadat u een volledig ClusterConfig.json-bestand dat is geconfigureerd volgens de instellingen van uw zelfstandige cluster hebt, kunt u uw cluster kunt implementeren. Volg de stappen in [maken van een zelfstandige Service Fabric-cluster](service-fabric-cluster-creation-for-windows-server.md). Gaat u verder met [uw cluster visualiseren met Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) en volg de stappen.

