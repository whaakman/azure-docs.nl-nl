---
title: Azure Machine Learning-implementatie oplossen | Microsoft Docs
description: Gids voor probleemoplossing voor implementatie en het maken van de service
services: machine-learning
author: raymondl
ms.author: raymondl
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 11/16/2017
ms.openlocfilehash: 8eafb16abeb939a16b1ddb024853300c453bcd9a
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2017
---
# <a name="troubleshooting-service-deployment-and-environment-setup"></a>Problemen met implementatie van de service en de omgeving in te stellen
De volgende informatie kunt de oorzaak van fouten bij het instellen van de model-omgeving.

## <a name="model-management-environment"></a>Model beheeromgeving
### <a name="contributor-permission-required"></a>Inzender toestemming vereist
Moet u bijdrager toegang tot het abonnement of de resourcegroep voor het instellen van een cluster voor de implementatie van uw webservices.

### <a name="resource-availability"></a>Beschikbaarheid van resources
U moet voldoende bronnen beschikbaar zijn in uw abonnement hebben, zodat u de omgeving-resources kunt inrichten.

### <a name="subscription-caps"></a>Abonnement Caps
Uw abonnement mogelijk een limiet op de facturering die kan verhinderen dat u de omgeving-resources inrichten. Verwijder die cap voor inrichting inschakelen.

### <a name="enable-debug-and-verbose-options"></a>Foutopsporing en uitgebreide opties inschakelen
Gebruik de `--debug` en `--verbose` vlaggen in de setup-opdracht om weer te geven informatie over foutopsporing en tracering als de omgeving wordt ingericht.

```
az ml env setup -l <location> -n <name> -c --debug --verbose 
```

## <a name="service-deployment"></a>Service-implementatie
De volgende informatie kunt de oorzaak van fouten tijdens de implementatie of bij het aanroepen van de webservice.

### <a name="service-logs"></a>Service-Logboeken
De `logs` optie van de CLI-service biedt logboekgegevens van Docker en Kubernetes.

```
az ml service logs realtime -i <web service id>
```

Voor extra logboekinstellingen, gebruikt u de `--help` (of `-h`) optie.

```
az ml service logs realtime -h
```

### <a name="debug-and-verbose-options"></a>Opties voor foutopsporing en uitgebreid
Gebruik de `--debug` vlag foutopsporingslogboeken weergegeven omdat de service wordt geïmplementeerd.

```
az ml service create realtime -m <modelfile>.pkl -f score.py -n <service name> -r python --debug
```

Gebruik de `--verbose` vlag voor aanvullende informatie zoals de service wordt geïmplementeerd.

```
az ml service create realtime -m <modelfile>.pkl -f score.py -n <service name> -r python --verbose
```

### <a name="enable-request-logging-in-app-insights"></a>Aanvraag in de App Insights logboekregistratie inschakelen
Stel de `-l` vlag op true bij het maken van een webservice voor het inschakelen van logboekregistratie op aanvraag. De aanvraag-Logboeken geschreven naar de App Insights-exemplaar voor uw omgeving in Azure. Zoeken naar dit exemplaar met de naam van de omgeving die u gebruikt als u de `az ml env setup` opdracht.

- Stel `-l` op true bij het maken van de service.
- Open de App Insights in Azure-portal. Met de omgevingsnaam van uw kunt u het exemplaar van de App Insights.
- Eenmaal App Insights, klik op Zoeken in het bovenste menu om de resultaten weer te geven.
- Of Ga naar `Analytics`  >  `Exceptions`  >  `exceptions take | 10`.


### <a name="add-error-handling-in-scoring-script"></a>Fout tijdens verwerken van in score berekenen voor script toevoegen
Gebruik afhandeling van uitzonderingen in uw `scoring.py` van script **uitvoeren** functie voor het retourneren van het foutbericht wordt weergegeven als onderdeel van de uitvoer van uw web-service.

Python-voorbeeld:
```
    try:
        <code to load model and score>
   except Exception as e:
        return(str(e))
```

## <a name="other-common-problems"></a>Andere veelvoorkomende problemen
- Als pip-installatie van azure cli ml met de fout mislukt `cannot find the path specified` op een Windows-machine, moet u lang pad ondersteuning inschakelen. Zie https://blogs.msdn.microsoft.com/jeremykuhne/2016/07/30/net-4-6-2-and-long-paths-on-windows-10/. 
- Als de `env setup` mislukt met de opdracht `LocationNotAvailableForResourceType`, gebruikt u waarschijnlijk de verkeerde locatie (regio) voor de machine learning-resources. Zorg ervoor dat de locatie die is opgegeven met de `-l` parameter `eastus2`, `westcentralus`, of `australiaeast`.
- Als de `env setup` mislukt met de opdracht `Resource quota limit exceeded`, Controleer of u voldoende kernen beschikbaar in uw abonnement hebt en dat uw resources niet worden gebruikt boven in andere processen.
- Als de `env setup` mislukt met de opdracht `Invalid environment name. Name must only contain lowercase alphanumeric characters`, zorg ervoor dat de servicenaam geen hoofdletters, symbolen of het onderstrepingsteken (_) (zoals in *my_environment*).
- Als de `service create` mislukt met de opdracht `Service Name: [service_name] is invalid. The name of a service must consist of lower case alphanumeric characters (etc.)`, Controleer of de servicenaam tussen 3 en 32 tekens lang is; begint en eindigt op kleine letters; en bevat geen hoofdletters, symbolen dan koppelteken (-) en periode ( . ), of het onderstrepingsteken (_) (zoals in *my_webservice*).
- Probeer het opnieuw als u krijgt een `502 Bad Gateway` fout bij het aanroepen van de webservice. Normaal gesproken betekent dit dat de container is niet aan het cluster nog geïmplementeerd.
- Als u krijgt `CrashLoopBackOff` fout bij het maken van een service, Raadpleeg de logboeken. Dit is meestal het resultaat van ontbrekende afhankelijkheden in de **init** functie.
