---
title: 'Zelfstudie: Zendesk configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en inrichting ongedaan maken met Zendesk-gebruikersaccounts.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd-msft
ms.assetid: 01d5e4d5-d856-42c4-a504-96fa554baf66
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: v-ant
ms.collection: M365-identity-device-management
ms.openlocfilehash: f559d2c2398998ba590419758de559f21d9b65f5
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "62114662"
---
# <a name="tutorial-configure-zendesk-for-automatic-user-provisioning"></a>Zelfstudie: Zendesk voor het automatisch inrichten van gebruikers configureren

In deze zelfstudie ziet u de stappen om uit te voeren in de Zendesk en Azure Active Directory (Azure AD) naar Azure AD configureren voor het automatisch inrichten en de inrichting van gebruikers en groepen met Zendesk.

> [!NOTE]
> Deze zelfstudie beschrijft een connector die gebaseerd op de Azure AD-gebruiker-service inricht. Zie voor informatie over de werking van deze service, hoe het werkt en veelgestelde vragen, [automatiseren van gebruikersinrichting en -opheffing in software-as-a-service (SaaS)-toepassingen met Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u hebt:

* Een Azure AD-tenant.
* Een Zendesk-tenant met de [Enterprise](https://www.zendesk.com/product/pricing/) plannen of beter ingeschakeld.
* Een gebruikersaccount in Zendesk met beheerdersmachtigingen.

> [!NOTE]
> De integratie wordt ingericht op Azure AD is afhankelijk van de [Zendesk Rest-API](https://developer.zendesk.com/rest_api/docs/core/introduction). Deze API is beschikbaar voor Zendesk teams op het Enterprise-plan of hoger.

## <a name="add-zendesk-from-the-azure-marketplace"></a>Zendesk toevoegen in Azure Marketplace

Voordat u Zendesk voor automatisch gebruikers inrichten met Azure AD configureren, toevoegen aan uw lijst met beheerde SaaS-toepassingen Zendesk vanuit Azure Marketplace.

Volg deze stappen om toe te voegen Zendesk vanuit de Marketplace.

1. In de [Azure-portal](https://portal.azure.com), selecteert u in het navigatiedeelvenster aan de linkerkant, **Azure Active Directory**.

    ![De Azure Active Directory-pictogram](common/select-azuread.png)

2. Ga naar **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u een nieuwe toepassing wilt toevoegen, selecteert u **Nieuwe toepassing** bovenaan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Voer in het zoekvak **Zendesk** en selecteer **Zendesk** vanuit het deelvenster resultaten. Als u wilt de toepassing hebt toegevoegd, selecteert u **toevoegen**.

    ![Zendesk in de resultatenlijst](common/search-new-app.png)

## <a name="assign-users-to-zendesk"></a>Gebruikers toewijzen aan Zendesk

Azure Active Directory maakt gebruik van een concept genaamd *toewijzingen* om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers, worden alleen de gebruikers of groepen die zijn toegewezen aan een toepassing in Azure AD gesynchroniseerd.

Voordat u configureren en automatische inrichting inschakelen, moet u bepalen welke gebruikers of groepen in Azure AD toegang hebben tot Zendesk moeten. Volg de instructies in deze gebruikers of groepen om aan te wijzen Zendesk, [een gebruiker of groep toewijzen aan een enterprise-app](../manage-apps/assign-user-or-group-access-portal.md).

### <a name="important-tips-for-assigning-users-to-zendesk"></a>Belangrijke tips voor het toewijzen van gebruikers met Zendesk

* Vandaag de dag worden Zendesk-functies dynamisch en automatisch ingevuld in de gebruikersinterface van Azure portal. Voordat u een Zendesk-rollen aan gebruikers toewijzen, controleert u of een eerste synchronisatie is voltooid op basis van Zendesk om op te halen van de meest recente rollen in uw Zendesk-tenant.

* Het is raadzaam dat u één Azure AD-gebruiker met Zendesk voor het testen van uw eerste automatisch gebruikers inrichten van configuratie. U kunt extra gebruikers of groepen later nadat de tests geslaagd zijn.

* Wanneer u een gebruiker aan Zendesk toewijzen, selecteer een geldige toepassingsspecifieke rol, indien beschikbaar, in het dialoogvenster toewijzing. Gebruikers met de **standaardtoegang** rol worden uitgesloten van het inrichten.

## <a name="configure-automatic-user-provisioning-to-zendesk"></a>Automatisch gebruikers inrichten met Zendesk configureren 

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD-inrichtingsservice. Deze gebruiken voor het maken, bijwerken en uitschakelen van gebruikers of groepen in Zendesk op basis van de gebruiker of groep toewijzingen in Azure AD.

> [!TIP]
> U kunt ook inschakelen SAML gebaseerde eenmalige aanmelding voor Zendesk. Volg de instructies in de [één Zendesk aanmeldings-zelfstudie](zendesk-tutorial.md). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatisch gebruikers inrichten, hoewel deze twee functies elkaar aanvullen.

### <a name="configure-automatic-user-provisioning-for-zendesk-in-azure-ad"></a>Automatisch gebruikers inrichten voor Zendesk in Azure AD configureren

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Selecteer **bedrijfstoepassingen** > **alle toepassingen** > **Zendesk**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Zendesk**.

    ![De Zendesk-koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer de **Provisioning** tabblad.

    ![Zendesk Provisioning](./media/zendesk-provisioning-tutorial/ZenDesk16.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Inrichten van Zendesk-modus](./media/zendesk-provisioning-tutorial/ZenDesk1.png)

5. Onder de **beheerdersreferenties** sectie, voer de gebruikersnaam van beheerder, de token voor geheim en het domein van uw Zendesk-account. Voorbeelden van deze waarden zijn:

   * In de **Admin Username** vak, vult u de gebruikersnaam van het beheerdersaccount dat op uw Zendesk-tenant. Een voorbeeld is admin@contoso.com.

   * In de **geheim Token** vak, vult in het token voor geheim, zoals beschreven in stap 6.

   * In de **domein** vak, vult u in het subdomein van uw Zendesk-tenant. Bijvoorbeeld, voor een account met een tenant-URL van `https://my-tenant.zendesk.com`, een subdomein is **mijn tenant**.

6. Het token voor geheim voor uw Zendesk-account bevindt zich in **Admin** > **API** > **instellingen**. Zorg ervoor dat **tokentoegang** is ingesteld op **ingeschakeld**.

    ![Zendesk-Beheerderinstellingen](./media/zendesk-provisioning-tutorial/ZenDesk4.png)

    ![Zendesk-token voor geheim](./media/zendesk-provisioning-tutorial/ZenDesk2.png)

7. Nadat u in de vakken weergegeven in stap 5 hebt ingevuld, selecteert u **testverbinding** om ervoor te zorgen dat Azure AD verbinding kunt maken met Zendesk. Als de verbinding is mislukt, zorg ervoor dat uw Zendesk-account beheerdersmachtigingen heeft en probeer het opnieuw.

    ![Zendesk-testverbinding](./media/zendesk-provisioning-tutorial/ZenDesk19.png)

8. In de **e-mailmelding** vak, voer het e-mailadres van de persoon of groep voor het ontvangen van de inrichting fout-meldingen. Selecteer de **e-mailmelding verzenden wanneer er een fout optreedt** selectievakje.

    ![E-mailmelding Zendesk](./media/zendesk-provisioning-tutorial/ZenDesk9.png)

9. Selecteer **Opslaan**.

10. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory: gebruikers met Zendesk**.

    ![Synchronisatie van Zendesk-gebruiker](./media/zendesk-provisioning-tutorial/ZenDesk10.png)

11. Controleer de kenmerken van de gebruiker die worden gesynchroniseerd vanuit Azure AD met Zendesk in de **kenmerktoewijzingen** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in Zendesk voor update-bewerkingen. Als u wilt alle wijzigingen hebt opgeslagen, selecteert u **opslaan**.

    ![Zendesk overeenkomende gebruikerskenmerken](./media/zendesk-provisioning-tutorial/ZenDesk11.png)

12. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory-groepen met Zendesk**.

    ![Synchronisatie met Zendesk](./media/zendesk-provisioning-tutorial/ZenDesk12.png)

13. Bekijk de groepskenmerken die worden gesynchroniseerd vanuit Azure AD met Zendesk in de **kenmerktoewijzingen** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de groepen in Zendesk voor update-bewerkingen. Als u wilt alle wijzigingen hebt opgeslagen, selecteert u **opslaan**.

    ![Overeenkomende groepskenmerken Zendesk](./media/zendesk-provisioning-tutorial/ZenDesk13.png)

14. Als u wilt configureren bereikfilters, volg de instructies in de [scoping filter zelfstudie](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

15. Om in te schakelen van de Azure AD-inrichtingsservice voor Zendesk, in de **instellingen** sectie, wijzigt u **Inrichtingsstatus** naar **op**.

    ![Zendesk-Inrichtingsstatus](./media/zendesk-provisioning-tutorial/ZenDesk14.png)

16. Definieer de gebruikers of groepen die u wilt inrichten met Zendesk. In de **instellingen** sectie, selecteert u de waarden in de gewenste **bereik**.

    ![Zendesk Scope](./media/zendesk-provisioning-tutorial/ZenDesk15.png)

17. Wanneer u klaar om in te richten bent, selecteert u **opslaan**.

    ![Zendesk Save](./media/zendesk-provisioning-tutorial/ZenDesk18.png)

Met deze bewerking wordt gestart voor de initiële synchronisatie van alle gebruikers of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan later wordt gesynchroniseerd. Ze plaatsvindt ongeveer elke 40 minuten, zolang het Azure AD-inrichtingsservice wordt uitgevoerd. 

U kunt de **synchronisatiedetails** sectie aan de vooruitgang en koppelingen naar het rapport over de inrichtingsactiviteit volgen. Het rapport beschrijft de acties die zijn uitgevoerd door de Azure AD-inrichtingsservice in Zendesk.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Connector-beperkingen

* Zendesk ondersteunt het gebruik van groepen voor gebruikers met **Agent** alleen rollen. Zie voor meer informatie de [Zendesk documentatie](https://support.zendesk.com/hc/en-us/articles/203661966-Creating-managing-and-using-groups).

* Wanneer een aangepaste rol is toegewezen aan een gebruiker of groep, de rol die standaard wordt toegewezen door de automatische Azure AD-gebruiker inrichtingsservice ook **Agent**. Alleen Agents kunnen worden toegewezen als een aangepaste rol. Zie voor meer informatie de [Zendesk-API-documentatie](https://developer.zendesk.com/rest_api/docs/support/users#json-format-for-agent-or-admin-requests). 

## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/zendesk-tutorial/tutorial_general_01.png
[2]: ./media/zendesk-tutorial/tutorial_general_02.png
[3]: ./media/zendesk-tutorial/tutorial_general_03.png
