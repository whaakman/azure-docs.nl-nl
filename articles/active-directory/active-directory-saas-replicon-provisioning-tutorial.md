---
title: 'Zelfstudie: Replicon configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en gebruikersaccounts aan Replicon ongedaan in te richten.
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
ms.date: 02/20/2018
ms.author: v-ant
ms.openlocfilehash: b6f536ba1711076eab0eb9dcbea7d357e9cebf3d
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-configure-replicon-for-automatic-user-provisioning"></a>Zelfstudie: Replicon configureren voor het automatisch gebruikers inrichten

Het doel van deze zelfstudie is ter illustratie van de stappen in Replicon en Azure Active Directory (Azure AD) naar Azure AD configureren om automatisch in te richten en ongedaan inrichten gebruikers en/of groepen aan Replicon worden uitgevoerd.

> [!NOTE]
> Deze zelfstudie wordt een connector die is ingebouwd in de Azure AD-gebruiker inrichtingsservice beschreven. Zie voor belangrijke informatie over wat deze service doet, hoe het werkt en veelgestelde vragen [gebruikers inrichten en opheffen van inrichting voor SaaS-toepassingen met Azure Active Directory automatiseren](./active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure AD-tenant
*   Een tenant Replicon met de [Plus](https://www.replicon.com/time-bill-pricing/) plannen of beter ingeschakeld
*   Een gebruikersaccount in Replicon met beheerdersmachtigingen

> [!NOTE]
> De Azure AD integratie inrichting is afhankelijk van de [Replicon API](https://www.replicon.com/help/developers), die voor Replicon teams beschikbaar is op het plusteken plannen of sneller.

## <a name="adding-replicon-from-the-gallery"></a>Replicon uit de galerie toevoegen
Voordat u Replicon configureert voor het automatisch gebruikers inrichten met Azure AD, moet u Replicon uit de galerie van Azure AD-toepassing toevoegen aan de lijst met beheerde SaaS-toepassingen.

**Als u wilt Replicon uit de galerie van Azure AD-toepassing toevoegt, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op het **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen** > **alle toepassingen**.

    ![De bedrijfstoepassingen in de sectie][2]
    
3. Als u wilt toevoegen Replicon, klikt u op de **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Replicon**.

    ![Replicon toepassing zoeken](./media/active-directory-saas-replicon-provisioning-tutorial/RepliconAppSearch.png)

5. Selecteer in het deelvenster resultaten **Replicon**, en klik vervolgens op de **toevoegen** knop Replicon toevoegen aan de lijst met SaaS-toepassingen.

    ![Zoekresultaten replicon](./media/active-directory-saas-replicon-provisioning-tutorial/RepliconAppSearchResults.png)

    ![Replicon-toepassing maken](./media/active-directory-saas-replicon-provisioning-tutorial/RepliconAppCreate.png)


## <a name="assigning-users-to-replicon"></a>Gebruikers toewijzen aan Replicon

Azure Active Directory gebruikt een concept 'toewijzingen' genoemd om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch gebruikers inrichten, worden alleen de gebruikers en/of groepen die '' tot een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureren en inschakelen van automatisch gebruikers inrichten, moet u bepalen welke gebruikers en/of groepen in Azure AD toegang hebben tot Replicon moeten. Als besloten, kunt u deze gebruikers en/of groepen toewijzen aan Replicon door de volgende instructies te volgen:

*   [Een gebruiker of groep toewijzen aan een enterprise-app](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-replicon"></a>Belangrijke tips voor het toewijzen van gebruikers aan Replicon

*   Het is raadzaam om één Azure AD-gebruiker is toegewezen aan Replicon voor het testen van het automatische configuratie van gebruikers. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Wanneer een gebruiker aan Replicon toewijzen, moet u een geldige rol toepassingsspecifieke (indien beschikbaar) in het dialoogvenster toewijzing. Gebruikers met de **standaardtoegang** rol worden uitgesloten van de inrichting.

## <a name="configuring-automatic-user-provisioning-to-replicon"></a>Configureren van Automatische gebruikersaanvragen Replicon 

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD-service inricht voor het maken, bijwerken en uitschakelen van gebruikers en/of groepen in Replicon op basis van gebruiker en/of groep toewijzingen in Azure AD.

> [!TIP]
> U kunt er ook voor kiezen om in te schakelen op basis van SAML eenmalige aanmelding voor Replicon, vindt u de instructies te volgen in de [één Replicon aanmelding zelfstudie](active-directory-saas-replicon-tutorial.md). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatisch gebruikers inrichten, hoewel deze twee functies aanvulling van elkaar.

### <a name="to-configure-automatic-user-provisioning-for-replicon-in-azure-ad"></a>Voor het configureren van automatische gebruikersinrichting voor Replicon in Azure AD:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en blader naar **Azure Active Directory > bedrijfstoepassingen > alle toepassingen**.

2. Selecteer Replicon uit de lijst met SaaS-toepassingen.

    ![Replicon inrichten](./media/active-directory-saas-replicon-provisioning-tutorial/Replicon2.png)

3. Selecteer de **inrichten** tabblad.

    ![Replicon inrichten](./media/active-directory-saas-replicon-provisioning-tutorial/RepliconProvisioningTab.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Replicon inrichten](./media/active-directory-saas-replicon-provisioning-tutorial/Replicon1.png)

5. Onder de **beheerdersreferenties** sectie, voer de **Admin Username**, **beheerderswachtwoord**, **CompanyId**, en **domein**  van uw Replicon-account. Voorbeelden van deze waarden zijn:

    *   In de **Admin Username** veld, de gebruikersnaam van het beheerdersaccount op uw tenant Replicon vullen. Voorbeeld: contosoadmin.

    *   In de **beheerderswachtwoord** veld, vult u het wachtwoord dat overeenkomt met de gebruikersnaam van de beheerder.

    *   In de **CompanyId** veld, de CompanyId van uw tenant Replicon vullen. Bijvoorbeeld: De CompanyID op basis van het logboek in onderstaande is Contoso.

    ![Replicon aanmelding](./media/active-directory-saas-replicon-provisioning-tutorial/RepliconLogin.png)

    *   In de **domein** veld, vullen van het domein, zoals beschreven in stap 6.
    
6. Verkrijgen **serviceEndpointRootURL** voor uw Replicon tenant-account op basis van de stappen die worden vermeld in [Replicon Service Help](https://www.replicon.com/help/determining-the-url-for-your-service-calls). Bij het verkrijgen van de URL, de **domein** het subdomein van **serviceEndpointRootURL** als gemarkeerde. 

    ![Replicon inrichten](./media/active-directory-saas-replicon-provisioning-tutorial/RepliconEndpoint.png)

7. De velden die worden weergegeven in stap 5 in te vullen, klik op **testverbinding** om te controleren of Azure AD, kan verbinding maken met Replicon. Als de verbinding is mislukt, zorg ervoor dat uw account Replicon beheerdersmachtigingen heeft en probeer het opnieuw.

    ![Replicon inrichten](./media/active-directory-saas-replicon-provisioning-tutorial/RepliconTestConnection.png)

8. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet het inrichtingsproces fout meldingen ontvangen en schakel het selectievakje **e-mailmelding verzenden wanneer een foutoptreedt**.

    ![Replicon inrichten](./media/active-directory-saas-replicon-provisioning-tutorial/RepliconNotificationEmail.png)

9. Klik op **Opslaan**.

10. Onder de **toewijzingen** sectie **synchroniseren Azure Active Directory-gebruikers Replicon**.
    
    ![Replicon inrichten](./media/active-directory-saas-replicon-provisioning-tutorial/RepliconUserMapping.png)

11. Controleer de kenmerken van de gebruiker die gesynchroniseerd zijn van Azure AD naar Replicon in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen overeenkomen met de gebruikersaccounts in Replicon voor update-bewerkingen worden gebruikt. Selecteer de **opslaan** knop eventuele wijzigingen doorvoeren.

    ![Replicon inrichten](./media/active-directory-saas-replicon-provisioning-tutorial/RepliconUserMappingAtrributes.png)

12. Scope om filters te configureren, raadpleegt u de volgende instructies in de [Scoping filter zelfstudie](./active-directory-saas-scoping-filters.md).

13. Om de Azure AD-service voor Replicon inricht, wijzigen de **inrichting Status** naar **op** in de **instellingen** sectie.

    ![Replicon inrichten](./media/active-directory-saas-replicon-provisioning-tutorial/RepliconProvisioningStatus.png)

14. De gebruikers en/of groepen die u wilt definiëren om in te richten voor Replicon door het kiezen van de gewenste waarden in **bereik** in de **instellingen** sectie.

    ![Replicon inrichten](./media/active-directory-saas-replicon-provisioning-tutorial/UserGroupSelection.png)

15. Wanneer u klaar om in te richten bent, klikt u op **opslaan**.

    ![Replicon inrichten](./media/active-directory-saas-replicon-provisioning-tutorial/SaveProvisioning.png)

Deze bewerking begint de initiële synchronisatie van alle gebruikers en/of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de Azure AD-service inricht wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en volg de koppelingen voor het inrichten van activiteitenrapport waarin alle acties die worden uitgevoerd door de Azure AD-service op Replicon inricht.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over automatische account gebruikersaanvragen](./active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Het beheren van gebruikers account inrichten voor zakelijke Apps](active-directory-enterprise-apps-manage-provisioning.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Volgende stappen

* [Informatie over het bekijken van Logboeken en rapporten over het inrichten van de activiteit ophalen](active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/active-directory-saas-replicon-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-replicon-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-replicon-tutorial/tutorial_general_03.png
