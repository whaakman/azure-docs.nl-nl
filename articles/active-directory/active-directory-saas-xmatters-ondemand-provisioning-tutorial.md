---
title: 'Zelfstudie: XMatters OnDemand voor automatisch gebruikers inrichten met Azure Active Directory configureren | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en gebruikersaccounts aan xMatters OnDemand ongedaan in te richten.
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
ms.date: 04/19/2018
ms.author: v-ant-msft
ms.openlocfilehash: e0b945a99766ee52cb357f54d7135fbbdf1fada2
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-configure-xmatters-ondemand-for-automatic-user-provisioning"></a>Zelfstudie: XMatters OnDemand voor automatisch gebruikers inrichten configureren

Het doel van deze zelfstudie is ter illustratie van de stappen in xMatters OnDemand en Azure Active Directory (Azure AD) voor het configureren van Azure AD voor het automatisch inrichten en inrichten van de gebruikers en/of groepen aan xMatters OnDemand moeten worden uitgevoerd.

> [!NOTE]
> Deze zelfstudie wordt een connector die is ingebouwd in de Azure AD-gebruiker inrichtingsservice beschreven. Zie voor belangrijke informatie over wat deze service doet, hoe het werkt en veelgestelde vragen [gebruikers inrichten en opheffen van inrichting voor SaaS-toepassingen met Azure Active Directory automatiseren](./active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al het volgende hebt:

*   Een Azure AD-tenant
*   Een xMatters OnDemand-tenant met de [Starter](https://www.xmatters.com/pricing) plannen of beter ingeschakeld 
*   Een gebruikersaccount in xMatters OnDemand met beheerdersmachtigingen

> [!NOTE]
> De Azure AD integratie inrichting is afhankelijk van de [xMatters OnDemand API](https://help.xmatters.com/xmAPI), die beschikbaar zijn voor xMatters OnDemand teams op het plan Starter of hoger is.

## <a name="adding-xmatters-ondemand-from-the-gallery"></a>XMatters OnDemand uit de galerie toevoegen

Voordat u configureert xMatters OnDemand voor automatisch gebruikers inrichten met Azure AD, moet u xMatters OnDemand uit de galerie van Azure AD-toepassing toevoegen aan de lijst met beheerde SaaS-toepassingen.

**Als u wilt xMatters OnDemand uit de galerie van Azure AD-toepassing toevoegt, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op het **Azure Active Directory** pictogram.

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen** > **alle toepassingen**.

    ![De bedrijfstoepassingen in de sectie][2]
    
3. Als u wilt toevoegen xMatters OnDemand, klikt u op de **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **xMatters OnDemand**.

    ![xMatters OnDemand inrichten](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/AppSearch.png)

5. Selecteer in het deelvenster resultaten **xMatters OnDemand**, en klik vervolgens op de **toevoegen** knop xMatters OnDemand toevoegen aan de lijst met SaaS-toepassingen.

    ![xMatters OnDemand inrichten](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/AppSearchResults.png)

    ![xMatters OnDemand inrichten](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/AppCreation.png)

## <a name="assigning-users-to-xmatters-ondemand"></a>Gebruikers toewijzen aan xMatters OnDemand

Azure Active Directory gebruikt een concept 'toewijzingen' genoemd om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch gebruikers inrichten, worden alleen de gebruikers en/of groepen die '' tot een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureren en inschakelen van automatisch gebruikers inrichten, moet u bepalen welke gebruikers en/of groepen in Azure AD toegang hebben tot xMatters OnDemand moeten. Als besloten, kunt u deze gebruikers en/of groepen toewijzen aan xMatters OnDemand door de volgende instructies te volgen:

*   [Een gebruiker of groep toewijzen aan een enterprise-app](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-xmatters-ondemand"></a>Belangrijke tips voor het toewijzen van gebruikers aan xMatters OnDemand

*   Het is raadzaam om één Azure AD-gebruiker is toegewezen aan xMatters OnDemand voor het testen van het automatische configuratie van gebruikers. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Wanneer een gebruiker aan xMatters OnDemand toewijzen, moet u een geldige rol toepassingsspecifieke (indien beschikbaar) in het dialoogvenster toewijzing. Gebruikers met de **standaardtoegang** rol worden uitgesloten van de inrichting.

## <a name="configuring-automatic-user-provisioning-to-xmatters-ondemand"></a>Configureren van Automatische gebruikersaanvragen xMatters OnDemand 

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD als u wilt maken, bijwerken en uitschakelen van gebruikers en/of groepen in xMatters die OnDemand op basis van gebruiker en/of groepstoewijzingen in Azure AD-service inricht.

> [!TIP]
> U kunt er ook voor kiezen om in te schakelen op basis van SAML eenmalige aanmelding voor xMatters OnDemand, vindt u de instructies te volgen in de [xMatters eenmalige aanmelding OnDemand zelfstudie](active-directory-saas-xmatters-ondemand-tutorial.md). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatisch gebruikers inrichten, hoewel deze twee functies aanvulling van elkaar.

### <a name="to-configure-automatic-user-provisioning-for-xmatters-ondemand-in-azure-ad"></a>Voor het configureren van automatische gebruikersinrichting voor xMatters OnDemand in Azure AD:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en blader naar **Azure Active Directory > bedrijfstoepassingen > alle toepassingen**.

2. Selecteer xMatters OnDemand uit de lijst met SaaS-toepassingen.
 
    ![xMatters OnDemand inrichten](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/ApplicationInstanceSearch.png)

3. Selecteer de **inrichten** tabblad.
    
    ![xMatters OnDemand inrichten](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/ProvisioningTab.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![xMatters OnDemand inrichten](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/ProvisioningCredentials.png)

5. Onder de **beheerdersreferenties** sectie, voer de **Admin Username**, **beheerderswachtwoord**, en **domein** van uw xMatters OnDemand van account.

    *   In de **Admin Username** veld, de gebruikersnaam van het beheerdersaccount op de tenant van uw xMatters OnDemand vullen. Voorbeeld: admin@contoso.com.

    *   In de **beheerderswachtwoord** veld, vult u het wachtwoord dat overeenkomt met de gebruikersnaam van de beheerder.

    *   In de **domein** veld, vullen van het subdomein van uw xMatters OnDemand-tenant.
    Voorbeeld: voor een account met een tenant-URL van https://my-tenant.xmatters.com, een subdomein zou worden **mijn tenant**.

6. De velden die worden weergegeven in stap 5 in te vullen, klik op **testverbinding** om te controleren of Azure AD, kan verbinding maken met xMatters OnDemand. Als de verbinding is mislukt, zorg ervoor dat uw xMatters OnDemand account beheerdersmachtigingen heeft en probeer het opnieuw.

    ![xMatters OnDemand inrichten](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/TestConnection.png)
    
7. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet het inrichtingsproces fout meldingen ontvangen en schakel het selectievakje in - **e-mailmelding verzenden wanneer een foutoptreedt**.

    ![xMatters OnDemand inrichten](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/EmailNotification.png)

8. Klik op **Opslaan**.

9. Onder de **toewijzingen** sectie **synchroniseren Azure Active Directory-gebruikers xMatters OnDemand**.

    ![xMatters OnDemand inrichten](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/SyncUsers.png)

10. Controleer de kenmerken van de gebruiker is gesynchroniseerd vanuit Azure AD xMatters OnDemand in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen overeenkomen met de gebruikersaccounts in xMatters OnDemand voor update-bewerkingen worden gebruikt. Selecteer de **opslaan** knop eventuele wijzigingen doorvoeren.

    ![xMatters OnDemand inrichten](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/UserAttributes.png)

11. Onder de **toewijzingen** sectie **synchroniseren Azure Active Directory-groepen te xMatters OnDemand**.

    ![xMatters OnDemand inrichten](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/SyncGroups.png)

12. De Groepkenmerken die worden gesynchroniseerd vanuit Azure AD naar xMatters OnDemand bekijken in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen overeenkomen met de groepen in xMatters OnDemand voor update-bewerkingen worden gebruikt. Selecteer de **opslaan** knop eventuele wijzigingen doorvoeren.

    ![xMatters OnDemand inrichten](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/GroupAttributes.png)

13. Scope om filters te configureren, raadpleegt u de volgende instructies in de [Scoping filter zelfstudie](./active-directory-saas-scoping-filters.md).

14. Om de Azure AD-service voor xMatters OnDemand inricht, wijzigen de **inrichting Status** naar **op** in de **instellingen** sectie.

    ![xMatters OnDemand inrichten](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/ProvisioningStatus.png)

15. De gebruikers en/of groepen die u wilt definiëren om in te richten voor xMatters OnDemand door het kiezen van de gewenste waarden in **bereik** in de **instellingen** sectie.

    ![xMatters OnDemand inrichten](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/ScopeSelection.png)

16. Wanneer u klaar om in te richten bent, klikt u op **opslaan**.

    ![xMatters OnDemand inrichten](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/SaveProvisioning.png)

Deze bewerking begint de initiële synchronisatie van alle gebruikers en/of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de Azure AD-service inricht wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en volg de koppelingen voor het inrichten van activiteitenrapport waarin alle acties die worden uitgevoerd door de Azure AD-service op xMatters OnDemand inricht.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over automatische account gebruikersaanvragen](./active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Het beheren van gebruikers account inrichten voor zakelijke Apps](active-directory-enterprise-apps-manage-provisioning.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Volgende stappen

* [Informatie over het bekijken van Logboeken en rapporten over het inrichten van de activiteit ophalen](active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/tutorial_general_03.png
