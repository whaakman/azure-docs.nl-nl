---
title: Waarschuwingen gebruiken en problemen met apparaten verhelpen met de oplossing voor externe controle - Azure | Microsoft Docs
description: In deze zelfstudie ziet u hoe u waarschuwingen kunt gebruiken om problemen te identificeren en op te lossen met apparaten die zijn verbonden met de oplossingsverbetering voor externe controle.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 06/18/2018
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 9607705220450b30d2ffaf0f2be9fa2a5664b879
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37081785"
---
# <a name="troubleshoot-and-remediate-device-issues"></a>Problemen met apparaten oplossen en herstellen

In deze zelfstudie gebruikt u de oplossingsverbetering voor externe controle om problemen met verbonden IoT-apparaten te identificeren en te herstellen. U gebruikt waarschuwingen op het dashboard van de oplossingsverbetering om problemen te identificeren. Vervolgens voert u externe taken uit om deze problemen op te lossen.

Contoso test momenteel in de praktijk een nieuw **Prototypeapparaat**. Als Contoso-operator merkt u, tijdens het testen, dat voor het **Prototypeapparaat** onverwacht een temperatuurwaarschuwing wordt geactiveerd op het dashboard. U moet nu het gedrag onderzoeken van het **Prototypeapparaat** waarvoor de waarschuwing wordt weergegeven, en het probleem oplossen.

In deze zelfstudie hebt u:

>[!div class="checklist"]
> * Een waarschuwing voor een apparaat onderzocht
> * Het probleem met het apparaat opgelost

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie hebt u een geïmplementeerd exemplaar van de oplossingsverbetering voor externe controle in uw Azure-abonnement nodig.

Als u de oplossingsverbetering voor externe controle nog niet hebt geïmplementeerd, voltooit u eerst de snelstart [Een cloudoplossing voor externe controle implementeren](quickstart-remote-monitoring-deploy.md).

## <a name="investigate-an-alert"></a>Een waarschuwing onderzoeken

Op de **Dashboardpagina** ziet u dat onverwachte temperatuurwaarschuwingen zijn geactiveerd op basis van de regel die is gekoppeld aan de **Prototypeapparaten**:

[![Waarschuwingen worden weergegeven op het dashboard](./media/iot-accelerators-remote-monitoring-maintain/dashboardalarm-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/dashboardalarm-expanded.png#lightbox)

Als u het probleem nader wilt onderzoeken, kiest u de optie **Waarschuwing verkennen** naast de waarschuwing:

[![Waarschuwing verkennen vanaf het dashboard](./media/iot-accelerators-remote-monitoring-maintain/dashboardexplorealarm-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/dashboardexplorealarm-expanded.png#lightbox)

In de detailweergave van de waarschuwing wordt het volgende weergegeven:

* Wanneer de waarschuwing is geactiveerd
* Statusinformatie over de apparaten waarvoor de waarschuwing is afgegeven
* Telemetrie van de apparaten waarvoor de waarschuwing is afgegeven

[![Waarschuwingsdetails](./media/iot-accelerators-remote-monitoring-maintain/maintenancealarmdetail-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/maintenancealarmdetail-expanded.png#lightbox)

Als u de waarschuwing wilt accepteren, selecteert u alle **Voorvallen van de waarschuwing** en kiest u **Accepteren**. Door deze actie weten andere operators dat u de waarschuwing hebt gezien en ermee bezig bent:

[![De waarschuwingen accepteren](./media/iot-accelerators-remote-monitoring-maintain/maintenanceacknowledge-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/maintenanceacknowledge-expanded.png#lightbox)

Als u de waarschuwing hebt geaccepteerd, wordt de status van het voorval gewijzigd in **Geaccepteerd**.

In de lijst ziet u welk **Prototypeapparaat** verantwoordelijk is voor het activeren van de temperatuurwaarschuwing:

[![Een lijst maken met de apparaten waardoor de waarschuwing is veroorzaakt](./media/iot-accelerators-remote-monitoring-maintain/maintenanceresponsibledevice-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/maintenanceresponsibledevice-expanded.png#lightbox)

## <a name="resolve-the-issue"></a>Het probleem oplossen

Als u het probleem met het **Prototypeapparaat** wilt oplossen, moet u de methode **DecreaseTemperature** aanroepen op het apparaat.

Als u actie wilt ondernemen voor een apparaat, selecteert u het apparaat in de lijst en kiest u vervolgens **Taken**. Voor het **Prototypeapparaatmodel** zijn zes methoden opgegeven waarvoor een apparaat ondersteuning moet bieden:

[![De methoden weergeven die worden ondersteund op het apparaat](./media/iot-accelerators-remote-monitoring-maintain/maintenancemethods-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/maintenancemethods-expanded.png#lightbox)

Kies **DecreaseTemperature** en stel de taaknaam in op **DecreaseTemperature**. Kies vervolgens **Toepassen**:

[![De taak maken om de temperatuur te verlagen](./media/iot-accelerators-remote-monitoring-maintain/maintenancecreatejob-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/maintenancecreatejob-expanded.png#lightbox)

Als u de status van de taak wilt bijhouden, klikt u op **Taakstatus bekijken**. Gebruik de weergave **Taken** om alle aanroepen voor taken en methoden in de oplossing bij te houden:

[![De taak controleren om de temperatuur te verlagen](./media/iot-accelerators-remote-monitoring-maintain/maintenancerunningjob-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/maintenancerunningjob-expanded.png#lightbox)

U kunt controleren of de temperatuur van het apparaat is afgenomen door de telemetrie te bekijken op de **Dashboardpagina**:

[![De verlaging van de temperatuur weergeven](./media/iot-accelerators-remote-monitoring-maintain/jobresult-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/jobresult-expanded.png#lightbox)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u gezien hoe u waarschuwingen kunt gebruiken om problemen met apparaten te identificeren en welke acties u kunt ondernemen om de problemen op deze apparaten op te lossen. Ga verder met de praktische artikelen voor informatie over het verbinden van een fysiek apparaat met de oplossingsverbetering.

Nu u hebt geleerd hoe u problemen met apparaten aanpakt, is de volgende stap die we willen voorstellen het [verbinden van uw apparaat met de oplossingsverbetering voor externe controle](iot-accelerators-connecting-devices.md).
