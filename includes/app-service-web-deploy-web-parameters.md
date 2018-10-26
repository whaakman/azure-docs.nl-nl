---
author: cephalin
ms.service: app-service-web
ms.topic: include
ms.date: 11/03/2016
ms.author: cephalin
ms.openlocfilehash: 5bde217601d27129e044b64d90184727ea717950
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50132963"
---
Met Azure Resource Manager kunt u parameters definiëren voor waarden die u wilt opgeven wanneer de sjabloon wordt geïmplementeerd. De sjabloon bevat een sectie Parameters die alle parameterwaarden bevat.
U moet een parameter voor de waarden die variëren op basis van het project dat u wilt implementeren of op basis van de omgeving die u om te implementeren wilt definiëren. Definieer geen parameters voor waarden die worden altijd hetzelfde blijven. De waarde van elke parameter wordt gebruikt in de sjabloon voor het definiëren van de resources die worden geïmplementeerd. 

Bij het definiëren van parameters, gebruikt u de **allowedValues** aan te geven op welke waarden van een gebruiker kunt opgeven tijdens de implementatie. Gebruik de **defaultValue** veld een waarde toewijzen aan de parameter, als er geen waarde is opgegeven tijdens de implementatie.

Wordt wordt elke parameter in de sjabloon beschreven.

### <a name="sitename"></a>Sitenaam
De naam van de web-app die u wilt maken.

    "siteName":{
      "type":"string"
    }

### <a name="hostingplanname"></a>hostingPlanName
De naam van de App Service-plan te gebruiken voor het hosten van de web-app.

    "hostingPlanName":{
      "type":"string"
    }

### <a name="sku"></a>SKU
De prijscategorie voor het hostingabonnement.

    "sku": {
      "type": "string",
      "allowedValues": [
        "F1",
        "D1",
        "B1",
        "B2",
        "B3",
        "S1",
        "S2",
        "S3",
        "P1",
        "P2",
        "P3",
        "P4"
      ],
      "defaultValue": "S1",
      "metadata": {
        "description": "The pricing tier for the hosting plan."
      }
    }

De sjabloon definieert u de waarden die zijn toegestaan voor deze parameter en een standaardwaarde (S1) toegewezen, als er geen waarde is opgegeven.

### <a name="workersize"></a>workerSize
De exemplaargrootte van het hostingabonnement (klein, normaal of groot).

    "workerSize":{
      "type":"string",
      "allowedValues":[
        "0",
        "1",
        "2"
      ],
      "defaultValue":"0"
    }

De sjabloon definieert u de waarden die zijn toegestaan voor deze parameter (0, 1 of 2), en wijst een standaardwaarde (0) als er geen waarde is opgegeven. De waarden overeenkomen met kleine, middelgrote en grote.

