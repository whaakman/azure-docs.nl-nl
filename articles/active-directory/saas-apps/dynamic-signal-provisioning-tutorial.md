---
title: 'Zelfstudie: Dynamische signaal voor het automatisch gebruikers inrichten met Azure Active Directory configureren | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en inrichting van gebruikersaccounts aan dynamische signaal ongedaan maken.
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
ms.openlocfilehash: f79bc083105f997b08f7cfa6f8e08a8f4f455455
ms.sourcegitcommit: 399db0671f58c879c1a729230254f12bc4ebff59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65470548"
---
# <a name="tutorial-configure-dynamic-signal-for-automatic-user-provisioning"></a>Zelfstudie: Dynamische signaal voor het automatisch inrichten van gebruikers configureren

Het doel van deze zelfstudie is ter illustratie van de stappen om te worden uitgevoerd in de dynamische signaal en Azure Active Directory (Azure AD) naar Azure AD configureren voor het automatisch inrichten en verwijdering van gebruikers en/of groepen aan dynamische signaal.

> [!NOTE]
> Deze zelfstudie beschrijft een connector die is gebaseerd op de Provisioning-Service van Azure AD-gebruiker. Zie voor belangrijke informatie over wat deze service biedt, hoe het werkt en veelgestelde vragen [automatiseren van gebruikersinrichting en -opheffing in SaaS-toepassingen met Azure Active Directory](../manage-apps/user-provisioning.md).
>
> Deze connector is momenteel in openbare Preview. Zie voor meer informatie over de algemene Microsoft Azure gebruiksvoorwaarden voor Preview-functies, [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende vereisten hebt:

* Een Azure AD-tenant
* [Een dynamische signaal-tenant](https://dynamicsignal.com/)
* Een gebruikersaccount in dynamische signaal met beheerdersmachtigingen.

## <a name="add-dynamic-signal-from-the-gallery"></a>Dynamische signaal uit de galerie toevoegen

Voordat u dynamische signaal voor automatisch gebruikers inrichten met Azure AD configureert, moet u dynamische signaal uit de galerie met Azure AD toevoegen aan uw lijst met beheerde SaaS-toepassingen.

**Als u wilt toevoegen dynamische signaal uit de galerie met Azure AD, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, selecteer in het navigatievenster aan de linkerkant **Azure Active Directory**.

    ![De Azure Active Directory-knop](common/select-azuread.png)

2. Ga naar **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![De blade Enterprise-toepassingen](common/enterprise-applications.png)

3. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop aan de bovenkant van het deelvenster.

    ![De knop nieuwe toepassing](common/add-new-app.png)

4. Voer in het zoekvak **dynamische signaal**, selecteer **dynamische signaal** in het deelvenster voor resultaten en klik vervolgens op de **toevoegen** om toe te voegen van de toepassing.

    ![Dynamic Signal in de lijst met resultaten](common/search-new-app.png)

## <a name="assigning-users-to-dynamic-signal"></a>Gebruikers toewijzen aan dynamische signaal

Azure Active Directory maakt gebruik van een concept genaamd *toewijzingen* om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers, worden alleen de gebruikers en/of groepen die zijn toegewezen aan een toepassing in Azure AD gesynchroniseerd.

Voordat u configureren en inschakelen van automatische inrichten van gebruikers, moet u bepalen welke gebruikers en/of groepen in Azure AD toegang hebben tot dynamische signaal moeten. Wanneer u beslist, kunt u deze gebruikers en/of groepen toewijzen aan dynamische signaal door de instructies hier:

* [Een gebruiker of groep toewijzen aan een enterprise-app](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-dynamic-signal"></a>Belangrijke tips voor het toewijzen van gebruikers aan dynamische signaal

* Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan dynamische signaal voor het testen van de configuratie van de automatische gebruikersinrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

* Wanneer een gebruiker toewijzen aan dynamische signaal, moet u alle geldige toepassingsspecifieke rollen (indien beschikbaar) selecteren in het dialoogvenster toewijzing. Gebruikers met de **standaardtoegang** rol worden uitgesloten van het inrichten.

## <a name="configuring-automatic-user-provisioning-to-dynamic-signal"></a>Automatisch gebruikers inrichten voor dynamische signaal configureren 

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD-inrichtingsservice als u wilt maken, bijwerken en uitschakelen van gebruikers en/of groepen in dynamische signaal op basis van gebruiker en/of toewijzingen van groepen in Azure AD.

> [!TIP]
> U kunt er ook voor kiezen om in te schakelen op SAML gebaseerde eenmalige aanmelding voor dynamische signaal, vindt u de instructies te volgen in de [één dynamische signaal aanmeldings-zelfstudie](dynamicsignal-tutorial.md). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatisch gebruikers inrichten, hoewel deze twee functies een fraaie aanvulling in elkaar.

### <a name="to-configure-automatic-user-provisioning-for-dynamic-signal-in-azure-ad"></a>Het configureren van automatisch gebruikers inrichten voor dynamische signaal in Azure AD:

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Selecteer **bedrijfstoepassingen**en selecteer vervolgens **alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer **Dynamic Signal** in de lijst met toepassingen.

    ![De koppeling naar Dynamic Signal in de lijst met toepassingen](common/all-applications.png)

3. Selecteer de **Provisioning** tabblad.

    ![Inrichting](common/provisioning.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Inrichting](common/provisioning-automatic.png)

5. Onder de **beheerdersreferenties** sectie, voer de **Tenant-URL** en **geheim Token** van uw dynamische signaal account zoals beschreven in stap 6.

6. Navigeer in de beheerconsole dynamische signaal naar **Admin > Geavanceerd > API**.

    ![Dynamische signaal inrichten](./media/dynamic-signal-provisioning-tutorial/secret-token-1.png)

    Kopieer de **SCIM API URL** naar **Tenant-URL**. Klik op **nieuw Token genereren** voor het genereren van een **Bearer Token** en kopieer de waarde voor **geheim Token**.

    ![Dynamische signaal inrichten](./media/dynamic-signal-provisioning-tutorial/secret-token-2.png)

7. Bij het invullen van de velden die in stap 5 wordt weergegeven, klikt u op **testverbinding** om te controleren of Azure AD kunt verbinden met dynamische signaal. Als de verbinding is mislukt, zorg ervoor dat uw account dynamische signaal beheerdersmachtigingen heeft en probeer het opnieuw.

    ![Tenant-URL + Token](common/provisioning-testconnection-tenanturltoken.png)

8. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet de inrichting fout ontvangen en schakel het selectievakje in - **een e-mailmelding verzenden wanneer een foutoptreedt**.

    ![E-mailmelding](common/provisioning-notification-email.png)

9. Klik op **Opslaan**.

10. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory: gebruikers aan dynamische signaal**.

    ![Dynamische signaal gebruikerstoewijzingen](media/dynamic-signal-provisioning-tutorial/user-mappings.png)

11. Controleer de kenmerken van de gebruiker die worden gesynchroniseerd vanuit Azure AD aan dynamische signaal in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in dynamische signaal voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

    ![Dynamische signaal gebruikerskenmerken](media/dynamic-signal-provisioning-tutorial/user-mapping-attributes.png)

12. Als u wilt configureren bereikfilters, raadpleegt u de volgende instructies in de [Scoping filter zelfstudie](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

13. Om in te schakelen in de Azure AD-inrichtingsservice voor dynamische signaal, wijzigen de **Inrichtingsstatus** naar **op** in de **instellingen** sectie.

    ![Inrichtingsstatus ingeschakeld](common/provisioning-toggle-on.png)

14. De gebruikers en/of groepen die u wilt definiëren om in te richten op dynamische signaal door het kiezen van de gewenste waarden in **bereik** in de **instellingen** sectie.

    ![Inrichting van bereik](common/provisioning-scope.png)

15. Wanneer u klaar om in te richten bent, klikt u op **opslaan**.

    ![Bezig met opslaan van de Inrichtingsconfiguratie](common/provisioning-configuration-save.png)

Met deze bewerking wordt gestart voor de initiële synchronisatie van alle gebruikers en/of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de Azure AD-inrichtingsservice wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van rapport van de activiteit, die alle acties die worden uitgevoerd door de Azure AD-inrichtingsservice op dynamische signaal beschrijft.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Connector-beperkingen

* Dynamische signaal biedt geen ondersteuning voor permanente gebruiker verwijderd uit Azure AD. Als u wilt verwijderen van een gebruiker permanent in dynamische signaal, heeft de bewerking om te worden gemaakt via de beheerdersconsole dynamische signaal gebruikersinterface. 
* Dynamische signaal biedt momenteel geen ondersteuning voor groepen.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../manage-apps/check-status-user-account-provisioning.md)

