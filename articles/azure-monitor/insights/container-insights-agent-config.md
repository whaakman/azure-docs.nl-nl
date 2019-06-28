---
title: Azure Monitor configureren voor gegevensverzameling voor containers-agent | Microsoft Docs
description: Dit artikel wordt beschreven hoe u de Azure-Monitor voor containers-agent voor het beheren van stdout/stderr kunt configureren en omgevingsvariabelen Meld verzameling.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/22/2019
ms.author: magoedte
ms.openlocfilehash: 1e7506e311c38d87371dd1b65440b6fb41a7ce78
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/24/2019
ms.locfileid: "67341646"
---
# <a name="configure-agent-data-collection-for-azure-monitor-for-containers"></a>Configureren van de agent verzamelen van gegevens voor Azure Monitor voor containers

Azure Monitor voor containers verzamelt stdout, stderr en omgevingsvariabelen van containerwerkbelastingen geïmplementeerd op beheerde Kubernetes-clusters die worden gehost in Azure Kubernetes Service (AKS) van de agent in containers. U kunt de agent-instellingen voor het verzamelen van gegevens configureren door het maken van een aangepaste Kubernetes ConfigMaps voor het beheren van deze ervaring. In dit artikel ziet u hoe u ConfigMap maken en configureren van gegevensverzameling op basis van uw vereisten.

## <a name="configure-your-cluster-with-custom-data-collection-settings"></a>Uw cluster configureren met aangepaste instellingen voor het verzamelen

Een sjabloonbestand ConfigMap is opgegeven waarmee u eenvoudig zonder deze helemaal te maken met uw aanpassingen bewerken. Voordat u begint, moet u de Kubernetes-documentatie bekijken over [ConfigMaps](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/) en raken met het maken, configureren en implementeren van ConfigMaps. Hierdoor kunt u voor het filteren van stderr en stdout per naamruimte of in de hele cluster en omgevingsvariabelen voor elke container die wordt uitgevoerd voor alle schillen/knooppunten in het cluster.

>[!IMPORTANT]
>De minimale agentversie ondersteund door deze functie wordt door microsoft / oms:ciprod06142019 of hoger. 

### <a name="overview-of-configurable-data-collection-settings"></a>Overzicht van configureerbare instellingen voor het verzamelen

Hier volgen de instellingen die kunnen worden geconfigureerd voor het beheren van het verzamelen van gegevens.

|Sleutel |Gegevenstype |Value |Description |
|----|----------|------|------------|
|`schema-version` |Tekenreeks (hoofdlettergevoelig) |v1 |Dit is de schemaversie gebruikt door de agent bij het parseren van deze ConfigMap. Momenteel ondersteunde schemaversie is v1. U deze waarde wijzigt, wordt niet ondersteund en worden geweigerd als ConfigMap wordt geëvalueerd.|
|`config-version` |String | | Ondersteunt de mogelijkheid om dit configuratiebestand versie in uw systeem/resourcebeheerbibliotheek bij te houden. Maximaal toegestane aantal tekens zijn en met 10, en alle overige tekens worden afgekapt. |
|`[log_collection_settings.stdout] enabled =` |Boolean | waar of ONWAAR | Hiermee wordt geregeld of logboekverzameling stdout-container is ingeschakeld. Als de waarde `true` en er zijn geen naamruimten zijn uitgesloten voor de logboekverzameling stdout (`log_collection_settings.stdout.exclude_namespaces` instelling hieronder), stdout-logboeken worden verzameld van alle containers voor alle schillen/knooppunten in het cluster. Indien niet opgegeven in ConfigMaps, de standaardwaarde is `enabled = true`. |
|`[log_collection_settings.stdout] exclude_namespaces =`|String | Matrix met door komma's gescheiden |Matrix van Kubernetes-naamruimten voor welke stdout worden geen logboeken verzameld. Deze instelling geldt alleen als `log_collection_settings.stdout.enabled` is ingesteld op `true`. Indien niet opgegeven in ConfigMap, de standaardwaarde is `exclude_namespaces = ["kube-system"]`.|
|`[log_collection_settings.stderr] enabled =` |Boolean | waar of ONWAAR |Hiermee wordt geregeld of logboekverzameling stderr-container is ingeschakeld. Als de waarde `true` en er zijn geen naamruimten zijn uitgesloten voor de logboekverzameling stdout (`log_collection_settings.stderr.exclude_namespaces` instelling), stderr-logboeken worden verzameld van alle containers voor alle schillen/knooppunten in het cluster. Indien niet opgegeven in ConfigMaps, de standaardwaarde is `enabled = true`. |
|`[log_collection_settings.stderr] exclude_namespaces =` |String |Matrix met door komma's gescheiden |Matrix van Kubernetes-naamruimten voor welke stderr worden geen logboeken verzameld. Deze instelling geldt alleen als `log_collection_settings.stdout.enabled` is ingesteld op `true`. Indien niet opgegeven in ConfigMap, de standaardwaarde is `exclude_namespaces = ["kube-system"]`. |
| `[log_collection_settings.env_var] enabled =` |Boolean | waar of ONWAAR | Hiermee wordt geregeld of omgeving variabele verzameling is ingeschakeld. Als de waarde `false`, er zijn geen omgevingsvariabelen worden verzameld voor elke container die wordt uitgevoerd voor alle schillen/knooppunten in het cluster. Indien niet opgegeven in ConfigMap, de standaardwaarde is `enabled = true`. |

### <a name="configure-and-deploy-configmaps"></a>Configureer en implementeer ConfigMaps

De volgende stappen uitvoeren om te configureren en implementeren van uw configuratiebestand ConfigMap met uw cluster.

1. [Download](https://github.com/microsoft/OMS-docker/blob/ci_feature_prod/Kubernetes/container-azm-ms-agentconfig.yaml) de sjabloon ConfigMap yaml-bestand en sla deze op als de container-azm-ms-agentconfig.yaml.  
1. Bewerk het ConfigMap yaml-bestand met uw aanpassingen. 

    - Als u wilt uitsluiten van specifieke naamruimten voor stdout logboekgegevens verzamelen, die u configureert het sleutel/waarde in het volgende voorbeeld: `[log_collection_settings.stdout] enabled = true exclude_namespaces = ["my-namespace-1", "my-namespace-2"]`.
    - Als wilt uitschakelen omgeving variabele verzameling voor een specifieke container, stelt u de sleutel/waarde `[log_collection_settings.env_var] enabled = true` variabelenverzameling wereldwijd inschakelen en volg de stappen [hier](container-insights-manage-agent.md#how-to-disable-environment-variable-collection-on-a-container) naar de volledige configuratie voor de specifieke container.
    - Als wilt uitschakelen stderr logboekverzameling brede, door het cluster, configureert u de sleutel/waarde in het volgende voorbeeld: `[log_collection_settings.stderr] enabled = false`.

1. ConfigMap maken met de volgende kubectl-opdracht: `kubectl apply -f <configmap_yaml_file.yaml>`.
    
    Voorbeeld: `kubectl apply -f container-azm-ms-agentconfig.yaml`. 
    
    Wijzigen van de configuratie kan een paar minuten voltooid voordat u gaat duren en alle omsagent schillen in het cluster wordt opnieuw opgestart. De computer opnieuw is alle omsagent schillen rolling opnieuw worden opgestart, niet alle start opnieuw op hetzelfde moment. Wanneer de opnieuw wordt opgestart, zijn voltooid, wordt er een bericht weergegeven die het resultaat is vergelijkbaar met het volgende: `configmap "container-azm-ms-agentconfig" created`.

Als u wilt controleren of de configuratie is toegepast, kunt u de volgende opdracht gebruiken om te controleren van de logboeken van een agent-schil: `kubectl logs omsagent-fdf58 -n=kube-system`. Als er fouten in de configuratie van de schillen osmagent, worden de uitvoer fouten weergegeven die vergelijkbaar is met het volgende:

``` 
***************Start Config Processing******************** 
config::unsupported/missing config schema version - 'v21' , using defaults
```

Fouten te voorkomen dat omsagent parseren van het bestand, waardoor het systeem opnieuw opstarten en de standaardconfiguratie gebruikt. Nadat u de fouten in ConfigMap corrigeert, sla het yaml-bestand en de bijgewerkte ConfigMaps toepassen met de opdracht: `kubectl apply -f <configmap_yaml_file.yaml`.

## <a name="applying-updated-configmap"></a>Toepassen van ConfigMap bijgewerkt

Als u al een ConfigMap voor uw cluster hebt geïmplementeerd en u wilt bijwerken met een nieuwere configuratie, kunt u gewoon de ConfigMap bestand dat u eerder hebt gebruikt en vervolgens toepassen met behulp van dezelfde opdracht als eerder bewerken `kubectl apply -f <configmap_yaml_file.yaml`.

Wijzigen van de configuratie kan een paar minuten voltooid voordat u gaat duren en alle omsagent schillen in het cluster wordt opnieuw opgestart. De computer opnieuw is alle omsagent schillen rolling opnieuw worden opgestart, niet alle start opnieuw op hetzelfde moment. Wanneer de opnieuw wordt opgestart, zijn voltooid, wordt er een bericht weergegeven die het resultaat is vergelijkbaar met het volgende: `configmap "container-azm-ms-agentconfig" updated`.

## <a name="verifying-schema-version"></a>Schemaversie controleren

Ondersteunde configuratie schema versies zijn beschikbaar als pod aantekening (schema-versies) op de schil omsagent. U kunt ze bekijken met de volgende kubectl-opdracht: `kubectl describe pod omsagent-fdf58 -n=kube-system`

De uitvoer weergegeven die vergelijkbaar is met het volgende bij de aantekening schema-versies:

```
    Name:           omsagent-fdf58
    Namespace:      kube-system
    Node:           aks-agentpool-95673144-0/10.240.0.4
    Start Time:     Mon, 10 Jun 2019 15:01:03 -0700
    Labels:         controller-revision-hash=589cc7785d
                    dsName=omsagent-ds
                    pod-template-generation=1
    Annotations:    agentVersion=1.10.0.1
                  dockerProviderVersion=5.0.0-0
                    schema-versions=v1 
```

## <a name="next-steps"></a>Volgende stappen

- Om door te gaan met het leren over het gebruik van Azure Monitor en controleren van andere aspecten van uw AKS-cluster [weergave Azure Kubernetes Service health](container-insights-analyze.md).
- Weergave [Meld u voorbeelden van](container-insights-log-search.md#search-logs-to-analyze-data) om te zien van vooraf gedefinieerde query's en voorbeelden om te evalueren of aanpassen voor waarschuwingen, visualiseren en analyseren van uw clusters.