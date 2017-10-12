---
title: Beleid voor gegevenstoegang in Azure Time Series Insights | Microsoft Docs
description: In deze zelfstudie leert u hoe u beleid voor gegevenstoegang kunt beheren in Time Series Insights
keywords: 
services: time-series-insights
documentationcenter: 
author: op-ravi
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: tsi
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/01/2017
ms.author: omravi
ms.openlocfilehash: 5258bf5de6f7aa1ea246f1235e7d362b1b7d0181
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="grant-data-access-to-a-time-series-insights-environment-using-azure-portal"></a>Gegevenstoegang verlenen tot een Time Series Insights-omgeving met Azure Portal

Time Series Insights-omgevingen hebben twee onafhankelijke typen toegangsbeleid:

* Beleid voor beheertoegang
* Beleid voor gegevenstoegang

Beide soorten beleid verlenen Azure Active Directory-principals (gebruikers en apps) verschillende machtigingen voor een specifieke omgeving. De principals (gebruikers en apps) moeten behoren tot de Active Directory (of Azure-tenant) die is gekoppeld aan het abonnement dat de omgeving bevat.

Beleid voor beheertoegang verleent machtigingen die betrekking hebben op de configuratie van de omgeving, zoals
*   maken en verwijderen van de omgeving, gebeurtenisbronnen, referentiegegevenssets, en
*   beheer van het beleid voor gegevenstoegang.

Beleid voor gegevenstoegang verleent machtigingen voor het uitvoeren van gegevensquery's, het bewerken van referentiegegevens in de omgeving en het delen van opgeslagen query's en perspectieven die aan de omgeving zijn gekoppeld.

De twee typen beleid maken een duidelijke scheiding mogelijk tussen toegang tot het beheer van de omgeving en toegang tot de gegevens in de omgeving. Het is bijvoorbeeld mogelijk om een omgeving zo in te stellen dat de eigenaar/maker van de omgeving geen toegang tot de gegevens heeft. Ook kan aan gebruikers en services die vanuit de omgeving gegevens kunnen lezen, geen toegang worden verleend tot de configuratie van de omgeving.

## <a name="grant-data-access"></a>Gegevenstoegang verlenen
De volgende stappen laten zien hoe gegevenstoegang wordt verleend aan een gebruiker-principal:

1.  Meld u aan bij [Azure Portal](https://portal.azure.com).
2.  Klik op Alle resources in het menu aan de linkerkant van Azure Portal.
3.  Selecteer uw Time Series Insights-omgeving.

  ![Beheren van de Time Series Insights-bron - omgeving](media/data-access/getstarted-grant-data-access1.png)

4.  Selecteer Toegang gegevenslaag en klik op Toevoegen

  ![Beheren van de Time Series Insights-bron - toevoegen](media/data-access/getstarted-grant-data-access2.png)

5.  Klik op Gebruiker selecteren.
6.  Zoek en selecteer de gebruiker aan de hand van zijn e-mailadres.
7.  Klik op de blade Gebruiker selecteren op Selecteren.

  ![Beheren van de Time Series Insights-bron - gebruiker selecteren](media/data-access/getstarted-grant-data-access3.png)

8.  Klik op Rol selecteren.
9.  Selecteer Bijdrager als u wilt toestaan dat de gebruiker referentiegegevens mag wijzigen en opgeslagen query's en perspectieven mag delen met andere gebruikers van de omgeving. Selecteer anders Lezer. Hiermee staat u de gebruiker toe om gegevens in de omgeving op te vragen en persoonlijke (niet-gedeelde) query's op te slaan in de omgeving.
10. Klik op de blade Rol selecteren op OK.

  ![Beheren van de Time Series Insights-bron - rol selecteren](media/data-access/getstarted-grant-data-access4.png)

11. Klik op de blade Gebruikersrol selecteren op OK.
12. U ziet het volgende:

  ![Beheren van de Time Series Insights-bron - resultaten](media/data-access/getstarted-grant-data-access5.png)

## <a name="next-steps"></a>Volgende stappen

* [Een gebeurtenisbron maken](time-series-insights-add-event-source.md)
* [Gebeurtenissen verzenden](time-series-insights-send-events.md) naar de gebeurtenisbron
* Uw omgeving bekijken in de [Time Series Insights-portal](https://insights.timeseries.azure.com)
