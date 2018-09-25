---
title: Problemen oplossen met Azure Machine Learning-implementatie | Microsoft Docs
description: Gids voor probleemoplossing voor implementatie en het maken van services
services: machine-learning
author: aashishb
ms.author: aashishb
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 11/16/2017
ROBOTS: NOINDEX
ms.openlocfilehash: a2867545e454e2b13360d87f5282e684753d6476
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46994493"
---
# <a name="troubleshooting-service-deployment-and-environment-setup"></a>Het oplossen van service-implementatie en installatie van de omgeving

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]

De volgende informatie kan helpen de oorzaak van fouten bij het instellen van de model-management-omgeving.

## <a name="model-management-environment"></a>Modelbeheeromgeving
### <a name="contributor-permission-required"></a>Inzender voor machtiging is vereist
U moet Inzender-toegang tot het abonnement of de resourcegroep voor het instellen van een cluster voor de implementatie van uw webservices.

### <a name="resource-availability"></a>Beschikbaarheid van resources
U moet voldoende beschikbare resources in uw abonnement hebt, zodat u kunt de omgevingsresources inrichten.

### <a name="subscription-caps"></a>Limieten voor abonnement
Uw abonnement mogelijk een bovengrens voor facturering die kan voorkomen dat u de inrichting van de omgevingsresources. Verwijder die cap om in te schakelen wordt ingericht.

### <a name="enable-debug-and-verbose-options"></a>Foutopsporing en uitgebreide opties inschakelen
Gebruik de `--debug` en `--verbose` markeringen in de setup-opdracht om weer te geven informatie over foutopsporing en tracering als de omgeving wordt ingericht.

```
az ml env setup -l <location> -n <name> -c --debug --verbose 
```

## <a name="service-deployment"></a>Service-implementatie
De volgende informatie kunt u eenvoudiger bepalen van de oorzaak van fouten tijdens de implementatie of bij het aanroepen van de webservice.

### <a name="service-logs"></a>Service-Logboeken
De `logs` -optie van de CLI-service kunt u logboekgegevens van Docker en Kubernetes.

```
az ml service logs realtime -i <web service id>
```

Voor extra logboekinstellingen, gebruikt u de `--help` (of `-h`) optie.

```
az ml service logs realtime -h
```

### <a name="debug-and-verbose-options"></a>Opties voor foutopsporing en uitgebreid
Gebruik de `--debug` vlag om weer te geven van Logboeken voor foutopsporing als de service wordt geïmplementeerd.

```
az ml service create realtime -m <modelfile>.pkl -f score.py -n <service name> -r python --debug
```

Gebruik de `--verbose` vlag voor aanvullende informatie zoals de service wordt geïmplementeerd.

```
az ml service create realtime -m <modelfile>.pkl -f score.py -n <service name> -r python --verbose
```

### <a name="enable-request-logging-in-app-insights"></a>Aanvraag in de App Insights logboekregistratie inschakelen
Stel de `-l` vlag op true als het maken van een webservice om in te schakelen logboekregistratie op de aanvraag. De aanvraag-logboeken worden geschreven naar de App Insights-exemplaar voor uw omgeving in Azure. Zoeken naar dit exemplaar met behulp van de naam van de omgeving die u hebt gebruikt bij het gebruik van de `az ml env setup` opdracht.

- Stel `-l` op true als het maken van de service.
- Open de App Insights in Azure portal. Met de omgevingsnaam van uw kunt u de App Insights-exemplaar.
- Eenmaal in de App Insights, klik op Zoeken in het bovenste menu om de resultaten weer te geven.
- Of Ga naar `Analytics`  >  `Exceptions`  >  `exceptions take | 10`.


### <a name="add-error-handling-in-scoring-script"></a>Foutafhandeling in het scoring-script toevoegen
Gebruik afhandeling van uitzonderingen in uw `scoring.py` van script **uitvoeren** functie voor het retourneren van het foutbericht wordt weergegeven als onderdeel van de uitvoer van uw web-service.

Python-voorbeeld:
```
    try:
        <code to load model and score>
   except Exception as e:
        return(str(e))
```

## <a name="other-common-problems"></a>Andere veelvoorkomende problemen
- Als pip-installatie van azure-cli-ml is mislukt met de fout `cannot find the path specified` op een Windows-machine, moet u ondersteuning voor lange-pad. Zie https://blogs.msdn.microsoft.com/jeremykuhne/2016/07/30/net-4-6-2-and-long-paths-on-windows-10/. 
- Als de `env setup` opdracht is mislukt met `LocationNotAvailableForResourceType`, gebruikt u waarschijnlijk de verkeerde locatie (regio) voor de machine learning-resources. Zorg ervoor dat de locatie die is opgegeven met de `-l` parameter `eastus2`, `westcentralus`, of `australiaeast`.
- Als de `env setup` opdracht is mislukt met `Resource quota limit exceeded`, zorg ervoor dat u hebt onvoldoende cores beschikbaar zijn in uw abonnement en dat uw resources niet worden gebruikt u in andere processen.
- Als de `env setup` opdracht is mislukt met `Invalid environment name. Name must only contain lowercase alphanumeric characters`, zorg ervoor dat de naam van de service bevat geen hoofdletters, symbolen of het onderstrepingsteken (_) (zoals in *my_environment*).
- Als de `service create` opdracht is mislukt met `Service Name: [service_name] is invalid. The name of a service must consist of lower case alphanumeric characters (etc.)`, zorg ervoor dat de naam van de service is tussen 3 en 32 tekens lang; begint en eindigt met kleine letters, alfanumerieke tekens; en bevat geen hoofdletters, symbolen dan het koppelteken (-) en de periode ( . ), of het onderstrepingsteken (_) (zoals in *my_webservice*).
- Probeer het opnieuw als er een `502 Bad Gateway` fout bij het aanroepen van de webservice. Normaal gesproken betekent dit dat de container nog niet is geïmplementeerd in het cluster nog.
- Als er `CrashLoopBackOff` fout bij het maken van een service, Raadpleeg de logboeken. Dit is meestal het resultaat van de ontbrekende afhankelijkheden in de **init** functie.
