---
title: Gebruikers van B2B-samenwerking toevoegen in Azure portal - Azure Active Directory | Microsoft Docs
description: Laat zien hoe een beheerder gastgebruikers kunt toevoegen aan de directory van een partnerorganisatie die met behulp van Azure Active Directory (Azure AD) B2B-samenwerking.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 07/10/2018
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 6dfa1f247a079bf801f28d1083c86d36a74117c5
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2018
ms.locfileid: "40004195"
---
# <a name="add-azure-active-directory-b2b-collaboration-users-in-the-azure-portal"></a>Gebruikers van Azure Active Directory B2B-samenwerking toevoegen in Azure portal

Als een globale beheerder of een gebruiker aan wie een van de beperkte beheerder directory-rollen is toegewezen, kunt u de Azure-portal kunt gebruiken om uit te nodigen gebruikers van B2B-samenwerking. U kunt gastgebruikers ook kunnen naar de map, een groep of een toepassing kunt uitnodigen. Nadat u een gebruiker via een van deze methoden uitnodigen, account van de uitgenodigde gebruiker is toegevoegd aan Azure Active Directory (Azure AD), met een gebruikerstype van *Gast*. De gastgebruiker moet vervolgens de uitnodiging voor toegang tot resources inwisselen.

Nadat u een gastgebruiker aan de directory toevoegen, u kunt ofwel de gastgebruiker een directe koppeling sturen naar een gedeelde app of de gastgebruiker kunt klikken op de URL voor inschrijving in de uitnodiging per e-mail. Zie voor meer informatie over het proces inwisselen [B2B-samenwerking uitnodiging inwisselen](redemption-experience.md).

> [!IMPORTANT]
> U moet de stappen in [instructies: het toevoegen van privacy-informatie van uw organisatie in Azure Active Directory](https://aka.ms/adprivacystatement) om toe te voegen van de URL van privacyverklaring van uw organisatie. Als onderdeel van de eerste keer uitnodiging inwisselen proces, moet een van de uitgenodigde gebruiker toestemming geven aan uw privacyvoorwaarden om door te gaan. 

## <a name="add-guest-users-to-the-directory"></a>Gastgebruikers toevoegen aan de directory

Gebruikers van B2B-samenwerking toevoegen aan de directory, de volgende stappen uit:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) als een Azure AD-beheerder.
2. Selecteer in het navigatiedeelvenster **Azure Active Directory**.
3. Onder **beheren**, selecteer **gebruikers**.
4. Selecteer **nieuwe gastgebruiker**.

   ![Laat zien waar nieuwe gastgebruiker zich bevindt in de gebruikersinterface](./media/add-users-administrator/NewGuestUser-Directory.png) 
 
5. Onder **gebruikersnaam**, voer het e-mailadres van de externe gebruiker. (Optioneel) een welkomstbericht bevatten. Bijvoorbeeld:

   ![Laat zien waar nieuwe gastgebruiker zich bevindt in de gebruikersinterface](./media/add-users-administrator/InviteGuest.png) 

    > [!NOTE]
    > Sommige e-mailproviders toestaan dat gebruikers om toe te voegen een plusteken (+)-symbool en aanvullende tekst naar hun e-mailadressen met een zaken zoals het filteren van postvak in. Echter, Azure AD ondersteunt momenteel geen plus-teken in e-mailadressen. Levering om problemen te voorkomen, laat u het plusteken en alle tekens tot na het @-teken.

6. Selecteer **uitnodigen** voor het automatisch verzenden van de uitnodiging voor de gastgebruiker. 
 
Nadat u de uitnodiging verzendt, wordt het gebruikersaccount automatisch toegevoegd aan de map als gast.


![B2B-gebruiker met de Gast gebruikerstype bevat](./media/add-users-administrator/GuestUserType.png)  

## <a name="add-guest-users-to-a-group"></a>Gastgebruikers toevoegen aan een groep
Als u wilt dat gebruikers van B2B-samenwerking handmatig toevoegen aan een groep als een Azure AD-beheerder, volgt u deze stappen:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) als een Azure AD-beheerder.
2. Selecteer in het navigatiedeelvenster **Azure Active Directory**.
3. Onder **beheren**, selecteer **groepen**.
4. Selecteer een groep (of klik op **nieuwe groep** naar een nieuwe maken). Er is een goed idee om op te nemen in de groepsbeschrijving van de die de B2B-gastgebruikers bevat.
5. Selecteer **leden**. 
6. Een van de volgende handelingen uit:
   - Als de gastgebruiker al in de directory bestaat, kunt u zoeken naar de B2B-gebruiker. Selecteer de gebruiker en klik vervolgens op **Selecteer** om toe te voegen van de gebruiker aan de groep.
   - Als de gastgebruiker niet al in de map bestaat, nodigen voor de groep door hun e-mailadres in het zoekvak te typen en vervolgens te klikken op een optioneel persoonlijke bericht typen **Selecteer**. De uitnodiging wordt automatisch verzonden naar de uitgenodigde gebruiker.
     
     ![De knop uitnodigen om toe te voegen gastleden toevoegen](./media/add-users-administrator/GroupInvite.png)
   
U kunt dynamische groepen ook gebruiken met Azure AD B2B-samenwerking. Zie voor meer informatie, [dynamische groepen en Azure Active Directory B2B-samenwerking](use-dynamic-groups.md).

## <a name="add-guest-users-to-an-application"></a>Gastgebruikers toevoegen aan een toepassing

Gebruikers van B2B-samenwerking toevoegen aan een toepassing als een Azure AD-beheerder, de volgende stappen uit:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) als een Azure AD-beheerder.
2. Selecteer in het navigatiedeelvenster **Azure Active Directory**.
3. Onder **beheren**, selecteer **bedrijfstoepassingen** > **alle toepassingen**.
4. Selecteer de toepassing die u wilt toevoegen van gastgebruikers.
5. Selecteer op het dashboard van de toepassing, **totaal aantal gebruikers** openen de **gebruikers en groepen** deelvenster.

    ![Totaal aantal gebruikers om toe te voegen open gebruikers en groepen](./media/add-users-administrator/AppUsersAndGroups.png)

6. Selecteer **gebruiker toevoegen**.
7. Onder **toevoegen toewijzing**, selecteer **gebruikers en groepen**.
8. Een van de volgende handelingen uit:
   - Als de gastgebruiker al in de directory bestaat, kunt u zoeken naar de B2B-gebruiker. Selecteer de gebruiker, klikt u op **Selecteer**, en klik vervolgens op **toewijzen** om toe te voegen van de gebruiker naar de app.
   - Als de gastgebruiker niet al in de map bestaat, selecteert u **uitnodigen**.
           
       ![De knop uitnodigen om toe te voegen gastleden toevoegen](./media/add-users-administrator/AppInviteUsers.png)
   
      Onder **een gast uitnodigen**, voer het e-mailadres, typ desgewenst een persoonlijk bericht en selecteer vervolgens **uitnodigen**. Klik op **Selecteer**, en klik vervolgens op **toewijzen** om toe te voegen van de gebruiker naar de app. Een uitnodiging wordt automatisch verzonden naar de uitgenodigde gebruiker.

9. De gastgebruiker wordt weergegeven in de toepassingsmap **gebruikers en groepen** lijst met de toegewezen rol van **standaardtoegang**. Als u wijzigen van de rol wilt, het volgende doen:
   - Selecteer de gastgebruiker, en selecteer vervolgens **bewerken**. 
   - Onder **toewijzing bewerken**, klikt u op **rol selecteren**, en selecteer de rol die u wilt toewijzen aan de geselecteerde gebruiker.
   - Klik op **Selecteren**.
   - Klik op **Toewijzen**.
 
## <a name="resend-invitations-to-guest-users"></a>Uitnodigingen voor gastgebruikers opnieuw verzenden

Als een gastgebruiker is nog niet ingewisseld voor hun uitnodiging, kunt u de uitnodiging per e-mail opnieuw verzenden.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) als een Azure AD-beheerder.
2. Selecteer in het navigatiedeelvenster **Azure Active Directory**.
3. Onder **beheren**, selecteer **gebruikers**.
5. Selecteer het gebruikersaccount.
6. Onder **beheren**, selecteer **profiel**.
7. Als de gebruiker nog niet de uitnodiging heeft geaccepteerd een **uitnodiging opnieuw verzenden** optie beschikbaar is. Selecteer deze knop om opnieuw te versturen.

   ![Optie in het gebruikersprofiel uitnodiging opnieuw verzenden](./media/add-users-administrator/Resend-Invitation.png)

> [!NOTE]
> Als u een uitnodiging die oorspronkelijk doorgestuurd van de gebruiker voor een specifieke app opnieuw verzenden, moet u begrijpen dat de koppeling in de nieuwe uitnodiging de gebruiker naar het toegangsvenster op het hoogste niveau in plaats daarvan wordt.

## <a name="next-steps"></a>Volgende stappen

- Als u wilt weten hoe beheerders van niet-Azure AD B2B-gastgebruikers kunt toevoegen, Zie [hoe IT-medewerkers Voeg gebruikers van B2B-samenwerking?](add-users-information-worker.md)
- Zie voor meer informatie over de uitnodiging per e-mail [de elementen van de B2B-samenwerking uitnodigingse-mail](invitation-email-elements.md).

