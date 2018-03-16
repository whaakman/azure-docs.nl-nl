---
title: 'Zelfstudie: Samanage configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en gebruikersaccounts aan Samanage ongedaan in te richten.
services: active-directory
documentationcenter: 
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
ms.author: v-ant
ms.openlocfilehash: 8f11beff2c78386f4c3e8130c8e5d72465b8fe87
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="tutorial-configure-samanage-for-automatic-user-provisioning"></a>Zelfstudie: Samanage configureren voor het automatisch gebruikers inrichten

Het doel van deze zelfstudie is ter illustratie van de stappen in Samanage en Azure Active Directory (Azure AD) naar Azure AD configureren om automatisch in te richten en ongedaan inrichten gebruikers en/of groepen aan Samanage worden uitgevoerd.

> [!NOTE]
> Deze zelfstudie wordt een connector die is ingebouwd in de Azure AD-gebruiker inrichtingsservice beschreven. Zie voor belangrijke informatie over wat deze service doet, hoe het werkt en veelgestelde vragen [gebruikers inrichten en opheffen van inrichting voor SaaS-toepassingen met Azure Active Directory automatiseren](./active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al het volgende hebt:

*   Een Azure Active Directory-tenant
*   Een tenant Samanage met de [Professional](https://www.samanage.com/pricing/) plannen of beter ingeschakeld 
*   Een gebruikersaccount in Samanage met beheerdersmachtigingen 

> [!NOTE]
> De Azure AD integratie inrichting is afhankelijk van de [Samanage REST-API](https://www.samanage.com/api/), die beschikbaar zijn voor Samanage teams op het plan Professional of een betere is.

## <a name="adding-samanage-from-the-gallery"></a>Samanage uit de galerie toevoegen
Voordat u Samanage configureert voor automatische gebruikers inrichten met Azure AD, moet u Samanage uit de galerie van Azure AD-toepassing toevoegen aan de lijst met beheerde SaaS-toepassingen.

**Als u wilt Samanage uit de galerie van Azure AD-toepassing toevoegt, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op het **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen** > **alle toepassingen**.

    ![De bedrijfstoepassingen in de sectie][2]
    
3. Als u wilt toevoegen Samanage, klikt u op de **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Samanage**.

    ![Samanage inrichten](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage4.png)

5. Selecteer in het deelvenster resultaten **Samanage**, en klik vervolgens op de **toevoegen** knop Samanage toevoegen aan de lijst met SaaS-toepassingen.

    ![Samanage inrichten](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage6.png)

    ![Samanage inrichten](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage5.png)

## <a name="assigning-users-to-samanage"></a>Gebruikers toewijzen aan Samanage

Azure Active Directory gebruikt een concept 'toewijzingen' genoemd om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch gebruikers inrichten, worden alleen de gebruikers en/of groepen die '' tot een toepassing in Azure AD toegewezen zijn gesynchroniseerd. 

Voordat u configureren en inschakelen van automatisch gebruikers inrichten, moet u bepalen welke gebruikers en/of groepen in Azure AD toegang hebben tot Samanage moeten. Als besloten, kunt u deze gebruikers en/of groepen toewijzen aan Samanage door de volgende instructies te volgen:

*   [Een gebruiker of groep toewijzen aan een enterprise-app](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-samanage"></a>Belangrijke tips voor het toewijzen van gebruikers aan Samanage

*   Het is raadzaam om één Azure AD-gebruiker is toegewezen aan Samanage voor het testen van het automatische configuratie van gebruikers. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Wanneer een gebruiker aan Samanage toewijzen, moet u een geldige rol toepassingsspecifieke (indien beschikbaar) in het dialoogvenster toewijzing. Gebruikers met de **standaardtoegang** rol worden uitgesloten van de inrichting.

## <a name="configuring-automatic-user-provisioning-to-samanage"></a>Configureren van Automatische gebruikersaanvragen Samanage

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD-service inricht voor het maken, bijwerken en uitschakelen van gebruikers en/of groepen in Samanage op basis van gebruiker en/of groep toewijzingen in Azure AD.

> [!TIP]
> U kunt er ook voor kiezen om in te schakelen op basis van SAML eenmalige aanmelding voor Samanage, vindt u de instructies te volgen in de [één Samanage aanmelding zelfstudie](active-directory-saas-samanage-tutorial.md). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatisch gebruikers inrichten, hoewel deze twee functies aanvulling van elkaar. Zie voor meer informatie de [één Samanage aanmelding zelfstudie](active-directory-saas-samanage-tutorial.md).

### <a name="to-configure-automatic-user-provisioning-for-samanage-in-azure-ad"></a>Voor het configureren van automatische gebruikersinrichting voor Samanage in Azure AD:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en blader naar **Azure Active Directory > bedrijfstoepassingen > alle toepassingen**.

2. Selecteer Samanage uit de lijst met SaaS-toepassingen.
 
    ![Samanage inrichten](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage7.png)

3. Selecteer de **inrichten** tabblad.
    
    ![Samanage inrichten](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage8.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Samanage inrichten](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage9.png)

5. Onder de **beheerdersreferenties** sectie, voer de **Admin Username**, en **beheerderswachtwoord** van uw Samanages-account. Voorbeelden van deze waarden zijn:

    *   In de **Admin Username** veld, de gebruikersnaam van het beheerdersaccount op uw Samanage Tenant vullen. Voorbeeld: admin@contoso.com.

    *   In de **beheerderswachtwoord** veld, vult u het wachtwoord dat overeenkomt met de gebruikersnaam van de beheerder eerder hebt opgegeven.

6. De velden die worden weergegeven in stap 5 in te vullen, klik op **testverbinding** om te controleren of Azure AD, kan verbinding maken met Samanage. Als de verbinding is mislukt, zorg ervoor dat uw account Samanage beheerdersmachtigingen heeft en probeer het opnieuw.

    ![Samanage inrichten](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage10.png)

7. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet het inrichtingsproces fout meldingen ontvangen en schakel het selectievakje **e-mailmelding verzenden wanneer een foutoptreedt**.

    ![Samanage inrichten](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage11.png)

8. Klik op **Opslaan**.

9. Onder de **toewijzingen** sectie **synchroniseren Azure Active Directory-gebruikers Samanage**.

    ![Samanage inrichten](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage12.png)

10. Controleer de kenmerken van de gebruiker die gesynchroniseerd zijn van Azure AD naar Samanage in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen overeenkomen met de gebruikersaccounts in Samanage voor update-bewerkingen worden gebruikt. Selecteer de **opslaan** knop eventuele wijzigingen doorvoeren.

    ![Samanage inrichten](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage13.png)

12. Onder de **toewijzingen** sectie **synchroniseren Azure Active Directory-groepen te Samanage**.

    ![Samanage inrichten](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage14.png)

13. Controleer de kenmerken die worden gesynchroniseerd vanuit Azure AD naar Samanage in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen overeenkomen met de groepen in Samanage voor update-bewerkingen worden gebruikt. Selecteer de **opslaan** knop eventuele wijzigingen doorvoeren.

    ![Samanage inrichten](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage15.png)

14. Scope om filters te configureren, raadpleegt u de volgende instructies in de [Scoping filter zelfstudie](./active-directory-saas-scoping-filters.md).

15. Om de Azure AD-service voor Samanage inricht, wijzigen de **inrichting Status** naar **op** in de **instellingen** sectie.

    ![Samanage inrichten](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage16.png)

16. De gebruikers en/of groepen die u wilt definiëren om in te richten voor Samanage door het kiezen van de gewenste waarden in **bereik** in de **instellingen** sectie.

    ![Samanage inrichten](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage17.png)

17. Wanneer u klaar om in te richten bent, klikt u op **opslaan**.

    ![Samanage inrichten](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage18.png)

Deze bewerking begint de initiële synchronisatie van alle gebruikers en/of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de Azure AD-service inricht wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en volg de koppelingen voor het inrichten van activiteitenrapport waarin alle acties die worden uitgevoerd door de Azure AD-service op Samanage inricht.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over automatische account gebruikersaanvragen](./active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Het beheren van gebruikers account inrichten voor zakelijke Apps](active-directory-enterprise-apps-manage-provisioning.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Volgende stappen

* [Informatie over het bekijken van Logboeken en rapporten over het inrichten van de activiteit ophalen](active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/active-directory-saas-samanage-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_03.png
