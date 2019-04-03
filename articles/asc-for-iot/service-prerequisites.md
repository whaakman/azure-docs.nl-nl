---
title: Azure Security Center voor IoT-vereisten Preview | Microsoft Docs
description: De details van alles wat nodig is om aan de slag met Azure Security Center voor de vereisten voor IoT-service.
services: asc-for-iot
ms.service: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 790cbcb7-1340-4cc1-9509-7b262e7c3181
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: f09d768c0c6c1d351f737b053da9fd3282867099
ms.sourcegitcommit: d83fa82d6fec451c0cb957a76cfba8d072b72f4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58861686"
---
# <a name="azure-security-center-for-iot-prerequisites"></a>Azure Security Center voor IoT-vereisten

> [!IMPORTANT]
> Azure Security Center voor IoT is momenteel in openbare preview.
> Deze preview-versie wordt geleverd zonder een service level agreement, en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

In dit artikel bevat een uitleg van de verschillende bouwstenen van de Azure Security Center (ASC) voor IoT-service, wat u nodig hebt om te beginnen met en basisconcepten voor meer informatie over de service. 

## <a name="minimum-requirements"></a>Minimale vereisten

- IoT Hub Standard-laag
    - RBAC-rol **eigenaar** bevoegdheden van het niveau 
- [Log Analytics-werkruimte](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace) 
- Azure Security Center (aanbevolen)
    - Tijdens het gebruik van Azure Security Center is alleen een aanbeveling en niet een vereiste zonder dit niet mogelijk om uw andere Azure-resources in de IoT Hub weer te geven. 
 
## <a name="working-with-asc-for-iot-service"></a>Werken met ASC voor IoT-service

ASC voor IoT-inzichten en rapportages zijn beschikbaar met behulp van Azure IoT Hub en Azure Security Center. Voor het inschakelen van ASC voor IoT op uw Azure-IoT-Hub, een account met **eigenaar** bevoegdheden is vereist. Na het inschakelen van ASC voor IoT in uw IoT-Hub, ASC voor IoT-inzichten worden weergegeven als de **Security** functie in Azure IoT Hub en als **IoT** in Azure Security Center. 

## <a name="supported-service-regions"></a>Ondersteunde regio 's 

ASC voor IoT wordt momenteel ondersteund voor IoT-Hubs in de volgende Azure-regio's:
  - US - centraal
  - Noord-Europa
  - Azië - zuidoost

## <a name="wheres-my-iot-hub"></a>Waar zijn mijn IoT-Hub?

Controleer de locatie van uw IoT Hub om te controleren of de beschikbaarheid van de service voordat u begint. 

1. Open uw IoT-Hub. 
2. Klik op **Overzicht**. 
3. Controleer of de locatie die overeenkomt met een van de [ondersteunde regio's voor service](#supported-service-regions). 


## <a name="supported-platforms-for-agents"></a>Ondersteunde platforms voor agents 

ASC voor IoT-agents biedt ondersteuning voor een groeiende lijst met apparaten en platforms. Zie de [lijst met platforms ondersteund](how-to-deploy-agent.md) om te controleren of uw bibliotheek van het bestaande of geplande.  

## <a name="next-steps"></a>Volgende stappen
- [Overzicht](overview.md)
- [De service inschakelen](quickstart-onboard-iot-hub.md)
- [ASC voor veelgestelde vragen over IoT](resources-frequently-asked-questions.md)
- [Inzicht krijgen in ASC voor IoT-waarschuwingen](concept-security-alerts.md)
