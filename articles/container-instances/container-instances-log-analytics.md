---
title: Logboekregistratie van containerinstanties met Azure Monitor-logboeken
description: Leer hoe u logboeken van Azure-containerinstanties naar Azure Monitor-logboeken verzenden.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: overview
ms.date: 07/09/2019
ms.author: danlep
ms.openlocfilehash: cab0bc4d2d0491c70a1d2f11f3a5d5d831ade6cf
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67722641"
---
# <a name="container-instance-logging-with-azure-monitor-logs"></a>Logboekregistratie van containerinstanties met Azure Monitor-logboeken

Log Analytics-werkruimten bieden een centrale locatie voor het opslaan en opvragen van logboekgegevens van niet alleen Azure-resources, maar ook on-premises bronnen en resources in andere clouds. Azure Container Instances bevat ingebouwde ondersteuning voor het verzenden van gegevens naar Azure Monitor-logboeken.

Container-exemplaar om gegevens te verzenden naar Azure Monitor-Logboeken, moet u een Log Analytics-werkruimte-ID en werkruimte-sleutel opgeven bij het maken van een containergroep. In de volgende secties wordt beschreven hoe u een containergroep kunt maken die geschikt is voor logboekregistratie en hoe u query's op logboeken kunt uitvoeren.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Vereisten

Voor het inschakelen van logboekregistratie voor uw containerexemplaren hebt u het volgende nodig:

* [Log Analytics-werkruimte](../azure-monitor/learn/quick-create-workspace.md)
* [Azure CLI](/cli/azure/install-azure-cli) (of [Cloud Shell](/azure/cloud-shell/overview))

## <a name="get-log-analytics-credentials"></a>Log Analytics-referenties verkrijgen

Azure Container Instances heeft toestemming nodig om gegevens te verzenden naar uw Log Analytics-werkruimte. U kunt deze machtiging verlenen en de mogelijkheid tot logboekregistratie inschakelen als u de Log Analytics-werkruimte-id en een van de sleutels ervan opgeeft (primaire of secundaire) op het moment dat u de containergroep maakt.

De Log Analytics-werkruimte-id en de primaire sleutel verkrijgt u als volgt:

1. Navigeer in Azure Portal naar uw Log Analytics-werkruimte.
1. Onder **instellingen**, selecteer **geavanceerde instellingen**
1. Selecteer **Verbonden bronnen** > **Windows-servers** (of **Linux-servers** - de ID en de sleutels zijn voor beide hetzelfde).
1. Noteer het volgende:
   * **WERKRUIMTE-ID**
   * **PRIMAIRE SLEUTEL**

## <a name="create-container-group"></a>Containergroep maken

Nu dat u de Log Analytics-werkruimte-id en de primaire sleutel hebt, kunt u een containergroep maken die geschikt is voor logboekregistratie.

De volgende voorbeelden ziet u twee manieren om te maken van een containergroep met één [fluentd][fluentd] container: Azure CLI en Azure CLI met een YAML-sjabloon. In de standaardconfiguratie genereert de Fluentd-container verschillende regels met uitvoer. Omdat deze uitvoer naar de Log Analytics-werkruimte wordt verzonden, is deze heel geschikt is om te illustreren hoe logboeken kunnen worden weergegeven en hoe er query's op kunnen worden uitgevoerd.

### <a name="deploy-with-azure-cli"></a>Implementeren met Azure CLI

Als u wilt implementeren met de Azure CLI, geef de `--log-analytics-workspace` en `--log-analytics-workspace-key` parameters in de [az container maken][az-container-create] opdracht. Vervang de twee werkruimtewaarden door de waarden die u hebt verkregen in de vorige stap (en werk de naam van de resourcegroep bij) voordat u de volgende opdracht gaat uitvoeren.

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainergroup001 \
    --image fluent/fluentd \
    --log-analytics-workspace <WORKSPACE_ID> \
    --log-analytics-workspace-key <WORKSPACE_KEY>
```

### <a name="deploy-with-yaml"></a>Implementeren met YAML

Gebruik deze methode als u liever containergroepen met YAML wilt implementeren. Met de volgende YAML wordt een containergroep met een enkele container gedefinieerd. Kopieer de YAML naar een nieuw bestand en vervang `LOG_ANALYTICS_WORKSPACE_ID` en `LOG_ANALYTICS_WORKSPACE_KEY` door de waarden die u hebt verkregen in de vorige stap. Sla het bestand op als **deploy-aci.yaml**.

```yaml
apiVersion: 2018-10-01
location: eastus
name: mycontainergroup001
properties:
  containers:
  - name: mycontainer001
    properties:
      environmentVariables: []
      image: fluent/fluentd
      ports: []
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
  osType: Linux
  restartPolicy: Always
  diagnostics:
    logAnalytics:
      workspaceId: LOG_ANALYTICS_WORKSPACE_ID
      workspaceKey: LOG_ANALYTICS_WORKSPACE_KEY
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

Vervolgens worden de volgende opdracht voor het implementeren van de containergroep uitgevoerd. Vervang `myResourceGroup` met een resource-groep in uw abonnement (of maakt u eerst een resourcegroep met de naam 'myResourceGroup'):

```azurecli-interactive
az container create --resource-group myResourceGroup --name mycontainergroup001 --file deploy-aci.yaml
```

Kort nadat u de opdracht hebt opgegeven, zou u een reactie van Azure met implementatiedetails moeten ontvangen.

## <a name="view-logs-in-azure-monitor-logs"></a>Weergave van logboeken in Azure Monitor-logboeken

Nadat u de containergroep hebt geïmplementeerd, kan het enkele minuten (wel tien minuten) duren voor de eerste logboekvermeldingen worden weergegeven in Azure Portal. Logboeken van de containergroep weergeven:

1. Navigeer in Azure Portal naar uw Log Analytics-werkruimte.
1. Onder **algemene**, selecteer **Logboeken**  
1. Typ de volgende query: `search *`
1. Selecteer **uitvoeren**

U ziet nu enkele resultaten die door de query `search *` worden weergegeven. Als u niet in eerste instantie geen resultaten ziet, wacht een paar minuten en selecteer vervolgens de **uitvoeren** om de query opnieuw uitvoeren. Standaard logboekvermeldingen worden weergegeven in **tabel** indeling. U kunt vervolgens een rij uitbreiden om de inhoud van een afzonderlijke logboekvermelding te zien.

![Resultaten van zoekopdrachten in logboeken in Azure Portal][log-search-01]

## <a name="query-container-logs"></a>Query's uitvoeren op containerlogboeken

Logboeken in Azure Monitor bevat een uitgebreide [querytaal][query_lang] voor het ophalen van gegevens uit misschien wel duizenden regels van de uitvoer.

De logboekregistratieagent van Azure Container Instances verzendt gegevens naar de `ContainerInstanceLog_CL`-tabel in uw Log Analytics-werkruimte. De basisstructuur van een query wordt gevormd door de brontabel (`ContainerInstanceLog_CL`) gevolgd door een reeks operatoren gescheiden door het sluisteken (`|`). U kunt verschillende operatoren aan elkaar koppelen om de resultaten te verfijnen en geavanceerde functies uit te voeren.

Als u voorbeelden van queryresultaten wilt zien, plakt u de volgende query in het tekstvak van de query (onder 'Verouderd conversieprogramma voor taal weergeven') en selecteert u de knop **UITVOEREN** om de query uit te voeren. Deze query geeft alle logboekvermeldingen weer waarvan het veld 'Bericht' het woord 'waarschuwen' bevat:

```query
ContainerInstanceLog_CL
| where Message contains("warn")
```

Complexere query's worden ook ondersteund. Met deze query worden bijvoorbeeld de logboekvermeldingen voor de containergroep 'mycontainergroup001' weergegeven die het afgelopen uur zijn gegenereerd:

```query
ContainerInstanceLog_CL
| where (ContainerGroup_s == "mycontainergroup001")
| where (TimeGenerated > ago(1h))
```

## <a name="next-steps"></a>Volgende stappen

### <a name="azure-monitor-logs"></a>Azure Monitor-logboeken

Voor meer informatie over het uitvoeren van query's op logboeken en het configureren van waarschuwingen in Azure Monitor-logboeken, zie:

* [Zoekopdrachten in Azure Monitor-logboeken begrijpen](../log-analytics/log-analytics-log-search.md)
* [Consistente waarschuwingen in Azure Monitor](../azure-monitor/platform/alerts-overview.md)


### <a name="monitor-container-cpu-and-memory"></a>CPU en geheugen bewaken

Voor informatie over het bewaken van CPU- en geheugenresources van containerinstanties, zie:

* [Containerresources in Azure Container Instances bewaken](container-instances-monitor.md).

<!-- IMAGES -->
[log-search-01]: ./media/container-instances-log-analytics/portal-query-01.png

<!-- LINKS - External -->
[fluentd]: https://hub.docker.com/r/fluent/fluentd/
[query_lang]: https://aka.ms/LogAnalyticsLanguage

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create