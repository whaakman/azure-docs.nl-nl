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
ms.openlocfilehash: cf747fb75ea663d2c64038d73f48adb19d9fb804
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2019
ms.locfileid: "59278578"
---
# <a name="tutorial-configure-zendesk-for-automatic-user-provisioning"></a>Zelfstudie: Zendesk voor het automatisch inrichten van gebruikers configureren

Het doel van deze zelfstudie is ter illustratie van de stappen om te worden uitgevoerd in de Zendesk en Azure Active Directory (Azure AD) voor Azure AD configureren voor het automatisch inrichten en de inrichting ongedaan maken van gebruikers en/of groepen met Zendesk.

> [!NOTE]
> Deze zelfstudie beschrijft een connector die is gebaseerd op de Provisioning-Service van Azure AD-gebruiker. Zie voor belangrijke informatie over wat deze service biedt, hoe het werkt en veelgestelde vragen [automatiseren van gebruikersinrichting en -opheffing in SaaS-toepassingen met Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende vereisten hebt:

* Een Azure AD-tenant
* Een Zendesk-tenant met de [Enterprise](https://www.zendesk.com/product/pricing/) plannen of beter ingeschakeld
* Een gebruikersaccount in Zendesk met beheerdersmachtigingen

> [!NOTE]
> De integratie wordt ingericht op Azure AD is afhankelijk van de [Zendesk Rest-API](https://developer.zendesk.com/rest_api/docs/core/introduction), die beschikbaar zijn voor Zendesk-teams van het Enterprise-plan of hoger is.

## <a name="adding-zendesk-from-the-gallery"></a>Zendesk toevoegen vanuit de galerie

Voordat u Zendesk configureert voor automatisch gebruikers inrichten met Azure AD, moet u Zendesk uit de galerie met Azure AD toevoegen aan uw lijst met beheerde SaaS-toepassingen.

**Als u wilt toevoegen Zendesk uit de galerie met Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ **Zendesk** in het zoekvak, selecteer **Zendesk** in het resultaatvenster en klik op de knop **Toevoegen** om de toepassing toe te voegen.

    ![Zendesk in de resultatenlijst](common/search-new-app.png)

## <a name="assigning-users-to-zendesk"></a>Gebruikers toewijzen aan Zendesk

Azure Active Directory maakt gebruik van een concept genaamd "toewijzingen" om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers, worden alleen de gebruikers en/of groepen die '' aan een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureren en inschakelen van automatische inrichten van gebruikers, moet u bepalen welke gebruikers en/of groepen in Azure AD toegang hebben tot Zendesk moeten. Wanneer u beslist, kunt u deze gebruikers en/of groepen toewijzen aan Zendesk door de instructies hier:

* [Een gebruiker of groep toewijzen aan een enterprise-app](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-zendesk"></a>Belangrijke tips voor het toewijzen van gebruikers met Zendesk

* Zendesk-functies worden dynamisch en automatisch ingevuld in de gebruikersinterface van Azure portal vandaag nog. Voordat u Zendesk-rollen toewijzen aan gebruikers, moet een eerste synchronisatie is voltooid op basis van Zendesk om op te halen van de meest recente rollen in uw Zendesk-tenant.

* Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan Zendesk voor het testen van uw eerste automatisch gebruikers inrichten van configuratie. Extra gebruikers en/of groepen kunnen worden toegewezen later als de tests geslaagd zijn.
  
* Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan Zendesk voor het testen van de configuratie van de automatische gebruikersinrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

* Bij het toewijzen van een gebruiker met Zendesk, moet u een geldige toepassingsspecifieke-rol (indien beschikbaar) in het dialoogvenster toewijzing selecteren. Gebruikers met de **standaardtoegang** rol worden uitgesloten van het inrichten.

## <a name="configuring-automatic-user-provisioning-to-zendesk"></a>Configuratie van automatisch gebruikers inrichten met Zendesk 

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD-inrichtingsservice als u wilt maken, bijwerken en uitschakelen van gebruikers en/of groepen in Zendesk op basis van gebruiker en/of groep toewijzingen in Azure AD.

> [!TIP]
> U kunt er ook voor kiezen om in te schakelen op SAML gebaseerde eenmalige aanmelding voor Zendesk, vindt u de instructies te volgen in de [één Zendesk aanmeldings-zelfstudie](zendesk-tutorial.md). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatisch gebruikers inrichten, hoewel deze twee functies een fraaie aanvulling in elkaar.

### <a name="to-configure-automatic-user-provisioning-for-zendesk-in-azure-ad"></a>Het configureren van automatisch gebruikers inrichten voor Zendesk in Azure AD:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en selecteer **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **Zendesk**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Zendesk**.

    ![De koppeling Zendesk in de lijst met toepassingen](common/all-applications.png)

3. Selecteer de **Provisioning** tabblad.

    ![Zendesk Provisioning](./media/zendesk-provisioning-tutorial/ZenDesk16.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Zendesk Provisioning](./media/zendesk-provisioning-tutorial/ZenDesk1.png)

5. Onder de **beheerdersreferenties** sectie, voer de **Admin Username**, **geheim Token**, en **domein** van van uw Zendesk-account. Voorbeelden van deze waarden zijn:

   * In de **Admin Username** veld, de gebruikersnaam van het beheerdersaccount dat op uw Zendesk-tenant te vullen. Voorbeeld: admin@contoso.com.

   * In de **geheim Token** veld, vul het token voor geheim zoals beschreven in stap 6.

   * In de **domein** veld, vullen het subdomein van uw Zendesk-tenant.
     Voorbeeld: Voor een account met een tenant-URL van `https://my-tenant.zendesk.com`, zou uw subdomein **mijn tenant**.

6. De **geheim Token** voor uw Zendesk-account zich bevindt in **Admin > API > instellingen**.
   Zorg ervoor dat **tokentoegang** is ingesteld op **ingeschakeld**.

    ![Zendesk Provisioning](./media/zendesk-provisioning-tutorial/ZenDesk4.png)

    ![Zendesk Provisioning](./media/zendesk-provisioning-tutorial/ZenDesk2.png)

7. Bij het invullen van de velden die in stap 5 wordt weergegeven, klikt u op **testverbinding** om te controleren of Azure AD verbinding kunt maken met Zendesk. Als de verbinding is mislukt, zorg ervoor dat uw Zendesk-account beheerdersmachtigingen heeft en probeer het opnieuw.

    ![Zendesk Provisioning](./media/zendesk-provisioning-tutorial/ZenDesk19.png)

8. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet de inrichting fout ontvangen en schakel het selectievakje in - **een e-mailmelding verzenden wanneer een foutoptreedt**.

    ![Zendesk Provisioning](./media/zendesk-provisioning-tutorial/ZenDesk9.png)

9. Klik op **Opslaan**.

10. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory: gebruikers met Zendesk**.

    ![Zendesk Provisioning](./media/zendesk-provisioning-tutorial/ZenDesk10.png)

11. Controleer de kenmerken van de gebruiker die worden gesynchroniseerd vanuit Azure AD met Zendesk in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in Zendesk voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

    ![Zendesk Provisioning](./media/zendesk-provisioning-tutorial/ZenDesk11.png)

12. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory-groepen met ZenDesk**.

    ![Zendesk Provisioning](./media/zendesk-provisioning-tutorial/ZenDesk12.png)

13. Bekijk de groepskenmerken die worden gesynchroniseerd vanuit Azure AD met Zendesk in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de groepen in Zendesk voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

    ![Zendesk Provisioning](./media/zendesk-provisioning-tutorial/ZenDesk13.png)

14. Als u wilt configureren bereikfilters, raadpleegt u de volgende instructies in de [Scoping filter zelfstudie](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

15. Als wilt inschakelen in de Azure AD-inrichtingsservice voor Zendesk, wijzigt de **Inrichtingsstatus** naar **op** in de **instellingen** sectie.

    ![Zendesk Provisioning](./media/zendesk-provisioning-tutorial/ZenDesk14.png)

16. De gebruikers en/of groepen die u wilt inrichten met Zendesk definiëren door het kiezen van de gewenste waarden in **bereik** in de **instellingen** sectie.

    ![Zendesk Provisioning](./media/zendesk-provisioning-tutorial/ZenDesk15.png)

17. Wanneer u klaar om in te richten bent, klikt u op **opslaan**.

    ![Zendesk Provisioning](./media/zendesk-provisioning-tutorial/ZenDesk18.png)

Met deze bewerking wordt gestart voor de initiële synchronisatie van alle gebruikers en/of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de Azure AD-inrichtingsservice wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van activiteitenrapport, die alle acties die worden uitgevoerd door de Azure AD-inrichtingsservice in Zendesk wordt beschreven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Connector-beperkingen

* Zendesk ondersteunt het gebruik van groepen voor gebruikers met alleen de rollen voor de Agent. Raadpleeg voor meer informatie, [van Zendesk-documentatie](https://support.zendesk.com/hc/en-us/articles/203661966-Creating-managing-and-using-groups).

* Wanneer een aangepaste rol is toegewezen aan een gebruiker en/of de groep, de Azure AD-gebruiker automatische inrichting van de service de standaardrol ook toewijzen **Agent**. Alleen **Agents** een aangepaste rol kan worden toegewezen. Raadpleeg voor meer informatie naar deze [Zendesk-API-documentatie](https://developer.zendesk.com/rest_api/docs/support/users#json-format-for-agent-or-admin-requests).  

## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/zendesk-tutorial/tutorial_general_01.png
[2]: ./media/zendesk-tutorial/tutorial_general_02.png
[3]: ./media/zendesk-tutorial/tutorial_general_03.png
