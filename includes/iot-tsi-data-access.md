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
ms.openlocfilehash: fb45ea02f365cf4e7b394e249f9b91a784e5469f
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46368671"
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