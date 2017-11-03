---
title: 'Reporting: Azure AD SSPR | Microsoft Docs'
description: Rapportage van Azure AD zelf uw wachtwoord opnieuw instellen gebeurtenissen
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 5b28e15d643497dbdf827b3976ad7dcdc73507b1
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="reporting-options-for-azure-ad-password-management"></a>Rapportageopties voor Azure AD-wachtwoordbeheer

Na de implementatie veel organisaties willen weten hoe of als SSPR echt wordt gebruikt. Azure AD levert rapportagefuncties die u helpen bij het beantwoorden van vragen met behulp van voorgedefinieerde rapporten en u op de juiste wijze een licentie, kunt u aangepaste query's maken.

![Rapportage][Reporting]

Kunnen de volgende vragen worden beantwoord door rapporten die zijn opgenomen in de [Azure portal] (https://portal.azure.com/).

> [!NOTE]
> U moet [een globale beheerder](active-directory-assign-admin-roles.md#assign-or-remove-administrator-roles) en moet aanmelden voor deze gegevens worden verzameld namens uw organisatie door naar de logboeken van de reporting tabblad of audit ten minste één keer te gaan. Pas als u dit doet, wordt geen gegevens verzameld voor uw organisatie

* Hoeveel mensen hebben geregistreerd voor het wachtwoord opnieuw instellen?
* Die is geregistreerd voor het wachtwoord opnieuw instellen?
* Welke gegevens registreert personen?
* Hoeveel mensen dat op het in de afgelopen zeven dagen hun wachtwoord opnieuw instellen?
* Wat zijn de meest voorkomende methoden gebruikers of beheerders gebruiken hun wachtwoorden opnieuw kunnen instellen?
* Wat zijn veelvoorkomende problemen met gebruikers of beheerders face bij een poging tot het gebruik van wachtwoord opnieuw instellen?
* Wat beheerders zijn opnieuw instellen van hun eigen wachtwoorden vaak?
* Is er een verdachte activiteit gebeurt met wachtwoord opnieuw instellen?

## <a name="power-bi-content-pack"></a>Power BI-inhoudspakket

Als u een Power BI-gebruiker bent, is er een inhoudspakket voor Azure AD met eenvoudig te gebruiken reporting voor SSPR. Meer informatie vinden over het gebruik en de implementatie van het inhoudspakket in het artikel [het gebruik van de Azure Active Directory Power BI-inhoud Pack](active-directory-reporting-power-bi-content-pack-how-to.md). U kunt uw eigen dashboards maken en ze delen met anderen in uw organisatie.

## <a name="how-to-view-password-management-reports-in-the-azure-portal"></a>Wachtwoord-rapporten weergeven in de Azure-portal

In de Azure portal ervaring hebben we een betere manier om weer te geven voor wachtwoordherstel en registratie-activiteit wachtwoord opnieuw instellen.  Volg onderstaande stappen voor het vinden van dat het wachtwoord opnieuw instellen en het wachtwoord opnieuw instellen registratie gebeurtenissen:

1. Navigeer naar [ **portal.azure.com**](https://portal.azure.com)
2. Klik op de **meer services** menu op de belangrijkste Azure portal linkermenubalk
3. Zoeken naar **Azure Active Directory** in de lijst met services en dit selecteren
4. Klik op **gebruikers en groepen** uit het navigatiemenu Azure Active Directory
5. Klik op de **controlelogboeken** navigatie-item van het navigatiemenu gebruikers en groepen. Hiermee geeft u alle van de controlegebeurtenissen die plaatsvinden op basis van de gebruikers in uw directory. U kunt deze weergeven voor een overzicht van alle de wachtwoord-gerelateerde gebeurtenissen ook filteren.
6. Als u wilt filteren deze weergave zodanig dat alleen het wachtwoord opnieuw instellen van gerelateerde gebeurtenissen, klikt u op de **Filter** knop aan de bovenkant van de blade.
7. Van de **Filter** selecteert u de **categorie** vervolgkeuzelijst en wijzig dit in de **Self-service wachtwoordbeheer** categorietype.
8. Eventueel verdere filter de lijst door het kiezen van de specifieke **activiteit** u geïnteresseerd bent

## <a name="how-to-retrieve-password-management-events-from-the-azure-ad-reports-and-events-api"></a>Het ophalen van gebeurtenissen voor wachtwoord van de Azure AD-rapporten en gebeurtenissen API

De Azure AD-rapporten en gebeurtenissen API ondersteunt bij het ophalen van de gegevens die zijn opgenomen in het wachtwoord opnieuw instellen en het wachtwoord opnieuw instellen van registratie van rapporten. Met behulp van deze API kunt u afzonderlijke wachtwoordherstel downloaden en gebeurtenissen voor de integratie met de reporting-technologie van uw keuze registratie voor wachtwoord opnieuw instellen.

### <a name="how-to-get-started-with-the-reporting-api"></a>Hoe u aan de slag met de rapportage-API

Voor toegang tot deze gegevens, moet u een kleine app of het script voor het ophalen van onze servers worden geschreven. [Meer informatie over het aan de slag met de Azure AD rapportage-API](active-directory-reporting-api-getting-started.md).

Zodra u een werkende script hebt, wilt u naast de wachtwoord opnieuw instellen en registratie gebeurtenissen onderzoeken die u ophalen kunt om te voldoen aan uw scenario's.

* [SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent): geeft een lijst van de beschikbare kolommen voor wachtwoord opnieuw instellen van gebeurtenissen
* [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent): geeft een lijst van de beschikbare kolommen voor gebeurtenissen van de registratie voor wachtwoord opnieuw instellen

### <a name="reporting-api-data-retrieval-limitations"></a>Rapportage-API-beperkingen voor gegevens ophalen

Op dit moment wordt de Azure AD-rapporten en gebeurtenissen API haalt maximaal **75.000 afzonderlijke gebeurtenissen** van de [SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent) en [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent) typen, spanning de **afgelopen 30 dagen**.

Als u wilt ophalen of gegevens buiten dit venster opslaan, het is raadzaam het persistent maken in een externe database en de API gebruiken om op te vragen van de delta's die het resultaat. Onze aanbeveling is begonnen met het ophalen van deze gegevens wanneer u begint met behulp van SSPR in uw organisatie, extern behouden en vervolgens doorgaan met het bijhouden van de delta's vanaf dit punt.

## <a name="how-to-download-password-reset-registration-events-quickly-with-powershell"></a>Het downloaden van wachtwoord opnieuw instellen van inschrijving gebeurtenissen snel met PowerShell

Naast het rechtstreeks gebruik van de Azure AD-rapporten en gebeurtenissen API, u kunt ook de onderstaande PowerShell-script voor een recente registratie gebeurtenissen in uw directory. Dit is handig als u zien wie onlangs is geregistreerd of wilt ervoor te zorgen wilt dat uw wachtwoord opnieuw instellen van implementatie plaatsvindt zoals verwacht.

* [Azure AD SSPR-registratie activiteit PowerShell-Script](https://gallery.technet.microsoft.com/scriptcenter/azure-ad-self-service-e31b8aee)

### <a name="description-of-report-columns-in-azure-portal"></a>Beschrijving van de kolommen in Azure-portal

De volgende lijst wordt elk van de rapportkolommen in detail uitgelegd:

* **Gebruiker** – de gebruiker die heeft geprobeerd een wachtwoord opnieuw instellen van bewerking voor de registratie.
* **Rol** : de rol van de gebruiker in de directory.
* **Datum en tijd** – de datum en tijd van de poging.
* **Geregistreerde gegevens** : registratie van welke verificatiegegevens die de gebruiker tijdens het wachtwoord opgegeven opnieuw instellen.

### <a name="description-of-report-values-in-azure-portal"></a>Beschrijving van de waarden in Azure-portal

De volgende tabel beschrijft de verschillende waarden voor elke kolom toegestaan:

| Kolom | Toegestane waarden en hun betekenis |
| --- | --- |
| Gegevens die zijn geregistreerd |**Alternatief e-mailadres** – gebruiker alternatieve e-mailadres of verificatie e-mailadres gebruikt om te verifiëren<p><p>**Telefoon (werk)**– telefoon (werk) gebruiker gebruikt om te verifiëren<p>**Mobiele telefoon** -gebruiker gebruikt mobiele telefoon of telefoon voor authenticatie om te verifiëren<p>**Beveiligingsvragen** – gebruiker beveiligingsvragen gebruikt om te verifiëren<p>**Een combinatie van de bovenstaande (bijvoorbeeld alternatieve e-mail + mobiele telefoon)** – treedt op wanneer een beleid 2-gate is opgegeven, ziet u welke twee methoden voor de gebruiker gebruikt voor verificatie aanvraag hun wachtwoord opnieuw instellen. |

## <a name="self-service-password-management-activity-types"></a>Wachtwoordbeheer activiteitstypen selfservice

De volgende activiteitstypen worden weergegeven in de **Self-Service wachtwoordbeheer** audit gebeurteniscategorie.  Een beschrijving voor elk volgt.

* [**Selfservice voor wachtwoordherstel geblokkeerd** ](#activity-type-blocked-from-self-service-password-reset) -geeft aan een gebruiker heeft geprobeerd een wachtwoord opnieuw instellen, een specifieke poort gebruiken of een telefoonnummer valideren meer dan 5 totale aantal keren in 24 uur.
* [**Wachtwoord (self-service) wijzigen** ](#activity-type-change-password-self-service) -geeft aan een gebruiker een vrijwillige uitgevoerd of gedwongen (als gevolg van verlopen) wijzigen van wachtwoorden.
* [**Wachtwoord opnieuw instellen (door admin)** ](#activity-type-reset-password-by-admin) -geeft aan dat een beheerder een namens een gebruiker vanuit de Azure-portal voor wachtwoordherstel uitgevoerd.
* [**Wachtwoord opnieuw instellen (self-service)** ](#activity-type-reset-password-self-service) -geeft aan een gebruiker gereset hun wachtwoord van de [Portal Azure AD-wachtwoord opnieuw instellen](https://passwordreset.microsoftonline.com).
* [**Selfservice voor wachtwoordherstel stroom activiteit uitgevoerd** ](#activity-type-self-serve-password-reset-flow-activity-progress) -geeft aan elke specifieke stap van een gebruiker doorlopen (zoals het doorgeven van een specifiek wachtwoord opnieuw instellen van verificatiepoort) als onderdeel van het wachtwoord opnieuw instellen van proces.
* [**Ontgrendelen van gebruikersaccount (self-service)** ](#activity-type-unlock-user-account-self-service) -geeft aan een gebruiker de Active Directory-account is ontgrendeld zonder de fabrieksinstellingen van het wachtwoord van de [Portal Azure AD-wachtwoord opnieuw instellen](https://passwordreset.microsoftonline.com) met behulp van het AD-account ontgrendelen zonder de functie.
* [**Gebruiker is geregistreerd voor selfservice voor wachtwoordherstel** ](#activity-type-user-registered-for-self-service-password-reset) -geeft aan een gebruiker de vereiste informatie om te kunnen hun wachtwoord in overeenstemming met het beleid voor wachtwoordherstel momenteel opgegeven tenant is geregistreerd.

### <a name="activity-type-blocked-from-self-service-password-reset"></a>Activiteitstype: geen toegang tot selfservice voor wachtwoordherstel

De volgende lijst wordt deze activiteit in detail uitgelegd:

* **Beschrijving van de activiteit** – geeft aan dat een gebruiker heeft geprobeerd te uw wachtwoord opnieuw instellen, een specifieke poort gebruiken of een telefoonnummer valideren meer dan 5 totale aantal keren in 24 uur.
* **Activiteit Actor** -bewerkingen door de gebruiker die is beperkt uit te voeren aanvullende opnieuw instellen. Mogelijk een eindgebruiker of een beheerder.
* **Doel van de activiteit** -bewerkingen door de gebruiker die is beperkt uit te voeren aanvullende opnieuw instellen. Mogelijk een eindgebruiker of een beheerder.
* **Toegestane activiteit statussen**
  * _Geslaagde_ -geeft aan een gebruiker is beperkt uit te voeren eventuele aanvullende opnieuw ingesteld, geen extra verificatiemethoden proberen of eventuele extra telefoonnummers valideren voor de eerstvolgende 24 uur.
* **Reden van de activiteit Status Fout** : niet van toepassing

### <a name="activity-type-change-password-self-service"></a>Activiteitstype: wachtwoord wijzigen (self-service)

De volgende lijst wordt deze activiteit in detail uitgelegd:

* **Beschrijving van de activiteit** – Hiermee geeft u een gebruiker vrijwillig uitgevoerd of gedwongen (als gevolg van verlopen) wijzigen van wachtwoorden.
* **Activiteit Actor** -de gebruiker die hun wachtwoord gewijzigd. Mogelijk een eindgebruiker of een beheerder.
* **Doel van de activiteit** -de gebruiker die hun wachtwoord gewijzigd. Mogelijk een eindgebruiker of een beheerder.
* **Toegestane activiteit statussen**
  * _Geslaagde_ -geeft aan een gebruiker het wachtwoord is gewijzigd
  * _Fout_ -geeft aan een gebruiker is mislukt om hun wachtwoord te wijzigen. Op de rij te klikken, kunt u zien de **activiteit statusreden** categorie voor meer informatie over waarom de fout is opgetreden.
* **Reden van de activiteit Status Fout** - 
  * _FuzzyPolicyViolationInvalidPassword_ -de gebruiker een wachtwoord dat automatisch is verboden als gevolg van Microsoft zijn Geband wachtwoord detectiemogelijkheden vinden om te worden te veel voorkomt of vooral zwakke geselecteerd.

### <a name="activity-type-reset-password-by-admin"></a>Activiteitstype: wachtwoord opnieuw instellen (door de beheerder)

De volgende lijst wordt deze activiteit in detail uitgelegd:

* **Beschrijving van de activiteit** – geeft aan dat een beheerder een namens een gebruiker vanuit de Azure-portal voor wachtwoordherstel uitgevoerd.
* **Activiteit Actor** -beheerder het wachtwoord opnieuw instellen namens een andere gebruiker of beheerder uitgevoerd. Moet een globale beheerder, een wachtwoordbeheerder, een Gebruikersbeheerder of een beheerder van de helpdesk.
* **Doel van de activiteit** -de gebruiker waarvan het wachtwoord is opnieuw ingesteld. Mogelijk een eindgebruiker of een andere beheerder.
* **Toegestane activiteit statussen**
  * _Geslaagde_ -geeft een beheerder is opnieuw ingesteld wachtwoord van een gebruiker
  * _Fout_ -geeft een beheerder kan niet het wachtwoord van een gebruiker wijzigen. Op de rij te klikken, kunt u zien de **activiteit statusreden** categorie voor meer informatie over waarom de fout is opgetreden.

### <a name="activity-type-reset-password-self-service"></a>Activiteitstype: wachtwoord opnieuw instellen (self-service)

De volgende lijst wordt deze activiteit in detail uitgelegd:

* **Beschrijving van de activiteit** – geeft aan dat een gebruiker hun wachtwoord van gereset de [Portal Azure AD-wachtwoord opnieuw instellen](https://passwordreset.microsoftonline.com).
* **Activiteit Actor** -de gebruiker die hun wachtwoord opnieuw instellen. Mogelijk een eindgebruiker of een beheerder.
* **Doel van de activiteit** -de gebruiker die hun wachtwoord opnieuw instellen. Mogelijk een eindgebruiker of een beheerder.
* **Toegestane activiteit statussen**
  * _Geslaagde_ -geeft aan een gebruiker met succes hun eigen wachtwoord opnieuw instellen
  * _Fout_ -aangeeft dat hun eigen wachtwoord opnieuw instellen van een gebruiker is mislukt. Op de rij te klikken, kunt u zien de **activiteit statusreden** categorie voor meer informatie over waarom de fout is opgetreden.
* **Reden van de activiteit Status Fout** -
  * _FuzzyPolicyViolationInvalidPassword_ -admin geselecteerd een wachtwoord dat automatisch als gevolg van Microsoft zijn Geband wachtwoord detectiemogelijkheden vinden om te worden te veel voorkomt of vooral zwakke is verboden.

### <a name="activity-type-self-serve-password-reset-flow-activity-progress"></a>Activiteitstype: eigen wachtwoord opnieuw instellen van stroom activiteit uitgevoerd beheer

De volgende lijst wordt deze activiteit in detail uitgelegd:

* **Beschrijving van de activiteit** – geeft elke specifieke stap van een gebruiker doorlopen (zoals het doorgeven van een specifiek wachtwoord opnieuw instellen van verificatiepoort) als onderdeel van het wachtwoord opnieuw instellen van proces aan.
* **Activiteit Actor** -stroom door de gebruiker die heeft uitgevoerd deel van het wachtwoord opnieuw instellen. Mogelijk een eindgebruiker of een beheerder.
* **Doel van de activiteit** -stroom door de gebruiker die heeft uitgevoerd deel van het wachtwoord opnieuw instellen. Mogelijk een eindgebruiker of een beheerder.
* **Toegestane activiteit statussen**
  * _Geslaagde_ -geeft aan een gebruiker een specifieke stap van de stroom van wachtwoord opnieuw instellen is voltooid.
  * _Fout_ -geeft aan een specifieke stap van het wachtwoord opnieuw instellen van stroom is mislukt. Op de rij te klikken, kunt u zien de **activiteit statusreden** categorie voor meer informatie over waarom de fout is opgetreden.
* **Activiteit statusredenen toegestaan**
  * Zie onderstaande tabel voor [alle toegestaan activiteit opnieuw instellen van statusredenen](#allowed-values-for-details-column)

### <a name="activity-type-unlock-user-account-self-service"></a>Activiteitstype: ontgrendelen gebruikersaccount (self-service)

De volgende lijst wordt deze activiteit in detail uitgelegd:

* **Beschrijving van de activiteit** – geeft aan dat een gebruiker de Active Directory-account is ontgrendeld zonder de fabrieksinstellingen van het wachtwoord van de [Portal Azure AD-wachtwoord opnieuw instellen](https://passwordreset.microsoftonline.com) met behulp van het AD-account ontgrendelen zonder de functie.
* **Activiteit Actor** -de gebruiker die het account ontgrendeld zonder hun wachtwoord opnieuw instellen. Mogelijk een eindgebruiker of een beheerder.
* **Doel van de activiteit** -de gebruiker die het account ontgrendeld zonder hun wachtwoord opnieuw instellen. Mogelijk een eindgebruiker of een beheerder.
* **Toegestane activiteit statussen**
  * _Geslaagde_ -geeft aan een gebruiker een eigen account is ontgrendeld
  * _Fout_ -geeft aan een gebruiker is mislukt om hun account te ontgrendelen. Op de rij te klikken, kunt u zien de **activiteit statusreden** categorie voor meer informatie over waarom de fout is opgetreden.

### <a name="activity-type-user-registered-for-self-service-password-reset"></a>Activiteitstype: gebruiker is geregistreerd voor selfservice voor wachtwoordherstel

De volgende lijst wordt deze activiteit in detail uitgelegd:

* **Beschrijving van de activiteit** – geeft aan een gebruiker de vereiste informatie om te kunnen hun wachtwoord in overeenstemming met het beleid voor wachtwoordherstel momenteel opgegeven tenant is geregistreerd. 
* **Activiteit Actor** -de gebruiker die zich hebben geregistreerd voor wachtwoordherstel. Mogelijk een eindgebruiker of een beheerder.
* **Doel van de activiteit** -de gebruiker die zich hebben geregistreerd voor wachtwoordherstel. Mogelijk een eindgebruiker of een beheerder.
* **Toegestane activiteit statussen**
  * _Geslaagde_ -geeft aan een gebruiker is geregistreerd voor wachtwoordherstel in overeenstemming met het huidige beleid. 
  * _Fout_ -geeft aan een gebruiker kan niet worden geregistreerd voor wachtwoordherstel. Op de rij te klikken, kunt u zien de **activiteit statusreden** categorie voor meer informatie over waarom de fout is opgetreden. Opmerking: dit betekent niet dat een gebruiker zich niet alleen hun eigen wachtwoord opnieuw instellen, dat ze niet het registratieproces is voltooid. Als er niet-geverifieerde gegevens op hun account juist (zoals een telefoonnummer die niet worden gevalideerd), ondanks dat ze dit telefoonnummer niet hebt gecontroleerd, software kan nog steeds worden gebruikt om hun wachtwoord te stellen. Zie voor meer informatie [wat er gebeurt wanneer een gebruiker registreert?](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-learn-more#what-happens-when-a-user-registers)

## <a name="next-steps"></a>Volgende stappen

* [Hoe ik een geslaagde implementatie van SSPR voltooien?](active-directory-passwords-best-practices.md)
* [Opnieuw instellen of wijzigen van uw wachtwoord](active-directory-passwords-update-your-own-password.md).
* [Registreren voor selfservice voor wachtwoordherstel](active-directory-passwords-reset-register.md).
* [Hebt u een vraag licentieverlening?](active-directory-passwords-licensing.md)
* [Welke gegevens wordt gebruikt door de SSPR en welke gegevens moet u voor uw gebruikers vullen?](active-directory-passwords-data.md)
* [Welke verificatiemethoden zijn beschikbaar voor gebruikers?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Wat zijn de beleidsopties met SSPR?](active-directory-passwords-policy.md)
* [Wat is Write-back van wachtwoord en waarom ik van belang?](active-directory-passwords-writeback.md)
* [Wat zijn alle opties in de SSPR en wat ze aan de hand?](active-directory-passwords-how-it-works.md)
* [Ik denk dat er iets is verbroken. Hoe kan ik SSPR oplossen?](active-directory-passwords-troubleshoot.md)
* [Ik heb een vraag waarvoor is geen ergens anders](active-directory-passwords-faq.md)

[Reporting]: ./media/active-directory-passwords-reporting/sspr-reporting.png "Voorbeeld van SSPR activiteit audit wordt geregistreerd in Azure AD"
