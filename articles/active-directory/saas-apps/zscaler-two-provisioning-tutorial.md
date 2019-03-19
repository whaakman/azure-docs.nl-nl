---
title: 'Zelfstudie: Twee Zscaler configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en de inrichting ongedaan maken-gebruikersaccounts met Zscaler twee twee.
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
ms.openlocfilehash: 797804be2588fb5c04c052c6f14c5b2b51146c32
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58106505"
---
# <a name="tutorial-configure-zscaler-two-for-automatic-user-provisioning"></a>Zelfstudie: Twee Zscaler configureren voor het automatisch inrichten van gebruikers

Het doel van deze zelfstudie is ter illustratie van de stappen om te worden uitgevoerd in de twee Zscaler en Azure Active Directory (Azure AD) voor Azure AD configureren automatisch inrichten en de inrichting ongedaan maken van gebruikers en/of groepen aan twee Zscaler.

> [!NOTE]
> Deze zelfstudie beschrijft een connector die is gebaseerd op de Provisioning-Service van Azure AD-gebruiker. Zie voor belangrijke informatie over wat deze service biedt, hoe het werkt en veelgestelde vragen [automatiseren van gebruikersinrichting en -opheffing in SaaS-toepassingen met Azure Active Directory](../active-directory-saas-app-provisioning.md).
> 
> Deze connector is momenteel in openbare Preview. Zie voor meer informatie over de algemene Microsoft Azure gebruiksvoorwaarden voor Preview-functies, [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al het volgende hebt:

*   Een Azure AD-tenant
*   Een twee Zscaler-tenant
*   Een gebruikersaccount in twee Zscaler met beheerdersmachtigingen

> [!NOTE]
> De integratie van Azure AD-inrichting is afhankelijk van de Zscaler twee SCIM API die voor twee Zscaler-ontwikkelaars voor accounts met het Enterprise-pakket beschikbaar is.

## <a name="adding-zscaler-two-from-the-gallery"></a>Toe te voegen Zscaler twee uit de galerie
Voordat u gaat twee Zscaler configureren voor automatisch gebruikers inrichten met Azure AD, moet u toevoegen Zscaler twee uit de galerie met Azure AD aan uw lijst met beheerde SaaS-toepassingen.

**Als u wilt toevoegen Zscaler twee uit de galerie met Azure AD, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op de **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen** > **alle toepassingen**.

    ![De sectie voor bedrijfstoepassingen][2]

3. Als u wilt toevoegen Zscaler twee, klikt u op de **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

4. Typ in het zoekvak **Zscaler twee**.

    ![Zscaler Two Provisioning](./media/zscaler-two-provisioning-tutorial/app-search.png)

5. Selecteer in het deelvenster resultaten **Zscaler twee**, en klik vervolgens op de **toevoegen** knop Zscaler twee toevoegen aan uw lijst met SaaS-toepassingen.

    ![Zscaler Two Provisioning](./media/zscaler-two-provisioning-tutorial/app-search-results.png)

    ![Zscaler Two Provisioning](./media/zscaler-two-provisioning-tutorial/app-creation.png)

## <a name="assigning-users-to-zscaler-two"></a>Gebruikers toewijzen aan twee Zscaler

Azure Active Directory maakt gebruik van een concept genaamd "toewijzingen" om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers, worden alleen de gebruikers en/of groepen die '' aan een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureren en inschakelen van automatische inrichten van gebruikers, moet u bepalen welke gebruikers en/of groepen in Azure AD toegang hebben tot twee Zscaler moeten. Wanneer u beslist, kunt u deze gebruikers en/of groepen toewijzen aan Zscaler twee door de instructies hier:

*   [Een gebruiker of groep toewijzen aan een enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-zscaler-two"></a>Belangrijke tips voor het toewijzen van gebruikers aan twee Zscaler

*   Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan twee Zscaler voor het testen van de configuratie van de automatische gebruikersinrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Wanneer een gebruiker toewijzen aan twee Zscaler, moet u alle geldige toepassingsspecifieke rollen (indien beschikbaar) selecteren in het dialoogvenster toewijzing. Gebruikers met de **standaardtoegang** rol worden uitgesloten van het inrichten.

## <a name="configuring-automatic-user-provisioning-to-zscaler-two"></a>Configuratie van automatisch gebruikers inrichten naar twee Zscaler

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD-inrichtingsservice als u wilt maken, bijwerken en uitschakelen van gebruikers en/of groepen in Zscaler twee op basis van gebruiker en/of toewijzingen van groepen in Azure AD.

> [!TIP]
> U kunt er ook voor kiezen om in te schakelen op SAML gebaseerde eenmalige aanmelding voor twee Zscaler, vindt u de instructies te volgen in de [Zscaler twee eenmalige aanmelding zelfstudie](zscaler-two-tutorial.md). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatisch gebruikers inrichten, hoewel deze twee functies een fraaie aanvulling in elkaar.

### <a name="to-configure-automatic-user-provisioning-for-zscaler-two-in-azure-ad"></a>Het configureren van automatisch gebruikers inrichten voor twee Zscaler in Azure AD:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en blader naar **Azure Active Directory > bedrijfstoepassingen > alle toepassingen**.

2. Selecteer twee Zscaler uit de lijst met SaaS-toepassingen.

    ![Zscaler Two Provisioning](./media/zscaler-two-provisioning-tutorial/app-instance-search.png)

3. Selecteer de **Provisioning** tabblad.

    ![Zscaler Two Provisioning](./media/zscaler-two-provisioning-tutorial/provisioning-tab.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Zscaler Two Provisioning](./media/zscaler-two-provisioning-tutorial/provisioning-credentials.png)

5. Onder de **beheerdersreferenties** sectie, voer de **Tenant-URL** en **geheim Token** van uw Zscaler twee account zoals beschreven in stap 6.

6. Verkrijgen van de **Tenant-URL** en **geheim Token**, gaat u naar **beheer > verificatie-instellingen** in de twee Zscaler portal-gebruikersinterface en klik op  **SAML** onder **verificatietype**. 

    ![Zscaler Two Provisioning](./media/zscaler-two-provisioning-tutorial/secret-token-1.png)
    
    Klik op **configureren SAML** openen **configuratie SAML** opties. 

    ![Zscaler Two Provisioning](./media/zscaler-two-provisioning-tutorial/secret-token-2.png)
    
    Selecteer **Enable SCIM-Based inrichting** om op te halen **basis-URL** en **Bearer Token**, sla de instellingen. Kopiëren de **basis-URL** naar **Tenant-URL** en **Bearer Token** naar **geheim Token** in Azure portal.

7. Bij het invullen van de velden die in stap 5 wordt weergegeven, klikt u op **testverbinding** om te controleren of Azure AD verbinding kunt maken met Zscaler twee. Als de verbinding is mislukt, zorg ervoor dat uw Zscaler twee account beheerdersmachtigingen heeft en probeer het opnieuw.

    ![Zscaler Two Provisioning](./media/zscaler-two-provisioning-tutorial/test-connection.png)
    
8. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet de inrichting fout ontvangen en schakel het selectievakje in **een e-mailmelding verzenden wanneer een foutoptreedt**.

    ![Zscaler Two Provisioning](./media/zscaler-two-provisioning-tutorial/notification.png)

9. Klik op **Opslaan**.

10. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory: gebruikers aan twee Zscaler**.

    ![Zscaler Two Provisioning](./media/zscaler-two-provisioning-tutorial/user-mappings.png)

11. Controleer de kenmerken van de gebruiker die van Azure AD worden gesynchroniseerd met Zscaler twee in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in twee Zscaler voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

    ![Zscaler Two Provisioning](./media/zscaler-two-provisioning-tutorial/user-attribute-mappings.png)

12. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory-groepen aan twee Zscaler**.

    ![Zscaler Two Provisioning](./media/zscaler-two-provisioning-tutorial/group-mappings.png)

13. Bekijk de groepskenmerken die van Azure AD worden gesynchroniseerd met Zscaler twee in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de groepen in twee Zscaler voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

    ![Zscaler Two Provisioning](./media/zscaler-two-provisioning-tutorial/group-attribute-mappings.png)

14. Als u wilt configureren bereikfilters, raadpleegt u de volgende instructies in de [Scoping filter zelfstudie](./../active-directory-saas-scoping-filters.md).

15. Wijzigen zodat de Azure AD-inrichtingsservice voor twee Zscaler de **Inrichtingsstatus** naar **op** in de **instellingen** sectie.

    ![Zscaler Two Provisioning](./media/zscaler-two-provisioning-tutorial/provisioning-status.png)

16. De gebruikers en/of groepen die u wilt definiëren om in te richten op twee Zscaler door het kiezen van de gewenste waarden in **bereik** in de **instellingen** sectie.

    ![Zscaler Two Provisioning](./media/zscaler-two-provisioning-tutorial/scoping.png)

17. Wanneer u klaar om in te richten bent, klikt u op **opslaan**.

    ![Zscaler Two Provisioning](./media/zscaler-two-provisioning-tutorial/save-provisioning.png)

Met deze bewerking wordt gestart voor de initiële synchronisatie van alle gebruikers en/of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de Azure AD-inrichtingsservice wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van rapport van de activiteit, die alle acties die worden uitgevoerd door de Azure AD-inrichtingsservice op twee Zscaler beschrijft.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/zscaler-two-provisioning-tutorial/tutorial-general-01.png
[2]: ./media/zscaler-two-provisioning-tutorial/tutorial-general-02.png
[3]: ./media/zscaler-two-provisioning-tutorial/tutorial-general-03.png
