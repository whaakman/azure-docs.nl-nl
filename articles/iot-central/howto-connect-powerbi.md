---
title: Visualiseer uw Azure IoT Central-gegevens in Power BI-dashboard | Microsoft Docs
description: Gebruik de Power BI-oplossing voor Azure IoT Central te visualiseren en analyseren van uw IoT Central-gegevens.
ms.service: iot-central
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 02/15/2019
ms.topic: conceptual
ms.openlocfilehash: 322be1e13662d92a3cb0a805a9ccaacd05928f7d
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56328092"
---
# <a name="visualize-and-analyze-your-azure-iot-central-data-in-a-power-bi-dashboard"></a>Visualiseer en Analyseer uw Azure IoT Central-gegevens in Power BI-dashboard

*In dit onderwerp is bedoeld voor beheerders.*

![Power BI-oplossing pijplijn](media/howto-connect-powerbi/iot-continuous-data-export.png)

Gebruik de Power BI-oplossing voor Azure IoT Central te krachtige Power BI-dashboard voor het controleren van de prestaties van uw IoT-apparaten maken. In uw Power BI-dashboard, kunt u het volgende doen:
- Bijhouden hoeveel gegevens uw apparaten worden verzonden na verloop van tijd
- Gegevensvolume tussen de telemetrie, statussen en gebeurtenissen vergelijken
- Apparaten identificeren die veel van de metingen wilt rapporteren
- Bekijk historische trends van apparaat-metingen
- Problematische apparaten identificeren die veel kritieke gebeurtenissen verzenden

Deze oplossing stelt u de pijplijn die de gegevens in uw Azure Blob storage-account van neemt [continue gegevensexport](howto-export-data.md). Deze gegevens stromen via Azure Functions, Azure Data Factory en Azure SQL Database de gegevens te verwerken en transformeren. De uitvoer kan worden gevisualiseerd en geanalyseerd in een Power BI-rapport dat u als een PBIX-bestand downloaden kunt. Al deze resources worden gemaakt in uw Azure-abonnement, zodat u elk onderdeel aan de behoeften van uw behoeften kunt aanpassen.

## <a name="get-the-power-bi-solution-for-azure-iot-centralhttpsakamsiotcentralpowerbisolutiontemplate-from-microsoft-appsource"></a>Krijgen de [Power BI-oplossing voor Azure IoT Central](https://aka.ms/iotcentralpowerbisolutiontemplate) uit Microsoft AppSource.

## <a name="prerequisites"></a>Vereisten
Instellen van de oplossing is het volgende vereist:
- Toegang tot een Azure-abonnement
- Geëxporteerde gegevens met [continue gegevensexport](howto-export-data.md) van uw IoT Central-app. U wordt aangeraden dat u inschakelen metingen, apparaten en apparaten sjabloon streams optimaal gebruikmaken van de Power BI-dashboard.
- Power BI Desktop (meest recente versie)
- Power BI Pro (als u het dashboard delen met anderen wilt)

## <a name="reports"></a>Rapporten

Twee rapporten worden automatisch gegenereerd. 

De eerste lijst ziet u een historisch overzicht van de metingen die zijn gerapporteerd door apparaten, en een uitsplitsing van de verschillende typen van metingen en apparaten die het hoogste aantal metingen verzonden.

![Power BI-rapportpagina 1](media/howto-connect-powerbi/template-page1-hasdata.PNG)

De tweede lijst dives dieper in gebeurtenissen en ziet u een historisch overzicht van fouten en waarschuwingen die zijn gerapporteerd. Ook ziet u welke apparaten zijn alles op het hoogste aantal gebeurtenissen melden en speciaal foutgebeurtenissen en waarschuwingen.

![Power BI-rapportpagina 2](media/howto-connect-powerbi/template-page2-hasdata.PNG)

## <a name="architecture"></a>Architectuur
Alle resources die zijn gemaakt kunnen worden geopend in de Azure-portal. Alles zou moeten in één resourcegroep zijn.

![Azure Portal-weergave van resourcegroep](media/howto-connect-powerbi/azure-deployment.PNG)

De details van elke resource en hoe deze wordt gebruikt, wordt hieronder beschreven.

### <a name="azure-functions"></a>Azure Functions
De Azure-functie-app wordt geactiveerd telkens wanneer die een nieuw bestand wordt geschreven naar de Blob storage. De functies ophalen van de velden in elke metingen, apparaten en sjablonen apparaatbestand gedetecteerd en gevuld verschillende tussenliggende SQL-tabellen moet worden gebruikt door Azure Data Factory.

### <a name="azure-data-factory"></a>Azure Data Factory
Azure Data Factory maakt verbinding met de SQL-database als een gekoppelde service. Deze opgeslagen procedure-activiteiten die de gegevens te verwerken en op te slaan in de tabellen analyse wordt uitgevoerd.

### <a name="azure-sql-database"></a>Azure SQL Database
Deze tabellen worden automatisch gemaakt voor het vullen van de standaardrapporten. Verken deze schema's in Power BI en u uw eigen visualisaties op deze gegevens kunt bouwen.

| Tabelnaam |
|------------|
|[analytics]. [Metingen]|
|[analytics]. [Berichten]|
|[fase]. [Metingen]|
|[analytics]. [Eigenschappen]|
|[analytics]. [PropertyDefinitions]|
|[analytics]. [MeasurementDefinitions]|
|[analytics]. [Apparaten]|
|[analytics]. [DeviceTemplates]|
|[dbo]. [datum]|
|[dbo].[ChangeTracking]|

## <a name="estimated-costs"></a>Geschatte kosten

Hier volgt een schatting van de kosten voor Azure (Azure-functie, Data Factory, Azure SQL) die betrokken zijn. Alle prijzen zijn beschikbaar in Amerikaanse dollars. Houd er rekening mee dat de prijzen per regio, variëren zodat u altijd de meest recente prijzen van de afzonderlijke services voor het ontvangen van de werkelijke prijzen moet opzoeken.
De volgende standaardwaarden zijn ingesteld voor u in de sjabloon (u kunt wijzigen van deze nadat dingen die u hebt ingesteld):

- Azure Functions: App Service-plan S1, $74.40/ maand
- Azure SQL S1, ~$30/month

We raden u aan te raken met de verschillende prijsmodellen en wat aan uw wensen aanpassen.

## <a name="resources"></a>Resources

Ga naar AppSource om op te halen de [Power BI-oplossing voor Azure IoT Central](https://aka.ms/iotcentralpowerbisolutiontemplate).

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd om uw gegevens in Power BI te visualiseren, volgt de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [Apparaten beheren](howto-manage-devices.md)