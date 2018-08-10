---
title: Beveiliging configureren om te openen en beheren van Azure Time Series Insights | Microsoft Docs
description: In dit artikel wordt beschreven hoe u beveiliging en machtigingen configureren als toegang tot de beleidsregels en data access-beleid voor het beveiligen van Azure Time Series Insights.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/15/2017
ms.openlocfilehash: 97c9480d6f2b75d83252bfb6410d7b5f946757ef
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39630650"
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

De twee typen beleid maken een duidelijke scheiding mogelijk tussen toegang tot het beheer van de omgeving en toegang tot de gegevens in de omgeving. Het is bijvoorbeeld mogelijk het instellen van een omgeving zodanig dat de eigenaar/maker van de omgeving is verwijderd uit de toegang tot gegevens. Bovendien gebruikers en services die zijn toegestaan voor het lezen van gegevens van de omgeving mogelijk geen toegang worden verleend aan de configuratie van de omgeving.

## <a name="grant-data-access"></a>Gegevenstoegang verlenen
Volg deze stappen voor het verlenen van toegang tot gegevens voor een gebruiker-principal:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Ga naar uw Time Series Insights-omgeving. Type **Time Series** in de **zoeken** vak. Selecteer **Time Series-omgeving** in de lijst met zoekresultaten. 

3. Selecteer uw Time Series Insights-omgeving in de lijst.
   
4. Selecteer **beleid voor gegevenstoegang**en selecteer vervolgens **+ toevoegen**.
  ![Beheren van de Time Series Insights-bron - omgeving](media/data-access/getstarted-grant-data-access1.png)

5. Selecteer **gebruiker selecteren**.  Zoeken naar de gebruiker de naam of e-mailadres om te vinden van de gebruiker die u wilt toevoegen. Klik op **Selecteer** de selectie te bevestigen. 

   ![Beheren van de Time Series Insights-bron - toevoegen](media/data-access/getstarted-grant-data-access2.png)

6. Selecteer **rol selecteren**. Kies de juiste rol voor de gebruiker:
   - Selecteer **Inzender** als u wilt toestaan dat gebruiker referentiegegevens en delen die zijn opgeslagen query's en perspectieven met andere gebruikers van de omgeving wijzigen. 
   - Selecteer anders **lezer** querygegevens van gebruikers toestaan in de omgeving en persoonlijke (niet-gedeelde) query's opslaan in de omgeving.

   Selecteer **Ok** de keuze van de rol te bevestigen.

   ![Beheren van de Time Series Insights-bron - gebruiker selecteren](media/data-access/getstarted-grant-data-access3.png)

8. Selecteer **Ok** in de **gebruikersrol selecteren** pagina.

   ![Beheren van de Time Series Insights-bron - rol selecteren](media/data-access/getstarted-grant-data-access4.png)

9. De **beleid voor gegevenstoegang** pagina vindt u de gebruikers en de rollen zijn voor elke gebruiker.

   ![Beheren van de Time Series Insights-bron - resultaten](media/data-access/getstarted-grant-data-access5.png)

## <a name="next-steps"></a>Volgende stappen
* Informatie over [een Event Hub-gebeurtenisbron toevoegen aan uw Azure Time Series Insights-omgeving](time-series-insights-how-to-add-an-event-source-eventhub.md).
* [Gebeurtenissen verzenden](time-series-insights-send-events.md) naar de gebeurtenisbron.
* Uw omgeving bekijken in [Verkenner van Time Series Insights](https://insights.timeseries.azure.com).
