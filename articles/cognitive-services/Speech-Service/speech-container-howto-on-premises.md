---
title: Kubernetes on-premises gebruiken
titleSuffix: Azure Cognitive Services
description: Met behulp van Kubernetes en Helm voor het definiëren van de spraak-naar-tekst en spraak containerinstallatiekopieën, maken we een Kubernetes-pakket. Dit pakket wordt geïmplementeerd op een Kubernetes-cluster on-premises.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 7/10/2019
ms.author: dapine
ms.openlocfilehash: 33d9de956a6d43145fc68f4ec46b09b8e8bf0188
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67786241"
---
# <a name="use-kubernetes-on-premises"></a>Kubernetes on-premises gebruiken

Met behulp van Kubernetes en Helm voor het definiëren van de spraak-naar-tekst en spraak containerinstallatiekopieën, maken we een Kubernetes-pakket. Dit pakket wordt geïmplementeerd op een Kubernetes-cluster on-premises. Tot slot wordt beschreven hoe de geïmplementeerde services en verschillende configuratieopties te testen.

## <a name="prerequisites"></a>Vereisten

U moet voldoen aan de volgende vereisten voordat u met spraak containers on-premises:

|Verplicht|Doel|
|--|--|
| Azure-Account | Als u nog geen abonnement op Azure hebt, maak dan een [gratis account][free-azure-account] aan voordat u begint. |
| Toegang tot container Registry | In de volgorde voor Kubernetes voor het ophalen van de docker-installatiekopieën in het cluster, moet deze toegang tot het containerregister. U moet [toegang aanvragen tot de container registry][speech-preview-access] eerste. |
| Kubernetes CLI | De [Kubernetes CLI][kubernetes-cli] is vereist voor het beheren van de gedeelde referenties van het containerregister. Kubernetes is bovendien vereist voordat Helm, dit het pakketbeheerprogramma voor Kubernetes is. |
| Helm CLI | Als onderdeel van de [Helm CLI][helm-install] install, you'll also need to initialize Helm which will install [Tiller][tiller-install]. |
|Spraak-resource |Als u wilt deze containers gebruiken, moet u het volgende hebben:<br><br>Een _spraak_ Azure-resource om de bijbehorende facturering sleutel en facturering URI van het eindpunt te verkrijgen. Beide waarden zijn beschikbaar op de Azure-portal **spraak** overzicht en sleutels pagina's en zijn vereist voor het starten van de container.<br><br>**{API_KEY}** : bronsleutel<br><br>**{ENDPOINT_URI}** : voorbeeld van de eindpunt-URI is: `https://westus.api.cognitive.microsoft.com/sts/v1.0`|

## <a name="the-recommended-host-computer-configuration"></a>De configuratie van de aanbevolen host-computer

Raadpleeg de [Spraakservice container hostcomputer][speech-container-host-computer] details als uitgangspunt. Dit *helm-diagram* berekent automatisch CPU en geheugen vereisten op basis van hoeveel decodeert (gelijktijdige aanvragen) die de gebruiker opgeeft. Bovendien wordt aangepast op basis van of optimalisaties voor audio/tekstinvoer zijn geconfigureerd als `enabled`. Het helm-grafiek standaard ingesteld op, twee gelijktijdige aanvragen en optimalisatie van uitschakelen.

| Service | CPU / Container | Geheugen / Container |
|--|--|--|
| **Spraak-naar-tekst** | een decoder vereist een minimum van 1.150 millicores. Als de `optimizedForAudioFile` is ingeschakeld, en vervolgens 1,950 millicores vereist zijn. (standaard: twee decoders) | Vereist: 2 GB<br>Beperkt:  4 GB |
| **Tekst naar spraak** | een gelijktijdige aanvraag vereist een minimum van 500 millicores. Als de `optimizeForTurboMode` is ingeschakeld, dan 1000 millicores vereist zijn. (standaard: twee gelijktijdige aanvragen) | Vereist: 1 GB<br> Beperkt: 2 GB |

## <a name="connect-to-the-kubernetes-cluster"></a>Verbinding maken met het Kubernetes-cluster

De hostcomputer wordt verwacht dat een beschikbare Kubernetes-cluster. In deze zelfstudie bekijken op [een Kubernetes-cluster implementeren](../../aks/tutorial-kubernetes-deploy-cluster.md) voor een conceptueel begrip van hoe u een Kubernetes-cluster implementeert op een hostcomputer.

### <a name="sharing-docker-credentials-with-the-kubernetes-cluster"></a>Docker-referenties delen met het Kubernetes-cluster

Om toe te staan van het Kubernetes-cluster aan `docker pull` de geconfigureerde installatiekopieën uit de `containerpreview.azurecr.io` containerregister, moet u het overdragen van de docker-referenties in het cluster. Uitvoeren van de [ `kubectl create` ][kubectl-create] onderstaande opdracht maken een *docker-register geheim* op basis van de referenties die zijn opgegeven in de vereiste container registry toegang.

Voer de volgende opdracht uit via de opdrachtregelinterface van keuze. Vervang de `<username>`, `<password>`, en `<email-address>` met de referenties van de container-register.

```console
kubectl create secret docker-registry containerpreview \
    --docker-server=containerpreview.azurecr.io \
    --docker-username=<username> \
    --docker-password=<password> \
    --docker-email=<email-address>
```

> [!NOTE]
> Als u al toegang tot hebt de `containerpreview.azurecr.io` containerregister, kunt u een Kubernetes-geheim met behulp van de algemene vlag in plaats daarvan. Houd rekening met de volgende opdracht uit die wordt uitgevoerd op basis van de configuratie van uw Docker JSON.
> ```console
>  kubectl create secret generic containerpreview \
>      --from-file=.dockerconfigjson=~/.docker/config.json \
>      --type=kubernetes.io/dockerconfigjson
> ```

De volgende uitvoer wordt weergegeven in de console wanneer de geheime sleutel is gemaakt.

```console
secret "containerpreview" created
```

Uitvoeren om te controleren of het geheim is gemaakt, de [ `kubectl get` ][kubectl-get] met de `secrets` vlag.

```console
kuberctl get secrets
```

Uitvoeren van de `kubectl get secrets` alle geconfigureerde geheimen worden afgedrukt.

```console
NAME                  TYPE                                  DATA      AGE
containerpreview      kubernetes.io/dockerconfigjson        1         30s
```

## <a name="configure-helm-chart-values-for-deployment"></a>Waarden van de Helm-grafiek voor de implementatie configureren

Ga naar de [Microsoft Helm Hub][ms-helm-hub] voor alle algemeen beschikbare helm-grafieken die worden aangeboden door Microsoft. Uit de Microsoft-Helm-Hub, vindt u de **Cognitive Services spraak On-Premises grafiek**. De **Cognitive Services spraak On-Premises** is het diagram we installeren, maar we moet eerst maken een `config-values.yaml` bestand met expliciete configuraties. Laten we beginnen door de Microsoft-bibliotheek toe te voegen aan onze Helm-exemplaar.

```console
helm repo add microsoft https://microsoft.github.io/charts/repo
```

Vervolgens configureert we de waarden van onze Helm-grafiek. Kopieer en plak de volgende YAML in een bestand met de naam `config-values.yaml`. Voor meer informatie over het aanpassen van de **Cognitive Services spraak On-Premises Helm-diagram**, Zie [helm-grafieken aanpassen](#customize-helm-charts). Vervang de `billing` en `apikey` waarden door uw eigen.

```yaml
# These settings are deployment specific and users can provide customizations

# speech-to-text configurations
speechToText:
  enabled: true
  numberOfConcurrentRequest: 3
  optimizeForAudioFile: true
  image:
    registry: containerpreview.azurecr.io
    repository: microsoft/cognitive-services-speech-to-text
    tag: latest
    pullSecrets:
      - containerpreview # Or an existing secret
    args:
      eula: accept
      billing: # < Your billing URL >
      apikey: # < Your API Key >

# text-to-speech configurations
textToSpeech:
  enabled: true
  numberOfConcurrentRequest: 3
  optimizeForTurboMode: true
  image:
    registry: containerpreview.azurecr.io
    repository: microsoft/cognitive-services-text-to-speech
    tag: latest
    pullSecrets:
      - containerpreview # Or an existing secret
    args:
      eula: accept
      billing: # < Your billing URL >
      apikey: # < Your API Key >
```

> [!IMPORTANT]
> Als de `billing` en `apikey` waarden zijn niet opgegeven, de services verloopt na 15 minuten. Evenzo, mislukt de verificatie als de services niet meer beschikbaar.

### <a name="the-kubernetes-package-helm-chart"></a>De Kubernetes-pakket (Helm-diagram)

De *Helm-diagram* bevat de configuratie van die installatiekopieën van docker om op te halen uit de `containerpreview.azurecr.io` containerregister.

> Een [Helm-diagram][helm-charts] is een verzameling van bestanden die worden beschreven van een gerelateerde set Kubernetes-resources. Een grafiek kan worden gebruikt voor het implementeren iets eenvoudigs, zoals een memcached-schil of iets complex zijn, zoals een volledige web-app-stack met HTTP-servers, databases, caches, enzovoort.

De opgegeven *Helm-grafieken* pull-docker-installatiekopieën van de spraak-Service, tekst naar spraak en de spraak-naar-tekst-services van zowel de `containerpreview.azurecr.io` containerregister.

## <a name="install-the-helm-chart-on-the-kubernetes-cluster"></a>Het Helm-diagram installeren op de Kubernetes-cluster

Voor het installeren van de *helm-diagram* we om uit te voeren, moet de [ `helm install` ][helm-install-cmd] opdracht, vervangt de `<config-values.yaml>` met het juiste pad en de bestandsnaam de naam van argument. De `microsoft/cognitive-services-speech-onpremise` Helm-diagram waarnaar wordt verwezen hieronder vindt u op de [Microsoft Helm Hub hier][ms-helm-hub-speech-chart].

```console
helm install microsoft/cognitive-services-speech-onpremise \
    --version 0.1.0 \
    --values <config-values.yaml> \
    --name onprem-speech
```

Hier volgt een voorbeeld van uitvoer u had verwacht om te zien van de uitvoering van een geslaagde installatie:

```console
NAME:   onprem-speech
LAST DEPLOYED: Tue Jul  2 12:51:42 2019
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/Pod(related)
NAME                             READY  STATUS             RESTARTS  AGE
speech-to-text-7664f5f465-87w2d  0/1    Pending            0         0s
speech-to-text-7664f5f465-klbr8  0/1    ContainerCreating  0         0s
text-to-speech-56f8fb685b-4jtzh  0/1    ContainerCreating  0         0s
text-to-speech-56f8fb685b-frwxf  0/1    Pending            0         0s

==> v1/Service
NAME            TYPE          CLUSTER-IP    EXTERNAL-IP  PORT(S)       AGE
speech-to-text  LoadBalancer  10.0.252.106  <pending>    80:31811/TCP  1s
text-to-speech  LoadBalancer  10.0.125.187  <pending>    80:31247/TCP  0s

==> v1beta1/PodDisruptionBudget
NAME                                MIN AVAILABLE  MAX UNAVAILABLE  ALLOWED DISRUPTIONS  AGE
speech-to-text-poddisruptionbudget  N/A            20%              0                    1s
text-to-speech-poddisruptionbudget  N/A            20%              0                    1s

==> v1beta2/Deployment
NAME            READY  UP-TO-DATE  AVAILABLE  AGE
speech-to-text  0/2    2           0          0s
text-to-speech  0/2    2           0          0s

==> v2beta2/HorizontalPodAutoscaler
NAME                       REFERENCE                  TARGETS        MINPODS  MAXPODS  REPLICAS  AGE
speech-to-text-autoscaler  Deployment/speech-to-text  <unknown>/50%  2        10       0         0s
text-to-speech-autoscaler  Deployment/text-to-speech  <unknown>/50%  2        10       0         0s


NOTES:
cognitive-services-speech-onpremise has been installed!
Release is named onprem-speech
```

De Kubernetes-implementatie kunt over enkele minuten duren. Om te bevestigen dat zowel schillen en services goed geïmplementeerd en beschikbaar zijn, voert u de volgende opdracht uit:

```console
kubectl get all
```

U kunt verwachten iets soortgelijks als in de volgende uitvoer:

```console
NAME                                  READY     STATUS    RESTARTS   AGE
pod/speech-to-text-7664f5f465-87w2d   1/1       Running   0          34m
pod/speech-to-text-7664f5f465-klbr8   1/1       Running   0          34m
pod/text-to-speech-56f8fb685b-4jtzh   1/1       Running   0          34m
pod/text-to-speech-56f8fb685b-frwxf   1/1       Running   0          34m

NAME                     TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)        AGE
service/kubernetes       ClusterIP      10.0.0.1       <none>           443/TCP        3h
service/speech-to-text   LoadBalancer   10.0.252.106   52.162.123.151   80:31811/TCP   34m
service/text-to-speech   LoadBalancer   10.0.125.187   65.52.233.162    80:31247/TCP   34m

NAME                             DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/speech-to-text   2         2         2            2           34m
deployment.apps/text-to-speech   2         2         2            2           34m

NAME                                        DESIRED   CURRENT   READY     AGE
replicaset.apps/speech-to-text-7664f5f465   2         2         2         34m
replicaset.apps/text-to-speech-56f8fb685b   2         2         2         34m

NAME                                                            REFERENCE                   TARGETS   MINPODS   MAXPODS   REPLICAS   AGE
horizontalpodautoscaler.autoscaling/speech-to-text-autoscaler   Deployment/speech-to-text   1%/50%    2         10        2          34m
horizontalpodautoscaler.autoscaling/text-to-speech-autoscaler   Deployment/text-to-speech   0%/50%    2         10        2          34m
```

### <a name="verify-helm-deployment-with-helm-tests"></a>Helm-implementatie met Helm tests controleren

De geïnstalleerde Helm-grafieken definiëren *Helm tests*, die dienen als nuttig voor verificatie. Deze tests valideren gereedheid van de service. Om te controleren of beide **spraak-naar-tekst** en **tekst naar spraak** services, die we u uitvoeren de [Helm test][helm-test] opdracht.

```console
helm test onprem-speech
```

> [!IMPORTANT]
> Deze tests mislukken als de status van de schil niet is `Running` of als de implementatie niet wordt vermeld onder de `AVAILABLE` kolom. Patiënt zijn omdat dit meer dan tien minuten duren kan om uit te voeren.

Deze tests worden verschillende statusresultaten uitvoer:

```console
RUNNING: speech-to-text-readiness-test
PASSED: speech-to-text-readiness-test
RUNNING: text-to-speech-readiness-test
PASSED: text-to-speech-readiness-test
```

Als alternatief voor het uitvoeren van de *helm tests*, u kunt verzamelen, het *extern IP-adres* adressen en bijbehorende poorten van de `kubectl get all` opdracht. Met behulp van het IP- en de poort, open een webbrowser en navigeer naar `http://<external-ip>:<port>:/swagger/index.html` naar de API swagger pagina('s) weergave.

## <a name="customize-helm-charts"></a>Helm-grafieken aanpassen

Helm-grafieken zijn hiërarchische gegevens. Diagram overname, kunt u hiërarchische wordt deze ook caters met het concept van specifieke, waar de instellingen die specifiek overgenomen regels overschrijven.

[!INCLUDE [Speech umbrella-helm-chart-config](includes/speech-umbrella-helm-chart-config.md)]

[!INCLUDE [Speech-to-Text Helm Chart Config](includes/speech-to-text-chart-config.md)]

[!INCLUDE [Text-to-Speech Helm Chart Config](includes/text-to-speech-chart-config.md)]

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over het installeren van toepassingen met Helm in Azure Kubernetes Service (AKS) [vindt u hier][installing-helm-apps-in-aks].

> [!div class="nextstepaction"]
> [Cognitive Services-Containers][cog-svcs-containers]

<!-- LINKS - external -->
[free-azure-account]: https://azure.microsoft.com/free
[git-download]: https://git-scm.com/downloads
[azure-cli]: https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest
[docker-engine]: https://www.docker.com/products/docker-engine
[kubernetes-cli]: https://kubernetes.io/docs/tasks/tools/install-kubectl
[helm-install]: https://helm.sh/docs/using_helm/#installing-helm
[helm-install-cmd]: https://helm.sh/docs/helm/#helm-install
[tiller-install]: https://helm.sh/docs/install/#installing-tiller
[helm-charts]: https://helm.sh/docs/developing_charts
[speech-preview-access]: https://aka.ms/speechcontainerspreview
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[helm-test]: https://helm.sh/docs/helm/#helm-test
[ms-helm-hub]: https://hub.helm.sh/charts/microsoft
[ms-helm-hub-speech-chart]: https://hub.helm.sh/charts/microsoft/cognitive-services-speech-onpremise

<!-- LINKS - internal -->
[speech-container-host-computer]: speech-container-howto.md#the-host-computer
[installing-helm-apps-in-aks]: ../../aks/kubernetes-helm.md
[cog-svcs-containers]: ../cognitive-services-container-support.md
