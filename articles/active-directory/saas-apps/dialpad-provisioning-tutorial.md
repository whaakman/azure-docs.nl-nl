---
title: 'Zelfstudie: Kiesvenster configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en inrichting ongedaan maken-gebruikersaccounts met kiesvenster.
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
ms.author: zhchia
ms.openlocfilehash: 32e634bc089417aaa8080b30a5f77f663a3d8b33
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67611767"
---
# <a name="tutorial-configure-dialpad-for-automatic-user-provisioning"></a>Zelfstudie: Kiesvenster configureren voor het automatisch inrichten van gebruikers

Het doel van deze zelfstudie is ter illustratie van de stappen om te worden uitgevoerd in de kiesvenster en Azure Active Directory (Azure AD) naar Azure AD configureren voor automatisch inrichten en verwijdering van gebruikers en/of groepen aan kiesvenster.

> [!NOTE]
>  Deze zelfstudie beschrijft een connector die is gebaseerd op de Provisioning-Service van Azure AD-gebruiker. Zie voor belangrijke informatie over wat deze service biedt, hoe het werkt en veelgestelde vragen [automatiseren van gebruikersinrichting en -opheffing in SaaS-toepassingen met Azure Active Directory](../manage-apps/user-provisioning.md).

> Deze connector is momenteel in Preview. Zie voor meer informatie over de algemene Microsoft Azure gebruiksvoorwaarden voor Preview-functies, [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende vereisten hebt:

* Een Azure AD-tenant.
* [Een tenant kiesvenster](https://www.dialpad.com/pricing/).
* Een gebruikersaccount in kiesvenster met beheerdersmachtigingen.

## <a name="assign-users-to-dialpad"></a>Gebruikers toewijzen aan kiesvenster
Azure Active Directory maakt gebruik van een concept genaamd toewijzingen om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers, worden alleen de gebruikers en/of groepen die zijn toegewezen aan een toepassing in Azure AD gesynchroniseerd.

Voordat u configureren en inschakelen van automatische inrichten van gebruikers, moet u bepalen welke gebruikers en/of groepen in Azure AD toegang hebben tot kiesvenster moeten. Wanneer u beslist, kunt u deze gebruikers en/of groepen toewijzen aan kiesvenster door de instructies hier:
 
* [Een gebruiker of groep toewijzen aan een enterprise-app](../manage-apps/assign-user-or-group-access-portal.md) 

 ## <a name="important-tips-for-assigning-users-to-dialpad"></a>Belangrijke tips voor het toewijzen van gebruikers aan kiesvenster

 * Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan kiesvenster voor het testen van de configuratie van de automatische gebruikersinrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

* Wanneer een gebruiker aan kiesvenster toewijzen, moet u alle geldige toepassingsspecifieke rollen (indien beschikbaar) selecteren in het dialoogvenster toewijzing. Gebruikers met de functie standaardtoegang worden uitgesloten van het inrichten.

## <a name="setup-dialpad-for-provisioning"></a>Kiesvenster instellen voor het inrichten van

Voordat u kiesvenster configureert voor automatisch gebruikers inrichten met Azure AD, moet u enkele inrichting gegevens ophalen uit kiesvenster.

1. Aanmelden bij uw [kiesvenster-beheerconsole](https://dialpadbeta.com/login) en selecteer **beheerdersinstellingen**. Zorg ervoor dat **mijn bedrijf** is geselecteerd in de vervolgkeuzelijst. Navigeer naar **verificatie > API-sleutels**.

    ![Kiesvenster SCIM toevoegen](media/dialpad-provisioning-tutorial/dialpad01.png)

2. Genereer een nieuwe sleutel door te klikken op **toevoegen van een sleutel** en de eigenschappen van het token voor geheim configureren.

    ![Kiesvenster SCIM toevoegen](media/dialpad-provisioning-tutorial/dialpad02.png)

    ![Kiesvenster SCIM toevoegen](media/dialpad-provisioning-tutorial/dialpad03.png)

3. Klik op de **Klik hierop om waarde** knop voor het laatst gemaakte API-sleutel en kopieer de waarde die wordt weergegeven. Deze waarde moet worden ingevoerd in de **geheim Token** veld op het tabblad inrichten van uw toepassing kiesvenster in Azure portal. 

    ![Kiesvenster Token maken](media/dialpad-provisioning-tutorial/dialpad04.png)

## <a name="add-dialpad-from-the-gallery"></a>Kiesvenster uit de galerie toevoegen

Met het configureren van kiesvenster voor automatisch gebruikers inrichten met Azure AD, moet u kiesvenster uit de galerie met Azure AD toevoegen aan uw lijst met beheerde SaaS-toepassingen.

**Als u wilt toevoegen kiesvenster uit de galerie met Azure AD, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)** , selecteer in het navigatievenster aan de linkerkant **Azure Active Directory**.

    ![De Azure Active Directory-knop](common/select-azuread.png)

2. Ga naar **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![De blade Enterprise-toepassingen](common/enterprise-applications.png)

3. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop aan de bovenkant van het deelvenster.

    ![De knop nieuwe toepassing](common/add-new-app.png)

4. Voer in het zoekvak **kiesvenster**, selecteer **kiesvenster** in het deelvenster resultaten.
    ![Kiesvenster in de lijst met resultaten](common/search-new-app.png)

5. Navigeer naar de **URL** hieronder gemarkeerd in een afzonderlijke browser. 

    ![Kiesvenster SCIM toevoegen](media/dialpad-provisioning-tutorial/dialpad05.png)

6. Selecteer in de rechterbovenhoek **aanmelden > Gebruik kiesvenster online**.

    ![Kiesvenster SCIM toevoegen](media/dialpad-provisioning-tutorial/dialpad06.png)

7. Als Kiesvenster van een app OpenIDConnect is, kiest u aanmelden bij kiesvenster met uw Microsoft-werkaccount.

    ![Kiesvenster SCIM toevoegen](media/dialpad-provisioning-tutorial/loginpage.png)

8. Na een geslaagde verificatie door de toestemmingsprompt voor voor de pagina toestemming te accepteren. De toepassing wordt vervolgens automatisch worden toegevoegd aan uw tenant en u wordt omgeleid naar uw account kiesvenster.

    ![Kiesvenster SCIM toevoegen](media/dialpad-provisioning-tutorial/redirect.png)

 ## <a name="configure-automatic-user-provisioning-to-dialpad"></a>Automatisch gebruikers inrichten voor kiesvenster configureren

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD-inrichtingsservice als u wilt maken, bijwerken en uitschakelen van gebruikers en/of groepen in kiesvenster op basis van gebruiker en/of groep toewijzingen in Azure AD.

### <a name="to-configure-automatic-user-provisioning-for-dialpad-in-azure-ad"></a>Het configureren van automatisch gebruikers inrichten voor kiesvenster in Azure AD:

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Selecteer **bedrijfstoepassingen**en selecteer vervolgens **alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **kiesvenster**.

    ![De koppeling kiesvenster in de lijst met toepassingen](common/all-applications.png)

3. Selecteer de **Provisioning** tabblad.

    ![Inrichting](common/provisioning.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Inrichting](common/provisioning-automatic.png)

5. Onder de **beheerdersreferenties** sectie invoer `https://dialpad.com/scim` in **Tenant-URL**. De waarde die u hebt opgehaald en eerder hebt opgeslagen van kiesvenster in invoer **geheim Token**. Klik op **testverbinding** om te controleren of Azure AD kunt verbinden met kiesvenster. Als de verbinding is mislukt, zorg ervoor dat uw account kiesvenster beheerdersmachtigingen heeft en probeer het opnieuw.

    ![Tenant-URL + Token](common/provisioning-testconnection-tenanturltoken.png)

6. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet de inrichting fout ontvangen en schakel het selectievakje in - **een e-mailmelding verzenden wanneer een foutoptreedt**.

    ![E-mailmelding](common/provisioning-notification-email.png)

7. Klik op **Opslaan**.

8. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory: gebruikers aan kiesvenster**.

    ![Kiesvenster gebruikerstoewijzingen](media/dialpad-provisioning-tutorial/dialpad-user-mappings-new.png)

9. Controleer de kenmerken van de gebruiker die van Azure AD worden gesynchroniseerd naar kiesvenster in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in kiesvenster voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

    ![Kiesvenster gebruikerskenmerken](media/dialpad-provisioning-tutorial/dialpad07.png)

10. Als u wilt configureren bereikfilters, raadpleegt u de volgende instructies in de [Scoping filter zelfstudie](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

11. Om in te schakelen in de Azure AD-inrichtingsservice voor kiesvenster, wijzigen de **Inrichtingsstatus** naar **op** in de **instellingen** sectie.

    ![Inrichtingsstatus ingeschakeld](common/provisioning-toggle-on.png)

12. De gebruikers en/of groepen die u wilt definiëren voor het inrichten van kiesvenster door het kiezen van de gewenste waarden in **bereik** in de **instellingen** sectie.

    ![Inrichting van bereik](common/provisioning-scope.png)

13. Wanneer u klaar om in te richten bent, klikt u op **opslaan**.

    ![Bezig met opslaan van de Inrichtingsconfiguratie](common/provisioning-configuration-save.png)

Met deze bewerking wordt gestart voor de initiële synchronisatie van alle gebruikers en/of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de Azure AD-inrichtingsservice wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van rapport van de activiteit, die alle acties die worden uitgevoerd door de Azure AD-inrichtingsservice op kiesvenster beschrijft.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md)
##  <a name="connector-limitations"></a>Connector-beperkingen
* Kiesvenster ondersteunt geen naam van de groep vandaag nog. Dit betekent dat eventuele wijzigingen in de **displayName** van een groep in Azure AD niet worden bijgewerkt en weergegeven in kiesvenster.

## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../manage-apps/check-status-user-account-provisioning.md)
