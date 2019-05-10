---
title: 'Zelfstudie: Comeet werving Software configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en inrichting ongedaan maken-gebruikersaccounts met Comeet werving Software.
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
ms.date: 05/07/2019
ms.author: zchia
ms.openlocfilehash: e8414b9737e0ee7f847827a432dd9887931a2532
ms.sourcegitcommit: 399db0671f58c879c1a729230254f12bc4ebff59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65470390"
---
# <a name="tutorial-configure-comeet-recruiting-software-for-automatic-user-provisioning"></a>Zelfstudie: Comeet werving Software configureren voor het automatisch inrichten van gebruikers

Het doel van deze zelfstudie is ter illustratie van de stappen om te worden uitgevoerd in Comeet werven van Software en Azure Active Directory (Azure AD) naar Azure AD configureren voor automatisch inrichten en verwijdering van gebruikers en/of groepen aan Comeet werving Software.

> [!NOTE]
> Deze zelfstudie beschrijft een connector die is gebaseerd op de Provisioning-Service van Azure AD-gebruiker. Zie voor belangrijke informatie over wat deze service biedt, hoe het werkt en veelgestelde vragen [automatiseren van gebruikersinrichting en -opheffing in SaaS-toepassingen met Azure Active Directory](../manage-apps/user-provisioning.md).
>
> Deze connector is momenteel in openbare Preview. Zie voor meer informatie over de algemene Microsoft Azure gebruiksvoorwaarden voor Preview-functies, [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende vereisten hebt:

* Een Azure AD-tenant
* [Een tenant Comeet werving Software](https://www.comeet.co/)
* Een gebruikersaccount in Comeet werven van Software met beheerdersmachtigingen.

## <a name="add-comeet-recruiting-software-from-the-gallery"></a>Comeet werving Software uit de galerie toevoegen

Voordat u Software voor Comeet werving configureert voor het automatisch gebruikers inrichten met Azure AD, moet u Comeet werving Software uit de galerie met Azure AD toevoegen aan uw lijst met beheerde SaaS-toepassingen.

**Als u wilt toevoegen Comeet werving Software uit de galerie met Azure AD, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, selecteer in het navigatievenster aan de linkerkant **Azure Active Directory**.

    ![De Azure Active Directory-knop](common/select-azuread.png)

2. Ga naar **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![De blade Enterprise-toepassingen](common/enterprise-applications.png)

3. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop aan de bovenkant van het deelvenster.

    ![De knop nieuwe toepassing](common/add-new-app.png)

4. Voer in het zoekvak **Comeet werving Software**, selecteer **Comeet werving Software** in het deelvenster voor resultaten en klik vervolgens op de **toevoegen** om toe te voegen van de toepassing.

    ![Comeet Recruiting Software in de lijst met resultaten](common/search-new-app.png)

## <a name="assigning-users-to-comeet-recruiting-software"></a>Gebruikers toewijzen aan Comeet werving Software

Azure Active Directory maakt gebruik van een concept genaamd *toewijzingen* om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers, worden alleen de gebruikers en/of groepen die zijn toegewezen aan een toepassing in Azure AD gesynchroniseerd.

Voordat u configureren en inschakelen van automatische inrichten van gebruikers, moet u bepalen welke gebruikers en/of groepen in Azure AD toegang hebben tot Comeet werving Software moeten. Wanneer besloten, kunt u deze gebruikers en/of groepen toewijzen aan Comeet werving Software door de instructies hier:

* [Een gebruiker of groep toewijzen aan een enterprise-app](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-comeet-recruiting-software"></a>Belangrijke tips voor het toewijzen van gebruikers aan Comeet werving Software

* Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan Comeet werving Software voor het testen van de configuratie van de automatische gebruikersinrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

* Wanneer een gebruiker toewijzen aan Comeet werven van Software, moet u alle geldige toepassingsspecifieke rollen (indien beschikbaar) in het dialoogvenster toewijzing selecteren. Gebruikers met de **standaardtoegang** rol worden uitgesloten van het inrichten.

## <a name="configuring-automatic-user-provisioning-to-comeet-recruiting-software"></a>Automatisch gebruikers inrichten voor Comeet werving Software configureren 

Deze sectie handleidingen u via de stappen voor het configureren van de Azure AD-inrichtingsservice maken, bijwerken en uitschakelen van gebruikers en/of groepen in Comeet werven van Software op basis van gebruiker en/of toewijzingen van groepen in Azure AD.

> [!TIP]
> U kunt er ook voor kiezen om in te schakelen op SAML gebaseerde eenmalige aanmelding voor Comeet werven van Software, vindt u de instructies te volgen in de [één Comeet werving Software aanmeldings-zelfstudie](comeetrecruitingsoftware-tutorial.md). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatisch gebruikers inrichten, hoewel deze twee functies een fraaie aanvulling in elkaar.

### <a name="to-configure-automatic-user-provisioning-for-comeet-recruiting-software-in-azure-ad"></a>Het configureren van automatisch gebruikers inrichten voor Comeet werving Software in Azure AD:

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Selecteer **bedrijfstoepassingen**en selecteer vervolgens **alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Comeet Recruiting Software**.

    ![De koppeling Comeet Recruiting Software in de lijst met toepassingen](common/all-applications.png)

3. Selecteer de **Provisioning** tabblad.

    ![Inrichting](common/provisioning.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Inrichting](common/provisioning-automatic.png)

5. Onder de **beheerdersreferenties** sectie, voer de **Tenant-URL** en **geheim Token** van Comeet werving van uw Software-account, zoals beschreven in stap 6.

6. In de [Comeet werven van Software-beheerconsole](https://app.comeet.co/), gaat u naar **Comeet > Instellingen > verificatie > Microsoft Azure**, en kopieer de **geheim Token voor uw bedrijf**waarde die moet worden de **geheim Token** veld in Azure AD.

    ![Comeet werving softwarelevering](./media/comeet-recruiting-software-provisioning-tutorial/secret-token-1.png)

7. Bij het invullen van de velden die in stap 5 wordt weergegeven, klikt u op **testverbinding** om te controleren of Azure AD kunt verbinden met Comeet werving Software. Als de verbinding is mislukt, zorg ervoor dat uw account Comeet werving Software beheerdersmachtigingen heeft en probeer het opnieuw.

    ![Token](common/provisioning-testconnection-token.png)

8. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet de inrichting fout ontvangen en schakel het selectievakje in - **een e-mailmelding verzenden wanneer een foutoptreedt**.

    ![E-mailmelding](common/provisioning-notification-email.png)

9. Klik op **Opslaan**.

10. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory: gebruikers aan Comeet**.

    ![Comeet werving gebruikerstoewijzingen Software](media/comeet-recruiting-software-provisioning-tutorial/user-mappings.png)

11. Controleer de kenmerken van de gebruiker die worden gesynchroniseerd vanuit Azure AD met Comeet werving Software in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in Comeet werving Software voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

    ![Organisatie voor personeelswerving Software groepskenmerken comeet](media/comeet-recruiting-software-provisioning-tutorial/user-mapping-attributes.png)

12. Als u wilt configureren bereikfilters, raadpleegt u de volgende instructies in de [Scoping filter zelfstudie](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

13. Wijzigen zodat de Azure AD-inrichtingsservice voor Comeet werving Software de **Inrichtingsstatus** naar **op** in de **instellingen** sectie.

    ![Inrichtingsstatus ingeschakeld](common/provisioning-toggle-on.png)

14. De gebruikers en/of groepen die u wilt definiëren om in te richten op Comeet werving Software door het kiezen van de gewenste waarden in **bereik** in de **instellingen** sectie.

    ![Inrichting van bereik](common/provisioning-scope.png)

15. Wanneer u klaar om in te richten bent, klikt u op **opslaan**.

    ![Bezig met opslaan van de Inrichtingsconfiguratie](common/provisioning-configuration-save.png)

Met deze bewerking wordt gestart voor de initiële synchronisatie van alle gebruikers en/of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de Azure AD-inrichtingsservice wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van rapport van de activiteit, die alle acties die worden uitgevoerd door de Azure AD-inrichtingsservice op Comeet werving Software wordt beschreven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Connector-beperkingen

* Comeet werving Software biedt momenteel geen ondersteuning voor groepen.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../manage-apps/check-status-user-account-provisioning.md)

