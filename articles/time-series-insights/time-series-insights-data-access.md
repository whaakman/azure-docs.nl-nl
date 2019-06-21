---
title: Beveiliging configureren om te openen en beheren van Azure Time Series Insights Preview | Microsoft Docs
description: In dit artikel wordt beschreven hoe u beveiliging en machtigingen configureren als toegang tot de beleidsregels en data access-beleid voor het beveiligen van Azure Time Series Insights Preview.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.workload: big-data
ms.topic: conceptual
ms.date: 05/01/2019
ms.custom: seodec18
ms.openlocfilehash: 69180e17714b7d7004e63dce0de82a50e1f0b3af
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67164628"
---
# <a name="grant-data-access-to-an-environment"></a>Gegevenstoegang verlenen tot een omgeving

Dit artikel worden de twee typen beleidsregels voor toegang van Azure Time Series Insights Preview.

## <a name="sign-in-to-time-series-insights"></a>Aanmelden bij de Time Series Insights

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
1. Ga naar uw Time Series Insights-omgeving. Voer `Time Series` in de **zoeken** vak. Selecteer **Time Series-omgeving** in de lijst met zoekresultaten.
1. Selecteer uw Time Series Insights-omgeving in de lijst.

## <a name="grant-data-access"></a>Gegevenstoegang verlenen

Volg deze stappen voor het verlenen van toegang tot gegevens voor een gebruiker-principal.

1. Selecteer **beleid voor gegevenstoegang**, en selecteer vervolgens **+ toevoegen**.

    [![Data-access-one](media/data-access/data-access-one.png)](media/data-access/data-access-one.png#lightbox)

1. Kies **gebruiker selecteren**. Zoeken naar de gebruiker de naam of e-mailadres om te vinden van de gebruiker die u wilt toevoegen. Selecteer **Selecteer** de selectie te bevestigen.

    [![Data-access-two](media/data-access/data-access-two.png)](media/data-access/data-access-two.png#lightbox)

1. Kies **rol selecteren**. Kies de juiste rol voor de gebruiker:

    * Selecteer **Inzender** als u wilt toestaan dat de gebruiker referentiegegevens en delen die zijn opgeslagen query's en perspectieven met andere gebruikers van de omgeving wijzigen.

    * Selecteer anders **lezer** toestaan dat de gebruiker om gegevens te doorzoeken in de omgeving en persoonlijk, niet-gedeelde query's opslaan in de omgeving.

   Selecteer **OK** de keuze van de rol te bevestigen.

    [![Data-access-three](media/data-access/data-access-three.png)](media/data-access/data-access-three.png#lightbox)

1. Selecteer **OK** op de **gebruikersrol selecteren** pagina.

    [![Data-access-four](media/data-access/data-access-four.png)](media/data-access/data-access-four.png#lightbox)

1. Bevestig dat de **beleid voor gegevenstoegang** pagina vindt u de gebruikers en de rollen voor elke gebruiker.

    [![Data-access-five](media/data-access/data-access-five.png)](media/data-access/data-access-five.png#lightbox)

## <a name="provide-guest-access-from-another-aad-tenant"></a>Toegang voor gasten bieden van een andere AAD-tenant

`Guest` is niet een beheerrol. Het is een term die wordt gebruikt voor een account dat wordt uitgenodigd van één tenant naar een andere. Nadat de Gast-account wordt uitgenodigd naar de map van de tenant, heeft dit de dezelfde toegangsbeheer toegepast, zoals een ander account. U kunt beheertoegang verlenen aan een Time Series Insights-omgeving met behulp van de blade van de Access Control (IAM). Of u kunt toegang verlenen tot de gegevens in de omgeving via de blade beleid voor gegevenstoegang. Lees voor meer informatie over de toegang voor gasten van Azure Active Directory (Azure AD)-tenant, [gebruikers toevoegen Azure Active Directory B2B-samenwerking in Azure portal](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator).

Volg deze stappen voor het verlenen van toegang voor gasten met een Time Series Insights-omgeving naar een Azure AD-gebruiker van een andere tenant.

1. Selecteer **beleid voor gegevenstoegang**, en selecteer vervolgens **+ uitnodigen**.

    [![Data-access-six](media/data-access/data-access-six.png)](media/data-access/data-access-six.png#lightbox)

1. Voer het e-mailadres voor de gebruiker die u wilt uitnodigen. Dit e-mailadres moet worden gekoppeld aan Azure AD. U kunt desgewenst een persoonlijk bericht invoegen bij de uitnodiging opnemen.

    [![Data-access-seven](media/data-access/data-access-seven.png)](media/data-access/data-access-seven.png#lightbox)

1. Zoek naar de bevestiging bel die wordt weergegeven op het scherm.

    [![Data-access-eight](media/data-access/data-access-eight.png)](media/data-access/data-access-eight.png#lightbox)

1. Kies **gebruiker selecteren**. Zoeken naar het e-mailadres van de gastgebruiker die u uitgenodigd om te vinden van de gebruiker die u wilt toevoegen. Vervolgens **Selecteer** de selectie te bevestigen.

    [![Data-access-nine](media/data-access/data-access-nine.png)](media/data-access/data-access-nine.png#lightbox)

1. Kies **rol selecteren**. Kies de juiste rol voor de gastgebruiker:

    * Selecteer **Inzender** als u wilt toestaan dat de gebruiker referentiegegevens en delen die zijn opgeslagen query's en perspectieven met andere gebruikers van de omgeving wijzigen.

    * Selecteer anders **lezer** toestaan dat de gebruiker om gegevens te doorzoeken in de omgeving en persoonlijk, niet-gedeelde query's opslaan in de omgeving.

   Selecteer **OK** de keuze van de rol te bevestigen.

    [![Data-access-ten](media/data-access/data-access-ten.png)](media/data-access/data-access-ten.png#lightbox)

1. Selecteer **OK** op de **gebruikersrol selecteren** pagina.

1. Bevestig dat de **beleid voor gegevenstoegang** pagina geeft een lijst van de gastgebruiker en de functies voor elke gastgebruiker.

    [![Data-access-eleven](media/data-access/data-access-eleven.png)](media/data-access/data-access-eleven.png#lightbox)

1. Nu de gastgebruiker moet de stappen voor toegang tot de omgeving zich in de Azure-tenant waaraan u deze hebt uitgenodigd. Ze accepteren eerst de uitnodiging die u ze hebt verzonden. Deze uitnodiging is verzonden via e-mail naar het e-mailadres dat u in stap 5 hebt gebruikt. Ze selecteren **aan de slag** te accepteren.

    [![Data-access-twelve](media/data-access/data-access-twelve.png)](media/data-access/data-access-twelve.png#lightbox)

1. De gastgebruiker accepteert vervolgens de machtigingen die zijn gekoppeld aan de organisatie van de beheerder.

    [![Data-access-thirteen](media/data-access/data-access-thirteen.png)](media/data-access/data-access-thirteen.png#lightbox)

1. Nadat de gastgebruiker wordt ondertekend in naar het e-mailadres dat u hebt gebruikt om uit te nodigen, en ze de uitnodiging accepteren, ze naar insights.azure.com. Zodra er, ze selecteren de avatar naast het e-mailadres in de rechterbovenhoek van het scherm.

    [![gegevens-access-14](media/data-access/data-access-fourteen.png)](media/data-access/data-access-fourteen.png#lightbox)

1. Volgende, de Gast gebruiker selecteert de Azure-tenant van de directory in het menu. Deze tenant is de waaraan u deze hebt uitgenodigd.

    [![Data-access-fifteen](media/data-access/data-access-fifteen.png)](media/data-access/data-access-fifteen.png#lightbox)

Nadat de gastgebruiker selecteert uw tenant, zien ze de Time Series Insights-omgeving waarnaar u hebt opgegeven ze toegang. Ze hebben nu de mogelijkheden die zijn gekoppeld aan de rol die u hebt opgegeven dat ze met in **stap 5**.

## <a name="next-steps"></a>Volgende stappen

* Informatie over [toevoegen van een Azure Event Hubs-gebeurtenisbron](./time-series-insights-how-to-add-an-event-source-eventhub.md) aan uw Time Series Insights-omgeving.

* Verzenden [gebeurtenissen naar de gebeurtenisbron](./time-series-insights-send-events.md).

* Weergave [uw omgeving in de Verkenner van Time Series Insights Preview](./time-series-insights-update-explorer.md).
