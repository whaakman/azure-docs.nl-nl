---
title: Nieuwe gebruikers toevoegen aan Azure Active Directory | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u nieuwe gebruikers kunt toevoegen of gebruikersinformatie kunt wijzigen in Azure Active Directory.
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: e3673727-6bec-4fdc-87a4-d65b213c4c3c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/22/2016
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: f3787f72dbd8ee865899b71538816d2e8d30af32


---
# <a name="add-new-users--or-users-with-microsoft-accounts-to-azure-active-directory"></a>Nieuwe gebruikers of gebruikers met een Microsoft-account toevoegen aan Azure Active Directory
Gebruikers toevoegen om uw directory te vullen. In dit artikel wordt uitgelegd hoe u nieuwe gebruikers kunt toevoegen in uw organisatie en hoe u gebruikers met een Microsoft-account kunt toevoegen. Zie [Add users from other directories or partner companies in Azure Active Directory](active-directory-create-users-external.md) (Engelstalig) voor meer informatie over het toevoegen van gebruikers van andere directory's in Azure Active Directory of over het toevoegen van gebruikers van partnerbedrijven. Toegevoegde gebruikers hebben standaard geen gebruikersrechten, maar u kunt op elk gewenst moment rollen aan ze toewijzen.

## <a name="add-a-user"></a>Een gebruiker toevoegen
1. Meld u aan bij de [klassieke Azure-portal](https://manage.windowsazure.com) met een account met globale beheerdersrechten voor de directory.
2. Selecteer **Active Directory** en selecteer vervolgens de naam van de organisatiedirectory.
3. Selecteer de tab **Gebruikers** en selecteer vervolgens in de opdrachtbalk **Gebruiker toevoegen**.
4. Selecteer op de pagina **Vertel ons meer over deze gebruiker** onder **Type gebruiker** een van de volgende opties:
   
   * **Nieuwe gebruiker in uw organisatie**: hiermee voegt u een nieuw gebruikersaccount toe aan uw directory.
   * **Gebruiker met een bestaand Microsoft-account**: hiermee voegt u een bestaand Microsoft-consumentenaccount toe aan uw directory (bijvoorbeeld een Outlook-account)
5. Afhankelijk van de waarde van **Type gebruiker** voert u een gebruikersnaam in (voor de nieuwe gebruiker) of een e-mailadres (voor een gebruiker met een Microsoft-account).
6. Geef op de **profielpagina** van de gebruiker een voor- en achternaam op, een gebruiksvriendelijke naam en een gebruikersrol uit de lijst **Rollen**. Zie [Assigning administrator roles in Azure AD](active-directory-assign-admin-roles.md) (Engelstalig) voor meer informatie over gebruikers- en beheerdersrollen. Activeer desgewenst de optie **Meervoudige verificatie inschakelen** voor de gebruiker.
7. Selecteer op de pagina **Tijdelijk wachtwoord instellen** de optie **Maken**.

> [!IMPORTANT]
> Als uw organisatie meer dan één domein gebruikt, is het nuttig op de hoogte te zijn van de volgende problemen die zich kunnen voordoen bij het toevoegen van een gebruikersaccount:
> 
> * Als u gebruikersaccounts met dezelfde UPN (user principal name) wilt toevoegen in meerdere domeinen, voegt u bijvoorbeeld **eerst** geoffgrisso@contoso.onmicrosoft.com, toe, **gevolgd door** geoffgrisso@contoso.com.
> * Voeg **niet** eerst geoffgrisso@contoso.com en dan geoffgrisso@contoso.onmicrosoft.com. toe. De volgorde is belangrijk en het kan lastig zijn verkeerd ingevoerde adressen ongedaan te maken.
> 
> 

## <a name="change-user-information"></a>Gebruikersgegevens wijzigen
U kunt elk gebruikerskenmerk wijzigen, behalve de object-ID.

1. Open uw directory.
2. Selecteer de tab **Gebruikers** en selecteer vervolgens de weergavenaam van de gebruiker die u wilt wijzigen.
3. Voer de wijzigingen uit en klik vervolgens op **Opslaan**.

Als de gebruiker die u wijzigt, is gesynchroniseerd met uw on-premises Active Directory-service, kunt u de gebruikersinformatie niet wijzigen met deze procedure. Gebruik uw beheerhulpprogramma's voor on-premises Active Directory om de gebruiker te wijzigen.

## <a name="guest-user-management-and-limitations"></a>Beheer van gastgebruikers en beperkingen
Gastaccounts zijn accounts van gebruikers uit andere directory's. Ze zijn uitgenodigd voor uw directory zodat ze toegang hebben tot SharePoint-documenten, toepassingen of andere Azure-resources. Bij een gastaccount in een directory is het onderliggende UserType-kenmerk ingesteld op "Gast." Gewone gebruikers (met name de leden van uw directory) hebben het UserType-kenmerk "Lid."

Gasten hebben een beperkte set rechten in de directory. Deze rechten beperken de mogelijkheden van gasten om informatie over andere gebruikers in de directory te bekijken. Gastgebruikers kunnen echter wel communiceren met de gebruikers en groepen van de resources waarmee ze werken. Gastgebruikers hebben de volgende mogelijkheden:

* Andere gebruikers en groepen zien die gekoppeld zijn aan een Azure-abonnement waaraan ze zijn toegewezen
* De leden zien van groepen waar ze bij horen
* Andere gebruikers opzoeken in de directory, als ze het volledige e-mailadres van de gebruiker kennen
* Een beperkte set kenmerken bekijken van de gebruikers die ze opzoeken. Het gaat om de weergavenaam, het e-mailadres, de UPN (user principal name) en een foto van miniatuurformaat
* Een lijst samenstellen met geverifieerde domeinen in de directory
* Instemmen met toepassingen zodat ze dezelfde toegang hebben als leden in uw directory

## <a name="set-guest-user-access-policies"></a>Toegangsbeleid instellen voor gasten
De tab **Configureren** van een directory bevat opties voor het beheren van toegang van gastgebruikers. Deze opties kunnen alleen worden gewijzigd in de klassieke Azure-portal door een hoofdbeheerder van de directory. Op dit moment is er geen PowerShell- of API-methode beschikbaar.

Als u het tabblad **Configureren** wilt openen in de klassieke Azure-portal, selecteert u **Active Directory** en vervolgens de naam van de directory.

![Het tabblad Configureren in Azure Active Directory][1]

Vervolgens kunt u de opties voor het beheren van toegang van gastgebruikers bewerken.

![Opties voor het beheren van toegang voor gastgebruikers][2]

## <a name="whats-next"></a>Volgend onderwerp
* [Gebruikers vanuit andere mappen of partnerbedrijven toevoegen in Azure Active Directory](active-directory-create-users-external.md)
* [Administering Azure AD](active-directory-administer.md) (Azure AD beheren)
* [Wachtwoorden beheren in Azure AD](active-directory-manage-passwords.md)
* [Groepen beheren in Azure Active Directory](active-directory-manage-groups.md)

<!--Image references-->
[1]: ./media/active-directory-create-users/RBACDirConfigTab.png
[2]: ./media/active-directory-create-users/RBACGuestAccessControls.png



<!--HONumber=Dec16_HO1-->


