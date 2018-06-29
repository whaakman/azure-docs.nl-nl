---
title: Implementeren van Azure IoT Edge-modules (portal) | Microsoft Docs
description: De Azure portal gebruiken voor het implementeren van modules voor een IoT-randapparaat
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/06/2018
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 4082189d451f670c1ae3f76b8ec785d8bd0518b3
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37035940"
---
# <a name="deploy-azure-iot-edge-modules-from-the-azure-portal"></a>Azure IoT rand modules van de Azure-portal implementeren

Wanneer u IoT rand modules met uw bedrijfslogica maakt, die u wilt implementeren voor uw apparaten werken aan de rand. Als er meerdere modules die samenwerken om te verzamelen en gegevens verwerken, kunt u deze in één keer te implementeren en declareren de routeringsregels waarmee ze verbinding maken. 

Dit artikel laat zien hoe de Azure-portal begeleidt u bij het maken van een manifest voor implementatie en de implementatie naar een IoT-randapparaat pushen. Zie voor meer informatie over het maken van een implementatie die gericht is op meerdere apparaten op basis van hun gedeelde labels [implementeren en controleren van de rand van de IoT-modules op grote schaal](how-to-deploy-monitor.md)

## <a name="prerequisites"></a>Vereisten

* Een [IoT-hub](../iot-hub/iot-hub-create-through-portal.md) in uw Azure-abonnement. 
* Een [IoT randapparaat](how-to-register-device-portal.md) met de IoT-rand runtime geïnstalleerd. 

## <a name="select-your-device"></a>Selecteer het apparaat

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en navigeer naar uw IoT-hub.
2. Selecteer **IoT rand** in het menu.
3. Klik op de ID van het doelapparaat uit de lijst met apparaten. 
4. Selecteer **Modules instellen**.

## <a name="configure-a-deployment-manifest"></a>Configureren van een manifest voor implementatie

Een implementatiemanifest is een JSON-document dat wordt beschreven welke modules te implementeren en hoe de gegevens tussen de modules en de gewenste eigenschappen van de module horende loopt. Zie voor meer informatie over hoe implementatie manifesten werken en hoe ze worden gemaakt, [begrijpen hoe IoT rand modules kunnen worden gebruikt, geconfigureerd en hergebruikt](module-composition.md).

De Azure-portal is een wizard die u helpt bij het maken van het implementatiemanifest in plaats van handmatig maken van het JSON-document. Dit omvat drie stappen: **modules toevoegen**, **routes opgeven**, en **implementatie bekijken**. 

### <a name="add-modules"></a>Modules toevoegen

1. In de **registerinstellingen** sectie van de pagina, geef de referenties voor toegang tot alle persoonlijke container registers die uw module installatiekopieën bevatten. 
2. In de **implementatie modules** sectie van de pagina, selecteer **toevoegen**. 
3. Selecteer het type van de module in de vervolgkeuzelijst: 
   * **IoT rand Module** -de standaardoptie.
   * **Azure Stream Analytics-Module** -alleen modules die zijn gegenereerd op basis van een Azure Stream Analytics-werkbelasting. 

4. Geef een naam op voor de module en geeft u de installatiekopie van de container. Bijvoorbeeld: 
   * **Naam** -tempSensor
   * **Afbeelding van URI** -mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0
5. Vul de optionele velden indien nodig. Voor meer informatie over de container maken opties voor beleid voor opnieuw starten en de gewenste status Zie [EdgeAgent gewenst eigenschappen](module-edgeagent-edgehub.md#edgeagent-desired-properties). Zie voor meer informatie over de module-twin [definiëren of update gewenst eigenschappen](module-composition.md#define-or-update-desired-properties).
6. Selecteer **Opslaan**.
7. Herhaal stappen 2 tot en met 6 aanvullende modules toevoegen aan uw implementatie. 
8. Selecteer **volgende** om door te gaan naar de sectie routes.

### <a name="specify-routes"></a>Routes opgeven

Standaard de wizard u kunt een route aangeroepen **route** en zijn gedefinieerd als **FROM /* in $stroomopwaarts **, wat betekent dat alle berichten die door alle modules uitvoer naar uw IoT-hub worden verzonden.  

Toevoegen of bijwerken van de routes met informatie uit [declareren routes](module-composition.md#declare-routes), selecteer daarna **volgende** om door te gaan naar de sectie controleren.

### <a name="review-deployment"></a>Controleer de implementatie

De revisie sectie ziet u de JSON-deployment manifest dat is gemaakt op basis van uw selecties in de vorige twee secties. Er zijn twee modules gedeclareerd als u geen toevoegt: **$edgeAgent** en **$edgeHub**. Deze twee modules waaruit de [IoT rand runtime](iot-edge-runtime.md) en zijn vereiste standaardinstellingen in elke implementatie. 

Lees de informatie van uw implementatie en selecteer vervolgens **indienen**. 

## <a name="view-modules-on-your-device"></a>De modules weergeven op het apparaat

Als u modules hebt geïmplementeerd op het apparaat, kunt u alle afbeeldingen bekijken de **Apparaatdetails** pagina van de portal. Deze pagina bevat de naam van elke geïmplementeerde module, evenals de nuttige informatie zoals de implementatiecode status en afsluiten. 

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe [implementeren en controleren van de rand van de IoT-modules op grote schaal](how-to-deploy-monitor.md)
