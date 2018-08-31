---
title: Visualiseer uw Azure IoT Central-gegevens in Power BI-dashboard | Microsoft Docs
description: Gebruik de sjabloon van de Azure IoT Central Analytics Power BI-oplossing naar uw IoT Central-gegevens visualiseren en analyseren.
ms.service: iot-central
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 07/16/2018
ms.topic: conceptual
ms.openlocfilehash: 5cb55e73b379b909811bde728d2ab39e29635bf5
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2018
ms.locfileid: "43190696"
---
# <a name="visualize-and-analyze-your-azure-iot-central-data-in-a-power-bi-dashboard"></a>Visualiseer en Analyseer uw Azure IoT Central-gegevens in Power BI-dashboard

![Power BI-oplossing sjabloon pijplijn](media/howto-connect-powerbi/iot-continuous-data-export.png)

Gebruik de sjabloon van de Azure IoT Central Analytics Power BI-oplossing te maken van krachtige Power BI-dashboard voor het controleren van de prestaties van uw IoT-apparaten. In uw Power BI-dashboard, kunt u het volgende doen:
- Bijhouden hoeveel gegevens uw apparaten worden verzonden na verloop van tijd
- Gegevensvolume tussen de telemetrie, statussen en gebeurtenissen vergelijken
- Apparaten identificeren die veel van de metingen wilt rapporteren
- Bekijk historische trends van apparaat-metingen
- Problematische apparaten identificeren die veel kritieke gebeurtenissen verzenden

Deze oplossingssjabloon stelt u de pijplijn die de gegevens in uw Azure Blob storage-account van neemt [voortdurende gegevensexport](howto-export-data.md). Deze gegevens stromen via Azure Functions, Azure Data Factory en Azure SQL Database die verwerken en transformeren van de gegevens worden gevisualiseerd en geanalyseerd in een Power BI-rapport dat u als een PBIX-bestand downloaden kunt. Al deze resources worden gemaakt in uw Azure-abonnement, zodat u elk onderdeel aan de behoeften van uw behoeften kunt aanpassen. Deze oplossingssjabloon is volledig open-source, zodat u kunt meer informatie over de architectuur en breid de oplossing door naar de pagina uit de [Github-opslagplaats](https://aka.ms/iotcentralgithubpowerbisolutiontemplate).

**[Download de sjabloon van de Azure IoT Central Analytics-oplossing van Microsoft AppSource.](https://aka.ms/iotcentralpowerbisolutiontemplate)**

## <a name="prerequisites"></a>Vereisten
Instellen van de sjabloon vereist het volgende:
- Toegang tot een Azure-abonnement
- Geëxporteerde gegevens met [voortdurende gegevensexport](howto-export-data.md) van uw IoT Central-app. U wordt aangeraden dat u inschakelen metingen, apparaten en apparaten sjabloon streams optimaal gebruikmaken van de Power BI-dashboard.
- Power BI Desktop (meest recente versie)
- Power BI Pro (als u het dashboard delen met anderen wilt)

## <a name="reports"></a>Rapporten

Twee rapporten worden automatisch gegenereerd. 

De eerste lijst ziet u een historisch overzicht van de metingen die zijn gerapporteerd door apparaten, en een uitsplitsing van de verschillende typen van metingen en apparaten die het hoogste aantal metingen verzonden.

![Power BI-rapportpagina 1](media/howto-connect-powerbi/template-page1-hasdata.PNG)

De tweede lijst dives dieper in gebeurtenissen en ziet u een historisch overzicht van fouten en waarschuwingen die zijn gerapporteerd. Ook ziet u welke apparaten zijn alles op het hoogste aantal gebeurtenissen melden en speciaal foutgebeurtenissen en waarschuwingen.

![Power BI-rapportpagina 2](media/howto-connect-powerbi/template-page2-hasdata.PNG)

## <a name="resources"></a>Resources

Ga naar AppSource om op te halen de [oplossingssjabloon voor Azure IoT Central Analytics](https://aka.ms/iotcentralpowerbisolutiontemplate).

Ga naar de [Github-opslagplaats](https://aka.ms/iotcentralgithubpowerbisolutiontemplate) voor meer informatie over de architectuur en breid de oplossing uit.

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd om uw gegevens in Power BI te visualiseren, volgt de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [Apparaten beheren](howto-manage-devices.md)
