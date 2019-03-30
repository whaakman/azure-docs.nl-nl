---
title: Configureren van uw zelfstandige cluster van Azure Service Fabric | Microsoft Docs
description: Informatie over het configureren van uw on-premises of zelfstandig Azure Service Fabric-cluster.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: chackdan
editor: ''
ms.assetid: 0c5ec720-8f70-40bd-9f86-cd07b84a219d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/12/2018
ms.author: dekapur
ms.openlocfilehash: f94a65e469fdb3cee4f02bc5a8f6f5a4a1ea5a16
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58662326"
---
# <a name="configuration-settings-for-a-standalone-windows-cluster"></a>Configuratie-instellingen voor een zelfstandige Windows-cluster
Dit artikel wordt beschreven configuratie-instellingen van een zelfstandig Azure Service Fabric-cluster die kan worden ingesteld in de *ClusterConfig.json* bestand. U gebruikt dit bestand om op te geven informatie over de knooppunten van het cluster, beveiligingsconfiguraties, evenals de netwerktopologie in termen van fout- en upgradedomeinen.  Na het wijzigen of configuratie-instellingen toe te voegen, kunt u een [maken van een zelfstandige cluster](service-fabric-cluster-creation-for-windows-server.md) of [upgrade van de configuratie van een zelfstandige cluster](service-fabric-cluster-config-upgrade-windows-server.md).

Wanneer u [het zelfstandige Service Fabric-pakket downloaden](service-fabric-cluster-creation-for-windows-server.md#downloadpackage), ClusterConfig.json voorbeelden zijn ook opgenomen. De voorbeelden die 'DevCluster' in hun namen hebben maken een cluster met alle drie knooppunten op dezelfde computer, met behulp van de logische knooppunten. Deze knooppunten moet ten minste één worden gemarkeerd als een primaire knooppunt. Dit type cluster is handig voor ontwikkeling- of testomgevingen. Het wordt niet ondersteund als een productiecluster. De voorbeelden die 'MultiMachine' in hun namen hebben helpen productie geavanceerde-clusters maken met elk knooppunt van een afzonderlijke virtuele machine. Het aantal primaire knooppunten voor deze clusters is gebaseerd op van het cluster [betrouwbaarheidsniveau](#reliability). In versie 5.7, API-versie 05-2017, we hebben het betrouwbaarheidsniveau eigenschap verwijderd. Onze code berekend in plaats daarvan het betrouwbaarheidsniveau van de meest optimale voor uw cluster. Probeer niet om in te stellen van een waarde voor deze eigenschap in versies 5.7 of hoger.

* ClusterConfig.Unsecure.DevCluster.json en ClusterConfig.Unsecure.MultiMachine.json laten u zien over het maken van een niet-beveiligde test of een productiecluster, respectievelijk.

* ClusterConfig.Windows.DevCluster.json en ClusterConfig.Windows.MultiMachine.json laten zien hoe het maken van test- of productieomgeving clusters die zijn beveiligd met behulp van [Windows security](service-fabric-windows-cluster-windows-security.md).

* ClusterConfig.X509.DevCluster.json en ClusterConfig.X509.MultiMachine.json laten zien hoe het maken van test- of productieomgeving clusters die zijn beveiligd met behulp van [X509 beveiliging op basis van certificaat](service-fabric-windows-cluster-x509-security.md).

Nu gaan we de verschillende secties van een bestand ClusterConfig.json onderzoeken.

## <a name="general-cluster-configurations"></a>Algemene clusterconfiguraties
Algemene clusterconfiguraties betrekking hebben op de brede cluster-specifieke configuraties, zoals wordt weergegeven in de volgende JSON-fragment:

```json
    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "01-2017",
```

U kunt een willekeurige beschrijvende naam geven aan uw Service Fabric-cluster aan de naamvariabele toe te wijzen. De clusterConfigurationVersion is het versienummer van het cluster. Verhoog het telkens wanneer u een upgrade uitvoert van uw Service Fabric-cluster. ApiVersion ingesteld laat op de standaardwaarde.

## <a name="nodes-on-the-cluster"></a>Knooppunten op het cluster
U kunt de knooppunten op uw Service Fabric-cluster configureren met behulp van de sectie knooppunten als het volgende codefragment bevat:
```json
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
```

Een Service Fabric-cluster moet ten minste drie knooppunten bevatten. U kunt meer knooppunten toevoegen aan deze sectie op basis van uw installatie. De volgende tabel beschrijft de configuratie-instellingen voor elk knooppunt:

| **Knooppuntconfiguratie** | **Beschrijving** |
| --- | --- |
| nodeName |U kunt een willekeurige beschrijvende naam geven aan het knooppunt. |
| iPAddress |Ontdek het IP-adres van het knooppunt door een opdrachtvenster openen en te typen `ipconfig`. Noteer het IPV4-adres en deze toewijzen aan de variabele IP-adres. |
| nodeTypeRef |Elk knooppunt kan worden toegewezen als het type van een ander knooppunt. De [knooppunttypen](#node-types) zijn gedefinieerd in de volgende sectie. |
| faultDomain |Domeinen met fouten kunnen clusterbeheerders voor het definiëren van de fysieke knooppunten die op hetzelfde moment vanwege een gedeelde fysieke afhankelijkheden kunnen mislukken. |
| upgradeDomain |Upgradedomeinen beschrijven sets knooppunten die voor Service Fabric-upgrades op rond dezelfde tijd worden afgesloten. U kunt knooppunten om toe te wijzen aan welke upgradedomeinen, omdat ze zijn niet door fysieke vereisten beperkt. |

## <a name="cluster-properties"></a>Eigenschappen van cluster
De sectie met eigenschappen in de ClusterConfig.json wordt gebruikt om het cluster configureren zoals wordt weergegeven:

### <a name="reliability"></a>Betrouwbaarheid
Het concept van kunt reliabilityLevel definieert het aantal replica's of exemplaren van de Service Fabric-systeemservices die kunnen worden uitgevoerd op de primaire knooppunten van het cluster. Bepaalt de betrouwbaarheid van deze services en wordt daarmee het cluster. De waarde wordt berekend door het systeem gelijktijdig met cluster maken en bijwerken.

### <a name="diagnostics"></a>Diagnostiek
In de sectie diagnosticsStore kunt u parameters voor het inschakelen van diagnostische gegevens en het oplossen van fouten in de knooppunt- of -cluster, zoals wordt weergegeven in het volgende codefragment: 

```json
"diagnosticsStore": {
    "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
    "dataDeletionAgeInDays": "7",
    "storeType": "FileShare",
    "IsEncrypted": "false",
    "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
}
```

De metagegevens een beschrijving van de diagnostische gegevens van uw cluster is en kan worden ingesteld op basis van uw installatie. Deze variabelen te helpen bij het verzamelen van ETW-traceerlogboeken en crashdumps en prestatiemeteritems. Zie voor meer informatie over ETW-traceerlogboeken [Tracelog](https://msdn.microsoft.com/library/windows/hardware/ff552994.aspx) en [ETW-tracering](https://msdn.microsoft.com/library/ms751538.aspx). Alle logboeken, met inbegrip van [crashdumps](https://blogs.technet.microsoft.com/askperf/2008/01/08/understanding-crash-dump-files/) en [prestatiemeteritems](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx), kunnen worden omgeleid naar de connectionString-map op uw computer. U kunt ook AzureStorage gebruiken voor het opslaan van diagnostische gegevens. Zie het volgende voorbeeld-fragment:

```json
"diagnosticsStore": {
    "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
    "dataDeletionAgeInDays": "7",
    "storeType": "AzureStorage",
    "IsEncrypted": "false",
    "connectionstring": "xstore:DefaultEndpointsProtocol=https;AccountName=[AzureAccountName];AccountKey=[AzureAccountKey]"
}
```

### <a name="security"></a>Beveiliging
De Beveiligingssectie is vereist voor een veilige zelfstandige Service Fabric-cluster. Het volgende fragment toont een deel van deze sectie:

```json
"security": {
    "metadata": "This cluster is secured using X509 certificates.",
    "ClusterCredentialType": "X509",
    "ServerCredentialType": "X509",
    . . .
}
```

De metagegevens is een beschrijving van het beveiligde cluster en kan worden ingesteld op basis van uw installatie. De ClusterCredentialType en ServerCredentialType bepaalt het type dat door het cluster en de knooppunten implementeren. Ze kunnen worden ingesteld op *X509* voor beveiliging op basis van certificaten of *Windows* voor beveiliging op basis van Azure Active Directory. De rest van de Beveiligingssectie is gebaseerd op het type beveiliging. Zie voor meer informatie over het invullen van de rest van de Beveiligingssectie [beveiliging op basis van certificaten in een zelfstandige cluster](service-fabric-windows-cluster-x509-security.md) of [Windows-beveiliging in een zelfstandige cluster](service-fabric-windows-cluster-windows-security.md).

### <a name="node-types"></a>Knooppunttypen
De sectie nodeTypes beschrijving van het type van de knooppunten die het cluster is. Ten minste één knooppunttype moet worden opgegeven voor een cluster, zoals wordt weergegeven in het volgende codefragment: 

```json
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
```

De naam is de beschrijvende naam voor dit knooppunttype. Wijs voor het maken van een knooppunt van dit knooppunttype, de beschrijvende naam in de variabele nodeTypeRef voor dat knooppunt, als [genoemde](#nodes-on-the-cluster). Definieer de verbindingseindpunten die worden gebruikt voor elk knooppunttype. U kunt een ander poortnummer voor deze verbindingseindpunten, zolang ze geen conflict met alle andere eindpunten in dit cluster veroorzaken. In een cluster met meerdere knooppunten, moet u er een of meer primaire knooppunten zijn (dat wil zeggen, isPrimary is ingesteld op *waar*), afhankelijk van de [kunt reliabilityLevel van](#reliability). Zie voor meer informatie over de primaire en nonprimary knooppunttypen [Service Fabric-cluster overwegingen voor capaciteitsplanning](service-fabric-cluster-capacity.md) voor informatie over nodeTypes en kunt reliabilityLevel van. 

#### <a name="endpoints-used-to-configure-the-node-types"></a>Eindpunten die worden gebruikt voor het configureren van de knooppunttypen
* clientConnectionEndpointPort is de poort die wordt gebruikt door de client verbinding maken met het cluster wanneer de client-API's worden gebruikt. 
* clusterConnectionEndpointPort is de poort waarop de knooppunten met elkaar communiceren.
* leaseDriverEndpointPort is de poort die wordt gebruikt door het stuurprogramma van de lease cluster om erachter te komen als de knooppunten nog steeds actief zijn. 
* serviceConnectionEndpointPort is de poort die wordt gebruikt voor de toepassingen en services die zijn geïmplementeerd op een knooppunt om te communiceren met de Service Fabric-client op dat knooppunt bepaald.
* httpGatewayEndpointPort is de poort die door Service Fabric Explorer gebruikt om te verbinden met het cluster.
* ephemeralPorts overschrijven de [dynamische poorten gebruikt door het besturingssysteem](https://support.microsoft.com/kb/929851). Service Fabric gebruikt een deel van deze poorten als toepassingspoorten en de resterende zijn beschikbaar voor het besturingssysteem. Er wordt ook aangegeven dit bereik aan het bestaande bereik aanwezig is in het besturingssysteem, zodat u de bereiken die is opgegeven in de voorbeeld-JSON-bestanden voor alle doeleinden gebruiken kunt. Zorg ervoor dat het verschil tussen de begin- en de end-poorten ten minste 255 is. U kunt tegenkomen conflicten als dit verschil te laag is, omdat dit bereik wordt gedeeld met het besturingssysteem. Als u wilt zien van de geconfigureerde dynamisch poortbereik, `netsh int ipv4 show dynamicport tcp`.
* applicationPorts zijn de poorten die worden gebruikt door de Service Fabric-toepassingen. Het bereik moet groot genoeg is voor de behoefte van het eindpunt van uw toepassingen. Dit bereik moet exclusief uit het bereik van de dynamische poort op de computer, dat wil zeggen, het bereik van ephemeralPorts zoals ingesteld in de configuratie. Service Fabric gebruikt deze poorten wanneer er nieuwe poorten vereist zijn en zorgt dat de firewall voor deze poorten te openen. 
* reverseProxyEndpointPort is een optionele reverse proxy-eindpunt. Zie voor meer informatie, [Service Fabric reverse proxy](service-fabric-reverseproxy.md). 

### <a name="log-settings"></a>Logboekinstellingen
In de sectie instelling fabricSettings stelt u de hoofdmappen voor de Service Fabric-gegevens en Logboeken. U kunt deze mappen alleen tijdens het maken van het oorspronkelijke cluster. Zie het volgende voorbeeld fragment van deze sectie:

```json
"fabricSettings": [{
    "name": "Setup",
    "parameters": [{
        "name": "FabricDataRoot",
        "value": "C:\\ProgramData\\SF"
    }, {
        "name": "FabricLogRoot",
        "value": "C:\\ProgramData\\SF\\Log"
}]
```

U wordt aangeraden dat u een niet-OS-station als de FabricDataRoot en FabricLogRoot gebruiken. Het biedt meer betrouwbaarheid in situaties voorkomen wanneer het besturingssysteem niet meer reageert. Als u alleen de gegevenshoofdmap wilt aanpassen, wordt één niveau onder de gegevenshoofdmap in de hoofdmap van het logboek geplaatst.

### <a name="stateful-reliable-services-settings"></a>Instellingen voor stateful Reliable Services
In de sectie KtlLogger kunt u de globale configuratie-instellingen voor Reliable Services instellen. Zie voor meer informatie over deze instellingen [Stateful Reliable Services configureren](service-fabric-reliable-services-configuration.md). Het volgende voorbeeld laat zien hoe om te wijzigen van de gedeelde transactielogboek die back-ups maken geen betrouwbare verzamelingen voor stateful services wordt gemaakt:

```json
"fabricSettings": [{
    "name": "KtlLogger",
    "parameters": [{
        "name": "SharedLogSizeInMB",
        "value": "4096"
    }]
}]
```

### <a name="add-on-features"></a>Functies van invoegtoepassingen
Voor het configureren van extra functies van de apiVersion configureren als 04-2017 of hoger en de addonFeatures configureren, zoals hier wordt weergegeven:

```json
"apiVersion": "04-2017",
"properties": {
    "addOnFeatures": [
        "DnsService",
        "RepairManager"
    ]
}
```

### <a name="container-support"></a>Ondersteuning voor containers
Als u wilt inschakelen voor zowel Windows Server-containers en Hyper-V-containers voor zelfstandige clusters ondersteuning voor containers, moet de functie voor de DNS-service-invoegtoepassing zijn ingeschakeld.

## <a name="next-steps"></a>Volgende stappen
Nadat u een complete hebt *ClusterConfig.json* dat is geconfigureerd op basis van uw zelfstandige cluster setup, kunt u uw cluster implementeren. Volg de stappen in [een zelfstandige Service Fabric-cluster maken](service-fabric-cluster-creation-for-windows-server.md). 

Als u een zelfstandige cluster geïmplementeerd, kunt u ook [upgrade van de configuratie van een zelfstandige cluster](service-fabric-cluster-config-upgrade-windows-server.md). 

Meer informatie over het [uw cluster visualiseren met Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).

