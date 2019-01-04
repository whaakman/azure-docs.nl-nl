---
title: Beveiliging configureren om te openen en beheren van Azure Time Series Insights Preview | Microsoft Docs
description: In dit artikel wordt beschreven hoe u beveiliging en machtigingen configureren als toegang tot de beleidsregels en data access-beleid voor het beveiligen van Azure Time Series Insights Preview.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/26/2018
ms.custom: seodec18
ms.openlocfilehash: 9aea7a9c9dd96bf30ebb3def9354df9e4bd30114
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2018
ms.locfileid: "53558513"
---
# <a name="grant-data-access-to-an-environment"></a>Gegevenstoegang verlenen tot een omgeving

Dit artikel worden de twee typen beleidsregels voor toegang van Azure Time Series Insights Preview.

## <a name="grant-data-access"></a>Gegevenstoegang verlenen

Volg deze stappen voor het verlenen van toegang tot gegevens voor een gebruiker-principal.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
1. Ga naar uw Time Series Insights-omgeving. Voer `Time Series` in de **zoeken** vak. Selecteer **Time Series-omgeving** in de lijst met zoekresultaten.
1. Selecteer uw Time Series Insights-omgeving in de lijst.
1. Selecteer **beleid voor gegevenstoegang**, en selecteer vervolgens **+ toevoegen**.

    ![gegevens-access-een][1]

1. Kies **gebruiker selecteren**. Zoeken naar de gebruiker de naam of e-mailadres om te vinden van de gebruiker die u wilt toevoegen. Klik op **Selecteer** de selectie te bevestigen.

    ![data access-twee][2]

1. Kies **rol selecteren**. Kies de juiste rol voor de gebruiker:

    * Selecteer **Inzender** als u wilt toestaan dat de gebruiker referentiegegevens en delen die zijn opgeslagen query's en perspectieven met andere gebruikers van de omgeving wijzigen.

    * Selecteer anders **lezer** toestaan dat de gebruiker om gegevens te doorzoeken in de omgeving en persoonlijk, niet-gedeelde query's opslaan in de omgeving.

   Selecteer **OK** de keuze van de rol te bevestigen.

    ![data access-3][3]

1. Selecteer **OK** op de **gebruikersrol selecteren** pagina.

    ![data access-vier][4]

1. Bevestig dat de **beleid voor gegevenstoegang** pagina vindt u de gebruikers en de rollen voor elke gebruiker.

    ![Data access-vijf][5]

## <a name="provide-guest-access-to-a-user-from-another-azure-active-directory-tenant"></a>Toegang voor gasten bieden aan een gebruiker van een andere Azure Active Directory-tenant

`Guest` is niet een beheerrol. Het is een term die wordt gebruikt voor een account dat wordt uitgenodigd van één tenant naar een andere. Nadat de Gast-account wordt uitgenodigd naar de map van de tenant, heeft dit de dezelfde toegangsbeheer toegepast, zoals een ander account. U kunt beheertoegang verlenen aan een Time Series Insights-omgeving met behulp van de blade van de Access Control (IAM). Of u kunt toegang verlenen tot de gegevens in de omgeving via de blade beleid voor gegevenstoegang. Lees voor meer informatie over de toegang voor gasten van Azure Active Directory (Azure AD)-tenant, [gebruikers toevoegen Azure Active Directory B2B-samenwerking in Azure portal](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator).

Volg deze stappen voor het verlenen van toegang voor gasten met een Time Series Insights-omgeving naar een Azure AD-gebruiker van een andere tenant.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
1. Ga naar uw Time Series Insights-omgeving. Voer **Time Series** in de **zoeken** vak. Selecteer **Time Series-omgeving** in de lijst met zoekresultaten.
1. Selecteer uw Time Series Insights-omgeving in de lijst.
1. Selecteer **beleid voor gegevenstoegang**, en selecteer vervolgens **+ uitnodigen**.

    ![gegevens-access-6][6]

1. Voer het e-mailadres voor de gebruiker die u wilt uitnodigen. Dit e-mailadres moet worden gekoppeld aan Azure AD. U kunt desgewenst een persoonlijk bericht invoegen bij de uitnodiging opnemen.

    ![gegevens-access-7][7]

1. Zoek naar de bevestiging bel die wordt weergegeven op het scherm.

    ![acht-access-gegevens][8]

1. Kies **gebruiker selecteren**. Zoeken naar het e-mailadres van de gastgebruiker die u uitgenodigd om te vinden van de gebruiker die u wilt toevoegen. Klik op **Selecteer** de selectie te bevestigen.

    ![gegevens-access-9][9]

1. Kies **rol selecteren**. Kies de juiste rol voor de gastgebruiker:

    * Selecteer **Inzender** als u wilt toestaan dat de gebruiker referentiegegevens en delen die zijn opgeslagen query's en perspectieven met andere gebruikers van de omgeving wijzigen.

    * Selecteer anders **lezer** toestaan dat de gebruiker om gegevens te doorzoeken in de omgeving en persoonlijk, niet-gedeelde query's opslaan in de omgeving.

   Selecteer **OK** de keuze van de rol te bevestigen.

    ![gegevens-access-tien][10]

1. Selecteer **OK** op de **gebruikersrol selecteren** pagina.

1. Bevestig dat de **beleid voor gegevenstoegang** pagina geeft een lijst van de gastgebruiker en de functies voor elke gastgebruiker.

    ![gegevens-access-11][11]

1. Nu de gastgebruiker moet de stappen voor toegang tot de omgeving zich in de Azure-tenant waaraan u deze hebt uitgenodigd. Ze accepteren eerst de uitnodiging die u ze hebt verzonden. Deze uitnodiging is verzonden via e-mail naar het e-mailadres dat u in stap 5 hebt gebruikt. Ze selecteren **aan de slag** te accepteren.

    ![gegevens-access-twaalf][12]

1. De gastgebruiker accepteert vervolgens de machtigingen die zijn gekoppeld aan de organisatie van de beheerder.

    ![gegevens-access-13][13]

1. Nadat de gastgebruiker wordt ondertekend in naar het e-mailadres dat u hebt gebruikt om uit te nodigen, en ze de uitnodiging accepteren, ze naar insights.azure.com. Zodra er, ze selecteren de avatar naast het e-mailadres in de rechterbovenhoek van het scherm.

    ![gegevens-access-14][14]

1. Volgende, de Gast gebruiker selecteert de Azure-tenant van de directory in het menu. Deze tenant is de waaraan u deze hebt uitgenodigd.

    ![gegevens-access-vijftien][15]

Nadat de gastgebruiker selecteert uw tenant, zien ze de Time Series Insights-omgeving waarnaar u hebt opgegeven ze toegang. Ze beschikt nu over de mogelijkheden die zijn gekoppeld aan de rol die u hebt deze met in stap 8 opgegeven.

## <a name="next-steps"></a>Volgende stappen

* Informatie over [toevoegen van een Azure Event Hubs-gebeurtenisbron](./time-series-insights-how-to-add-an-event-source-eventhub.md) aan uw Time Series Insights-omgeving.
* Verzenden [gebeurtenissen naar de gebeurtenisbron](./time-series-insights-send-events.md).
* Weergave [uw omgeving in de Verkenner van Time Series Insights Preview](./time-series-insights-update-explorer.md).

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