---
title: 'Zelfstudie: Drie Zscaler configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: In deze zelfstudie leert u hoe u Azure Active Directory voor het automatisch inrichten en inrichting ongedaan maken van gebruikersaccounts met Zscaler drie configureert.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: 385a1153-0f47-4e41-8f44-da1b49d7629e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: v-ant-msft
ms.openlocfilehash: d96444984c503da68ccbda3aef9fea0ede5c7ff9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "67048987"
---
# <a name="tutorial-configure-zscaler-three-for-automatic-user-provisioning"></a>Zelfstudie: Drie Zscaler configureren voor het automatisch inrichten van gebruikers

In deze zelfstudie leert u hoe u Azure Active Directory (Azure AD) voor het automatisch inrichten en de inrichting van gebruikers en/of groepen die moeten worden drie Zscaler configureert.

> [!NOTE]
> Deze zelfstudie beschrijft een connector die gebouwd op de Azure AD-gebruiker-service inricht. Voor belangrijke informatie over wat deze service heeft en hoe het werkt en op veelgestelde vragen antwoorden, Zie [automatiseren van gebruikersinrichting en -opheffing in SaaS-toepassingen met Azure Active Directory](../active-directory-saas-app-provisioning.md).
>
> Deze connector is momenteel in openbare Preview. Zie voor meer informatie over de algemene Azure gebruiksvoorwaarden voor Preview-functies, [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Vereisten

Als u wilt de stappen in deze zelfstudie hebt voltooid, hebt u het volgende nodig:

* Een Azure AD-tenant.
* Een drie Zscaler-tenant.
* Een gebruikersaccount in drie Zscaler met beheerdersmachtigingen.

> [!NOTE]
> De integratie van Azure AD-inrichting is afhankelijk van de Zscaler ZSCloud SCIM API, die beschikbaar is voor Enterprise-accounts.

## <a name="adding-zscaler-three-from-the-gallery"></a>Uit de galerie toe te voegen drie Zscaler

Voordat u drie Zscaler voor automatisch gebruikers inrichten met Azure AD configureren, moet u drie Zscaler toevoegen uit de galerie met Azure AD aan uw lijst met beheerde SaaS-toepassingen.

In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster **Azure Active Directory**:

![Selecteer Azure Active Directory](common/select-azuread.png)

Ga naar **bedrijfstoepassingen** en selecteer vervolgens **alle toepassingen**:

![Bedrijfstoepassingen](common/enterprise-applications.png)

Als u wilt een toepassing hebt toegevoegd, selecteert u **nieuwe toepassing** aan de bovenkant van het venster:

![Nieuwe toepassing selecteren](common/add-new-app.png)

Voer in het zoekvak **Zscaler drie**. Selecteer **Zscaler drie** in de resultaten en selecteer vervolgens **toevoegen**.

![Lijst met resultaten](common/search-new-app.png)

## <a name="assign-users-to-zscaler-three"></a>Gebruikers toewijzen aan drie Zscaler

Azure AD-gebruikers moeten worden toegang tot geselecteerde apps toegewezen voordat ze deze kunnen gebruiken. In de context van het automatisch inrichten van gebruikers, worden alleen de gebruikers of groepen die zijn toegewezen aan een toepassing in Azure AD gesynchroniseerd.

Voordat u configureren en automatische inrichting inschakelen, moet u bepalen welke gebruikers en/of groepen in Azure AD toegang hebben tot drie Zscaler moeten. Nadat u hebt besloten dat, kunt u deze gebruikers en groepen op drie Zscaler toewijzen volgens de instructies in [een gebruiker of groep toewijzen aan een enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

### <a name="important-tips-for-assigning-users-to-zscaler-three"></a>Belangrijke tips voor het toewijzen van gebruikers aan drie Zscaler

* Het wordt aangeraden dat u eerst een enkel toewijzen aan drie Zscaler voor het testen van de configuratie van de automatische gebruikersinrichting Azure AD-gebruiker. U kunt meer gebruikers en groepen later toewijzen.

* Wanneer u een gebruiker aan Zscaler drie toewijzen, moet u alle geldige toepassingsspecifieke rollen (indien beschikbaar) in het dialoogvenster toewijzing te selecteren. Gebruikers met de **standaardtoegang** rol worden uitgesloten van het inrichten.

## <a name="set-up-automatic-user-provisioning"></a>Automatische inrichting instellen

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD-inrichtingsservice als u wilt maken, bijwerken en uitschakelen van gebruikers en groepen in Zscaler drie op basis van gebruiker en toewijzingen van groepen in Azure AD.

> [!TIP]
> U kunt ook op SAML gebaseerde eenmalige aanmelding voor drie Zscaler inschakelen. Als u dit doet, volg de instructies in de [Zscaler drie eenmalige aanmelding zelfstudie](zscaler-three-tutorial.md). Eenmalige aanmelding kan onafhankelijk van automatisch inrichten van gebruikers worden geconfigureerd, maar de twee functies elkaar aanvullen.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en selecteer **bedrijfstoepassingen** > **alle toepassingen** > **Zscaler drie**:

    ![Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Zscaler drie**:

    ![Lijst Toepassingen](common/all-applications.png)

3. Selecteer de **Provisioning** tabblad:

    ![Zscaler drie inrichten](./media/zscaler-three-provisioning-tutorial/provisioning-tab.png)

4. Stel de **Inrichtingsmodus** naar **automatische**:

    ![Instellen van de Inrichtingsmodus](./media/zscaler-three-provisioning-tutorial/provisioning-credentials.png)

5. In de **beheerdersreferenties** sectie, voer de **Tenant-URL** en **geheim Token** van uw Zscaler drie account, zoals beschreven in de volgende stap.

6. Aan de **Tenant-URL** en **geheim Token**, gaat u naar **beheer** > **verificatie-instellingen** in de Zscaler Drie portal en selecteer **SAML** onder **verificatietype**:

    ![Zscaler drie verificatie-instellingen](./media/zscaler-three-provisioning-tutorial/secret-token-1.png)

    Selecteer **configureren SAML** openen de **configureren SAML** venster:

    ![SAML-venster configureren](./media/zscaler-three-provisioning-tutorial/secret-token-2.png)

    Selecteer **Enable SCIM-Based inrichting** en kopieer de **basis-URL** en **Bearer Token**, en sla vervolgens de instellingen. Plak in de Azure-portal, de **basis-URL** in de **Tenant-URL** vak en de **Bearer Token** in de **geheim Token** vak.

7. Nadat u de waarden in de **Tenant-URL** en **geheim Token** vakken, selecteer **testverbinding** om ervoor te zorgen dat Azure AD kan verbinding maken met Zscaler drie. Als de verbinding is mislukt, zorg ervoor dat uw Zscaler drie account beheerdersmachtigingen heeft en probeer het opnieuw.

    ![De verbinding testen](./media/zscaler-three-provisioning-tutorial/test-connection.png)

8. In de **e-mailmelding** voert u het e-mailadres van een persoon of groep die de inrichting fout meldingen moet ontvangen. Selecteer **e-mailmelding verzenden wanneer er een fout optreedt**:

    ![E-mailmelding instellen](./media/zscaler-three-provisioning-tutorial/notification.png)

9. Selecteer **Opslaan**.

10. In de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory: gebruikers aan ZscalerThree**:

    ![Azure AD-gebruikers synchroniseren](./media/zscaler-three-provisioning-tutorial/user-mappings.png)

11. Controleer de kenmerken van de gebruiker die van Azure AD worden gesynchroniseerd met Zscaler drie in de **kenmerktoewijzingen** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in drie Zscaler voor update-bewerkingen. Selecteer **opslaan** doorvoeren van wijzigingen.

    ![Kenmerktoewijzingen](./media/zscaler-three-provisioning-tutorial/user-attribute-mappings.png)

12. In de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory-groepen aan ZscalerThree**:

    ![Azure AD-groepen synchroniseren](./media/zscaler-three-provisioning-tutorial/group-mappings.png)

13. Bekijk de groepskenmerken die van Azure AD worden gesynchroniseerd met Zscaler drie in de **kenmerktoewijzingen** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de groepen in drie Zscaler voor update-bewerkingen. Selecteer **opslaan** doorvoeren van wijzigingen.

    ![Kenmerktoewijzingen](./media/zscaler-three-provisioning-tutorial/group-attribute-mappings.png)

14. Als u wilt configureren bereikfilters, raadpleegt u de instructies in de [Scoping filter zelfstudie](./../active-directory-saas-scoping-filters.md).

15. Wijzigen zodat de Azure AD-inrichtingsservice voor drie Zscaler de **Inrichtingsstatus** naar **op** in de **instellingen** sectie:

    ![Inrichtingsstatus](./media/zscaler-three-provisioning-tutorial/provisioning-status.png)

16. De gebruikers en/of groepen die u wilt definiëren om in te richten op drie Zscaler door het kiezen van de waarden die u wilt gebruiken onder **bereik** in de **instellingen** sectie:

    ![Waarden voor het bereik](./media/zscaler-three-provisioning-tutorial/scoping.png)

17. Wanneer u klaar om in te richten bent, selecteert u **opslaan**:

    ![Opslaan selecteren](./media/zscaler-three-provisioning-tutorial/save-provisioning.png)

Met deze bewerking wordt gestart voor de initiële synchronisatie van alle gebruikers en groepen die zijn gedefinieerd onder **bereik** in de **instellingen** sectie. De eerste synchronisatie duurt langer dan het volgende wordt gesynchroniseerd, die zich over elke 40 minuten, voordoen zolang het Azure AD-inrichtingsservice wordt uitgevoerd. U kunt de voortgang bewaken de **synchronisatiedetails** sectie. U kunt ook koppelingen naar een rapport over de inrichtingsactiviteit, waarin wordt beschreven alle acties die worden uitgevoerd door de Azure AD-inrichtingsservice op drie Zscaler volgen.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/zscaler-three-provisioning-tutorial/tutorial-general-01.png
[2]: ./media/zscaler-three-provisioning-tutorial/tutorial-general-02.png
[3]: ./media/zscaler-three-provisioning-tutorial/tutorial-general-03.png
