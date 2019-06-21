---
title: 'Zelfstudie: Proxyclick configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en inrichting ongedaan maken-gebruikersaccounts met Proxyclick.
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
ms.date: 06/3/2019
ms.author: zchia
ms.openlocfilehash: 2117f481d213c14f8feeb23eb8af3670db81cab4
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67276637"
---
# <a name="tutorial-configure-proxyclick-for-automatic-user-provisioning"></a>Zelfstudie: Proxyclick configureren voor het automatisch inrichten van gebruikers

Het doel van deze zelfstudie is ter illustratie van de stappen om te worden uitgevoerd in de Proxyclick en Azure Active Directory (Azure AD) naar Azure AD configureren voor automatisch inrichten en verwijdering van gebruikers en/of groepen aan Proxyclick.

> [!NOTE]
> Deze zelfstudie beschrijft een connector die is gebaseerd op de Provisioning-Service van Azure AD-gebruiker. Zie voor belangrijke informatie over wat deze service biedt, hoe het werkt en veelgestelde vragen [automatiseren van gebruikersinrichting en -opheffing in SaaS-toepassingen met Azure Active Directory](../manage-apps/user-provisioning.md).
>
> Deze connector is momenteel in openbare Preview. Zie voor meer informatie over de algemene Microsoft Azure gebruiksvoorwaarden voor Preview-functies, [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende vereisten hebt:

* Een Azure AD-tenant
* [Een tenant Proxyclick](https://www.proxyclick.com/pricing)
* Een gebruikersaccount in Proxyclick met beheerdersmachtigingen.

## <a name="add-proxyclick-from-the-gallery"></a>Proxyclick uit de galerie toevoegen

Voordat u Proxyclick configureert voor automatisch gebruikers inrichten met Azure AD, moet u Proxyclick uit de galerie met Azure AD toevoegen aan uw lijst met beheerde SaaS-toepassingen.

**Als u wilt toevoegen Proxyclick uit de galerie met Azure AD, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)** , selecteer in het navigatievenster aan de linkerkant **Azure Active Directory**.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Ga naar **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop aan de bovenkant van het deelvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Voer in het zoekvak **Proxyclick**, selecteer **Proxyclick** in het deelvenster voor resultaten en klik vervolgens op de **toevoegen** om toe te voegen van de toepassing.

    ![Proxyclick in de lijst met resultaten](common/search-new-app.png)

## <a name="assigning-users-to-proxyclick"></a>Gebruikers toewijzen aan Proxyclick

Azure Active Directory maakt gebruik van een concept genaamd *toewijzingen* om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers, worden alleen de gebruikers en/of groepen die zijn toegewezen aan een toepassing in Azure AD gesynchroniseerd.

Voordat u configureren en inschakelen van automatische inrichten van gebruikers, moet u bepalen welke gebruikers en/of groepen in Azure AD toegang hebben tot Proxyclick moeten. Wanneer u beslist, kunt u deze gebruikers en/of groepen toewijzen aan Proxyclick door de instructies hier:

* [Een gebruiker of groep toewijzen aan een enterprise-app](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-proxyclick"></a>Belangrijke tips voor het toewijzen van gebruikers aan Proxyclick

* Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan Proxyclick voor het testen van de configuratie van de automatische gebruikersinrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

* Wanneer een gebruiker aan Proxyclick toewijzen, moet u alle geldige toepassingsspecifieke rollen (indien beschikbaar) selecteren in het dialoogvenster toewijzing. Gebruikers met de **standaardtoegang** rol worden uitgesloten van het inrichten.

## <a name="configuring-automatic-user-provisioning-to-proxyclick"></a>Automatisch gebruikers inrichten voor Proxyclick configureren 

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD-inrichtingsservice als u wilt maken, bijwerken en uitschakelen van gebruikers en/of groepen in Proxyclick op basis van gebruiker en/of groep toewijzingen in Azure AD.

> [!TIP]
> U kunt er ook voor kiezen om in te schakelen op SAML gebaseerde eenmalige aanmelding voor Proxyclick, vindt u de instructies te volgen in de [één Proxyclick aanmeldings-zelfstudie](proxyclick-tutorial.md). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatisch gebruikers inrichten, hoewel deze twee functies een fraaie aanvulling in elkaar.

### <a name="to-configure-automatic-user-provisioning-for-proxyclick-in-azure-ad"></a>Het configureren van automatisch gebruikers inrichten voor Proxyclick in Azure AD:

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Selecteer **bedrijfstoepassingen**en selecteer vervolgens **alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Proxyclick**.

    ![De koppeling Proxyclick in de lijst met toepassingen](common/all-applications.png)

3. Selecteer de **Provisioning** tabblad.

    ![Inrichting](common/provisioning.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Inrichting](common/provisioning-automatic.png)

5. Om op te halen de **Tenant-URL** en **geheim Token** van uw account Proxyclick, volgt u de procedure zoals beschreven in stap 6.

6. Aanmelden bij uw [Proxyclick-beheerconsole](https://app.proxyclick.com/login//?destination=%2Fdefault). Navigeer naar **instellingen** > **integraties** > **bladeren Marketplace**.

    ![Instellingen voor Proxyclick](media/proxyclick-provisioning-tutorial/proxyclick09.png)

    ![Proxyclick integraties](media/proxyclick-provisioning-tutorial/proxyclick01.png)

    ![Proxyclick Marketplace](media/proxyclick-provisioning-tutorial/proxyclick02.png)

    Selecteer **Azure AD**. Klik op **nu installeren**.

    ![Proxyclick Azure AD](media/proxyclick-provisioning-tutorial/proxyclick03.png)

    ![Proxyclick installeren](media/proxyclick-provisioning-tutorial/proxyclick04.png)

    Selecteer **Gebruikersinrichting** en klikt u op **Start integratie**. 

    ![Proxyclick gebruikers inrichten](media/proxyclick-provisioning-tutorial/proxyclick05.png)

    De configuratie van de juiste instellingen gebruikersinterface moet nu worden weergegeven onder **instellingen** > **integraties**. Selecteer **instellingen** onder **Azure AD (Gebruikersinrichting)** .

    ![Proxyclick Create](media/proxyclick-provisioning-tutorial/proxyclick06.png)

    U vindt de **Tenant-URL** en **geheim Token** hier.

    ![Proxyclick Token maken](media/proxyclick-provisioning-tutorial/proxyclick07.png)

7. Bij het invullen van de velden die in stap 5 wordt weergegeven, klikt u op **testverbinding** om te controleren of Azure AD kunt verbinden met Proxyclick. Als de verbinding is mislukt, zorg ervoor dat uw account Proxyclick beheerdersmachtigingen heeft en probeer het opnieuw.

    ![Token](common/provisioning-testconnection-tenanturltoken.png)

8. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet de inrichting fout ontvangen en schakel het selectievakje in - **een e-mailmelding verzenden wanneer een foutoptreedt**.

    ![E-mailmelding](common/provisioning-notification-email.png)

9. Klik op **Opslaan**.

10. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory: gebruikers aan Proxyclick**.

    ![Proxyclick gebruikerstoewijzingen](media/proxyclick-provisioning-tutorial/Proxyclick-user-mappings.png)

11. Controleer de kenmerken van de gebruiker die van Azure AD worden gesynchroniseerd naar Proxyclick in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in Proxyclick voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

    ![Proxyclick gebruikerskenmerken](media/proxyclick-provisioning-tutorial/Proxyclick-user-attribute.png)

13. Als u wilt configureren bereikfilters, raadpleegt u de volgende instructies in de [Scoping filter zelfstudie](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

14. Wijzigen zodat de Azure AD-inrichtingsservice voor Proxyclick de **Inrichtingsstatus** naar **op** in de **instellingen** sectie.

    ![Inrichtingsstatus ingeschakeld](common/provisioning-toggle-on.png)

15. De gebruikers en/of groepen die u wilt definiëren voor het inrichten van Proxyclick door het kiezen van de gewenste waarden in **bereik** in de **instellingen** sectie.

    ![Inrichting van bereik](common/provisioning-scope.png)

16. Wanneer u klaar om in te richten bent, klikt u op **opslaan**.

    ![Bezig met opslaan van de Inrichtingsconfiguratie](common/provisioning-configuration-save.png)

Met deze bewerking wordt gestart voor de initiële synchronisatie van alle gebruikers en/of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de Azure AD-inrichtingsservice wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van rapport van de activiteit, die alle acties die worden uitgevoerd door de Azure AD-inrichtingsservice op Proxyclick beschrijft.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Connector-beperkingen

* Proxyclick vereist **e-mailberichten** en **gebruikersnaam** de dezelfde waarde hebben. Updates op een van beide kenmerken worden de andere waarde wijzigen.
* Proxyclick biedt geen ondersteuning voor groepen wordt ingericht.

## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../manage-apps/check-status-user-account-provisioning.md)

