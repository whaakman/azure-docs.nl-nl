---
title: Azure Functions op Kubernetes met KEDA
description: Meer informatie over het uitvoeren van Azure Functions in Kubernetes in de cloud of on-premises met KEDA, automatisch schalen op basis van Kubernetes op gebeurtenissen gebaseerde.
services: functions
documentationcenter: na
author: jeffhollan
manager: jeconnoc
keywords: Azure functions, functies, gebeurtenisverwerking, dynamisch berekenen, architectuur zonder server, kubernetes
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: jehollan
ms.openlocfilehash: c82ed7aa841f53f5c81f3281ed1b09926e565e75
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65077618"
---
# <a name="azure-functions-on-kubernetes-with-keda"></a>Azure Functions op Kubernetes met KEDA

De Azure Functions-runtime biedt flexibiliteit voor het hosten van waar en hoe u wilt.  [KEDA](https://github.com/kedacore/kore) (op basis van Kubernetes gebeurtenis aangedreven automatisch schalen) paren naadloos met de Azure Functions-runtime en hulpprogramma's om te schalen op gebeurtenissen gebaseerde in Kubernetes.

## <a name="how-kubernetes-based-functions-work"></a>Hoe Kubernetes op basis van functies werken

De Azure Functions-service bestaat uit twee hoofdonderdelen: een runtime en een scale-controller.  De Functions-runtime wordt uitgevoerd en uw code wordt uitgevoerd.  De runtime, bevat de logica voor het activeren, aanmelden en beheren van de functie-uitvoeringen.  Het andere onderdeel is een scale-controller.  De controller schalen de snelheid van gebeurtenissen die zijn gericht op uw functie wordt gecontroleerd en wordt het aantal exemplaren uitvoeren van uw app proactief geschaald.  Zie voor meer informatie, [Azure Functions-schaal en hosting](functions-scale.md).

Op basis van een Kubernetes-functies in de Functions-runtime biedt een [Docker-container](functions-create-function-linux-custom-image.md) met gebeurtenisgestuurde schalen via KEDA.  KEDA omlaag kunt schalen naar 0-exemplaren (als er geen gebeurtenissen plaatsvinden) en maximaal *n* exemplaren. Dit gebeurt bij het blootstellen van aangepaste metrische gegevens voor het Kubernetes-automatisch schalen (horizontale schillen automatisch schalen).  Met behulp van Functions-containers met KEDA, maakt het mogelijk om te repliceren van de mogelijkheden van de functie zonder server in een Kubernetes-cluster.  Deze functies kunnen ook worden geïmplementeerd met behulp van [Azure Kubernetes-Services (AKS) virtuele knooppunten](../aks/virtual-nodes-cli.md) functie voor de serverloze infrastructuur.

## <a name="managing-keda-and-functions-in-kubernetes"></a>Beheer van KEDA en functies in Kubernetes

Functies op uw Kubernetes-cluster uitgevoerd, moet u het onderdeel KEDA installeren. U kunt installeert dit onderdeel met [Azure Functions Core Tools](functions-run-local.md).

### <a name="installing-with-the-azure-functions-core-tools"></a>Installeren met de Azure Functions Core Tools

Core-hulpprogramma's installeert standaard KEDA en Osiris onderdelen, die ondersteuning bieden voor gebeurtenisafhankelijke en HTTP-schaal, respectievelijk.  Maakt gebruik van de installatie van de `kubectl` die worden uitgevoerd in de huidige context.

KEDA installeren in uw cluster door het uitvoeren van de volgende installatieopdracht:

```cli
func kubernetes install --namespace keda
```

## <a name="deploying-a-function-app-to-kubernetes"></a>Een functie-app implementeren in Kubernetes

U kunt een functie-app implementeren met een Kubernetes-cluster KEDA uitgevoerd.  Omdat uw functies worden uitgevoerd in een Docker-container, moet uw project een `Dockerfile`.  Als deze nog niet bevat, kunt u een docker-bestand met de volgende opdracht in de hoofdmap van uw Functions-project toevoegen:

```cli
func init --docker-only
```

Als u wilt een installatiekopie bouwen en implementeren van uw functies naar Kubernetes, voer de volgende opdracht:

```cli
func kubernetes deploy --name <name-of-function-deployment> --registry <container-registry-username>
```

> Vervang `<name-of-function-deployment>` met de naam van uw functie-app.

Hiermee maakt u een Kubernetes `Deployment` resource, een `ScaledObject` resource, en `Secrets`, waaronder omgevingsvariabelen die zijn geïmporteerd uit uw `local.settings.json` bestand.

## <a name="removing-a-function-app-from-kubernetes"></a>Een functie-app verwijderen van Kubernetes

Nadat een functie implementeren u verwijderen kunt door het verwijderen van de bijbehorende `Deployment`, `ScaledObject`, een `Secrets` gemaakt.

```cli
kubectl delete deploy <name-of-function-deployment>
kubectl delete ScaledObject <name-of-function-deployment>
kubectl delete secret <name-of-function-deployment>
```

## <a name="uninstalling-keda-from-kubernetes"></a>Verwijderen van KEDA van Kubernetes

U kunt de volgende opdracht van de hulpprogramma's core KEDA verwijderen uit een Kubernetes-cluster uitvoeren:

```cli
func kubernetes remove --namespace keda
```

## <a name="supported-triggers-in-keda"></a>Ondersteunde triggers in KEDA

KEDA is momenteel in de bètafase met ondersteuning voor de volgende Azure-functie-triggers:

* [Azure Storage-wachtrijen](functions-bindings-storage-queue.md)
* [Azure Service Bus-wachtrijen](functions-bindings-service-bus.md)
* [HTTP](functions-bindings-http-webhook.md)
* [Apache Kafka](https://github.com/azure/azure-functions-kafka-extension)

## <a name="next-steps"></a>Volgende stappen
Zie de volgende bronnen voor meer informatie:

* [Een functie met een aangepaste installatiekopie maken](functions-create-function-linux-custom-image.md)
* [Azure-functies lokaal programmeren en testen](functions-develop-local.md)
* [De werking van het verbruiksabonnement voor Azure-functie](functions-scale.md)