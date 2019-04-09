---
title: 'Zelfstudie: Zscaler ZSCloud configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en de inrichting ongedaan maken-gebruikersaccounts met Zscaler ZSCloud.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: a752be80-d3ef-45d1-ac8f-4fb814c07b07
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: v-ant-msft
ms.openlocfilehash: 8962f0cf79a8e4874018021b1f9009cf3dad844e
ms.sourcegitcommit: b4ad15a9ffcfd07351836ffedf9692a3b5d0ac86
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2019
ms.locfileid: "59058523"
---
# <a name="tutorial-configure-zscaler-zscloud-for-automatic-user-provisioning"></a>Zelfstudie: Zscaler ZSCloud configureren voor het automatisch inrichten van gebruikers

Het doel van deze zelfstudie is ter illustratie van de stappen om te worden uitgevoerd in de Zscaler ZSCloud en Azure Active Directory (Azure AD) naar Azure AD configureren voor het automatisch inrichten en verwijdering van gebruikers en/of groepen aan Zscaler ZSCloud.

> [!NOTE]
> Deze zelfstudie beschrijft een connector die is gebaseerd op de Provisioning-Service van Azure AD-gebruiker. Zie voor belangrijke informatie over wat deze service biedt, hoe het werkt en veelgestelde vragen [automatiseren van gebruikersinrichting en -opheffing in SaaS-toepassingen met Azure Active Directory](../active-directory-saas-app-provisioning.md).
> 
> Deze connector is momenteel in openbare Preview. Zie voor meer informatie over de algemene Microsoft Azure gebruiksvoorwaarden voor Preview-functies, [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al het volgende hebt:

* Een Azure AD-tenant
* A Zscaler ZSCloud tenant
* Een gebruikersaccount in Zscaler ZSCloud met beheerdersmachtigingen

> [!NOTE]
> De integratie van Azure AD-inrichting is afhankelijk van de Zscaler ZSCloud SCIM API, die voor ontwikkelaars van de Zscaler ZSCloud voor accounts met het Enterprise-pakket beschikbaar is.

## <a name="adding-zscaler-zscloud-from-the-gallery"></a>Zscaler ZSCloud uit de galerie toe te voegen

Voordat u Zscaler ZSCloud configureert voor automatisch gebruikers inrichten met Azure AD, moet u Zscaler ZSCloud uit de galerie met Azure AD toevoegen aan uw lijst met beheerde SaaS-toepassingen.

**Als u wilt toevoegen Zscaler ZSCloud uit de galerie met Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Zscaler ZSCloud**, selecteer **Zscaler ZSCloud** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Zscaler ZSCloud in de lijst met resultaten](common/search-new-app.png)

## <a name="assigning-users-to-zscaler-zscloud"></a>Gebruikers toewijzen aan Zscaler ZSCloud

Azure Active Directory maakt gebruik van een concept genaamd "toewijzingen" om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers, worden alleen de gebruikers en/of groepen die '' aan een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureren en inschakelen van automatische inrichten van gebruikers, moet u bepalen welke gebruikers en/of groepen in Azure AD toegang hebben tot Zscaler ZSCloud moeten. Wanneer u beslist, kunt u deze gebruikers en/of groepen toewijzen aan Zscaler ZSCloud door de instructies hier:

* [Een gebruiker of groep toewijzen aan een enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-zscaler-zscloud"></a>Belangrijke tips voor het toewijzen van gebruikers aan Zscaler ZSCloud

* Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan Zscaler ZSCloud voor het testen van de configuratie van de automatische gebruikersinrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

* Wanneer u een gebruiker toewijst aan Zscaler ZSCloud, moet u een geldige toepassingsspecifieke-rol (indien beschikbaar) in het dialoogvenster toewijzing selecteren. Gebruikers met de **standaardtoegang** rol worden uitgesloten van het inrichten.

## <a name="configuring-automatic-user-provisioning-to-zscaler-zscloud"></a>Automatisch gebruikers inrichten voor Zscaler ZSCloud configureren

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD-inrichtingsservice als u wilt maken, bijwerken en uitschakelen van gebruikers en/of groepen in Zscaler ZSCloud op basis van gebruiker en/of toewijzingen van groepen in Azure AD.

> [!TIP]
> U kunt er ook voor kiezen om in te schakelen op SAML gebaseerde eenmalige aanmelding voor Zscaler ZSCloud, vindt u de instructies te volgen in de [één Zscaler ZSCloud aanmeldings-zelfstudie](zscaler-zsCloud-tutorial.md). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatisch gebruikers inrichten, hoewel deze twee functies een fraaie aanvulling in elkaar.

### <a name="to-configure-automatic-user-provisioning-for-zscaler-zscloud-in-azure-ad"></a>Het configureren van automatisch gebruikers inrichten voor Zscaler ZSCloud in Azure AD:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en selecteer **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **Zscaler ZSCloud**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Zscaler ZSCloud**.

    ![De koppeling Zscaler ZSCloud in de lijst met toepassingen](common/all-applications.png)

3. Selecteer de **Provisioning** tabblad.

    ![Zscaler ZSCloud Provisioning](./media/zscaler-zscloud-provisioning-tutorial/provisioningtab.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Zscaler ZSCloud Provisioning](./media/zscaler-zscloud-provisioning-tutorial/provisioningcredentials.png)

5. Onder de **beheerdersreferenties** sectie, voer de **Tenant-URL** en **geheim Token** van uw account Zscaler ZSCloud zoals beschreven in stap 6.

6. Verkrijgen van de **Tenant-URL** en **geheim Token**, gaat u naar **beheer > verificatie-instellingen** in de portal-gebruikersinterface Zscaler ZSCloud en klik op  **SAML** onder **verificatietype**.

    ![Zscaler ZSCloud Provisioning](./media/zscaler-zscloud-provisioning-tutorial/secrettoken1.png)

    Klik op **configureren SAML** openen **configuratie SAML** opties.

    ![Zscaler ZSCloud Provisioning](./media/zscaler-zscloud-provisioning-tutorial/secrettoken2.png)

    Selecteer **Enable SCIM-Based inrichting** om op te halen **basis-URL** en **Bearer Token**, sla de instellingen. Kopiëren de **basis-URL** naar **Tenant-URL** en **Bearer Token** naar **geheim Token** in Azure portal.

7. Bij het invullen van de velden die in stap 5 wordt weergegeven, klikt u op **testverbinding** om te controleren of Azure AD kunt verbinden met Zscaler ZSCloud. Als de verbinding is mislukt, zorg ervoor dat uw account Zscaler ZSCloud beheerdersmachtigingen heeft en probeer het opnieuw.

    ![Zscaler ZSCloud Provisioning](./media/zscaler-zscloud-provisioning-tutorial/testconnection.png)

8. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet de inrichting fout ontvangen en schakel het selectievakje in **een e-mailmelding verzenden wanneer een foutoptreedt**.

    ![Zscaler ZSCloud Provisioning](./media/zscaler-zscloud-provisioning-tutorial/Notification.png)

9. Klik op **Opslaan**.

10. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory: gebruikers aan Zscaler ZSCloud**.

    ![Zscaler ZSCloud Provisioning](./media/zscaler-zscloud-provisioning-tutorial/usermappings.png)

11. Controleer de kenmerken van de gebruiker die van Azure AD worden gesynchroniseerd met Zscaler ZSCloud in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in Zscaler ZSCloud voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

    ![Zscaler ZSCloud Provisioning](./media/zscaler-zscloud-provisioning-tutorial/userattributemappings.png)

12. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory-groepen aan Zscaler ZSCloud**.

    ![Zscaler ZSCloud Provisioning](./media/zscaler-zscloud-provisioning-tutorial/groupmappings.png)

13. Bekijk de groepskenmerken die van Azure AD worden gesynchroniseerd met Zscaler ZSCloud in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de groepen in Zscaler ZSCloud voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

    ![Zscaler ZSCloud Provisioning](./media/zscaler-zscloud-provisioning-tutorial/groupattributemappings.png)

14. Als u wilt configureren bereikfilters, raadpleegt u de volgende instructies in de [Scoping filter zelfstudie](./../active-directory-saas-scoping-filters.md).

15. Wijzigen zodat de Azure AD-inrichtingsservice voor Zscaler ZSCloud de **Inrichtingsstatus** naar **op** in de **instellingen** sectie.

    ![Zscaler ZSCloud Provisioning](./media/zscaler-zscloud-provisioning-tutorial/provisioningstatus.png)

16. De gebruikers en/of groepen die u wilt definiëren voor het inrichten van de Zscaler ZSCloud door het kiezen van de gewenste waarden in **bereik** in de **instellingen** sectie.

    ![Zscaler ZSCloud Provisioning](./media/zscaler-zscloud-provisioning-tutorial/scoping.png)

17. Wanneer u klaar om in te richten bent, klikt u op **opslaan**.

    ![Zscaler ZSCloud Provisioning](./media/zscaler-zscloud-provisioning-tutorial/saveprovisioning.png)

Met deze bewerking wordt gestart voor de initiële synchronisatie van alle gebruikers en/of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de Azure AD-inrichtingsservice wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van rapport van de activiteit, die alle acties die worden uitgevoerd door de Azure AD-inrichtingsservice op Zscaler ZSCloud beschrijft.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/zscaler-zscloud-provisioning-tutorial/tutorial-general-01.png
[2]: ./media/zscaler-zscloud-provisioning-tutorial/tutorial-general-02.png
[3]: ./media/zscaler-zscloud-provisioning-tutorial/tutorial-general-03.png
