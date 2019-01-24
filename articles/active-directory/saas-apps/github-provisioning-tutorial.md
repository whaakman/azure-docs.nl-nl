---
title: 'Zelfstudie: GitHub configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en verwijdering van gebruikersaccounts naar GitHub.
services: active-directory
documentationcenter: ''
author: asmalser-msft
writer: asmalser-msft
manager: daveba
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: asmalser-msft
ms.openlocfilehash: 7d24e11a0361e5f01e688179680587131eedc01a
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54823807"
---
# <a name="tutorial-configure-github-for-automatic-user-provisioning"></a>Zelfstudie: GitHub voor het automatisch inrichten van gebruikers configureren


Het doel van deze zelfstudie is om weer te geven u de stappen die u uitvoeren in GitHub en Azure AD wilt om automatisch inrichten en verwijdering van gebruikersaccounts vanuit Azure AD naar GitHub. 

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure Active directory-tenant
*   Een GitHub-tenant met de [zakelijke Cloud-plan](https://help.github.com/articles/organization-billing-plans/#github-business-cloud) of beter ingeschakeld 
*   Een gebruikersaccount in GitHub met beheerdersmachtigingen 

> [!NOTE]
> De integratie wordt ingericht op Azure AD is afhankelijk van de [GitHub SCIM API](https://developer.github.com/v3/scim/), die beschikbaar zijn voor GitHub-teams van het plan zakelijke of hoger is.

## <a name="assigning-users-to-github"></a>Gebruikers toewijzen aan GitHub

Azure Active Directory maakt gebruik van een concept genaamd "toewijzingen" om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het inrichten van automatische gebruikersaccounts, wordt alleen de gebruikers en groepen die '' aan een toepassing in Azure AD toegewezen zijn gesynchroniseerd. 

Voordat u configureren en inschakelen van de inrichtingsservice, moet u bepalen welke gebruikers en/of groepen in Azure AD vertegenwoordigen de gebruikers die toegang nodig tot uw GitHub-app. Als besloten, kunt u deze gebruikers toewijzen aan uw GitHub-app door de instructies hier:

[Een gebruiker of groep toewijzen aan een enterprise-app](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-github"></a>Belangrijke tips voor het toewijzen van gebruikers met GitHub

*   Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan GitHub voor het testen van de configuratie van de inrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Bij het toewijzen van een gebruiker met GitHub, moet u ofwel de **gebruiker** functie of een andere geldige toepassingsspecifieke-rol (indien beschikbaar) in het dialoogvenster toewijzing. De **standaardtoegang** rol werkt niet voor het inrichten en deze gebruikers worden overgeslagen.


## <a name="configuring-user-provisioning-to-github"></a>Configuratie van gebruikers inrichten met GitHub 

In deze sectie helpt u bij uw Azure AD verbinden met de GitHub-gebruikersaccount Inrichtings-API en configureren van de provisioning-service voor het maken, bijwerken en uitschakelen van toegewezen gebruikersaccounts in GitHub op basis van gebruikers en groepen in Azure AD.

> [!TIP]
> U kunt ook op SAML gebaseerde eenmalige aanmelding ingeschakeld voor GitHub, vindt u de instructies te volgen in [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies een fraaie aanvulling in elkaar.


### <a name="configure-automatic-user-account-provisioning-to-github-in-azure-ad"></a>Automatisch inrichten van gebruikersaccounts naar GitHub in Azure AD configureren


1. In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory > Bedrijfsapps > alle toepassingen** sectie.

2. Als u al GitHub hebt geconfigureerd voor eenmalige aanmelding, zoeken naar uw exemplaar van GitHub met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar de **GitHub** in de toepassingengalerie. Selecteer GitHub in de resultaten voor zoeken en toe te voegen aan uw lijst met toepassingen.

3. Selecteer uw exemplaar van GitHub en selecteer vervolgens de **Provisioning** tabblad.

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Het inrichten van GitHub](./media/github-provisioning-tutorial/GitHub1.png)

5. Onder de **beheerdersreferenties** sectie, klikt u op **autoriseren**. Met deze bewerking wordt een GitHub-autorisatie-dialoogvenster geopend in een nieuw browservenster. 

6. In het nieuwe venster, zich aanmelden bij GitHub met uw beheerdersaccount. Selecteer in het dialoogvenster resulterende autorisatie het GitHub-team dat u inschakelen wilt voor inrichting, en selecteer vervolgens **autoriseren**. Als voltooid, terug naar de Azure-portal om de configuratie van de inrichting te voltooien.

    ![Autorisatie-dialoogvenster](./media/github-provisioning-tutorial/GitHub2.png)

7. Voer in de Azure-portal **Tenant-URL** en klikt u op **testverbinding** om te controleren of Azure AD kunt verbinden met uw GitHub-app. Als de verbinding is mislukt, controleert u of uw GitHub-account beheerdersmachtigingen heeft en **Tenant-URl** correct is ingevoerde en probeer het opnieuw de stap 'Autoriseren' (u kunt vormen **Tenant-URL** door regel: `https://api.github.com/scim/v2/organizations/<Organization_name>` , u kunt uw organisaties vinden onder uw GitHub-account: **Instellingen voor** > **organisaties**).

    ![Autorisatie-dialoogvenster](./media/github-provisioning-tutorial/GitHub3.png)

8. Voer het e-mailadres van een persoon of groep die inrichting fout meldingen moet ontvangen de **e-mailmelding** veld en schakel het selectievakje "Een e-mailmelding verzenden wanneer een fout optreedt."

9. Klik op **Opslaan**. 

10. Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory: gebruikers naar GitHub**.

11. In de **kenmerktoewijzingen** sectie, controleert u de kenmerken van de gebruiker die worden gesynchroniseerd vanuit Azure AD naar GitHub. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in GitHub voor update-bewerkingen. Selecteer de knop Opslaan om door te voeren van eventuele wijzigingen.

12. Wijzigen zodat de Azure AD-inrichtingsservice voor GitHub, de **Inrichtingsstatus** naar **op** in de **instellingen** sectie

13. Klik op **Opslaan**. 

Met deze bewerking wordt gestart voor de initiële synchronisatie van alle gebruikers en/of groepen die zijn toegewezen aan GitHub in de sectie gebruikers en groepen. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de service wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van activiteitenlogboeken, waarin alle acties die worden uitgevoerd door de inrichtingsservice worden beschreven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).


## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../manage-apps/check-status-user-account-provisioning.md)
