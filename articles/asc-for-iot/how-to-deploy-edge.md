---
title: Azure Security Center implementeren voor IoT Edge-module | Microsoft Docs
description: Meer informatie over het implementeren van Azure Security Center voor IoT security-agent op IoT Edge.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 32a9564d-16fd-4b0d-9618-7d78d614ce76
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2019
ms.author: mlottner
ms.openlocfilehash: 2a201fe649d52ad9604c7ac6675b26d60e7f2dd1
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58754768"
---
# <a name="deploy-security-module-on-your-iot-edge-device"></a>Beveiligingsmodule voor op uw IoT Edge-apparaat implementeren

> [!IMPORTANT]
> Azure Security Center voor IoT is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Azure Security Center (ASC) voor IoT **azureiotsecurity** module biedt een uitgebreide oplossing voor uw IoT Edge-apparaat.
Security module verzamelt, verzamelt en analyseert onbewerkte beveiligingsgegevens van uw besturingssysteem en de Container-systeem in de praktijk toepasbare aanbevelingen en waarschuwingen.
Zie voor meer informatie, [Security-module voor IoT Edge](security-edge-architecture.md).

In deze handleiding leert u hoe u een security-module op uw IoT Edge-apparaat implementeren.

## <a name="deploy-security-module"></a>Beveiligingsmodule implementeren

Gebruik de volgende stappen voor het implementeren van een ASC voor IoT-beveiliging-module voor IoT Edge.

### <a name="prerequisites"></a>Vereisten

1. Zorg ervoor dat uw apparaat is [geregistreerd als een IoT Edge-apparaat](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-register-device-portal).

1. De ASC voor IoT Edge-module vereist de [AuditD framework](https://linux.die.net/man/8/auditd) op het Edge-apparaat is geïnstalleerd.

   Installeer framework door te voeren van de volgende opdracht uit op uw Edge-apparaat:
   
   `apt-get install auditd audispd-plugins`

### <a name="deployment-using-azure-portal"></a>Implementatie met behulp van Azure portal

1. Open **Marketplace** in Azure portal.

1. Zoeken naar **azure iot-beveiliging** en klikt u op **Azure IoT-beveiliging**.

   ![](media/howto/edge_onboarding_7.png)

1. Klik op **Create**.

1. Kies uw **abonnement**, **IoT-Hub** en **IoT Edge-apparaatnaam**, klikt u vervolgens op **maken**.

1. Klik op **volgende** twee keer op **revisie implementatie**.

1. Zorg ervoor dat **edgeHub.settings.createOptions** is als volgt geconfigureerd:

   `"createOptions": "{\"HostConfig\":{\"PortBindings\":{\"8883/tcp\":[{\"HostPort\":\"8883\"}],\"443/tcp\":[{\"HostPort\":\"443\"}],\"5671/tcp\":[{\"HostPort\":\"5671\"}]}}}"`

1. Klik op **indienen** om uit te voeren van de implementatie.


## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over configuratie-opties, blijven de handleiding voor de configuratie van de module. 
> [!div class="nextstepaction"]
> [Configuratie van de module hoe u met betrekking tot](./how-to-agent-configuration.md)
