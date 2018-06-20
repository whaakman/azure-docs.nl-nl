---
title: 'Zelfstudie: BlueJeans configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en gebruikersaccounts aan BlueJeans ongedaan in te richten.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd-msft
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/01/2018
ms.author: v-ant
ms.openlocfilehash: d4fe2d14485207aae89c5f82d6c8eaf3ada7f28d
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36226527"
---
# <a name="tutorial-configure-bluejeans-for-automatic-user-provisioning"></a>Zelfstudie: BlueJeans configureren voor het automatisch gebruikers inrichten

Het doel van deze zelfstudie is ter illustratie van de stappen in BlueJeans en Azure Active Directory (Azure AD) naar Azure AD configureren om automatisch in te richten en ongedaan inrichten gebruikers en/of groepen aan BlueJeans worden uitgevoerd.

> [!NOTE]
> Deze zelfstudie wordt een connector die is ingebouwd in de Azure AD-gebruiker inrichtingsservice beschreven. Zie voor belangrijke informatie over wat deze service doet, hoe het werkt en veelgestelde vragen [gebruikers inrichten en opheffen van inrichting voor SaaS-toepassingen met Azure Active Directory automatiseren](./../active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al het volgende hebt:

*   Een Azure AD-tenant
*   Een tenant BlueJeans met de [mijn bedrijf](https://www.BlueJeans.com/pricing) plannen of beter ingeschakeld
*   Een gebruikersaccount in BlueJeans met beheerdersmachtigingen

> [!NOTE]
> De Azure AD integratie inrichting is afhankelijk van de [BlueJeans API](https://BlueJeans.github.io/developer), die beschikbaar zijn voor BlueJeans teams op de standaard-plan of hoger is.

## <a name="adding-bluejeans-from-the-gallery"></a>BlueJeans uit de galerie toevoegen
Voordat u BlueJeans configureert voor het automatisch gebruikers inrichten met Azure AD, moet u BlueJeans uit de galerie van Azure AD-toepassing toevoegen aan de lijst met beheerde SaaS-toepassingen.

**Als u wilt BlueJeans uit de galerie van Azure AD-toepassing toevoegt, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op het **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen** > **alle toepassingen**.

    ![De bedrijfstoepassingen in de sectie][2]
    
3. Als u wilt toevoegen BlueJeans, klikt u op de **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **BlueJeans**.

    ![BlueJeans inrichten](./media/bluejeans-provisioning-tutorial/BluejeansAppSearch.png)

5. Selecteer in het deelvenster resultaten **BlueJeans**, en klik vervolgens op de **toevoegen** knop BlueJeans toevoegen aan de lijst met SaaS-toepassingen.

    ![BlueJeans inrichten](./media/bluejeans-provisioning-tutorial/BluejeansAppSearchResults.png)

    ![BlueJeans inrichten](./media/bluejeans-provisioning-tutorial/BluejeansAppCreate.png)
    
## <a name="assigning-users-to-bluejeans"></a>Gebruikers toewijzen aan BlueJeans

Azure Active Directory gebruikt een concept 'toewijzingen' genoemd om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch gebruikers inrichten, worden alleen de gebruikers en/of groepen die '' tot een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureren en inschakelen van automatisch gebruikers inrichten, moet u bepalen welke gebruikers en/of groepen in Azure AD toegang hebben tot BlueJeans moeten. Als besloten, kunt u deze gebruikers en/of groepen toewijzen aan BlueJeans door de volgende instructies te volgen:

*   [Een gebruiker of groep toewijzen aan een enterprise-app](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-bluejeans"></a>Belangrijke tips voor het toewijzen van gebruikers aan BlueJeans

*   Het is raadzaam om één Azure AD-gebruiker is toegewezen aan BlueJeans voor het testen van het automatische configuratie van gebruikers. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Wanneer een gebruiker aan BlueJeans toewijzen, moet u een geldige rol toepassingsspecifieke (indien beschikbaar) in het dialoogvenster toewijzing. Gebruikers met de **standaardtoegang** rol worden uitgesloten van de inrichting.

## <a name="configuring-automatic-user-provisioning-to-bluejeans"></a>Configureren van Automatische gebruikersaanvragen BlueJeans

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD-service inricht voor het maken, bijwerken en uitschakelen van gebruikers en/of groepen in BlueJeans op basis van gebruiker en/of groep toewijzingen in Azure AD.

> [!TIP]
> U kunt er ook voor kiezen om in te schakelen op basis van SAML eenmalige aanmelding voor BlueJeans, vindt u de instructies te volgen in de [één BlueJeans aanmelding zelfstudie](bluejeans-tutorial.md). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatisch gebruikers inrichten, hoewel deze twee functies aanvulling van elkaar.

### <a name="to-configure-automatic-user-provisioning-for-bluejeans-in-azure-ad"></a>Voor het configureren van automatische gebruikersinrichting voor BlueJeans in Azure AD:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en blader naar **Azure Active Directory > bedrijfstoepassingen > alle toepassingen**.

2. Selecteer BlueJeans uit de lijst met SaaS-toepassingen.
 
    ![BlueJeans inrichten](./media/bluejeans-provisioning-tutorial/Bluejeans2.png)

3. Selecteer de **inrichten** tabblad.

    ![BlueJeans inrichten](./media/bluejeans-provisioning-tutorial/BluejeansProvisioningTab.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![BlueJeans inrichten](./media/bluejeans-provisioning-tutorial/Bluejeans1.png)

5. Onder de **beheerdersreferenties** sectie, voer de **Admin Username**, en **beheerderswachtwoord** van uw account BlueJeans. Voorbeelden van deze waarden zijn:

    *   In de **Admin Username** veld, de gebruikersnaam van het beheerdersaccount op uw tenant BlueJeans vullen. Voorbeeld: admin@contoso.com.

    *   In de **beheerderswachtwoord** veld, vult u het wachtwoord dat overeenkomt met de gebruikersnaam van de beheerder.

6. De velden die worden weergegeven in stap 5 in te vullen, klik op **testverbinding** om te controleren of Azure AD, kan verbinding maken met BlueJeans. Als de verbinding is mislukt, zorg ervoor dat uw account BlueJeans beheerdersmachtigingen heeft en probeer het opnieuw.

    ![BlueJeans inrichten](./media/bluejeans-provisioning-tutorial/BluejeansTestConnection.png)

7. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet het inrichtingsproces fout meldingen ontvangen en schakel het selectievakje in - **e-mailmelding verzenden wanneer een foutoptreedt**.

    ![BlueJeans inrichten](./media/bluejeans-provisioning-tutorial/BluejeansNotificationEmail.png)

8. Klik op **Opslaan**.

9. Onder de **toewijzingen** sectie **synchroniseren Azure Active Directory-gebruikers BlueJeans**.

    ![BlueJeans inrichten](./media/bluejeans-provisioning-tutorial/BluejeansMapping.png)

10. Controleer de kenmerken van de gebruiker die gesynchroniseerd zijn van Azure AD naar BlueJeans in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen overeenkomen met de gebruikersaccounts in BlueJeans voor update-bewerkingen worden gebruikt. Selecteer de **opslaan** knop eventuele wijzigingen doorvoeren.

    ![BlueJeans inrichten](./media/bluejeans-provisioning-tutorial/BluejeansUserMappingAtrributes.png)

11. Scope om filters te configureren, raadpleegt u de volgende instructies in de [Scoping filter zelfstudie](./../active-directory-saas-scoping-filters.md).

12. Om de Azure AD-service voor BlueJeans inricht, wijzigen de **inrichting Status** naar **op** in de **instellingen** sectie.

    ![BlueJeans inrichten](./media/bluejeans-provisioning-tutorial/BluejeansProvisioningStatus.png)

13. De gebruikers en/of groepen die u wilt definiëren om in te richten voor BlueJeans door het kiezen van de gewenste waarden in **bereik** in de **instellingen** sectie.

    ![BlueJeans inrichten](./media/bluejeans-provisioning-tutorial/UserGroupSelection.png)

14. Wanneer u klaar om in te richten bent, klikt u op **opslaan**.

    ![BlueJeans inrichten](./media/bluejeans-provisioning-tutorial/SaveProvisioning.png)

Deze bewerking begint de initiële synchronisatie van alle gebruikers en/of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de Azure AD-service inricht wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en volg de koppelingen voor het inrichten van activiteitenrapport waarin alle acties die worden uitgevoerd door de Azure AD-service op BlueJeans inricht.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over automatische account gebruikersaanvragen](../active-directory-saas-provisioning-reporting.md).

## <a name="connector-limitations"></a>Connector-beperkingen

* Bluejeans gebruikersnamen die langer dan 30 tekens zijn niet toegestaan.

## <a name="additional-resources"></a>Aanvullende resources

* [Het beheren van gebruikers account inrichten voor zakelijke Apps](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Volgende stappen

* [Informatie over het bekijken van Logboeken en rapporten over het inrichten van de activiteit ophalen](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/bluejeans-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/bluejeans-tutorial/tutorial_general_02.png
[3]: ./media/bluejeans-tutorial/tutorial_general_03.png
