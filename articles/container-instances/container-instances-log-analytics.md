---
title: Logboekregistratie van containerinstanties met Azure Log Analytics
description: Leer hoe u containeruitvoer (STDOUT en STDERR) naar Azure Log Analytics kunt verzenden.
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: overview
ms.date: 07/17/2018
ms.author: marsma
ms.openlocfilehash: e4c1efbf4c2c844bae971fa1136e0fe3bed18bcc
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39112960"
---
# <a name="container-instance-logging-with-azure-log-analytics"></a>Logboekregistratie van containerinstanties met Azure Log Analytics

Log Analytics-werkruimten bieden centrale locaties voor het opslaan van logboekgegevens en het uitvoeren van query's hierop. Deze logboekgegevens kunnen behalve van Azure-resources ook van on-premises resources en resources in andere clouds afkomstig zijn. Azure Container Instances bevat ingebouwde ondersteuning voor het verzenden van gegevens naar Log Analytics.

Voordat u gegevens van containerinstanties naar Log Analytics kunt verzenden, moet u een containergroep maken met behulp van Azure CLI (of Cloud Shell) en een YAML-bestand. In de volgende secties wordt beschreven hoe u een containergroep kunt maken die geschikt is voor logboekregistratie en hoe u query's op logboeken kunt uitvoeren.

## <a name="prerequisites"></a>Vereisten

Voor het inschakelen van logboekregistratie voor uw containerexemplaren hebt u het volgende nodig:

* [Log Analytics-werkruimte](../log-analytics/log-analytics-quick-create-workspace.md)
* [Azure CLI](/cli/azure/install-azure-cli) (of [Cloud Shell](/azure/cloud-shell/overview))

## <a name="get-log-analytics-credentials"></a>Log Analytics-referenties verkrijgen

Azure Container Instances heeft toestemming nodig om gegevens te verzenden naar uw Log Analytics-werkruimte. U kunt deze machtiging verlenen en de mogelijkheid tot logboekregistratie inschakelen als u de Log Analytics-werkruimte-id en een van de sleutels ervan opgeeft (primaire of secundaire) op het moment dat u de containergroep maakt.

De Log Analytics-werkruimte-ID en de primaire sleutel verkrijgt u als volgt:

1. Navigeer in Azure Portal naar uw Log Analytics-werkruimte.
1. Selecteer **Geavanceerde instellingen** bij **INSTELLINGEN**.
1. Selecteer **Verbonden bronnen** > **Windows-servers** (of **Linux-servers** - de ID en de sleutels zijn voor beide hetzelfde).
1. Noteer het volgende:
   * **WERKRUIMTE-ID**
   * **PRIMAIRE SLEUTEL**

## <a name="create-container-group"></a>Containergroep maken

Nu dat u de Log Analytics-werkruimte-id en de primaire sleutel hebt, kunt u een containergroep maken die geschikt is voor logboekregistratie.

In de volgende voorbeelden worden twee manieren getoond om een containergroep te maken met één [fluentd][fluentd]-container: Azure CLI en Azure CLI met een YAML-sjabloon. In de standaardconfiguratie genereert de Fluentd-container verschillende regels met uitvoer. Omdat deze uitvoer naar de Log Analytics-werkruimte wordt verzonden, is deze heel geschikt is om te illustreren hoe logboeken kunnen worden weergegeven en hoe er query's op kunnen worden uitgevoerd.

### <a name="deploy-with-azure-cli"></a>Implementeren met Azure CLI

Als u Azure CLI wilt gebruiken om te implementeren, moet u de parameters `--log-analytics-workspace` en `--log-analytics-workspace-key` opgeven in de opdracht [az container create][az-container-create]. Vervang de twee werkruimtewaarden door de waarden die u hebt verkregen in de vorige stap (en werk de naam van de resourcegroep bij) voordat u de volgende opdracht gaat uitvoeren.

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
apiVersion: 2018-06-01
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

Voer vervolgens de volgende opdracht uit om de containergroep te implementeren; vervang daarbij `myResourceGroup` door een resourcegroep in uw abonnement (of maak eerst een resourcegroep met de naam 'myResourceGroup'):

```azurecli-interactive
az container create --resource-group myResourceGroup --name mycontainergroup001 --file deploy-aci.yaml
```

Kort nadat u de opdracht hebt opgegeven, zou u een reactie van Azure met implementatiedetails moeten ontvangen.

## <a name="view-logs-in-log-analytics"></a>Logboeken bekijken in Log Analytics

Nadat u de containergroep hebt geïmplementeerd, kan het enkele minuten (wel tien minuten) duren voor de eerste logboekvermeldingen worden weergegeven in Azure Portal. Als u logboeken van de containergroep wilt bekijken, moet u uw Log Analytics-werkruimte openen en het volgende doen:

1. In het overzicht van **OMS-werkruimte** selecteert u **Zoeken in logboeken**.
1. Onder **Nog een aantal query's om te proberen** selecteert u de koppeling **Alle verzamelde gegevens**.

U ziet nu enkele resultaten die door de query `search *` worden weergegeven. Als u eerst geen resultaten ziet, wacht u een paar minuten en selecteert u vervolgens de knop **UITVOEREN** om de query opnieuw uit te voeren. Standaard worden logboekvermeldingen weergegeven in de lijstweergave. Selecteer **Tabel** om de logboekvermeldingen in een meer gecomprimeerde indeling weer te geven. U kunt vervolgens een rij uitbreiden om de inhoud van een afzonderlijke logboekvermelding te zien.

![Resultaten van zoekopdrachten in logboeken in Azure Portal][log-search-01]

## <a name="query-container-logs"></a>Query's uitvoeren op containerlogbestanden

Log Analytics bevat een uitgebreide [querytaal] [ query_lang] voor het ophalen van gegevens uit wel duizenden regels logboekuitvoer.

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

### <a name="log-analytics"></a>Log Analytics

Voor meer informatie over het uitvoeren van query's op logboeken en het configureren van waarschuwingen in Azure Log Analytics, zie:

* [Zoekopdrachten in logboeken in Log Analytics begrijpen](../log-analytics/log-analytics-log-search.md)
* [Consistente waarschuwingen in Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md)

### <a name="monitor-container-cpu-and-memory"></a>CPU en geheugen bewaken

Voor informatie over het bewaken van CPU- en geheugenresources van containerinstanties, zie:

* [Containerresources in Azure Container Instances bewaken](container-instances-monitor.md).

<!-- IMAGES -->
[log-search-01]: ./media/container-instances-log-analytics/portal-query-01.png

<!-- LINKS - External -->
[fluentd]: https://hub.docker.com/r/fluent/fluentd/
[query_lang]: https://docs.loganalytics.io/

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create