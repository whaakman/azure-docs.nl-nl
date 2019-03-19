---
title: 'Zelfstudie: Cisco Spark configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en inrichting ongedaan maken-gebruikersaccounts met Cisco Spark.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2018
ms.author: v-wingf
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8fd7145be5f82a90acced0f02e35f6c8880dbcad
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58120394"
---
# <a name="tutorial-configure-cisco-spark-for-automatic-user-provisioning"></a>Zelfstudie: Cisco Spark voor het automatisch inrichten van gebruikers configureren


Het doel van deze zelfstudie is ter illustratie van de stappen om te worden uitgevoerd in de Cisco Spark en Azure Active Directory (Azure AD) naar Azure AD configureren voor automatisch inrichten en de inrichting van gebruikers met Cisco Spark ongedaan maken.


> [!NOTE]
> Deze zelfstudie beschrijft een connector die is gebaseerd op de Provisioning-Service van Azure AD-gebruiker. Zie voor belangrijke informatie over wat deze service biedt, hoe het werkt en veelgestelde vragen [automatiseren van gebruikersinrichting en -opheffing in SaaS-toepassingen met Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende vereisten hebt:

*   Een Azure AD-tenant
*   Een Cisco-Spark-tenant
*   Een gebruikersaccount in Cisco Spark met beheerdersmachtigingen


> [!NOTE]
> De integratie wordt ingericht op Azure AD is afhankelijk van de [Cisco Spark Webservice](https://developer.webex.com/getting-started.html), die beschikbaar is voor teams van Cisco Spark.

## <a name="adding-cisco-spark-from-the-gallery"></a>Cisco Spark toevoegen vanuit de galerie
Voordat u Cisco Spark configureert voor het automatisch gebruikers inrichten met Azure AD, moet u Cisco Spark uit de galerie met Azure AD toevoegen aan uw lijst met beheerde SaaS-toepassingen.

**Als u wilt toevoegen Cisco Spark uit de galerie met Azure AD, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op de **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen** > **alle toepassingen**.

    ![De sectie voor bedrijfstoepassingen][2]

3. Cisco Spark toevoegen, klikt u op de **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

4. Typ in het zoekvak **Cisco Spark**.

    ![Cisco Spark Provisioning](./media/cisco-spark-provisioning-tutorial/AppSearch.png)

5. Selecteer in het deelvenster resultaten **Cisco Spark**, en klik vervolgens op de **toevoegen** knop Cisco Spark toevoegen aan uw lijst met SaaS-toepassingen.

    ![Cisco Spark Provisioning](./media/cisco-spark-provisioning-tutorial/AppSearchResults.png)

    ![Cisco Spark Provisioning](./media/cisco-spark-provisioning-tutorial/AppCreation.png)

## <a name="assigning-users-to-cisco-spark"></a>Gebruikers toewijzen aan Cisco Spark

Azure Active Directory maakt gebruik van een concept genaamd "toewijzingen" om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers, worden alleen de gebruikers en/of groepen die '' aan een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureren en inschakelen van automatisch inrichten van gebruikers, moet u bepalen welke gebruikers in Azure AD moeten toegang hebben tot Cisco Spark. Als besloten, kunt u deze gebruikers toewijzen aan Cisco Spark en volg de instructies hier:

*   [Een gebruiker of groep toewijzen aan een enterprise-app](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-cisco-spark"></a>Belangrijke tips voor het toewijzen van gebruikers met Cisco Spark

*   Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan Cisco Spark voor het testen van de configuratie van de automatische gebruikersinrichting. Extra gebruikers kunnen later worden toegewezen.

*   Bij het toewijzen van een gebruiker met Cisco Spark, moet u een geldige toepassingsspecifieke-rol (indien beschikbaar) in het dialoogvenster toewijzing selecteren. Gebruikers met de **standaardtoegang** rol worden uitgesloten van het inrichten.

## <a name="configuring-automatic-user-provisioning-to-cisco-spark"></a>Configuratie van automatisch gebruikers inrichten met Cisco Spark

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD-inrichtingsservice als u wilt maken, bijwerken en gebruikers uitschakelen in Cisco Spark op basis van de toewijzingen van gebruikers in Azure AD.


### <a name="to-configure-automatic-user-provisioning-for-cisco-spark-in-azure-ad"></a>Het configureren van automatisch gebruikers inrichten voor Cisco Spark in Azure AD:


1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en blader naar **Azure Active Directory > bedrijfstoepassingen > alle toepassingen**.

2. Selecteer Cisco Spark in uw lijst met SaaS-toepassingen.

    ![Cisco Spark Provisioning](./media/cisco-spark-provisioning-tutorial/Successcenter2.png)

3. Selecteer de **Provisioning** tabblad.

    ![Cisco Spark Provisioning](./media/cisco-spark-provisioning-tutorial/ProvisioningTab.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Cisco Spark Provisioning](./media/cisco-spark-provisioning-tutorial/ProvisioningCredentials.png)

5. Onder de **beheerdersreferenties** sectie, voer de **Tenant-URL**, en **geheim Token** van uw Cisco-Spark-account.

    *   In de **Tenant-URL** veld, de URL van de Cisco Spark SCIM API invullen voor uw tenant, waarbij de vorm van `https://api.ciscospark.com/v1/scim/[Tenant ID]/`, waarbij `[Tenant ID]` is een alfanumerieke tekenreeks, zoals beschreven in stap 6.

    *   In de **geheim Token** veld, vul het geheim Token zoals beschreven in stap 6.

1. De **Tenant-ID** en **geheim Token** voor uw Spark Cisco account kan worden gevonden door bent aangemeld bij de [Cisco Spark developer-site](https://developer.webex.com/) met uw beheerdersaccount. Één keer aangemeld:
   * Ga naar de [pagina aan de slag](https://developer.webex.com/getting-started.html)
   * Schuif omlaag naar de [-verificatiesectie](https://developer.webex.com/getting-started.html#authentication)
     ![Cisco Spark-verificatietoken](./media/cisco-spark-provisioning-tutorial/SecretToken.png)
   * De alfanumerieke tekenreeks in het vak is uw **geheim Token**. Dit token naar het Klembord kopiëren
   * Ga naar de [pagina Mijn eigen Details ophalen](https://developer.webex.com/endpoint-people-me-get.html)
       * Zorg ervoor dat de testmodus op
       * Typ het woord 'Bearer' gevolgd door een spatie en plakt u het geheim-Token in het veld autorisatie ![Cisco Spark-verificatietoken](./media/cisco-spark-provisioning-tutorial/GetMyDetails.png)
       * Klik op uitvoeren
   * In de antwoordtekst aan de rechterkant, het **Tenant-ID** wordt weergegeven als 'orgId':

     ```json
     {
       "id": "(...)",
       "emails": [
           "admin.user@contoso.com"
       ],
       "displayName": "John Smith",
       "nickName": "John",
       "orgId": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
       (...)
     }
     ```

1. Bij het invullen van de velden die in stap 5 wordt weergegeven, klikt u op **testverbinding** om te controleren of Azure AD een verbinding kunt maken met Cisco Spark. Als de verbinding is mislukt, zorg ervoor dat uw Cisco Spark account beheerdersmachtigingen heeft en probeer het opnieuw.

    ![Cisco Spark Provisioning](./media/cisco-spark-provisioning-tutorial/TestConnection.png)

8. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet de inrichting fout ontvangen en schakel het selectievakje in - **een e-mailmelding verzenden wanneer een foutoptreedt**.

    ![Cisco Spark Provisioning](./media/cisco-spark-provisioning-tutorial/EmailNotification.png)

9. Klik op **Opslaan**.

10. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory: gebruikers met Cisco Spark**.

    ![Cisco Spark Provisioning](./media/cisco-spark-provisioning-tutorial/UserMapping.png)

11. Controleer de kenmerken van de gebruiker die worden gesynchroniseerd vanuit Azure AD met Cisco Spark in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in Cisco Spark voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

    ![Cisco Spark Provisioning](./media/cisco-spark-provisioning-tutorial/UserMappingAttributes.png)

12. Als u wilt configureren bereikfilters, raadpleegt u de volgende instructies in de [Scoping filter zelfstudie](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

13. Om in te schakelen in de Azure AD-inrichtingsservice voor Cisco Spark, wijzigen de **Inrichtingsstatus** naar **op** in de **instellingen** sectie.

    ![Cisco Spark Provisioning](./media/cisco-spark-provisioning-tutorial/ProvisioningStatus.png)

14. De gebruikers en/of groepen die u wilt inrichten met Cisco Spark definiëren door het kiezen van de gewenste waarden in **bereik** in de **instellingen** sectie.

    ![Cisco Spark Provisioning](./media/cisco-spark-provisioning-tutorial/SyncScope.png)

15. Wanneer u klaar om in te richten bent, klikt u op **opslaan**.

    ![Cisco Spark Provisioning](./media/cisco-spark-provisioning-tutorial/Save.png)


Met deze bewerking wordt gestart voor de initiële synchronisatie van alle gebruikers en/of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de Azure AD-inrichtingsservice wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van rapport van de activiteit, die alle acties die worden uitgevoerd door de Azure AD-inrichtingsservice op Cisco Spark beschrijft.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Connector-beperkingen

* Cisco Spark is momenteel in de Cisco vroege veld testen (EFT) fase. Voor meer informatie, neem contact op met [van Cisco-ondersteuningsteam](https://www.webex.co.in/support/support-overview.html). 

## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)


## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/cisco-spark-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/cisco-spark-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/cisco-spark-provisioning-tutorial/tutorial_general_03.png
