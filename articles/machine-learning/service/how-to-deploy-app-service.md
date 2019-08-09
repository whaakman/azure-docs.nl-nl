---
title: Ml-modellen implementeren op Azure App Service (preview-versie)
titleSuffix: Azure Machine Learning service
description: Meer informatie over het gebruik van de Azure Machine Learning-service voor het implementeren van een model in een web-app in Azure App Service.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 07/01/2019
ms.openlocfilehash: 84de9d53b19f5aa9b73570aa0d115d204e8b6596
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68848219"
---
# <a name="deploy-a-machine-learning-model-to-azure-app-service-preview"></a>Een machine learning model implementeren op Azure App Service (preview-versie)

Meer informatie over het implementeren van een model van de Azure Machine Learning-service als een web-app in Azure App Service.

> [!IMPORTANT]
> Hoewel zowel Azure Machine Learning-service als Azure App Service algemeen beschikbaar zijn, is de mogelijkheid om een model van de Machine Learning-service naar App Service te implementeren, een preview-versie.

Met Azure Machine Learning-service kunt u docker-installatie kopieën maken op basis van getrainde machine learning modellen. Deze installatie kopie bevat een webservice waarmee gegevens worden ontvangen, naar het model worden verzonden en het antwoord vervolgens wordt geretourneerd. Azure App Service kan worden gebruikt om de installatie kopie te implementeren en biedt de volgende functies:

* [SSL-ondersteuning](/azure/app-service/app-service-web-ssl-cert-load) voor beveiligde communicatie tussen clients en de service.
* [Uitschalen](/azure/azure-monitor/platform/autoscale-get-started?toc=%2fazure%2fapp-service%2ftoc.json) naar meerdere exemplaren zonder opnieuw te hoeven implementeren.
* [Geavanceerde verificatie](/azure/app-service/configure-authentication-provider-aad) voor verbeterde beveiliging.

Zie [app service-overzicht](/azure/app-service/overview)voor meer informatie over de functies van Azure app service.

## <a name="prerequisites"></a>Vereisten

* Een werkruimte van Azure Machine Learning-service. Zie het artikel [een werk ruimte maken](how-to-manage-workspace.md) voor meer informatie.
* Een getraind machine learning model dat is geregistreerd in uw werk ruimte. Als u geen model hebt, gebruikt u de [zelf studie voor installatie kopie classificatie: Train model](tutorial-train-models-with-aml.md) om er een te trainen en te registreren.
* Een docker-installatie kopie die is gemaakt op basis van het model. Als u geen installatie kopie hebt, gebruikt u de [afbeeldings classificatie: model implementeren](tutorial-deploy-models-with-aml.md) om er een te maken.

## <a name="deploy-image-as-a-web-app"></a>Een installatie kopie implementeren als een web-app

1. Selecteer uw Azure Machine Learning-werk ruimte in de [Azure Portal](https://portal.azure.com). Gebruik in de sectie __overzicht__ de __register__ koppeling om toegang te krijgen tot de Azure container Registry voor de werk ruimte.

    ![Scherm afbeelding van het overzicht voor de werk ruimte](media/how-to-deploy-app-service/workspace-overview.png)

2. Selecteer in de Azure Container Registry __opslag__plaatsen en selecteer vervolgens de naam van de __installatie kopie__ die u wilt implementeren. Voor de versie die u wilt implementeren, selecteert u de vermelding __...__ en implementeert __u deze in web app__.

    ![Scherm opname van implementeren vanuit ACR naar een web-app](media/how-to-deploy-app-service/deploy-to-web-app.png)

3. Als u de Web-App wilt maken, geeft u een site naam, een abonnement, een resource groep op en selecteert u het app service-plan/de locatie. Selecteer tot slot __maken__.

    ![Scherm afbeelding van het dialoog venster nieuwe web-app](media/how-to-deploy-app-service/web-app-for-containers.png)

## <a name="use-the-web-app"></a>De web-app gebruiken

Selecteer de web-app die u in de vorige stap hebt gemaakt in de [Azure Portal](https://portal.azure.com). Kopieer de __URL__in de sectie __overzicht__ . Deze waarde is de __basis-URL__ van de service.

![Scherm afbeelding van het overzicht voor de web-app](media/how-to-deploy-app-service/web-app-overview.png)

De webservice die aanvragen doorgeeft aan het model, bevindt zich op `{baseurl}/score`. Bijvoorbeeld `https://mywebapp.azurewebsites.net/score`. De volgende python-code laat zien hoe u gegevens indient naar de URL en hoe de reactie wordt weer gegeven:

```python
import requests
import json

scoring_uri = "https://mywebapp.azurewebsites.net/score"

headers = {'Content-Type':'application/json'}

if service.auth_enabled:
    headers['Authorization'] = 'Bearer '+service.get_keys()[0]

print(headers)
    
test_sample = json.dumps({'data': [
    [1,2,3,4,5,6,7,8,9,10], 
    [10,9,8,7,6,5,4,3,2,1]
]})

response = requests.post(scoring_uri, data=test_sample, headers=headers)
print(response.status_code)
print(response.elapsed)
print(response.json())
```

## <a name="next-steps"></a>Volgende stappen

* Zie de documentatie [over app service in Linux](/azure/app-service/containers/) voor meer informatie over het configureren van uw web-app.
* Zie [aan de slag met automatisch schalen in azure](/azure/azure-monitor/platform/autoscale-get-started?toc=%2fazure%2fapp-service%2ftoc.json)voor meer informatie over schalen.
* Zie [een SSL-certificaat gebruiken in uw Azure app service](/azure/app-service/app-service-web-ssl-cert-load)voor meer informatie over SSL-ondersteuning.
* Zie [uw app service-app configureren voor het gebruik van Azure Active Directory-aanmelding](/azure/app-service/configure-authentication-provider-aad)voor meer informatie over verificatie.