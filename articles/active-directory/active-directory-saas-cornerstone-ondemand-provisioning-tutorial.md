---
title: 'Zelfstudie: Basis OnDemand configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en gebruikersaccounts naar basis OnDemand ongedaan in te richten.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: v-ant
ms.openlocfilehash: 203bc0934b8db15478e78cbc64e463eafdf83fa9
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-configure-cornerstone-ondemand-for-automatic-user-provisioning"></a>Zelfstudie: Basis OnDemand configureren voor het automatisch gebruikers inrichten


Het doel van deze zelfstudie is ter illustratie van de stappen worden uitgevoerd in basis OnDemand en Azure Active Directory (Azure AD) naar Azure AD configureren om automatisch inrichten en gebruikers en/of groepen aanpassen aan de basis OnDemand ongedaan in te richten.


> [!NOTE]
> Deze zelfstudie wordt een connector die is ingebouwd in de Azure AD-gebruiker inrichtingsservice beschreven. Zie voor belangrijke informatie over wat deze service doet, hoe het werkt en veelgestelde vragen [gebruikers inrichten en opheffen van inrichting voor SaaS-toepassingen met Azure Active Directory automatiseren](./active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende vereisten hebt:

*   Een Azure AD-tenant
*   Een basis OnDemand-tenant
*   Een gebruikersaccount in basis OnDemand met beheerdersmachtigingen


> [!NOTE]
> De Azure AD integratie inrichting is afhankelijk van de [basis OnDemand Webservice](https://help.csod.com/help/csod_0/Content/Resources/Documents/WebServices/CSOD_-_Summary_of_Web_Services_v20151106.pdf), die beschikbaar is voor basis OnDemand teams.

## <a name="adding-cornerstone-ondemand-from-the-gallery"></a>Basis OnDemand uit de galerie toevoegen
Voordat u basis OnDemand configureert voor het automatisch gebruikers inrichten met Azure AD, moet u basis OnDemand uit de galerie van Azure AD-toepassing toevoegen aan de lijst met beheerde SaaS-toepassingen.

**Als u wilt basis OnDemand uit de galerie van Azure AD-toepassing toevoegt, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op het **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen** > **alle toepassingen**.

    ![De bedrijfstoepassingen in de sectie][2]
    
3. Basis OnDemand toevoegen: klik op de **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **basis OnDemand**.

    ![Basis OnDemand inrichten](./media/active-directory-saas-cornerstone-ondemand-provisioning-tutorial/AppSearch.png)

5. Selecteer in het deelvenster resultaten **basis OnDemand**, en klik vervolgens op de **toevoegen** knop basis OnDemand toevoegen aan de lijst met SaaS-toepassingen.

    ![Basis OnDemand inrichten](./media/active-directory-saas-cornerstone-ondemand-provisioning-tutorial/AppSearchResults.png)

    ![Basis OnDemand inrichten](./media/active-directory-saas-cornerstone-ondemand-provisioning-tutorial/AppCreation.png)

## <a name="assigning-users-to-cornerstone-ondemand"></a>Gebruikers toewijzen aan de basis OnDemand

Azure Active Directory gebruikt een concept 'toewijzingen' genoemd om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch gebruikers inrichten, worden alleen de gebruikers en/of groepen die '' tot een toepassing in Azure AD toegewezen zijn gesynchroniseerd. 

Voordat u configureren en inschakelen van automatisch gebruikers inrichten, moet u bepalen welke gebruikers en/of groepen in Azure AD toegang hebben tot basis OnDemand moeten. Als besloten, kunt u deze gebruikers en/of groepen toewijzen aan de basis OnDemand door de volgende instructies te volgen:

*   [Een gebruiker of groep toewijzen aan een enterprise-app](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-cornerstone-ondemand"></a>Belangrijke tips voor het toewijzen van gebruikers aan de basis OnDemand

*   Het is raadzaam om één Azure AD-gebruiker is toegewezen aan de basis OnDemand voor het testen van het automatische configuratie van gebruikers. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Wanneer een gebruiker aan de basis OnDemand toewijzen, moet u een geldige rol toepassingsspecifieke (indien beschikbaar) in het dialoogvenster toewijzing. Gebruikers met de **standaardtoegang** rol worden uitgesloten van de inrichting.

## <a name="configuring-automatic-user-provisioning-to-cornerstone-ondemand"></a>Automatisch gebruikers inrichten op basis OnDemand configureren

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD als u wilt maken, bijwerken en uitschakelen van gebruikers en/of groepen in basis OnDemand op basis van gebruiker en/of groepstoewijzingen in Azure AD-service inricht.


### <a name="to-configure-automatic-user-provisioning-for-cornerstone-ondemand-in-azure-ad"></a>Voor het configureren van automatische gebruikersinrichting voor basis OnDemand in Azure AD:


1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en blader naar **Azure Active Directory > bedrijfstoepassingen > alle toepassingen**.

2. Selecteer basis OnDemand uit de lijst met SaaS-toepassingen.
 
    ![Basis OnDemand inrichten](./media/active-directory-saas-cornerstone-ondemand-provisioning-tutorial/Successcenter2.png)

3. Selecteer de **inrichten** tabblad.

    ![Basis OnDemand inrichten](./media/active-directory-saas-cornerstone-ondemand-provisioning-tutorial/ProvisioningTab.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Basis OnDemand inrichten](./media/active-directory-saas-cornerstone-ondemand-provisioning-tutorial/ProvisioningCredentials.png)

5. Onder de **beheerdersreferenties** sectie, voer de **Admin Username**, **beheerderswachtwoord**, en **domein** van de basis-OnDemand account.

    *   In de **Admin Username** veld, domein\gebruikersnaam van het beheerdersaccount op uw tenant basis OnDemand vullen. Voorbeeld: contoso\admin.

    *   In de **beheerderswachtwoord** veld, vult u het wachtwoord dat overeenkomt met de gebruikersnaam van de beheerder.

    *   In de **domein** veld, de URL van de webservice van de tenant basis OnDemand vullen. Voorbeeld: De service zich bevindt op `https://ws-[corpname].csod.com/feed30/clientdataservice.asmx`voor Contoso van het domein is `https://ws-contoso.csod.com/feed30/clientdataservice.asmx`. Zie voor meer informatie over het ophalen van de URL van de webservice [hier](https://help.csod.com/help/csod_0/Content/Resources/Documents/WebServices/CSOD_Web_Services_-_User-OU_Technical_Specification_v20160222.pdf).

6. De velden die worden weergegeven in stap 5 in te vullen, klik op **testverbinding** om te controleren of Azure AD, kan verbinding maken met basis OnDemand. Als de verbinding is mislukt, zorg ervoor dat uw account basis OnDemand beheerdersmachtigingen heeft en probeer het opnieuw.

    ![Basis OnDemand inrichten](./media/active-directory-saas-cornerstone-ondemand-provisioning-tutorial/TestConnection.png)

7. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet het inrichtingsproces fout meldingen ontvangen en schakel het selectievakje in - **e-mailmelding verzenden wanneer een foutoptreedt**.

    ![Basis OnDemand inrichten](./media/active-directory-saas-cornerstone-ondemand-provisioning-tutorial/EmailNotification.png)

8. Klik op **Opslaan**.

9. Onder de **toewijzingen** sectie **synchroniseren Azure Active Directory-gebruikers basis OnDemand**.

    ![Basis OnDemand inrichten](./media/active-directory-saas-cornerstone-ondemand-provisioning-tutorial/UserMapping.png)

10. Controleer de kenmerken van de gebruiker die zijn gesynchroniseerd vanuit Azure AD op basis van OnDemand in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen overeenkomen met de gebruikersaccounts in basis OnDemand voor update-bewerkingen worden gebruikt. Selecteer de **opslaan** knop eventuele wijzigingen doorvoeren.

    ![Basis OnDemand inrichten](./media/active-directory-saas-cornerstone-ondemand-provisioning-tutorial/UserMappingAttributes.png)

11. Scope om filters te configureren, raadpleegt u de volgende instructies in de [Scoping filter zelfstudie](./active-directory-saas-scoping-filters.md).

12. Om de Azure AD-service voor basis OnDemand inricht, wijzigen de **inrichting Status** naar **op** in de **instellingen** sectie.

    ![Basis OnDemand inrichten](./media/active-directory-saas-cornerstone-ondemand-provisioning-tutorial/ProvisioningStatus.png)

13. De gebruikers en/of groepen die u wilt inrichten op basis OnDemand definiëren door het kiezen van de gewenste waarden in **bereik** in de **instellingen** sectie.

    ![Basis OnDemand inrichten](./media/active-directory-saas-cornerstone-ondemand-provisioning-tutorial/SyncScope.png)

14. Wanneer u klaar om in te richten bent, klikt u op **opslaan**.

    ![Basis OnDemand inrichten](./media/active-directory-saas-cornerstone-ondemand-provisioning-tutorial/Save.png)


Deze bewerking begint de initiële synchronisatie van alle gebruikers en/of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de Azure AD-service inricht wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en volg de koppelingen voor het inrichten van activiteitenrapport waarin alle acties die worden uitgevoerd door de Azure AD-service op basis OnDemand inricht.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over automatische account gebruikersaanvragen](./active-directory-saas-provisioning-reporting.md).
## <a name="connector-limitations"></a>Connector-beperkingen

* De basis-OnDemand **positie** kenmerk verwacht een waarde die overeenkomt met de rollen op de basis OnDemand-portal. De lijst met geldige **positie** waarden kunnen worden verkregen door te navigeren naar **gebruikersrecord bewerken > organisatiestructuur > positie** in de basis OnDemand-portal.
    ![Gebruiker basis OnDemand inrichting bewerken](./media/active-directory-saas-cornerstone-ondemand-provisioning-tutorial/UserEdit.png) ![basis OnDemand inrichting positie](./media/active-directory-saas-cornerstone-ondemand-provisioning-tutorial/UserPosition.png) ![basis OnDemand lijst posities inrichten](./media/active-directory-saas-cornerstone-ondemand-provisioning-tutorial/PostionId.png)
    
## <a name="additional-resources"></a>Aanvullende resources

* [Het beheren van gebruikers account inrichten voor zakelijke Apps](active-directory-enterprise-apps-manage-provisioning.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](manage-apps/what-is-single-sign-on.md)



## <a name="next-steps"></a>Volgende stappen

* [Informatie over het bekijken van Logboeken en rapporten over het inrichten van de activiteit ophalen](active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/active-directory-saas-cornerstone-ondemand-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cornerstone-ondemand-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cornerstone-ondemand-provisioning-tutorial/tutorial_general_03.png
