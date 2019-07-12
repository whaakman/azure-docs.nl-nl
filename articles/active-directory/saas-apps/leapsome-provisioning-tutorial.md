---
title: 'Zelfstudie: Leapsome configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en inrichting ongedaan maken-gebruikersaccounts met Leapsome.
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
ms.date: 06/28/2019
ms.author: jeedes
ms.openlocfilehash: e02deaa29b40e64b63d9afb471717feef78b3cee
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67672684"
---
# <a name="tutorial-configure-leapsome-for-automatic-user-provisioning"></a>Zelfstudie: Leapsome configureren voor het automatisch inrichten van gebruikers

Het doel van deze zelfstudie is ter illustratie van de stappen om te worden uitgevoerd in de Leapsome en Azure Active Directory (Azure AD) naar Azure AD configureren voor automatisch inrichten en verwijdering van gebruikers en/of groepen aan Leapsome.

> [!NOTE]
>  Deze zelfstudie beschrijft een connector die is gebaseerd op de Provisioning-Service van Azure AD-gebruiker. Zie voor belangrijke informatie over wat deze service biedt, hoe het werkt en veelgestelde vragen [automatiseren van gebruikersinrichting en -opheffing in SaaS-toepassingen met Azure Active Directory](../manage-apps/user-provisioning.md).
>
> Deze connector is momenteel in Preview. Zie voor meer informatie over de algemene Microsoft Azure gebruiksvoorwaarden voor Preview-functies, [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende vereisten hebt:

* Een Azure AD-tenant.
* Een [Leapsome](https://www.Leapsome.com/en/pricing) tenant.
* Een gebruikersaccount in Leapsome met beheerdersmachtigingen.

## <a name="assigning-users-to-leapsome"></a>Gebruikers toewijzen aan Leapsome

Azure Active Directory maakt gebruik van een concept genaamd *toewijzingen* om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers, worden alleen de gebruikers en/of groepen die zijn toegewezen aan een toepassing in Azure AD gesynchroniseerd.

Voordat u configureren en inschakelen van automatische inrichten van gebruikers, moet u bepalen welke gebruikers en/of groepen in Azure AD toegang hebben tot Leapsome moeten. Wanneer u beslist, kunt u deze gebruikers en/of groepen toewijzen aan Leapsome door de instructies hier:
* [Een gebruiker of groep toewijzen aan een enterprise-app](../manage-apps/assign-user-or-group-access-portal.md)


## <a name="important-tips-for-assigning-users-to-leapsome"></a>Belangrijke tips voor het toewijzen van gebruikers aan Leapsome

* Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan Leapsome voor het testen van de configuratie van de automatische gebruikersinrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

* Wanneer een gebruiker aan Leapsome toewijzen, moet u alle geldige toepassingsspecifieke rollen (indien beschikbaar) selecteren in het dialoogvenster toewijzing. Gebruikers met de **standaardtoegang** rol worden uitgesloten van het inrichten.


## <a name="setup-leapsome-for-provisioning"></a>Leapsome instellen voor het inrichten van

1. Aanmelden bij uw [Leapsome-beheerconsole](https://www.Leapsome.com/app/#/login). Navigeer naar **instellingen > beheerdersinstellingen**.

    ![Leapsome-beheerconsole](media/Leapsome-provisioning-tutorial/leapsome-admin-console.png)

2.  Navigeer naar **integraties > SCIM gebruikersinrichting**.

    ![Leapsome SCIM toevoegen](media/Leapsome-provisioning-tutorial/leapsome-add-scim.png)

3.  Kopieer de **SCIM verificatietoken**. Deze waarde moet worden ingevoerd in het Token geheim veld op het tabblad inrichten van uw toepassing Leapsome in Azure portal.

    ![Leapsome Token maken](media/Leapsome-provisioning-tutorial/leapsome-create-token.png)

## <a name="add-leapsome-from-the-gallery"></a>Leapsome uit de galerie toevoegen

Voordat u Leapsome configureert voor automatisch gebruikers inrichten met Azure AD, moet u Leapsome uit de galerie met Azure AD toevoegen aan uw lijst met beheerde SaaS-toepassingen.

**Als u wilt toevoegen Leapsome uit de galerie met Azure AD, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)** , selecteer in het navigatievenster aan de linkerkant **Azure Active Directory**.

    ![De Azure Active Directory-knop](common/select-azuread.png)

2. Ga naar **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![De blade Enterprise-toepassingen](common/enterprise-applications.png)

3. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop aan de bovenkant van het deelvenster.

    ![De knop nieuwe toepassing](common/add-new-app.png)

4. Voer in het zoekvak **Leapsome**, selecteer **Leapsome** in het deelvenster voor resultaten en klik vervolgens op de **toevoegen** om toe te voegen van de toepassing.

    ![Leapsome in de lijst met resultaten](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-leapsome"></a>Automatisch gebruikers inrichten voor Leapsome configureren 

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD-inrichtingsservice als u wilt maken, bijwerken en uitschakelen van gebruikers en/of groepen in Leapsome op basis van gebruiker en/of groep toewijzingen in Azure AD.

> [!TIP]
> U kunt er ook voor kiezen om in te schakelen op SAML gebaseerde eenmalige aanmelding voor Leapsome, vindt u de instructies te volgen in de [Leapsome Single sign-on zelfstudie](Leapsome-tutorial.md). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatisch gebruikers inrichten, hoewel deze twee functies een fraaie aanvulling in elkaar

### <a name="to-configure-automatic-user-provisioning-for-leapsome-in-azure-ad"></a>Het configureren van automatisch gebruikers inrichten voor Leapsome in Azure AD:

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Selecteer **bedrijfstoepassingen**en selecteer vervolgens **alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Leapsome**.

    ![De koppeling Leapsome in de lijst met toepassingen](common/all-applications.png)

3. Selecteer de **Provisioning** tabblad.

    ![Inrichting](common/provisioning.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Inrichting](common/provisioning-automatic.png)

5. Onder de **beheerdersreferenties** sectie invoer `https://www.leapsome.com/api/scim` in **Tenant-URL**. Invoer van de **SCIM-verificatietoken** waarde eerder hebt opgehaald **geheim Token**. Klik op **testverbinding** om te controleren of Azure AD kunt verbinden met Leapsome. Als de verbinding is mislukt, zorg ervoor dat uw account Leapsome beheerdersmachtigingen heeft en probeer het opnieuw.

    ![Tenant-URL + Token](common/provisioning-testconnection-tenanturltoken.png)

6. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet de inrichting fout ontvangen en schakel het selectievakje in - **een e-mailmelding verzenden wanneer een foutoptreedt**.

    ![E-mailmelding](common/provisioning-notification-email.png)

7. Klik op **Opslaan**.

8. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory: gebruikers aan Leapsome**.

    ![Leapsome gebruikerstoewijzingen](media/Leapsome-provisioning-tutorial/Leapsome-user-mappings.png)

9. Controleer de kenmerken van de gebruiker die van Azure AD worden gesynchroniseerd naar Leapsome in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in Leapsome voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

    ![Leapsome gebruikerskenmerken](media/Leapsome-provisioning-tutorial/Leapsome-user-attributes.png)

10. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory-groepen aan Leapsome**.

    ![Groepstoewijzingen Leapsome](media/Leapsome-provisioning-tutorial/Leapsome-group-mappings.png)

11. Bekijk de groepskenmerken die worden gesynchroniseerd vanuit Azure AD naar Leapsome in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de groepen in Leapsome voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

    ![Leapsome groepskenmerken](media/Leapsome-provisioning-tutorial/Leapsome-group-attributes.png)

12. Als u wilt configureren bereikfilters, raadpleegt u de volgende instructies in de [Scoping filter zelfstudie](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

13. Wijzigen zodat de Azure AD-inrichtingsservice voor Leapsome de **Inrichtingsstatus** naar **op** in de **instellingen** sectie.

    ![Inrichtingsstatus ingeschakeld](common/provisioning-toggle-on.png)

14. De gebruikers en/of groepen die u wilt definiëren voor het inrichten van Leapsome door het kiezen van de gewenste waarden in **bereik** in de **instellingen** sectie.

    ![Inrichting van bereik](common/provisioning-scope.png)

15. Wanneer u klaar om in te richten bent, klikt u op **opslaan**.

    ![Bezig met opslaan van de Inrichtingsconfiguratie](common/provisioning-configuration-save.png)

Met deze bewerking wordt gestart voor de initiële synchronisatie van alle gebruikers en/of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de Azure AD-inrichtingsservice wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van rapport van de activiteit, die alle acties die worden uitgevoerd door de Azure AD-inrichtingsservice op Leapsome beschrijft.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Connector-beperkingen

* Leapsome vereist **gebruikersnaam** uniek zijn.
* Leapsome kunt u alleen zakelijke e-mailadressen moeten worden opgeslagen.

## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../manage-apps/check-status-user-account-provisioning.md)
