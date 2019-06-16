---
title: 'Zelfstudie: Hoeksteen OnDemand configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en gebruikersaccounts met hoeksteen OnDemand inrichting ongedaan maken.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: v-ant
ms.collection: M365-identity-device-management
ms.openlocfilehash: 85ddcf3aff7d15c946230cedb0da190bca6aeab7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64721937"
---
# <a name="tutorial-configure-cornerstone-ondemand-for-automatic-user-provisioning"></a>Zelfstudie: Hoeksteen OnDemand configureren voor het automatisch inrichten van gebruikers

In deze zelfstudie ziet u de stappen om uit te voeren in de hoeksteen OnDemand en Azure Active Directory (Azure AD) naar Azure AD configureren voor automatisch inrichten en de inrichting van gebruikers of groepen hoeksteen OnDemand ongedaan maken.

> [!NOTE]
> Deze zelfstudie beschrijft een connector die gebaseerd op de Azure AD-gebruiker-service inricht. Zie voor informatie over de werking van deze service, hoe het werkt en veelgestelde vragen, [automatiseren van gebruikersinrichting en -opheffing in software-as-a-service (SaaS)-toepassingen met Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u hebt:

* Een Azure AD-tenant.
* Een OnDemand-hoeksteen-tenant.
* Een gebruikersaccount in hoeksteen OnDemand met beheerdersmachtigingen.

> [!NOTE]
> De integratie wordt ingericht op Azure AD is afhankelijk van de [hoeksteen OnDemand-webservice](https://help.csod.com/help/csod_0/Content/Resources/Documents/WebServices/CSOD_-_Summary_of_Web_Services_v20151106.pdf). Deze service is beschikbaar voor hoeksteen OnDemand teams.

## <a name="add-cornerstone-ondemand-from-the-azure-marketplace"></a>Hoeksteen OnDemand toevoegen in Azure Marketplace

Voordat u hoeksteen OnDemand voor automatisch gebruikers inrichten met Azure AD configureren, toevoegen aan uw lijst met beheerde SaaS-toepassingen hoeksteen OnDemand vanuit de Marketplace.

Volg deze stappen om toe te voegen hoeksteen OnDemand vanuit de Marketplace.

1. In de [Azure-portal](https://portal.azure.com), selecteert u in het navigatiedeelvenster aan de linkerkant, **Azure Active Directory**.

    ![De Azure Active Directory-pictogram](common/select-azuread.png)

2. Ga naar **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u een nieuwe toepassing wilt toevoegen, selecteert u **Nieuwe toepassing** bovenaan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Voer in het zoekvak **hoeksteen OnDemand** en selecteer **hoeksteen OnDemand** vanuit het deelvenster resultaten. Als u wilt de toepassing hebt toegevoegd, selecteert u **toevoegen**.

    ![Cornerstone OnDemand in de resultatenlijst](common/search-new-app.png)

## <a name="assign-users-to-cornerstone-ondemand"></a>Gebruikers toewijzen aan hoeksteen OnDemand

Azure Active Directory maakt gebruik van een concept genaamd *toewijzingen* om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers, worden alleen de gebruikers of groepen die zijn toegewezen aan een toepassing in Azure AD gesynchroniseerd.

Voordat u configureren en automatische inrichting inschakelen, moet u bepalen welke gebruikers of groepen in Azure AD toegang hebben tot hoeksteen OnDemand moeten. Volg de instructies in deze gebruikers of groepen om aan te wijzen hoeksteen OnDemand, [een gebruiker of groep toewijzen aan een enterprise-app](../manage-apps/assign-user-or-group-access-portal.md).

### <a name="important-tips-for-assigning-users-to-cornerstone-ondemand"></a>Belangrijke tips voor het toewijzen van gebruikers aan hoeksteen OnDemand

* Het is raadzaam dat u één Azure AD-gebruiker te hoeksteen OnDemand voor het testen van de configuratie van de automatische gebruikersinrichting. U kunt extra gebruikers of groepen later toewijzen.

* Wanneer u een gebruiker aan hoeksteen OnDemand toewijzen, selecteer een geldige toepassingsspecifieke rol, indien beschikbaar, in het dialoogvenster toewijzing. Gebruikers met de **standaardtoegang** rol worden uitgesloten van het inrichten.

## <a name="configure-automatic-user-provisioning-to-cornerstone-ondemand"></a>Automatisch gebruikers inrichten voor hoeksteen OnDemand configureren

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD-inrichtingsservice. Gebruik te maken, bijwerken en gebruikers of groepen in hoeksteen OnDemand uitschakelen op basis van de gebruiker of groep toewijzingen in Azure AD.

Volg deze stappen voor het configureren van automatisch gebruikers inrichten voor hoeksteen OnDemand in Azure AD.

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Selecteer **bedrijfstoepassingen** > **alle toepassingen** > **hoeksteen OnDemand**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **hoeksteen OnDemand**.

    ![De hoeksteen OnDemand-koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer de **Provisioning** tabblad.

    ![Cornerstone OnDemand Provisioning](./media/cornerstone-ondemand-provisioning-tutorial/ProvisioningTab.png)

4. Stel **Inrichtingsmodus** naar **automatische**.

    ![Hoeksteen OnDemand inrichting modus](./media/cornerstone-ondemand-provisioning-tutorial/ProvisioningCredentials.png)

5. Onder de **beheerdersreferenties** sectie, voert u de gebruikersnaam van beheerder, admin-wachtwoord en domein van uw hoeksteen-OnDemand-account:

    * In de **Admin Username** vak, vult u in het domein of de gebruikersnaam van het beheerdersaccount dat op uw hoeksteen OnDemand-tenant. Een voorbeeld is contoso\admin.

    * In de **beheerderswachtwoord** vak, vult u het wachtwoord dat overeenkomt met de gebruikersnaam van de beheerder.

    * In de **domein** vak, vult u in de URL van de webservice van de hoeksteen OnDemand-tenant. Bijvoorbeeld, de service bevindt zich in `https://ws-[corpname].csod.com/feed30/clientdataservice.asmx`, en het domein is voor Contoso `https://ws-contoso.csod.com/feed30/clientdataservice.asmx`. Zie voor meer informatie over het ophalen van de URL van de webservice [deze pdf](https://help.csod.com/help/csod_0/Content/Resources/Documents/WebServices/CSOD_Web_Services_-_User-OU_Technical_Specification_v20160222.pdf).

6. Nadat u in de vakken weergegeven in stap 5 hebt ingevuld, selecteert u **testverbinding** om ervoor te zorgen dat Azure AD kunt verbinden met hoeksteen OnDemand. Als de verbinding is mislukt, zorg ervoor dat uw account hoeksteen OnDemand beheerdersmachtigingen heeft en probeer het opnieuw.

    ![OnDemand-testverbinding hoeksteen](./media/cornerstone-ondemand-provisioning-tutorial/TestConnection.png)

7. In de **e-mailmelding** vak, voer het e-mailadres van de persoon of groep voor het ontvangen van de inrichting fout-meldingen. Selecteer de **e-mailmelding verzenden wanneer er een fout optreedt** selectievakje.

    ![Hoeksteen OnDemand meldingse-mail](./media/cornerstone-ondemand-provisioning-tutorial/EmailNotification.png)

8. Selecteer **Opslaan**.

9. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory: gebruikers aan hoeksteen OnDemand**.

    ![Hoeksteen OnDemand synchronisatie](./media/cornerstone-ondemand-provisioning-tutorial/UserMapping.png)

10. Controleer de kenmerken van de gebruiker die van Azure AD worden gesynchroniseerd naar hoeksteen OnDemand in de **kenmerktoewijzingen** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in hoeksteen OnDemand voor update-bewerkingen. Als u wilt alle wijzigingen hebt opgeslagen, selecteert u **opslaan**.

    ![Hoeksteen OnDemand kenmerktoewijzingen](./media/cornerstone-ondemand-provisioning-tutorial/UserMappingAttributes.png)

11. Als u wilt configureren bereikfilters, volg de instructies in de [scoping filter zelfstudie](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

12. Om in te schakelen van de Azure AD-inrichtingsservice voor hoeksteen OnDemand, in de **instellingen** sectie, wijzigt u **Inrichtingsstatus** naar **op**.

    ![Hoeksteen OnDemand-Inrichtingsstatus](./media/cornerstone-ondemand-provisioning-tutorial/ProvisioningStatus.png)

13. De gebruikers of groepen die u wilt definiëren voor het inrichten van hoeksteen OnDemand. In de **instellingen** sectie, selecteert u de waarden in de gewenste **bereik**.

    ![Hoeksteen OnDemand bereik](./media/cornerstone-ondemand-provisioning-tutorial/SyncScope.png)

14. Wanneer u klaar om in te richten bent, selecteert u **opslaan**.

    ![Hoeksteen OnDemand opslaan](./media/cornerstone-ondemand-provisioning-tutorial/Save.png)

Met deze bewerking wordt gestart voor de initiële synchronisatie van alle gebruikers of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan later wordt gesynchroniseerd. Ze plaatsvindt ongeveer elke 40 minuten, zolang het Azure AD-inrichtingsservice wordt uitgevoerd. 

U kunt de **synchronisatiedetails** sectie aan de vooruitgang en koppelingen naar het rapport over de inrichtingsactiviteit volgen. Het rapport worden alle acties die zijn uitgevoerd door de Azure AD-inrichtingsservice op hoeksteen OnDemand beschreven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Connector-beperkingen

De hoeksteen OnDemand **positie** kenmerk wordt verwacht dat een waarde die overeenkomt met de rollen op de hoeksteen OnDemand-portal. Voor een lijst van geldige **positie** waarden, Ga naar **gebruikersrecord bewerken > structuur van organisatie > positie** in de hoeksteen OnDemand-portal.

![Hoeksteen OnDemand inrichting gebruikersrecord bewerken](./media/cornerstone-ondemand-provisioning-tutorial/UserEdit.png)

![Cornerstone OnDemand Provisioning Position](./media/cornerstone-ondemand-provisioning-tutorial/UserPosition.png)

![Lijst van hoeksteen positie OnDemand inrichten](./media/cornerstone-ondemand-provisioning-tutorial/PostionId.png)

## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/cornerstone-ondemand-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/cornerstone-ondemand-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/cornerstone-ondemand-provisioning-tutorial/tutorial_general_03.png
