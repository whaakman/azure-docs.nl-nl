---
title: ASC inschakelen voor IoT-service in de Preview-fase van IoT Hub | Microsoft Docs
description: Informatie over het inschakelen van ASC voor IoT-service in uw IoT-Hub.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 670e6d2b-e168-4b14-a9bf-51a33c2a9aad
ms.service: ascforiot
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/25/2019
ms.author: mlottner
ms.openlocfilehash: 4fd7fb68fd2eb8be0c1c109d76cff341faba9391
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541630"
---
# <a name="quickstart-enable-service-in-iot-hub"></a>Quickstart: In IoT Hub-service inschakelen

> [!IMPORTANT]
> ASC voor IoT is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

In dit artikel bevat een uitleg over het inschakelen van de ASC voor IoT preview-service op uw IoT-Hub.  

> [!NOTE]
> ASC voor IoT op dit moment biedt alleen ondersteuning voor standard-laag en hoger IoT-Hubs.
>ASC voor IoT is een oplossing voor één hub. Als u meerdere hubs vereist, moeten er meerdere oplossingen. 

## <a name="prerequisites-for-enabling-the-service"></a>Vereisten voor het inschakelen van de service

- Log Analytics-werkruimte
  - Twee typen gegevens worden opgeslagen in uw Log Analytics-werkruimte door door ASC standaard voor IoT; **beveiligingswaarschuwingen** en **aanbevelingen**. 
  - U kunt opslag van een type aanvullende informatie toevoegen **ruwe gebeurtenissen**. Houd er rekening mee dat opslaan **ruwe gebeurtenissen** in Log Analytics voert u de kosten voor extra opslag. 
- IoT Hub (standard-laag of hoger)

## <a name="enable-asc-for-iot-on-your-iot-hub"></a>ASC inschakelen voor IoT op uw IoT-Hub 

Schakel beveiliging van uw IoT-Hub door het volgende doen: 

1. Open uw **IoT-Hub** in Azure portal. 
2. Selecteer en open **Security** in het menu links. 
3. Kies **IoT-beveiliging inschakelen**. 
4. Geef de details van uw Log Analytics-werkruimte. 
   - Er ook voor kiezen om op te slaan **ruwe gebeurtenissen** naast de standaard-gegevenstypen van opslag door te laten de **onbewerkte gebeurtenis** in-/ uitschakelen **op**. 
   - Er ook voor kiezen om in te schakelen **twin verzameling** door over te laten de **twin verzameling** in-/ uitschakelen **op**. 
5. Klik op **OK**. 
6. Klik op **Opslaan**. 

Gefeliciteerd! U hebt voltooid ASC inschakelen voor IoT op uw IoT-Hub. 

## <a name="next-steps"></a>Volgende stappen

Ga naar het volgende artikel voor meer informatie over het configureren van uw oplossing...

> [!div class="nextstepaction"]
> [Uw oplossing configureren](quickstart-configure-your-solution.md)