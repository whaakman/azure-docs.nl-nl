---
title: CI/CD met Jenkins - invoegtoepassing voor Azure IoT Edge inschakelen | Microsoft Docs
description: De Azure IoT Edge-extensie voor Jenkins kunt u IoT Edge devlopment- en implementatietaken integreren in uw bestaande DevOps-oplossing.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 01/22/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 173e6ff91acd2ad28d7203b2b5db65e0ee0ecc43
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2019
ms.locfileid: "54910826"
---
# <a name="integrate-azure-iot-edge-with-jenkins-pipelines"></a>Azure IoT Edge integreren in Jenkins pipelines

Azure IoT Edge heeft ingebouwde ondersteuning voor Azure DevOps en Azure DevOps Projects, maar biedt ook een invoegtoepassing om uit te breiden van DevOps-functionaliteit met Jenkins. [Jenkins](https://jenkins.io/) is een open-source automatiseringsserver die gebruikmaakt van invoegtoepassingen ter ondersteuning van veel typen van de ontwikkeling en implementatie van projecten, met inbegrip van IoT Edge. 

De Azure IoT Edge-invoegtoepassing voor Jenkins is gericht op continue integratie en continue implementatie. U kunt een build- en push pijplijn maken die modules bouwt en hun containerinstallatiekopieën naar uw containerregister gepusht. Vervolgens maakt u een release-pijplijn waarmee modules in uw IoT Edge-apparaten wordt geïmplementeerd. 

Voordat u begint met het gebruik van de IoT Edge-invoegtoepassing voor Jenkins, moet u een IoT-hub in Azure en een container registry voor het opslaan van uw containerinstallatiekopieën. Gebruik een Service-Principal van Azure om Jenkins-bijdragerrechten naar uw IoT-hub zodat de invoegtoepassing kunt implementaties voor uw IoT-Edge-apparaten maken. 

Als u klaar bent om aan de slag, installatie vinden en gebruiken van details voor de [Azure IoT Edge-invoegtoepassing voor Jenkins](https://plugins.jenkins.io/azure-iot-edge).