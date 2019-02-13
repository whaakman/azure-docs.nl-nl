---
title: 'Zelfstudie: Bonusly configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en inrichting ongedaan maken-gebruikersaccounts met Bonusly.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: na
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: v-wingf-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: 74c14de0d09006aefb342590f613edb129ab5016
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56173705"
---
# <a name="tutorial-configure-bonusly-for-automatic-user-provisioning"></a>Zelfstudie: Bonusly configureren voor het automatisch inrichten van gebruikers

Het doel van deze zelfstudie is ter illustratie van de stappen om te worden uitgevoerd in de Bonusly en Azure Active Directory (Azure AD) naar Azure AD configureren voor automatisch inrichten en verwijdering van gebruikers en/of groepen aan Bonusly.

> [!NOTE]
> Deze zelfstudie beschrijft een connector die is gebaseerd op de Provisioning-Service van Azure AD-gebruiker. Zie voor belangrijke informatie over wat deze service biedt, hoe het werkt en veelgestelde vragen [automatiseren van gebruikersinrichting en -opheffing in SaaS-toepassingen met Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al het volgende hebt:

*   Een Azure AD-tenant
*   Een [Bonusly tenant](https://bonus.ly/pricing)
*   Een gebruikersaccount in Bonusly met beheerdersmachtigingen

> [!NOTE]
> De integratie wordt ingericht op Azure AD is afhankelijk van de [Bonusly Rest-API](https://bonusly.gelato.io/reference), die voor Bonusly ontwikkelaars beschikbaar is.

## <a name="adding-bonusly-from-the-gallery"></a>Bonusly uit de galerie toe te voegen
Voordat u Bonusly configureert voor het automatisch gebruikers inrichten met Azure AD, moet u Bonusly uit de galerie met Azure AD toevoegen aan uw lijst met beheerde SaaS-toepassingen.

**Als u wilt toevoegen Bonusly uit de galerie met Azure AD, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op de **Azure Active Directory** pictogram. 

    ![De knop Azure Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen** > **alle toepassingen**.

    ![De sectie voor bedrijfstoepassingen][2]
    
3. Als u wilt toevoegen Bonusly, klikt u op de **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

4. Typ in het zoekvak **Bonusly**.

    ![Bonusly inrichten](./media/bonusly-provisioning-tutorial/AppSearch.png)

5. Selecteer in het deelvenster resultaten **Bonusly**, en klik vervolgens op de **toevoegen** knop Bonusly toevoegen aan uw lijst met SaaS-toepassingen.

    ![Bonusly inrichten](./media/bonusly-provisioning-tutorial/AppSearchResults.png)

    ![Bonusly inrichten](./media/bonusly-provisioning-tutorial/AppCreation.png)

## <a name="assigning-users-to-bonusly"></a>Gebruikers toewijzen aan Bonusly

Azure Active Directory maakt gebruik van een concept genaamd "toewijzingen" om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers, worden alleen de gebruikers en/of groepen die '' aan een toepassing in Azure AD toegewezen zijn gesynchroniseerd. 

Voordat u configureren en inschakelen van automatische inrichten van gebruikers, moet u bepalen welke gebruikers en/of groepen in Azure AD toegang hebben tot Bonusly moeten. Wanneer u beslist, kunt u deze gebruikers en/of groepen toewijzen aan Bonusly door de instructies hier:

*   [Een gebruiker of groep toewijzen aan een enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-bonusly"></a>Belangrijke tips voor het toewijzen van gebruikers aan Bonusly

*   Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan Bonusly voor het testen van de configuratie van de automatische gebruikersinrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Wanneer een gebruiker aan Bonusly toewijzen, moet u alle geldige toepassingsspecifieke rollen (indien beschikbaar) selecteren in het dialoogvenster toewijzing. Gebruikers met de **standaardtoegang** rol worden uitgesloten van het inrichten.

## <a name="configuring-automatic-user-provisioning-to-bonusly"></a>Automatisch gebruikers inrichten voor Bonusly configureren

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD-inrichtingsservice als u wilt maken, bijwerken en uitschakelen van gebruikers en/of groepen in Bonusly op basis van gebruiker en/of groep toewijzingen in Azure AD.

> [!TIP]
> U kunt er ook voor kiezen om in te schakelen op SAML gebaseerde eenmalige aanmelding voor Bonusly, vindt u de instructies te volgen in de [Bonusly eenmalige aanmelding zelfstudie](bonus-tutorial.md). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatisch gebruikers inrichten, hoewel deze twee functies een fraaie aanvulling in elkaar.

### <a name="to-configure-automatic-user-provisioning-for-bonusly-in-azure-ad"></a>Het configureren van automatisch gebruikers inrichten voor Bonusly in Azure AD:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en blader naar **Azure Active Directory > bedrijfstoepassingen > alle toepassingen**.

2. Selecteer Bonusly uit de lijst met SaaS-toepassingen.
 
    ![Bonusly inrichten](./media/bonusly-provisioning-tutorial/AppInstanceSearch.png)

3. Selecteer de **Provisioning** tabblad.
    
    ![Bonusly inrichten](./media/bonusly-provisioning-tutorial/ProvisioningTab.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Bonusly inrichten](./media/bonusly-provisioning-tutorial/ProvisioningCredentials.png)

5. Onder de **beheerdersreferenties** sectie, voer de **geheim Token** van uw Bonusly-account, zoals beschreven in stap 6.

6. De **geheim Token** voor uw Bonusly account zich bevindt in **Admin > bedrijfsportal > integraties**. In de **als u code wilt** sectie, klikt u op **API > maken nieuwe API toegangstoken** om een nieuw geheim Token te maken.

    ![Bonusly inrichten](./media/bonusly-provisioning-tutorial/BonuslyIntegrations.png)

    ![Bonusly inrichten](./media/bonusly-provisioning-tutorial/BonsulyRestApi.png)

    ![Bonusly inrichten](./media/bonusly-provisioning-tutorial/CreateToken.png)

7. Typ in het volgende scherm een naam voor het toegangstoken in de opgegeven tekstvak, drukt u vervolgens op **Api-sleutel maken**. Het nieuwe toegangstoken wordt voor een paar seconden in een pop-upvenster weergegeven.

    ![Bonusly inrichten](./media/bonusly-provisioning-tutorial/Token01.png)

    ![Bonusly inrichten](./media/bonusly-provisioning-tutorial/Token02.png)

8. Bij het invullen van de velden die in stap 5 wordt weergegeven, klikt u op **testverbinding** om te controleren of Azure AD kunt verbinden met Bonusly. Als de verbinding is mislukt, zorg ervoor dat uw Bonusly account beheerdersmachtigingen heeft en probeer het opnieuw.

    ![Bonusly inrichten](./media/bonusly-provisioning-tutorial/TestConnection.png)
    
9. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet de inrichting fout ontvangen en schakel het selectievakje in **een e-mailmelding verzenden wanneer een foutoptreedt**.

    ![Bonusly inrichten](./media/bonusly-provisioning-tutorial/EmailNotification.png)

10. Klik op **Opslaan**.

11. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory: gebruikers aan Bonusly**.

    ![Bonusly inrichten](./media/bonusly-provisioning-tutorial/UserMappings.png)

12. Controleer de kenmerken van de gebruiker die van Azure AD worden gesynchroniseerd naar Bonusly in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in Bonusly voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

    ![Bonusly inrichten](./media/bonusly-provisioning-tutorial/UserAttributeMapping.png)

13. Als u wilt configureren bereikfilters, raadpleegt u de volgende instructies in de [Scoping filter zelfstudie](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

14. Wijzigen zodat de Azure AD-inrichtingsservice voor Bonusly de **Inrichtingsstatus** naar **op** in de **instellingen** sectie.

    ![Bonusly inrichten](./media/bonusly-provisioning-tutorial/ProvisioningStatus.png)

15. De gebruikers en/of groepen die u wilt definiëren voor het inrichten van Bonusly door het kiezen van de gewenste waarden in **bereik** in de **instellingen** sectie.

    ![Bonusly inrichten](./media/bonusly-provisioning-tutorial/ScopeSync.png)

16. Wanneer u klaar om in te richten bent, klikt u op **opslaan**.

    ![Bonusly inrichten](./media/bonusly-provisioning-tutorial/SaveProvisioning.png)


Met deze bewerking wordt gestart voor de initiële synchronisatie van alle gebruikers en/of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de Azure AD-inrichtingsservice wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van rapport van de activiteit, die alle acties die worden uitgevoerd door de Azure AD-inrichtingsservice op Bonusly beschrijft.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)


## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/bonusly-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/bonusly-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/bonusly-provisioning-tutorial/tutorial_general_03.png
