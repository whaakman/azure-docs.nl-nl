---
title: Azure Time Series Preview management - over het inrichten en beheren van de Preview van Azure Time Series. | Microsoft Docs
description: Informatie over hoe u kunt inrichten en beheren van Azure Time Series Insights Preview.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/10/2018
ms.custom: seodec18
ms.openlocfilehash: 1834a59ff675f109267a406e3c2b03411e3bb50b
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53273497"
---
# <a name="how-to-provision-and-manage-azure-time-series-insights-preview"></a>Over het inrichten en beheren van Azure Time Series Insights Preview

Dit document wordt beschreven hoe u maken en beheren van een Azure Time Series Insights Preview-omgeving met behulp van de [Azure-portal](https://portal.azure.com/).

## <a name="overview"></a>Overzicht

Een kort overzicht over het inrichten van een Azure Time Series Insights Preview-omgeving wordt hieronder beschreven:

* Azure Time Series Insights Preview-omgevingen zijn **PAYG** omgevingen.
  * Als onderdeel van het proces voor het maken moet u opgeven met een Time Series-ID. Time Series-id mag maximaal drie sleutels. Meer informatie over het selecteren van een Time Series-ID door te lezen [u bij het kiezen van een Time Series-ID](./time-series-insights-update-how-to-id.md).
  * Als u een Azure Time Series Insights Preview-omgeving inricht, kunt u twee Azure-resources maken:

    * Een Azure Time Series Insights Preview-omgeving  
    * Een Azure Storage-account voor algemeen gebruik V1
  
    Informatie over [hoe u uw omgeving plannen](./time-series-insights-update-plan.md).

    >[!IMPORTANT]
    > Voor klanten die gebruikmaken van V2 Schakel-accounts, geen koud/archivering eigenschappen op het opslagaccount dat u wilt gebruiken.

* Elke Azure Time Series Insights Preview-omgeving kan worden (optioneel) gekoppeld aan een gebeurtenisbron zijn. Lezen [toevoegen van een Event Hub-bron](./time-series-insights-how-to-add-an-event-source-eventhub.md) en [hoe u een IoT Hub-bron toevoegen](./time-series-insights-how-to-add-an-event-source-iothub.md) voor meer informatie.
  * U krijgt een Timestamp-ID-eigenschap en een unieke consumergroep tijdens deze stap. Hiermee, zorgt u ervoor dat de omgeving toegang tot de bijbehorende gebeurtenissen heeft.

* Zodra de inrichting is voltooid, kunt u wijzigen het beleid voor toegang en andere kenmerken van de omgeving met suite uw zakelijke vereisten.

## <a name="new-environment-creation"></a>Nieuwe omgeving maken

De volgende stappen wordt beschreven hoe u een Azure Time Series Insights Preview-omgeving maken:

1. Selecteer de **PAYG** knop onder de **SKU** menu. Geef de omgevingsnaam van een, wijzen welke abonnementsgroep, en welke resourcegroep waarin u wilt gebruiken. Selecteer een ondersteunde locatie voor de omgeving worden gehost.

1. Een Time Series-ID invoeren

    >[!NOTE]
    > * De Time Series-ID is hoofdlettergevoelig en onveranderbare (deze kan niet worden gewijzigd nadat deze is ingesteld).
    > * Time Series-id mag maximaal 3 sleutels.
    > * Meer informatie over het selecteren van een Time Series-ID door te lezen [u bij het kiezen van een Time Series-ID](./time-series-insights-update-how-to-id.md).

1. Een Azure Storage-account maken door het selecteren van een Azure Storage-accountnaam en het toewijzen van een replicatie-optie. Hiermee maakt u dan automatisch een Azure Storage-account voor algemeen gebruik V1. Deze wordt gemaakt in dezelfde regio als de Azure Time Series Insights Preview-omgeving die u eerder hebt geselecteerd.

    ![Maak een Azure Time Series Insights-exemplaar.][1]

1. U kunt eventueel een gebeurtenisbron toevoegen.

   * Time Series Insights ondersteunt [Azure IoT Hub](./time-series-insights-how-to-add-an-event-source-iothub.md) en [Event Hubs](./time-series-insights-how-to-add-an-event-source-eventhub.md) als opties. U kunt alleen een enkel gebeurtenisbron toevoegen tijdens de aanmaak van de omgeving, kunt u een extra gebeurtenisbron later toevoegen. Het is raadzaam om te maken van een unieke consumergroep om te controleren of dat alle gebeurtenissen zijn zichtbaar voor uw exemplaar van Azure Time Series Insights Preview. U kunt een bestaande consumergroep selecteert of maakt u een nieuwe consumentengroep bij het toevoegen van de bron van de gebeurtenis.

   * U moet ook de juiste eigenschap Timestamp aanwijzen. Azure Time Series Insights gebruikt standaard de wachtrijduur van het bericht voor elke bron van gebeurtenis.

     > [!TIP]
     > De wachtrijduur van bericht mogelijk niet de beste geconfigureerde instelling wilt gebruiken in batch gebeurtenis of historische gegevens uploaden scenario's. Zorg ervoor dat uw beslissing om te gebruiken of een eigenschap Timestamp niet gebruiken in dergelijke gevallen controleren.

    ![Een gebeurtenisbron toevoegen aan uw exemplaar.][2]

1. Controleren en maken

    ![Een gebeurtenisbron toevoegen aan uw exemplaar.][3]

Bevestig dat uw omgeving is ingericht met de gewenste instellingen.

## <a name="management"></a>Beheer

U hebt de mogelijkheid voor het beheren van uw Azure Time Series Insights Preview-omgeving met behulp van de Azure portal. Hier volgen de belangrijkste verschillen bij het beheren van een **PAYG** Azure Time Series Insights Preview-omgeving in plaats van een S1 of S2-omgeving met behulp van Azure Portal:

* De Azure-portal *overzicht* ongewijzigd blade in Azure Time Series Insights, behalve in de volgende manieren:
  * Capaciteit is verwijderd omdat dit concept is niet relevant zijn voor **PAYG** omgevingen.
  * De Time Series-ID-eigenschap is toegevoegd. Hiermee bepaalt u hoe uw gegevens zijn gepartitioneerd.
  * Sets van verwijzingsgegevens worden verwijderd.
  * De weergegeven URL zorgt ervoor dat u de [Azure Time Series Insights Preview explorer](./time-series-insights-update-explorer.md).
  * De naam van uw Azure Storage-account wordt vermeld.

* De Azure-Portal *configureren* blade in Azure Time Series Insights Preview is verwijderd omdat er **PAYG** omgevingen zijn niet configureerbaar.

* De Azure-Portal *verwijzing* gegevens blade in Azure Time Series Insights Preview is verwijderd sinds de referentiegegevens is geen onderdeel van **PAYG** omgevingen.

![Controleer of uw exemplaar.][4]

## <a name="next-steps"></a>Volgende stappen

Lezen [hoe u uw omgeving plannen](./time-series-insights-update-plan.md).

Lezen [toevoegen van een Event Hub-bron](./time-series-insights-how-to-add-an-event-source-eventhub.md).

Lezen [hoe u een IoT Hub-bron toevoegen](./time-series-insights-how-to-add-an-event-source-iothub.md).

<!-- Images -->
[1]: media/v2-update-manage/manage_one.png
[2]: media/v2-update-manage/manage_two.png
[3]: media/v2-update-manage/manage_three.png
[4]: media/v2-update-manage/manage_four.png
