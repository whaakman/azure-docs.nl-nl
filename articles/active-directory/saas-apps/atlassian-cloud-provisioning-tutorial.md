---
title: 'Zelfstudie: Atlassian Cloud configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en inrichting van gebruikersaccounts naar Atlassian Cloud ongedaan maken.
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
ms.date: 03/03/2019
ms.author: v-ant
ms.openlocfilehash: ca9a569d28e42baafeabc15f49fb7f5206566730
ms.sourcegitcommit: dd1a9f38c69954f15ff5c166e456fda37ae1cdf2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57572290"
---
# <a name="tutorial-configure-atlassian-cloud-for-automatic-user-provisioning"></a>Zelfstudie: Atlassian Cloud configureren voor het automatisch inrichten van gebruikers

Het doel van deze zelfstudie is ter illustratie van de stappen in de Atlassian-Cloud en Azure Active Directory (Azure AD) voor Azure AD configureren automatisch inrichten en de inrichting ongedaan maken van gebruikers en/of groepen aan Atlassian Cloud worden uitgevoerd. 

> [!NOTE]
> Deze zelfstudie beschrijft een connector die is gebaseerd op de Provisioning-Service van Azure AD-gebruiker. Zie voor belangrijke informatie over wat deze service biedt, hoe het werkt en veelgestelde vragen [automatiseren van gebruikersinrichting en -opheffing in SaaS-toepassingen met Azure Active Directory](../manage-apps/user-provisioning.md).

> Deze connector is momenteel in openbare Preview. Zie voor meer informatie over de algemene Microsoft Azure gebruiksvoorwaarden voor Preview-functies, [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende vereisten hebt:

*   Een Azure AD-tenant
*   [Een Atlassian Cloud-tenant](https://www.atlassian.com/licensing/cloud)
*   Een gebruikersaccount in de Atlassian Cloud met beheerdersmachtigingen.

> [!NOTE]
> De integratie wordt ingericht op Azure AD is afhankelijk van de **Atlassian Cloud SCIM API**, die voor teams van Atlassian Cloud beschikbaar is.

## <a name="adding-atlassian-cloud-from-the-gallery"></a>Atlassian Cloud toevoegen vanuit de galerie
Voordat het Atlassian Cloud configureren voor automatisch gebruikers inrichten met Azure AD, moet u Atlassian Cloud uit de galerie met Azure AD toevoegen aan uw lijst met beheerde SaaS-toepassingen.

**Als u wilt toevoegen Atlassian Cloud uit de galerie met Azure AD, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op de **Azure Active Directory** pictogram. 

    ![De knop Azure Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen** > **alle toepassingen**.

    ![De sectie voor bedrijfstoepassingen][2]
    
3. Als u wilt toevoegen Atlassian Cloud, klikt u op de **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

4. Typ in het zoekvak **Atlassian Cloud**.

    ![Cloudinrichting-Atlassian](./media/atlassian-cloud-provisioning-tutorial/app-search.png)

5. Selecteer in het deelvenster resultaten **Atlassian Cloud**, en klik vervolgens op de **toevoegen** knop Atlassian Cloud toevoegen aan uw lijst met SaaS-toepassingen.

    ![Cloudinrichting-Atlassian](./media/atlassian-cloud-provisioning-tutorial/app-create.png)

    ![Cloudinrichting-Atlassian](./media/atlassian-cloud-provisioning-tutorial/app-instance.png)

## <a name="assigning-users-to-atlassian-cloud"></a>Gebruikers toewijzen aan Atlassian Cloud

Azure Active Directory maakt gebruik van een concept genaamd "toewijzingen" om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers, worden alleen de gebruikers en/of groepen die '' aan een toepassing in Azure AD toegewezen zijn gesynchroniseerd. 

Voordat u configureren en inschakelen van automatische inrichten van gebruikers, moet u bepalen welke gebruikers en/of groepen in Azure AD toegang hebben tot de Atlassian-Cloud moeten. Wanneer u beslist, kunt u deze gebruikers en/of groepen toewijzen aan Atlassian Cloud door de instructies hier:

*   [Een gebruiker of groep toewijzen aan een enterprise-app](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-atlassian-cloud"></a>Belangrijke tips voor het toewijzen van gebruikers naar de Atlassian Cloud

*   Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan Atlassian Cloud voor het testen van de configuratie van de automatische gebruikersinrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Wanneer een gebruiker toewijzen aan Atlassian-Cloud, moet u alle geldige toepassingsspecifieke rollen (indien beschikbaar) selecteren in het dialoogvenster toewijzing. Gebruikers met de **standaardtoegang** rol worden uitgesloten van het inrichten.

## <a name="configuring-automatic-user-provisioning-to-atlassian-cloud"></a>Automatisch gebruikers inrichten naar Atlassian Cloud configureren 

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD-inrichtingsservice als u wilt maken, bijwerken en uitschakelen van gebruikers en/of groepen in de Atlassian Cloud op basis van gebruiker en/of toewijzingen van groepen in Azure AD.

> [!TIP]
> U kunt er ook voor kiezen om in te schakelen op SAML gebaseerde eenmalige aanmelding voor Atlassian Cloud, vindt u de instructies te volgen in de [één Atlassian Cloud aanmeldings-zelfstudie](atlassian-cloud-tutorial.md). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatisch gebruikers inrichten, hoewel deze twee functies een fraaie aanvulling in elkaar.

### <a name="to-configure-automatic-user-provisioning-for-atlassian-cloud-in-azure-ad"></a>Het configureren van automatisch gebruikers inrichten voor Atlassian Cloud in Azure AD:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en blader naar **Azure Active Directory > bedrijfstoepassingen > alle toepassingen**.

2. Selecteer Atlassian Cloud in uw lijst met SaaS-toepassingen.
 
    ![Cloudinrichting-Atlassian](./media/atlassian-cloud-provisioning-tutorial/application-instance-search.png)

3. Selecteer de **Provisioning** tabblad.
    
    ![Cloudinrichting-Atlassian](./media/atlassian-cloud-provisioning-tutorial/provisioning-tab.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Cloudinrichting-Atlassian](./media/atlassian-cloud-provisioning-tutorial/credentials.png)

5. Onder de **beheerdersreferenties** sectie, voer de **Tenant-URL** en **geheim Token** van uw Atlassian-Cloud-account. Voorbeelden van deze waarden zijn:

    *   In de **Tenant-URL** veld, vult u het eindpunt voor de specifieke tenant u van de Atlassian ontvangen, zoals beschreven in stap 6. Bijvoorbeeld: **https://api.atlassian.com/scim/directory/{directoryId}**

    *   In de **geheim Token** veld, vul het token voor geheim zoals beschreven in stap 6.

6. Navigeer naar [Atlassian organisatie Manager](https://admin.atlassian.com) **> Gebruikersinrichting** en klikt u op **maken van een Token**. Kopiëren de **Directory basis-URL** en **Bearer Token** naar de **Tenant-URL** en **geheim Token** respectievelijk in velden.

    ![Cloudinrichting-Atlassian](./media/atlassian-cloud-provisioning-tutorial/secret-token-1.png) ![Cloudinrichting-Atlassian](./media/atlassian-cloud-provisioning-tutorial/secret-token-2.png)
    
    ![Cloudinrichting-Atlassian](./media/atlassian-cloud-provisioning-tutorial/secret-token-3.png)

7. Bij het invullen van de velden die in stap 5 wordt weergegeven, klikt u op **testverbinding** om te controleren of Azure AD kunt verbinden met Atlassian-Cloud. Als de verbinding is mislukt, zorg ervoor dat uw Atlassian Cloud-account beheerdersmachtigingen heeft en probeer het opnieuw.

    ![Cloudinrichting-Atlassian](./media/atlassian-cloud-provisioning-tutorial/test-connection.png)
    
8. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet de inrichting fout ontvangen en schakel het selectievakje in - **een e-mailmelding verzenden wanneer een foutoptreedt**.

    ![Cloudinrichting-Atlassian](./media/atlassian-cloud-provisioning-tutorial/notification.png)

9. Klik op **Opslaan**.

10. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory: gebruikers naar de Atlassian Cloud**.

    ![Cloudinrichting-Atlassian](./media/atlassian-cloud-provisioning-tutorial/provision-users.png)

11. Controleer de kenmerken van de gebruiker die worden gesynchroniseerd vanuit Azure AD naar Atlassian Cloud in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in de Atlassian Cloud voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

    ![Cloudinrichting-Atlassian](./media/atlassian-cloud-provisioning-tutorial/user-mapping.png)

12. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory-groepen naar de Atlassian Cloud**.

    ![Cloudinrichting-Atlassian](./media/atlassian-cloud-provisioning-tutorial/provision-groups.png)

13. Bekijk de groepskenmerken die worden gesynchroniseerd vanuit Azure AD naar Atlassian Cloud in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de groepen in de Atlassian Cloud voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

    ![Cloudinrichting-Atlassian](./media/atlassian-cloud-provisioning-tutorial/group-mapping.png)

14. Als u wilt configureren bereikfilters, raadpleegt u de volgende instructies in de [Scoping filter zelfstudie](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

15. Om in te schakelen in de Azure AD-inrichtingsservice voor Atlassian Cloud, wijzigen de **Inrichtingsstatus** naar **op** in de **instellingen** sectie.

    ![Cloudinrichting-Atlassian](./media/atlassian-cloud-provisioning-tutorial/provisioning-on.png)

16. De gebruikers en/of groepen die u wilt definiëren voor het inrichten van Atlassian Cloud door het kiezen van de gewenste waarden in **bereik** in de **instellingen** sectie.

    ![Cloudinrichting-Atlassian](./media/atlassian-cloud-provisioning-tutorial/provisioning-options.png)

17. Wanneer u klaar om in te richten bent, klikt u op **opslaan**.

    ![Cloudinrichting-Atlassian](./media/atlassian-cloud-provisioning-tutorial/save.png)


Met deze bewerking wordt gestart voor de initiële synchronisatie van alle gebruikers en/of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de Azure AD-inrichtingsservice wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van rapport van de activiteit, die alle acties die worden uitgevoerd door de Azure AD-inrichtingsservice op Atlassian Cloud beschrijft.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Connector-beperkingen

* Atlassian Cloud staat inrichting van gebruikers kunnen alleen [geverifieerde domeinen](https://confluence.atlassian.com/cloud/organization-administration-938859734.html).
* Atlassian Cloud ondersteunt geen naam van de groep vandaag nog. Dit betekent dat eventuele wijzigingen in de displayName op van een groep in Azure AD niet worden bijgewerkt en in de Atlassian-Cloud weergegeven.
* De waarde van de **e-mail** gebruikerskenmerk in Azure AD wordt alleen ingevuld als de gebruiker een Microsoft Exchange-postvak heeft. Als de gebruiker beschikt niet over een, is het raadzaam om toe te wijzen een andere gewenste kenmerk aan de **e-mailberichten** kenmerk in de Atlassian-Cloud.

## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/atlassian-cloud-provisioning-tutorial/tutorial-general-01.png
[2]: ./media/atlassian-cloud-provisioning-tutorial/tutorial-general-02.png
[3]: ./media/atlassian-cloud-provisioning-tutorial/tutorial-general-03.png
