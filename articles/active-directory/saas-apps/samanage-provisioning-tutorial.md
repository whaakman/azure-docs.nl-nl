---
title: 'Zelfstudie: Samanage configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en inrichting ongedaan maken-gebruikersaccounts met Samanage.
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
ms.author: v-wingf-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: ca43b62e66e3a736aa52fdd10fe36e635daba245
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59280346"
---
# <a name="tutorial-configure-samanage-for-automatic-user-provisioning"></a>Zelfstudie: Samanage configureren voor het automatisch inrichten van gebruikers

Het doel van deze zelfstudie is ter illustratie van de stappen om te worden uitgevoerd in de Samanage en Azure Active Directory (Azure AD) naar Azure AD configureren voor automatisch inrichten en verwijdering van gebruikers en/of groepen aan Samanage.

> [!NOTE]
> Deze zelfstudie beschrijft een connector die is gebaseerd op de Provisioning-Service van Azure AD-gebruiker. Zie voor belangrijke informatie over wat deze service biedt, hoe het werkt en veelgestelde vragen [automatiseren van gebruikersinrichting en -opheffing in SaaS-toepassingen met Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al het volgende hebt:

* Een Azure AD-tenant
* Een [Samanage tenant](https://www.samanage.com/pricing/) met het pakket voor professionals
* Een gebruikersaccount in Samanage met beheerdersmachtigingen

> [!NOTE]
> De integratie wordt ingericht op Azure AD is afhankelijk van de [Samanage Rest-API](https://www.samanage.com/api/), die voor ontwikkelaars voor accounts met het pakket voor professionals Samanage beschikbaar is.

## <a name="adding-samanage-from-the-gallery"></a>Samanage toevoegen vanuit de galerie

Voordat u Samanage configureert voor automatisch gebruikers inrichten met Azure AD, moet u Samanage uit de galerie met Azure AD toevoegen aan uw lijst met beheerde SaaS-toepassingen.

**Als u wilt toevoegen Samanage uit de galerie met Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ **Samanage** in het zoekvak, selecteer **Samanage** in het resultaatvenster en klik op de knop **Toevoegen** om de toepassing toe te voegen.

    ![Samanage in de resultatenlijst](common/search-new-app.png)

## <a name="assigning-users-to-samanage"></a>Gebruikers toewijzen aan Samanage

Azure Active Directory maakt gebruik van een concept genaamd "toewijzingen" om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers, worden alleen de gebruikers en/of groepen die '' aan een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureren en inschakelen van automatische inrichten van gebruikers, moet u bepalen welke gebruikers en/of groepen in Azure AD toegang hebben tot Samanage moeten. Wanneer u beslist, kunt u deze gebruikers en/of groepen toewijzen aan Samanage door de instructies hier:

*   [Een gebruiker of groep toewijzen aan een enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-samanage"></a>Belangrijke tips voor het toewijzen van gebruikers aan Samanage

*    Samanage functies worden dynamisch en automatisch ingevuld in de gebruikersinterface van Azure portal vandaag nog. Voordat u Samanage rollen toewijzen aan gebruikers, moet u een eerste synchronisatie is voltooid op basis van Samanage naar de nieuwste functies in uw tenant Samanage ophalen.

*    Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan Samanage voor het testen van uw eerste automatisch gebruikers inrichten van configuratie. Extra gebruikers en/of groepen kunnen worden toegewezen later als de tests geslaagd zijn.

*   Wanneer een gebruiker aan Samanage toewijzen, moet u alle geldige toepassingsspecifieke rollen (indien beschikbaar) selecteren in het dialoogvenster toewijzing. Gebruikers met de **standaardtoegang** rol worden uitgesloten van het inrichten.

## <a name="configuring-automatic-user-provisioning-to-samanage"></a>Automatisch gebruikers inrichten voor Samanage configureren

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD-inrichtingsservice als u wilt maken, bijwerken en uitschakelen van gebruikers en/of groepen in Samanage op basis van gebruiker en/of groep toewijzingen in Azure AD.

> [!TIP]
> U kunt er ook voor kiezen om in te schakelen op SAML gebaseerde eenmalige aanmelding voor Samanage, vindt u de instructies te volgen in de [één Samanage aanmeldings-zelfstudie](samanage-tutorial.md). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatisch gebruikers inrichten, hoewel deze twee functies een fraaie aanvulling in elkaar.

### <a name="to-configure-automatic-user-provisioning-for-samanage-in-azure-ad"></a>Het configureren van automatisch gebruikers inrichten voor Samanage in Azure AD:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en selecteer **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **Samanage**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer **Samanage** in de lijst met toepassingen.

    ![De koppeling Samanage in de lijst met toepassingen](common/all-applications.png)

3. Selecteer de **Provisioning** tabblad.

    ![Samanage inrichten](./media/samanage-provisioning-tutorial/ProvisioningTab.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Samanage inrichten](./media/samanage-provisioning-tutorial/ProvisioningCredentials.png)

5. Onder de **beheerdersreferenties** sectie, voer de **Admin Username** en **beheerderswachtwoord** van uw account Samanage. Voorbeelden van deze waarden zijn:

   * In de **Admin Username** veld, de gebruikersnaam van het beheerdersaccount dat op uw tenant Samanage vullen. Voorbeeld: admin@contoso.com.

   * In de **beheerderswachtwoord** veld, het wachtwoord van het beheerdersaccount dat overeenkomt met de gebruikersnaam van beheerder vullen.

6. Bij het invullen van de velden die in stap 5 wordt weergegeven, klikt u op **testverbinding** om te controleren of Azure AD kunt verbinden met Samanage. Als de verbinding is mislukt, zorg ervoor dat uw account Samanage beheerdersmachtigingen heeft en probeer het opnieuw.

    ![Samanage inrichten](./media/samanage-provisioning-tutorial/TestConnection.png)

7. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet de inrichting fout ontvangen en schakel het selectievakje in **een e-mailmelding verzenden wanneer een foutoptreedt**.

    ![Samanage inrichten](./media/samanage-provisioning-tutorial/EmailNotification.png)

8. Klik op **Opslaan**.

9. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory: gebruikers aan Samanage**.

    ![Samanage inrichten](./media/samanage-provisioning-tutorial/UserMappings.png)

10. Controleer de kenmerken van de gebruiker die van Azure AD worden gesynchroniseerd naar Samanage in de **kenmerktoewijzingen** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in Samanage voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

    ![Samanage inrichten](./media/samanage-provisioning-tutorial/UserAttributeMapping.png)

11. Om in te schakelen groepstoewijzingen, onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory-groepen aan Samanage**.

    ![Samanage inrichten](./media/samanage-provisioning-tutorial/GroupMappings.png)

12. Stel **ingeschakeld** naar **Ja** groepen synchroniseren. Bekijk de groepskenmerken die worden gesynchroniseerd vanuit Azure AD naar Samanage in de **kenmerktoewijzingen** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in Samanage voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

    ![Samanage inrichten](./media/samanage-provisioning-tutorial/GroupAttributeMapping.png)

13. Als u wilt configureren bereikfilters, raadpleegt u de volgende instructies in de [Scoping filter zelfstudie](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

14. Wijzigen zodat de Azure AD-inrichtingsservice voor Samanage de **Inrichtingsstatus** naar **op** in de **instellingen** sectie.

    ![Samanage inrichten](./media/samanage-provisioning-tutorial/ProvisioningStatus.png)

15. De gebruikers en/of groepen die u wilt definiëren voor het inrichten van Samanage door het kiezen van de gewenste waarden in **bereik** in de **instellingen** sectie. Bij het kiezen van de **alle gebruikers en groepen synchroniseren** optie, houd rekening met de beperkingen, zoals beschreven in de **Connector beperkingen** onderstaande sectie.

    ![Samanage inrichten](./media/samanage-provisioning-tutorial/ScopeSync.png)

16. Wanneer u klaar om in te richten bent, klikt u op **opslaan**.

    ![Samanage inrichten](./media/samanage-provisioning-tutorial/SaveProvisioning.png)


Met deze bewerking wordt gestart voor de initiële synchronisatie van alle gebruikers en/of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de Azure AD-inrichtingsservice wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van rapport van de activiteit, die alle acties die worden uitgevoerd door de Azure AD-inrichtingsservice op Samanage beschrijft.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Connector-beperkingen

* Als de **alle gebruikers en groepen synchroniseren** optie is geselecteerd en een standaardwaarde is geconfigureerd voor de Samanage **rollen** kenmerk, zorg ervoor dat de gewenste waarde onder de **standaardwaarde indien null (is optioneel)** veld wordt uitgedrukt in de volgende indeling **{'displayName': 'rol'}** waar rol is de gewenste standaardwaarde.

## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)


## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/samanage-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/samanage-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/samanage-provisioning-tutorial/tutorial_general_03.png
