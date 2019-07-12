---
title: 'Zelfstudie: Federatieve Directory configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en inrichting van gebruikersaccounts met federatieve Directory ongedaan maken.
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
ms.openlocfilehash: 19f5690a6852161abce2565a8c4a52ce86ff5187
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67840587"
---
# <a name="tutorial-configure-federated-directory-for-automatic-user-provisioning"></a>Zelfstudie: Federatieve Directory configureren voor het automatisch inrichten van gebruikers

Het doel van deze zelfstudie is ter illustratie van de stappen worden uitgevoerd in de federatieve Directory en Azure Active Directory (Azure AD) naar Azure AD configureren automatisch inrichten en de inrichting ongedaan maken van gebruikers en/of groepen die moeten worden Gefedereerd Directory.

> [!NOTE]
>  Deze zelfstudie beschrijft een connector die is gebaseerd op de Provisioning-Service van Azure AD-gebruiker. Zie voor belangrijke informatie over wat deze service biedt, hoe het werkt en veelgestelde vragen [automatiseren van gebruikersinrichting en -opheffing in SaaS-toepassingen met Azure Active Directory](../manage-apps/user-provisioning.md).
>
> Deze connector is momenteel in openbare Preview. Zie voor meer informatie over de algemene Microsoft Azure gebruiksvoorwaarden voor Preview-functies, [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende vereisten hebt:

* Een Azure AD-tenant.
* [Een Directory is Gefedereerd](https://www.federated.directory/pricing).
* Een gebruikersaccount in de Directory Gefedereerd met beheerdersmachtigingen.

## <a name="assign-users-to-federated-directory"></a>Gebruikers toewijzen aan federatieve Directory
Azure Active Directory maakt gebruik van een concept genaamd toewijzingen om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers, worden alleen de gebruikers en/of groepen die zijn toegewezen aan een toepassing in Azure AD gesynchroniseerd.

Voordat u configureren en inschakelen van automatische inrichten van gebruikers, moet u bepalen welke gebruikers en/of groepen in Azure AD toegang hebben tot federatieve Directory moeten. Wanneer besloten, kunt u deze gebruikers en/of groepen toewijzen aan federatieve Directory door de instructies hier:

 * [Een gebruiker of groep toewijzen aan een enterprise-app](../manage-apps/assign-user-or-group-access-portal.md) 
 
 ## <a name="important-tips-for-assigning-users-to-federated-directory"></a>Belangrijke tips voor het toewijzen van gebruikers aan de Directory federatieve
 * Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan federatieve Directory voor het testen van de configuratie van de automatische gebruikersinrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

* Wanneer een gebruiker toewijzen aan federatieve Directory, moet u alle geldige toepassingsspecifieke rollen (indien beschikbaar) in het dialoogvenster toewijzing selecteren. Gebruikers met de functie standaardtoegang worden uitgesloten van het inrichten.
    
 ## <a name="set-up-federated-directory-for-provisioning"></a>Federatieve Directory instellen voor het inrichten van

Voordat u federatieve Directory configureert voor het automatisch gebruikers inrichten met Azure AD, moet u SCIM inrichting op federatieve Directory inschakelen.

1. Aanmelden bij uw [federatieve Directory-beheerconsole](https://federated.directory/of)

    ![Federatieve Directory-zelfstudie](media/federated-directory-provisioning-tutorial/companyname.png)

2. Navigeer naar **mappen > gebruiker mappen** en selecteert u uw tenant. 

    ![federatieve directory](media/federated-directory-provisioning-tutorial/ad-user-directories.png)

3.  Voor het genereren van een permanente bearer-token, gaat u naar **Directory sleutels > Create New Key.** 

    ![federatieve directory](media/federated-directory-provisioning-tutorial/federated01.png)

4. De sleutel van een map maken. 

    ![federatieve directory](media/federated-directory-provisioning-tutorial/federated02.png)
    

5. Kopieer de **toegangstoken** waarde. Deze waarde moet worden ingevoerd in de **geheim Token** veld op het tabblad inrichten van uw federatieve Directory-toepassing in Azure portal. 

    ![federatieve directory](media/federated-directory-provisioning-tutorial/federated03.png)
    
## <a name="add-federated-directory-from-the-gallery"></a>Federatieve-map vanuit de galerie toevoegen

Als u wilt configureren voor federatieve Directory voor automatisch gebruikers inrichten met Azure AD, moet u federatieve map uit de galerie met Azure AD toevoegen aan uw lijst met beheerde SaaS-toepassingen.

**Als u wilt toevoegen voor federatieve Directory uit de galerie met Azure AD, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)** , selecteer in het navigatievenster aan de linkerkant **Azure Active Directory**.

    ![De Azure Active Directory-knop](common/select-azuread.png)

2. Ga naar **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![De blade Enterprise-toepassingen](common/enterprise-applications.png)

3. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop aan de bovenkant van het deelvenster.

    ![De knop nieuwe toepassing](common/add-new-app.png)

4. Voer in het zoekvak **Directory federatieve**, selecteer **Directory federatieve** in het deelvenster resultaten.

    ![Federatieve-map in de lijst met resultaten](common/search-new-app.png)

5. Navigeer naar de **URL** hieronder gemarkeerd in een afzonderlijke browser. 

    ![federatieve directory](media/federated-directory-provisioning-tutorial/loginpage1.png)

6. Klik op **aanmelden**.

    ![federatieve directory](media/federated-directory-provisioning-tutorial/federated04.png)

7.  Als federatieve Directory een app OpenIDConnect is, kiest u aanmelden bij federatieve Directory met uw Microsoft-werkaccount.
    
    ![federatieve directory](media/federated-directory-provisioning-tutorial/loginpage3.png)
 
8. Na een geslaagde verificatie door de toestemmingsprompt voor voor de pagina toestemming te accepteren. De toepassing wordt vervolgens automatisch worden toegevoegd aan uw tenant en u wordt omgeleid naar uw federatieve Directory-account.

    ![federatieve directory toevoegen SCIM](media/federated-directory-provisioning-tutorial/premission.png)



## <a name="configuring-automatic-user-provisioning-to-federated-directory"></a>Automatisch gebruikers inrichten met federatieve Directory configureren 

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD-inrichtingsservice als u wilt maken, bijwerken en uitschakelen van gebruikers en/of groepen in federatieve Directory op basis van gebruiker en/of toewijzingen van groepen in Azure AD.

### <a name="to-configure-automatic-user-provisioning-for-federated-directory-in-azure-ad"></a>Het configureren van automatisch gebruikers inrichten voor federatieve Directory in Azure AD:

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Selecteer **bedrijfstoepassingen**en selecteer vervolgens **alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Directory federatieve**.

    ![De Directory federatieve-koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer de **Provisioning** tabblad.

    ![Inrichting](common/provisioning.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Inrichting](common/provisioning-automatic.png)

5. Onder de **beheerdersreferenties** sectie invoer `https://api.federated.directory/v2/` in de Tenant-URL. Voer de waarde die u hebt opgehaald en eerder hebt opgeslagen vanuit de map federatieve in **geheim Token**. Klik op **testverbinding** om te controleren of Azure AD, kan verbinding maken met federatieve Directory. Als de verbinding is mislukt, zorg ervoor dat uw federatieve Directory-account beheerdersmachtigingen heeft en probeer het opnieuw.

    ![Tenant-URL + Token](common/provisioning-testconnection-tenanturltoken.png)

8. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet de inrichting fout ontvangen en schakel het selectievakje in - **een e-mailmelding verzenden wanneer een foutoptreedt**.

    ![E-mailmelding](common/provisioning-notification-email.png)

9. Klik op **Opslaan**.

10. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory: gebruikers aan Directory federatieve**.

    ![Federatieve Directory-zelfstudie](media/federated-directory-provisioning-tutorial/user-mappings.png)
    
    
11. Controleer de kenmerken van de gebruiker die worden gesynchroniseerd vanuit Azure AD naar federatieve map in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in de Directory federatieve voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

    ![Federatieve Directory-zelfstudie](media/federated-directory-provisioning-tutorial/user-attributes.png)
    

12. Als u wilt configureren bereikfilters, raadpleegt u de volgende instructies in de [Scoping filter zelfstudie](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

13. Zodat de Azure AD-inrichtingsservice voor federatieve Directory wijzigen de **Inrichtingsstatus** naar **op** in de **instellingen** sectie.

    ![Inrichtingsstatus ingeschakeld](common/provisioning-toggle-on.png)

14. De gebruikers en/of groepen die u wilt definiëren voor het inrichten van federatieve Directory door de gewenste waarden in **bereik** in de **instellingen** sectie.

    ![Inrichting van bereik](common/provisioning-scope.png)

15. Wanneer u klaar om in te richten bent, klikt u op **opslaan**.

    ![Bezig met opslaan van de Inrichtingsconfiguratie](common/provisioning-configuration-save.png)

Met deze bewerking wordt gestart voor de initiële synchronisatie van alle gebruikers en/of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de Azure AD-inrichtingsservice wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van rapport van de activiteit, die alle acties die worden uitgevoerd door de Azure AD-inrichtingsservice op federatieve Directory beschrijft.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md)
## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../manage-apps/check-status-user-account-provisioning.md)