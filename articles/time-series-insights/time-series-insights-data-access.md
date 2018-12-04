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
ms.date: 11/26/2018
ms.openlocfilehash: 3694ff0db63d685cb63f586062158b4f8acac5cf
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52847606"
---
# <a name="grant-data-access-to-an-environment"></a>Gegevenstoegang verlenen tot een omgeving

Dit artikel worden de twee typen beleidsregels voor toegang van Azure Time Series Insights (preview).

## <a name="grant-data-access"></a>Gegevenstoegang verlenen

Volg deze stappen voor het verlenen van toegang tot gegevens voor een gebruiker-principal:

1. Meld u aan bij [Azure-portal](https://portal.azure.com/).
1. Ga naar uw Time Series Insights (TSI)-omgeving. Type `Time Series` in de **zoeken** vak. Selecteer **Time Series-omgeving** in de lijst met zoekresultaten.
1. Selecteer uw TSI-omgeving in de lijst.
1. Selecteer **beleid voor gegevenstoegang**en selecteer vervolgens **+ toevoegen**.

    ![gegevens-access-een][1]

1. Selecteer **gebruiker selecteren**. Zoeken naar de gebruiker de naam of e-mailadres om te vinden van de gebruiker die u wilt toevoegen. Klik op **Selecteer** de selectie te bevestigen.

    ![data access-twee][2]

1. Selecteer **rol selecteren**. Kies de juiste rol voor de gebruiker:

    * Selecteer **Inzender** als u wilt toestaan dat gebruiker referentiegegevens en delen die zijn opgeslagen query's en perspectieven met andere gebruikers van de omgeving wijzigen.

    * Selecteer anders **lezer** querygegevens van gebruikers toestaan in de omgeving en persoonlijke (niet-gedeelde) query's opslaan in de omgeving.

    Selecteer **Ok** de keuze van de rol te bevestigen.

    ![data access-3][3]

1. Selecteer **Ok** in de **gebruikersrol selecteren** pagina.

    ![data access-vier][4]

1. De **beleid voor gegevenstoegang** pagina vindt u de gebruikers en de rollen zijn voor elke gebruiker.

    ! [data access-vijf[5]

## <a name="provide-guest-access-to-a-user-from-another-azure-active-directory-tenant"></a>Toegang voor gasten bieden aan een gebruiker van een andere Azure Active Directory-tenant

`Guest` is niet een beheerrol; het is een term die wordt gebruikt voor een account dat wordt uitgenodigd van één tenant naar een andere. Nadat de Gast-account is uitgenodigd naar de map van de tenant, kan deze hebben de dezelfde toegangsbeheer toegepast, zoals een andere account management om toegang te verlenen aan een TSI-omgeving met behulp van de Access Control (IAM)-blade of toegang verlenen tot de gegevens in de omgeving via de blade beleid voor gegevenstoegang. Lees voor meer informatie over de toegang voor gasten van Azure Active Directory (AAD)-tenant, [gebruikers toevoegen Azure Active Directory B2B-samenwerking in Azure portal](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator).

Volg deze stappen voor het verlenen van toegang voor gasten met een TSI-omgeving naar een AAD-gebruiker van een andere tenant:

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
1. Ga naar uw omgeving TSI. Type **Time Series** in het zoekvak in. Selecteer **Time Series-omgeving** in de lijst met zoekresultaten.
1. Selecteer uw TSI-omgeving in de lijst.
1. Selecteer **beleid voor gegevenstoegang**en selecteer vervolgens **+ uitnodigen**.

    ![gegevens-access-6][6]

1. Bieden van de gebruiker e-mail die u wilt uitnodigen. Let op: dit moet een e-mailbericht dat is gekoppeld aan AAD. U kunt desgewenst een persoonlijk bericht invoegen bij de uitnodiging opnemen.

    ![gegevens-access-7][7]

1. U ziet een bevestiging bel worden weergegeven op het scherm.

    ![acht-access-gegevens][8]

1. Selecteer **gebruiker selecteren**. Zoeken naar het e-mailadres van de gastgebruiker die u zojuist hebt uitgenodigd om te vinden van de gebruiker die u wilt toevoegen. Klik op **Selecteer** de selectie te bevestigen.

    ![gegevens-access-9][9]

1. Selecteer **Selecteer** rol. Kies de juiste rol voor de gastgebruiker:

    1. Selecteer **Inzender** als u wilt toestaan dat de gebruiker referentiegegevens en delen die zijn opgeslagen query's en perspectieven met andere gebruikers van de omgeving wijzigen.
    1. Selecteer anders **lezer** querygegevens van gebruikers toestaan in de omgeving en persoonlijke (niet-gedeelde) query's opslaan in de omgeving.
    1. Selecteer **Ok** de keuze van de rol te bevestigen.

    ![gegevens-access-tien][10]

1. Selecteer **Ok** in de **gebruikersrol selecteren** pagina.

1. De **beleid voor gegevenstoegang** pagina geeft nu de gastgebruiker en de rollen zijn voor elke gastgebruiker.

    ![gegevens-access-11][11]

1. Nu de gastgebruiker nemen bepaalde stappen moet voor toegang tot de omgeving zich bevindt in de Azure-tenant u zojuist hebt uitgenodigd laten. Eerst moeten ze accepteren van de uitnodiging die ze zojuist hebt verzonden. Deze uitnodiging is verzonden via e-mail naar het e-mailadres dat u in stap 5 hebt uitgenodigd. Zij moeten klikken **aan de slag**, om te accepteren.

    ![gegevens-access-twaalf][12]

1. De gastgebruiker moet vervolgens de machtigingen die zijn gekoppeld aan de organisatie van de beheerder accepteren.

    ![gegevens-access-13][13]

1. Wanneer de gastgebruiker is aangemeld bij het e-mailadres u uitgenodigd, en ze de uitnodiging accepteren, ze zullen raadpleegt u insights.azure.com. Zodra er, ze moet klikt u op de avatar naast hun e-mailadres in de rechterbovenhoek van het scherm.

    ![gegevens-access-14][14]

1. De gastgebruiker Selecteer vervolgens wordt uw Azure-tenant van de directory in het menu. Dit is de tenant die u hebt deze uitgenodigd.

    ![gegevens-access-vijftien][15]

1. Wanneer de gastgebruiker selecteert uw tenant, zien ze ten slotte de TSI-omgeving die u zojuist hebt verstrekt ze toegang tot. Ze moeten nu alle mogelijkheden die zijn gekoppeld aan de rol die is opgegeven in stap 8 hebben.

## <a name="next-steps"></a>Volgende stappen

* Informatie over [toevoegen van een Event Hub-gebeurtenisbron](./time-series-insights-how-to-add-an-event-source-eventhub.md) aan uw Azure TSI-omgeving.
* Verzenden [gebeurtenissen naar de gebeurtenisbron](./time-series-insights-send-events.md).
* Weergave [uw omgeving in de Verkenner van Time Series Insights](./time-series-insights-update-explorer.md).

<!-- Images -->
[1]: media/data-access/data-access-one.png
[2]: media/data-access/data-access-two.png
[3]: media/data-access/data-access-three.png
[4]: media/data-access/data-access-four.png
[5]: media/data-access/data-access-five.png
[6]: media/data-access/data-access-six.png
[7]: media/data-access/data-access-seven.png
[8]: media/data-access/data-access-eight.png
[9]: media/data-access/data-access-nine.png
[10]: media/data-access/data-access-ten.png
[11]: media/data-access/data-access-eleven.png
[12]: media/data-access/data-access-twelve.png
[13]: media/data-access/data-access-thirteen.png
[14]: media/data-access/data-access-fourteen.png
[15]: media/data-access/data-access-fifteen.png