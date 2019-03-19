---
title: 'Zelfstudie: Zscaler bèta configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en inrichting ongedaan maken-gebruikersaccounts met Zscaler Beta.
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
ms.date: 03/03/2019
ms.author: v-ant-msft
ms.openlocfilehash: 47cfa5b2d97148c6b925a071f46518afd3ec8d5e
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58109641"
---
# <a name="tutorial-configure-zscaler-beta-for-automatic-user-provisioning"></a>Zelfstudie: Zscaler bèta configureren voor het automatisch inrichten van gebruikers

Het doel van deze zelfstudie is ter illustratie van de stappen om te worden uitgevoerd in de Zscaler Beta en Azure Active Directory (Azure AD) naar Azure AD configureren voor automatisch inrichten en verwijdering van gebruikers en/of groepen aan Zscaler Beta.

> [!NOTE]
> Deze zelfstudie beschrijft een connector die is gebaseerd op de Provisioning-Service van Azure AD-gebruiker. Zie voor belangrijke informatie over wat deze service biedt, hoe het werkt en veelgestelde vragen [automatiseren van gebruikersinrichting en -opheffing in SaaS-toepassingen met Azure Active Directory](../active-directory-saas-app-provisioning.md).
> 
> Deze connector is momenteel in openbare Preview. Zie voor meer informatie over de algemene Microsoft Azure gebruiksvoorwaarden voor Preview-functies, [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al het volgende hebt:

*   Een Azure AD-tenant
*   Een bètaversie van de Zscaler-tenant
*   Een gebruikersaccount in de bètafase Zscaler met beheerdersmachtigingen

> [!NOTE]
> De integratie van Azure AD-inrichting is afhankelijk van de Zscaler bèta SCIM API, die voor ontwikkelaars van de Zscaler Bèta voor accounts met het Enterprise-pakket beschikbaar is.

## <a name="adding-zscaler-beta-from-the-gallery"></a>Zscaler Beta toevoegen vanuit de galerie
Voordat u Zscaler bèta configureert voor automatisch gebruikers inrichten met Azure AD, moet u Zscaler bèta uit de galerie met Azure AD toevoegen aan uw lijst met beheerde SaaS-toepassingen.

**Als u wilt toevoegen Zscaler bèta van de Azure AD-toepassingsgalerie, kunt u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op de **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen** > **alle toepassingen**.

    ![De sectie voor bedrijfstoepassingen][2]

3. Als u wilt toevoegen Zscaler Bèta, klikt u op de **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

4. Typ in het zoekvak **Zscaler Bèta**.

    ![Zscaler bèta inrichten](./media/zscaler-beta-provisioning-tutorial/app-search.png)

5. Selecteer in het deelvenster resultaten **Zscaler Bèta**, en klik vervolgens op de **toevoegen** knop Zscaler Bèta toevoegen aan uw lijst met SaaS-toepassingen.

    ![Zscaler bèta inrichten](./media/zscaler-beta-provisioning-tutorial/app-search-results.png)

    ![Zscaler bèta inrichten](./media/zscaler-beta-provisioning-tutorial/app-creation.png)

## <a name="assigning-users-to-zscaler-beta"></a>Gebruikers toewijzen aan de Zscaler bèta

Azure Active Directory maakt gebruik van een concept genaamd "toewijzingen" om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers, worden alleen de gebruikers en/of groepen die '' aan een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureren en inschakelen van automatische inrichten van gebruikers, moet u bepalen welke gebruikers en/of groepen in Azure AD toegang hebben tot Zscaler Beta moeten. Wanneer besloten, kunt u deze gebruikers en/of groepen toewijzen aan Zscaler bèta door de instructies hier:

*   [Een gebruiker of groep toewijzen aan een enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-zscaler-beta"></a>Belangrijke tips voor het toewijzen van gebruikers naar de bètaversie Zscaler

*   Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan Zscaler Bèta voor het testen van de configuratie van de automatische gebruikersinrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Wanneer een gebruiker toewijzen aan Zscaler Bèta, moet u alle geldige toepassingsspecifieke rollen (indien beschikbaar) selecteren in het dialoogvenster toewijzing. Gebruikers met de **standaardtoegang** rol worden uitgesloten van het inrichten.

## <a name="configuring-automatic-user-provisioning-to-zscaler-beta"></a>Configuratie van automatisch gebruikers inrichten naar de bètaversie Zscaler

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD-inrichtingsservice als u wilt maken, bijwerken en uitschakelen van gebruikers en/of groepen in de bètafase Zscaler op basis van gebruiker en/of toewijzingen van groepen in Azure AD.

> [!TIP]
> U kunt er ook voor kiezen om in te schakelen op SAML gebaseerde eenmalige aanmelding voor Zscaler Beta, vindt u de instructies te volgen in de [één Zscaler bèta aanmeldings-zelfstudie](zscaler-beta-tutorial.md). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatisch gebruikers inrichten, hoewel deze twee functies een fraaie aanvulling in elkaar.

### <a name="to-configure-automatic-user-provisioning-for-zscaler-beta-in-azure-ad"></a>Het configureren van automatisch gebruikers inrichten voor Zscaler Beta in Azure AD:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en blader naar **Azure Active Directory > bedrijfstoepassingen > alle toepassingen**.

2. Selecteer Zscaler Beta in uw lijst met SaaS-toepassingen.

    ![Zscaler bèta inrichten](./media/zscaler-beta-provisioning-tutorial/app-instance-search.png)

3. Selecteer de **Provisioning** tabblad.

    ![Zscaler bèta inrichten](./media/zscaler-beta-provisioning-tutorial/provisioning-tab.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Zscaler bèta inrichten](./media/zscaler-beta-provisioning-tutorial/provisioning-credentials.png)

5. Onder de **beheerdersreferenties** sectie, voer de **Tenant-URL** en **geheim Token** van uw Zscaler bèta-account, zoals beschreven in stap 6.

6. Verkrijgen van de **Tenant-URL** en **geheim Token**, gaat u naar **beheer > verificatie-instellingen** in de portal Zscaler bèta-gebruikersinterface en klik op  **SAML** onder **verificatietype**. 

    ![Zscaler bèta inrichten](./media/zscaler-beta-provisioning-tutorial/secret-token-1.png)
    
    Klik op **SAML configureren** openen de **configuratie SAML** opties. 

    ![Zscaler bèta inrichten](./media/zscaler-beta-provisioning-tutorial/secret-token-2.png)
    
    Selecteer **Enable SCIM-Based inrichting** om op te halen **basis-URL** en **Bearer Token**, sla de instellingen. Kopiëren de **basis-URL** naar **Tenant-URL**, en **Bearer Token** naar **geheim Token** in Azure portal.

7. Bij het invullen van de velden die in stap 5 wordt weergegeven, klikt u op **testverbinding** om te controleren of Azure AD kunt verbinden met Zscaler Beta. Als de verbinding is mislukt, zorg ervoor dat uw Zscaler bèta-account beheerdersmachtigingen heeft en probeer het opnieuw.

    ![Zscaler bèta inrichten](./media/zscaler-beta-provisioning-tutorial/test-connection.png)
    
8. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet de inrichting fout ontvangen en schakel het selectievakje in **een e-mailmelding verzenden wanneer een foutoptreedt**.

    ![Zscaler bèta inrichten](./media/zscaler-beta-provisioning-tutorial/notification.png)

9. Klik op **Opslaan**.

10. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory: gebruikers naar de bètaversie Zscaler**.

    ![Zscaler bèta inrichten](./media/zscaler-beta-provisioning-tutorial/user-mappings.png)

11. Controleer de kenmerken van de gebruiker die van Azure AD worden gesynchroniseerd met Zscaler Beta in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in de bètafase Zscaler voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

    ![Zscaler bèta inrichten](./media/zscaler-beta-provisioning-tutorial/user-attribute-mappings.png)

12. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory-groepen naar de bètaversie Zscaler**.

    ![Zscaler bèta inrichten](./media/zscaler-beta-provisioning-tutorial/group-mappings.png)

13. Bekijk de groepskenmerken die van Azure AD worden gesynchroniseerd met Zscaler Beta in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de groepen in de bètafase Zscaler voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

    ![Zscaler bèta inrichten](./media/zscaler-beta-provisioning-tutorial/group-attribute-mappings.png)

14. Als u wilt configureren bereikfilters, raadpleegt u de volgende instructies in de [Scoping filter zelfstudie](./../active-directory-saas-scoping-filters.md).

15. Om in te schakelen in de Azure AD-inrichtingsservice voor Zscaler Beta, wijzigen de **Inrichtingsstatus** naar **op** in de **instellingen** sectie.

    ![Zscaler bèta inrichten](./media/zscaler-beta-provisioning-tutorial/provisioning-status.png)

16. De gebruikers en/of groepen die u wilt definiëren voor het inrichten van de Zscaler bèta door het kiezen van de gewenste waarden in **bereik** in de **instellingen** sectie.

    ![Zscaler bèta inrichten](./media/zscaler-beta-provisioning-tutorial/scoping.png)

17. Wanneer u klaar om in te richten bent, klikt u op **opslaan**.

    ![Zscaler bèta inrichten](./media/zscaler-beta-provisioning-tutorial/save-provisioning.png)

Met deze bewerking wordt gestart voor de initiële synchronisatie van alle gebruikers en/of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de Azure AD-inrichtingsservice wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van rapport van de activiteit, die alle acties die worden uitgevoerd door de Azure AD-inrichtingsservice op Zscaler bèta beschrijft.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/zscaler-beta-provisioning-tutorial/tutorial-general-01.png
[2]: ./media/zscaler-beta-provisioning-tutorial/tutorial-general-02.png
[3]: ./media/zscaler-beta-provisioning-tutorial/tutorial-general-03.png
