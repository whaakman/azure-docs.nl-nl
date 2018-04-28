---
title: 'Zelfstudie: Zendesk configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en gebruikersaccounts aan Zendesk ongedaan in te richten.
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
ms.author: v-ant
ms.openlocfilehash: 342c535464ad4145137c89fe38281e85c9dd0463
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="tutorial-configure-zendesk-for-automatic-user-provisioning"></a>Zelfstudie: Zendesk configureren voor het automatisch gebruikers inrichten

Het doel van deze zelfstudie is ter illustratie van de stappen in de Zendesk en Azure Active Directory (Azure AD) naar Azure AD configureren om automatisch te richten en ongedaan inrichten gebruikers en/of groepen aan Zendesk moeten worden uitgevoerd. 

> [!NOTE]
> Deze zelfstudie wordt een connector die is ingebouwd in de Azure AD-gebruiker inrichtingsservice beschreven. Zie voor belangrijke informatie over wat deze service doet, hoe het werkt en veelgestelde vragen [gebruikers inrichten en opheffen van inrichting voor SaaS-toepassingen met Azure Active Directory automatiseren](./active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende vereisten hebt:

*   Een Azure AD-tenant
*   Een Zendesk-tenant met de [Enterprise](https://www.zendesk.com/product/pricing/) plannen of beter ingeschakeld 
*   Een gebruikersaccount in de Zendesk met beheerdersmachtigingen 

> [!NOTE]
> De Azure AD integratie inrichting is afhankelijk van de [Zendesk Rest-API](https://developer.zendesk.com/rest_api/docs/core/introduction), die beschikbaar zijn voor Zendesk-teams van het plan Enterprise of hoger is.

## <a name="adding-zendesk-from-the-gallery"></a>Zendesk uit de galerie toevoegen
Voordat u Zendesk configureert voor automatische gebruikers inrichten met Azure AD, moet u Zendesk uit de galerie van Azure AD-toepassing toevoegen aan de lijst met beheerde SaaS-toepassingen.

**Als u wilt Zendesk uit de galerie van Azure AD-toepassing toevoegt, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op het **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen** > **alle toepassingen**.

    ![De bedrijfstoepassingen in de sectie][2]
    
3. Als u wilt toevoegen Zendesk, klikt u op de **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Zendesk**.

    ![Zendesk-inrichting](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk6.png)

5. Selecteer in het deelvenster resultaten **Zendesk**, en klik vervolgens op de **toevoegen** knop Zendesk toevoegen aan de lijst met SaaS-toepassingen.

    ![Zendesk-inrichting](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk7.png)

    ![Zendesk-inrichting](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk20.png)

## <a name="assigning-users-to-zendesk"></a>Gebruikers toewijzen aan Zendesk

Azure Active Directory gebruikt een concept 'toewijzingen' genoemd om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch gebruikers inrichten, worden alleen de gebruikers en/of groepen die '' tot een toepassing in Azure AD toegewezen zijn gesynchroniseerd. 

Voordat u configureren en inschakelen van automatisch gebruikers inrichten, moet u bepalen welke gebruikers en/of groepen in Azure AD toegang hebben tot Zendesk moeten. Als besloten, kunt u deze gebruikers en/of groepen toewijzen aan Zendesk door de volgende instructies te volgen:

*   [Een gebruiker of groep toewijzen aan een enterprise-app](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-zendesk"></a>Belangrijke tips voor het toewijzen van gebruikers aan Zendesk

*   Het is raadzaam om één Azure AD-gebruiker is toegewezen aan Zendesk voor het testen van het automatische configuratie van gebruikers. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Wanneer een gebruiker aan Zendesk toewijzen, moet u een geldige rol toepassingsspecifieke (indien beschikbaar) in het dialoogvenster toewijzing. Gebruikers met de **standaardtoegang** rol worden uitgesloten van de inrichting.

## <a name="configuring-automatic-user-provisioning-to-zendesk"></a>Configureren van Automatische gebruikersaanvragen Zendesk 

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD-service inricht voor het maken, bijwerken en uitschakelen van gebruikers en/of groepen in de Zendesk op basis van gebruiker en/of groep toewijzingen in Azure AD.

> [!TIP]
> U kunt er ook voor kiezen om in te schakelen op basis van SAML eenmalige aanmelding voor Zendesk, vindt u de instructies te volgen in de [één Zendesk aanmelding zelfstudie](active-directory-saas-zendesk-tutorial.md). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatisch gebruikers inrichten, hoewel deze twee functies aanvulling van elkaar.

### <a name="to-configure-automatic-user-provisioning-for-zendesk-in-azure-ad"></a>Voor het configureren van automatische gebruikersinrichting voor Zendesk in Azure AD:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en blader naar **Azure Active Directory > bedrijfstoepassingen > alle toepassingen**.

2. Selecteer Zendesk uit de lijst met SaaS-toepassingen.
 
    ![Zendesk-inrichting](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk3.png)

3. Selecteer de **inrichten** tabblad.
    
    ![Zendesk-inrichting](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk16.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Zendesk-inrichting](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk1.png)

5. Onder de **beheerdersreferenties** sectie, voer de **Admin Username**, **geheim Token**, en **domein** van uw Zendesk-account. Voorbeelden van deze waarden zijn:

    *   In de **Admin Username** veld, vullen van de gebruikersnaam van het beheerdersaccount op uw Zendesk-tenant. Voorbeeld: admin@contoso.com.

    *   In de **geheim Token** veld, token van de geheime vullen, zoals beschreven in stap 6.

    *   In de **domein** veld, vullen van het subdomein van uw Zendesk-tenant.
    Voorbeeld: voor een account met een tenant-URL van https://my-tenant.zendesk.com, een subdomein zou worden **mijn tenant**.

6. De **geheim Token** voor uw Zendesk-account zich bevindt in **Admin > API > instellingen**. 

    ![Inrichting Zendesk](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk4.png) ![Zendesk-inrichting](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk2.png)

7. De velden die worden weergegeven in stap 5 in te vullen, klik op **testverbinding** om te controleren of Azure AD verbinding kan maken met het Zendesk. Als de verbinding is mislukt, zorg ervoor dat uw Zendesk-account beheerdersmachtigingen heeft en probeer het opnieuw.

    ![Zendesk-inrichting](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk19.png)
    
8. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet het inrichtingsproces fout meldingen ontvangen en schakel het selectievakje in - **e-mailmelding verzenden wanneer een foutoptreedt**.

    ![Zendesk-inrichting](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk9.png)

9. Klik op **Opslaan**.

10. Onder de **toewijzingen** sectie **synchroniseren Azure Active Directory-gebruikers Zendesk**.

    ![Zendesk-inrichting](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk10.png)

11. Controleer de kenmerken van de gebruiker die gesynchroniseerd zijn van Azure AD naar Zendesk in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen overeenkomen met de gebruikersaccounts in de Zendesk voor update-bewerkingen worden gebruikt. Selecteer de **opslaan** knop eventuele wijzigingen doorvoeren.

    ![Zendesk-inrichting](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk11.png)

12. Onder de **toewijzingen** sectie **synchroniseren Azure Active Directory-groepen te ZenDesk**.

    ![Zendesk-inrichting](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk12.png)

13. Controleer de kenmerken die worden gesynchroniseerd vanuit Azure AD naar Zendesk in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen overeenkomen met de groepen in de Zendesk voor update-bewerkingen worden gebruikt. Selecteer de **opslaan** knop eventuele wijzigingen doorvoeren.

    ![Zendesk-inrichting](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk13.png)

14. Scope om filters te configureren, raadpleegt u de volgende instructies in de [Scoping filter zelfstudie](./active-directory-saas-scoping-filters.md).

15. Om de Azure AD voor Zendesk-service inricht, wijzigen de **inrichting Status** naar **op** in de **instellingen** sectie.

    ![Zendesk-inrichting](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk14.png)

16. De gebruikers en/of groepen die u wilt definiëren om in te richten voor Zendesk door het kiezen van de gewenste waarden in **bereik** in de **instellingen** sectie.

    ![Zendesk-inrichting](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk15.png)

17. Wanneer u klaar om in te richten bent, klikt u op **opslaan**.

    ![Zendesk-inrichting](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk18.png)


Deze bewerking begint de initiële synchronisatie van alle gebruikers en/of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de Azure AD-service inricht wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en volg de koppelingen voor het inrichten van activiteitenrapport waarin alle acties die worden uitgevoerd door de Azure AD Zendesk-service inricht.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over automatische account gebruikersaanvragen](./active-directory-saas-provisioning-reporting.md).

## <a name="connector-limitations"></a>Connector-beperkingen
* Zendesk ondersteunt informatie over het gebruik van groepen voor gebruikers met alleen de rollen voor de Agent. Raadpleeg voor meer informatie [Zendesk van documentatie](https://support.zendesk.com/hc/en-us/articles/203661966-Creating-managing-and-using-groups).

## <a name="additional-resources"></a>Aanvullende resources

* [Het beheren van gebruikers account inrichten voor zakelijke Apps](active-directory-enterprise-apps-manage-provisioning.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Volgende stappen

* [Informatie over het bekijken van Logboeken en rapporten over het inrichten van de activiteit ophalen](active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_03.png
