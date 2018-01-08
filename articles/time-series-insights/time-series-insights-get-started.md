---
title: Een Azure Time Series Insights-omgeving maken | Microsoft Docs
description: In dit artikel wordt beschreven hoe de Azure portal gebruiken om een nieuwe Time Series Insights-omgeving te maken.
services: time-series-insights
ms.service: time-series-insights
author: ashannon7
ms.author: anshan
manager: jhubbard
editor: MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: article
ms.date: 11/15/2017
ms.openlocfilehash: 20156432e17d5eca90779271bd18dc49fa988d7c
ms.sourcegitcommit: 719dd33d18cc25c719572cd67e4e6bce29b1d6e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="create-a-new-time-series-insights-environment-in-the-azure-portal"></a>Een nieuwe Time Series Insights-omgeving maken in Azure Portal
Dit artikel wordt beschreven hoe u een nieuwe Time Series Insights-omgeving met de Azure portal maakt.

Time Series Insights kunt u aan de slag te visualiseren en opvragen van gegevens die binnenkomen in Azure IoT Hubs en Event Hubs in minuten, zodat u grote hoeveelheden reeksgegevens query in seconden.  Het is ontworpen voor de schaal van internet der dingen (IoT) en terabytes aan gegevens kan verwerken.

## <a name="steps-to-create-the-environment"></a>Stappen voor het maken van de omgeving
Volg deze stappen voor het maken van een omgeving:

1.  Meld u aan bij [Azure Portal](https://portal.azure.com).

2.  Selecteer de **+ nieuw** knop.

3.  Selecteer de **Internet der dingen** categorie en selecteer **Time Series Insights**.

   ![De Time Series Insights-omgeving maken](media/time-series-insights-get-started/1-new-tsi.png)

4.  Op de **Time Series Insights** pagina **maken**.

5. Vul de vereiste parameters. De volgende tabel bevat uitleg over elke parameter:
   
   ![De Time Series Insights-resourcegroep maken](media/time-series-insights-get-started/2-create-tsi.png)
   
   Instelling|Voorgestelde waarde|Beschrijving
   ---|---|---
   Naam van de omgeving | Een unieke naam | Deze naam vertegenwoordigt de omgeving in [time series explorer](https://insights.timeseries.azure.com)
   Abonnement | Uw abonnement | Als u meerdere abonnementen hebt, kiest u het abonnement dat bij voorkeur de gebeurtenisbron bevat. Time Series Insights kan Azure IoT Hub- en Event Hub-resources die in hetzelfde abonnement aanwezig zijn, automatisch detecteren.
   Resourcegroep | Maak een nieuwe of bestaande gebruiken | Een resourcegroep is een verzameling Azure-resources die samen worden gebruikt. U kunt een bestaande resourcegroep, bijvoorbeeld de database met uw Event Hub of IoT Hub. Of u kunt een nieuwe maken als deze resource is niet gerelateerd aan de andere bronnen.
   Locatie | Dichtstbijzijnde uw gebeurtenisbron | Kies bij voorkeur dezelfde data center locatie met de brongegevens gebeurtenis in de poging om te voorkomen dat toegevoegd regio-overschrijdende en cross-zone bandbreedtekosten en latentie toegevoegd bij het verplaatsen van gegevens buiten de regio.
   Prijscategorie | S1 | Kies de doorvoer die nodig zijn. Selecteer voor de laagste kosten en starter capaciteit, S1.
   Capaciteit | 1 | De capaciteit is dat de vermenigvuldigingsfactor van toepassing is op de snelheid van inkomende, opslagcapaciteit en kosten die zijn gekoppeld aan de geselecteerde SKU.  U kunt de capaciteit van een omgeving wijzigen nadat deze is gemaakt. Voor de laagste kosten, selecteert u een capaciteit van 1. 
  
6. Controleer **vastmaken aan dashboard** beste eenvoudig toegang tot uw Time Series-omgeving in de toekomst.

   ![Time Series Insights vastmaken aan het dashboard](media/time-series-insights-get-started/3-pin-create.png)

7. Selecteer **maken** om te beginnen met het inrichtingsproces. Het kan enkele minuten duren.

8. Selecteer voor het controleren van het implementatieproces de **meldingen** symbool (belpictogram).

   ![Bekijk de meldingen](media/time-series-insights-get-started/4-notifications.png)

Als de implementatie is geslaagd, kunt u **gaat u naar de resource** voor het configureren van andere eigenschappen instellen van de beveiliging met gegevenstoegangsbeleid, het toevoegen van bronnen van gebeurtenissen en andere acties.

## <a name="next-steps"></a>Volgende stappen
* [Definieer gegevenstoegangsbeleid](time-series-insights-data-access.md) voor het beveiligen van uw omgeving.
* [Toevoegen van een Event Hub gebeurtenisbron](time-series-insights-how-to-add-an-event-source-eventhub.md) aan uw Azure Time Series Insights-omgeving. 
* [Verzenden van gebeurtenissen](time-series-insights-send-events.md) aan de gebeurtenisbron-.
* Weergeven van uw omgeving in [Time Series Insights explorer](https://insights.timeseries.azure.com).
