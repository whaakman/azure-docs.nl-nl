---
title: Aanbevolen procedures Developer - resourcebeheer in Azure Kubernetes Services (AKS)
description: Informatie over de toepassing developer best practices voor resourcebeheer in Azure Kubernetes Service (AKS)
services: container-service
author: zr-msft
ms.service: container-service
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: zarhoads
ms.openlocfilehash: aebade14f3a8a1095925d17325ce99b78031dc32
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58757249"
---
# <a name="best-practices-for-application-developers-to-manage-resources-in-azure-kubernetes-service-aks"></a>Aanbevolen procedures voor ontwikkelaars van toepassingen voor het beheren van resources in Azure Kubernetes Service (AKS)

Als u toepassingen ontwikkelen en in Azure Kubernetes Service (AKS uitvoeren), zijn er enkele belangrijke gebieden te houden. Het beheren van uw implementaties van toepassingen kunnen een negatieve invloed hebben op de gebruikerservaring van services die u opgeeft. Als u wilt gebruiken om te laten slagen, houd er rekening mee enkele aanbevolen procedures kunt u volgen als u toepassingen ontwikkelen en in AKS uitvoeren.

Deze aanbevolen procedures voor richt zich voor het uitvoeren van uw cluster en de werkbelasting van een toepassing developer-perspectief. Zie voor meer informatie over best practices administratieve [operator aanbevolen procedures voor het beheer van isolatie en resource in Azure Kubernetes Service (AKS)-Cluster][operator-best-practices-isolation]. In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Wat zijn de resourceaanvragen pod en limieten
> * Manieren om te ontwikkelen en implementeren van toepassingen met Dev spaties en Visual Studio Code
> * Het gebruik van de `kube-advisor` hulpprogramma om te controleren op problemen met implementaties

## <a name="define-pod-resource-requests-and-limits"></a>Definieer pod resourceaanvragen en -limieten

**Aanbevolen procedurerichtlijn** - ingesteld pod aanvragen en beperkingen met betrekking tot alle schillen in de manifesten YAML. Als de AKS-cluster maakt gebruik van *resourcequota*, uw implementatie kan worden geweigerd als u deze waarden niet definieert.

Er is een primaire manier voor het beheren van de compute-resources binnen een AKS-cluster met pod aanvragen en -limieten. Deze aanvragen en -limieten kunnen de Kubernetes-scheduler weet wat rekenresources die een schil moet worden toegewezen.

* **Pod aanvragen** definiëren van een vaste hoeveelheid CPU en geheugen die de schil nodig heeft. Deze aanvragen moet de hoeveelheid rekenresources die de schil hoeft op te geven van een aanvaardbaar niveau van de prestaties.
    * Wanneer de Kubernetes-scheduler wil een schil op een knooppunt plaatst, wordt de schil-aanvragen worden gebruikt om te bepalen welk knooppunt voldoende bronnen beschikbaar heeft.
    * Bewaak de prestaties van uw toepassing om aan te passen van deze aanvragen om te controleren of dat u minder bronnen die nodig voor het onderhouden van een aanvaardbaar niveau van de prestaties niet definiëren.
* **Pod limieten** zijn de maximale hoeveelheid CPU en geheugen die een schil kunt gebruiken. Deze limieten te voorkomen dat één of twee overmatig schillen te veel CPU en geheugen van het knooppunt. In dit scenario zou verminderen de prestaties van het knooppunt en andere schillen die erop worden uitgevoerd.
    * Stelt u niet een pod-limiet hoger dan de knooppunten kunnen ondersteunen. Elk knooppunt AKS behoudt een vaste hoeveelheid CPU en geheugen voor de belangrijkste onderdelen die Kubernetes. Uw toepassing probeert te verbruiken te veel bronnen op het knooppunt voor andere schillen moeten worden uitgevoerd.
    * Nogmaals, de prestaties van uw toepassing op verschillende tijdstippen gedurende de dag of week controleren. Bepalen wanneer de vraag is, en de schil er gelden limieten voor bronnen die nodig zijn om te voldoen aan de behoeften van de toepassing uitlijnen.

In de specificaties van uw schil is het aanbevolen procedure voor het definiëren van deze aanvragen en -limieten. Als u deze waarden niet opgeeft, niet de Kubernetes-planner te begrijpen welke resources nodig zijn. De scheduler kan de schil op een knooppunt zonder voldoende bronnen zijn voor het opgeven van acceptabele prestaties plannen. Kan de Clusterbeheerder ingesteld *resourcequota* in een naamruimte die moet worden ingesteld van resourceaanvragen en -limieten. Zie voor meer informatie, [resourcequota in AKS clusters][resource-quotas].

Wanneer u een aanvraag van de CPU of de limiet hebt gedefinieerd, wordt de waarde wordt gemeten in eenheden van de CPU. *1.0* CPU komt overeen met één onderliggende virtuele CPU-kern op het knooppunt. Dezelfde meting wordt gebruikt voor GPU's. Ook kunt u een decimale aanvraag of een limiet, doorgaans in millicpu. Bijvoorbeeld, *100 miljoen* is *0.1* van een onderliggende virtuele CPU-kern.

In het volgende eenvoudige voorbeeld voor een enkele NGINX-schil de schil aanvragen *100 miljoen* van CPU-tijd en *128Mi* van het geheugen. De resourcelimieten voor de schil zijn ingesteld op *250 miljoen* CPU en *256Mi* geheugen:

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: mypod
spec:
  containers:
  - name: mypod
    image: nginx:1.15.5
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 250m
        memory: 256Mi
```

Zie voor meer informatie over resource metingen en toewijzingen [beheren rekenbronnen voor containers][k8s-resource-limits].

## <a name="develop-and-debug-applications-against-an-aks-cluster"></a>Ontwikkelen en testen van toepassingen op basis van een AKS-cluster

**Aanbevolen procedurerichtlijn** -Development-teams te implementeren en er foutopsporing op basis van een AKS-cluster met behulp van Dev spaties. Dit ontwikkelingsmodel zorgt ervoor dat de op rollen gebaseerd toegangsbeheer, netwerk- of opslagbehoeften worden geïmplementeerd voordat de app wordt geïmplementeerd naar productie.

Met Azure Dev spaties, ontwikkelen, fouten opsporen en testen van toepassingen rechtstreeks op basis van een AKS-cluster. Ontwikkelaars in een team samenwerken om te bouwen en testen gedurende de levensduur van toepassingen. U kunt echter ook doorgaan met bestaande hulpprogramma's zoals Visual Studio of Visual Studio Code. Een extensie is geïnstalleerd voor Dev spaties waarmee een optie voor het uitvoeren en fouten opsporen in de toepassing in een AKS-cluster:

![Fouten opsporen in toepassingen in een AKS-cluster met opslagruimten ontwikkelen](media/developer-best-practices-resource-management/dev-spaces-debug.png)

Deze geïntegreerde ontwikkelings- en verwerken met Dev spaties vermindert de noodzaak voor lokale testomgevingen, zoals [minikube][minikube]. In plaats daarvan ontwikkelen en testen op basis van een AKS-cluster. Dit cluster kan worden beveiligd en dat staat vermeld in de vorige sectie over het gebruik van naamruimten voor het isoleren van een cluster logisch geïsoleerd. Als uw apps klaar om te implementeren naar productie zijn, kunt u vol vertrouwen omdat het ontwikkelen van apps is uitgevoerd op basis van een echte AKS-cluster implementeren.

## <a name="use-the-visual-studio-code-extension-for-kubernetes"></a>Gebruik de Visual Studio Code-extensie voor Kubernetes

**Aanbevolen procedurerichtlijn** -installeren en gebruiken de VS Code-extensie voor Kubernetes bij het schrijven van YAML-manifesten. U kunt ook de extensie voor geïntegreerde implementatieoplossing waarmee toepassingseigenaren die minder vaak communiceren met het AKS-cluster.

De [Visual Studio Code-extensie voor Kubernetes] [ vscode-kubernetes] helpt u bij het ontwikkelen en implementeren van toepassingen voor AKS. De extensie biedt intellisense voor Kubernetes-resources, en Helm-grafieken en sjablonen. U kunt ook bladeren, implementeren en Kubernetes-resources van VS Code bewerken. De extensie biedt een intellisense-controle voor resourceaanvragen ook of beperkt in de specificaties pod wordt ingesteld:

![VS Code-extensie voor Kubernetes-waarschuwing over ontbrekende geheugenlimieten](media/developer-best-practices-resource-management/vs-code-kubernetes-extension.png)

## <a name="regularly-check-for-application-issues-with-kube-advisor"></a>Regelmatig controleren op problemen in toepassingen met kube-advisor

**Aanbevolen procedurerichtlijn** -Voer regelmatig de nieuwste versie van `kube-advisor` open source-hulpprogramma om problemen te detecteren in uw cluster. Als u resourcequota op een bestaand AKS-cluster toepast, voert u `kube-advisor` eerst te vinden schillen waarvoor geen resourceaanvragen en -limieten die zijn gedefinieerd.

De [kube-advisor] [ kube-advisor] hulpprogramma is een bijbehorende AKS open source-project, die een Kubernetes-cluster wordt gescand en rapporten weergeven over problemen die worden gevonden. Een nuttige controle is het identificeren van schillen die niet over een bron aanvragen en -limieten in plaats.

In een AKS-cluster die als host fungeert voor veel development-teams en toepassingen, kan het lastig zijn om bij te houden van schillen zonder deze resource-aanvragen en set beperkt. Als een best practice regelmatig uitgevoerd `kube-advisor` op uw AKS-clusters.

## <a name="next-steps"></a>Volgende stappen

Deze aanbevolen procedures voor gericht op het uitvoeren van uw cluster en werkbelastingen vanuit het oogpunt operator cluster. Zie voor meer informatie over best practices administratieve [operator aanbevolen procedures voor het beheer van isolatie en resource in Azure Kubernetes Service (AKS)-Cluster][operator-best-practices-isolation].

Voor het implementeren van sommige van deze aanbevolen procedures, Zie de volgende artikelen:

* [Ontwikkelen met Dev-opslagruimten][dev-spaces]
* [Controleren op problemen met kube-advisor][aks-kubeadvisor]

<!-- EXTERNAL LINKS -->
[k8s-resource-limits]: https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/
[vscode-kubernetes]: https://github.com/Azure/vscode-kubernetes-tools
[kube-advisor]: https://github.com/Azure/kube-advisor
[minikube]: https://kubernetes.io/docs/setup/minikube/

<!-- INTERNAL LINKS -->
[aks-kubeadvisor]: kube-advisor-tool.md
[dev-spaces]: ../dev-spaces/get-started-netcore.md
[operator-best-practices-isolation]: operator-best-practices-cluster-isolation.md
[resource-quotas]: operator-best-practices-scheduler.md#enforce-resource-quotas
