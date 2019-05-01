---
title: Azure Monitor - Application Insights voor Kubernetes met service Net Istio | Microsoft Docs
description: Application Insights voor Kubernetes is een oplossing voor prestatiecontrole waarmee u voor het verzamelen van telemetrie van Application Insights die betrekking hebben op binnenkomende en uitgaande aanvragen naar en van schillen uitgevoerd in uw Kubernetes-cluster met behulp van service mesh-technologie met de naam Istio.
services: application-insights
author: tokaplan
manager: carmonm
ms.service: application-insights
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: alkaplan
ms.openlocfilehash: f3b278c2678542ec127c1c644cc0267622ca39fa
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2019
ms.locfileid: "64870685"
---
# <a name="application-insights-for-kubernetes-with-service-mesh"></a>Application Insights voor Kubernetes met service mesh

> [!IMPORTANT]
> Application Insights voor Kubernetes via service Net is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Azure Monitor maakt nu gebruik van service mesh tech op uw Kubernetes-cluster voor buiten de box-toepassingsbewaking voor elke app die wordt gehost Kubernetes. Met standaard Application Insights-functies zoals [Toepassingsoverzicht](../../azure-monitor/app/app-map.md) als model voor uw afhankelijkheden, [Live Metrics Stream](../../azure-monitor/app/live-stream.md) voor realtime-controle, krachtige visualisaties met de [standaard dashboard](../../azure-monitor/app/overview-dashboard.md), [Metric Explorer](../../azure-monitor/platform/metrics-getting-started.md), en [werkmappen](../../azure-monitor/app/usage-workbooks.md). Met deze functie kunt gebruikers spot knelpunten en fout hotspots in al hun workloads Kubernetes binnen een geselecteerde Kubernetes-naamruimte. Door te profiteren van uw bestaande service mesh-investeringen met technologieën zoals Istio, kan Azure Monitor automatisch geïnstrumenteerd app-controle zonder wijzigingen in de code van uw toepassing.

> [!NOTE]
> Dit is een van de vele manieren om uit te voeren toepassingsbewaking in Kubernetes. U kunt ook een app wordt gehost in Kubernetes met behulp van instrumenteren de [Application Insights-SDK](../../azure-monitor/azure-monitor-app-hub.md) zonder de noodzaak van een service-Net. Kubernetes bewaken zonder het instrumenteren van de toepassing met een SDK kunt u de volgende methode.

## <a name="prerequisites"></a>Vereisten

- Een [Kubernetes-cluster](https://docs.microsoft.com/azure/aks/concepts-clusters-workloads).
- Toegang tot het cluster om uit te voeren console *kubectl*.
- Een [Application Insights-resource](create-new-resource.md)
- Een service-net hebben. Als uw cluster geen Istio geïmplementeerd, leert u hoe u [installeren en gebruiken van Istio in Azure Kubernetes Service](https://docs.microsoft.com/azure/aks/istio-install).

## <a name="capabilities"></a>Functionaliteit

Met behulp van Application Insights voor uw gehoste Kubernetes-app, zich kunt u kunt gebruiken:

- [Toepassingskaart](../../azure-monitor/app/app-map.md)
- [Live Metrics Stream](../../azure-monitor/app/live-stream.md)
- [Dashboards](../../azure-monitor/app/overview-dashboard.md)
- [Metrics Explorer](../../azure-monitor/platform/metrics-getting-started.md)
- [Gedistribueerde tracering](../../azure-monitor/app/distributed-tracing.md)
- [Bewaking van webtoepassingstransacties end-to-end](../../azure-monitor/learn/tutorial-performance.md#identify-slow-server-operations)

## <a name="installation-steps"></a>Installatiestappen

Zodat de oplossing wordt we de volgende stappen uitvoeren:
- Implementeer de toepassing (indien nog niet is geïmplementeerd).
- Zorg ervoor dat de toepassing maakt deel uit van het service-Net.
- Bekijk de verzamelde telemetriegegevens.

### <a name="configure-your-app-to-work-with-a-service-mesh"></a>Uw app om te werken met een service-NET configureren

Istio ondersteunt twee manieren om [instrumenteren een schil](https://istio.io/docs/setup/kubernetes/additional-setup/sidecar-injection/).
In de meeste gevallen is het gemakkelijkst te markeren van de Kubernetes-naamruimte met uw toepassing met de *istio-injectie* label:

```console
kubectl label namespace <my-app-namespace> istio-injection=enabled
```

> [!NOTE]
> Omdat service mesh liften gegevens de kabel buiten, niet wordt versleuteld verkeer onderscheppen. Voor verkeer dat niet u het cluster laat, moet u een niet-versleutelde (bijvoorbeeld HTTP)-protocol gebruiken. Voor extern verkeer die moet worden versleuteld, kunt u overwegen [instellen van SSL-beëindiging](https://kubernetes.io/docs/concepts/services-networking/ingress/#tls) op de controller voor binnenkomend verkeer.

Toepassingen die worden uitgevoerd buiten het NET service worden niet beïnvloed.

### <a name="deploy-your-application"></a>Uw toepassing implementeren

- De toepassing implementeren naar *mijn-app-naamruimte* naamruimte. Als de toepassing al is geïmplementeerd en u de automatische sidecar injectie methode hierboven hebt gevolgd, moet u opnieuw maken schillen om ervoor te zorgen dat istio injects de sidecar; een rolling update starten of afzonderlijke schillen verwijderen en wachten tot deze opnieuw worden gemaakt.
- Zorg ervoor dat uw toepassing voldoet aan de [Istio vereisten](https://istio.io/docs/setup/kubernetes/prepare/requirements/).

### <a name="deploy-application-insights-for-kubernetes"></a>Application Insights voor Kubernetes implementeren

1. Downloaden en uitpakken van een [ *Application Insights voor Kubernetes* release](https://github.com/Microsoft/Application-Insights-Istio-Adapter/releases/).
2. Navigeer naar */src/kubernetes/* in de releasemap.
3. Edit *application-insights-istio-mixer-adapter-deployment.yaml*
    - Bewerk de waarde van *ISTIO_MIXER_PLUGIN_AI_INSTRUMENTATIONKEY* omgevingsvariabele bevat de instrumentatiesleutel van de Application Insights-resource in Azure portal om de telemetrie bevatten.
    - Indien nodig, bewerkt u de waarde van *ISTIO_MIXER_PLUGIN_WATCHLIST_NAMESPACES* omgevingsvariabele naar een door komma's gescheiden lijst met naamruimten waarvoor u wilt inschakelen bewaking bevatten. Laat dit veld leeg voor het bewaken van alle naamruimten.
4. Van toepassing *elke* YAML-bestand te vinden onder *src/kubernetes/* door het uitvoeren van de volgende (u moet nog steeds binnen */src/kubernetes/*):

   ```console
   kubectl apply -f .
   ```

### <a name="verify-application-insights-for-kubernetes-deployment"></a>Controleren of de Application Insights voor Kubernetes-implementatie

- Zorg ervoor dat de dat Application Insights voor Kubernetes-adapter is geïmplementeerd:

  ```console
  kubectl get pods -n istio-system -l "app=application-insights-istio-mixer-adapter"
  ```
> [!NOTE]
> In sommige gevallen is het vereist afstemmen aan te passen. Als u wilt opnemen of uitsluiten van telemetrie voor een afzonderlijke pod uit die worden verzameld, gebruikt u *appinsights/monitoring.enabled* label op deze pod. Dit heeft prioriteit boven de configuratie van alle op basis van een naamruimte. Instellen *appinsights/monitoring.enabled* naar *waar* om op te nemen van de schil en *false* dit uit te sluiten.

### <a name="view-application-insights-telemetry"></a>Application Insights-telemetrie weergeven

- Genereer een voorbeeld van een aanvraag voor uw toepassing om te bevestigen dat bewaking correct werkt.
- Binnen 3 tot 5 minuten, moet u beginnen te zien van de telemetrie weergegeven in de Azure portal. Zorg ervoor dat u uitchecken de *Toepassingsoverzicht* sectie van uw Application Insights-resource in de Portal.

## <a name="troubleshooting"></a>Problemen oplossen

Hieronder wordt het oplossen van problemen met flow moet worden gebruikt als telemetrie niet wordt weergegeven in de Azure portal als verwacht.

1. Zorg ervoor dat de toepassing wordt belast en is verzenden/ontvangen aanvragen in gewone HTTP. Sinds de telemetrie uit de kabel wordt opgeheven, worden versleuteld verkeer wordt niet ondersteund. Als er geen binnenkomende of uitgaande aanvragen zijn, er worden geen telemetrie ofwel.
2. Zorg ervoor dat de juiste instrumentatiesleutel vindt u in de *ISTIO_MIXER_PLUGIN_AI_INSTRUMENTATIONKEY* omgevingsvariabele in *application-insights-istio-mixer-adapter-deployment.yaml*. De instrumentatiesleutel is gevonden op de *overzicht* tabblad van de Application Insights-resource in Azure portal.
3. Zorg ervoor dat de juiste Kubernetes-naamruimte is opgegeven in de *ISTIO_MIXER_PLUGIN_WATCHLIST_NAMESPACES* omgevingsvariabele in *application-insights-istio-mixer-adapter-deployment.yaml*. Laat dit veld leeg voor het bewaken van alle naamruimten.
4. Zorg ervoor dat van uw toepassing schillen zijn sidecar geïnjecteerd door Istio. Controleer of de Istio sidecar op elke schil.

   ```console
   kubectl describe pod -n <my-app-namespace> <my-app-pod-name>
   ```
   Controleer of er een container met de naam *istio-proxy* die worden uitgevoerd op de schil.

5. Weergave *Application Insights voor Kubernetes* traceringen van de adapter.

   ```console
   kubectl get pods -n istio-system -l "app=application-insights-istio-mixer-adapter"
   kubectl logs -n istio-system application-insights-istio-mixer-adapter-<fill in from previous command output>
   ```

   Het aantal ontvangen telemetrie-items wordt elke minuut bijgewerkt. Als deze niet minuut minuut den - is geen telemetrie wordt verzonden naar de adapter door Istio.
   Zoek naar eventuele fouten in het logboek.
6. Als er is vastgesteld dat *Application Insights voor Kubernetes* adapter is niet wordt gevoed telemetrie, controleert u de Istio van Mixer Logboeken om te achterhalen waarom deze niet gegevens naar de adapter verzendt:

   ```console
   kubectl get pods -n istio-system -l "istio=mixer,app=telemetry"
   kubectl logs -n istio-system istio-telemetry-<fill in from previous command output> -c mixer
   ```
   Zoek naar eventuele fouten, met name met betrekking tot communicatie met *applicationinsightsadapter* adapter.

## <a name="faq"></a>Veelgestelde vragen

Voor de meest recente gegevens voor de voortgang op het project, gaat u naar de [Application Insights-adapter voor Istio Mixer-project GitHub](https://github.com/Microsoft/Application-Insights-Istio-Adapter/blob/master/SETUP.md#faq).

## <a name="uninstall"></a>Verwijderen

Verwijderen van het product voor *elke* YAML-bestand te vinden onder *src/kubernetes/* uitvoeren:

```console
kubectl delete -f <filename.yaml>
```


## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over hoe Azure Monitor en containers bezoek samenwerken [Azure Monitor voor containers: overzicht](../../azure-monitor/insights/container-insights-overview.md)