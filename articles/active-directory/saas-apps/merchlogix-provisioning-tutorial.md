---
title: 'Zelfstudie: MerchLogix configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en inrichting ongedaan maken-gebruikersaccounts met MerchLogix.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd-msft
ms.assetid: na
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/01/2019
ms.author: zhchia
ms.collection: M365-identity-device-management
ms.openlocfilehash: fe36969661ae1b729601681c02f79e777b2f8cab
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57344802"
---
# <a name="tutorial-configure-merchlogix-for-automatic-user-provisioning"></a>Zelfstudie: MerchLogix configureren voor het automatisch inrichten van gebruikers

Het doel van deze zelfstudie is ter illustratie van de stappen om te worden uitgevoerd in de MerchLogix en Azure Active Directory (Azure AD) naar Azure AD configureren voor automatisch inrichten en verwijdering van gebruikers en/of groepen aan MerchLogix. 

> [!NOTE]
> Deze zelfstudie beschrijft een connector die is gebaseerd op de Provisioning-Service van Azure AD-gebruiker. Zie voor belangrijke informatie over wat deze service biedt, hoe het werkt en veelgestelde vragen [automatiseren van gebruikersinrichting en -opheffing in SaaS-toepassingen met Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende vereisten hebt:

*   Een Azure AD-tenant
*   Een tenant MerchLogix
*   Een technische contactpersoon op MerchLogix die bieden kan de SCIM eindpunt-URL en geheim token vereist voor het inrichten van gebruikers

## <a name="adding-merchlogix-from-the-gallery"></a>MerchLogix uit de galerie toe te voegen
Voordat u MerchLogix configureert voor automatisch gebruikers inrichten met Azure AD, moet u MerchLogix uit de galerie met Azure AD toevoegen aan uw lijst met beheerde SaaS-toepassingen.

**Als u wilt toevoegen MerchLogix uit de galerie met Azure AD, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op de **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen** > **alle toepassingen**.

    ![De sectie voor bedrijfstoepassingen][2]
    
3. Als u wilt toevoegen MerchLogix, klikt u op de **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **MerchLogix**.

5. Selecteer in het deelvenster resultaten **MerchLogix**, en klik vervolgens op de **toevoegen** knop MerchLogix toevoegen aan uw lijst met SaaS-toepassingen.

    ![MerchLogix inrichten][4]

## <a name="assigning-users-to-merchlogix"></a>Gebruikers toewijzen aan MerchLogix

Azure Active Directory maakt gebruik van een concept genaamd "toewijzingen" om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers, worden alleen de gebruikers en/of groepen die '' aan een toepassing in Azure AD toegewezen zijn gesynchroniseerd. 

Voordat u configureren en inschakelen van automatische inrichten van gebruikers, moet u bepalen welke gebruikers en/of groepen in Azure AD toegang hebben tot MerchLogix moeten. Wanneer u beslist, kunt u deze gebruikers en/of groepen toewijzen aan MerchLogix door de instructies hier:

*   [Een gebruiker of groep toewijzen aan een enterprise-app](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-merchlogix"></a>Belangrijke tips voor het toewijzen van gebruikers aan MerchLogix

*    Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan MerchLogix voor het testen van uw eerste automatisch gebruikers inrichten van configuratie. Extra gebruikers en/of groepen kunnen worden toegewezen later als de tests geslaagd zijn.

*   Wanneer een gebruiker aan MerchLogix toewijzen, moet u alle geldige toepassingsspecifieke rollen (indien beschikbaar) selecteren in het dialoogvenster toewijzing. Gebruikers met de **standaardtoegang** rol worden uitgesloten van het inrichten.

## <a name="configuring-automatic-user-provisioning-to-merchlogix"></a>Automatisch gebruikers inrichten voor MerchLogix configureren 

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD-inrichtingsservice als u wilt maken, bijwerken en uitschakelen van gebruikers en/of groepen in MerchLogix op basis van gebruiker en/of groep toewijzingen in Azure AD.

> [!TIP]
> U kunt er ook voor kiezen om in te schakelen op SAML gebaseerde eenmalige aanmelding voor MerchLogix, vindt u de instructies te volgen in de [één MerchLogix aanmeldings-zelfstudie](merchlogix-tutorial.md). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatisch gebruikers inrichten, hoewel deze twee functies een fraaie aanvulling in elkaar.

### <a name="to-configure-automatic-user-provisioning-for-merchlogix-in-azure-ad"></a>Het configureren van automatisch gebruikers inrichten voor MerchLogix in Azure AD:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en blader naar **Azure Active Directory > bedrijfstoepassingen > alle toepassingen**.

2. Selecteer MerchLogix uit de lijst met SaaS-toepassingen.

3. Selecteer de **Provisioning** tabblad.

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![MerchLogix inrichten](./media/merchlogix-provisioning-tutorial/Merchlogix1.png)

5. Onder de **beheerdersreferenties** sectie:

    *   In de **Tenant-URL** en voer de eindpunt-URL van het SCIM geleverd door uw MerchLogix technische contactpersoon.

    *   In de **geheim Token** Voer geheime token dat is opgegeven door de MerchLogix technische contactpersoon.

6. Bij het invullen van de velden die in stap 5 wordt weergegeven, klikt u op **testverbinding** om te controleren of Azure AD kunt verbinden met MerchLogix. Als de verbinding is mislukt, zorg ervoor dat uw account MerchLogix beheerdersmachtigingen heeft en probeer het opnieuw.

    
7. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet de inrichting fout ontvangen en schakel het selectievakje in - **een e-mailmelding verzenden wanneer een foutoptreedt**.

8. Klik op **Opslaan**.

9. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory: gebruikers aan MerchLogix**.

10. Controleer de kenmerken van de gebruiker die van Azure AD worden gesynchroniseerd naar MerchLogix in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in MerchLogix voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

11. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory-groepen aan MerchLogix**.

12. Bekijk de groepskenmerken die worden gesynchroniseerd vanuit Azure AD naar MerchLogix in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de groepen in MerchLogix voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

13. Wijzigen zodat de Azure AD-inrichtingsservice voor MerchLogix de **Inrichtingsstatus** naar **op** in de **instellingen** sectie.

14. Wanneer u klaar om in te richten bent, klikt u op **opslaan**.


Met deze bewerking wordt gestart voor de initiële synchronisatie van alle gebruikers en/of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de Azure AD-inrichtingsservice wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van rapport van de activiteit, die alle acties die worden uitgevoerd door de Azure AD-inrichtingsservice op MerchLogix beschrijft.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: common/select-azuread.png
[2]: common/enterprise-applications.png
[3]: common/add-new-app.png
[4]: common/search-new-app.png
