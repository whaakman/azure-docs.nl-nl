---
title: Beveiliging configureren om te raadplegen en beheren van Azure Time Series Insights | Microsoft Docs
description: In dit artikel wordt beschreven hoe u beveiliging en machtigingen als access management-beleid en de data access-beleid voor het beveiligen van Azure Time Series Insights.
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
ms.openlocfilehash: c7d4079c9106226e0d07aa97c4a52c16ddb257c3
ms.sourcegitcommit: 719dd33d18cc25c719572cd67e4e6bce29b1d6e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="grant-data-access-to-a-time-series-insights-environment-using-azure-portal"></a>Gegevenstoegang verlenen tot een Time Series Insights-omgeving met Azure Portal

Time Series Insights-omgevingen hebben twee onafhankelijke typen toegangsbeleid:

* Beleid voor beheertoegang
* Beleid voor gegevenstoegang

Beide soorten beleid verlenen Azure Active Directory-principals (gebruikers en apps) verschillende machtigingen voor een specifieke omgeving. De principals (gebruikers en apps) moeten behoren tot de active directory (ook wel de Azure-tenant) die zijn gekoppeld aan het abonnement met de omgeving.

Beleid voor beheertoegang verleent machtigingen die betrekking hebben op de configuratie van de omgeving, zoals
*   maken en verwijderen van de omgeving, gebeurtenisbronnen, referentiegegevenssets, en
*   beheer van het beleid voor gegevenstoegang.

Beleid voor gegevenstoegang verleent machtigingen voor het uitvoeren van gegevensquery's, het bewerken van referentiegegevens in de omgeving en het delen van opgeslagen query's en perspectieven die aan de omgeving zijn gekoppeld.

De twee typen beleid maken een duidelijke scheiding mogelijk tussen toegang tot het beheer van de omgeving en toegang tot de gegevens in de omgeving. Het is bijvoorbeeld mogelijk voor het instellen van een omgeving zodanig dat de eigenaar/maker van de omgeving wordt verwijderd uit de data access. Bovendien kunnen gebruikers en services die kunnen gegevens lezen uit de omgeving worden verleend geen toegang tot de configuratie van de omgeving.

## <a name="grant-data-access"></a>Gegevenstoegang verlenen
Volg deze stappen voor het verlenen van toegang tot gegevens voor een user principal:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Zoek uw Time Series Insights-omgeving. Type **Time Series** in de **search** vak. Selecteer **omgeving reeks** in de zoekresultaten. 

3. Selecteer uw Time Series Insights-omgeving in de lijst.
   
4. Selecteer **Gegevenstoegangsbeleid**, selecteer daarna **+ toevoegen**.
  ![Beheren van de Time Series Insights-bron - omgeving](media/data-access/getstarted-grant-data-access1.png)

5. Selecteer **gebruiker selecteren**.  Zoeken naar de gebruikersnaam of e-mailadres vinden van de gebruiker die u wilt toevoegen. Klik op **Selecteer** de selectie te bevestigen. 

   ![Beheren van de Time Series Insights-bron - toevoegen](media/data-access/getstarted-grant-data-access2.png)

6. Selecteer **Functieservices selecteren**. Kies de juiste rol voor de gebruiker:
   - Selecteer **Inzender** als u wilt toestaan dat gebruiker wijzigen referentiegegevens en share opgeslagen query's en perspectieven met andere gebruikers van de omgeving. 
   - Selecteer anders **lezer** query gebruikersgegevens in de omgeving toestaan en persoonlijke (niet-gedeelde) query's opslaan in de omgeving.

   Selecteer **Ok** de keuze van de rol te bevestigen.

   ![Beheren van de Time Series Insights-bron - gebruiker selecteren](media/data-access/getstarted-grant-data-access3.png)

8. Selecteer **Ok** in de **gebruikersrol selecteren** pagina.

   ![Beheren van de Time Series Insights-bron - rol selecteren](media/data-access/getstarted-grant-data-access4.png)

9. De **Gegevenstoegangsbeleid** pagina vindt u de gebruikers en de rollen voor elke gebruiker.

   ![Beheren van de Time Series Insights-bron - resultaten](media/data-access/getstarted-grant-data-access5.png)

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [een Event Hub gebeurtenisbron toevoegen aan uw omgeving Azure Time Series Insights](time-series-insights-how-to-add-an-event-source-eventhub.md).
* [Verzenden van gebeurtenissen](time-series-insights-send-events.md) aan de gebeurtenisbron-.
* Weergeven van uw omgeving in [Time Series Insights explorer](https://insights.timeseries.azure.com).
