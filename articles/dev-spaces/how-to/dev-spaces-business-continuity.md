---
title: Zakelijke continuïteit en herstel na noodgevallen in Azure Dev spaties | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: lisaguthrie
ms.author: lcozzens
ms.date: 01/28/2019
ms.topic: article
description: Snelle Kubernetes-ontwikkeling met containers en microservices in Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers
manager: jeconnoc
ms.openlocfilehash: 7ecb792e45aeab90f9ae315e5b31247966bcd184
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55199089"
---
# <a name="business-continuity-and-disaster-recovery-in-azure-dev-spaces"></a>Zakelijke continuïteit en herstel na noodgevallen in ruimten voor het ontwikkelen van Azure

## <a name="review-disaster-recovery-guidance-for-azure-kubernetes-service-aks"></a>Lees de richtlijnen voor herstel na noodgevallen voor Azure Kubernetes Service (AKS)

Azure Dev opslagruimten is een functie van Azure Kubernetes Service (AKS). U moet rekening houden met richtlijnen voor herstel na noodgevallen in AKS en overwegen of ze van toepassing zijn op de AKS-clusters die u voor Dev spaties gebruikt. Raadpleeg voor meer informatie [aanbevolen procedures voor zakelijke continuïteit en herstel na noodgevallen in Azure Kubernetes Service (AKS)](https://docs.microsoft.com/azure/aks/operator-best-practices-multi-region)

## <a name="enable-dev-spaces-on-aks-clusters-in-different-regions"></a>Dev-ruimten in AKS-clusters in verschillende regio's inschakelen

Dev-ruimten in AKS-clusters in verschillende regio's inschakelen, kunt u ontwikkelen opslagruimten direct na de storing van een Azure-regio blijven gebruiken.

Raadpleeg voor algemene informatie over implementaties in meerdere regio's van AKS [plannen voor implementatie in meerdere regio's](https://docs.microsoft.com/azure/aks/operator-best-practices-multi-region#plan-for-multi-region-deployment)

Zie voor meer informatie over het implementeren van een AKS-cluster dat compatibel is met Azure Dev spaties [maken van een Kubernetes-cluster met behulp van Azure Cloud Shell](https://docs.microsoft.com/azure/dev-spaces/how-to/create-cluster-cloud-shell)

### <a name="enable-dev-spaces-via-the-azure-portal"></a>Ontwikkel spaties inschakelen via de Azure-portal

Klik op de **Dev spaties** navigatie-item onder de eigenschappen van elk cluster in Azure portal. Kies vervolgens de optie voor het inschakelen van Dev spaties.

![Ontwikkel spaties inschakelen via Azure portal](../media/common/enable-dev-spaces.jpg)

Dit proces herhalen voor elk cluster.

### <a name="enable-dev-spaces-via-the-azure-cli"></a>Ontwikkel spaties via de Azure CLI inschakelen

U kunt ook Dev spaties inschakelen op de opdrachtregel:

```cmd
az aks use-dev-spaces -g <resource group name> -n <cluster name>
```

## <a name="deploy-your-teams-baseline-to-each-cluster"></a>Basislijn van uw team op elk cluster implementeren

Als u werkt met Dev spaties, implementeren u doorgaans de gehele toepassing naar een bovenliggende dev-ruimte op uw Kubernetes-cluster. Standaard de `default` ruimte wordt gebruikt. De eerste implementatie bevat alle services, evenals de externe bronnen die deze services afhankelijk zijn, zoals databases of wachtrijen. Dit staat bekend als de *basislijn*. Nadat u een basislijn ingesteld in de bovenliggende dev-ruimte, kunt u herhalen en fouten opsporen in afzonderlijke services binnen onderliggende dev spaties.

U moet de meest recente versies van uw basislijnset services implementeren met clusters in meerdere regio's. Bijwerken van uw basislijn-services op deze manier zorgt u ervoor dat u ontwikkelen spaties gebruiken blijven kunt als er een fout van de Azure-regio. Bijvoorbeeld, als u uw basislijn via een CI/CD-pijplijn implementeert, de pijplijn zodanig aanpassen dat deze worden geïmplementeerd op meerdere clusters in verschillende regio's.

## <a name="select-the-correct-aks-cluster-to-use-for-dev-spaces"></a>Selecteer het juiste AKS-cluster moet worden gebruikt voor Dev-opslagruimten

Wanneer u een back-cluster met de basislijn van uw team correct hebt geconfigureerd, kunt u snel Schakel over naar de back-cluster op elk gewenst moment. Vervolgens kunt u de afzonderlijke services die u bezig bent in Dev-ruimten opnieuw uitvoeren.

Selecteer een ander cluster met de volgende CLI-opdracht:

```cmd
az aks use-dev-spaces -g <new resource group name> -n <new cluster name>
```

U kunt een lijst van de beschikbare dev-ruimten op het nieuwe cluster met de volgende opdracht:

```cmd
azds space list
```

U kunt maken van een nieuwe dev-ruimte om te werken in of Selecteer een bestaande adresruimte van het ontwikkelen met de volgende opdracht:

```cmd
azds space select -n <space name>
```

Na het uitvoeren van deze opdrachten, wordt de geselecteerde cluster en de dev-ruimte worden gebruikt voor de volgende CLI-bewerkingen, en voor het opsporen van fouten in projecten met behulp van de Visual Studio Code-extensie voor Azure Dev spaties.

Als u Visual Studio gebruikt, kunt u overschakelen van het cluster dat wordt gebruikt door een bestaand project via de volgende stappen uit:

1. Open het project in Visual Studio.
1. Klik met de rechtermuisknop op de naam van het project in Solution Explorer en klikt u op **eigenschappen**
1. Klik in het linkerdeelvenster op **foutopsporing**
1. Klik op de eigenschappenpagina van foutopsporing op de **profiel** vervolgkeuzelijst lijst en kies **Azure Dev spaties**.
1. Klik op de **wijziging** knop.
1. In het dialoogvenster dat wordt weergegeven, selecteert u het AKS-cluster dat u wilt gebruiken. Indien gewenst, kiest u een spatie verschillende ontwikkelen om in te werken of maakt u een nieuwe dev-ruimte, door de juiste optie kiezen uit de **ruimte** vervolgkeuzelijst.

Nadat u de juiste cluster en de ruimte hebt geselecteerd, drukt u op F5 voor het uitvoeren van de service in Dev spaties.

Herhaal deze stappen voor eventuele andere projecten die zijn geconfigureerd voor het gebruik van het oorspronkelijke cluster.

## <a name="access-a-service-on-a-backup-cluster"></a>Toegang tot een service op een back-cluster

Als u uw service voor het gebruik van een openbare DNS-naam hebt geconfigureerd, klikt u vervolgens heeft de service een andere URL als u deze op een back-cluster uitvoert. Openbare DNS-namen zijn altijd in de indeling `<space name>.s.<service name>.<cluster GUID>.<region>.aksapp.io`. Als u naar een ander cluster overschakelt, wordt de GUID van het cluster en mogelijk de regio wijzigen.

Ontwikkel spaties geeft altijd de juiste URL voor de service bij het uitvoeren van `azds up`, of in het uitvoervenster van Visual Studio onder **Azure Dev spaties**.

U kunt ook de URL vinden door te voeren de `azds list-uris` opdracht:
```
$ azds list-uris
Uri                                                     Status
------------------------------------------------------  ---------
http://mywebapi.d05afe7e006a4fddb73c.eastus.aksapp.io/  Available
```

Deze URL gebruiken bij het openen van de service.