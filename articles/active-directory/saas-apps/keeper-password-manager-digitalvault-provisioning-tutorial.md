---
title: 'Zelfstudie: Wachtwoordbeheer houder & digitale kluis configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en inrichting van gebruikersaccounts naar houder wachtwoordbeheer & digitale kluis ongedaan maken.
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
ms.date: 04/25/2019
ms.author: zchia
ms.openlocfilehash: b7b096bd1ecf0a4df37ed4de0cf618216dccc2bc
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65159463"
---
# <a name="tutorial-configure-keeper-password-manager--digital-vault-for-automatic-user-provisioning"></a>Zelfstudie: Wachtwoordbeheer houder & digitale kluis configureren voor het automatisch inrichten van gebruikers

Het doel van deze zelfstudie is ter illustratie van de stappen worden uitgevoerd in de houder wachtwoordbeheer & digitale Vault en Azure Active Directory (Azure AD) naar Azure AD configureren automatisch inrichten en de inrichting ongedaan maken van gebruikers en/of groepen houder wachtwoord Manager & digitale kluis.

> [!NOTE]
> Deze zelfstudie beschrijft een connector die is gebaseerd op de Provisioning-Service van Azure AD-gebruiker. Zie voor belangrijke informatie over wat deze service biedt, hoe het werkt en veelgestelde vragen [automatiseren van gebruikersinrichting en -opheffing in SaaS-toepassingen met Azure Active Directory](../manage-apps/user-provisioning.md).
>
> Deze connector is momenteel in openbare Preview. Zie voor meer informatie over de algemene Microsoft Azure gebruiksvoorwaarden voor Preview-functies, [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende vereisten hebt:

* Een Azure AD-tenant
* [Een tenant houder wachtwoordbeheer & digitale kluis](https://keepersecurity.com/pricing.html?t=e)
* Een gebruikersaccount in de houder wachtwoordbeheer & digitale kluis met beheerdersmachtigingen.

## <a name="add-keeper-password-manager--digital-vault-from-the-gallery"></a>Wachtwoordbeheer houder & digitale kluis uit de galerie toevoegen

Voordat u wachtwoordbeheer houder & digitale kluis configureert voor het automatisch gebruikers inrichten met Azure AD, moet u wachtwoordbeheer houder en digitale kluis uit de galerie met Azure AD toevoegen aan uw lijst met beheerde SaaS-toepassingen.

**Als u wilt toevoegen houder wachtwoordbeheer & digitale kluis uit de galerie met Azure AD, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, selecteer in het navigatievenster aan de linkerkant **Azure Active Directory**.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Ga naar **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop aan de bovenkant van het deelvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Voer in het zoekvak **houder wachtwoordbeheer & digitale kluis**, selecteer **houder wachtwoordbeheer & digitale kluis** in het deelvenster voor resultaten en klik vervolgens op de **toevoegen**om toe te voegen van de toepassing.

    ![Keeper Password Manager & Digital Vault toevoegen vanuit de galerie](common/search-new-app.png)

## <a name="assigning-users-to-keeper-password-manager--digital-vault"></a>Gebruikers toewijzen aan houder wachtwoordbeheer & digitale kluis

Azure Active Directory maakt gebruik van een concept genaamd *toewijzingen* om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers, worden alleen de gebruikers en/of groepen die zijn toegewezen aan een toepassing in Azure AD gesynchroniseerd.

Voordat u configureren en inschakelen van automatische inrichten van gebruikers, moet u bepalen welke gebruikers en/of groepen in Azure AD toegang hebben tot wachtwoordbeheer houder & digitale kluis moeten. Wanneer besloten, kunt u deze gebruikers en/of groepen toewijzen aan houder wachtwoordbeheer & digitale kluis door de instructies hier:

* [Een gebruiker of groep toewijzen aan een enterprise-app](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-keeper-password-manager--digital-vault"></a>Belangrijke tips voor het toewijzen van gebruikers aan de houder wachtwoordbeheer & digitale kluis

* Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan de houder wachtwoordbeheer & digitale Vault voor het testen van de configuratie van de automatische gebruikersinrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

* Wanneer een gebruiker voor wachtwoordbeheer houder en digitale kluis toewijzen, moet u alle geldige toepassingsspecifieke rollen (indien beschikbaar) in het dialoogvenster toewijzing selecteren. Gebruikers met de **standaardtoegang** rol worden uitgesloten van het inrichten.

## <a name="configuring-automatic-user-provisioning-to-keeper-password-manager--digital-vault"></a>Automatisch gebruikers inrichten naar houder wachtwoordbeheer & digitale kluis configureren 

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD-inrichtingsservice als u wilt maken, bijwerken en uitschakelen van gebruikers en/of groepen in houder wachtwoordbeheer & digitale kluis op basis van gebruiker en/of toewijzingen van groepen in Azure AD.

> [!TIP]
> U kunt er ook voor kiezen om in te schakelen op SAML gebaseerde eenmalige aanmelding voor wachtwoordbeheer houder & digitale kluis, vindt u de instructies te volgen in de [houder wachtwoordbeheer & digitale kluis één aanmelding zelfstudie](keeperpasswordmanager-tutorial.md). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatisch gebruikers inrichten, hoewel deze twee functies een fraaie aanvulling in elkaar.

### <a name="to-configure-automatic-user-provisioning-for-keeper-password-manager--digital-vault-in-azure-ad"></a>Het configureren van automatisch gebruikers inrichten voor wachtwoordbeheer houder & digitale kluis in Azure AD:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en selecteer **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **houder wachtwoordbeheer & digitale Kluis**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer **Keeper Password Manager & Digital Vault** in de lijst met toepassingen.

    ![De koppeling naar Keeper Password Manager & Digital Vault in de lijst met toepassingen](common/all-applications.png)

3. Selecteer de **Provisioning** tabblad.

    ![Inrichting](common/provisioning.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Inrichting](common/provisioning-automatic.png)

5. Onder de **beheerdersreferenties** sectie, voer de **Tenant-URL** en **geheim Token** van uw wachtwoordbeheer houder & digitale Vault-account, zoals beschreven in stap 6.

6. Aanmelden bij uw [houder-beheerconsole](https://keepersecurity.com/console/#login). Klik op **Admin** en selecteer een bestaand knooppunt of een nieuwe maken. Navigeer naar de **Provisioning** tabblad en selecteer **methode toevoegen**.

    ![Houder-beheerconsole](media/keeperpasswordmanagerdigitalvault-provisioning-tutorial/keeper-admin-console.png)

    Selecteer **SCIM (systeem voor meerdere domeinen identiteitsbeheer**.

    ![Houder SCIM toevoegen](media/keeperpasswordmanagerdigitalvault-provisioning-tutorial/keeper-add-scim.png)

    Klik op **maken inrichting Token**.

    ![Houder eindpunt maken](media/keeperpasswordmanagerdigitalvault-provisioning-tutorial/keeper-create-endpoint.png)

    Kopieer de waarden voor **URL** en **Token** en plak ze in **Tenant-URL** en **geheim Token** in Azure AD. Klik op **opslaan** om het inrichtingsproces installatie op houder te voltooien.

    ![Houder Token maken](media/keeperpasswordmanagerdigitalvault-provisioning-tutorial/keeper-create-token.png)

7. Bij het invullen van de velden die in stap 5 wordt weergegeven, klikt u op **testverbinding** om te controleren of Azure AD, kunt verbinding maken met de houder wachtwoordbeheer & digitale kluis. Als de verbinding is mislukt, zorg ervoor dat uw account houder wachtwoordbeheer & digitale kluis beheerdersmachtigingen heeft en probeer het opnieuw.

    ![Tenant-URL + Token](common/provisioning-testconnection-tenanturltoken.png)

8. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet de inrichting fout ontvangen en schakel het selectievakje in - **een e-mailmelding verzenden wanneer een foutoptreedt**.

    ![E-mailmelding](common/provisioning-notification-email.png)

9. Klik op **Opslaan**.

10. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory: gebruikers houder wachtwoordbeheer & digitale kluis**.

    ![Houder gebruikerstoewijzingen](media/keeperpasswordmanagerdigitalvault-provisioning-tutorial/keeper-user-mappings.png)

11. Controleer de kenmerken van de gebruiker die worden gesynchroniseerd vanuit Azure AD naar houder wachtwoordbeheer & digitale kluis in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in de houder wachtwoordbeheer & digitale kluis voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

    ![Houder gebruikerskenmerken](media/keeperpasswordmanagerdigitalvault-provisioning-tutorial/keeper-user-attributes.png)

12. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory-groepen naar houder wachtwoordbeheer & digitale kluis**.

    ![Groepstoewijzingen houder](media/keeperpasswordmanagerdigitalvault-provisioning-tutorial/keeper-group-mappings.png)

13. Bekijk de groepskenmerken die worden gesynchroniseerd vanuit Azure AD naar houder wachtwoordbeheer & digitale kluis in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de groepen in de houder wachtwoordbeheer & digitale kluis voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

    ![Groepskenmerken van houder](media/keeperpasswordmanagerdigitalvault-provisioning-tutorial/keeper-group-attributes.png)

14. Als u wilt configureren bereikfilters, raadpleegt u de volgende instructies in de [Scoping filter zelfstudie](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

15. Wijzigen zodat de Azure AD-inrichtingsservice voor wachtwoordbeheer houder & digitale kluis de **Inrichtingsstatus** naar **op** in de **instellingen** sectie.

    ![Inrichtingsstatus ingeschakeld](common/provisioning-toggle-on.png)

16. De gebruikers en/of groepen die u wilt definiëren voor het inrichten van houder wachtwoordbeheer & digitale kluis door het kiezen van de gewenste waarden in **bereik** in de **instellingen** sectie.

    ![Inrichting van bereik](common/provisioning-scope.png)

17. Wanneer u klaar om in te richten bent, klikt u op **opslaan**.

    ![Bezig met opslaan van de Inrichtingsconfiguratie](common/provisioning-configuration-save.png)

Met deze bewerking wordt gestart voor de initiële synchronisatie van alle gebruikers en/of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de Azure AD-inrichtingsservice wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van rapport van de activiteit, die alle acties die worden uitgevoerd door de Azure AD-inrichtingsservice op houder wachtwoordbeheer beschrijft & Digitale-kluis.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Connector-beperkingen

* Wachtwoordbeheer houder & digitale kluis vereist **e-mailberichten** en **gebruikersnaam** hebben dezelfde bronwaarde, zoals updates op een van beide kenmerken worden de andere waarde wijzigen.
* Wachtwoordbeheer houder & digitale kluis niet ondersteunen van de gebruiker verwijdert, alleen uitschakelen. Uitgeschakelde gebruikers wordt weergegeven als vergrendeld in de gebruikersinterface van houder Administrator-Console.

## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/atlassian-cloud-provisioning-tutorial/tutorial-general-01.png
[2]: ./media/atlassian-cloud-provisioning-tutorial/tutorial-general-02.png
[3]: ./media/atlassian-cloud-provisioning-tutorial/tutorial-general-03.png
