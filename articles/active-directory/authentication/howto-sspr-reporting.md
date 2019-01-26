---
title: Self-service voor wachtwoord opnieuw instellen van rapporten - Azure Active Directory
description: Rapportage over Azure AD Self-service voor wachtwoord opnieuw instellen van gebeurtenissen
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.openlocfilehash: 891ab7ce0218e3532b8e503bc9b06da04a13d2c4
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/26/2019
ms.locfileid: "55074544"
---
# <a name="reporting-options-for-azure-ad-password-management"></a>Rapportage-opties voor Azure AD-wachtwoordbeheer

Na de implementatie in veel organisaties willen weten hoe of als selfservice voor wachtwoordherstel (SSPR) echt wordt gebruikt. De rapportagefunctie die Azure Active Directory (Azure AD) biedt vindt u antwoorden op vragen met behulp van vooraf gedefinieerde rapporten. Als u op de juiste wijze in licentie gegeven bent, kunt u ook aangepaste query's maken.

![Rapportage][Reporting]

De volgende vragen kunnen worden beantwoord door de rapporten die zijn opgenomen in de [Azure-portal](https://portal.azure.com/):

> [!NOTE]
> U moet [een globale beheerder](../users-groups-roles/directory-assign-admin-roles.md), en u moet zich aanmelden voor deze gegevens worden verzameld namens uw organisatie. Als u wilt deelnemen, gaat u naar de **rapportage** tabblad of de audit ten minste één keer worden geregistreerd. Tot die tijd worden geen gegevens verzameld voor uw organisatie.
>

* Hoeveel mensen hebben geregistreerd voor wachtwoord opnieuw instellen?
* Die is geregistreerd voor wachtwoord opnieuw instellen?
* Welke gegevens worden door personen registreren?
* Het aantal mensen dat op het in de afgelopen zeven dagen hun wachtwoord opnieuw instellen?
* Wat zijn de meest voorkomende methoden die gebruikmaken van gebruikers of beheerders hun wachtwoord opnieuw instellen?
* Wat zijn de algemene problemen met gebruikers of beheerders face wanneer u probeert te gebruiken voor wachtwoord opnieuw instellen?
* Wat beheerders zijn opnieuw instellen van hun eigen wachtwoorden vaak?
* Is er een verdachte activiteit gebeurt met wachtwoord opnieuw instellen?

## <a name="power-bi-content-pack"></a>Power BI-inhoudspakket

Als u een Power BI-gebruiker bent, moet u er een inhoudspakket voor Azure AD met eenvoudig te gebruiken, rapportage voor SSPR is. Zie voor meer informatie over het gebruik en de implementatie van het inhoudspakket [over het gebruik van de Azure Active Directory Power BI-inhoudspakket](../reports-monitoring/howto-power-bi-content-pack.md). Met het inhoudspakket kunt u uw eigen dashboards maken en deze delen met anderen in uw organisatie.

## <a name="how-to-view-password-management-reports-in-the-azure-portal"></a>Wachtwoord-rapporten weergeven in de Azure-portal

In de Azure portal-ervaring, zijn de manier waarop u voor wachtwoord opnieuw instellen bekijken kunt en registratie-activiteit wachtwoord opnieuw instellen van vereisten verbeterd. Gebruik de volgende de stappen voor het vinden van het wachtwoord opnieuw instellen en registratie van gebeurtenissen voor wachtwoord opnieuw instellen:

1. Blader naar de [Azure-portal](https://portal.azure.com).
2. Selecteer **alle services** in het linkerdeelvenster.
3. Zoeken naar **Azure Active Directory** in de lijst met services en selecteer deze.
4. Selecteer **Gebruikers en groepen**.
5. Selecteer **auditlogboeken** uit de **gebruikers en groepen** menu. Dit ziet u alle van de controlegebeurtenissen die is opgetreden op basis van alle gebruikers in uw directory. U kunt deze weergave om te zien van alle gebeurtenissen met betrekking tot wachtwoord filteren.
6. Als u wilt filteren in deze weergave om te zien alleen de gebeurtenissen in verband met wachtwoord opnieuw instellen, selecteert u de **Filter** knop aan de bovenkant van het deelvenster.
7. Uit de **Filter** in het menu het **categorie** vervolgkeuzelijst lijst en wijzig deze in de **Self-service wachtwoordbeheer** categorietype.
8. (Optioneel) de lijst met door het kiezen van de specifieke verder filteren **activiteit** u geïnteresseerd bent in.

### <a name="converged-registration-preview"></a>Geconvergeerde registratie (preview)

Als u de openbare preview van geconvergeerde registratie deelneemt, gegevens over gebruikersactiviteit in de auditlogboeken gevonden onder de categorie **verificatiemethoden**.

## <a name="description-of-the-report-columns-in-the-azure-portal"></a>Beschrijving van de rapportkolommen in de Azure-portal

De volgende lijst wordt elk van de kolommen in de Azure portal in detail uitgelegd:

* **Gebruiker**: De gebruiker die heeft geprobeerd een wachtwoord opnieuw instellen van bewerking voor de registratie.
* **Rol**: De rol van de gebruiker in de map.
* **Datum en tijd**: De datum en tijd van de poging.
* **Gegevens die zijn geregistreerd**: Registratie van de verificatiegegevens die de gebruiker tijdens het wachtwoord opgegeven opnieuw instellen.

## <a name="description-of-the-report-values-in-the-azure-portal"></a>Beschrijving van de waarden van het rapport in de Azure-portal

De volgende tabel beschrijft de verschillende waarden die dat u kunt instellen voor elke kolom in de Azure-portal:

| Kolom | Toegestane waarden en hun betekenis |
| --- | --- |
| Gegevens die zijn geregistreerd |**Alternatief e-mailadres**: De gebruiker gebruikt een alternatief e-mailadres of verificatieadres voor verificatie.<p><p>**Telefoon (werk)**: De gebruiker gebruikt een telefoon (werk) om te verifiëren.<p>**Mobiele telefoon**: De gebruiker gebruikt een mobiele telefoon of telefoon voor authenticatie voor verificatie.<p>**Vragen over de beveiliging**: De gebruiker gebruikt vragen over de beveiliging voor verificatie.<p>**Een combinatie van de vorige methoden, bijvoorbeeld, alternatief e-mail + mobiele telefoon**: Treedt op wanneer een beleid voor twee-poort is opgegeven en ziet u welke twee methoden voor de gebruiker gebruikt voor verificatie verzoek hun wachtwoord opnieuw instellen. |

## <a name="self-service-password-management-activity-types"></a>Selfservice wachtwoordbeheer activiteitstypen

De volgende activiteitstypen worden weergegeven in de **Self-Service wachtwoordbeheer** categorie audit-gebeurtenis:

* [Self-service voor wachtwoord opnieuw instellen geblokkeerd](#activity-type-blocked-from-self-service-password-reset): Geeft aan dat een gebruiker heeft geprobeerd een wachtwoord opnieuw instellen, gebruikt u een specifieke poort of een telefoonnummer in dat meer dan vijf totale aantal keren dat in 24 uur valideren.
* [Wachtwoord wijzigen (selfservice)](#activity-type-change-password-self-service): Geeft aan dat een gebruiker vrijwillig uitgevoerd of (als gevolg van verlopen geforceerde) wachtwoord wijzigen.
* [Wachtwoord opnieuw instellen (door beheerder)](#activity-type-reset-password-by-admin): Geeft aan dat een beheerder een namens een gebruiker vanuit de Azure-portal voor wachtwoordherstel uitgevoerd.
* [Wachtwoord opnieuw instellen (selfservice)](#activity-type-reset-password-self-service): Geeft aan dat een gebruiker is hun wachtwoord opnieuw instellen van de [Azure AD portal voor wachtwoordherstel](https://passwordreset.microsoftonline.com).
* [Self-service voor wachtwoord opnieuw instellen van stroomactiviteitvoortgang](#activity-type-self-serve-password-reset-flow-activity-progress): Geeft aan dat elke specifieke stap die van een gebruiker wordt uitgevoerd in, zoals een specifiek wachtwoord te geven verificatiepoort, opnieuw worden ingesteld als onderdeel van het proces voor wachtwoordherstel.
* [Gebruikersaccount ontgrendelen (selfservice)](#activity-type-unlock-user-account-self-service): Geeft aan dat een gebruiker het Active Directory-account is ontgrendeld zonder hun wachtwoord opnieuw in te stellen de [Azure AD portal voor wachtwoordherstel](https://passwordreset.microsoftonline.com) met behulp van de functie Active Directory van account ontgrendelen zonder opnieuw instellen.
* [De gebruiker geregistreerd voor selfservice voor wachtwoordherstel](#activity-type-user-registered-for-self-service-password-reset): Geeft aan dat een gebruiker de vereiste informatie om te kunnen hun wachtwoord in overeenstemming met het hulpprogramma voor het opnieuw instellen van de opgegeven tenant-wachtwoordbeleid is geregistreerd.

### <a name="activity-type-blocked-from-self-service-password-reset"></a>Activiteitstype: Selfservice voor wachtwoord opnieuw instellen geblokkeerd

De volgende lijst wordt deze activiteit in detail uitgelegd:

* **Beschrijving van de activiteit**: Geeft aan dat een gebruiker heeft geprobeerd een wachtwoord opnieuw instellen, gebruikt u een specifieke poort of een telefoonnummer in dat meer dan vijf totale aantal keren dat in 24 uur valideren.
* **Activiteit actor**: De gebruiker die wordt beperkt uit te voeren extra resetbewerkingen. De gebruiker kan een eindgebruiker of een beheerder zijn.
* **Doel van de activiteit**: De gebruiker die wordt beperkt uit te voeren extra resetbewerkingen. De gebruiker kan een eindgebruiker of een beheerder zijn.
* **Activiteitsstatus**:
  * _Succes_: Geeft aan dat een gebruiker van een extra opnieuw instellen van wachtwoorden uitvoeren, probeert een extra verificatiemethoden of eventuele extra telefoonnummers valideren voor de volgende 24 uur wordt beperkt.
* **Activiteit status van de reden van fout**: Niet van toepassing.

### <a name="activity-type-change-password-self-service"></a>Activiteitstype: Wachtwoord wijzigen (selfservice)

De volgende lijst wordt deze activiteit in detail uitgelegd:

* **Beschrijving van de activiteit**: Geeft aan dat een gebruiker vrijwillig uitgevoerd of (als gevolg van verlopen geforceerde) wachtwoord wijzigen.
* **Activiteit actor**: De gebruiker die hun wachtwoord gewijzigd. De gebruiker kan een eindgebruiker of een beheerder zijn.
* **Doel van de activiteit**: De gebruiker die hun wachtwoord gewijzigd. De gebruiker kan een eindgebruiker of een beheerder zijn.
* **Status van de activiteit**:
  * _Succes_: Geeft aan dat een gebruiker het wachtwoord is gewijzigd.
  * _Fout_: Geeft aan dat een gebruiker is mislukt om hun wachtwoord te wijzigen. U kunt de rij om te zien de **reden van de activiteit status** categorie voor meer informatie over waarom de fout is opgetreden.
* **Activiteit status van de reden van fout**: 
  * _FuzzyPolicyViolationInvalidPassword_: De gebruiker een wachtwoord dat automatisch is geblokkeerd omdat de detectiemogelijkheden Microsoft verboden wachtwoord gevonden worden te veel voorkomt of vooral zwakke worden geselecteerd.

### <a name="activity-type-reset-password-by-admin"></a>Activiteitstype: Wachtwoord opnieuw instellen (door beheerder)

De volgende lijst wordt deze activiteit in detail uitgelegd:

* **Beschrijving van de activiteit**: Geeft aan dat een beheerder een namens een gebruiker vanuit de Azure-portal voor wachtwoordherstel uitgevoerd.
* **Activiteit actor**: De beheerder van het wachtwoord opnieuw instellen van namens een andere gebruiker of beheerder uitgevoerd. Moet een globale beheerder, wachtwoordbeheerder, Gebruikerbeheerder, of helpdesk-beheerder.
* **Doel van de activiteit**: De gebruiker waarvan het wachtwoord is opnieuw ingesteld. De gebruiker kan een eindgebruiker of een andere beheerder zijn.
* **Status van de activiteit**:
  * _Succes_: Geeft aan dat een beheerder is opnieuw ingesteld wachtwoord van een gebruiker.
  * _Fout_: Geeft aan dat een beheerder kan niet worden gewijzigd wachtwoord van een gebruiker. U kunt de rij om te zien de **reden van de activiteit status** categorie voor meer informatie over waarom de fout is opgetreden.

### <a name="activity-type-reset-password-self-service"></a>Activiteitstype: Wachtwoord opnieuw instellen (selfservice)

De volgende lijst wordt deze activiteit in detail uitgelegd:

* **Beschrijving van de activiteit**: Geeft aan dat een gebruiker is hun wachtwoord opnieuw instellen van de [Azure AD portal voor wachtwoordherstel](https://passwordreset.microsoftonline.com).
* **Activiteit actor**: De gebruiker die hun wachtwoord opnieuw instellen. De gebruiker kan een eindgebruiker of een beheerder zijn.
* **Doel van de activiteit**: De gebruiker die hun wachtwoord opnieuw instellen. De gebruiker kan een eindgebruiker of een beheerder zijn.
* **Status van de activiteit**:
  * _Succes_: Geeft aan dat een gebruiker is hun eigen wachtwoord opnieuw.
  * _Fout_: Geeft aan dat een gebruiker kan niet opnieuw instellen van hun eigen wachtwoord. U kunt de rij om te zien de **reden van de activiteit status** categorie voor meer informatie over waarom de fout is opgetreden.
* **Activiteit status van de reden van fout**: 
  * _FuzzyPolicyViolationInvalidPassword_: De beheerder een wachtwoord dat automatisch is geblokkeerd omdat de detectiemogelijkheden Microsoft verboden wachtwoord gevonden worden te veel voorkomt of vooral zwakke worden geselecteerd.

### <a name="activity-type-self-serve-password-reset-flow-activity-progress"></a>Activiteitstype: Eigen beheer stroomactiviteitvoortgang voor wachtwoordherstel

De volgende lijst wordt deze activiteit in detail uitgelegd:

* **Beschrijving van de activiteit**: Geeft aan dat elke specifieke stap die van een gebruiker wordt uitgevoerd in (zoals het doorgeven van een specifiek wachtwoord opnieuw instellen van de verificatiepoort) als onderdeel van het proces voor wachtwoordherstel.
* **Activiteit actor**: De gebruiker die heeft uitgevoerd, onderdeel van het wachtwoord opnieuw instellen van flow. De gebruiker kan een eindgebruiker of een beheerder zijn.
* **Doel van de activiteit**: De gebruiker die heeft uitgevoerd, onderdeel van het wachtwoord opnieuw instellen van flow. De gebruiker kan een eindgebruiker of een beheerder zijn.
* **Status van de activiteit**:
  * _Succes_: Geeft aan dat een specifieke stap van de stroom van wachtwoord opnieuw instellen die door een gebruiker is voltooid.
  * _Fout_: Geeft aan dat een specifieke stap van het wachtwoord opnieuw instellen van stroom is mislukt. U kunt de rij om te zien de **reden van de activiteit status** categorie voor meer informatie over waarom de fout is opgetreden.
* **Activiteit statusredenen**:   Zie de volgende tabel voor [alle toegestane reset activiteit statusredenen](#allowed-values-for-details-column).

### <a name="activity-type-unlock-a-user-account-self-service"></a>Activiteitstype: Een gebruikersaccount ontgrendelen (selfservice)

De volgende lijst wordt deze activiteit in detail uitgelegd:

* **Beschrijving van de activiteit**: Geeft aan dat een gebruiker het Active Directory-account is ontgrendeld zonder hun wachtwoord opnieuw in te stellen de [Azure AD portal voor wachtwoordherstel](https://passwordreset.microsoftonline.com) met behulp van de functie Active Directory van account ontgrendelen zonder opnieuw instellen.
* **Activiteit actor**: De gebruiker die hun account ontgrendelen zonder hun wachtwoord opnieuw instellen. De gebruiker kan een eindgebruiker of een beheerder zijn.
* **Doel van de activiteit**: De gebruiker die hun account ontgrendelen zonder hun wachtwoord opnieuw instellen. De gebruiker kan een eindgebruiker of een beheerder zijn.
* **Activiteit statussen toegestaan**:
  * _Succes_: Geeft aan dat een gebruiker is hun eigen account ontgrendeld.
  * _Fout_: Geeft aan dat een gebruiker is mislukt om hun account te ontgrendelen. U kunt de rij om te zien de **reden van de activiteit status** categorie voor meer informatie over waarom de fout is opgetreden.

### <a name="activity-type-user-registered-for-self-service-password-reset"></a>Activiteitstype: Door gebruiker geregistreerd voor selfservice voor het opnieuw instellen van een wachtwoord

De volgende lijst wordt deze activiteit in detail uitgelegd:

* **Beschrijving van de activiteit**: Geeft aan dat een gebruiker de vereiste informatie om te kunnen hun wachtwoord in overeenstemming met het hulpprogramma voor het opnieuw instellen van de opgegeven tenant-wachtwoordbeleid is geregistreerd. 
* **Activiteit actor**: De gebruiker die zich hebben geregistreerd voor wachtwoord opnieuw instellen. De gebruiker kan een eindgebruiker of een beheerder zijn.
* **Doel van de activiteit**: De gebruiker die zich hebben geregistreerd voor wachtwoord opnieuw instellen. De gebruiker kan een eindgebruiker of een beheerder zijn.
* **Activiteit statussen toegestaan**:
  * _Succes_: Geeft aan dat een gebruiker is geregistreerd voor wachtwoord opnieuw instellen in overeenstemming met het huidige beleid. 
  * _Fout_: Geeft aan dat een gebruiker kan niet registreren voor wachtwoord opnieuw instellen. U kunt de rij om te zien de **reden van de activiteit status** categorie voor meer informatie over waarom de fout is opgetreden. 

     >[!NOTE]
     >Fout betekent niet dat een gebruiker zich niet op hun eigen wachtwoord opnieuw instellen. Dit betekent dat ze het registratieproces is niet voltooid. Als er niet-geverifieerde gegevens op het account dat klopt, zoals een telefoonnummer op dat niet wordt gevalideerd, zelfs als ze dit telefoonnummer niet hebben geverifieerd, kan nog steeds worden gebruikt voor hun wachtwoord opnieuw instellen. Zie voor meer informatie, [wat er gebeurt wanneer een gebruiker registreert?](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-learn-more#what-happens-when-a-user-registers).
     >

## <a name="next-steps"></a>Volgende stappen

* [Hoe kan ik een geslaagde implementatie van SSPR voltooien?](howto-sspr-deployment.md)
* [Uw wachtwoord opnieuw instellen of wijzigen](../user-help/active-directory-passwords-update-your-own-password.md).
* [Registreer u voor selfservice voor wachtwoordherstel](../user-help/active-directory-passwords-reset-register.md).
* [Hebt u een vraag over licenties?](concept-sspr-licensing.md)
* [Welke gegevens worden gebruikt door selfservice voor wachtwoordherstel en welke gegevens moet u voor uw gebruikers invullen?](howto-sspr-authenticationdata.md)
* [Welke verificatiemethoden zijn beschikbaar voor gebruikers?](concept-sspr-howitworks.md#authentication-methods)
* [Wat zijn de beleidsopties bij selfservice voor wachtwoordherstel?](concept-sspr-policy.md)
* [Wat is Wachtwoord terugschrijven en waarom is dit van belang?](howto-sspr-writeback.md)
* [Wat zijn alle opties in selfservice voor wachtwoordherstel en wat houden ze in?](concept-sspr-howitworks.md)
* [Ik denk dat er iets misgaat. Hoe los ik problemen in selfservice voor wachtwoordherstel op?](active-directory-passwords-troubleshoot.md)
* [Ik heb een vraag die nog niet is beantwoord](active-directory-passwords-faq.md)

[Reporting]: ./media/howto-sspr-reporting/sspr-reporting.png "Voorbeeld van SSPR-Activiteitencontrole registreert in Azure AD"
