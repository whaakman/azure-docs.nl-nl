---
title: Externe bewaking data access-control - Azure | Microsoft Docs
description: In dit artikel bevat informatie over hoe u besturingselementen voor toegang voor de Verkenner van Time Series Insights telemetrie in de oplossingsverbetering voor externe controle kunt configureren
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 08/06/2018
ms.topic: conceptual
ms.openlocfilehash: 9d5d572c3e32e3645e65ba8d6fc28b567b3c1e9a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65827240"
---
# <a name="configure-access-controls-for-the-time-series-insights-telemetry-explorer"></a>Besturingselementen voor toegang voor de Verkenner van Time Series Insights-telemetrie

In dit artikel bevat informatie over het configureren van besturingselementen voor toegang voor de Time Series Insights explorer in de oplossingsverbetering voor externe controle. Als u wilt toestaan dat gebruikers van de oplossingsversneller voor toegang tot de Verkenner van Time Series Insights, moet u elke gebruiker gegevenstoegang verlenen.

Beleid voor gegevenstoegang verleent machtigingen voor het uitvoeren van gegevensquery's, het bewerken van referentiegegevens in de omgeving en het delen van opgeslagen query's en perspectieven die aan de omgeving zijn gekoppeld.

## <a name="grant-data-access"></a>Gegevenstoegang verlenen

Volg deze stappen voor het verlenen van toegang tot gegevens voor een gebruiker-principal:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Ga naar uw Time Series Insights-omgeving. Type **Time Series** in de **zoeken** vak. Selecteer **Time Series-omgeving** in de lijst met zoekresultaten. 

3. Selecteer uw Time Series Insights-omgeving in de lijst.

4. Selecteer **beleid voor gegevenstoegang**en selecteer vervolgens **+ toevoegen**.
    ![Beheren van de Time Series Insights-bron - omgeving](media/iot-accelerators-remote-monitoring-rbac-tsi/getstarted-grant-data-access1.png)

5. Selecteer **gebruiker selecteren**.  Zoeken naar de gebruiker de naam of e-mailadres om te vinden van de gebruiker die u wilt toevoegen. Klik op **Selecteer** de selectie te bevestigen. 

    ![Beheren van de Time Series Insights-bron - toevoegen](media/iot-accelerators-remote-monitoring-rbac-tsi/getstarted-grant-data-access2.png)

6. Selecteer **rol selecteren**. Kies de juiste rol voor de gebruiker:
   - Selecteer **Inzender** als u wilt toestaan dat gebruiker referentiegegevens en delen die zijn opgeslagen query's en perspectieven met andere gebruikers van de omgeving wijzigen. 
   - Selecteer anders **lezer** querygegevens van gebruikers toestaan in de omgeving en persoonlijke (niet-gedeelde) query's opslaan in de omgeving.

     Selecteer **Ok** de keuze van de rol te bevestigen.

     ![Beheren van de Time Series Insights-bron - gebruiker selecteren](media/iot-accelerators-remote-monitoring-rbac-tsi/getstarted-grant-data-access3.png)

7. Selecteer **Ok** in de **gebruikersrol selecteren** pagina.

    ![Beheren van de Time Series Insights-bron - rol selecteren](media/iot-accelerators-remote-monitoring-rbac-tsi/getstarted-grant-data-access4.png)

8. De **beleid voor gegevenstoegang** pagina vindt u de gebruikers en de rollen zijn voor elke gebruiker.

    ![Beheren van de Time Series Insights-bron - resultaten](media/iot-accelerators-remote-monitoring-rbac-tsi/getstarted-grant-data-access5.png)

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe besturingselementen voor toegang worden verleend voor de Time Series Insights explorer in de oplossingsverbetering voor externe controle.

Zie voor meer informatie over de oplossingsverbetering voor externe controle [architectuur voor externe controle](iot-accelerators-remote-monitoring-sample-walkthrough.md)

Zie voor meer informatie over het aanpassen van de oplossing voor externe controle [aanpassen en opnieuw implementeren van een microservice](iot-accelerators-microservices-example.md)
<!-- Next tutorials in the sequence -->