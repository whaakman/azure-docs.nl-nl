---
title: Toepassingssjablonen gebruiken in Azure IoT Central | Microsoft Docs
description: Als operator stelt het gebruik van apparaten in uw Azure IoT Central-toepassing.
author: dominicbetts
ms.author: dobett
ms.date: 05/30/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: a26f70c5a61f3855a3de991072a7e84103e87b69
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66499217"
---
# <a name="use-application-templates"></a>Toepassingssjablonen gebruiken

Dit artikel wordt beschreven hoe u, als de manager van een oplossing te maken en gebruiken van sjablonen voor toepassingen.

Wanneer u een Azure IoT Central-toepassing maakt, hebt u een ruime keuze aan ingebouwde voorbeeldsjablonen. U kunt ook uw eigen Toepassingssjablonen maken van bestaande IoT Central-toepassingen. U kunt uw eigen Toepassingssjablonen vervolgens gebruiken bij het maken van nieuwe toepassingen.

Wanneer u een sjabloon maakt, bevat de volgende items vanuit uw bestaande toepassing:

- De toepassing standaarddashboard, met inbegrip van de indeling van het dashboard en de tegels die u hebt gedefinieerd.
- Apparaatsjablonen, met inbegrip van metingen, instellingen, eigenschappen, opdrachten en dashboard.
- Regels. Alle regeldefinities zijn opgenomen. Acties, met uitzondering van e-mailacties, worden echter niet opgenomen.
- Apparaat wordt ingesteld, inclusief de voorwaarden en dashboards.

> [!WARNING]
> Als een dashboard bevat tegels die informatie over de specifieke apparaten weergeven, wordt deze tegels weergeven **de aangevraagde resource is niet gevonden** in de nieuwe toepassing. U moet deze tegels voor informatie over apparaten in uw nieuwe toepassing opnieuw configureren.

Wanneer u een sjabloon maakt, bevat deze geen de volgende items:

- Apparaten
- Gebruikers
- Taakdefinities
- Continue gegevensexport definities

Deze items handmatig toevoegen aan alle toepassingen die zijn gemaakt op basis van een sjabloon voor toepassingen.

## <a name="create-an-application-template"></a>Een toepassingssjabloon maken

Een toepassingssjabloon maken vanuit een bestaande IoT Central-toepassing:

1. Ga naar de **beheer** sectie in uw toepassing.
1. Selecteer **toepassing sjabloon exporteren**.
1. Op de **toepassing sjabloon exporteren** pagina, voer een naam en beschrijving voor de sjabloon.
1. Selecteer de **exporteren** knop om de toepassingssjabloon te maken. U kunt nu kopiëren de **deelbaar koppeling** waarmee iemand om een nieuwe toepassing van de sjabloon te maken:

![Een toepassingssjabloon maken](media/howto-use-app-templates/create-template.png)

## <a name="use-an-application-template"></a>Een toepassingssjabloon gebruiken

Als u een sjabloon om een nieuwe IoT Central-toepassing te maken, moet u een eerder gemaakte **deelbaar koppeling**. Plak de **deelbaar koppeling** in de adresbalk van uw browser. De **maken van een toepassing** pagina wordt weergegeven met uw aangepaste toepassing hebt geselecteerd:

![Een toepassing maken met een sjabloon](media/howto-use-app-templates/create-app.png)

Selecteer uw abonnement en de andere velden in het formulier invullen. Selecteer vervolgens **maken** naar een nieuwe IoT Central-toepassing van de toepassingssjabloon maken.

## <a name="manage-application-templates"></a>Toepassingssjablonen beheren

Op de **toepassing sjabloon exporteren** pagina, u kunt verwijderen of de toepassingssjabloon van de bijwerken.

Als u een toepassingssjabloon verwijdert, kunt u de eerder gegenereerde deelbaar koppeling niet meer gebruiken om nieuwe toepassingen te maken.

Voor het bijwerken van uw sjabloon, wijzigt u de sjabloonnaam of beschrijving op de **toepassing sjabloon exporteren** pagina. Selecteer vervolgens de **exporteren** knop opnieuw. Deze actie genereert een nieuwe **deelbaar koppeling** en worden alle vorige ongeldig **deelbaar koppeling** URL.

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe u sjablonen voor toepassingen, de voorgestelde volgende stap is te leren hoe u [IoT Central beheren vanuit Azure portal](howto-manage-iot-central-from-portal.md)
