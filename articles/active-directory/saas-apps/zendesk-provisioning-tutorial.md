---
title: 'Zelfstudie: Zendesk configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en inrichting ongedaan maken met Zendesk-gebruikersaccounts.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd-msft
ms.assetid: na
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: v-ant
ms.openlocfilehash: d8d6df221a8c520cea5e6e938447ffd881e7374c
ms.sourcegitcommit: e37fa6e4eb6dbf8d60178c877d135a63ac449076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53322769"
---
# <a name="tutorial-configure-zendesk-for-automatic-user-provisioning"></a>Zelfstudie: Zendesk voor het automatisch inrichten van gebruikers configureren

Het doel van deze zelfstudie is ter illustratie van de stappen om te worden uitgevoerd in de Zendesk en Azure Active Directory (Azure AD) voor Azure AD configureren voor het automatisch inrichten en de inrichting ongedaan maken van gebruikers en/of groepen met Zendesk. 

> [!NOTE]
> Deze zelfstudie beschrijft een connector die is gebaseerd op de Provisioning-Service van Azure AD-gebruiker. Zie voor belangrijke informatie over wat deze service biedt, hoe het werkt en veelgestelde vragen [automatiseren van gebruikersinrichting en -opheffing in SaaS-toepassingen met Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende vereisten hebt:

*   Een Azure AD-tenant
*   Een Zendesk-tenant met de [Enterprise](https://www.zendesk.com/product/pricing/) plannen of beter ingeschakeld 
*   Een gebruikersaccount in Zendesk met beheerdersmachtigingen 

> [!NOTE]
> De integratie wordt ingericht op Azure AD is afhankelijk van de [Zendesk Rest-API](https://developer.zendesk.com/rest_api/docs/core/introduction), die beschikbaar zijn voor Zendesk-teams van het Enterprise-plan of hoger is.

## <a name="adding-zendesk-from-the-gallery"></a>Zendesk uit de galerie toe te voegen
Voordat u Zendesk configureert voor automatisch gebruikers inrichten met Azure AD, moet u Zendesk uit de galerie met Azure AD toevoegen aan uw lijst met beheerde SaaS-toepassingen.

**Als u wilt toevoegen Zendesk uit de galerie met Azure AD, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op de **Azure Active Directory** pictogram. 

    ![De knop Azure Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen** > **alle toepassingen**.

    ![De sectie voor bedrijfstoepassingen][2]
    
3. Als u wilt toevoegen Zendesk, klikt u op de **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

4. Typ in het zoekvak **Zendesk**.

    ![Zendesk-inrichting](./media/zendesk-provisioning-tutorial/ZenDesk6.png)

5. Selecteer in het deelvenster resultaten **Zendesk**, en klik vervolgens op de **toevoegen** knop Zendesk toevoegen aan uw lijst met SaaS-toepassingen.

    ![Zendesk-inrichting](./media/zendesk-provisioning-tutorial/ZenDesk7.png)

    ![Zendesk-inrichting](./media/zendesk-provisioning-tutorial/ZenDesk20.png)

## <a name="assigning-users-to-zendesk"></a>Gebruikers toewijzen aan Zendesk

Azure Active Directory maakt gebruik van een concept genaamd "toewijzingen" om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers, worden alleen de gebruikers en/of groepen die '' aan een toepassing in Azure AD toegewezen zijn gesynchroniseerd. 

Voordat u configureren en inschakelen van automatische inrichten van gebruikers, moet u bepalen welke gebruikers en/of groepen in Azure AD toegang hebben tot Zendesk moeten. Wanneer u beslist, kunt u deze gebruikers en/of groepen toewijzen aan Zendesk door de instructies hier:

*   [Een gebruiker of groep toewijzen aan een enterprise-app](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-zendesk"></a>Belangrijke tips voor het toewijzen van gebruikers met Zendesk

*    Zendesk-functies worden dynamisch en automatisch ingevuld in de gebruikersinterface van Azure portal vandaag nog. Voordat u Zendesk-rollen toewijzen aan gebruikers, moet een eerste synchronisatie is voltooid op basis van Zendesk om op te halen van de meest recente rollen in uw Zendesk-tenant.

*    Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan Zendesk voor het testen van uw eerste automatisch gebruikers inrichten van configuratie. Extra gebruikers en/of groepen kunnen worden toegewezen later als de tests geslaagd zijn.
  
*   Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan Zendesk voor het testen van de configuratie van de automatische gebruikersinrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Bij het toewijzen van een gebruiker met Zendesk, moet u een geldige toepassingsspecifieke-rol (indien beschikbaar) in het dialoogvenster toewijzing selecteren. Gebruikers met de **standaardtoegang** rol worden uitgesloten van het inrichten.

## <a name="configuring-automatic-user-provisioning-to-zendesk"></a>Configuratie van automatisch gebruikers inrichten met Zendesk 

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD-inrichtingsservice als u wilt maken, bijwerken en uitschakelen van gebruikers en/of groepen in Zendesk op basis van gebruiker en/of groep toewijzingen in Azure AD.

> [!TIP]
> U kunt er ook voor kiezen om in te schakelen op SAML gebaseerde eenmalige aanmelding voor Zendesk, vindt u de instructies te volgen in de [één Zendesk aanmeldings-zelfstudie](zendesk-tutorial.md). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatisch gebruikers inrichten, hoewel deze twee functies een fraaie aanvulling in elkaar.

### <a name="to-configure-automatic-user-provisioning-for-zendesk-in-azure-ad"></a>Het configureren van automatisch gebruikers inrichten voor Zendesk in Azure AD:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en blader naar **Azure Active Directory > bedrijfstoepassingen > alle toepassingen**.

2. Selecteer Zendesk in uw lijst met SaaS-toepassingen.
 
    ![Zendesk-inrichting](./media/zendesk-provisioning-tutorial/ZenDesk3.png)

3. Selecteer de **Provisioning** tabblad.
    
    ![Zendesk-inrichting](./media/zendesk-provisioning-tutorial/ZenDesk16.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Zendesk-inrichting](./media/zendesk-provisioning-tutorial/ZenDesk1.png)

5. Onder de **beheerdersreferenties** sectie, voer de **Admin Username**, **geheim Token**, en **domein** van van uw Zendesk-account. Voorbeelden van deze waarden zijn:

    *   In de **Admin Username** veld, de gebruikersnaam van het beheerdersaccount dat op uw Zendesk-tenant te vullen. Voorbeeld: admin@contoso.com.

    *   In de **geheim Token** veld, vul het token voor geheim zoals beschreven in stap 6.

    *   In de **domein** veld, vullen het subdomein van uw Zendesk-tenant.
    Voorbeeld: Voor een account met een tenant-URL van https://my-tenant.zendesk.com, zou uw subdomein **mijn tenant**.

6. De **geheim Token** voor uw Zendesk-account zich bevindt in **Admin > API > instellingen**. 

    ![Het inrichten van Zendesk](./media/zendesk-provisioning-tutorial/ZenDesk4.png) ![Zendesk inrichten](./media/zendesk-provisioning-tutorial/ZenDesk2.png)

7. Bij het invullen van de velden die in stap 5 wordt weergegeven, klikt u op **testverbinding** om te controleren of Azure AD verbinding kunt maken met Zendesk. Als de verbinding is mislukt, zorg ervoor dat uw Zendesk-account beheerdersmachtigingen heeft en probeer het opnieuw.

    ![Zendesk-inrichting](./media/zendesk-provisioning-tutorial/ZenDesk19.png)
    
8. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet de inrichting fout ontvangen en schakel het selectievakje in - **een e-mailmelding verzenden wanneer een foutoptreedt**.

    ![Zendesk-inrichting](./media/zendesk-provisioning-tutorial/ZenDesk9.png)

9. Klik op **Opslaan**.

10. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory: gebruikers met Zendesk**.

    ![Zendesk-inrichting](./media/zendesk-provisioning-tutorial/ZenDesk10.png)

11. Controleer de kenmerken van de gebruiker die worden gesynchroniseerd vanuit Azure AD met Zendesk in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in Zendesk voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

    ![Zendesk-inrichting](./media/zendesk-provisioning-tutorial/ZenDesk11.png)

12. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory-groepen met ZenDesk**.

    ![Zendesk-inrichting](./media/zendesk-provisioning-tutorial/ZenDesk12.png)

13. Bekijk de groepskenmerken die worden gesynchroniseerd vanuit Azure AD met Zendesk in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de groepen in Zendesk voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

    ![Zendesk-inrichting](./media/zendesk-provisioning-tutorial/ZenDesk13.png)

14. Als u wilt configureren bereikfilters, raadpleegt u de volgende instructies in de [Scoping filter zelfstudie](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

15. Als wilt inschakelen in de Azure AD-inrichtingsservice voor Zendesk, wijzigt de **Inrichtingsstatus** naar **op** in de **instellingen** sectie.

    ![Zendesk-inrichting](./media/zendesk-provisioning-tutorial/ZenDesk14.png)

16. De gebruikers en/of groepen die u wilt inrichten met Zendesk definiëren door het kiezen van de gewenste waarden in **bereik** in de **instellingen** sectie.

    ![Zendesk-inrichting](./media/zendesk-provisioning-tutorial/ZenDesk15.png)

17. Wanneer u klaar om in te richten bent, klikt u op **opslaan**.

    ![Zendesk-inrichting](./media/zendesk-provisioning-tutorial/ZenDesk18.png)


Met deze bewerking wordt gestart voor de initiële synchronisatie van alle gebruikers en/of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de Azure AD-inrichtingsservice wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van activiteitenrapport, die alle acties die worden uitgevoerd door de Azure AD-inrichtingsservice in Zendesk wordt beschreven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Connector-beperkingen
* Zendesk ondersteunt het gebruik van groepen voor gebruikers met alleen de rollen voor de Agent. Raadpleeg voor meer informatie, [van Zendesk-documentatie](https://support.zendesk.com/hc/en-us/articles/203661966-Creating-managing-and-using-groups).

## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/zendesk-tutorial/tutorial_general_01.png
[2]: ./media/zendesk-tutorial/tutorial_general_02.png
[3]: ./media/zendesk-tutorial/tutorial_general_03.png
