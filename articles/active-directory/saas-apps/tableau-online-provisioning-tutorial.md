---
title: 'Zelfstudie: Tableau Online configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en gebruikersaccounts met Tableau Online inrichting ongedaan maken.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: 0be9c435-f9a1-484d-8059-e578d5797d8e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3e203e88de8d806f489e5a7ab9bfd227c8232f84
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67670898"
---
# <a name="tutorial-configure-tableau-online-for-automatic-user-provisioning"></a>Zelfstudie: Tableau Online configureren voor het automatisch inrichten van gebruikers

In deze zelfstudie ziet u de stappen om uit te voeren in de Online Tableau en Azure Active Directory (Azure AD) voor Azure AD configureren automatisch inrichten en de inrichting van gebruikers en groepen met Tableau Online.

> [!NOTE]
> Deze zelfstudie beschrijft een connector die gebaseerd op de Azure AD-gebruiker-service inricht. Zie voor informatie over de werking van deze service, hoe het werkt en veelgestelde vragen, [automatiseren van gebruikersinrichting en -opheffing in software-as-a-service (SaaS)-toepassingen met Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u hebt:

*   Een Azure AD-tenant.
*   Een [Tableau Online-tenant](https://www.tableau.com/).
*   Een gebruikersaccount in Tableau Online met beheerdersmachtigingen.

> [!NOTE]
> De integratie wordt ingericht op Azure AD is afhankelijk van de [Tableau Online Rest-API](https://onlinehelp.tableau.com/current/api/rest_api/en-us/help.htm). Deze API is beschikbaar voor ontwikkelaars van Tableau Online.

## <a name="add-tableau-online-from-the-azure-marketplace"></a>Tableau Online toevoegen in Azure Marketplace
Voordat u Online Tableau voor automatisch gebruikers inrichten met Azure AD configureren, Tableau Online toevoegen vanuit de Azure Marketplace aan uw lijst met beheerde SaaS-toepassingen.

Volg deze stappen om toe te voegen Tableau Online vanuit de Marketplace.

1. In de [Azure-portal](https://portal.azure.com), selecteert u in het navigatiedeelvenster aan de linkerkant, **Azure Active Directory**.

    ![De Azure Active Directory-pictogram](common/select-azuread.png)

2. Ga naar **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![De blade Enterprise-toepassingen](common/enterprise-applications.png)

3. Als u een nieuwe toepassing wilt toevoegen, selecteert u **Nieuwe toepassing** bovenaan het dialoogvenster.

    ![De knop nieuwe toepassing](common/add-new-app.png)

4. Voer in het zoekvak **Tableau Online** en selecteer **Tableau Online** vanuit het deelvenster resultaten. Als u wilt de toepassing hebt toegevoegd, selecteert u **toevoegen**.

    ![Tableau Online in de lijst met resultaten](common/search-new-app.png)

## <a name="assign-users-to-tableau-online"></a>Gebruikers toewijzen aan Tableau Online

Azure Active Directory maakt gebruik van een concept genaamd *toewijzingen* om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers, worden alleen de gebruikers of groepen die zijn toegewezen aan een toepassing in Azure AD gesynchroniseerd.

Voordat u configureren en automatische inrichting inschakelen, moet u bepalen welke gebruikers of groepen in Azure AD toegang hebben tot Tableau Online moeten. Volg de instructies in deze gebruikers of groepen om aan te wijzen Tableau Online, [een gebruiker of groep toewijzen aan een enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

### <a name="important-tips-for-assigning-users-to-tableau-online"></a>Belangrijke tips voor het toewijzen van gebruikers tot Tableau Online

*   Het is raadzaam dat u één Azure AD-gebruiker met Tableau Online om te testen van de configuratie van de automatische gebruikersinrichting. U kunt extra gebruikers of groepen later toewijzen.

*   Wanneer u een gebruiker aan Tableau Online toewijzen, selecteer een geldige toepassingsspecifieke rol, indien beschikbaar, in het dialoogvenster toewijzing. Gebruikers met de **standaardtoegang** rol worden uitgesloten van het inrichten.

## <a name="configure-automatic-user-provisioning-to-tableau-online"></a>Automatisch gebruikers inrichten met Tableau Online configureren

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD-inrichtingsservice. Gebruik te maken, bijwerken en uitschakelen van gebruikers of groepen in Tableau Online op basis van de gebruiker of groep toewijzingen in Azure AD.

> [!TIP]
> U kunt ook inschakelen SAML gebaseerde eenmalige aanmelding voor Tableau Online. Volg de instructies in de [Tableau eenmalige aanmelding onlinezelfstudie](tableauonline-tutorial.md). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatisch gebruikers inrichten, hoewel deze twee functies elkaar aanvullen.

### <a name="configure-automatic-user-provisioning-for-tableau-online-in-azure-ad"></a>Automatisch gebruikers inrichten voor Online Tableau in Azure AD configureren

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Selecteer **bedrijfstoepassingen** > **alle toepassingen** > **Tableau Online**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Tableau Online**.

    ![De Online Tableau koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer de **Provisioning** tabblad.

    ![Tableau Online inrichten](./media/tableau-online-provisioning-tutorial/ProvisioningTab.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Tableau Online Inrichtingsmodus](./media/tableau-online-provisioning-tutorial/ProvisioningCredentials.png)

5. Onder de **beheerdersreferenties** sectie, voer het domein, gebruikersnaam van beheerder, beheerderswachtwoord en inhouds-URL van uw Tableau Online-account:

   * In de **domein** vak, vult u in het subdomein op basis van stap 6.

   * In de **Admin Username** vak, vult u de gebruikersnaam van het beheerdersaccount dat op uw Clarizen Tenant. Een voorbeeld is admin@contoso.com.

   * In de **beheerderswachtwoord** vak, vult u het wachtwoord van het beheerdersaccount dat overeenkomt met de gebruikersnaam van de beheerder.

   * In de **inhouds-URL** vak, vult u in het subdomein op basis van stap 6.

6. Nadat u zich aanmeldt bij uw account met beheerdersrechten voor Online Tableau, krijgt u de waarden voor **domein** en **inhouds-URL** van de URL van de beheerpagina.

    * De **domein** voor uw Tableau Online account van dit deel van de URL kan worden gekopieerd:

        ![Tableau Online domein](./media/tableau-online-provisioning-tutorial/DomainUrlPart.png)

    * De **inhouds-URL** voor uw Tableau Online account kan worden gekopieerd in deze sectie. Er is een waarde die gedefinieerd tijdens de installatie van het account. In dit voorbeeld is de waarde 'contoso':

        ![Tableau Online inhouds-URL](./media/tableau-online-provisioning-tutorial/ContentUrlPart.png)

        > [!NOTE]
        > Uw **domein** kan afwijken van de hieronder.

7. Nadat u in de vakken weergegeven in stap 5 hebt ingevuld, selecteert u **testverbinding** om ervoor te zorgen dat Azure AD, kan verbinding maken met Tableau Online. Als de verbinding is mislukt, zorg ervoor dat uw Tableau Online-account beheerdersmachtigingen heeft en probeer het opnieuw.

    ![Tableau Online testverbinding](./media/tableau-online-provisioning-tutorial/TestConnection.png)

8. In de **e-mailmelding** vak, voer het e-mailadres van de persoon of groep voor het ontvangen van de inrichting fout-meldingen. Selecteer de **e-mailmelding verzenden wanneer er een fout optreedt** selectievakje.

    ![Tableau Online meldingse-mail](./media/tableau-online-provisioning-tutorial/EmailNotification.png)

9. Selecteer **Opslaan**.

10. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory-gebruikers Tableau**.

    ![Tableau Online Gebruikerssynchronisatie](./media/tableau-online-provisioning-tutorial/UserMappings.png)

11. Controleer de kenmerken van de gebruiker die worden gesynchroniseerd vanuit Azure AD naar Online Tableau in de **kenmerktoewijzingen** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in Tableau Online voor update-bewerkingen. Als u wilt alle wijzigingen hebt opgeslagen, selecteert u **opslaan**.

    ![Tableau Online overeenkomende gebruikerskenmerken](./media/tableau-online-provisioning-tutorial/UserAttributeMapping.png)

12. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory-groepen aan Tableau**.

    ![Tableau Online groepssynchronisatie](./media/tableau-online-provisioning-tutorial/GroupMappings.png)

13. Bekijk de groepskenmerken die worden gesynchroniseerd vanuit Azure AD naar Online Tableau in de **kenmerktoewijzingen** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in Tableau Online voor update-bewerkingen. Als u wilt alle wijzigingen hebt opgeslagen, selecteert u **opslaan**.

    ![Tableau Online overeenkomende groepskenmerken](./media/tableau-online-provisioning-tutorial/GroupAttributeMapping.png)

14. Als u wilt configureren bereikfilters, volg de instructies in de [scoping filter zelfstudie](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

15. Om in te schakelen van de Azure AD-inrichtingsservice voor Online Tableau, in de **instellingen** sectie, wijzigt u **Inrichtingsstatus** naar **op**.

    ![Tableau Online Inrichtingsstatus](./media/tableau-online-provisioning-tutorial/ProvisioningStatus.png)

16. De gebruikers of groepen die u wilt definiëren om in te richten met Tableau Online. In de **instellingen** sectie, selecteert u de waarden in de gewenste **bereik**.

    ![Tableau Online Scope](./media/tableau-online-provisioning-tutorial/ScopeSync.png)

17. Wanneer u klaar om in te richten bent, selecteert u **opslaan**.

    ![Tableau Online Save](./media/tableau-online-provisioning-tutorial/SaveProvisioning.png)

Met deze bewerking wordt gestart voor de initiële synchronisatie van alle gebruikers of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan later wordt gesynchroniseerd. Ze plaatsvindt ongeveer elke 40 minuten, zolang het Azure AD-inrichtingsservice wordt uitgevoerd. 

U kunt de **synchronisatiedetails** sectie aan de vooruitgang en koppelingen naar het rapport over de inrichtingsactiviteit volgen. Het rapport worden alle acties die zijn uitgevoerd door de Azure AD-inrichtingsservice op Tableau Online beschreven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/tableau-online-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/tableau-online-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/tableau-online-provisioning-tutorial/tutorial_general_03.png
