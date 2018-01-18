---
title: Selfservice voor wachtwoordherstel rapporten - Azure Active Directory
description: Rapportage van Azure AD zelf uw wachtwoord opnieuw instellen gebeurtenissen
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
ms.custom: it-pro;seohack1
ms.openlocfilehash: ae9475218141f4fdf08ef6b39fb85f3b18711e61
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="reporting-options-for-azure-ad-password-management"></a>Rapportageopties voor Azure AD-wachtwoordbeheer

Na de implementatie veel organisaties willen weten hoe of als selfservice voor wachtwoordherstel (SSPR) echt wordt gebruikt. De rapportagefunctie waarmee Azure Active Directory (Azure AD) helpt u vragen beantwoorden met behulp van vooraf gedefinieerde rapporten. Als u bent op de juiste wijze een licentie, kunt u ook aangepaste query's maken.

![Rapportage][Reporting]

Kunnen de volgende vragen worden beantwoord door de rapporten die zijn opgenomen in de [Azure portal] (https://portal.azure.com/):

> [!NOTE]
> U moet [een globale beheerder](active-directory-assign-admin-roles-azure-portal.md), en u moet aanmelden voor deze gegevens worden verzameld namens uw organisatie. Als u wilt deelnemen, moet u bezoekt de **rapportage** aanmelden ten minste eenmaal tabblad of de controle. Tot die tijd worden geen gegevens verzameld voor uw organisatie.
>

* Hoeveel mensen hebben geregistreerd voor het wachtwoord opnieuw instellen?
* Die is geregistreerd voor het wachtwoord opnieuw instellen?
* Welke gegevens registreert personen?
* Hoeveel mensen dat op het in de afgelopen zeven dagen hun wachtwoord opnieuw instellen?
* Wat zijn de meest voorkomende methoden die gebruikers of beheerders gebruiken om hun wachtwoord opnieuw instellen?
* Wat zijn veelvoorkomende problemen met gebruikers of beheerders face bij een poging tot het gebruik van wachtwoord opnieuw instellen?
* Wat beheerders zijn opnieuw instellen van hun eigen wachtwoorden vaak?
* Is er een verdachte activiteit gebeurt met wachtwoord opnieuw instellen?

## <a name="power-bi-content-pack"></a>Power BI-inhoudspakket

Als u een Power BI-gebruiker bent, kunt u er een inhoudspakket voor Azure AD met eenvoudig te gebruiken voor SSPR reporting is. Zie voor meer informatie over het gebruik en de implementatie van het inhoudspakket [het gebruik van het Azure Active Directory Power BI-inhoudspakket](active-directory-reporting-power-bi-content-pack-how-to.md). Met het inhoudspakket kunt u uw eigen dashboards maken en ze delen met anderen in uw organisatie.

## <a name="how-to-view-password-management-reports-in-the-azure-portal"></a>Wachtwoord-rapporten weergeven in de Azure-portal

In de Azure portal ervaring hebben we een verbeterde de manier waarop u wachtwoordherstel weergeven kunt en registratie-activiteit wachtwoord opnieuw instellen. Gebruik de volgende de stappen voor het vinden van het wachtwoord opnieuw instellen en gebeurtenissen van de registratie voor wachtwoord opnieuw instellen:

1. Blader naar de [Azure-portal](https://portal.azure.com).
2. Selecteer **alle services** in het linkerdeelvenster.
3. Zoeken naar **Azure Active Directory** in de lijst met services en selecteer deze.
4. Selecteer **gebruikers en groepen**.
5. Selecteer **controlelogboeken** van de **gebruikers en groepen** menu. Hiermee geeft u alle van de controlegebeurtenissen die is opgetreden voor de gebruikers in uw directory. U kunt deze weergeven voor een overzicht van alle de wachtwoord-gebeurtenissen filteren.
6. Selecteer dit om weergave te filteren om alleen het wachtwoord opnieuw instellen van gerelateerde gebeurtenissen te bekijken, de **Filter** knop aan de bovenkant van het deelvenster.
7. Van de **Filter** selecteert u de **categorie** vervolgkeuzelijst weergeven en wijzigen in de **Self-service wachtwoordbeheer** categorietype.
8. Eventueel verder filter de lijst op de specifieke kiezen **activiteit** u geïnteresseerd bent in.

## <a name="how-to-retrieve-password-management-events-from-the-azure-ad-reports-and-events-api"></a>Het ophalen van gebeurtenissen voor wachtwoord van de Azure AD-rapporten en gebeurtenissen API

De Azure AD-rapporten en gebeurtenissen API biedt ondersteuning voor het ophalen van de gegevens die zijn opgenomen in het wachtwoord opnieuw instellen en rapporten voor registratie voor wachtwoord opnieuw instellen. U kunt met behulp van deze API afzonderlijke wachtwoord opnieuw instellen en het wachtwoord opnieuw instellen van inschrijving gebeurtenissen downloaden en de rapportage-technologie van uw keuze integreren.

> [!IMPORTANT]
> Op dit moment wordt de Azure AD-rapporten en gebeurtenissen API haalt maximaal *75.000 afzonderlijke gebeurtenissen* van de [SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent) en [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent) typen. De API omvat de afgelopen 30 dagen.
> 
> Als u wilt ophalen of gegevens buiten dit venster opslaan, het is raadzaam persistent maken in een externe database met behulp van de API om op te vragen van de delta's die het resultaat. Het is raadzaam dat u begint met het ophalen van deze gegevens wanneer u begint met behulp van SSPR in uw organisatie. Extern behouden en vervolgens doorgaan met het bijhouden van de delta's vanaf dat moment.
>

### <a name="how-to-get-started-with-the-reporting-api"></a>Hoe u aan de slag met de rapportage-API

Toegang tot deze gegevens, moet u een kleine toepassing of het script voor het ophalen van onze servers schrijven. Zie voor meer informatie [aan de slag met Azure AD rapportage-API](active-directory-reporting-api-getting-started.md).

Nadat u een werkscript hebt, moet u de wachtwoord opnieuw instellen en registratie gebeurtenissen onderzoeken die u ophalen kunt om te voldoen aan uw scenario's:

* [SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent): geeft een lijst van de beschikbare kolommen voor wachtwoord opnieuw instellen voor gebeurtenissen.
* [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent): geeft een lijst van de beschikbare kolommen voor gebeurtenissen van de registratie voor wachtwoord opnieuw instellen.

## <a name="description-of-the-report-columns-in-the-azure-portal"></a>Beschrijving van de rapportkolommen in de Azure portal

De volgende lijst wordt elk van de rapportkolommen in de Azure portal in detail uitgelegd:

* **Gebruiker**: de gebruiker die heeft geprobeerd een wachtwoord opnieuw instellen van bewerking voor de registratie.
* **Rol**: de rol van de gebruiker in de directory.
* **Datum en tijd**: de datum en tijd van de poging.
* **Geregistreerde gegevens**: de verificatiegegevens die registratie van de gebruiker is opgegeven tijdens het wachtwoord opnieuw instellen.

## <a name="description-of-the-report-values-in-the-azure-portal"></a>Beschrijving van de waarden van het rapport in de Azure-portal

De volgende tabel beschrijft de verschillende waarden die dat u kunt instellen voor elke kolom in de Azure-portal:

| Kolom | Toegestane waarden en hun betekenis |
| --- | --- |
| Gegevens die zijn geregistreerd |**Alternatief e-mailadres**: de gebruiker een alternatief e of e-verificatie gebruikt om te verifiëren.<p><p>**Telefoon (werk)**: de gebruiker een telefoon (werk) gebruikt om te verifiëren.<p>**Mobiele telefoon**: de gebruiker een mobiele telefoon of telefoon voor authenticatie gebruikt om te verifiëren.<p>**Beveiligingsvragen**: de gebruiker beveiligingsvragen gebruikt om te verifiëren.<p>**Een combinatie van de vorige methoden, bijvoorbeeld de alternatieve e-mail + mobiele telefoon**: treedt op wanneer een twee-gate-beleid is opgegeven en de gebruiker ziet u welke twee methoden voor verificatie aanvraag hun wachtwoord opnieuw instellen. |

## <a name="self-service-password-management-activity-types"></a>Wachtwoordbeheer activiteitstypen selfservice

De volgende activiteitstypen worden weergegeven in de **Self-Service wachtwoordbeheer** audit gebeurteniscategorie:

* [Selfservice voor wachtwoordherstel geblokkeerd](#activity-type-blocked-from-self-service-password-reset): geeft aan dat een gebruiker heeft geprobeerd een wachtwoord opnieuw instellen, een specifieke poort gebruiken of een telefoonnummer valideren meer dan vijf totale aantal keren in 24 uur.
* [Wachtwoord (self-service) wijzigen](#activity-type-change-password-self-service): geeft aan dat een gebruiker een vrijwillige uitgevoerd of (als gevolg van verlopen gedwongen) wijzigen van wachtwoorden.
* [Wachtwoord opnieuw instellen (door admin)](#activity-type-reset-password-by-admin): geeft aan dat een beheerder een namens een gebruiker vanuit de Azure-portal voor wachtwoordherstel uitgevoerd.
* [Wachtwoord opnieuw instellen (self-service)](#activity-type-reset-password-self-service): geeft aan dat een gebruiker met succes hun wachtwoord opnieuw instellen van de [portal opnieuw instellen van Azure AD-wachtwoord](https://passwordreset.microsoftonline.com).
* [Selfservice voor wachtwoordherstel stroom activiteit uitgevoerd](#activity-type-self-serve-password-reset-flow-activity-progress): geeft aan elke specifieke stap van een gebruiker doorlopen, zoals het doorgeven van een specifiek wachtwoord opnieuw instellen verificatiepoort, als onderdeel van het wachtwoord opnieuw instellen van proces.
* [Ontgrendelen van gebruikersaccount (self-service)](#activity-type-unlock-user-account-self-service): geeft aan dat een gebruiker de Active Directory-account is ontgrendeld zonder de fabrieksinstellingen van het wachtwoord van de [portal opnieuw instellen van Azure AD-wachtwoord](https://passwordreset.microsoftonline.com) met behulp van de actieve Directory-functie van het account ontgrendelen zonder opnieuw instellen.
* [Gebruiker is geregistreerd voor selfservice voor wachtwoordherstel](#activity-type-user-registered-for-self-service-password-reset): geeft aan dat een gebruiker de vereiste informatie om te kunnen hun wachtwoord in overeenstemming met het beleid voor wachtwoordherstel momenteel opgegeven tenant is geregistreerd.

### <a name="activity-type-blocked-from-self-service-password-reset"></a>Activiteitstype: geen toegang tot selfservice voor wachtwoordherstel

De volgende lijst wordt deze activiteit in detail uitgelegd:

* **Beschrijving van de activiteit**: geeft aan dat een gebruiker heeft geprobeerd een wachtwoord opnieuw instellen, een specifieke poort gebruiken of een telefoonnummer valideren meer dan vijf totale aantal keren in 24 uur.
* **Activiteit actor**: de gebruiker die is beperkt uit te voeren aanvullende bewerkingen opnieuw instellen. De gebruiker kan een eindgebruiker of een beheerder zijn.
* **Doel van de activiteit**: de gebruiker die is beperkt uit te voeren aanvullende bewerkingen opnieuw instellen. De gebruiker kan een eindgebruiker of een beheerder zijn.
* **De activiteitsstatus**:
  * _Geslaagde_: geeft aan dat een gebruiker is beperkt van eventuele aanvullende opnieuw uitvoert, probeert een extra verificatiemethoden of eventuele extra telefoonnummers valideren voor de eerstvolgende 24 uur.
* **Reden van de activiteit status fout**: niet van toepassing.

### <a name="activity-type-change-password-self-service"></a>Activiteitstype: wachtwoord wijzigen (self-service)

De volgende lijst wordt deze activiteit in detail uitgelegd:

* **Beschrijving van de activiteit**: geeft aan dat een gebruiker een vrijwillige uitgevoerd of (als gevolg van verlopen gedwongen) wijzigen van wachtwoorden.
* **Activiteit actor**: de gebruiker die hun wachtwoord gewijzigd. De gebruiker kan een eindgebruiker of een beheerder zijn.
* **Doel van de activiteit**: de gebruiker die hun wachtwoord gewijzigd. De gebruiker kan een eindgebruiker of een beheerder zijn.
* **Activiteit statussen**:
  * _Geslaagde_: geeft aan dat een gebruiker het wachtwoord is gewijzigd.
  * _Fout_: geeft aan dat een gebruiker is mislukt om hun wachtwoord te wijzigen. U kunt de rij om te zien selecteren de **activiteit statusreden** categorie voor meer informatie over waarom de fout is opgetreden.
* **Reden van de activiteit status fout**: 
  * _FuzzyPolicyViolationInvalidPassword_: de gebruiker een wachtwoord dat automatisch is geweigerd omdat de detectiemogelijkheden Microsoft verboden wachtwoord geconstateerd dat te veel voorkomt of vooral zwakke geselecteerd.

### <a name="activity-type-reset-password-by-admin"></a>Activiteitstype: wachtwoord opnieuw instellen (door de beheerder)

De volgende lijst wordt deze activiteit in detail uitgelegd:

* **Beschrijving van de activiteit**: geeft aan dat een beheerder een namens een gebruiker vanuit de Azure-portal voor wachtwoordherstel uitgevoerd.
* **Activiteit actor**: de beheerder die het wachtwoord opnieuw instellen namens een andere gebruiker of beheerder uitgevoerd. Moet een globale beheerder, een wachtwoordbeheerder, een Gebruikersbeheerder of een beheerder van de helpdesk.
* **Doel van de activiteit**: de gebruiker waarvan het wachtwoord is opnieuw ingesteld. De gebruiker kan een eindgebruiker of een andere beheerder zijn.
* **Activiteit statussen**:
  * _Geslaagde_: geeft aan dat een beheerder is opnieuw ingesteld wachtwoord van een gebruiker.
  * _Fout_: geeft aan dat een beheerder kan niet het wachtwoord van een gebruiker wijzigen. U kunt de rij om te zien selecteren de **activiteit statusreden** categorie voor meer informatie over waarom de fout is opgetreden.

### <a name="activity-type-reset-password-self-service"></a>Activiteitstype: wachtwoord opnieuw instellen (self-service)

De volgende lijst wordt deze activiteit in detail uitgelegd:

* **Beschrijving van de activiteit**: geeft aan dat een gebruiker met succes hun wachtwoord opnieuw instellen van de [portal opnieuw instellen van Azure AD-wachtwoord](https://passwordreset.microsoftonline.com).
* **Activiteit actor**: de gebruiker die hun wachtwoord opnieuw instellen. De gebruiker kan een eindgebruiker of een beheerder zijn.
* **Doel van de activiteit**: de gebruiker die hun wachtwoord opnieuw instellen. De gebruiker kan een eindgebruiker of een beheerder zijn.
* **Activiteit statussen**:
  * _Geslaagde_: geeft aan dat een gebruiker hun eigen wachtwoord gereset.
  * _Fout_: geeft aan dat een gebruiker hun eigen wachtwoord opnieuw instellen is mislukt. U kunt de rij om te zien selecteren de **activiteit statusreden** categorie voor meer informatie over waarom de fout is opgetreden.
* **Reden van de activiteit status fout**: 
  * _FuzzyPolicyViolationInvalidPassword_: de beheerder een wachtwoord dat automatisch is geweigerd omdat de detectiemogelijkheden Microsoft verboden wachtwoord geconstateerd dat te veel voorkomt of vooral zwakke geselecteerd.

### <a name="activity-type-self-serve-password-reset-flow-activity-progress"></a>Activiteitstype: eigen wachtwoord opnieuw instellen van stroom activiteit uitgevoerd beheer

De volgende lijst wordt deze activiteit in detail uitgelegd:

* **Beschrijving van de activiteit**: geeft aan elke specifieke stap van een gebruiker doorlopen (zoals het doorgeven van een specifiek wachtwoord opnieuw instellen van verificatiepoort) als onderdeel van het wachtwoord opnieuw instellen van proces.
* **Activiteit actor**: de gebruiker die heeft uitgevoerd deel van het wachtwoord opnieuw instellen van stroom. De gebruiker kan een eindgebruiker of een beheerder zijn.
* **Doel van de activiteit**: de gebruiker die heeft uitgevoerd deel van het wachtwoord opnieuw instellen van stroom. De gebruiker kan een eindgebruiker of een beheerder zijn.
* **Activiteit statussen**:
  * _Geslaagde_: geeft aan dat een gebruiker is een specifieke stap van het wachtwoord opnieuw instellen van stroom voltooid.
  * _Fout_: geeft aan dat een specifieke stap van het wachtwoord opnieuw instellen van stroom is mislukt. U kunt de rij om te zien selecteren de **activiteit statusreden** categorie voor meer informatie over waarom de fout is opgetreden.
* **Activiteit statusredenen**: Zie de volgende tabel voor [alle de toegestane reset activiteit statusredenen](#allowed-values-for-details-column).

### <a name="activity-type-unlock-a-user-account-self-service"></a>Activiteitstype: een gebruikersaccount (self-service) ontgrendelen

De volgende lijst wordt deze activiteit in detail uitgelegd:

* **Beschrijving van de activiteit**: geeft aan dat een gebruiker de Active Directory-account is ontgrendeld zonder de fabrieksinstellingen van het wachtwoord van de [portal opnieuw instellen van Azure AD-wachtwoord](https://passwordreset.microsoftonline.com) met behulp van de functie Active Directory van account ontgrendelen zonder opnieuw instellen.
* **Activiteit actor**: de gebruiker die het account ontgrendeld zonder hun wachtwoord opnieuw instellen. De gebruiker kan een eindgebruiker of een beheerder zijn.
* **Doel van de activiteit**: de gebruiker die het account ontgrendeld zonder hun wachtwoord opnieuw instellen. De gebruiker kan een eindgebruiker of een beheerder zijn.
* **Activiteit statussen toegestaan**:
  * _Geslaagde_: geeft aan dat een gebruiker is een eigen account ontgrendeld.
  * _Fout_: geeft aan dat een gebruiker is mislukt om hun account te ontgrendelen. U kunt de rij om te zien selecteren de **activiteit statusreden** categorie voor meer informatie over waarom de fout is opgetreden.

### <a name="activity-type-user-registered-for-self-service-password-reset"></a>Activiteitstype: gebruiker is geregistreerd voor selfservice voor wachtwoordherstel

De volgende lijst wordt deze activiteit in detail uitgelegd:

* **Beschrijving van de activiteit**: geeft aan dat een gebruiker de vereiste informatie om te kunnen hun wachtwoord in overeenstemming met het beleid voor wachtwoordherstel momenteel opgegeven tenant is geregistreerd. 
* **Activiteit actor**: de gebruiker die zich hebben geregistreerd voor wachtwoordherstel. De gebruiker kan een eindgebruiker of een beheerder zijn.
* **Doel van de activiteit**: de gebruiker die zich hebben geregistreerd voor wachtwoordherstel. De gebruiker kan een eindgebruiker of een beheerder zijn.
* **Activiteit statussen toegestaan**:
  * _Geslaagde_: geeft aan dat een gebruiker is geregistreerd voor wachtwoordherstel in overeenstemming met het huidige beleid. 
  * _Fout_: geeft aan dat een gebruiker kan niet worden geregistreerd voor wachtwoordherstel. U kunt de rij om te zien selecteren de **activiteit statusreden** categorie voor meer informatie over waarom de fout is opgetreden. 

     >[!NOTE]
     >Fout betekent niet dat een gebruiker zich niet in te stellen hun eigen wachtwoord. Dit betekent dat ze het registratieproces is niet voltooid. Als er niet-geverifieerde gegevens op hun account dat juist is, zoals een telefoonnummer op dat niet is gevalideerd, zelfs als ze dit telefoonnummer niet geverifieerd, kunnen ze het nog steeds gebruiken hun wachtwoord opnieuw instellen. Zie voor meer informatie [wat er gebeurt wanneer een gebruiker registreert?](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-learn-more#what-happens-when-a-user-registers).
     >

## <a name="next-steps"></a>Volgende stappen

* [Hoe kan ik een geslaagde implementatie van SSPR voltooien?](active-directory-passwords-best-practices.md)
* [Uw wachtwoord opnieuw instellen of wijzigen](active-directory-passwords-update-your-own-password.md).
* [Registreer u voor selfservice voor wachtwoordherstel](active-directory-passwords-reset-register.md).
* [Hebt u een vraag over licenties?](active-directory-passwords-licensing.md)
* [Welke gegevens worden gebruikt door selfservice voor wachtwoordherstel en welke gegevens moet u voor uw gebruikers invullen?](active-directory-passwords-data.md)
* [Welke verificatiemethoden zijn beschikbaar voor gebruikers?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Wat zijn de beleidsopties bij selfservice voor wachtwoordherstel?](active-directory-passwords-policy.md)
* [Wat is Wachtwoord terugschrijven en waarom is dit van belang?](active-directory-passwords-writeback.md)
* [Wat zijn alle opties in selfservice voor wachtwoordherstel en wat houden ze in?](active-directory-passwords-how-it-works.md)
* [Ik denk dat er iets misgaat. Hoe los ik problemen in selfservice voor wachtwoordherstel op?](active-directory-passwords-troubleshoot.md)
* [Ik heb een vraag die nog niet is beantwoord](active-directory-passwords-faq.md)

[Reporting]: ./media/active-directory-passwords-reporting/sspr-reporting.png "Voorbeeld van SSPR activiteit audit wordt geregistreerd in Azure AD"
