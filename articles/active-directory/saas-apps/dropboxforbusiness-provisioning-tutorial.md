---
title: 'Zelfstudie: Dropbox voor bedrijven configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en de inrichting ongedaan maken-gebruikersaccounts met Dropbox voor bedrijven.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: na
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/20/2019
ms.author: zchia
ms.openlocfilehash: c95346ff9026b7fc8c9141234fb3cde9c0809d23
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67053463"
---
# <a name="tutorial-configure-dropbox-for-business-for-automatic-user-provisioning"></a>Zelfstudie: Dropbox voor bedrijven configureren voor het automatisch inrichten van gebruikers

Het doel van deze zelfstudie is ter illustratie van de stappen worden uitgevoerd in Dropbox voor bedrijven en Azure Active Directory (Azure AD) naar Azure AD configureren voor het automatisch inrichten en de inrichting ongedaan maken van gebruikers en/of groepen met Dropbox voor bedrijven.

> [!NOTE]
> Deze zelfstudie beschrijft een connector die is gebaseerd op de Provisioning-Service van Azure AD-gebruiker. Zie voor belangrijke informatie over wat deze service biedt, hoe het werkt en veelgestelde vragen [automatiseren van gebruikersinrichting en -opheffing in SaaS-toepassingen met Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende vereisten hebt:

* Een Azure AD-tenant
* [Een Dropbox voor bedrijven-tenant](https://www.dropbox.com/business/pricing)
* Een gebruikersaccount in Dropbox voor bedrijven met beheerdersmachtigingen.

## <a name="add-dropbox-for-business-from-the-gallery"></a>Dropbox voor bedrijven uit de galerie toevoegen

Voordat u Dropbox voor bedrijven configureert voor automatisch gebruikers inrichten met Azure AD, moet u Dropbox voor bedrijven uit de galerie met Azure AD toevoegen aan uw lijst met beheerde SaaS-toepassingen.

**Als u wilt toevoegen Dropbox voor bedrijven uit de galerie met Azure AD, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)** , selecteer in het navigatievenster aan de linkerkant **Azure Active Directory**.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Ga naar **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop aan de bovenkant van het deelvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Voer in het zoekvak **Dropbox voor bedrijven**, selecteer **Dropbox voor bedrijven** in het deelvenster voor resultaten en klik vervolgens op de **toevoegen** om toe te voegen van de toepassing.

    ![Dropbox voor Bedrijven in de lijst met resultaten](common/search-new-app.png)

## <a name="assigning-users-to-dropbox-for-business"></a>Gebruikers toewijzen aan Dropbox voor bedrijven

Azure Active Directory maakt gebruik van een concept genaamd *toewijzingen* om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers, worden alleen de gebruikers en/of groepen die zijn toegewezen aan een toepassing in Azure AD gesynchroniseerd.

Voordat u configureren en inschakelen van automatische inrichten van gebruikers, moet u bepalen welke gebruikers en/of groepen in Azure AD toegang nodig tot Dropbox voor bedrijven. Wanneer u beslist, kunt u deze gebruikers en/of groepen toewijzen aan Dropbox voor bedrijven door de instructies hier:

* [Een gebruiker of groep toewijzen aan een enterprise-app](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-dropbox-for-business"></a>Belangrijke tips voor het toewijzen van gebruikers aan Dropbox voor bedrijven

* Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan Dropbox voor bedrijven voor het testen van de configuratie van de automatische gebruikersinrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

* Bij het toewijzen van een gebruiker in Dropbox voor bedrijven, moet u een geldige toepassingsspecifieke-rol (indien beschikbaar) in het dialoogvenster toewijzing selecteren. Gebruikers met de **standaardtoegang** rol worden uitgesloten van het inrichten.

## <a name="configuring-automatic-user-provisioning-to-dropbox-for-business"></a>Automatisch gebruikers inrichten naar Dropbox voor bedrijven configureren 

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD-inrichtingsservice als u wilt maken, bijwerken en uitschakelen van gebruikers en/of groepen in Dropbox voor bedrijven op basis van gebruiker en/of groep toewijzingen in Azure AD.

> [!TIP]
> U kunt er ook voor kiezen om in te schakelen op SAML gebaseerde eenmalige aanmelding voor Dropbox voor bedrijven, vindt u de instructies te volgen in de [Dropbox voor één zakelijke aanmelding zelfstudie](dropboxforbusiness-tutorial.md). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatisch gebruikers inrichten, hoewel deze twee functies een fraaie aanvulling in elkaar.

### <a name="to-configure-automatic-user-provisioning-for-dropbox-for-business-in-azure-ad"></a>Het configureren van automatisch gebruikers inrichten voor Dropbox voor bedrijven in Azure AD:

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Selecteer **bedrijfstoepassingen**en selecteer vervolgens **alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Dropbox voor bedrijven**.

    ![De koppeling Dropbox voor Bedrijven in de lijst met toepassingen](common/all-applications.png)

3. Selecteer de **Provisioning** tabblad.

    ![Inrichting](common/provisioning.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Inrichting](common/provisioning-automatic.png)

5. Onder de **beheerdersreferenties** sectie, klikt u op **autoriseren**. Een Dropbox voor bedrijven-aanmeldvenster wordt het geopend in een nieuw browservenster.

    ![Inrichten ](common/provisioning-oauth.png)

6. Op de **aanmelden bij Dropbox voor bedrijven om te koppelen aan Azure AD** dialoogvenster Aanmelden bij uw Dropbox voor bedrijven-tenant en uw identiteit verifiëren.

    ![Dropbox voor bedrijven-aanmelding](media/dropboxforbusiness-provisioning-tutorial/dropbox01.png)

7. Bij de voltooiing van stap 5 en 6, klikt u op **testverbinding** om te controleren of Azure AD, kan verbinding maken met Dropbox voor bedrijven. Als de verbinding is mislukt, zorg ervoor dat uw Dropbox voor bedrijven-account beheerdersmachtigingen heeft en probeer het opnieuw.

    ![Token](common/provisioning-testconnection-oauth.png)

8. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet de inrichting fout ontvangen en schakel het selectievakje in - **een e-mailmelding verzenden wanneer een foutoptreedt**.

    ![E-mailmelding](common/provisioning-notification-email.png)

9. Klik op **Opslaan**.

10. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory: gebruikers aan Dropbox**.

    ![Dropbox gebruikerstoewijzingen](media/dropboxforbusiness-provisioning-tutorial/dropbox-user-mapping.png)

11. Controleer de kenmerken van de gebruiker die van Azure AD worden gesynchroniseerd in Dropbox in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in Dropbox voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

    ![Kenmerken van gebruiker in dropbox](media/dropboxforbusiness-provisioning-tutorial/dropbox-user-attributes.png)

12. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory-groepen naar Dropbox**.

    ![Dropbox-groepstoewijzingen](media/dropboxforbusiness-provisioning-tutorial/dropbox-group-mapping.png)

13. Bekijk de groepskenmerken die worden gesynchroniseerd vanuit Azure AD naar Dropbox in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de groepen in Dropbox voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

    ![Groepskenmerken van dropbox](media/dropboxforbusiness-provisioning-tutorial/dropbox-group-attributes.png)

14. Als u wilt configureren bereikfilters, raadpleegt u de volgende instructies in de [Scoping filter zelfstudie](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

15. Als wilt inschakelen in de Azure AD-inrichtingsservice voor Dropbox, wijzigt de **Inrichtingsstatus** naar **op** in de **instellingen** sectie.

    ![Inrichtingsstatus ingeschakeld](common/provisioning-toggle-on.png)

16. De gebruikers en/of groepen die u wilt inrichten in Dropbox definiëren door het kiezen van de gewenste waarden in **bereik** in de **instellingen** sectie.

    ![Inrichting van bereik](common/provisioning-scope.png)

17. Wanneer u klaar om in te richten bent, klikt u op **opslaan**.

    ![Bezig met opslaan van de Inrichtingsconfiguratie](common/provisioning-configuration-save.png)

Met deze bewerking wordt gestart voor de initiële synchronisatie van alle gebruikers en/of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de Azure AD-inrichtingsservice wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van rapport van de activiteit, die alle acties die worden uitgevoerd door de Azure AD-inrichtingsservice in Dropbox wordt beschreven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Connector-beperkingen
 
* Dropbox biedt geen ondersteuning voor het onderbreken van de uitgenodigde gebruikers. Als een van de uitgenodigde gebruiker is onderbroken, wordt die gebruiker worden verwijderd.

## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../manage-apps/check-status-user-account-provisioning.md)

