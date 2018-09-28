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
ms.openlocfilehash: c9bddf8e20524433b31793e277efd954a5d1320e
ms.sourcegitcommit: 42405ab963df3101ee2a9b26e54240ffa689f140
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47423371"
---
# <a name="grant-data-access-to-a-time-series-insights-environment-using-azure-portal"></a>Gegevenstoegang verlenen tot een Time Series Insights-omgeving met Azure Portal

Dit artikel worden de twee typen beleid voor Time Series Insights-toegang.

## <a name="video"></a>Video: 

### <a name="in-this-video-we-cover-creating-and-managing-access-policies-within-time-series-insights-br"></a>In deze video behandelen we het maken en beheren van toegangsbeleid binnen de Time Series Insights. </br>

> [!VIDEO https://www.youtube.com/embed/5zTIdyHMJW8]

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

## <a name="provide-guest-access-to-a-user-from-another-aad-tenant"></a>Toegang voor gasten bieden aan een gebruiker van een andere AAD-tenant

'Gast' is niet een beheerrol; het is een term die wordt gebruikt voor een account dat wordt uitgenodigd van één tenant naar een andere. Nadat de Gast-account is uitgenodigd naar de map van de tenant, kan deze hebben de dezelfde toegangsbeheer toegepast, zoals een andere account management om toegang te verlenen aan een TSI-omgeving met behulp van de Access Control (IAM)-blade of toegang verlenen tot de gegevens in de omgeving via de blade beleid voor gegevenstoegang. Zie voor meer informatie over de toegang voor gasten van AAD-tenant, [document](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator).

Volg deze stappen voor het verlenen van toegang voor gasten met een Time Series Insights-omgeving naar een AAD-gebruiker van een andere tenant:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Ga naar uw Time Series Insights-omgeving. Type **Time Series** in de **zoeken** vak. Selecteer **Time Series-omgeving** in de lijst met zoekresultaten.

3. Selecteer uw Time Series Insights-omgeving in de lijst.

4. Selecteer **beleid voor gegevenstoegang**en selecteer vervolgens + **uitnodigen**.

    ![Beheren van de Time Series Insights-bron - gebruiker uitnodigen](media/data-access/getstarted-grant-data-access6.png)

5. Bieden van de gebruiker e-mail die u wilt uitnodigen. Let op: dit moet een e-mailbericht dat is gekoppeld aan AAD. U kunt desgewenst een persoonlijk bericht invoegen bij de uitnodiging opnemen.

    ![Beheren van de Time Series Insights-bron - gebruiker selecteren](media/data-access/getstarted-grant-data-access7.png)

6. U ziet een bevestiging bel worden weergegeven op het scherm.

    ![Beheren van de Time Series Insights-bron - gebruiker bevestigen](media/data-access/getstarted-grant-data-access8.png)

7. Selecteer **gebruiker selecteren**. Zoeken naar het e-mailadres van de gastgebruiker die u zojuist hebt uitgenodigd om te vinden van de gebruiker die u wilt toevoegen. Klik op **Selecteer** de selectie te bevestigen.
  
    ![Beheren van de Time Series Insights-bron - gebruiker bevestigen](media/data-access/getstarted-grant-data-access9.png)

8. Selecteer **rol selecteren**. Kies de juiste rol voor de gastgebruiker:

    * Selecteer **Inzender** als u wilt toestaan dat de gebruiker referentiegegevens en delen die zijn opgeslagen query's en perspectieven met andere gebruikers van de omgeving wijzigen.

    * Selecteer anders **lezer** querygegevens van gebruikers toestaan in de omgeving en persoonlijke (niet-gedeelde) query's opslaan in de omgeving.

    Selecteer **Ok** de keuze van de rol te bevestigen.

    ![Beheren van de Time Series Insights-bron - rol selecteren](media/data-access/getstarted-grant-data-access10.png)

9. Selecteer **Ok** in de **gebruikersrol selecteren** pagina.

10. De **beleid voor gegevenstoegang** pagina geeft nu de gastgebruiker en de rollen zijn voor elke gastgebruiker.

    ![Beheren van de Time Series Insights-bron - rol bevestigen](media/data-access/getstarted-grant-data-access11.png)

11. Nu de gastgebruiker nemen bepaalde stappen moet voor toegang tot de omgeving zich bevindt in de Azure-tenant u zojuist hebt uitgenodigd laten. Eerst moeten ze accepteren van de uitnodiging die ze zojuist hebt verzonden. Deze uitnodiging is verzonden via e-mail naar het e-mailadres dat u in stap 5 hebt uitgenodigd. Ze klikt op 'Aan de slag' om te accepteren.

    ![Beheren van de Time Series Insights-bron - gebruiker uitnodigen](media/data-access/getstarted-grant-data-access12.png)

12. De gastgebruiker moet vervolgens de machtigingen die zijn gekoppeld aan de organisatie van de beheerder accepteren.

    ![Beheren van de Time Series Insights-bron - machtigingen accepteren](media/data-access/getstarted-grant-data-access13.png)

13. Wanneer de gastgebruiker is aangemeld bij het e-mailadres u uitgenodigd, en ze de uitnodiging accepteren, ze zullen raadpleegt u insights.azure.com. Zodra er, ze moet klikt u op de avatar naast hun e-mailadres in de rechterbovenhoek van het scherm. 

    ![Beheren van de Time Series Insights-bron - machtigingen accepteren](media/data-access/getstarted-grant-data-access14.png)

14. De gastgebruiker Selecteer vervolgens wordt uw Azure-tenant van de directory in het menu. Dit is de tenant die u hebt deze uitgenodigd. 

    ![Beheren van de Time Series Insights-bron - machtigingen accepteren](media/data-access/getstarted-grant-data-access15.png)

15. Wanneer de gastgebruiker selecteert uw tenant, zien ze ten slotte de Time Series Insights-omgeving die u zojuist hebt verstrekt ze toegang tot. Ze moeten nu alle mogelijkheden die zijn gekoppeld aan de rol die is opgegeven in stap 8 hebben.

## <a name="next-steps"></a>Volgende stappen
* Informatie over [een Event Hub-gebeurtenisbron toevoegen aan uw Azure Time Series Insights-omgeving](time-series-insights-how-to-add-an-event-source-eventhub.md).
* [Gebeurtenissen verzenden](time-series-insights-send-events.md) naar de gebeurtenisbron.
* Uw omgeving bekijken in [Verkenner van Time Series Insights](https://insights.timeseries.azure.com).
