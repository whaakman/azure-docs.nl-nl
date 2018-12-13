---
title: Azure Cognitive Services implementeren in Azure Stack | Microsoft Docs
description: Informatie over het implementeren van Azure Cognitive Services op Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/11/2018
ms.author: mabrigg
ms.reviewer: guanghu
ms.openlocfilehash: 5af508714b5eae5cdd23c940af0ae21300c0c5b8
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53194670"
---
# <a name="deploy-azure-cognitive-services-to-azure-stack"></a>Azure Cognitive Services implementeren in Azure Stack

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

> [!Note]  
> Azure Cognitive Services in Azure Stack is in preview.

U kunt Azure Cognitive Services gebruiken met ondersteuning voor containers in Azure Stack. Ondersteuning voor containers in Azure Cognitive Services kunt u de uitgebreide API's die beschikbaar in Azure zijn. Uw gebruik van containers kan flexibiliteit van waar u wilt implementeren en hosten van de services die worden aangeboden in [Docker-containers](https://www.docker.com/what-container). Ondersteuning voor containers is momenteel beschikbaar in Preview-versie voor een subset van Azure Cognitive Services, met inbegrip van onderdelen van [Computer Vision](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home), [Face](https://docs.microsoft.com/azure/cognitive-services/face/overview), en [Tekstanalyse](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview), en [Language Understanding](https://docs.microsoft.com/azure/cognitive-services/luis/luis-container-howto) (LUIS).

Containerstrategie is een benadering voor softwaredistributie waarin een toepassing of service, met inbegrip van de afhankelijkheden en de configuratie zijn verpakt als een containerinstallatiekopie. Met weinig of geen wijziging, kunt u een installatiekopie implementeert op een containerhost. Elke container wordt geïsoleerd van andere containers en het onderliggende besturingssysteem. Het systeem zelf, heeft alleen de onderdelen die nodig zijn om uit te voeren van uw installatiekopie. Een containerhost heeft een kleinere footprint dan een virtuele machine. Bovendien u containers kunt maken van installatiekopieën voor taken op korte termijn en verwijderd als het niet meer nodig hebt.

## <a name="use-containers-with-cognitive-services-on-azure-stack"></a>Gebruik van containers met Cognitive Services in Azure Stack

- **Controle over gegevens**  
  Toestaan dat uw appgebruikers hebben controle over hun gegevens tijdens het gebruik van Cognitive Services. U kunt Cognitive Services leveren voor appgebruikers die gegevens naar Azure met globale of de openbare cloud kunnen niet verzenden.

- **Controle over gegevensmodellen kunnen bijwerken**  
  App-gebruikers-versie en de update van de modellen die zijn geïmplementeerd in hun oplossing bieden.

- **Draagbare-architectuur**  
  Het maken van een draagbare app-architectuur inschakelen, zodat u uw oplossing voor de openbare cloud, om een privécloud op systemen of de rand implementeren kunt. U kunt de container implementeren in Azure Kubernetes Service, Azure Container Instances, of in een Kubernetes-cluster in met Azure Stack. Zie voor meer informatie, [Kubernetes met Azure Stack implementeren](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy).

- **Hoge doorvoer en lage latentie**  
   Uw appgebruikers de mogelijkheid om te schalen met pieken in verkeer voor hoge doorvoer en lage latentie bieden. Cognitive Services om uit te voeren in Azure Kubernetes Service fysiek dicht bij hun logica van toepassingen en gegevens inschakelen.

Met Azure Stack, Cognitive Services-containers in een Kubernetes-cluster, samen met uw toepassingscontainers voor hoge beschikbaarheid en elastisch schalen te implementeren. U kunt uw toepassing ontwikkelen met Cognitive services combineren met onderdelen die zijn gebouwd op App-Services, functies, Blob-opslag, of SQL- of mySQL-databases. 

Voor meer informatie over Cognitive Services, containers, gaat u naar [ondersteuning voor containers in Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-container-support).

## <a name="deploy-the-azure-face-api"></a>De Azure Face-API implementeren

In dit artikel wordt beschreven hoe u de Face-API van Azure op Kubernetes-cluster in Azure Stack implementeren. U kunt dezelfde benadering gebruiken andere cognitive services-containers in Azure Stack Kubernetes-cluster implementeren.

## <a name="prerequisites"></a>Vereisten

Voordat u begint, moet u naar:

1.  Aanvragen van toegang tot de containerregister voor het ophalen van de Face-containerinstallatiekopieën vanuit Azure Cognitive Services-Container Registry. Voor meer informatie gaat u naar de sectie van [toegang aanvragen tot de privécontainerregister](https://docs.microsoft.com/azure/cognitive-services/face/face-how-to-install-containers#request-access-to-the-private-container-registry).

2.  Bereid een Kubernetes-cluster in Azure Stack. U kunt het artikel volgen [Kubernetes met Azure Stack implementeren](azure-stack-solution-template-kubernetes-deploy.md).

## <a name="create-azure-resources"></a>Azure-resources maken

Maak een bron van Cognitive Service in Azure om een voorbeeld van de containers Face, LUIS of tekst herkennen, respectievelijk. U moet het abonnement sleutel en het eindpunt-URL van de resource exemplaar maken van de servicecontainers cognitive gebruiken.

1.  Maak een Azure-resource in Azure portal. Als u wilt om een voorbeeld van de Face-containers, moet u eerst een bijbehorende Face-resource maken in Azure portal. Zie voor meer informatie, [Quick Start: Een Cognitive Services-account maken in Azure portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account).

    >  [!Note]  
    >  De Face- of Computer Vision-bron moet de prijscategorie F0 gebruiken.

2.  De eindpunt-URL en abonnement-sleutel voor de Azure-resource ophalen. Nadat de Azure-resource is gemaakt, moet u het abonnement sleutel en het eindpunt-URL van die resource voor het starten van de overeenkomstige gezichten, LUIS of tekst herkennen container voor de Preview-versie.

## <a name="create-a-kubernetes-secret"></a>Een Kubernetes-geheim maken 

Toets maken gebruik van de Kubectl geheime opdracht voor toegang tot het privé containerregister te maken. Vervang **&lt;gebruikersnaam&gt;** met de naam van de gebruiker en **&lt;wachtwoord&gt;** met het wachtwoord dat is opgegeven in de referentie op die u hebt ontvangen van de Azure Cognitive Service-team.

```bash  
    kubectl create secret docker-registry <secretName> \
        --docker-server='containerpreview.azurecr.io' \
        --docker-username='<username>' \
        --docker-password='<password>' 
```

## <a name="prepare-a-yaml-configure-file"></a>Voorbereiden van een YAML-configuratiebestand

U gaat gebruiken de YAML bestand ter vereenvoudiging van de implementatie van de cognitive service op het Kubernetes-cluster configureren.

Hier volgt een voorbeeld van een YAML-bestand naar de Face-service implementeren in Azure Stack configureren:

```Yaml  
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: <deploymentName>
spec:
  replicas: <replicaNumber>
  template:
    metadata:
      labels:
        app: <appName>
    spec:
      containers:
      - name: <containersName>
        image: <ImageLocation>
        env: 
        - name: EULA
          value: accept 
        - name: Billing
          value: <billingURL> 
        - name: apikey
          value: <apiKey>
        tty: true
        stdin: true
        ports:
        - containerPort: 5000 
      imagePullSecrets:
        - name: <secretName>
---
apiVersion: v1
kind: Service
metadata:
  name: <LBName>
spec:
  type: LoadBalancer
  ports:
  - port: 5000
    targetPort : 5000
    name: <PortsName>
  selector:
    app: <appName>
```

In deze YAML bestand configureren, gebruikt u de geheime sleutel die u hebt gebruikt om op te halen van de containerinstallatiekopieën cognitive service van Azure Container Registry. U en gebruik het geheime bestand naar een specifieke replica van de container te implementeren. U maakt ook een load balancer om ervoor te zorgen dat gebruikers toegang hebben tot deze service extern.

Als u meer informatie over de velden voor sleutels:

| Veld | Opmerkingen |
| --- | --- |
| replicaNumber | Hiermee definieert u de initiële replica's van exemplaren te maken. U kunt deze staren schalen later na de implementatie. |
| imageLocation | Geeft de locatie van de installatiekopie van de specifieke cognitive service-container in de ACR. Bijvoorbeeld, de face-service: `aicpppe.azurecr.io/microsoft/cognitive-services-face` |
| BillingURL |De eindpunt-URL die u hebt genoteerd in stap van [Azure-Resource maken](#create-azure-resources) |
| ApiKey | De abonnementssleutel die u hebt genoteerd in stap van [Azure-Resource maken](#create-azure-resources) |
| secretName | De geheime naam die u zojuist hebt genoteerd in stap van [maken voor toegang tot de privécontainerregister secrete](#create-secrete-to-access-the-private-container-registry) |

## <a name="deploy-the-cognitive-service"></a>De cognitive service implementeren

Gebruik van de volgende opdracht uit om de cognitive servicecontainers implementeren

```bash  
    Kubectl apply -f <yamlFineName>
```
Gebruik van de volgende opdracht uit om te controleren hoe het wordt geïmplementeerd: 
```bash  
    Kubectl get pod – watch
```

## <a name="test-the-cognitive-service"></a>De cognitive service testen

Toegang tot de [OpenAPI-specificatie](https://swagger.io/docs/specification/about/) (voorheen de Swagger-specificatie), met een beschrijving van de bewerkingen die worden ondersteund door een geïnstantieerde container van de **/swagger** relatieve URI voor deze container. De volgende URI geeft bijvoorbeeld de toegang tot de OpenAPI-specificatie voor de container Sentimentanalyse die is gemaakt in het vorige voorbeeld:

```HTTP  
http:<External IP>:5000/swagger
```

U kunt het externe IP-adres krijgen van de volgende opdracht uit: 

```bash  
    Kubectl get svc <LoadBalancerName>
```

## <a name="try-the-services-with-python"></a>Probeer de services met Python

U kunt proberen om te valideren van de Cognitive services in uw Azure-Stack met enkele eenvoudige Python-scripts uitgevoerd. Er zijn officiële voorbeelden van Python Quick Start voor [Computer Vision](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home), [Face](https://docs.microsoft.com/azure/cognitive-services/face/overview), en [Tekstanalyse](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview), en [Language Understanding](https://docs.microsoft.com/azure/cognitive-services/luis/luis-container-howto) () LUIS) ter referentie.

Er zijn twee dingen nodig moet rekening houden met de Python-App uitvoeren in de services die worden uitgevoerd op containers: 
1. Cognitive services in de container hoeft niet subsleutels voor verificatie, maar we nog steeds nodig hebt voor het invoeren van een willekeurige tekenreeks als een tijdelijke aanduiding om te voldoen aan de SDK. 
2. De base_URL vervangen door de werkelijke service-eindpunt-IP-adres 

Hier volgt een voorbeeld van Python script dat wordt gebruikt Face services Python SDK om te detecteren en frame gezichten in een afbeelding. 

```Python  
import cognitive_face as CF

# Cognitive Services in container do not need sub keys for authentication
# Keep the invalid key to satisfy the SDK inputs requirement. 
KEY = '0'  #  (keeping the quotes in place).
CF.Key.set(KEY)

# Get your actual Ip Address of service endpoint of your cognitive service on Azure Stack
BASE_URL = 'http://<svc IP Address>:5000/face/v1.0/'  
CF.BaseUrl.set(BASE_URL)

# You can use this example JPG or replace the URL below with your own URL to a JPEG image.
img_url = 'https://raw.githubusercontent.com/Microsoft/Cognitive-Face-Windows/master/Data/detection1.jpg'
faces = CF.face.detect(img_url)
print(faces)

```

## <a name="next-steps"></a>Volgende stappen

[Over het installeren en uitvoeren van containers voor Computer Vision-API.](https://docs.microsoft.com/azure/cognitive-services/computer-vision/computer-vision-how-to-install-containers)

[Over het installeren en uitvoeren van containers voor Face-API](https://docs.microsoft.com/azure/cognitive-services/face/face-how-to-install-containers#create-a-face-resource-on-azure)

[Over het installeren en uitvoeren van containers voor Tekstanalyse-API](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-install-containers)

[Over het installeren en uitvoeren van containers met Language Understanding (LIUS)](https://docs.microsoft.com/azure/cognitive-services/luis/luis-container-howto)