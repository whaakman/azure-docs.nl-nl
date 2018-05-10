---
title: Apparaten in de oplossing voor externe controle - Azure oplossen | Microsoft Docs
description: Deze zelfstudie laat zien hoe u oplossen en het oplossen van problemen met apparaten in de oplossing voor externe controle.
services: iot-suite
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 05/01/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: a959276ea61ec0e44ad45197019dfc80f26b768e
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="troubleshoot-and-remediate-device-issues"></a>Problemen oplossen en het oplossen van problemen met apparaat

Deze zelfstudie leert u hoe u de **onderhoud** pagina in de oplossing voor het oplossen van apparaat-problemen oplossen. De zelfstudie maakt ter introductie van het volgen van deze mogelijkheden gebruik van een scenario in de Contoso IoT-toepassing.

Contoso is het testen van een nieuwe **Prototype** apparaat in het veld. Als een Contoso-operator, merkt u tijdens de tests die de **Prototype** apparaat is onverwacht activering van een waarschuwing temperatuur op het dashboard. U moet nu het gedrag van dit defecte onderzoeken **Prototype** apparaat.

In deze zelfstudie leert u het volgende:

>[!div class="checklist"]
> * Gebruik de **onderhoud** pagina voor het onderzoeken van de waarschuwing
> * Een apparaat methode aanroepen om het probleem oplossen

## <a name="prerequisites"></a>Vereisten

Volg deze zelfstudie, moet u een geïmplementeerd exemplaar van de oplossing voor externe controle in uw Azure-abonnement.

Als u de oplossing voor externe controle nog niet hebt geïmplementeerd, maar u moet voltooien de [implementeren van de externe controle oplossingsverbetering](iot-suite-remote-monitoring-deploy.md) zelfstudie.

## <a name="use-the-maintenance-dashboard"></a>Gebruik het dashboard onderhoud

Op de **Dashboard** pagina u er zijn onverwachte temperatuur waarschuwingen afkomstig is van de regel die is gekoppeld ziet aan de **Prototype** apparaten:

![Waarschuwingen op het dashboard wordt weergegeven](media/iot-suite-remote-monitoring-maintain/dashboardalarm.png)

Voor het onderzoeken van het probleem verder kiest de **waarschuwing verkennen** optie naast de waarschuwing:

![Waarschuwing vanuit het dashboard verkennen](media/iot-suite-remote-monitoring-maintain/dashboardexplorealarm.png)

De detailweergave van de waarschuwing wordt weergegeven:

* Wanneer de waarschuwing is geactiveerd
* Statusinformatie over de apparaten die zijn gekoppeld aan de waarschuwing
* Telemetrie van de apparaten die zijn gekoppeld aan de waarschuwing

![Waarschuwingsdetails](media/iot-suite-remote-monitoring-maintain/maintenancealarmdetail.png)

Om te bevestigen op de waarschuwing, selecteer de **waarschuwing voorvallen** en kies **bevestigings**. Deze actie kan andere operators om te zien dat u al de waarschuwing gezien hebt en wordt gewerkt.

![Bevestig de waarschuwingen](media/iot-suite-remote-monitoring-maintain/maintenanceacknowledge.png)

Als u de waarschuwing bevestigt, verandert de status van de instantie in **bevestigd**.

In de lijst ziet u de **Prototype** verantwoordelijk voor het starten van de waarschuwing temperatuur apparaat:

![Lijst van de apparaten waardoor de waarschuwing](media/iot-suite-remote-monitoring-maintain/maintenanceresponsibledevice.png)

## <a name="remediate-the-issue"></a>Het probleem oplossen

Oplossen van het probleem met de **Prototype** apparaat gebruikt, moet u aan te roepen de **DecreaseTemperature** methode op het apparaat.

Als u wilt uitvoeren op een apparaat, selecteert u deze in de lijst met apparaten en kies vervolgens **taken**. De **Prototype** Apparaatmodel bevat zes methoden die een apparaat moet ondersteunen:

![Bekijk de methoden die ondersteuning biedt voor het apparaat](media/iot-suite-remote-monitoring-maintain/maintenancemethods.png)

Kies **DecreaseTemperature** en stel de taaknaam op **DecreaseTemperature**. Kies vervolgens **toepassen**:

![De taak voor het verkleinen van de temperatuur maken](media/iot-suite-remote-monitoring-maintain/maintenancecreatejob.png)

De status van de taak volgen op de **onderhoud** pagina **taken**. Gebruik de **taken** om bij te houden van alle taken weergeven en methode-aanroepen in de oplossing:

![De taak voor het verkleinen van de temperatuur bewaken](media/iot-suite-remote-monitoring-maintain/maintenancerunningjob.png)

Als u wilt weergeven in de details van een bepaalde taak of methodeaanroep, kies in de lijst in de **taken** weergeven:

![Taakdetails weergeven](media/iot-suite-remote-monitoring-maintain/maintenancejobdetail.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u gezien hoe:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Gebruik de **onderhoud** pagina voor het onderzoeken van de waarschuwing
> * Een apparaat methode aanroepen om het probleem oplossen

Nu u hebt geleerd hoe problemen van apparaten te beheren, de voorgestelde volgende stap is te leren hoe u [testen van uw oplossing met gesimuleerde apparaten](iot-suite-remote-monitoring-test.md).

<!-- Next tutorials in the sequence -->