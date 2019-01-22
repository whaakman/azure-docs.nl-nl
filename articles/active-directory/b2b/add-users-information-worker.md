---
title: Gebruikers van B2B-samenwerking toevoegen als een Informatiemedewerker - Azure Active Directory | Microsoft Docs
description: B2B-samenwerking kan IT-medewerkers en eigenaren gastgebruikers toevoegen aan Azure AD voor toegang tot | Microsoft Docs
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: conceptual
ms.date: 12/19/2018
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: mal
ms.openlocfilehash: e15c70ddcc8f4e99c264596f01c83cde90fe0a7e
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54428457"
---
# <a name="how-users-in-your-organization-can-invite-guest-users-to-an-app"></a>Hoe gebruikers in uw organisatie kunnen uitnodigen van gastgebruikers ook kunnen aan een app

Na een Gast is gebruiker toegevoegd aan de directory in Azure AD wordt de eigenaar van een toepassing kan de gastgebruiker een directe koppeling sturen naar de app die zij willen delen. Azure AD-beheerders kunnen ook instellen van self-servicebeheer voor galerie of op basis van SAML-apps in hun Azure AD-tenant. Op deze manier toepassingseigenaren kunnen hun eigen gastgebruikers beheren, zelfs als de gastgebruikers bibliotheekscripts zijn toegevoegd aan de map nog. Wanneer een app is geconfigureerd voor self-service, eigenaar van de toepassing maakt gebruik van het toegangsvenster aan een gastgebruiker aan een app uit te nodigen of een gastgebruiker toevoegen aan een groep die toegang tot de app heeft. Self-service app-beheer voor de galerie en SAML gebaseerde apps, moet u eerste instellingen door een beheerder. Hieronder volgt een samenvatting van de installatiestappen uit (Zie voor meer instructies, [vereisten](#prerequisites) hoger op deze pagina):

 - Selfservice groepsbeheer voor uw tenant inschakelen
 - Maak een groep als u wilt toewijzen aan de app en de gebruiker of eigenaar maken
 - De app voor self-service configureren en de groep toewijzen aan de app.

> [!NOTE]
> Dit artikel wordt beschreven hoe u voor het instellen van self-servicebeheer voor galerie en op basis van SAML-apps die u hebt toegevoegd aan uw Azure AD-tenant. U kunt ook [instellen van Office 365-groepen selfservice](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-self-service-management) , zodat uw gebruikers toegang tot hun eigen Office 365-groepen kunnen beheren. Zie voor meer manieren waarop gebruikers Office-bestanden en apps met gastgebruikers ook kunnen delen kunnen, [toegang voor gasten in Office 365-groepen](https://support.office.com/article/guest-access-in-office-365-groups-bfc7a840-868f-4fd6-a390-f347bf51aff6) en [SharePoint delen bestanden of mappen](https://support.office.com/article/share-sharepoint-files-or-folders-1fe37332-0f9a-4719-970e-d2578da4941c).

## <a name="invite-a-guest-user-to-an-app-from-the-access-panel"></a>Een gastgebruiker uitnodigen voor een app vanuit het toegangsvenster

Nadat een app is geconfigureerd voor self-service, kunnen toepassingseigenaren hun eigen Toegangsvenster gebruiken om uit te nodigen een gastgebruiker de app die zij willen delen. De gastgebruiker hoeft niet per se te worden toegevoegd aan Azure AD vooraf. 

1. Uw Toegangsvenster openen door te gaan naar `https://myapps.microsoft.com`.
2. Verwijzen naar de app, selecteer het beletselteken (**...** ), en selecteer vervolgens **beheren app**.
 
   ![Toegangsdeelvensters beheren app](media/add-users-iw/access-panel-manage-app.png)
 
3. Aan de bovenkant van de lijst met gebruikers, selecteert u **+**.
   
   ![Toegangsvenster een gebruiker toevoegen](media/add-users-iw/access-panel-manage-app-add-user.png)
   
4. In de **leden toevoegen** het zoekvak, typ de e-mailadres voor de gastgebruiker. U kunt optioneel een welkomstbericht toevoegen.
   
   ![Toegang tot de uitnodiging voor deelvenster](media/add-users-iw/access-panel-invitation.png)
   
5. Selecteer **toevoegen** een uitnodiging voor de gastgebruiker te verzenden. Nadat u de uitnodiging verzendt, wordt het gebruikersaccount automatisch toegevoegd aan de map als gast.

## <a name="invite-someone-to-join-a-group-that-has-access-to-the-app"></a>Iemand uitnodigen voor deelname aan een groep die toegang tot de app heeft.
Nadat u een app is geconfigureerd voor self-service, kunnen toepassingseigenaren gastgebruikers ook kunnen aan de groepen die ze beheren die toegang hebben tot de apps die ze willen delen uitnodigen. De gastgebruikers hebben geen al bestaat in de map. Eigenaar van de toepassing Volg deze stappen voor het uitnodigen van een gastgebruiker aan de groep, zodat ze krijgen de app tot toegang.

1. Zorg ervoor dat u bent eigenaar van de selfservice-groep die toegang heeft tot de app die u wilt delen.
2. Uw Toegangsvenster openen door te gaan naar `https://myapps.microsoft.com`.
3. Selecteer de **groepen** app.
   
   ![Toegang tot Configuratiescherm-app van groepen](media/add-users-iw/access-panel-groups.png)
   
4. Onder **mijn groepen**, selecteer de groep die toegang heeft tot de app die u wilt delen.
   
   ![Toegangsvenster groepen die mijn bezit](media/add-users-iw/access-panel-groups-i-own.png)
   
5. Selecteer aan de bovenkant van de lijst met groepsleden **+**.
   
   ![Toegangsvenster groepen toevoegen lid](media/add-users-iw/access-panel-groups-add-member.png)
   
6. In de **leden toevoegen** het zoekvak, typ de e-mailadres voor de gastgebruiker. U kunt optioneel een welkomstbericht toevoegen.
   
   ![Uitnodiging voor toegang tot deelvenster groep](media/add-users-iw/access-panel-invitation.png)
   
7. Selecteer **toevoegen** voor het automatisch verzenden van de uitnodiging voor de gastgebruiker. Nadat u de uitnodiging verzendt, wordt het gebruikersaccount automatisch toegevoegd aan de map als gast.


## <a name="prerequisites"></a>Vereisten

Self-service app-beheer, moet u eerste instellingen door een globale beheerder bent en een Azure AD-beheerder. Als onderdeel van deze instelling kunt u de app voor self-service configureren en een groep toewijzen aan de app die eigenaar van de toepassing kunt beheren. U kunt ook de groep zodat iedereen lidmaatschap aanvragen, maar een Groepseigenaar goedkeuring vereisen. (Meer informatie over [selfservicegroepsbeheer](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-self-service-management).) 

> [!NOTE]
> U kunt gastgebruikers ook kunnen niet toevoegen aan een dynamische groep of voor een groep die is gesynchroniseerd met on-premises Active Directory.

### <a name="enable-self-service-group-management-for-your-tenant"></a>Selfservice groepsbeheer voor uw tenant inschakelen
1. Aanmelden bij de [Azure-portal](https://portal.azure.com) als globale beheerder.
2. Selecteer in het deelvenster navigatie **Azure Active Directory**.
3. Selecteer **groepen**.
4. Onder **instellingen**, selecteer **algemene**.
5. Onder **Self Service groepsbeheer**, naast **eigenaren kunnen aanvragen voor groepslidmaatschap in het toegangsvenster beheren**, selecteer **Ja**.
6. Selecteer **Opslaan**.

### <a name="create-a-group-to-assign-to-the-app-and-make-the-user-an-owner"></a>Maak een groep als u wilt toewijzen aan de app en de gebruiker of eigenaar maken
1. Aanmelden bij de [Azure-portal](https://portal.azure.com) als een Azure AD-beheerder of globale beheerder.
2. Selecteer in het deelvenster navigatie **Azure Active Directory**.
3. Selecteer **groepen**.
4. Selecteer **nieuwe groep**.
5. Onder **groepstype**, selecteer **Security**.
6. Typ een **groepsnaam** en **beschrijving van de groep**.
7. Onder **lidmaatschapstype**, selecteer **toegewezen**.
8. Selecteer **maken**, en sluit de **groep** pagina.
9. Op de **groepen - alle groepen** pagina, open de groep. 
10. Onder **beheren**, selecteer **eigenaren** > **eigenaren toevoegen**. Zoeken naar de gebruiker die toegang tot de toepassing moet beheren. Selecteer de gebruiker en klik vervolgens op **Selecteer**.

### <a name="configure-the-app-for-self-service-and-assign-the-group-to-the-app"></a>De app voor self-service configureren en de groep toewijzen aan de app.
1. Aanmelden bij de [Azure-portal](https://portal.azure.com) als een Azure AD-beheerder of globale beheerder.
2. Selecteer in het navigatiedeelvenster **Azure Active Directory**.
3. Onder **beheren**, selecteer **bedrijfstoepassingen** > **alle toepassingen**.
4. In de lijst met toepassingen zoeken en openen van de app.
5. Onder **beheren**, selecteer **eenmalige aanmelding**, en de toepassing configureren voor eenmalige aanmelding. (Zie voor meer informatie, [over het beheren van eenmalige aanmelding voor zakelijke apps](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-single-sign-on-portal).)
6. Onder **beheren**, selecteer **selfservice**, en toegang tot de app zelf instellen. (Zie voor meer informatie, [over het gebruik van toegang tot de app zelf](https://docs.microsoft.com/azure/active-directory/application-access-panel-self-service-applications-how-to).) 

    > [!NOTE]
    > Voor de instelling **aan welke groep moeten toegewezen gebruikers worden toegevoegd?** selecteert u de groep die u in de vorige sectie hebt gemaakt.
7. Onder **beheren**, selecteer **gebruikers en groepen**, en controleer of dat de selfservice door u gemaakte groep wordt weergegeven in de lijst.
8. Selecteer de app toevoegen aan de Groepseigenaar Toegangsvenster, **gebruiker toevoegen** > **gebruikers en groepen**. Zoeken naar de eigenaar van de groep en selecteer de gebruiker, klikt u op **Selecteer**, en klik vervolgens op **toewijzen** om toe te voegen van de gebruiker naar de app.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen op Azure AD B2B-samenwerking:

- [Wat is Azure AD B2B-samenwerking?](what-is-b2b.md)
- [Hoe voeg beheerders van Azure Active Directory gebruikers van B2B-samenwerking toe?](add-users-administrator.md)
- [B2B-samenwerking uitnodiging inwisselen](redemption-experience.md)
- [Licenties voor Azure AD B2B-samenwerking](licensing-guidance.md)
