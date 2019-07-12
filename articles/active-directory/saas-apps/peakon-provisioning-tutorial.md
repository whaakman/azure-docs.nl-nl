---
title: 'Zelfstudie: Configureer Peakon automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en inrichting ongedaan maken-gebruikersaccounts met Peakon.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: na
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2019
ms.author: zhchia
ms.openlocfilehash: 2547f34432ca1b4b52f34c343bb4aad2f2407f53
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67673143"
---
# <a name="tutorial-configure-peakon-for-automatic-user-provisioning"></a>Zelfstudie: Peakon configureren voor het automatisch inrichten van gebruikers

Het doel van deze zelfstudie is ter illustratie van de stappen om te worden uitgevoerd in de Peakon en Azure Active Directory (Azure AD) naar Azure AD configureren voor automatisch inrichten en verwijdering van gebruikers en/of groepen aan Peakon.

> [!NOTE]
>  Deze zelfstudie beschrijft een connector die is gebaseerd op de Provisioning-Service van Azure AD-gebruiker. Zie voor belangrijke informatie over wat deze service biedt, hoe het werkt en veelgestelde vragen [automatiseren van gebruikersinrichting en -opheffing in SaaS-toepassingen met Azure Active Directory](../manage-apps/user-provisioning.md).
>
> Deze connector is momenteel in Preview. Zie voor meer informatie over de algemene Microsoft Azure gebruiksvoorwaarden voor Preview-functies, [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u de volgende vereisten al hebt

* Een Azure AD-tenant.
* [Een tenant Peakon](https://peakon.com/us/pricing/).
* Een gebruikersaccount in Peakon met beheerdersmachtigingen.

## <a name="assigning-users-to-peakon"></a>Gebruikers toewijzen aan Peakon

Azure Active Directory maakt gebruik van een concept genaamd *toewijzingen* om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers, worden alleen de gebruikers en/of groepen die zijn toegewezen aan een toepassing in Azure AD gesynchroniseerd.

Voordat u configureren en inschakelen van automatische inrichten van gebruikers, moet u bepalen welke gebruikers en/of groepen in Azure AD toegang hebben tot Peakon moeten. Wanneer u beslist, kunt u deze gebruikers en/of groepen toewijzen aan Peakon door de instructies hier:

* [Een gebruiker of groep toewijzen aan een enterprise-app](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-peakon"></a>Belangrijke tips voor het toewijzen van gebruikers aan Peakon 

* Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan Peakon voor het testen van de configuratie van de automatische gebruikersinrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

* Wanneer een gebruiker aan Peakon toewijzen, moet u alle geldige toepassingsspecifieke rollen (indien beschikbaar) selecteren in het dialoogvenster toewijzing. Gebruikers met de **standaardtoegang** rol worden uitgesloten van het inrichten.

## <a name="set-up-peakon-for-provisioning"></a>Peakon instellen voor het inrichten van

1.  Aanmelden bij uw [Peakon-beheerconsole](https://app.Peakon.com/login). Klik op **configuratie**. 

    ![Peakon-beheerconsole](media/Peakon-provisioning-tutorial/Peakon-admin-configuration.png)

2.  Selecteer **integraties**.
    
    ![Werknemer van Peakon-inrichting](media/Peakon-provisioning-tutorial/Peakon-select-integration.png)

3.  Schakel **werknemer inrichting**.

    ![Werknemer van Peakon-inrichting](media/Peakon-provisioning-tutorial/peakon05.png)

4.  Kopieer de waarden voor **SCIM 2.0 URL** en **OAuth Bearer Token**. Deze waarden moeten worden ingevoerd in de **Tenant-URL** en **geheim Token** veld op het tabblad inrichten van uw toepassing Peakon in Azure portal.

    ![Peakon Token maken](media/Peakon-provisioning-tutorial/peakon04.png)

## <a name="add-peakon-from-the-gallery"></a>Peakon uit de galerie toevoegen

Met het configureren van Peakon voor automatisch gebruikers inrichten met Azure AD, moet u Peakon uit de galerie met Azure AD toevoegen aan uw lijst met beheerde SaaS-toepassingen.

1. In de  **[Azure-portal](https://portal.azure.com)** , selecteer in het navigatievenster aan de linkerkant **Azure Active Directory**.

    ![De Azure Active Directory-knop](common/select-azuread.png)

2. Ga naar **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![De blade Enterprise-toepassingen](common/enterprise-applications.png)

3. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop aan de bovenkant van het deelvenster.

    ![De knop nieuwe toepassing](common/add-new-app.png)

4. Voer in het zoekvak **Peakon**, selecteer **Peakon** in het deelvenster voor resultaten en klik vervolgens op de **toevoegen** om toe te voegen van de toepassing.

    ![Peakon in de lijst met resultaten](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-peakon"></a>Automatisch gebruikers inrichten voor Peakon configureren 

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD-inrichtingsservice als u wilt maken, bijwerken en uitschakelen van gebruikers en/of groepen in Peakon op basis van gebruiker en/of groep toewijzingen in Azure AD.

> [!TIP]
> U kunt er ook voor kiezen om in te schakelen op SAML gebaseerde eenmalige aanmelding voor Peakon, vindt u de instructies te volgen in de [Peakon Single sign-on zelfstudie](peakon-tutorial.md). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatisch gebruikers inrichten, hoewel deze twee functies een fraaie aanvulling in elkaar.

### <a name="to-configure-automatic-user-provisioning-for-peakon--in-azure-ad"></a>Het configureren van automatisch gebruikers inrichten voor Peakon in Azure AD:

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Selecteer **bedrijfstoepassingen**en selecteer vervolgens **alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Peakon**.

    ![De koppeling Peakon in de lijst met toepassingen](common/all-applications.png)

3. Selecteer de **Provisioning** tabblad.

    ![Inrichting](common/provisioning.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Inrichting](common/provisioning-automatic.png)

5. Onder de **beheerdersreferenties** sectie, voer de **SCIM 2.0 URL** en **OAuth Bearer Token** waarden eerder hebt opgehaald **Tenant-URL** en **geheim Token** respectievelijk. Klik op **testverbinding** om te controleren of Azure AD kunt verbinden met Peakon. Als de verbinding is mislukt, zorg ervoor dat uw account Peakon beheerdersmachtigingen heeft en probeer het opnieuw.

    ![Tenant-URL + Token](common/provisioning-testconnection-tenanturltoken.png)

7. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet de inrichting fout ontvangen en schakel het selectievakje in - **een e-mailmelding verzenden wanneer een foutoptreedt**.

    ![E-mailmelding](common/provisioning-notification-email.png)

8. Klik op **Opslaan**.

9. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory: gebruikers aan Peakon**.

    ![Peakon gebruikerstoewijzingen](media/Peakon-provisioning-tutorial/Peakon-user-mappings.png)

10. Controleer de kenmerken van de gebruiker die van Azure AD worden gesynchroniseerd naar Peakon in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in Peakon voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

    ![Peakon gebruikerskenmerken](media/Peakon-provisioning-tutorial/Peakon-user-attributes.png)

12. Als u wilt configureren bereikfilters, raadpleegt u de volgende instructies in de [Scoping filter zelfstudie](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).
    
    ![Inrichting van bereik](common/provisioning-scope.png)

15. Wanneer u klaar om in te richten bent, klikt u op **opslaan**.

    ![Bezig met opslaan van de Inrichtingsconfiguratie](common/provisioning-configuration-save.png)

Met deze bewerking wordt gestart voor de initiële synchronisatie van alle gebruikers en/of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de Azure AD-inrichtingsservice wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van rapport van de activiteit, die alle acties die worden uitgevoerd door de Azure AD-inrichtingsservice op Peakon beschrijft.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Connector-beperkingen

* Alle aangepaste gebruikerskenmerken in Peakon moeten worden uitgebreid van Peakon van aangepaste SCIM gebruiker uitbreiding van `urn:ietf:params:scim:schemas:extension:peakon:2.0:User`.

## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../manage-apps/check-status-user-account-provisioning.md)