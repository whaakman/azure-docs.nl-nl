---
title: 'Zelfstudie: Cisco Spark configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en gebruikersaccounts met Cisco Spark ongedaan in te richten.
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
ms.openlocfilehash: 74907693270e6cd340d3b34585a80077aa87f0f7
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37058959"
---
# <a name="tutorial-configure-cisco-spark-for-automatic-user-provisioning"></a>Zelfstudie: Cisco Spark configureren voor het automatisch gebruikers inrichten


Het doel van deze zelfstudie is ter illustratie van de stappen worden uitgevoerd in Cisco Spark en Azure Active Directory (Azure AD) naar Azure AD configureren om automatisch in te richten en gebruikers met Cisco Spark ongedaan hebt ingericht.


> [!NOTE]
> Deze zelfstudie wordt een connector die is ingebouwd in de Azure AD-gebruiker inrichtingsservice beschreven. Zie voor belangrijke informatie over wat deze service doet, hoe het werkt en veelgestelde vragen [gebruikers inrichten en opheffen van inrichting voor SaaS-toepassingen met Azure Active Directory automatiseren](./../active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende vereisten hebt:

*   Een Azure AD-tenant
*   Een tenant Cisco Spark
*   Een gebruikersaccount in Cisco Spark met beheerdersmachtigingen


> [!NOTE]
> De Azure AD integratie inrichting is afhankelijk van de [Cisco Spark Webservice](https://developer.webex.com/getting-started.html), die voor Cisco Spark teams beschikbaar is.

## <a name="adding-cisco-spark-from-the-gallery"></a>Cisco Spark uit de galerie toevoegen
Voordat u Cisco Spark configureert voor het automatisch gebruikers inrichten met Azure AD, moet u Cisco Spark uit de galerie van Azure AD-toepassing toevoegen aan de lijst met beheerde SaaS-toepassingen.

**Als u wilt Cisco Spark uit de galerie van Azure AD-toepassing toevoegt, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op het **Azure Active Directory** pictogram.

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen** > **alle toepassingen**.

    ![De bedrijfstoepassingen in de sectie][2]

3. Cisco Spark toevoegen: klik op de **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Cisco Spark**.

    ![Cisco Spark inrichten](./media/cisco-spark-provisioning-tutorial/AppSearch.png)

5. Selecteer in het deelvenster resultaten **Cisco Spark**, en klik vervolgens op de **toevoegen** knop Cisco Spark toevoegen aan de lijst met SaaS-toepassingen.

    ![Cisco Spark inrichten](./media/cisco-spark-provisioning-tutorial/AppSearchResults.png)

    ![Cisco Spark inrichten](./media/cisco-spark-provisioning-tutorial/AppCreation.png)

## <a name="assigning-users-to-cisco-spark"></a>Gebruikers toewijzen aan Cisco Spark

Azure Active Directory gebruikt een concept 'toewijzingen' genoemd om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch gebruikers inrichten, worden alleen de gebruikers en/of groepen die '' tot een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureren en inschakelen automatisch gebruikers inrichten, moet u bepalen welke gebruikers in Azure AD moeten toegang hebben tot Cisco Spark. Als besloten, kunt u deze gebruikers toewijzen aan Cisco Spark door de volgende instructies te volgen:

*   [Een gebruiker of groep toewijzen aan een enterprise-app](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-cisco-spark"></a>Belangrijke tips voor het toewijzen van gebruikers met Cisco Spark

*   Het is raadzaam om één Azure AD-gebruiker is toegewezen aan Cisco Spark voor het testen van het automatische configuratie van gebruikers. Extra gebruikers kunnen later worden toegewezen.

*   Bij het toewijzen van een gebruiker met Cisco Spark, moet u een geldige rol toepassingsspecifieke (indien beschikbaar) in het dialoogvenster toewijzing. Gebruikers met de **standaardtoegang** rol worden uitgesloten van de inrichting.

## <a name="configuring-automatic-user-provisioning-to-cisco-spark"></a>Automatisch gebruikers inrichten met Cisco Spark configureren

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD als u wilt maken, bijwerken en uitschakelen van gebruikers in Cisco Spark op basis van de toewijzingen van gebruikers in Azure AD-service inricht.


### <a name="to-configure-automatic-user-provisioning-for-cisco-spark-in-azure-ad"></a>Voor het configureren van automatische gebruikersinrichting voor Cisco Spark in Azure AD:


1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en blader naar **Azure Active Directory > bedrijfstoepassingen > alle toepassingen**.

2. Selecteer Cisco Spark uit de lijst met SaaS-toepassingen.

    ![Cisco Spark inrichten](./media/cisco-spark-provisioning-tutorial/Successcenter2.png)

3. Selecteer de **inrichten** tabblad.

    ![Cisco Spark inrichten](./media/cisco-spark-provisioning-tutorial/ProvisioningTab.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Cisco Spark inrichten](./media/cisco-spark-provisioning-tutorial/ProvisioningCredentials.png)

5. Onder de **beheerdersreferenties** sectie, voer de **Tenant-URL**, en **geheim Token** van uw Cisco-Spark-account.

    *   In de **Tenant-URL** veld, vullen van de Cisco Spark SCIM API-URL voor uw tenant, die de vorm van `https://api.ciscospark.com/v1/scim/[Tenant ID]/`, waarbij `[Tenant ID]` is een alfanumerieke tekenreeks, zoals beschreven in stap 6.

    *   In de **geheim Token** veld, vullen geheim Token, zoals beschreven in stap 6.

1. De **Tenant-ID** en **geheim Token** voor uw Spark Cisco account kan worden gevonden door logboekregistratie in de [Cisco Spark developer site](https://developer.webex.com/) met uw beheerdersaccount. Eenmaal aangemeld -
    * Ga naar de [pagina aan de slag](https://developer.webex.com/getting-started.html)
    * Schuif omlaag naar de [verificatiesectie](https://developer.webex.com/getting-started.html#authentication)
    ![Cisco Spark-verificatietoken](./media/cisco-spark-provisioning-tutorial/SecretToken.png)
    * De alfanumerieke tekenreeks in het vak is uw **geheim Token**. Dit token naar het Klembord kopiëren
    * Ga naar de [pagina Mijn eigen gegevens ophalen](https://developer.webex.com/endpoint-people-me-get.html)
        * Zorg ervoor dat de testmodus ingeschakeld is
        * Typ het woord 'Bearer' gevolgd door een spatie en plak het geheim Token in het veld autorisatie ![Cisco Spark-verificatietoken](./media/cisco-spark-provisioning-tutorial/GetMyDetails.png)
        * Klik op uitvoeren
    * In de antwoordtekst aan de rechterkant, de **Tenant-ID** wordt weergegeven als 'orgId':

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

1. De velden die worden weergegeven in stap 5 in te vullen, klik op **testverbinding** om te controleren of Azure AD, kan verbinding maken met Cisco Spark. Als de verbinding is mislukt, zorg ervoor dat uw account Cisco Spark beheerdersmachtigingen heeft en probeer het opnieuw.

    ![Cisco Spark inrichten](./media/cisco-spark-provisioning-tutorial/TestConnection.png)

8. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet het inrichtingsproces fout meldingen ontvangen en schakel het selectievakje in - **e-mailmelding verzenden wanneer een foutoptreedt**.

    ![Cisco Spark inrichten](./media/cisco-spark-provisioning-tutorial/EmailNotification.png)

9. Klik op **Opslaan**.

10. Onder de **toewijzingen** sectie **synchroniseren Azure Active Directory-gebruikers met Cisco Spark**.

    ![Cisco Spark inrichten](./media/cisco-spark-provisioning-tutorial/UserMapping.png)

11. Controleer de kenmerken van de gebruiker die zijn gesynchroniseerd vanuit Azure AD met Cisco Spark in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen overeenkomen met de gebruikersaccounts in Cisco Spark voor update-bewerkingen worden gebruikt. Selecteer de **opslaan** knop eventuele wijzigingen doorvoeren.

    ![Cisco Spark inrichten](./media/cisco-spark-provisioning-tutorial/UserMappingAttributes.png)

12. Scope om filters te configureren, raadpleegt u de volgende instructies in de [Scoping filter zelfstudie](../active-directory-saas-scoping-filters.md).

13. Om de Azure AD-service voor Cisco Spark inricht, wijzigen de **inrichting Status** naar **op** in de **instellingen** sectie.

    ![Cisco Spark inrichten](./media/cisco-spark-provisioning-tutorial/ProvisioningStatus.png)

14. De gebruikers en/of groepen die u wilt definiëren om in te richten met Cisco Spark door het kiezen van de gewenste waarden in **bereik** in de **instellingen** sectie.

    ![Cisco Spark inrichten](./media/cisco-spark-provisioning-tutorial/SyncScope.png)

15. Wanneer u klaar om in te richten bent, klikt u op **opslaan**.

    ![Cisco Spark inrichten](./media/cisco-spark-provisioning-tutorial/Save.png)


Deze bewerking begint de initiële synchronisatie van alle gebruikers en/of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de Azure AD-service inricht wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en volg de koppelingen voor het inrichten van activiteitenrapport waarin alle acties die worden uitgevoerd door de Azure AD Cisco Spark-service inricht.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over automatische account gebruikersaanvragen](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Het beheren van gebruikers account inrichten voor zakelijke Apps](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)


## <a name="next-steps"></a>Volgende stappen

* [Informatie over het bekijken van Logboeken en rapporten over het inrichten van de activiteit ophalen](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/cisco-spark-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/cisco-spark-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/cisco-spark-provisioning-tutorial/tutorial_general_03.png