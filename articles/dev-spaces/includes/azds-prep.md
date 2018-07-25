---
title: bestand opnemen
description: bestand opnemen
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: include
manager: douge
ms.openlocfilehash: d44f33b0e9f71c1d8d6e2c9878b08f9fa0e1f8a1
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38990985"
---
## <a name="preparing-code-for-docker-and-kubernetes-development"></a>Code voorbereiden voor de ontwikkeling van Docker en Kubernetes
U hebt nu een eenvoudige web-app die lokaal kan worden uitgevoerd. U gaat hier nu een container van maken door assets te maken waarmee de container van de app wordt gedefinieerd evenals hoe deze in Kubernetes wordt geïmplementeerd. Deze taak kan eenvoudig worden uitgevoerd met Azure Dev Spaces: 

1. Start VS Code en open de map `webfrontend`. (U kunt eventuele standaardprompts negeren om foutopsporingsassets toe te voegen of om het project te herstellen.)
1. Open de geïntegreerde terminal in VS Code (via het menu **Weergave > Geïntegreerde Terminal**).
1. Voer de volgende opdracht uit (controleer of **webfrontend** de huidige map is):

    ```cmd
    azds prep --public
    ```

Met de opdracht `azds prep` van Azure-CLI worden Docker- en Kubernetes-assets met standaardinstellingen gemaakt:
* In `./Dockerfile` wordt beschreven wat de containerinstallatiekopie van de app is, en hoe de broncode is opgebouwd en wordt uitgevoerd in de container.
* Met een [Helm-grafiek](https://docs.helm.sh) die zich onder `./charts/webfrontend` bevindt, wordt beschreven hoe de container in Kubernetes moet worden geïmplementeerd.

Op dit moment is het nog niet nodig om de volledige inhoud van deze bestanden te begrijpen. Wat u wel moet weten is dat **de dezelfde assets voor 'configuratie als code' in Docker en Kubernetes kunnen worden gebruikt van ontwikkeling tot productie, waardoor er meer consistentie tussen de verschillende omgevingen bestaat.**
 
Er wordt met de opdracht `prep` ook een bestand met de naam `./azds.yaml` gegenereerd. Dit is het configuratiebestand voor Azure Dev Spaces. Dit vormt een aanvulling op de Docker- en Kubernetes-artefacten met extra configuraties die herhaalbare ontwikkelingsmogelijkheden in Azure bieden.
