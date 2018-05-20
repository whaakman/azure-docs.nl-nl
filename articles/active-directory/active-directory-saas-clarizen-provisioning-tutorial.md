---
title: 'Zelfstudie: Clarizen configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en gebruikersaccounts aan Clarizen ongedaan in te richten.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd-msft
ms.assetid: na
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: v-ant-msft
ms.openlocfilehash: 779fa09781b5ea6e13450d609703bec0c08e38e4
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-configure-clarizen-for-automatic-user-provisioning"></a>Zelfstudie: Clarizen configureren voor het automatisch gebruikers inrichten

Het doel van deze zelfstudie is ter illustratie van de stappen in Clarizen en Azure Active Directory (Azure AD) naar Azure AD configureren om automatisch in te richten en ongedaan inrichten gebruikers en/of groepen aan Clarizen worden uitgevoerd.

> [!NOTE]
> Deze zelfstudie wordt een connector die is ingebouwd in de Azure AD-gebruiker inrichtingsservice beschreven. Zie voor belangrijke informatie over wat deze service doet, hoe het werkt en veelgestelde vragen [gebruikers inrichten en opheffen van inrichting voor SaaS-toepassingen met Azure Active Directory automatiseren](./active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al het volgende hebt:

*   Een Azure AD-tenant
*   Een tenant Clarizen met de [Enterprise Edition](https://www.clarizen.com/product/pricing/) plannen of beter ingeschakeld 
*   Een gebruikersaccount in Clarizen met beheerdersmachtigingen

> [!NOTE]
> De Azure AD integratie inrichting is afhankelijk van de [Clarizen API](https://api.clarizen.com/v2.0/services/), die beschikbaar zijn voor teams in de Enterprise-editie plan Clarizen of hoger is.

## <a name="adding-clarizen-from-the-gallery"></a>Clarizen uit de galerie toevoegen
Voordat u Clarizen configureert voor automatische gebruikers inrichten met Azure AD, moet u Clarizen uit de galerie van Azure AD-toepassing toevoegen aan de lijst met beheerde SaaS-toepassingen.

**Als u wilt Clarizen uit de galerie van Azure AD-toepassing toevoegt, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op het **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen** > **alle toepassingen**.

    ![De bedrijfstoepassingen in de sectie][2]
    
3. Als u wilt toevoegen Clarizen, klikt u op de **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Clarizen**.

    ![Clarizen inrichten](./media/active-directory-saas-clarizen-provisioning-tutorial/AppSearch.png)

5. Selecteer in het deelvenster resultaten **Clarizen**, en klik vervolgens op de **toevoegen** knop Clarizen toevoegen aan de lijst met SaaS-toepassingen.

    ![Clarizen inrichten](./media/active-directory-saas-clarizen-provisioning-tutorial/AppSearchResults.png)

    ![Clarizen inrichten](./media/active-directory-saas-clarizen-provisioning-tutorial/AppCreation.png)

## <a name="assigning-users-to-clarizen"></a>Gebruikers toewijzen aan Clarizen

Azure Active Directory gebruikt een concept 'toewijzingen' genoemd om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch gebruikers inrichten, worden alleen de gebruikers en/of groepen die '' tot een toepassing in Azure AD toegewezen zijn gesynchroniseerd. 

Voordat u configureren en inschakelen van automatisch gebruikers inrichten, moet u bepalen welke gebruikers en/of groepen in Azure AD toegang hebben tot Clarizen moeten. Als besloten, kunt u deze gebruikers en/of groepen toewijzen aan Clarizen door de volgende instructies te volgen:

*   [Een gebruiker of groep toewijzen aan een enterprise-app](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-clarizen"></a>Belangrijke tips voor het toewijzen van gebruikers aan Clarizen

*   Het is raadzaam om één Azure AD-gebruiker is toegewezen aan Clarizen voor het testen van het automatische configuratie van gebruikers. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Wanneer een gebruiker aan Clarizen toewijzen, moet u een geldige rol toepassingsspecifieke (indien beschikbaar) in het dialoogvenster toewijzing. Gebruikers met de **standaardtoegang** rol worden uitgesloten van de inrichting.

## <a name="configuring-automatic-user-provisioning-to-clarizen"></a>Configureren van Automatische gebruikersaanvragen Clarizen 

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD-service inricht voor het maken, bijwerken en uitschakelen van gebruikers en/of groepen in Clarizen op basis van gebruiker en/of groep toewijzingen in Azure AD.

> [!TIP]
> U kunt er ook voor kiezen om in te schakelen op basis van SAML eenmalige aanmelding voor Clarizen, vindt u de instructies te volgen in de [één Clarizen aanmelding zelfstudie](active-directory-saas-clarizen-tutorial.md). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatisch gebruikers inrichten, hoewel deze twee functies aanvulling van elkaar.

### <a name="to-configure-automatic-user-provisioning-for-clarizen-in-azure-ad"></a>Voor het configureren van automatische gebruikersinrichting voor Clarizen in Azure AD:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en blader naar **Azure Active Directory > bedrijfstoepassingen > alle toepassingen**.

2. Selecteer Clarizen uit de lijst met SaaS-toepassingen.
 
    ![Clarizen inrichten](./media/active-directory-saas-clarizen-provisioning-tutorial/AppInstanceSearch.png)

3. Selecteer de **inrichten** tabblad.
    
    ![Clarizen inrichten](./media/active-directory-saas-clarizen-provisioning-tutorial/ProvisioningTab.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Clarizen inrichten](./media/active-directory-saas-clarizen-provisioning-tutorial/ProvisioningCredentials.png)

5. Onder de **beheerdersreferenties** sectie, voer de **domein**, **Admin Username**, en **beheerderswachtwoord** van uw Clarizen-account. Voorbeelden van deze waarden zijn:

    *   In de **Admin Username** veld, de gebruikersnaam van het beheerdersaccount op uw Clarizen Tenant vullen. Voorbeeld: admin@contoso.com.

    *   In de **beheerderswachtwoord** veld, vult u het wachtwoord van het beheerdersaccount dat overeenkomt met de gebruikersnaam van de beheerder.

    *   In de **domein** veld, vullen subdomein op basis van stap 6.
    
6. Ophalen van de **serverLocation** voor uw Clarizen-account op basis van de stappen die worden vermeld onder **verificatie** van [Clarizen van Rest-API handleiding](https://success.clarizen.com/hc/en-us/articles/205711828-REST-API-Guide-Version-2). Gebruik bij het verkrijgen van de serverLocation, het subdomein van de URL als gemarkeerde hieronder, voor het vullen van de **domein** veld.

    ![Clarizen inrichten](./media/active-directory-saas-clarizen-provisioning-tutorial/ClarizenDomain.png)

7. De velden die worden weergegeven in stap 5 in te vullen, klik op **testverbinding** om te controleren of Azure AD, kan verbinding maken met Clarizen. Als de verbinding is mislukt, zorg ervoor dat uw account Clarizen beheerdersmachtigingen heeft en probeer het opnieuw.

    ![Clarizen inrichten](./media/active-directory-saas-clarizen-provisioning-tutorial/TestConnection.png)
    
8. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet het inrichtingsproces fout meldingen ontvangen en schakel het selectievakje **e-mailmelding verzenden wanneer een foutoptreedt**.

    ![Clarizen inrichten](./media/active-directory-saas-clarizen-provisioning-tutorial/NotificationEmail.png)

9. Klik op **Opslaan**.

10. Onder de **toewijzingen** sectie **synchroniseren Azure Active Directory-gebruikers Clarizen**.

    ![Clarizen inrichten](./media/active-directory-saas-clarizen-provisioning-tutorial/UserMapping.png)

11. Controleer de kenmerken van de gebruiker die gesynchroniseerd zijn van Azure AD naar Clarizen in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen overeenkomen met de gebruikersaccounts in Clarizen voor update-bewerkingen worden gebruikt. Selecteer de **opslaan** knop eventuele wijzigingen doorvoeren.

    ![Clarizen inrichten](./media/active-directory-saas-clarizen-provisioning-tutorial/UserMappingAttributes.png)

12. Onder de **toewijzingen** sectie **synchroniseren Azure Active Directory-groepen te Clarizen**.

    ![Clarizen inrichten](./media/active-directory-saas-clarizen-provisioning-tutorial/GroupMapping.png)

13. Controleer de kenmerken die worden gesynchroniseerd vanuit Azure AD naar Clarizen in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen overeenkomen met de groepen in Clarizen voor update-bewerkingen worden gebruikt. Selecteer de **opslaan** knop eventuele wijzigingen doorvoeren.

    ![Clarizen inrichten](./media/active-directory-saas-clarizen-provisioning-tutorial/GroupMappingAttributes.png)

14. Scope om filters te configureren, raadpleegt u de volgende instructies in de [Scoping filter zelfstudie](./active-directory-saas-scoping-filters.md).

15. Om de Azure AD-service voor Clarizen inricht, wijzigen de **inrichting Status** naar **op** in de **instellingen** sectie.

    ![Clarizen inrichten](./media/active-directory-saas-clarizen-provisioning-tutorial/ProvisioningStatus.png)

16. De gebruikers en/of groepen die u wilt definiëren om in te richten voor Clarizen door het kiezen van de gewenste waarden in **bereik** in de **instellingen** sectie.

    ![Clarizen inrichten](./media/active-directory-saas-clarizen-provisioning-tutorial/ScopeSelection.png)

17. Wanneer u klaar om in te richten bent, klikt u op **opslaan**.

    ![Clarizen inrichten](./media/active-directory-saas-clarizen-provisioning-tutorial/SaveProvisioning.png)


Deze bewerking begint de initiële synchronisatie van alle gebruikers en/of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de Azure AD-service inricht wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en volg de koppelingen voor het inrichten van activiteitenrapport waarin alle acties die worden uitgevoerd door de Azure AD-service op Clarizen inricht.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over automatische account gebruikersaanvragen](./active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Het beheren van gebruikers account inrichten voor zakelijke Apps](active-directory-enterprise-apps-manage-provisioning.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Volgende stappen

* [Informatie over het bekijken van Logboeken en rapporten over het inrichten van de activiteit ophalen](active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/active-directory-saas-clarizen-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_03.png
