---
title: Azure IoT Central persoonlijke dashboards maken | Microsoft Docs
description: Als een gebruiker, informatie over het maken en beheren van uw persoonlijke dashboards.
author: dominicbetts
ms.author: dobett
ms.date: 02/13/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: ae89d5a59dfbc7e2ab5fdd4de030665874b5e1e1
ms.sourcegitcommit: c712cb5c80bed4b5801be214788770b66bf7a009
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57220492"
---
# <a name="create-and-manage-personal-dashboards"></a>Maken en beheren van persoonlijke dashboards

De **Dashboard** is de pagina waarop wordt geladen wanneer u eerst naar uw toepassing navigeren. Een **builder** definieert het dashboard van de toepassing standaard voor alle gebruikers in uw toepassing. U kunt dit standaarddashboard vervangen door uw eigen, aangepaste toepassingsdashboard. U kunt verschillende dashboards die andere gegevens weergeven en tussen deze twee schakelt hebben.

## <a name="create-dashboard"></a>dashboard maken

De volgende schermafbeelding ziet u het dashboard in een toepassing gemaakt op basis van de **voorbeeld Contoso** sjabloon. U kunt het dashboard van de toepassing standaard vervangen door een persoonlijk dashboard. Om dit te doen, klikt u op **+ nieuw** aan de bovenkant van de pagina.

![Dashboard voor toepassingen op basis van de sjabloon 'Contoso voorbeeld'](media/howto-personalize-dashboard/defaultdashboard.png)

Te klikken op **+ nieuw**, wordt de Dashboardeditor geopend. In de editor die u kunt uw dashboard een naam kunt geven en ervoor kiest items uit de bibliotheek. De bibliotheek bevat de tegels en dashboard primitieven die kunt u het dashboard aanpassen.

![Bibliotheek voor het dashboard](media/howto-personalize-dashboard/dashboardeditor.png)

Bijvoorbeeld, u kunt toevoegen een **apparaatinstellingen en eigenschappen** tegel om weer te geven van de instellingen en eigenschappen waarden voor een van de apparaten die u beheert. Om dit te doen, selecteert u eerst een **apparaat sjabloon** Selecteer vervolgens een **apparaatexemplaar**. Geef een titel en selecteer de tegel een **instelling** of een **eigenschap** om weer te geven. De volgende schermafbeelding ziet u de **ventilator snelheid** instelling is ingeschakeld om toe te voegen aan de tegel. Klik op **gedaan** om op te slaan van de wijziging aan uw dashboard.

!['Apparaatdetails configureren' formulier met details voor instellingen en eigenschappen](media/howto-personalize-dashboard/dashboardsetting.png)

Wanneer u uw persoonlijke dashboard bekijkt, ziet u de nieuwe tegel met de **ventilator snelheid** instellen voor het apparaat:

![Tabblad met de weergegeven instellingen en eigenschappen voor de tegel 'Dashboard'](media/howto-personalize-dashboard/personaldashboard.png)

U kunt andere tegeltypen in de bibliotheek voor het detecteren van het aanpassen van uw persoonlijke dashboards verder kunt verkennen.

## <a name="manage-dashboards"></a>Beheren van dashboards

U kunt meerdere persoonlijke dashboards hebben en tussen deze twee schakelt of Kies het standaarddashboard toepassing:

![Switch-dashboard](media/howto-personalize-dashboard/switchdashboards.png)

U kunt uw persoonlijke dashboards bewerken en verwijderen die u niet meer nodig hebt:

![Dashboard verwijderen](media/howto-personalize-dashboard/managedashboards.png)

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd over het maken en beheren van persoonlijke dashboards, kunt u het volgende doen:

> [!div class="nextstepaction"]
> [Informatie over het beheren van uw Toepassingsvoorkeuren voor](howto-manage-preferences.md)
