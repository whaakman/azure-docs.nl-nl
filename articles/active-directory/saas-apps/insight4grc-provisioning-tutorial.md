---
title: 'Zelfstudie: Insight4GRC configureren voor het automatisch inrichten van gebruikers met Azure Active Directory | Microsoft Docs'
description: Meer informatie over het configureren van Azure Active Directory voor het automatisch inrichten en ongedaan maken van de inrichting van gebruikers accounts op Insight4GRC.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: 34718201-4f0e-4260-9af0-b3b70a1e8265
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2019
ms.author: zhchia
ms.openlocfilehash: 7e8c6c2277f29fc114033aac24844e9e85816b78
ms.sourcegitcommit: 78ebf29ee6be84b415c558f43d34cbe1bcc0b38a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/12/2019
ms.locfileid: "68951034"
---
# <a name="tutorial-configure-insight4grc--for-automatic-user-provisioning"></a>Zelfstudie: Insight4GRC configureren voor automatische gebruikers inrichting

Het doel van deze zelf studie is het demonstreren van de stappen die moeten worden uitgevoerd in Insight4GRC en Azure Active Directory (Azure AD) om Azure AD te configureren voor het automatisch inrichten en ongedaan maken van de inrichting van gebruikers en/of groepen in Insight4GRC.

> [!NOTE]
> In deze zelf studie wordt een connector beschreven die boven op de Azure AD User Provisioning-Service is gebouwd. Zie [Gebruikers inrichten en de inrichting ongedaan maken voor SaaS-toepassingen met Azure Active Directory](../manage-apps/user-provisioning.md)voor belang rijke informatie over de werking van deze service, hoe deze werkt en veelgestelde vragen.
>
> Deze connector bevindt zich momenteel in de open bare preview. Zie [aanvullende gebruiksrecht overeenkomst voor Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)previews voor meer informatie over de algemene Microsoft Azure gebruiksrecht overeenkomst voor preview-functies.

## <a name="prerequisites"></a>Vereisten

In het scenario dat in deze zelf studie wordt beschreven, wordt ervan uitgegaan dat u al beschikt over de volgende vereisten:

* Een Azure AD-tenant.
* [Een Insight4GRC-Tenant](https://www.rsmuk.com/).
* Een gebruikers account in Insight4GRC met beheerders machtigingen.

## <a name="assigning-users-to-insight4grc"></a>Gebruikers toewijzen aan Insight4GRC 

Azure Active Directory gebruikt een concept met de naam *toewijzingen* om te bepalen welke gebruikers toegang moeten krijgen tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers worden alleen de gebruikers en/of groepen die zijn toegewezen aan een toepassing in azure AD gesynchroniseerd.

Voordat u automatische gebruikers inrichting configureert en inschakelt, moet u beslissen welke gebruikers en/of groepen in azure AD toegang nodig hebben tot Insight4GRC. Eenmaal besloten, kunt u deze gebruikers en/of groepen toewijzen aan Insight4GRC door de volgende instructies te volgen:

* [Een gebruiker of groep toewijzen aan een bedrijfs-app](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-insight4grc"></a>Belang rijke tips voor het toewijzen van gebruikers aan Insight4GRC 

* U wordt aangeraden één Azure AD-gebruiker toe te wijzen aan Insight4GRC om de configuratie van automatische gebruikers inrichting te testen. Extra gebruikers en/of groepen kunnen later worden toegewezen.

* Wanneer u een gebruiker toewijst aan Insight4GRC, moet u een geldige toepassingsspecifieke rol (indien beschikbaar) selecteren in het dialoog venster toewijzing. Gebruikers met de rol **standaard toegang** worden uitgesloten van het inrichten.

## <a name="setup-insight4grc-for-provisioning"></a>Insight4GRC instellen voor inrichting

Voordat u Insight4GRC configureert voor het automatisch inrichten van gebruikers met Azure AD, moet u SCIM inrichten inschakelen op Insight4GRC.

1. Om het Bearer-token te verkrijgen, moet de eind gebruiker contact opnemen met het ondersteunings [team](mailto:support.ss@rsmuk.com) en kunnen zij het Bearer-token aan de klanten leveren.

2. Als u de URL van het SCIM-eind punt wilt ophalen, moet u de Insight4GRC-domein naam gereed hebben, omdat deze wordt gebruikt om uw SCIM-eind punt-URL te maken. U kunt uw Insight4GRC-domein naam ophalen als onderdeel van de oorspronkelijke software-aankoop met Insight4GRC.


## <a name="add-insight4grc--from-the-gallery"></a>Insight4GRC toevoegen vanuit de galerie

Als u Insight4GRC wilt configureren voor het automatisch inrichten van gebruikers met Azure AD, moet u Insight4GRC van de Azure AD-toepassings galerie toevoegen aan uw lijst met beheerde SaaS-toepassingen.

**Voer de volgende stappen uit om Insight4GRC toe te voegen vanuit de Azure AD-toepassings galerie:**

1. Selecteer in de **[Azure Portal](https://portal.azure.com)** in het navigatie venster links **Azure Active Directory**.

    ![De Azure Active Directory-knop](common/select-azuread.png)

2. Ga naar **bedrijfs toepassingen**en selecteer **alle toepassingen**.

    ![De blade Enterprise-toepassingen](common/enterprise-applications.png)

3. Als u een nieuwe toepassing wilt toevoegen, selecteert u de knop **nieuwe toepassing** boven aan het deel venster.

    ![De knop nieuwe toepassing](common/add-new-app.png)

4. Typ **Insight4GRC**in het zoekvak, selecteer **Insight4GRC** in het deel venster resultaten en klik vervolgens op de knop **toevoegen** om de toepassing toe te voegen.

    ![Insight4GRC in de lijst met resultaten](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-insight4grc"></a>Automatische gebruikers inrichting configureren voor Insight4GRC  

In deze sectie wordt u begeleid bij de stappen voor het configureren van de Azure AD-inrichtings service om gebruikers en/of groepen in Insight4GRC te maken, bij te werken en uit te scha kelen op basis van gebruikers-en/of groeps toewijzingen in azure AD.

> [!TIP]
> U kunt er ook voor kiezen om eenmalige aanmelding op basis van SAML in te scha kelen voor Insight4GRC, gevolgd door de instructies in de [zelf studie Insight4GRC-eenmalige aanmelding](insight4grc-tutorial.md). Eenmalige aanmelding kan onafhankelijk van automatische gebruikers inrichting worden geconfigureerd, hoewel deze twee functies elkaar behoeven.

### <a name="to-configure-automatic-user-provisioning-for-insight4grc--in-azure-ad"></a>Automatische gebruikers inrichting configureren voor Insight4GRC in azure AD:

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Selecteer **bedrijfs toepassingen**en selecteer **alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst toepassingen de optie **Insight4GRC**.

    ![De koppeling Insight4GRC in de lijst met toepassingen](common/all-applications.png)

3. Selecteer het tabblad **inrichten** .

    ![Tabblad inrichten](common/provisioning.png)

4. Stel de **inrichtings modus** in op **automatisch**.

    ![Tabblad inrichten](common/provisioning-automatic.png)

5.  Typ in de sectie beheerders referenties de `https://{Insight4GRC Domain Name}.insight4grc.com/public/api/scim/v2` invoer in de **Tenant-URL** met de waarde {Insight4GRC domain name} die eerder is opgehaald. Voer de **token waarde** in die eerder is opgehaald in het **geheime token**. Klik op **verbinding testen** om te controleren of Azure AD verbinding kan maken met Insight4GRC. Als de verbinding mislukt, zorg er dan voor dat uw Insight4GRC-account beheerders machtigingen heeft en probeer het opnieuw.

    ![Tenant-URL + token](common/provisioning-testconnection-tenanturltoken.png)

6. Voer in het veld **e-mail melding** het e-mail adres in van een persoon of groep die de inrichtings fout meldingen moet ontvangen en schakel het selectie vakje in om **een e-mail bericht te verzenden wanneer er een fout optreedt**.

    ![E-mailmelding](common/provisioning-notification-email.png)

7. Klik op **Opslaan**.

8. Selecteer in de sectie **toewijzingen** de optie **Azure Active Directory gebruikers synchroniseren met Insight4GRC**.

    ![Insight4GRC-gebruikers toewijzingen](media/insight4grc-provisioning-tutorial/usermapping.png)

9. Controleer de gebruikers kenmerken die zijn gesynchroniseerd vanuit Azure AD naar Insight4GRC in de sectie **kenmerk toewijzing** . De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt om te voldoen aan de gebruikers accounts in Insight4GRC voor bijwerk bewerkingen. Selecteer de knop **Opslaan** om eventuele wijzigingen door te voeren.

    ![Insight4GRC-gebruikers toewijzingen](media/insight4grc-provisioning-tutorial/userattribute.png)

10. Selecteer in de sectie **toewijzingen** de optie **Azure Active Directory groep synchroniseren naar Insight4GRC**

    ![Insight4GRC-groeps toewijzingen](media/insight4grc-provisioning-tutorial/groupmapping.png)

11. Controleer de groeps kenmerken die zijn gesynchroniseerd vanuit Azure AD naar Insight4GRC in de sectie **kenmerk toewijzing** . De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt om te voldoen aan de groeps accounts in Insight4GRC voor bijwerk bewerkingen. Selecteer de knop **Opslaan** om eventuele wijzigingen door te voeren.

    ![Insight4GRC-groeps toewijzingen](media/insight4grc-provisioning-tutorial/groupattribute.png)

10. Raadpleeg de volgende instructies in de [zelf studie](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)voor het filteren op bereik voor het configureren van bereik filters.

11. Als u de Azure AD-inrichtings service voor Insight4GRC wilt inschakelen, wijzigt u de inrichtings **status** in in het gedeelte **instellingen** .

    ![Inrichtings status inschakelt op](common/provisioning-toggle-on.png)

12. Definieer de gebruikers en/of groepen die u wilt inrichten voor Insight4GRC door de gewenste waarden in het **bereik** te kiezen in de sectie **instellingen** .

    ![Inrichtings bereik](common/provisioning-scope.png)

15. Wanneer u klaar bent om in te richten, klikt u op **Opslaan**.

    ![Inrichtings configuratie opslaan](common/provisioning-configuration-save.png)

Met deze bewerking wordt de eerste synchronisatie gestart van alle gebruikers en/of groepen die in het **bereik** zijn gedefinieerd in de sectie **instellingen** . Het duurt langer voordat de initiële synchronisatie is uitgevoerd dan volgende synchronisaties, die ongeveer elke 40 minuten optreden, zolang de Azure AD-inrichtings service wordt uitgevoerd. U kunt de sectie **synchronisatie Details** gebruiken om de voortgang te bewaken en koppelingen naar het rapport inrichtings activiteiten te volgen, waarin alle acties worden beschreven die worden uitgevoerd door de Azure AD Provisioning-Service op Insight4GRC.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).


## <a name="additional-resources"></a>Aanvullende resources

* Het [inrichten van een gebruikers account voor zakelijke apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md).
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en het ophalen van rapporten over inrichtings activiteiten](../manage-apps/check-status-user-account-provisioning.md).