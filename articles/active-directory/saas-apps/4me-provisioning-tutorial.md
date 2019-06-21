---
title: 'Zelfstudie: 4me voor het automatisch gebruikers inrichten met Azure Active Directory configureren | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en inrichting ongedaan maken-gebruikersaccounts met 4me.
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
ms.openlocfilehash: 0cfd760eb9d631bf6ab098afef0ef9b66c92cfa6
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67168153"
---
# <a name="tutorial-configure-4me-for-automatic-user-provisioning"></a>Zelfstudie: 4me voor het automatisch inrichten van gebruikers configureren

Het doel van deze zelfstudie is ter illustratie van de stappen worden uitgevoerd in Azure AD configureren voor het automatisch inrichten en verwijdering van gebruikers en/of groepen aan 4me 4me en Azure Active Directory (Azure AD).

> [!NOTE]
> Deze zelfstudie beschrijft een connector die is gebaseerd op de Provisioning-Service van Azure AD-gebruiker. Zie voor belangrijke informatie over wat deze service biedt, hoe het werkt en veelgestelde vragen [automatiseren van gebruikersinrichting en -opheffing in SaaS-toepassingen met Azure Active Directory](../manage-apps/user-provisioning.md).
>
> Deze connector is momenteel in openbare Preview. Zie voor meer informatie over de algemene Microsoft Azure gebruiksvoorwaarden voor Preview-functies, [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende vereisten hebt:

* Een Azure AD-tenant
* [Een tenant 4me](https://www.4me.com/trial/)
* Een gebruikersaccount in 4me met beheerdersmachtigingen.

## <a name="add-4me-from-the-gallery"></a>4me uit de galerie toevoegen

Voordat u 4me voor het automatisch gebruikers inrichten met Azure AD configureert, moet u 4me uit de galerie met Azure AD toevoegen aan uw lijst met beheerde SaaS-toepassingen.

**Als u wilt toevoegen 4me uit de galerie met Azure AD, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)** , selecteer in het navigatievenster aan de linkerkant **Azure Active Directory**.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Ga naar **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop aan de bovenkant van het deelvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Voer in het zoekvak **4me**, selecteer **4me** in het deelvenster voor resultaten en klik vervolgens op de **toevoegen** om toe te voegen van de toepassing.

    ![4me in de lijst met resultaten](common/search-new-app.png)

## <a name="assigning-users-to-4me"></a>Gebruikers toewijzen aan 4me

Azure Active Directory maakt gebruik van een concept genaamd *toewijzingen* om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers, worden alleen de gebruikers en/of groepen die zijn toegewezen aan een toepassing in Azure AD gesynchroniseerd.

Voordat u configureren en inschakelen van automatische inrichten van gebruikers, moet u bepalen welke gebruikers en/of groepen in Azure AD toegang hebben tot 4me moeten. Wanneer u beslist, kunt u deze gebruikers en/of groepen toewijzen aan 4me door de instructies hier:

* [Een gebruiker of groep toewijzen aan een enterprise-app](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-4me"></a>Belangrijke tips voor het toewijzen van gebruikers aan 4me

* Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan 4me voor het testen van de configuratie van de automatische gebruikersinrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

* Wanneer een gebruiker aan 4me toewijzen, moet u alle geldige toepassingsspecifieke rollen (indien beschikbaar) selecteren in het dialoogvenster toewijzing. Gebruikers met de **standaardtoegang** rol worden uitgesloten van het inrichten.

## <a name="configuring-automatic-user-provisioning-to-4me"></a>Automatisch gebruikers inrichten voor 4me configureren 

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD-inrichtingsservice als u wilt maken, bijwerken en uitschakelen van gebruikers en/of groepen in 4me op basis van gebruiker en/of groep toewijzingen in Azure AD.

> [!TIP]
> U kunt er ook voor kiezen om in te schakelen op SAML gebaseerde eenmalige aanmelding voor 4me, op. instructies hiervoor vindt de [één 4me aanmeldings-zelfstudie](4me-tutorial.md). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatisch gebruikers inrichten, hoewel deze twee functies een fraaie aanvulling in elkaar.

### <a name="to-configure-automatic-user-provisioning-for-4me-in-azure-ad"></a>Het configureren van automatisch gebruikers inrichten voor 4me in Azure AD:

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Selecteer **bedrijfstoepassingen**en selecteer vervolgens **alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer **4me** in de lijst met toepassingen.

    ![De 4me-koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer de **Provisioning** tabblad.

    ![Inrichting](common/provisioning.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Inrichting](common/provisioning-automatic.png)

5. Om op te halen de **Tenant-URL** en **geheim Token** van uw account 4me, volgt u de procedure zoals beschreven in stap 6.

6. Aanmelden bij uw 4me-beheerconsole. Navigeer naar **instellingen**.

    ![Instellingen voor 4me](media/4me-provisioning-tutorial/4me01.png)

    Typ in **apps** in de zoekbalk.

    ![4me apps](media/4me-provisioning-tutorial/4me02.png)

    Open de **SCIM** vervolgkeuzelijst om het geheim-Token en het eindpunt SCIM te halen.

    ![4me SCIM](media/4me-provisioning-tutorial/4me03.png)

7. Bij het invullen van de velden die in stap 5 wordt weergegeven, klikt u op **testverbinding** om te controleren of Azure AD kunt verbinden met 4me. Als de verbinding is mislukt, zorg ervoor dat uw account 4me beheerdersmachtigingen heeft en probeer het opnieuw.

    ![Token](common/provisioning-testconnection-tenanturltoken.png)

8. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet de inrichting fout ontvangen en schakel het selectievakje in - **een e-mailmelding verzenden wanneer een foutoptreedt**.

    ![E-mailmelding](common/provisioning-notification-email.png)

9. Klik op **Opslaan**.

10. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory: gebruikers aan 4me**.

    ![4me gebruikerstoewijzingen](media/4me-provisioning-tutorial/4me-user-mapping.png)
    
11. Controleer de kenmerken van de gebruiker die van Azure AD worden gesynchroniseerd naar 4me in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in 4me voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

    ![4me gebruikerstoewijzingen](media/4me-provisioning-tutorial/4me-user-attributes.png)
    
12. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory-groepen aan 4me**.

    ![4me gebruikerstoewijzingen](media/4me-provisioning-tutorial/4me-group-mapping.png)
    
13. Bekijk de groepskenmerken die worden gesynchroniseerd vanuit Azure AD naar 4me in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de groepen in 4me voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

    ![Groepstoewijzingen 4me](media/4me-provisioning-tutorial/4me-group-attribute.png)

14. Als u wilt configureren bereikfilters, raadpleegt u de volgende instructies in de [Scoping filter zelfstudie](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

15. Wijzigen zodat de Azure AD-inrichtingsservice voor 4me de **Inrichtingsstatus** naar **op** in de **instellingen** sectie.

    ![Inrichtingsstatus ingeschakeld](common/provisioning-toggle-on.png)

16. De gebruikers en/of groepen die u wilt definiëren voor het inrichten van 4me door het kiezen van de gewenste waarden in **bereik** in de **instellingen** sectie.

    ![Inrichting van bereik](common/provisioning-scope.png)

17. Wanneer u klaar om in te richten bent, klikt u op **opslaan**.

    ![Bezig met opslaan van de Inrichtingsconfiguratie](common/provisioning-configuration-save.png)

Met deze bewerking wordt gestart voor de initiële synchronisatie van alle gebruikers en/of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de Azure AD-inrichtingsservice wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van rapport van de activiteit, die alle acties die worden uitgevoerd door de Azure AD-inrichtingsservice op 4me beschrijft.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Connector-beperkingen

* 4me heeft verschillende SCIM eindpunt-URL's voor testomgevingen als productieomgevingen. Eindigt op de vorige **.qa** tijdens de laatste eindigt met **.com**
* 4me gegenereerd geheim Tokens hebben een vervaldatum van een maand van generatie.
* biedt geen ondersteuning voor 4me **verwijderen** bewerkingen

## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../manage-apps/check-status-user-account-provisioning.md)