---
title: Azure-container register opslagplaatsen | Microsoft Docs
description: "Het gebruik van Azure Container register-opslagplaatsen voor Docker-installatiekopieën"
services: container-registry
documentationcenter: 
author: cristy
manager: balans
editor: dlepow
ms.service: container-registry
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/22/2017
ms.author: cristyg
ms.openlocfilehash: 2090d4c951e2261529bf1b7b361510d5822060a5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-container-registry-repositories"></a>Register-opslagplaatsen voor Azure-container

Azure Container registers zijn compatibel met een groot aantal services en orchestrators. We zijn begonnen met de Docker-header-veld in het Docker.config-bestand voor het bijhouden van de bron-services en -agenten waaruit ACR wordt gebruikt te vereenvoudigen.



## <a name="viewing-repositories-in-the-portal"></a>Opslagplaatsen weergeven in de Portal

De headers ACR volgt u de indeling:
```
X-Meta-Source-Client: <cloud>/<service>/<optionalservicename>
```

* Cloud: Azure, Azure-Stack of andere government of land-specifieke Azure-clouds. Hoewel Azure Stack en government clouds worden momenteel niet ondersteund, schakelt u deze parameter toekomstige ondersteuning.
* Service: de naam van de service.
* Optionalservicename: een optionele parameter voor services met subservices of om op te geven van een SKU (ex: web-apps overeenkomen met de Azure/app-service/web-apps).

Partnerservices en orchestrators aangemoedigd specifieke headerwaarden gebruiken om u te helpen met onze telemetrie. Gebruikers kunnen ook de waarde die is doorgegeven aan de header desgewenst wijzigen.

De waarden willen we ACR-partners gebruiken om te vullen van het veld 'X-Meta-bron-Client' worden hieronder:

| Naam service              | Koptekst                                |
| ------------------------- | ------------------------------------- |
| Azure Container Service   | Azure/compute/azure-container-service |
| App Service - Web-Apps    | Azure /-/ web-apps van app service            |
| App Service - Logic Apps  | Azure /-/ logic-apps van app service          |
| Batch                     | compute-Azure/batch                   |
| Cloud-Console             | Azure-cloud-console                   |
| Functies                 | compute-Azure-functies               |
| Internet der dingen - Hub  | iot-Azure-hub                         |
| HDInsight                 | data-Azure/hdinsight                  |
| Jenkins                   | Azure/jenkins                         |
| Machine Learning          | Azure/data/machile-learning           |
| Service Fabric            | Azure/compute/service-infrastructuur          |
| VSTS                      | Azure/vsts                            |


## <a name="next-steps"></a>Volgende stappen
[Meer informatie over registers en de ondersteunde services orchestrators](container-registry-intro.md)
