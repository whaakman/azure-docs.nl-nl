---
title: 'Zelfstudie: Samanage configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en gebruikersaccounts met Samanage inrichting ongedaan maken.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: 62d0392f-37d4-436e-9aff-22f4e5b83623
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 67cfe5a26740837508ea3a3e76295a896c3cc107
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67670929"
---
# <a name="tutorial-configure-samanage-for-automatic-user-provisioning"></a>Zelfstudie: Samanage configureren voor het automatisch inrichten van gebruikers

In deze zelfstudie ziet u de stappen om uit te voeren in de Samanage en Azure Active Directory (Azure AD) naar Azure AD configureren voor automatisch inrichten en de inrichting van gebruikers en groepen met Samanage ongedaan maken.

> [!NOTE]
> Deze zelfstudie beschrijft een connector die gebaseerd op de Azure AD-gebruiker-service inricht. Zie voor informatie over de werking van deze service, hoe het werkt en veelgestelde vragen, [automatiseren van gebruikersinrichting en -opheffing in software-as-a-service (SaaS)-toepassingen met Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u hebt:

* Een Azure AD-tenant.
* Een [Samanage tenant](https://www.samanage.com/pricing/) met het pakket voor professionals.
* Een gebruikersaccount in Samanage met beheerdersmachtigingen.

> [!NOTE]
> De integratie wordt ingericht op Azure AD is afhankelijk van de [Samanage Rest-API](https://www.samanage.com/api/). Deze API is beschikbaar voor ontwikkelaars voor accounts met het pakket voor professionals Samanage.

## <a name="add-samanage-from-the-azure-marketplace"></a>Samanage toevoegen in Azure Marketplace

Voordat u Samanage voor automatisch gebruikers inrichten met Azure AD configureren, toevoegen aan uw lijst met beheerde SaaS-toepassingen Samanage vanuit Azure Marketplace.

Volg deze stappen om toe te voegen Samanage vanuit de Marketplace.

1. In de [Azure-portal](https://portal.azure.com), selecteert u in het navigatiedeelvenster aan de linkerkant, **Azure Active Directory**.

    ![De Azure Active Directory-pictogram](common/select-azuread.png)

2. Ga naar **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![De blade Enterprise-toepassingen](common/enterprise-applications.png)

3. Als u een nieuwe toepassing wilt toevoegen, selecteert u **Nieuwe toepassing** bovenaan het dialoogvenster.

    ![De knop nieuwe toepassing](common/add-new-app.png)

4. Voer in het zoekvak **Samanage** en selecteer **Samanage** vanuit het deelvenster resultaten. Als u wilt de toepassing hebt toegevoegd, selecteert u **toevoegen**.

    ![Samanage in de resultatenlijst](common/search-new-app.png)

## <a name="assign-users-to-samanage"></a>Gebruikers toewijzen aan Samanage

Azure Active Directory maakt gebruik van een concept genaamd *toewijzingen* om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers, worden alleen de gebruikers of groepen die zijn toegewezen aan een toepassing in Azure AD gesynchroniseerd.

Voordat u configureren en automatische inrichting inschakelen, moet u bepalen welke gebruikers of groepen in Azure AD toegang hebben tot Samanage moeten. Volg de instructies in deze gebruikers of groepen om aan te wijzen Samanage, [een gebruiker of groep toewijzen aan een enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

### <a name="important-tips-for-assigning-users-to-samanage"></a>Belangrijke tips voor het toewijzen van gebruikers aan Samanage

*    Vandaag de dag worden Samanage functies dynamisch en automatisch ingevuld in de gebruikersinterface van Azure portal. Voordat u Samanage rollen aan gebruikers toewijzen, controleert u of er een eerste synchronisatie is voltooid op basis van Samanage naar de nieuwste functies in uw tenant Samanage ophalen.

*    Het is raadzaam dat u één toewijst aan Samanage voor het testen van uw eerste automatisch gebruikers inrichten van configuratie van Azure AD-gebruiker. U kunt extra gebruikers en groepen later nadat de tests geslaagd zijn.

*    Wanneer u een gebruiker aan Samanage toewijzen, selecteer een geldige toepassingsspecifieke rol, indien beschikbaar, in het dialoogvenster toewijzing. Gebruikers met de **standaardtoegang** rol worden uitgesloten van het inrichten.

## <a name="configure-automatic-user-provisioning-to-samanage"></a>Automatisch gebruikers inrichten voor Samanage configureren

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD-inrichtingsservice. Deze gebruiken voor het maken, bijwerken en uitschakelen van gebruikers of groepen in Samanage op basis van de gebruiker of groep toewijzingen in Azure AD.

> [!TIP]
> U kunt ook inschakelen SAML gebaseerde eenmalige aanmelding voor Samanage. Volg de instructies in de [één Samanage aanmeldings-zelfstudie](samanage-tutorial.md). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatisch gebruikers inrichten, hoewel deze twee functies elkaar aanvullen.

### <a name="configure-automatic-user-provisioning-for-samanage-in-azure-ad"></a>Automatisch gebruikers inrichten voor Samanage in Azure AD configureren

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Selecteer **bedrijfstoepassingen** > **alle toepassingen** > **Samanage**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer **Samanage** in de lijst met toepassingen.

    ![De koppeling Samanage in de lijst met toepassingen](common/all-applications.png)

3. Selecteer de **Provisioning** tabblad.

    ![Samanage inrichten](./media/samanage-provisioning-tutorial/ProvisioningTab.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Modus voor Samanage inrichten](./media/samanage-provisioning-tutorial/ProvisioningCredentials.png)

5. Onder de **beheerdersreferenties** sectie, voer de gebruikersnaam van beheerder en het beheerderswachtwoord van uw account Samanage. Voorbeelden van deze waarden zijn:

   * In de **Admin Username** vak, vult u de gebruikersnaam van het beheerdersaccount dat op uw tenant Samanage. Een voorbeeld is admin@contoso.com.

   * In de **beheerderswachtwoord** vak, vult u het wachtwoord van het beheerdersaccount dat overeenkomt met de gebruikersnaam van de beheerder.

6. Nadat u in de vakken weergegeven in stap 5 hebt ingevuld, selecteert u **testverbinding** om ervoor te zorgen dat Azure AD kunt verbinden met Samanage. Als de verbinding is mislukt, zorg ervoor dat uw account Samanage beheerdersmachtigingen heeft en probeer het opnieuw.

    ![Verbinding Samanage testen](./media/samanage-provisioning-tutorial/TestConnection.png)

7. In de **e-mailmelding** vak, voer het e-mailadres van de persoon of groep voor het ontvangen van de inrichting fout-meldingen. Selecteer de **e-mailmelding verzenden wanneer er een fout optreedt** selectievakje.

    ![E-mailmelding Samanage](./media/samanage-provisioning-tutorial/EmailNotification.png)

8. Selecteer **Opslaan**.

9. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory: gebruikers aan Samanage**.

    ![Synchronisatie van de gebruiker Samanage](./media/samanage-provisioning-tutorial/UserMappings.png)

10. Controleer de kenmerken van de gebruiker die van Azure AD worden gesynchroniseerd naar Samanage in de **kenmerktoewijzingen** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in Samanage voor update-bewerkingen. Als u wilt alle wijzigingen hebt opgeslagen, selecteert u **opslaan**.

    ![Overeenkomende gebruikerskenmerken Samanage](./media/samanage-provisioning-tutorial/UserAttributeMapping.png)

11. Om in te schakelen groepstoewijzingen, onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory-groepen aan Samanage**.

    ![Synchronisatie met Samanage](./media/samanage-provisioning-tutorial/GroupMappings.png)

12. Stel **ingeschakeld** naar **Ja** groepen synchroniseren. Bekijk de groepskenmerken die worden gesynchroniseerd vanuit Azure AD naar Samanage in de **kenmerktoewijzingen** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in Samanage voor update-bewerkingen. Als u wilt alle wijzigingen hebt opgeslagen, selecteert u **opslaan**.

    ![Overeenkomende groepskenmerken Samanage](./media/samanage-provisioning-tutorial/GroupAttributeMapping.png)

13. Als u wilt configureren bereikfilters, volg de instructies in de [scoping filter zelfstudie](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

14. Om in te schakelen van de Azure AD-inrichtingsservice voor Samanage, in de **instellingen** sectie, wijzigt u **Inrichtingsstatus** naar **op**.

    ![Samanage Inrichtingsstatus](./media/samanage-provisioning-tutorial/ProvisioningStatus.png)

15. De gebruikers of groepen die u wilt definiëren voor het inrichten van Samanage. In de **instellingen** sectie, selecteert u de waarden in de gewenste **bereik**. Wanneer u selecteert de **alle gebruikers en groepen synchroniseren** optie, houd rekening met de beperkingen, zoals beschreven in de volgende sectie "Connector-beperkingen."

    ![Samanage bereik](./media/samanage-provisioning-tutorial/ScopeSync.png)

16. Wanneer u klaar om in te richten bent, selecteert u **opslaan**.

    ![Samanage opslaan](./media/samanage-provisioning-tutorial/SaveProvisioning.png)


Met deze bewerking wordt gestart voor de initiële synchronisatie van alle gebruikers of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan later wordt gesynchroniseerd. Ze plaatsvindt ongeveer elke 40 minuten, zolang het Azure AD-inrichtingsservice wordt uitgevoerd. 

U kunt de **synchronisatiedetails** sectie aan de vooruitgang en koppelingen naar het rapport over de inrichtingsactiviteit volgen. Het rapport worden alle acties die zijn uitgevoerd door de Azure AD-inrichtingsservice op Samanage beschreven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Connector-beperkingen

Als u selecteert de **alle gebruikers en groepen synchroniseren** optie en configureer een waarde voor de Samanage **rollen** kenmerk, de waarde onder de **standaardwaarde indien null (is optioneel)** vak moet worden uitgedrukt in de volgende indeling:

- {'displayName': 'rol'}, waarbij de rol is de standaardwaarde die u wilt.

## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)


## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/samanage-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/samanage-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/samanage-provisioning-tutorial/tutorial_general_03.png
