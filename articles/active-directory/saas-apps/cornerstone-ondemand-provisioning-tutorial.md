---
title: 'Zelfstudie: Hoeksteen OnDemand configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en inrichting van gebruikersaccounts naar hoeksteen OnDemand ongedaan maken.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: v-ant
ms.collection: M365-identity-device-management
ms.openlocfilehash: a24945cae273e072d3be314a66248674e25fe287
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56198861"
---
# <a name="tutorial-configure-cornerstone-ondemand-for-automatic-user-provisioning"></a>Zelfstudie: Hoeksteen OnDemand configureren voor het automatisch inrichten van gebruikers


Het doel van deze zelfstudie is ter illustratie van de stappen om te worden uitgevoerd in de hoeksteen OnDemand en Azure Active Directory (Azure AD) naar Azure AD configureren voor automatisch inrichten en verwijdering van gebruikers en/of groepen aan hoeksteen OnDemand.


> [!NOTE]
> Deze zelfstudie beschrijft een connector die is gebaseerd op de Provisioning-Service van Azure AD-gebruiker. Zie voor belangrijke informatie over wat deze service biedt, hoe het werkt en veelgestelde vragen [automatiseren van gebruikersinrichting en -opheffing in SaaS-toepassingen met Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende vereisten hebt:

*   Een Azure AD-tenant
*   Een OnDemand-hoeksteen-tenant
*   Een gebruikersaccount in hoeksteen OnDemand met beheerdersmachtigingen


> [!NOTE]
> De integratie wordt ingericht op Azure AD is afhankelijk van de [hoeksteen OnDemand Webservice](https://help.csod.com/help/csod_0/Content/Resources/Documents/WebServices/CSOD_-_Summary_of_Web_Services_v20151106.pdf), die voor hoeksteen OnDemand teams beschikbaar is.

## <a name="adding-cornerstone-ondemand-from-the-gallery"></a>Cornerstone OnDemand toevoegen vanuit de galerie
Voordat u hoeksteen OnDemand configureert voor het automatisch gebruikers inrichten met Azure AD, moet u hoeksteen OnDemand uit de galerie met Azure AD toevoegen aan uw lijst met beheerde SaaS-toepassingen.

**Als u wilt toevoegen hoeksteen OnDemand uit de galerie met Azure AD, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op de **Azure Active Directory** pictogram. 

    ![De knop Azure Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen** > **alle toepassingen**.

    ![De sectie voor bedrijfstoepassingen][2]
    
3. Als u wilt toevoegen hoeksteen OnDemand, klikt u op de **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

4. Typ in het zoekvak **hoeksteen OnDemand**.

    ![Cornerstone OnDemand Provisioning](./media/cornerstone-ondemand-provisioning-tutorial/AppSearch.png)

5. Selecteer in het deelvenster resultaten **hoeksteen OnDemand**, en klik vervolgens op de **toevoegen** knop hoeksteen OnDemand toevoegen aan uw lijst met SaaS-toepassingen.

    ![Cornerstone OnDemand Provisioning](./media/cornerstone-ondemand-provisioning-tutorial/AppSearchResults.png)

    ![Cornerstone OnDemand Provisioning](./media/cornerstone-ondemand-provisioning-tutorial/AppCreation.png)

## <a name="assigning-users-to-cornerstone-ondemand"></a>Gebruikers toewijzen aan hoeksteen OnDemand

Azure Active Directory maakt gebruik van een concept genaamd "toewijzingen" om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers, worden alleen de gebruikers en/of groepen die '' aan een toepassing in Azure AD toegewezen zijn gesynchroniseerd. 

Voordat u configureren en inschakelen van automatische inrichten van gebruikers, moet u bepalen welke gebruikers en/of groepen in Azure AD toegang hebben tot hoeksteen OnDemand moeten. Wanneer u beslist, kunt u deze gebruikers en/of groepen toewijzen aan hoeksteen OnDemand door de instructies hier:

*   [Een gebruiker of groep toewijzen aan een enterprise-app](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-cornerstone-ondemand"></a>Belangrijke tips voor het toewijzen van gebruikers aan hoeksteen OnDemand

*   Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan hoeksteen OnDemand voor het testen van de configuratie van de automatische gebruikersinrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Wanneer een gebruiker aan hoeksteen OnDemand toewijzen, moet u alle geldige toepassingsspecifieke rollen (indien beschikbaar) selecteren in het dialoogvenster toewijzing. Gebruikers met de **standaardtoegang** rol worden uitgesloten van het inrichten.

## <a name="configuring-automatic-user-provisioning-to-cornerstone-ondemand"></a>Automatisch gebruikers inrichten voor hoeksteen OnDemand configureren

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD-inrichtingsservice als u wilt maken, bijwerken en uitschakelen van gebruikers en/of groepen in hoeksteen OnDemand op basis van gebruiker en/of toewijzingen van groepen in Azure AD.


### <a name="to-configure-automatic-user-provisioning-for-cornerstone-ondemand-in-azure-ad"></a>Het configureren van automatisch gebruikers inrichten voor hoeksteen OnDemand in Azure AD:


1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en blader naar **Azure Active Directory > bedrijfstoepassingen > alle toepassingen**.

2. Selecteer hoeksteen OnDemand uit de lijst met SaaS-toepassingen.
 
    ![Cornerstone OnDemand Provisioning](./media/cornerstone-ondemand-provisioning-tutorial/Successcenter2.png)

3. Selecteer de **Provisioning** tabblad.

    ![Cornerstone OnDemand Provisioning](./media/cornerstone-ondemand-provisioning-tutorial/ProvisioningTab.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Cornerstone OnDemand Provisioning](./media/cornerstone-ondemand-provisioning-tutorial/ProvisioningCredentials.png)

5. Onder de **beheerdersreferenties** sectie, voer de **Admin Username**, **beheerderswachtwoord**, en **domein** van uw hoeksteen-OnDemand -account.

    *   In de **Admin Username** veld, de domein\gebruikersnaam van het beheerdersaccount dat op uw tenant hoeksteen OnDemand vullen. Voorbeeld: contoso\admin.

    *   In de **beheerderswachtwoord** veld, het wachtwoord dat overeenkomt met de gebruikersnaam van beheerder vullen.

    *   In de **domein** veld, de URL van de webservice van de tenant hoeksteen OnDemand vullen. Voorbeeld: De service bevindt zich in `https://ws-[corpname].csod.com/feed30/clientdataservice.asmx`voor het domein Contoso is `https://ws-contoso.csod.com/feed30/clientdataservice.asmx`. Zie voor meer informatie over het ophalen van de webservice-URL [hier](https://help.csod.com/help/csod_0/Content/Resources/Documents/WebServices/CSOD_Web_Services_-_User-OU_Technical_Specification_v20160222.pdf).

6. Bij het invullen van de velden die in stap 5 wordt weergegeven, klikt u op **testverbinding** om te controleren of Azure AD kunt verbinden met hoeksteen OnDemand. Als de verbinding is mislukt, zorg ervoor dat uw account hoeksteen OnDemand beheerdersmachtigingen heeft en probeer het opnieuw.

    ![Cornerstone OnDemand Provisioning](./media/cornerstone-ondemand-provisioning-tutorial/TestConnection.png)

7. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet de inrichting fout ontvangen en schakel het selectievakje in - **een e-mailmelding verzenden wanneer een foutoptreedt**.

    ![Cornerstone OnDemand Provisioning](./media/cornerstone-ondemand-provisioning-tutorial/EmailNotification.png)

8. Klik op **Opslaan**.

9. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory: gebruikers aan hoeksteen OnDemand**.

    ![Cornerstone OnDemand Provisioning](./media/cornerstone-ondemand-provisioning-tutorial/UserMapping.png)

10. Controleer de kenmerken van de gebruiker die van Azure AD worden gesynchroniseerd naar hoeksteen OnDemand in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in hoeksteen OnDemand voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

    ![Cornerstone OnDemand Provisioning](./media/cornerstone-ondemand-provisioning-tutorial/UserMappingAttributes.png)

11. Als u wilt configureren bereikfilters, raadpleegt u de volgende instructies in de [Scoping filter zelfstudie](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

12. Om in te schakelen in de Azure AD-inrichtingsservice voor hoeksteen OnDemand, wijzigen de **Inrichtingsstatus** naar **op** in de **instellingen** sectie.

    ![Cornerstone OnDemand Provisioning](./media/cornerstone-ondemand-provisioning-tutorial/ProvisioningStatus.png)

13. De gebruikers en/of groepen die u wilt definiëren voor het inrichten van hoeksteen OnDemand door het kiezen van de gewenste waarden in **bereik** in de **instellingen** sectie.

    ![Cornerstone OnDemand Provisioning](./media/cornerstone-ondemand-provisioning-tutorial/SyncScope.png)

14. Wanneer u klaar om in te richten bent, klikt u op **opslaan**.

    ![Cornerstone OnDemand Provisioning](./media/cornerstone-ondemand-provisioning-tutorial/Save.png)


Met deze bewerking wordt gestart voor de initiële synchronisatie van alle gebruikers en/of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de Azure AD-inrichtingsservice wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van rapport van de activiteit, die alle acties die worden uitgevoerd door de Azure AD-inrichtingsservice op hoeksteen OnDemand beschrijft.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).
## <a name="connector-limitations"></a>Connector-beperkingen

* De hoeksteen OnDemand **positie** kenmerk wordt verwacht dat een waarde die overeenkomt met de rollen op de hoeksteen OnDemand-portal. De lijst met geldige **positie** waarden kunnen worden opgehaald door te navigeren naar **gebruikersrecord bewerken > structuur van organisatie > positie** in de hoeksteen OnDemand-portal.
    ![Gebruiker hoeksteen OnDemand inrichting bewerken](./media/cornerstone-ondemand-provisioning-tutorial/UserEdit.png) ![hoeksteen OnDemand inrichting positie](./media/cornerstone-ondemand-provisioning-tutorial/UserPosition.png) ![hoeksteen OnDemand lijst posities inrichten](./media/cornerstone-ondemand-provisioning-tutorial/PostionId.png)
    
## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/cornerstone-ondemand-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/cornerstone-ondemand-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/cornerstone-ondemand-provisioning-tutorial/tutorial_general_03.png
