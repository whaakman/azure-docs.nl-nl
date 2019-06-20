---
title: 'Zelfstudie: Zoomen configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory automatisch inrichten en ongedaan maken inrichting accounts van gebruikers om te zoomen.
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
ms.openlocfilehash: ca79b3901e3933e25c5be92673e0c5bcc464782d
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67167854"
---
# <a name="tutorial-configure-zoom-for-automatic-user-provisioning"></a>Zelfstudie: Inzoomen op voor het automatisch inrichten van gebruikers configureren

Het doel van deze zelfstudie is ter illustratie van de stappen worden uitgevoerd in/uitzoomen en Azure Active Directory (Azure AD) naar Azure AD configureren voor automatisch inrichten en verwijderen, gebruikers en/of groepen om te zoomen.

> [!NOTE]
> Deze zelfstudie beschrijft een connector die is gebaseerd op de Provisioning-Service van Azure AD-gebruiker. Zie voor belangrijke informatie over wat deze service biedt, hoe het werkt en veelgestelde vragen [automatiseren van gebruikersinrichting en -opheffing in SaaS-toepassingen met Azure Active Directory](../manage-apps/user-provisioning.md).
>
> Deze connector is momenteel in openbare Preview. Zie voor meer informatie over de algemene Microsoft Azure gebruiksvoorwaarden voor Preview-functies, [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende vereisten hebt:

* Een Azure AD-tenant
* [Een tenant zoomen](https://zoom.us/pricing)
* Een gebruikersaccount in zoomen met beheerdersmachtigingen

## <a name="add-zoom-from-the-gallery"></a>Inzoomen op uit de galerie toevoegen

Voordat u kunt inzoomen op configureert voor het automatisch gebruikers inrichten met Azure AD, die u wilt inzoomen op uit de galerie met Azure AD toevoegen aan uw lijst met beheerde SaaS-toepassingen.

**Als u wilt inzoomen op uit de galerie met Azure AD toevoegen, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)** , selecteer in het navigatievenster aan de linkerkant **Azure Active Directory**.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Ga naar **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop aan de bovenkant van het deelvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Voer in het zoekvak **zoomen**, selecteer **zoomen** in het deelvenster voor resultaten en klik vervolgens op de **toevoegen** om toe te voegen van de toepassing.

    ![Zoom in de lijst met resultaten](common/search-new-app.png)

## <a name="assign-users-to-zoom"></a>Gebruikers toewijzen om te zoomen

Azure Active Directory maakt gebruik van een concept genaamd *toewijzingen* om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers, worden alleen de gebruikers en/of groepen die zijn toegewezen aan een toepassing in Azure AD gesynchroniseerd.

Voordat u configureren en inschakelen van automatisch inrichten van gebruikers, moet u bepalen welke gebruikers en/of groepen in Azure AD toegang hebben om te zoomen moeten. Als besloten, kunt u deze gebruikers en/of groepen toewijzen om te zoomen door de instructies hier:

* [Een gebruiker of groep toewijzen aan een enterprise-app](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-zoom"></a>Belangrijke tips voor het toewijzen van gebruikers om te zoomen

* Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen om te zoomen voor het testen van de configuratie van de automatische gebruikersinrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

* Bij het toewijzen van een gebruiker om te zoomen, moet u een geldige rol van toepassingsspecifieke selecteren (indien beschikbaar) in het dialoogvenster toewijzing. Gebruikers met de **standaardtoegang** rol worden uitgesloten van het inrichten.

## <a name="configure-automatic-user-provisioning-to-zoom"></a>Automatisch gebruikers inrichten om te zoomen configureren 

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD-inrichtingsservice als u wilt maken, bijwerken en uitschakelen van gebruikers of groepen in Inzoomen op basis van gebruiker en/of groep toewijzingen in Azure AD.

> [!TIP]
> U kunt er ook voor kiezen om in te schakelen op SAML gebaseerde eenmalige aanmelding voor inzoomen, vindt u de instructies te volgen in de [één zoomen aanmeldings-zelfstudie](zoom-tutorial.md). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatisch gebruikers inrichten, hoewel deze twee functies een fraaie aanvulling in elkaar.

### <a name="configure-automatic-user-provisioning-for-zoom-in-azure-ad"></a>Automatisch gebruikers inrichten voor inzoomen in Azure AD configureren

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Selecteer **bedrijfstoepassingen**en selecteer vervolgens **alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **zoomen**.

    ![De koppeling Zoom in de lijst met toepassingen](common/all-applications.png)

3. Selecteer de **Provisioning** tabblad.

    ![Inrichting](common/provisioning.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Inrichting](common/provisioning-automatic.png)

5. Onder de **beheerdersreferenties** sectie, voer `https://api.zoom.us/scim` in **Tenant-URL**. Om op te halen de **geheim Token** van uw account inzoomen, volgt u de procedure zoals beschreven in stap 6.

6. Aanmelden bij uw [Zoom-beheerconsole](https://zoom.us/signin). Navigeer naar **Geavanceerd > Zoomen voor ontwikkelaars** in het navigatiedeelvenster links.

    ![Zoomen integraties](media/zoom-provisioning-tutorial/zoom01.png)

    Navigeer naar **beheren** in de rechterbovenhoek van de pagina. 

    ![Zoomen installeren](media/zoom-provisioning-tutorial/zoom02.png)

    Navigeer naar de gemaakte Azure AD-app. 
    
    ![Inzoomen op App](media/zoom-provisioning-tutorial/zoom03.png)

    Selecteer **App referenties** in het navigatiedeelvenster links.

    ![Inzoomen op App](media/zoom-provisioning-tutorial/zoom04.png)

    Ophalen van de JWT-Token waarde hieronder wordt weergegeven en voer deze in de **geheim Token** veld in Azure AD. Als u een nieuw token voor niet-verlopen, moet u de vervaldatum configureren die wordt automatisch een nieuw token genereren. 

    ![Zoomen installeren](media/zoom-provisioning-tutorial/zoom05.png)

7. Bij het invullen van de velden die in stap 5 wordt weergegeven, klikt u op **testverbinding** Zorg ervoor dat Azure AD verbinding kan maken om te zoomen. Als de verbinding is mislukt, zorg ervoor dat uw account zoomen beheerdersmachtigingen heeft en probeer het opnieuw.

    ![Token](common/provisioning-testconnection-tenanturltoken.png)

8. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet de inrichting fout ontvangen en schakel het selectievakje in - **een e-mailmelding verzenden wanneer een foutoptreedt**.

    ![E-mailmelding](common/provisioning-notification-email.png)

9. Klik op **Opslaan**.

10. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory-gebruikers om te zoomen**.

    ![Zoomen gebruikerstoewijzingen](media/zoom-provisioning-tutorial/zoom-user-mapping.png)

11. Controleer de kenmerken van de gebruiker die worden gesynchroniseerd vanuit Azure AD om te zoomen in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in zoomen voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.
    
     ![Zoomen gebruikerstoewijzingen](media/zoom-provisioning-tutorial/zoom-user-attributes.png)

12. Als u wilt configureren bereikfilters, raadpleegt u de volgende instructies in de [Scoping filter zelfstudie](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

13. Wijzigen zodat de Azure AD-inrichtingsservice voor inzoomen op de **Inrichtingsstatus** naar **op** in de **instellingen** sectie.
    
    ![Inrichtingsstatus ingeschakeld](common/provisioning-toggle-on.png)

14. Het definiëren van de gebruikers en/of groepen die u wilt inrichten om te zoomen door het kiezen van de gewenste waarden in **bereik** in de **instellingen** sectie.

    ![Inrichting van bereik](common/provisioning-scope.png)

15. Wanneer u klaar om in te richten bent, klikt u op **opslaan**.

    ![Bezig met opslaan van de Inrichtingsconfiguratie](common/provisioning-configuration-save.png)

Met deze bewerking wordt gestart voor de initiële synchronisatie van alle gebruikers en/of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de Azure AD-inrichtingsservice wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van activiteitenrapport, waarin wordt beschreven alle acties die worden uitgevoerd door de Azure AD-inrichtingsservice op inzoomen.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Connector-beperkingen

* Zoomen biedt geen ondersteuning voor groepen wordt ingericht.

## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../manage-apps/check-status-user-account-provisioning.md)
