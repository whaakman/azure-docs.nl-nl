---
title: bestand opnemen
description: bestand opnemen
services: time-series-insights
author: ashannon7
ms.service: time-series-insights
ms.topic: include
ms.date: 08/20/2018
ms.author: anshan
ms.custom: include file
ms.openlocfilehash: c9daa86bf36b260001d9969385b9e8a98a8ac0cf
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58124996"
---
## <a name="grant-data-access"></a>Gegevenstoegang verlenen

Volg deze stappen voor het verlenen van toegang tot gegevens voor een gebruiker-principal:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Ga naar uw Time Series Insights-omgeving. Type **Time Series** in de **zoeken** vak. Selecteer **Time Series-omgeving** in de lijst met zoekresultaten. 

3. Selecteer uw Time Series Insights-omgeving in de lijst.

4. Selecteer **beleid voor gegevenstoegang**en selecteer vervolgens **+ toevoegen**.
    ![Beheren van de Time Series Insights-bron - omgeving](media/iot-tsi-data-access/getstarted-grant-data-access1.png)

5. Selecteer **gebruiker selecteren**.  Zoeken naar de gebruiker de naam of e-mailadres om te vinden van de gebruiker die u wilt toevoegen. Klik op **Selecteer** de selectie te bevestigen. 

    ![Beheren van de Time Series Insights-bron - toevoegen](media/iot-tsi-data-access/getstarted-grant-data-access2.png)

6. Selecteer **rol selecteren**. Kies de juiste rol voor de gebruiker:
   - Selecteer **Inzender** als u wilt toestaan dat gebruiker referentiegegevens en delen die zijn opgeslagen query's en perspectieven met andere gebruikers van de omgeving wijzigen. 
   - Selecteer anders **lezer** querygegevens van gebruikers toestaan in de omgeving en persoonlijke (niet-gedeelde) query's opslaan in de omgeving.

     Selecteer **Ok** de keuze van de rol te bevestigen.

     ![Beheren van de Time Series Insights-bron - gebruiker selecteren](media/iot-tsi-data-access/getstarted-grant-data-access3.png)

7. Selecteer **Ok** in de **gebruikersrol selecteren** pagina.

    ![Beheren van de Time Series Insights-bron - rol selecteren](media/iot-tsi-data-access/getstarted-grant-data-access4.png)

8. De **beleid voor gegevenstoegang** pagina vindt u de gebruikers en de rollen zijn voor elke gebruiker.

    ![Beheren van de Time Series Insights-bron - resultaten](media/iot-tsi-data-access/getstarted-grant-data-access5.png)