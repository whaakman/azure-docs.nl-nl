---
title: Gebruikers uit andere directory&quot;s of partnerbedrijven toevoegen in Azure Active Directory | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u gebruikers kunt toevoegen of gebruikersinformatie kunt wijzigen in Azure Active Directory, inclusief externe en gastgebruikers.
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 564a04ec-53c1-470b-9ab9-f3db57da0a89
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/14/2017
ms.author: curtand
ms.translationtype: Human Translation
ms.sourcegitcommit: 17c4dc6a72328b613f31407aff8b6c9eacd70d9a
ms.openlocfilehash: 661470454775d43ce88410a6c995bbcc5e06264c
ms.contentlocale: nl-nl
ms.lasthandoff: 05/16/2017


---
<a id="add-users-from-other-directories-or-partner-companies-in-azure-active-directory" class="xliff"></a>

# Add users from other directories or partner companies in Azure Active Directory (Engelstalig)
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-users-create-external-azure-portal.md )
> * [Klassieke Azure Portal](active-directory-create-users-external.md)
>
>

In dit artikel wordt uitgelegd hoe u gebruikers kunt toevoegen van andere directory's in Azure Active Directory of gebruikers van partnerbedrijven. Zie [Add new users to Azure Active Directory](active-directory-create-users.md) (Engelstalig) voor meer informatie over het toevoegen van nieuwe gebruikers in uw organisatie en het toevoegen van gebruikers die Microsoft-accounts hebben. Toegevoegde gebruikers hebben standaard geen gebruikersrechten, maar u kunt op elk gewenst moment rollen aan ze toewijzen.

<a id="add-a-user" class="xliff"></a>

## Een gebruiker toevoegen
1. Meld u aan bij de [klassieke Azure-portal](https://manage.windowsazure.com) met een account met globale beheerdersrechten voor de directory.
2. Selecteer **Active Directory** en open vervolgens uw directory.
3. Selecteer de tab **Gebruikers** en selecteer vervolgens in de opdrachtbalk **Gebruiker toevoegen**.
4. Selecteer op de pagina **Vertel ons meer over deze gebruiker** onder **Type gebruiker** een van de volgende opties:

   * **Gebruiker in een andere Azure AD-directory**: hiermee voegt u een gebruikersaccount toe aan uw directory dat afkomstig is uit een andere Azure AD-directory. U kunt alleen een gebruiker selecteren in een andere directory als u ook lid van die directory bent.
   * **Gebruikers in partnerbedrijven** - gebruik deze optie om gebruikers van partnerbedrijven uit te nodigen in uw directory (zie [Azure Active Directory B2B collaboration](active-directory-b2b-what-is-azure-ad-b2b.md) (Engelstalig)). Hiervoor moet u moet [een CSV-bestand met e-mailadressen uploaden](active-directory-b2b-references-csv-file-format.md).
5. Geef op de **profielpagina** van de gebruiker een voor- en achternaam op, een gebruiksvriendelijke naam en een gebruikersrol uit de lijst **Rollen**. Zie [Assigning administrator roles in Azure AD](active-directory-assign-admin-roles.md) (Engelstalig) voor meer informatie over gebruikers- en beheerdersrollen. Activeer desgewenst de optie **Meervoudige verificatie inschakelen** voor de gebruiker.
6. Selecteer op de pagina **Tijdelijk wachtwoord instellen** de optie **Maken**.

> [!IMPORTANT]
> Als uw organisatie meer dan één domein gebruikt, is het nuttig op de hoogte te zijn van de volgende problemen die zich kunnen voordoen bij het toevoegen van een gebruikersaccount:
>
> * Als u gebruikersaccounts met dezelfde UPN (user principal name) wilt toevoegen in meerdere domeinen, voegt u bijvoorbeeld **eerst** geoffgrisso@contoso.onmicrosoft.com toe, **gevolgd door** geoffgrisso@contoso.com.
> * Voeg geoffgrisso@contoso.com **niet** vóór geoffgrisso@contoso.onmicrosoft.com toe.
>

Als u informatie wijzigt voor een gebruiker wiens identiteit is gesynchroniseerd met uw on-premises Active Directory-service, kunt u de gebruikersinformatie niet wijzigen in de klassieke Azure-portal. Gebruik uw beheerhulpprogramma's voor on-premises Active Directory om de gebruikersinformatie te wijzigen.

<a id="add-external-users" class="xliff"></a>

## Externe gebruikers toevoegen
U kunt ook gebruikers toevoegen van een andere Azure AD-directory waar u lid van bent, of van partnerbedrijven. Hiervoor uploadt u een CSV-bestand. Als u een externe gebruiker wilt toevoegen, selecteert u voor **Type gebruiker** de optie **Gebruiker in een andere Microsoft Azure AD-directory** of **Gebruikers in partnerbedrijven**.

Gebruikers van beide typen zijn afkomstig van een andere directory en worden toegevoegd als **externe gebruikers**. Externe gebruikers kunnen samenwerken met andere gebruikers in een directory, zonder dat nieuwe accounts of referenties hoeven worden toegevoegd. Externe gebruikers melden zich aan met de gegevens van hun eigen directory, en die verificatie werkt ook voor andere directory's waaraan ze zijn toegevoegd.

<a id="external-user-management-and-limitations" class="xliff"></a>

## Beheer van externe gebruikers en beperkingen
Wanneer u een gebruiker vanuit een andere directory toevoegt aan uw directory, wordt die gebruiker beschouwd als een externe gebruiker in uw directory. De weergavenaam en de gebruikersnaam worden gekopieerd vanuit hun oorspronkelijke directory en gebruikt voor extern gebruik in uw directory. Vanaf dat moment zijn de eigenschappen van het externe gebruikersaccount geheel onafhankelijk. Als de eigenschappen van de gebruiker worden gewijzigd in de oorspronkelijke directory, gelden die wijzigingen niet voor het externe gebruikersaccount in uw directory.

De enige connectie tussen de twee accounts is dat de gebruiker altijd wordt geverifieerd met de oorspronkelijke directory of met een Microsoft-account. Daarom wordt er voor een externe gebruiker geen optie weergegeven voor het herstellen van het wachtwoord of voor het inschakelen van meervoudige verificatie. Op dit moment is het verificatiebeleid van de oorspronkelijke directory of het Microsoft-account het enige dat wordt geëvalueerd wanneer de gebruiker zich aanmeldt.

> [!NOTE]
> U kunt de externe gebruiker nog steeds uitschakelen in de directory. Op deze manier blokkeert u de toegang tot uw directory.
>
>

Als een gebruiker is verwijderd uit de oorspronkelijke directory of als hij of zij het Microsoft-account opzegt, bestaat de externe gebruiker nog steeds in uw directory. De gebruiker in uw directory heeft echter geen toegang meer tot resources, omdat hij of zij zich niet kan aanmelden met een oorspronkelijke directory of een Microsoft-account.

<a id="services-that-currently-support-access-by-azure-ad-external-users" class="xliff"></a>

### Services die momenteel toegang door externe gebruikers van Azure AD ondersteunen
* De **klassieke Azure-portal**: hier kan een externe gebruiker die beheerder is van meerdere directory's die directory's beheren.
* **SharePoint Online**: als extern delen is ingeschakeld, heeft een externe gebruiker toegang tot geautoriseerde resources van SharePoint Online.
* **Dynamics CRM**: externe gebruikers die via PowerShell over een licentie beschikken, hebben toegang tot geautoriseerde resources in Dynamics CRM.
* **Dynamics AX**: externe gebruikers die via PowerShell over een licentie beschikken, hebben toegang tot geautoriseerde resources in Dynamics AX. De beperkingen voor [externe gebruikers van Azure AD](#known-limitations-of-azure-ad-external-users) zijn ook van toepassing op gebruikers in Dynamics AX.

<a id="next-steps" class="xliff"></a>

## Volgende stappen
* [Nieuwe gebruikers toevoegen aan Azure Active Directory](active-directory-create-users.md)
* [Azure AD beheren](active-directory-administer.md)
* [Wachtwoorden beheren in Azure AD](active-directory-manage-passwords.md)
* [Groepen beheren in Azure Active Directory](active-directory-manage-groups.md)

