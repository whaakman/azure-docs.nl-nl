---
title: 'Zelfstudie: Een Zscaler configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en gebruikersaccounts aan één Zscaler inrichting ongedaan maken.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: 72f6ba2b-73ed-420a-863a-aff672f26fa3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: v-ant-msft
ms.openlocfilehash: 5319b0ac06c4ddf1a7627a4e7fe0bfb2694f79f6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64706599"
---
# <a name="tutorial-configure-zscaler-one-for-automatic-user-provisioning"></a>Zelfstudie: Een Zscaler configureren voor het automatisch inrichten van gebruikers

In deze zelfstudie ziet u de stappen om uit te voeren in één Zscaler en Azure Active Directory (Azure AD) naar Azure AD configureren automatisch inrichten en de inrichting van gebruikers en groepen met Zscaler één.

> [!NOTE]
> Deze zelfstudie beschrijft een connector die gebaseerd op de Azure AD-gebruiker-service inricht. Zie voor informatie over de werking van deze service, hoe het werkt en veelgestelde vragen, [automatiseren van gebruikersinrichting en -opheffing in software-as-a-service (SaaS)-toepassingen met Azure Active Directory](../active-directory-saas-app-provisioning.md).
>
> Deze connector is momenteel beschikbaar als preview. Zie voor meer informatie over de algemene Microsoft Azure gebruiksvoorwaarden voor preview-functies, [aanvullende gebruiksvoorwaarden voor Microsoft Azure previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u hebt:

* Een Azure AD-tenant.
* Een één Zscaler-tenant.
* Een gebruikersaccount in één Zscaler met beheerdersmachtigingen.

> [!NOTE]
> De integratie wordt ingericht op Azure AD is afhankelijk van de Zscaler-API voor één SCIM. Deze API is beschikbaar voor Zscaler One-ontwikkelaars voor accounts met het Enterprise-pakket.

## <a name="add-zscaler-one-from-the-azure-marketplace"></a>Zscaler één van de Azure Marketplace toevoegen

Voordat u een Zscaler voor automatisch gebruikers inrichten met Azure AD configureren, voeg Zscaler één toe vanuit Azure Marketplace aan uw lijst met beheerde SaaS-toepassingen.

Als u wilt toevoegen één Zscaler vanuit de Marketplace, de volgende stappen uit.

1. In de [Azure-portal](https://portal.azure.com), selecteert u in het navigatiedeelvenster aan de linkerkant, **Azure Active Directory**.

    ![De Azure Active Directory-pictogram](common/select-azuread.png)

2. Ga naar **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u een nieuwe toepassing wilt toevoegen, selecteert u **Nieuwe toepassing** bovenaan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Voer in het zoekvak **Zscaler één** en selecteer **Zscaler één** vanuit het deelvenster resultaten. Als u wilt de toepassing hebt toegevoegd, selecteert u **toevoegen**.

    ![Een Zscaler in de lijst met resultaten](common/search-new-app.png)

## <a name="assign-users-to-zscaler-one"></a>Gebruikers toewijzen aan één Zscaler

Azure Active Directory maakt gebruik van een concept genaamd *toewijzingen* om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers, worden alleen de gebruikers of groepen die zijn toegewezen aan een toepassing in Azure AD gesynchroniseerd.

Voordat u configureren en inschakelen van automatisch inrichten van gebruikers, moet u bepalen welke gebruikers of groepen in Azure AD toegang hebben tot één Zscaler moeten. Als u wilt deze gebruikers of groepen toewijzen aan één Zscaler, volg de instructies in [een gebruiker of groep toewijzen aan een enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

### <a name="important-tips-for-assigning-users-to-zscaler-one"></a>Belangrijke tips voor het toewijzen van gebruikers aan één Zscaler

* Het is raadzaam dat u één toewijzen aan één Zscaler voor het testen van de configuratie van de automatische gebruikersinrichting Azure AD-gebruiker. U kunt extra gebruikers of groepen later toewijzen.

* Wanneer u een gebruiker aan één Zscaler toewijzen, selecteer een geldige toepassingsspecifieke rol, indien beschikbaar, in het dialoogvenster toewijzing. Gebruikers met de **standaardtoegang** rol worden uitgesloten van het inrichten.

## <a name="configure-automatic-user-provisioning-to-zscaler-one"></a>Configureert automatisch inrichten van gebruikers aan één Zscaler

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD-inrichtingsservice. Gebruik te maken, bijwerken en gebruikers of groepen in één Zscaler uitschakelen op basis van de gebruiker of groep toewijzingen in Azure AD.

> [!TIP]
> U kunt ook inschakelen SAML gebaseerde eenmalige aanmelding voor één Zscaler. Volg de instructies in de [Zscaler een eenmalige aanmelding zelfstudie](zscaler-One-tutorial.md). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatisch gebruikers inrichten, hoewel deze twee functies elkaar aanvullen.

### <a name="configure-automatic-user-provisioning-for-zscaler-one-in-azure-ad"></a>Automatisch gebruikers inrichten voor één Zscaler in Azure AD configureren

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Selecteer **bedrijfstoepassingen** > **alle toepassingen** > **Zscaler één**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Zscaler één**.

    ![De Zscaler een koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer de **Provisioning** tabblad.

    ![Het inrichten van een Zscaler](./media/zscaler-one-provisioning-tutorial/provisioning-tab.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Een inrichtingsmethode Zscaler](./media/zscaler-one-provisioning-tutorial/provisioning-credentials.png)

5. Onder de **beheerdersreferenties** sectie, vult u de **Tenant-URL** en **geheim Token** vakken met de instellingen voor uw Zscaler één account zoals beschreven in stap 6.

6. Als u de tenant-URL en geheim token, gaat u naar **beheer** > **verificatie-instellingen** in de gebruikersinterface van de Zscaler één portal. Onder **verificatietype**, selecteer **SAML**.

    ![Zscaler een verificatie-instellingen](./media/zscaler-one-provisioning-tutorial/secret-token-1.png)

    a. Selecteer **configureren SAML** openen de **configureren SAML** opties.

    ![Zscaler een SAML configureren](./media/zscaler-one-provisioning-tutorial/secret-token-2.png)

    b. Selecteer **Enable SCIM-Based inrichting** om op te halen van de instellingen **basis-URL** en **Bearer Token**. Sla de instellingen. Kopieer de **basis-URL** instelt op **Tenant-URL** in Azure portal. Kopieer de **Bearer Token** instelt op **geheim Token** in Azure portal.

7. Nadat u in de vakken weergegeven in stap 5 hebt ingevuld, selecteert u **testverbinding** om ervoor te zorgen dat Azure AD verbinding kunt maken met een Zscaler. Als de verbinding is mislukt, zorg ervoor dat uw Zscaler één account beheerdersmachtigingen heeft en probeer het opnieuw.

    ![Een Test-Connection Zscaler](./media/zscaler-one-provisioning-tutorial/test-connection.png)

8. In de **e-mailmelding** vak, voer het e-mailadres van de persoon of groep voor het ontvangen van de inrichting fout-meldingen. Selecteer de **e-mailmelding verzenden wanneer er een fout optreedt** selectievakje.

    ![E-mail met melding voor een Zscaler](./media/zscaler-one-provisioning-tutorial/notification.png)

9. Selecteer **Opslaan**.

10. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory: gebruikers aan één Zscaler**.

    ![Synchronisatie met Zscaler één gebruiker](./media/zscaler-one-provisioning-tutorial/user-mappings.png)

11. Controleer de kenmerken van de gebruiker die van Azure AD worden gesynchroniseerd met Zscaler één in de **kenmerktoewijzingen** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in één Zscaler voor update-bewerkingen. Als u wilt alle wijzigingen hebt opgeslagen, selecteert u **opslaan**.

    ![Zscaler één overeenkomende gebruikerskenmerken](./media/zscaler-one-provisioning-tutorial/user-attribute-mappings.png)

12. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory-groepen aan één Zscaler**.

    ![Synchronisatie met een Zscaler](./media/zscaler-one-provisioning-tutorial/group-mappings.png)

13. Bekijk de groepskenmerken die worden gesynchroniseerd vanuit Azure AD aan één Zscaler in de **kenmerktoewijzingen** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de groepen in één Zscaler voor update-bewerkingen. Als u wilt alle wijzigingen hebt opgeslagen, selecteert u **opslaan**.

    ![Zscaler één overeenkomende groepskenmerken](./media/zscaler-one-provisioning-tutorial/group-attribute-mappings.png)

14. Als u wilt configureren bereikfilters, volg de instructies in de [scoping filter zelfstudie](./../active-directory-saas-scoping-filters.md).

15. Om in te schakelen van de Azure AD-inrichtingsservice voor één Zscaler, in de **instellingen** sectie, wijzigt u **Inrichtingsstatus** naar **op**.

    ![Een Inrichtingsstatus Zscaler](./media/zscaler-one-provisioning-tutorial/provisioning-status.png)

16. De gebruikers of groepen die u wilt definiëren om in te richten voor één Zscaler. In de **instellingen** sectie, selecteert u de waarden in de gewenste **bereik**.

    ![Zscaler één bereik](./media/zscaler-one-provisioning-tutorial/scoping.png)

17. Wanneer u klaar om in te richten bent, selecteert u **opslaan**.

    ![Zscaler opslaan](./media/zscaler-one-provisioning-tutorial/save-provisioning.png)

Met deze bewerking wordt gestart voor de initiële synchronisatie van alle gebruikers of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan later wordt gesynchroniseerd. Ze plaatsvindt ongeveer elke 40 minuten, zolang het Azure AD-inrichtingsservice wordt uitgevoerd. 

U kunt de **synchronisatiedetails** sectie aan de vooruitgang en koppelingen naar het rapport over de inrichtingsactiviteit volgen. Het rapport worden alle acties die zijn uitgevoerd door de Azure AD-inrichtingsservice op één Zscaler beschreven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/zscaler-one-provisioning-tutorial/tutorial-general-01.png
[2]: ./media/zscaler-one-provisioning-tutorial/tutorial-general-02.png
[3]: ./media/zscaler-one-provisioning-tutorial/tutorial-general-03.png
