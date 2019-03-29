---
title: Aan de slag met Azure Security Center (ASC) voor de Preview van IoT | Microsoft Docs
description: Aan de slag in informatie over de basiswerkstroom van ASC voor IoT-functies en -service.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 55c8d3b6-3126-4246-8d07-ef88fe5ea84f
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: 1186b362cf8f59f24020ae9afa3526e2e27b1794
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2019
ms.locfileid: "58575212"
---
# <a name="get-started-with-azure-security-center-asc-for-iot"></a>Aan de slag met Azure Security Center (ASC) voor IoT 

> [!IMPORTANT]
> ASC voor IoT is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

In dit artikel bevat een uitleg van de verschillende bouwstenen van de ASC voor IoT-service en wordt uitgelegd hoe u aan de slag met [inschakelen van de service](quickstart-onboard-iot-hub.md). 

ASC voor IoT kan naadloos worden geïntegreerd in uw IoT-Hub voor analyse van IoT hub-configuratie, de apparaat-id en de hub-apparaat communicatiepatronen.
ASC voor IoT biedt mogelijkheden voor verbeterde beveiliging, op basis van een agent verzamelen van beveiligingsgegevens van uw IoT-apparaten.

## <a name="asc-for-iot-seamless-iot-hub-integration"></a>ASC voor naadloze integratie van IoT-Hub IoT

Wanneer u probeert om de afzonderlijke IoT-apparaten te beveiligen, is de mogelijkheid voor het verzamelen van gegevens rechtstreeks vanuit de apparaten of vanuit het netwerk is vereist. ASC voor IoT biedt ter ondersteuning van deze inspanning, een arsenal van lage footprint security-agents voor bewaking en beveiliging.

Referentie-architectuur in ASC voor IoT-preview voor Linux en Windows security-agents, zowel in C# en C worden geleverd.
De agents onbewerkte gebeurtenissen verzamelen van het besturingssysteem van apparaat, gebeurtenis-aggregatie te verminderen van kosten en configuratie van een via een apparaatdubbel-module worden verwerkt.
Beveiligingsberichten worden verzonden via uw IoT-Hub in ASC van IoT analytics-services.

## <a name="asc-for-iot-basics"></a>ASC voor IoT-basisbeginselen

Kies het scenario voor workflow die het beste aan uw IoT-apparaat- en omgevingsvereisten:

### <a name="get-started-with-asc-for-iot-seamless-iot-hub-integration"></a>Aan de slag met ASC voor naadloze integratie van IoT-Hub IoT 

>[!Note]
>Deze werkstroom kunt u gebruikmaken van de service zonder ASC voor IoT-beveiliging-agents. 

Voor bewaking van uw apparaat gebruik identiteitsbeheer, het apparaat naar cloud en cloud naar apparaat communicatiepatronen, basiswerkstroom voor het testen en de service te starten na: 

1. [ASC inschakelen voor IoT-service op uw IoT-Hub](quickstart-onboard-iot-hub.md)
1. Als uw IoT-Hub geen geregistreerde apparaten heeft [registreren van een nieuw apparaat](https://docs.microsoft.com/azure/iot-accelerators/quickstart-device-simulation-deploy).
1. [Maken van een module van de beveiliging ascforiot](quickstart-create-security-twin.md) voor uw apparaten. 
1. Normale gedrag van apparaat- en system via definiëren [aangepaste waarschuwingen](quickstart-create-custom-alerts.md). 
1. Systeem testen om te controleren of de service en status van apparaten uitvoeren. 
1. Verken [waarschuwingen](concept-security-alerts.md), [aanbevelingen](concept-recommendations.md), en [gedetailleerde informatie over het gebruik van Log Analytics](how-to-security-data-access.md) met behulp van IoT-Hub. 


### <a name="get-started-with-asc-for-iot-security-agents"></a>Aan de slag met ASC voor IoT-beveiliging-agents

Maakt gebruik van ASC voor IoT enhanced security mogelijkheden, zoals het controleren van externe verbindingen, actieve toepassingen, aanmeldgebeurtenissen en aanbevolen procedures voor OS-configuratie met behulp van de volgende algemene werkstroom voor het testen en ervoor zorgen dat de: 

1. [ASC inschakelen voor IoT-service waarmee uw IoT-Hub](quickstart-onboard-iot-hub.md)
1. Als uw IoT-Hub geen geregistreerde apparaten heeft [registreren van een nieuw apparaat](https://docs.microsoft.com/azure/iot-accelerators/quickstart-device-simulation-deploy).
1. [Maken van een module van de beveiliging azureiotsecurity](quickstart-create-security-twin.md) voor uw apparaten.
1. De agent installeren op een Azure-gesimuleerd apparaat in plaats van installatie op een daadwerkelijk apparaat [draaien van een nieuwe Azure Virtual Machine (VM)](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-portal) in een zone beschikbaar. 
1. [Een ASC voor IoT security-agent implementeren](how-to-deploy-linux-cs.md) op uw IoT-apparaat of een nieuwe virtuele machine.
1. Volg de instructies voor [trigger_events](https://aka.ms/iot-security-github-trigger-events) om uit te voeren een simulatie van een aanval onschadelijk.
1. Controleer of u ASC voor IoT-waarschuwingen in reactie op de gesimuleerde aanval in de vorige stap. 
    - Beginnen met verificatie van vijf minuten nadat het script is uitgevoerd.
1. Verken [waarschuwingen](concept-security-alerts.md), [aanbevelingen](concept-recommendations.md), en [gedetailleerde informatie over het gebruik van Log Analytics](how-to-security-data-access.md) met behulp van IoT-Hub. 

## <a name="next-steps"></a>Volgende stappen

- Schakel [ASC voor IoT](quickstart-onboard-iot-hub.md)
- Configureer uw [oplossing](quickstart-configure-your-solution.md)
- [Beveiligingsmodules maken](quickstart-create-security-twin.md)
- Configureer [aangepaste waarschuwingen](quickstart-create-custom-alerts.md)
- [Een beveiligingsagent implementeren](how-to-deploy-agent.md)
