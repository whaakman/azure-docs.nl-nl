---
title: Inrichten en beheren van Azure Time Series-Preview | Microsoft Docs
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
ms.openlocfilehash: 3c6b5094613ee482b0fd3f63e3d9d69dfe4d40f4
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58094851"
---
# <a name="provision-and-manage-azure-time-series-insights-preview"></a>Inrichten en beheren van Azure Time Series Insights Preview

In dit artikel wordt beschreven hoe u maken en beheren van een Azure Time Series Insights Preview-omgeving met behulp van de [Azure-portal](https://portal.azure.com/).

## <a name="overview"></a>Overzicht

Azure Time Series Insights Preview-omgevingen worden betalen naar gebruik (betalen per gebruik)-omgevingen.

Als u een Azure Time Series Insights Preview-omgeving inricht, kunt u twee Azure-resources maken:

* Een Azure Time Series Insights Preview-omgeving  
* Een Azure Storage-account voor algemeen gebruik v1
  
Informatie over [hoe u uw omgeving plannen](./time-series-insights-update-plan.md).

>[!IMPORTANT]
> Als u een v2-account, Schakel geen koud/archivering eigenschappen op het opslagaccount dat u wilt gebruiken.

Desgewenst kunt u elke Azure Time Series Insights Preview-omgeving koppelen aan een gebeurtenisbron. Lees voor meer informatie, [een event hub-bron toevoegen](./time-series-insights-how-to-add-an-event-source-eventhub.md) en [een IoT hub-bron toevoegen](./time-series-insights-how-to-add-an-event-source-iothub.md). U opgeven een eigenschap Timestamp-ID en een unieke consumergroep tijdens deze stap. Hiermee zorgt u ervoor dat de omgeving toegang tot de bijbehorende gebeurtenissen heeft.

Nadat het inrichten is voltooid, kunt u het beleid voor toegang en andere kenmerken van de omgeving op basis van uw zakelijke vereisten.

## <a name="create-the-environment"></a>De omgeving maken

De volgende stappen wordt beschreven hoe u een Azure Time Series Insights Preview-omgeving maken:

1. Selecteer de **PAYG** knop onder de **SKU** menu. Geef de omgevingsnaam van een, en kies welke abonnementsgroep en welke resourcegroep waarin u wilt gebruiken. Selecteer een ondersteunde locatie voor de omgeving worden gehost.

   ![Maak een Azure Time Series Insights-exemplaar.][1]

1. Voer een Time Series-id.

    >[!NOTE]
    > * De ID van de reeks tijd is hoofdlettergevoelig en is onveranderbaar. Worden (deze kan niet gewijzigd nadat deze ingesteld.)
    > * Time Series-id mag maximaal drie sleutels.
    > * Lees voor meer informatie over het selecteren van een Time Series-ID [Kies een Time Series-ID](./time-series-insights-update-how-to-id.md).

1. Maak een Azure storage-account door de naam van een opslagaccount selecteren en het toewijzen van een replicatie-optie. Hiermee maakt u dan automatisch een Azure Storage-account voor algemeen gebruik v1. Deze wordt gemaakt in dezelfde regio als de Azure Time Series Insights Preview-omgeving die u eerder hebt geselecteerd.

    ![Een Azure storage-account voor uw exemplaar maken][5]

1. U kunt eventueel een gebeurtenisbron toevoegen.

   * Time Series Insights ondersteunt [Azure IoT Hub](./time-series-insights-how-to-add-an-event-source-iothub.md) en [Azure Event Hubs](./time-series-insights-how-to-add-an-event-source-eventhub.md) als opties. Hoewel u alleen de bron van een enkelvoudige gebeurtenis tijdens de aanmaak van de omgeving toevoegen kunt, kunt u een andere bron van gebeurtenis later toevoegen. Het is raadzaam om te maken van een unieke consumergroep om ervoor te zorgen dat alle gebeurtenissen zichtbaar voor uw exemplaar van Azure Time Series Insights Preview zijn. U kunt een bestaande consumergroep selecteert of maakt u een nieuwe consumentengroep bij het toevoegen van de bron van de gebeurtenis.

   * Ook moet u de juiste tijdstempeleigenschap kiezen. Azure Time Series Insights gebruikt standaard de wachtrijduur van het bericht voor elke bron van gebeurtenis.

     > [!TIP]
     > De wachtrijduur van bericht mogelijk niet de beste geconfigureerde instelling wilt gebruiken in batch gebeurtenis of historische gegevens uploaden scenario's. Zorg ervoor dat uw beslissing om te gebruiken of een eigenschap Timestamp niet gebruiken in dergelijke gevallen controleren.

     ![Tabblad voor bron van gebeurtenis][2]

1. Bevestig dat uw omgeving is ingericht met de gewenste instellingen.

    ![Beoordelen en tabblad maken][3]

## <a name="manage-the-environment"></a>De omgeving beheren

U kunt uw Azure Time Series Insights Preview-omgeving beheren met behulp van de Azure-portal. Hier volgen de belangrijkste verschillen bij het beheren van een betalen per gebruik Azure Time Series Insights Preview-omgeving, in plaats van een S1 of S2-omgeving, met behulp van de Azure-portal:

* De Azure-portal **overzicht** blade ongewijzigd in Azure Time Series Insights, behalve in de volgende manieren:
  * Capaciteit wordt verwijderd, omdat dit concept niet relevant zijn voor omgevingen met betalen per gebruik is.
  * De Time Series-ID-eigenschap is toegevoegd. Hiermee bepaalt u hoe uw gegevens zijn gepartitioneerd.
  * Sets van verwijzingsgegevens worden verwijderd.
  * De weergegeven URL zorgt ervoor dat u de [Azure Time Series Insights Preview explorer](./time-series-insights-update-explorer.md).
  * De naam van uw Azure storage-account wordt vermeld.

* De Azure-portal **configureren** blade in Azure Time Series Insights Preview is verwijderd omdat het betalen per gebruik-omgevingen zijn niet configureerbaar.

* De Azure-portal **verwijzen naar gegevens** blade in Azure Time Series Insights Preview is verwijderd omdat de referentiegegevens is geen onderdeel van betalen per gebruik-omgevingen.

![Time Series Insights-Preview-omgeving in Azure portal][4]

## <a name="next-steps"></a>Volgende stappen

Lezen [uw omgeving plannen](./time-series-insights-update-plan.md).

Lezen [een event hub-bron toevoegen](./time-series-insights-how-to-add-an-event-source-eventhub.md).

Lezen [een IoT hub-bron toevoegen](./time-series-insights-how-to-add-an-event-source-iothub.md).

<!-- Images -->
[1]: media/v2-update-manage/manage_one.PNG
[2]: media/v2-update-manage/manage_two.PNG
[3]: media/v2-update-manage/manage_three.PNG
[4]: media/v2-update-manage/manage_four.PNG
[5]: media/v2-update-manage/manage_five.PNG
