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
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938169"
---
## <a name="preparing-code-for-docker-and-kubernetes-development"></a>Code voorbereiden voor het ontwikkelen van Docker en Kubernetes
Tot nu toe hebt u een eenvoudige web-app die u lokaal kunt uitvoeren. U hebt nu het containerize door het maken van de activa die de app-container en hoe deze implementeert naar Kubernetes definiëren. Deze taak is eenvoudig doen met Azure Dev spaties: 

1. Starten van de VS Code en open de `webfrontend` map. (U kunt standaard prompts foutopsporing activa toevoegen of herstellen van het project negeren.)
1. Open de geïntegreerde Terminal in VS-Code (met behulp van de **weergave > geïntegreerde Terminal** menu).
1. Deze opdracht uitvoeren (Controleer of **webfrontend** is de huidige map):

    ```cmd
    azds prep --public
    ```

De Azure CLI `azds prep` opdracht genereert Docker en Kubernetes activa met standaardinstellingen:
* `./Dockerfile` Beschrijving van de app de installatiekopie van de container en hoe de broncode wordt samengesteld en wordt uitgevoerd in de container.
* Een [Helm grafiek](https://docs.helm.sh) onder `./charts/webfrontend` wordt beschreven hoe u de container geïmplementeerd naar Kubernetes.

Op dit moment is het niet nodig zijn om te begrijpen van de volledige inhoud van deze bestanden. Het verdient aanbeveling wijzen op, maar die **dezelfde Kubernetes en Docker als configuratiecode activa kunnen worden gebruikt vanuit ontwikkeling door naar de productie, zodat betere consistentie in verschillende omgevingen.**
 
Een bestand met de naam `./azds.yaml` ook wordt gegenereerd door de `prep` opdracht en deze wordt het configuratiebestand voor Azure Dev spaties. Er is een aanvulling op de Docker en Kubernetes artefacten met aanvullende configuratie waarmee een iteratieve ontwikkeling biedt bij Azure.
