---
title: Self-service voor wachtwoord opnieuw instellen van rapporten - Azure Active Directory
description: Rapportage over Azure AD Self-service voor wachtwoord opnieuw instellen van gebeurtenissen
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: 2560296c088491b7ae0cd414f88ae337c1383c9a
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39258793"
---
# <a name="reporting-options-for-azure-ad-password-management"></a>Rapportage-opties voor Azure AD-wachtwoordbeheer

Na de implementatie in veel organisaties willen weten hoe of als selfservice voor wachtwoordherstel (SSPR) echt wordt gebruikt. De rapportagefunctie die Azure Active Directory (Azure AD) biedt vindt u antwoorden op vragen met behulp van vooraf gedefinieerde rapporten. Als u op de juiste wijze in licentie gegeven bent, kunt u ook aangepaste query's maken.

![Rapportage][Reporting]

De volgende vragen kunnen worden beantwoord door de rapporten die zijn opgenomen in de [Azure portal] (https://portal.azure.com/):

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

Als u een Power BI-gebruiker bent, moet u er een inhoudspakket voor Azure AD met eenvoudig te gebruiken, rapportage voor SSPR is. Zie voor meer informatie over het gebruik en de implementatie van het inhoudspakket [over het gebruik van de Azure Active Directory Power BI-inhoudspakket](../active-directory-reporting-power-bi-content-pack-how-to.md). Met het inhoudspakket kunt u uw eigen dashboards maken en deze delen met anderen in uw organisatie.

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

* **Gebruiker**: bewerking voor de registratie door de gebruiker die heeft geprobeerd een wachtwoord opnieuw instellen.
* **Rol**: de rol van de gebruiker in de map.
* **Datum en tijd**: de datum en tijd van de poging.
* **Gegevens geregistreerd**: de verificatiegegevens die registratie van de gebruiker is opgegeven tijdens het wachtwoord opnieuw instellen.

## <a name="description-of-the-report-values-in-the-azure-portal"></a>Beschrijving van de waarden van het rapport in de Azure-portal

De volgende tabel beschrijft de verschillende waarden die dat u kunt instellen voor elke kolom in de Azure-portal:

| Kolom | Toegestane waarden en hun betekenis |
| --- | --- |
| Gegevens die zijn geregistreerd |**Alternatief e-mailadres**: de gebruiker een alternatief e-mailadres of verificatieadres gebruikt om te verifiëren.<p><p>**Telefoon (werk)**: de gebruiker een telefoon (werk) gebruikt om te verifiëren.<p>**Mobiele telefoon**: de gebruiker een mobiele telefoon of telefoon voor authenticatie gebruikt om te verifiëren.<p>**Vragen over de beveiliging**: de gebruiker vragen over de beveiliging gebruikt om te verifiëren.<p>**Een combinatie van de vorige methoden, bijvoorbeeld, alternatief e-mail + mobiele telefoon**: treedt op wanneer een beleid voor twee-poort is opgegeven en ziet u welke twee methoden voor de gebruiker gebruikt voor verificatie verzoek hun wachtwoord opnieuw instellen. |

## <a name="self-service-password-management-activity-types"></a>Selfservice wachtwoordbeheer activiteitstypen

De volgende activiteitstypen worden weergegeven in de **Self-Service wachtwoordbeheer** categorie audit-gebeurtenis:

* [Self-service voor wachtwoord opnieuw instellen geblokkeerd](#activity-type-blocked-from-self-service-password-reset): geeft aan dat een gebruiker heeft geprobeerd een wachtwoord opnieuw instellen, gebruikt u een specifieke poort of een telefoonnummer in dat meer dan vijf totale aantal keren dat in 24 uur valideren.
* [Wachtwoord (selfservice) wijzigen](#activity-type-change-password-self-service): geeft aan dat een gebruiker vrijwillig uitgevoerd of (als gevolg van verlopen geforceerde) wachtwoord wijzigen.
* [Wachtwoord opnieuw instellen (door beheerder)](#activity-type-reset-password-by-admin): geeft aan dat een beheerder een namens een gebruiker vanuit de Azure-portal voor wachtwoordherstel uitgevoerd.
* [Wachtwoord opnieuw instellen (selfservice)](#activity-type-reset-password-self-service): geeft aan dat een gebruiker is hun wachtwoord opnieuw instellen van de [Azure AD portal voor wachtwoordherstel](https://passwordreset.microsoftonline.com).
* [Self-service voor wachtwoord opnieuw instellen van stroomactiviteitvoortgang](#activity-type-self-serve-password-reset-flow-activity-progress): geeft aan dat elke specifieke stap van een gebruiker wordt uitgevoerd in, zoals een specifiek wachtwoord te geven verificatiepoort, opnieuw worden ingesteld als onderdeel van het proces voor wachtwoordherstel.
* [Gebruikersaccount ontgrendelen (selfservice)](#activity-type-unlock-user-account-self-service): geeft aan dat een gebruiker het Active Directory-account is ontgrendeld zonder hun wachtwoord opnieuw in te stellen de [Azure AD portal voor wachtwoordherstel](https://passwordreset.microsoftonline.com) met behulp van de actieve Directory-functie van account ontgrendelen zonder opnieuw instellen.
* [De gebruiker geregistreerd voor selfservice voor wachtwoordherstel](#activity-type-user-registered-for-self-service-password-reset): geeft aan dat een gebruiker de vereiste informatie om te kunnen hun wachtwoord in overeenstemming met het hulpprogramma voor het opnieuw instellen van de opgegeven tenant-wachtwoordbeleid is geregistreerd.

### <a name="activity-type-blocked-from-self-service-password-reset"></a>Activiteitstype: self-service voor wachtwoord opnieuw instellen geblokkeerd

De volgende lijst wordt deze activiteit in detail uitgelegd:

* **Beschrijving van de activiteit**: geeft aan dat een gebruiker heeft geprobeerd een wachtwoord opnieuw instellen, gebruikt u een specifieke poort of een telefoonnummer in dat meer dan vijf totale aantal keren dat in 24 uur valideren.
* **Activiteit actor**: de gebruiker die wordt beperkt uit te voeren als u meer bewerkingen opnieuw instellen. De gebruiker kan een eindgebruiker of een beheerder zijn.
* **Doel van de activiteit**: de gebruiker die wordt beperkt uit te voeren als u meer bewerkingen opnieuw instellen. De gebruiker kan een eindgebruiker of een beheerder zijn.
* **Activiteitsstatus**:
  * _Succes_: geeft aan dat een gebruiker van een extra opnieuw instellen van wachtwoorden uitvoeren, probeert een extra verificatiemethoden of eventuele extra telefoonnummers valideren voor de volgende 24 uur wordt beperkt.
* **Activiteit status van de reden van fout**: niet van toepassing.

### <a name="activity-type-change-password-self-service"></a>Activiteitstype: wachtwoord wijzigen (selfservice)

De volgende lijst wordt deze activiteit in detail uitgelegd:

* **Beschrijving van de activiteit**: geeft aan dat een gebruiker vrijwillig uitgevoerd of (als gevolg van verlopen geforceerde) wachtwoord wijzigen.
* **Activiteit actor**: de gebruiker die hun wachtwoord gewijzigd. De gebruiker kan een eindgebruiker of een beheerder zijn.
* **Doel van de activiteit**: de gebruiker die hun wachtwoord gewijzigd. De gebruiker kan een eindgebruiker of een beheerder zijn.
* **Status van de activiteit**:
  * _Succes_: geeft aan dat een gebruiker het wachtwoord is gewijzigd.
  * _Fout_: geeft aan dat een gebruiker is mislukt om hun wachtwoord te wijzigen. U kunt de rij om te zien de **reden van de activiteit status** categorie voor meer informatie over waarom de fout is opgetreden.
* **Activiteit status van de reden van fout**: 
  * _FuzzyPolicyViolationInvalidPassword_: de gebruiker een wachtwoord dat automatisch is geblokkeerd omdat de detectiemogelijkheden Microsoft verboden wachtwoord gevonden worden te veel voorkomt of vooral zwakke worden geselecteerd.

### <a name="activity-type-reset-password-by-admin"></a>Activiteitstype: wachtwoord opnieuw instellen (door beheerder)

De volgende lijst wordt deze activiteit in detail uitgelegd:

* **Beschrijving van de activiteit**: geeft aan dat een beheerder een namens een gebruiker vanuit de Azure-portal voor wachtwoordherstel uitgevoerd.
* **Activiteit actor**: de beheerder van het wachtwoord opnieuw instellen van namens een andere gebruiker of beheerder uitgevoerd. Moet een globale beheerder, wachtwoordbeheerder, Gebruikerbeheerder, of helpdesk-beheerder.
* **Doel van de activiteit**: de gebruiker waarvan het wachtwoord is opnieuw ingesteld. De gebruiker kan een eindgebruiker of een andere beheerder zijn.
* **Status van de activiteit**:
  * _Succes_: geeft aan dat een beheerder is opnieuw ingesteld wachtwoord van een gebruiker.
  * _Fout_: geeft aan dat een beheerder kan niet worden gewijzigd wachtwoord van een gebruiker. U kunt de rij om te zien de **reden van de activiteit status** categorie voor meer informatie over waarom de fout is opgetreden.

### <a name="activity-type-reset-password-self-service"></a>Activiteitstype: wachtwoord opnieuw instellen (selfservice)

De volgende lijst wordt deze activiteit in detail uitgelegd:

* **Beschrijving van de activiteit**: geeft aan dat een gebruiker is hun wachtwoord opnieuw instellen van de [Azure AD portal voor wachtwoordherstel](https://passwordreset.microsoftonline.com).
* **Activiteit actor**: de gebruiker die hun wachtwoord opnieuw instellen. De gebruiker kan een eindgebruiker of een beheerder zijn.
* **Doel van de activiteit**: de gebruiker die hun wachtwoord opnieuw instellen. De gebruiker kan een eindgebruiker of een beheerder zijn.
* **Status van de activiteit**:
  * _Succes_: geeft aan dat een gebruiker is hun eigen wachtwoord opnieuw.
  * _Fout_: geeft aan dat een gebruiker kan niet opnieuw instellen van hun eigen wachtwoord. U kunt de rij om te zien de **reden van de activiteit status** categorie voor meer informatie over waarom de fout is opgetreden.
* **Activiteit status van de reden van fout**: 
  * _FuzzyPolicyViolationInvalidPassword_: de beheerder een wachtwoord dat automatisch is geblokkeerd omdat de detectiemogelijkheden Microsoft verboden wachtwoord gevonden worden te veel voorkomt of vooral zwakke worden geselecteerd.

### <a name="activity-type-self-serve-password-reset-flow-activity-progress"></a>Activiteitstype: eigen wachtwoord opnieuw instellen van stroomactiviteitvoortgang beheer

De volgende lijst wordt deze activiteit in detail uitgelegd:

* **Beschrijving van de activiteit**: geeft aan dat elke specifieke stap van een gebruiker wordt uitgevoerd in (zoals het doorgeven van een specifiek wachtwoord opnieuw instellen van de verificatiepoort) als onderdeel van het proces voor wachtwoordherstel.
* **Activiteit actor**: de gebruiker die heeft uitgevoerd, onderdeel van het wachtwoord opnieuw instellen van flow. De gebruiker kan een eindgebruiker of een beheerder zijn.
* **Doel van de activiteit**: de gebruiker die heeft uitgevoerd, onderdeel van het wachtwoord opnieuw instellen van flow. De gebruiker kan een eindgebruiker of een beheerder zijn.
* **Status van de activiteit**:
  * _Succes_: geeft aan dat een specifieke stap van de stroom van wachtwoord opnieuw instellen die door een gebruiker is voltooid.
  * _Fout_: geeft aan dat een specifieke stap van het wachtwoord opnieuw instellen van stroom is mislukt. U kunt de rij om te zien de **reden van de activiteit status** categorie voor meer informatie over waarom de fout is opgetreden.
* **Activiteit statusredenen**: Zie de volgende tabel voor [alle toegestane reset activiteit statusredenen](#allowed-values-for-details-column).

### <a name="activity-type-unlock-a-user-account-self-service"></a>Activiteitstype: een gebruikersaccount ontgrendelen (selfservice)

De volgende lijst wordt deze activiteit in detail uitgelegd:

* **Beschrijving van de activiteit**: geeft aan dat een gebruiker het Active Directory-account is ontgrendeld zonder hun wachtwoord opnieuw in te stellen de [Azure AD portal voor wachtwoordherstel](https://passwordreset.microsoftonline.com) met behulp van de functie Active Directory van account ontgrendelen zonder opnieuw instellen.
* **Activiteit actor**: de gebruiker die hun account ontgrendelen zonder hun wachtwoord opnieuw instellen. De gebruiker kan een eindgebruiker of een beheerder zijn.
* **Doel van de activiteit**: de gebruiker die hun account ontgrendelen zonder hun wachtwoord opnieuw instellen. De gebruiker kan een eindgebruiker of een beheerder zijn.
* **Activiteit statussen toegestaan**:
  * _Succes_: geeft aan dat een gebruiker is hun eigen account ontgrendeld.
  * _Fout_: geeft aan dat een gebruiker is mislukt om hun account te ontgrendelen. U kunt de rij om te zien de **reden van de activiteit status** categorie voor meer informatie over waarom de fout is opgetreden.

### <a name="activity-type-user-registered-for-self-service-password-reset"></a>Activiteitstype: gebruiker geregistreerd voor selfservice voor wachtwoord opnieuw instellen

De volgende lijst wordt deze activiteit in detail uitgelegd:

* **Beschrijving van de activiteit**: geeft aan dat een gebruiker de vereiste informatie om te kunnen hun wachtwoord in overeenstemming met het hulpprogramma voor het opnieuw instellen van de opgegeven tenant-wachtwoordbeleid is geregistreerd. 
* **Activiteit actor**: de gebruiker die zich hebben geregistreerd voor wachtwoord opnieuw instellen. De gebruiker kan een eindgebruiker of een beheerder zijn.
* **Doel van de activiteit**: de gebruiker die zich hebben geregistreerd voor wachtwoord opnieuw instellen. De gebruiker kan een eindgebruiker of een beheerder zijn.
* **Activiteit statussen toegestaan**:
  * _Succes_: geeft aan dat een gebruiker is geregistreerd voor wachtwoord opnieuw instellen in overeenstemming met het huidige beleid. 
  * _Fout_: geeft aan dat een gebruiker kan niet registreren voor wachtwoord opnieuw instellen. U kunt de rij om te zien de **reden van de activiteit status** categorie voor meer informatie over waarom de fout is opgetreden. 

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
