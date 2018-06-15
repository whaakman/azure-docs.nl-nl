---
title: B2B-samenwerking gebruikers toevoegen in de Azure portal - Azure Active Directory | Microsoft Docs
description: Toont hoe een beheerder gastgebruikers kunt toevoegen aan de directory vanaf een andere organisatie met behulp van Azure Active Directory (Azure AD) B2B-samenwerking.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/11/2018
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 9d0565468d953c83ca5fee864b3079fbfee9bbf1
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2018
ms.locfileid: "34076762"
---
# <a name="add-azure-active-directory-b2b-collaboration-users-in-the-azure-portal"></a>Azure Active Directory B2B-samenwerking gebruikers toevoegen in de Azure portal

Als een globale beheerder of een gebruiker die is toegewezen voor de functies van de directory beperkt beheerder, kunt u de Azure-portal om uit te nodigen B2B-samenwerking gebruikers. U kunt uitnodigen gastgebruikers naar de map, een groep of een toepassing. Nadat u een gebruiker via een van deze methoden uitnodigen, de uitgenodigde gebruikersaccount is toegevoegd aan Azure Active Directory (Azure AD), met een gebruikerstype *Gast*. Deze gebruiker moet vervolgens de uitnodiging voor toegang tot bronnen inwisselen.

Nadat u een gastgebruiker naar de map toevoegt, u kunt ofwel verzenden de gastgebruiker een directe koppeling naar een gedeelde app of de gastgebruiker kunt klikken op de URL van de inwisselcode in de e-mail uitnodiging. Zie voor meer informatie over het proces inwisseling [B2B-samenwerking uitnodiging inwisseling](active-directory-b2b-redemption-experience.md).

> [!IMPORTANT]
> U moet de stappen in [instructies: het toevoegen van privacy-informatie van uw organisatie in Azure Active Directory](https://aka.ms/adprivacystatement) om toe te voegen van de URL van privacyverklaring van uw organisatie. Als onderdeel van de eerste keer uitnodiging inwisseling proces, moet een uitgenodigde gebruikers toestemming geven met de voorwaarden van uw privacy om door te gaan. 

## <a name="add-guest-users-to-the-directory"></a>Gastgebruikers toevoegen aan de directory

Als u wilt B2B-samenwerking gebruikers toevoegen aan de map, de volgende stappen uit:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) als beheerder van de Azure AD.
2. Selecteer in het navigatiedeelvenster **Azure Active Directory**.
3. Onder **beheren**, selecteer **gebruikers en groepen** > **alle gebruikers**.
4. Selecteer **nieuwe gastgebruiker**.

   ![Geeft aan waar nieuwe gastgebruiker in de gebruikersinterface](./media/active-directory-b2b-admin-add-users/NewGuestUser-Directory.png) 
 
7. Onder **een gast uitnodigen**, voer het e-mailadres van de externe gebruiker. U kunt desgewenst een welkomstbericht bevatten. Bijvoorbeeld:

   ![Geeft aan waar nieuwe gastgebruiker in de gebruikersinterface](./media/active-directory-b2b-admin-add-users/InviteGuest.png) 

8. Selecteer **uitnodigen** automatisch de uitnodiging te verzenden aan de gastgebruiker. In de **melding** gebied, zoekt u een **is uitgenodigde gebruiker** bericht. 
 
Nadat u de uitnodiging verzonden, wordt het gebruikersaccount automatisch toegevoegd aan de map als gast.


![B2B-gebruiker met gebruikerstype Gast toont](./media/active-directory-b2b-admin-add-users/GuestUserType.png)  

## <a name="add-guest-users-to-a-group"></a>Gastgebruikers toevoegen aan een groep
Als u B2B-samenwerking gebruikers handmatig toevoegen aan een groep als beheerder van de Azure AD wilt, volg deze stappen:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) als beheerder van de Azure AD.
2. Selecteer in het navigatiedeelvenster **Azure Active Directory**.
3. Onder **beheren**, selecteer **gebruikers en groepen** > **alle groepen**.
4. Een groep selecteren (of klik op **nieuwe groep** naar een nieuwe maken). Er is een goed idee om op te nemen in de groepsbeschrijving van de of de groep B2B gastgebruikers bevat.
5. Selecteer **leden** > **leden toevoegen**. 
6. Een van de volgende handelingen uit:
   - Als deze gebruiker al in de directory bestaat, kunt u zoeken naar de B2B-gebruiker. Selecteer de gebruiker > klikt u op **Selecteer** de gebruiker wilt toevoegen aan de groep.
   - Als de gastgebruiker niet al in de map bestaat, selecteert u **uitnodigen**.
   ![Knop voor het toevoegen van leden van de Gast uitnodigen toevoegen](./media/active-directory-b2b-admin-add-users/GroupInvite.png)
   
      Onder **een gast uitnodigen**, voer het e-mailadres en een optionele persoonlijk bericht > Selecteer **uitnodigen**. Klik op **Selecteer** de gebruiker wilt toevoegen aan de groep.

      De uitnodiging gaat automatisch naar de uitgenodigde gebruiker uit. In de **melding** gebied, zoekt u een geslaagde **Invited gebruiker** bericht. 

U kunt ook dynamische groepen gebruiken met Azure AD B2B-samenwerking. Zie voor meer informatie [dynamische groepen en Azure Active Directory B2B-samenwerking](active-directory-b2b-dynamic-groups.md).

## <a name="add-guest-users-to-an-application"></a>Gastgebruikers toevoegen aan een toepassing

Als u wilt B2B-samenwerking gebruikers toevoegen aan een toepassing als beheerder van de Azure AD, de volgende stappen uit:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) als beheerder van de Azure AD.
2. Selecteer in het navigatiedeelvenster **Azure Active Directory**.
3. Onder **beheren**, selecteer **bedrijfstoepassingen** > **alle toepassingen**.
4. Selecteer de toepassing die u wilt toevoegen gastgebruikers.
5. Onder **beheren**, selecteer **gebruikers en groepen**.
6. Selecteer **gebruiker toevoegen**.
7. Onder **toevoegen toewijzing**, selecteer **gebruikers en groepen**.
8. Een van de volgende handelingen uit:
   - Als deze gebruiker al in de directory bestaat, kunt u zoeken naar de B2B-gebruiker. Selecteer de gebruiker en klik vervolgens op **Selecteer** de gebruiker toevoegen aan de app.
   - Als de gastgebruiker niet al in de map bestaat, selecteert u **uitnodigen**.
   ![Knop voor het toevoegen van leden van de Gast uitnodigen toevoegen](./media/active-directory-b2b-admin-add-users/AppInviteUsers.png)
   
      Onder **een gast uitnodigen**, voer het e-mailadres en een optionele persoonlijk bericht > Selecteer **uitnodigen**. Klik op **Selecteer** de gebruiker toevoegen aan de app.

      De uitnodiging gaat automatisch naar de uitgenodigde gebruiker uit. In de **melding** gebied, zoekt u een geslaagde **Invited gebruiker** bericht.

9. Onder **toevoegen toewijzing**, klikt u op **rol selecteren** > Selecteer een rol toe te passen aan de geselecteerde gebruiker (indien van toepassing) > Selecteer **OK**.
10. Klik op **Toewijzen**.
 
## <a name="resend-invitations-to-guest-users"></a>Uitnodigingen voor gastgebruikers verzenden

Als een gastgebruiker nog niet voor de uitnodiging gebruikt is, kunt u de uitnodiging via e-mail opnieuw verzenden.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) als beheerder van de Azure AD.
2. Selecteer in het navigatiedeelvenster **Azure Active Directory**.
3. Onder **beheren**, selecteer **gebruikers en groepen**.
4. Selecteer **alle gebruikers**.
5. Selecteer het gebruikersaccount.
6. Onder **beheren**, selecteer **profiel**.
7. Als de gebruiker nog niet de uitnodiging heeft geaccepteerd een **uitnodiging opnieuw verzenden** optie is beschikbaar. Selecteer deze opnieuw te verzenden.

   ![Optie voor opnieuw verzenden uitnodiging in het gebruikersprofiel](./media/active-directory-b2b-admin-add-users/Resend-Invitation.png)

> [!NOTE]
> Als u een uitnodiging die de gebruiker naar een specifieke app oorspronkelijk verwezen verzenden, moet u begrijpen dat de koppeling in de nieuwe uitnodiging de gebruiker naar het Toegangspaneel op het hoogste niveau in plaats daarvan leidt.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over hoe beheerders van niet-Azure AD B2B-gastgebruikers kunt toevoegen, [hoe IT-medewerkers Voeg B2B-samenwerking gebruikers?](active-directory-b2b-iw-add-users.md)
- Zie voor meer informatie over de uitnodigingsmail [de elementen van de uitnodigingsmail voor B2B-samenwerking](active-directory-b2b-invitation-email.md).

