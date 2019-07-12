---
title: 'Zelfstudie: Configureer Figma automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en inrichting ongedaan maken-gebruikersaccounts met Figma.
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
ms.date: 07/12/2019
ms.author: zhchia
ms.openlocfilehash: b71aa6709b1c93688ea3eece4ce3f4066f9a6b7a
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67673169"
---
# <a name="tutorial-configure-figma-for-automatic-user-provisioning"></a>Zelfstudie: Figma configureren voor het automatisch inrichten van gebruikers

Het doel van deze zelfstudie is ter illustratie van de stappen om te worden uitgevoerd in de Figma en Azure Active Directory (Azure AD) naar Azure AD configureren voor automatisch inrichten en verwijdering van gebruikers en/of groepen aan Figma.

> [!NOTE]
> Deze zelfstudie beschrijft een connector die is gebaseerd op de Provisioning-Service van Azure AD-gebruiker. Zie voor belangrijke informatie over wat deze service biedt, hoe het werkt en veelgestelde vragen [automatiseren van gebruikersinrichting en -opheffing in SaaS-toepassingen met Azure Active Directory](../manage-apps/user-provisioning.md).
>
> Deze connector is momenteel in openbare Preview. Zie voor meer informatie over de algemene Microsoft Azure gebruiksvoorwaarden voor Preview-functies, [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende vereisten hebt:

* Een Azure AD-tenant.
* [Een tenant Figma](https://www.figma.com/pricing/).
* Een gebruikersaccount in Figma met beheerdersmachtigingen.

## <a name="assign-users-to-figma"></a>Gebruikers toewijzen aan Figma.
Azure Active Directory maakt gebruik van een concept genaamd toewijzingen om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers, worden alleen de gebruikers en/of groepen die zijn toegewezen aan een toepassing in Azure AD gesynchroniseerd.

Voordat u configureren en inschakelen van automatische inrichten van gebruikers, moet u bepalen welke gebruikers en/of groepen in Azure AD toegang hebben tot Figma moeten. Wanneer u beslist, kunt u deze gebruikers en/of groepen toewijzen aan Figma door de instructies hier:
 
* [Een gebruiker of groep toewijzen aan een enterprise-app](../manage-apps/assign-user-or-group-access-portal.md)
## <a name="important-tips-for-assigning-users-to-figma"></a>Belangrijke tips voor het toewijzen van gebruikers aan Figma

 * Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan Figma voor het testen van de configuratie van de automatische gebruikersinrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

* Wanneer een gebruiker aan Figma toewijzen, moet u alle geldige toepassingsspecifieke rollen (indien beschikbaar) selecteren in het dialoogvenster toewijzing. Gebruikers met de functie standaardtoegang worden uitgesloten van het inrichten.

## <a name="set-up-figma-for-provisioning"></a>Figma instellen voor het inrichten van

Voordat u Figma configureert voor automatisch gebruikers inrichten met Azure AD, moet u enkele inrichting gegevens ophalen uit Figma.

1. Aanmelden bij uw [Figma-beheerconsole](https://www.Figma.com/). Klik op het tandwielpictogram naast uw tenant.

    ![FigmaFigma-employee-provision](media/Figma-provisioning-tutorial/image0.png)

2. Navigeer naar **Algemeen > logboek in de instellingen bijwerken**.

    ![FigmaFigma-employee-provision](media/Figma-provisioning-tutorial/figma03.png)

3. Kopieer de **Tenant-ID**. Deze waarde wordt gebruikt om samen te stellen de SCIM eindpunt-URL moet worden ingevoerd in de **Tenant-URL** veld op het tabblad inrichten van uw toepassing Figma in Azure portal.

    ![Figma Token maken](media/Figma-provisioning-tutorial/figma-tenantid.png)

4. Schuif naar beneden en klik op **API-Token genereren**.

    ![Figma Token maken](media/Figma-provisioning-tutorial/token.png)

5. Kopieer de **API-Token** waarde. Deze waarde moet worden ingevoerd in de **geheim Token** veld op het tabblad inrichten van uw toepassing Figma in Azure portal. 

    ![Figma Token maken](media/Figma-provisioning-tutorial/figma04.png)

## <a name="add-figma-from-the-gallery"></a>Figma uit de galerie toevoegen

Als u wilt configureren Figma voor automatisch gebruikers inrichten met Azure AD, moet u Figma uit de galerie met Azure AD toevoegen aan uw lijst met beheerde SaaS-toepassingen.

1. In de  **[Azure-portal](https://portal.azure.com)** , selecteer in het navigatievenster aan de linkerkant **Azure Active Directory**.

    ![De Azure Active Directory-knop](common/select-azuread.png)

2. Ga naar **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![De blade Enterprise-toepassingen](common/enterprise-applications.png)

3. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop aan de bovenkant van het deelvenster.

    ![De knop nieuwe toepassing](common/add-new-app.png)

4. Voer in het zoekvak **Figma**, selecteer **Figma** in het deelvenster voor resultaten en klik vervolgens op de **toevoegen** om toe te voegen van de toepassing.

    ![Figma in de lijst met resultaten](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-figma"></a>Automatisch gebruikers inrichten voor Figma configureren 

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD-inrichtingsservice als u wilt maken, bijwerken en uitschakelen van gebruikers en/of groepen in Figma op basis van gebruiker en/of groep toewijzingen in Azure AD.

> [!TIP]
> U kunt er ook voor kiezen om in te schakelen op SAML gebaseerde eenmalige aanmelding voor Figma, vindt u de instructies te volgen in de [Figma Single sign-on zelfstudie](figma-tutorial.md). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatisch gebruikers inrichten, hoewel deze twee functies een fraaie aanvulling in elkaar.

### <a name="to-configure-automatic-user-provisioning-for-figma--in-azure-ad"></a>Het configureren van automatisch gebruikers inrichten voor Figma in Azure AD:

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Selecteer **bedrijfstoepassingen**en selecteer vervolgens **alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer **Figma** in de lijst met toepassingen.

    ![De koppeling Figma in de lijst met toepassingen](common/all-applications.png)

3. Selecteer de **Provisioning** tabblad.

    ![Inrichting](common/provisioning.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Inrichting](common/provisioning-automatic.png)

5. Onder de **beheerdersreferenties** sectie invoer `https://www.figma.com/scim/v2/<TenantID>` in **Tenant-URL** waar **TenantID** is de waarde die u eerder hebt van Figma opgehaald. Invoer van de **API-Token** waarde in de **geheim Token**. Klik op **testverbinding** om te controleren of Azure AD kunt verbinden met Figma. Als de verbinding is mislukt, zorg ervoor dat uw account Figma beheerdersmachtigingen heeft en probeer het opnieuw.

    ![Tenant-URL + Token](common/provisioning-testconnection-tenanturltoken.png)

8. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet de inrichting fout ontvangen en schakel het selectievakje in - **een e-mailmelding verzenden wanneer een foutoptreedt**.

    ![E-mailmelding](common/provisioning-notification-email.png)

9. Klik op **Opslaan**.

10. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory: gebruikers aan Figma**.

    ![Figma gebruikerstoewijzingen](media/Figma-provisioning-tutorial/figma05.png)

11. Controleer de kenmerken van de gebruiker die van Azure AD worden gesynchroniseerd naar Figma in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in Figma voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

    ![Figma gebruikerskenmerken](media/Figma-provisioning-tutorial/figma06.png)

12. Als u wilt configureren bereikfilters, raadpleegt u de volgende instructies in de [Scoping filter zelfstudie](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

13. Wijzigen zodat de Azure AD-inrichtingsservice voor Figma de **Inrichtingsstatus** naar **op** in de **instellingen** sectie.

    ![Inrichtingsstatus ingeschakeld](common/provisioning-toggle-on.png)

14. De gebruikers en/of groepen die u wilt definiëren voor het inrichten van Figma door het kiezen van de gewenste waarden in **bereik** in de **instellingen** sectie.

    ![Inrichting van bereik](common/provisioning-scope.png)

15. Wanneer u klaar om in te richten bent, klikt u op **opslaan**.

    ![Bezig met opslaan van de Inrichtingsconfiguratie](common/provisioning-configuration-save.png)

Met deze bewerking wordt gestart voor de initiële synchronisatie van alle gebruikers en/of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de Azure AD-inrichtingsservice wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van rapport van de activiteit, die alle acties die worden uitgevoerd door de Azure AD-inrichtingsservice op Figma beschrijft.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../manage-apps/check-status-user-account-provisioning.md)