---
title: 'Zelfstudie: Smartsheet configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en inrichting ongedaan maken-gebruikersaccounts met Smartsheet.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: na
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2019
ms.author: jeedes
ms.openlocfilehash: f0ca2dfa90e1312db664962e7ffbe6b3f4dd96e1
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67670940"
---
# <a name="tutorial-configure-smartsheet-for-automatic-user-provisioning"></a>Zelfstudie: Smartsheet voor het automatisch inrichten van gebruikers configureren

Het doel van deze zelfstudie is ter illustratie van de stappen worden uitgevoerd in Smartsheet en Azure Active Directory (Azure AD) naar Azure AD configureren voor het automatisch inrichten en de inrichting ongedaan maken van gebruikers en/of groepen met Smartsheet.

> [!NOTE]
> Deze zelfstudie beschrijft een connector die is gebaseerd op de Provisioning-Service van Azure AD-gebruiker. Zie voor belangrijke informatie over wat deze service biedt, hoe het werkt en veelgestelde vragen [automatiseren van gebruikersinrichting en -opheffing in SaaS-toepassingen met Azure Active Directory](../manage-apps/user-provisioning.md).
>
> Deze connector is momenteel in openbare Preview. Zie voor meer informatie over de algemene Microsoft Azure gebruiksvoorwaarden voor Preview-functies, [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende vereisten hebt:

* Een Azure AD-tenant
* [Een Smartsheet-tenant](https://www.smartsheet.com/pricing)
* Een gebruikersaccount op een Smartsheet Enterprise of Premier Enterprise-plan met de systeembeheerder machtigingen.

## <a name="assign-users-to-smartsheet"></a>Gebruikers toewijzen aan een Smartsheet

Azure Active Directory maakt gebruik van een concept genaamd *toewijzingen* om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers, worden alleen de gebruikers en/of groepen die zijn toegewezen aan een toepassing in Azure AD gesynchroniseerd.

Voordat u configureren en inschakelen van automatische inrichten van gebruikers, moet u bepalen welke gebruikers en/of groepen in Azure AD toegang hebben tot Smartsheet moeten. Wanneer u beslist, kunt u deze gebruikers en/of groepen toewijzen aan Smartsheet door de instructies hier:

* [Een gebruiker of groep toewijzen aan een enterprise-app](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-smartsheet"></a>Belangrijke tips voor het toewijzen van gebruikers met Smartsheet

* Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan de Smartsheet voor het testen van de configuratie van de automatische gebruikersinrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

* Bij het toewijzen van een gebruiker met Smartsheet, moet u een geldige toepassingsspecifieke-rol (indien beschikbaar) in het dialoogvenster toewijzing selecteren. Gebruikers met de **standaardtoegang** rol worden uitgesloten van het inrichten.

* Om ervoor te zorgen pariteit in roltoewijzingen gebruiker tussen Smartsheet en Azure AD, is het aanbevolen gebruikmaken van de dezelfde roltoewijzingen ingevuld in de volledige lijst met een Smartsheet-gebruikers. Als u wilt deze lijst met gebruikers uit Smartsheet ophalen, gaat u naar **accountbeheerder > Gebruikersbeheer > meer acties > downloaden van de gebruikerslijst (csv)** .

* Smartsheet vereist voor toegang tot bepaalde functies in de app moet een gebruiker meerdere rollen hebben. Voor meer informatie over de gebruikerstypen en machtigingen in Smartsheet, gaat u naar [gebruikerstypen en machtigingen](https://help.smartsheet.com/learning-track/shared-users/user-types-and-permissions).

*  Als een gebruiker meerdere rollen toegewezen in Smartsheet, heeft u **moet** ervoor te zorgen dat deze roltoewijzingen in Azure AD om te voorkomen dat een scenario waarbij gebruikers kunnen geen toegang meer tot Smartsheet objecten definitief worden gerepliceerd. Elke unieke rol in Smartsheet **moet** worden toegewezen aan een andere groep in Azure AD. De gebruiker **moet** en vervolgens worden toegevoegd aan elk van de groepen die overeenkomt met de gewenste rollen. 

## <a name="set-up-smartsheet-for-provisioning"></a>Smartsheet instellen voor het inrichten van

Voordat u Smartsheet configureert voor automatisch gebruikers inrichten met Azure AD, moet u SCIM inrichting op Smartsheet inschakelen.

1. Meld u aan als een **SysAdmin** in de **[Smartsheet portal](https://app.smartsheet.com/b/home)** en navigeer naar **accountbeheerder**.

    ![Beheerder van het Smartsheet-Account](media/smartsheet-provisioning-tutorial/smartsheet-accountadmin.png)

2. Ga naar **beveiligingsmechanismen > gebruiker automatisch ingericht > bewerken**.

    ![Maatregelen voor Smartsheet](media/smartsheet-provisioning-tutorial/smartsheet-securitycontrols.png)

3. Voeg en valideren van de e-maildomeinen voor de gebruikers die u van plan om in te richten van Azure AD met Smartsheet bent. Kies **niet ingeschakeld** om ervoor te zorgen dat alle inrichting acties alleen afkomstig zijn uit Azure AD, en ook voor zorgen dat de lijst van uw Smartsheet-gebruiker is gesynchroniseerd met Azure AD-toewijzingen.

    ![Smartsheet gebruikers inrichten](media/smartsheet-provisioning-tutorial/smartsheet-userprovisioning.png)

4. Als validatie voltooid is, moet u het domein te activeren. 

    ![Smartsheet domein activeren](media/smartsheet-provisioning-tutorial/smartsheet-activatedomain.png)

5. Genereren van de **geheim Token** vereist voor het configureren van automatisch gebruikers inrichten met Azure AD door te navigeren naar **Apps en integraties**.

    ![Smartsheet installeren](media/smartsheet-provisioning-tutorial/Smartsheet05.png)

6. Kies **API-toegang**. Klik op **genereren nieuw toegangstoken**.

    ![Smartsheet installeren](media/smartsheet-provisioning-tutorial/Smartsheet06.png)

7. De naam van de API-toegangstoken definiëren. Klik op **OK**.

    ![Smartsheet installeren](media/smartsheet-provisioning-tutorial/Smartsheet07.png)

8. De API-toegangstoken kopiëren en opslaan als dit is de enige keer dat u deze kunt bekijken. Dit is vereist de **geheim Token** veld in Azure AD.

    ![Smartsheet-token](media/smartsheet-provisioning-tutorial/Smartsheet08.png)

## <a name="add-smartsheet-from-the-gallery"></a>Smartsheet uit de galerie toevoegen

Smartsheet configureren voor automatisch gebruikers inrichten met Azure AD, moet u Smartsheet uit de galerie met Azure AD toevoegen aan uw lijst met beheerde SaaS-toepassingen.

1. In de  **[Azure-portal](https://portal.azure.com)** , selecteer in het navigatievenster aan de linkerkant **Azure Active Directory**.

    ![De Azure Active Directory-knop](common/select-azuread.png)

2. Ga naar **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![De blade Enterprise-toepassingen](common/enterprise-applications.png)

3. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop aan de bovenkant van het deelvenster.

    ![De knop nieuwe toepassing](common/add-new-app.png)

4. Voer in het zoekvak **Smartsheet**, selecteer **Smartsheet** in het deelvenster resultaten. 

    ![Smartsheet in de lijst met resultaten](common/search-new-app.png)

5. Selecteer de **Meld u aan voor Smartsheet** knop waarmee u wordt omgeleid naar de Smartsheet-aanmeldingspagina. 

    ![Smartsheet OIDC toevoegen](media/smartsheet-provisioning-tutorial/smartsheet-OIDC-add.png)

6. Smartsheet is een app OpenIDConnect, kiest u aanmelden bij Smartsheet met uw Microsoft-werkaccount.

    ![Smartsheet OIDC-aanmelding](media/smartsheet-provisioning-tutorial/smartsheet-OIDC-login.png)

7. Na een geslaagde verificatie door de toestemmingsprompt voor voor de pagina toestemming te accepteren. De toepassing wordt vervolgens automatisch worden toegevoegd aan uw tenant en u wordt omgeleid naar uw Smartsheet-account.

    ![Smartsheet OIDc toestemming](media/smartsheet-provisioning-tutorial/smartsheet-OIDC-consent.png)

## <a name="configure-automatic-user-provisioning-to-smartsheet"></a>Automatisch gebruikers inrichten met Smartsheet configureren 

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD-inrichtingsservice als u wilt maken, bijwerken en uitschakelen van gebruikers en/of groepen in Smartsheet op basis van gebruiker en/of groep toewijzingen in Azure AD.

### <a name="to-configure-automatic-user-provisioning-for-smartsheet-in-azure-ad"></a>Het configureren van automatisch gebruikers inrichten voor Smartsheet in Azure AD:

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Selecteer **bedrijfstoepassingen**en selecteer vervolgens **alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Smartsheet**.

    ![De Smartsheet-koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer de **Provisioning** tabblad.

    ![Inrichting](common/provisioning.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Inrichting](common/provisioning-automatic.png)

5. Onder de **beheerdersreferenties** sectie invoer `https://scim.smartsheet.com/v2/` in **Tenant-URL**. De waarde die u hebt opgehaald en eerder hebt opgeslagen vanuit Smartsheet in invoer **geheim Token**. Klik op **testverbinding** om te controleren of Azure AD verbinding kunt maken met Smartsheet. Als de verbinding is mislukt, zorg ervoor dat uw Smartsheet-account SysAdmin-machtigingen heeft en probeer het opnieuw.

    ![Token](common/provisioning-testconnection-tenanturltoken.png)

6. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet de inrichting fout ontvangen en schakel het selectievakje in - **een e-mailmelding verzenden wanneer een foutoptreedt**.

    ![E-mailmelding](common/provisioning-notification-email.png)

7. Klik op **Opslaan**.

8. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory: gebruikers met Smartsheet**.

    ![Smartsheet gebruikerstoewijzingen](media/smartsheet-provisioning-tutorial/smartsheet-user-mappings.png)

9. Controleer de kenmerken van de gebruiker die worden gesynchroniseerd vanuit Azure AD met Smartsheet in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in Smartsheet voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

    ![Gebruikerskenmerken Smartsheet](media/smartsheet-provisioning-tutorial/smartsheet-user-attributes.png)

10. Als u wilt configureren bereikfilters, raadpleegt u de volgende instructies in de [Scoping filter zelfstudie](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

11. Wijzigen zodat de Azure AD-inrichtingsservice voor Smartsheet de **Inrichtingsstatus** naar **op** in de **instellingen** sectie.

    ![Inrichtingsstatus ingeschakeld](common/provisioning-toggle-on.png)

12. De gebruikers en/of groepen die u wilt inrichten met Smartsheet definiëren door het kiezen van de gewenste waarden in **bereik** in de **instellingen** sectie.

    ![Inrichting van bereik](common/provisioning-scope.png)

13. Wanneer u klaar om in te richten bent, klikt u op **opslaan**.

    ![Bezig met opslaan van de Inrichtingsconfiguratie](common/provisioning-configuration-save.png)

Met deze bewerking wordt gestart voor de initiële synchronisatie van alle gebruikers en/of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de Azure AD-inrichtingsservice wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van rapport van de activiteit, die alle acties die worden uitgevoerd door de Azure AD-inrichtingsservice op Smartsheet beschrijft.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Connector-beperkingen

* Smartsheet biedt geen ondersteuning voor voorlopig verwijderen. Wanneer een gebruiker **active** kenmerk is ingesteld op False, Smartsheet wordt de gebruiker permanent verwijderd.

## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../manage-apps/check-status-user-account-provisioning.md)
