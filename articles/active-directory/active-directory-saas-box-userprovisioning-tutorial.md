---
title: 'Zelfstudie: Azure Active Directory-integratie met vak | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en vak.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1c959595-6e57-4954-9c0d-67ba03ee212b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 9f061f3f5a0a4825854b893150ceccc8951487de
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-box-for-automatic-user-provisioning"></a>Zelfstudie: Vak configureren voor het automatisch gebruikers inrichten

Het doel van deze zelfstudie is het ziet u de stappen die u wilt uitvoeren in het vak en Azure AD aan automatisch leveren en intrekken gebruikersaccounts vanuit Azure AD aan vak.

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure Active directory-tenant.
*   Een selectievakje eenmalige aanmelding ingeschakeld abonnement.
*   Een gebruikersaccount in het vak met beheerdersmachtigingen Team.

## <a name="assigning-users-to-box"></a>Gebruikers toewijzen aan vak 

Azure Active Directory gebruikt een concept 'toewijzingen' genoemd om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van automatische gebruikers account inrichten, alleen de gebruikers en groepen die '' tot een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureren en inschakelen van de inrichting service, moet u bepalen welke gebruikers en/of groepen in Azure AD vertegenwoordigen de gebruikers die toegang nodig tot de Box-app. Als besloten, kunt u deze gebruikers toewijzen aan uw app vak door de volgende instructies te volgen:

[Een gebruiker of groep toewijzen aan een enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

## <a name="assign-users-and-groups"></a>Gebruikers en groepen toewijzen
De **vak > gebruikers en groepen** tabblad in de Azure portal kunt u opgeven welke gebruikers en groepen toegang moeten worden verleend aan vak. Toewijzing van een gebruiker of groep zorgt ervoor dat de volgende taken uitvoeren:

* Azure AD kan de toegewezen gebruiker (via een rechtstreekse toewijzing toe of groepslidmaatschap) om het selectievakje te verifiëren. Als een gebruiker niet is toegewezen, Azure AD staat niet toe dat ze aan te melden bij vak en retourneert een fout op de aanmeldingspagina van Azure AD.
* Een app-tegel voor Box wordt toegevoegd aan de gebruiker [toepassingsstartprogramma](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).
* Als automatische inrichting is ingeschakeld, worden klikt u vervolgens de toegewezen gebruikers en/of groepen toegevoegd aan de inrichting wachtrij automatisch worden ingericht.
  
  * Als alleen gebruikersobjecten zijn geconfigureerd om te worden ingericht, vervolgens alle rechtstreeks toegewezen gebruikers in de inrichting wachtrij zijn geplaatst en alle gebruikers die lid van een toegewezen groepen zijn in de inrichting wachtrij zijn geplaatst. 
  * Als groepsobjecten zijn geconfigureerd om te worden ingericht, zijn alle toegewezen groepsobjecten ingericht op het selectievakje en alle gebruikers die lid zijn van die groepen. De groeps- en -lidmaatschappen blijven behouden bij het vak wordt geschreven.

U kunt de **kenmerken > Single Sign-On** tabblad configureren welke gebruikerskenmerken (of de claims), worden aangeboden aan vak tijdens de verificatie op basis van SAML en de **kenmerken > inrichten** tab naar configureren hoe de gebruikers- en groepskenmerken wordt overgebracht van Azure AD naar vak tijdens het inrichten van bewerkingen.

### <a name="important-tips-for-assigning-users-to-box"></a>Belangrijke tips voor het toewijzen van gebruikers aan vak 

*   Het is raadzaam om één Azure AD-gebruiker is toegewezen aan het selectievakje voor het testen van de configuratie van de inrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Wanneer een gebruiker toewijzen aan vak, moet u een geldige gebruikersrol selecteren. De rol 'Default toegang' werkt niet voor het inrichten.

## <a name="enable-automated-user-provisioning"></a>Geautomatiseerde Gebruikersinrichting inschakelen

Deze sectie helpt bij het verbinding maken met uw Azure AD gebruikersaccount van het vak API-inrichting en configureren van de inrichting service te maken, bijwerken en uitschakelen van toegewezen gebruikersaccounts in vak op basis van gebruikers en groepen toewijzen in Azure AD.

Als automatische inrichting is ingeschakeld, worden klikt u vervolgens de toegewezen gebruikers en/of groepen toegevoegd aan de inrichting wachtrij automatisch worden ingericht.
    
 * Als er slechts gebruikersobjecten zijn geconfigureerd om te worden ingericht, en vervolgens rechtstreeks toegewezen gebruikers in de inrichting wachtrij zijn geplaatst en alle gebruikers die lid van een toegewezen groepen zijn in de inrichting wachtrij worden geplaatst. 
    
 * Als groepsobjecten zijn geconfigureerd om te worden ingericht, zijn alle toegewezen groepsobjecten ingericht op het selectievakje en alle gebruikers die lid zijn van die groepen. De groeps- en -lidmaatschappen blijven behouden bij het vak wordt geschreven.

> [!TIP] 
> U kunt ook op basis van SAML eenmalige aanmelding voor het selectievakje is ingeschakeld, vindt u de instructies te volgen in [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies aanvulling van elkaar.

### <a name="to-configure-automatic-user-account-provisioning"></a>Voor het configureren van automatische account gebruikersaanvragen:

Het doel van deze sectie is het inschakelen van de inrichting van Active Directory-gebruikersaccounts aan vak overzicht.

1. In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory > zakelijke Apps > alle toepassingen** sectie.

2. Als u het selectievakje al hebt geconfigureerd voor eenmalige aanmelding, zoeken naar uw exemplaar van het kiezen van het zoekveld. Selecteer anders **toevoegen** en zoek naar **vak** in de galerie met toepassingen. Schakel in in de zoekresultaten en toe te voegen aan uw lijst met toepassingen.

3. Selecteer uw exemplaar van het vak en selecteer vervolgens de **inrichten** tabblad.

4. Stel de **Inrichtingsmodus** naar **automatische**. 

    ![Inrichting](./media/active-directory-saas-box-userprovisioning-tutorial/provisioning.png)

5. Onder de **beheerdersreferenties** sectie, klikt u op **autoriseren** een dialoogvenster voor aanmelding openen in een nieuw browservenster.

6. Op de **aanmelden om toegang te verlenen aan vak** pagina, geef de vereiste referenties op en klik vervolgens op **autoriseren**. 
   
    ![Schakel Automatische gebruikersaanvragen](./media/active-directory-saas-box-userprovisioning-tutorial/IC769546.png "automatische gebruikersinrichting inschakelen")

7. Klik op **toegang verlenen aan vak** voor het autoriseren van deze bewerking en terug te keren naar de Azure portal. 
   
    ![Schakel Automatische gebruikersaanvragen](./media/active-directory-saas-box-userprovisioning-tutorial/IC769549.png "automatische gebruikersinrichting inschakelen")

8. Klik in de Azure-portal op **testverbinding** om te controleren of Azure AD, kan verbinding maken met uw app vak. Als de verbinding is mislukt, zorg je account vak Team beheerdersmachtigingen heeft en probeer de **'Autoriseren'** stap opnieuw.

9. Voer het e-mailadres van een persoon of groep die in inrichting fout meldingen moet ontvangen de **e-mailmelding** veld en schakel het selectievakje in.

10. Klik op **opslaan.**

11. Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory: gebruikers aan vak.**

12. In de **kenmerktoewijzingen** sectie, controleert u de kenmerken van de gebruiker die zijn gesynchroniseerd vanuit Azure AD aan vak. De kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen overeenkomen met de gebruikersaccounts in vak voor update-bewerkingen worden gebruikt. Selecteer de knop Opslaan eventuele wijzigingen doorvoeren.

13. Om de Azure AD-service voor Box inricht, wijzigen de **inrichting Status** naar **op** in de sectie instellingen

14. Klik op **opslaan.**

De initiële synchronisatie van gebruikers en/of groepen die zijn toegewezen aan het vak in de sectie gebruikers en groepen worden gestart. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer 20 minuten optreden als de service wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en volg de koppelingen voor het inrichten van de activiteitsrapporten, waarin alle acties die worden uitgevoerd door de inrichting service op de Box-app.

U kunt nu een testaccount maken. Wacht 20 minuten duren om te verifiëren dat het account is gesynchroniseerd naar vak.

In uw tenant vak gesynchroniseerde gebruikers worden vermeld in **beheerde gebruikers** in de **beheerconsole**.

![Integratiestatus](./media/active-directory-saas-box-userprovisioning-tutorial/IC769556.png "integratie-status")


## <a name="additional-resources"></a>Aanvullende bronnen

* [Het beheren van gebruikers account inrichten voor zakelijke Apps](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Eenmalige aanmelding configureren](active-directory-saas-box-tutorial.md)