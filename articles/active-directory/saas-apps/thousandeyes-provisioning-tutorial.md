---
title: 'Zelfstudie: ThousandEyes configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en inrichting ongedaan maken-gebruikersaccounts met ThousandEyes.
services: active-directory
documentationcenter: ''
author: asmalser-msft
writer: asmalser-msft
manager: daveba
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: asmalser-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: 74b9b39cfc6ac760c41b58c050cb1ebf39d3f93a
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56180926"
---
# <a name="tutorial-configure-thousandeyes-for-automatic-user-provisioning"></a>Zelfstudie: ThousandEyes configureren voor het automatisch inrichten van gebruikers


Het doel van deze zelfstudie is om weer te geven u de stappen die u uitvoeren in ThousandEyes en Azure AD wilt voor het automatisch inrichten en inrichting van gebruikersaccounts vanuit Azure AD naar ThousandEyes ongedaan maken. 

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure Active directory-tenant
*   Een tenant ThousandEyes met de [Standard-abonnement](https://www.thousandeyes.com/pricing) of beter ingeschakeld 
*   Een gebruikersaccount in ThousandEyes met beheerdersmachtigingen 

> [!NOTE]
> De integratie wordt ingericht op Azure AD is afhankelijk van de [ThousandEyes SCIM API](https://success.thousandeyes.com/PublicArticlePage?articleIdParam=kA044000000CnWrCAK), die beschikbaar zijn voor teams ThousandEyes Standard-abonnement of hoger is.

## <a name="assigning-users-to-thousandeyes"></a>Gebruikers toewijzen aan ThousandEyes

Azure Active Directory maakt gebruik van een concept genaamd "toewijzingen" om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het inrichten van automatische gebruikersaccounts, wordt alleen de gebruikers en groepen die '' aan een toepassing in Azure AD toegewezen zijn gesynchroniseerd. 

Voordat u configureren en inschakelen van de inrichtingsservice, moet u om te bepalen welke gebruikers en/of groepen in Azure AD de gebruikers die toegang nodig tot uw app ThousandEyes vertegenwoordigen. Als besloten, kunt u deze gebruikers toewijzen aan uw app ThousandEyes door de instructies hier:

[Een gebruiker of groep toewijzen aan een enterprise-app](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-thousandeyes"></a>Belangrijke tips voor het toewijzen van gebruikers aan ThousandEyes

*   Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan ThousandEyes voor het testen van de configuratie van de inrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Wanneer een gebruiker aan ThousandEyes toewijzen, moet u ofwel de **gebruiker** functie of een andere geldige toepassingsspecifieke-rol (indien beschikbaar) in het dialoogvenster toewijzing. De **standaardtoegang** rol werkt niet voor het inrichten en deze gebruikers worden overgeslagen.


## <a name="configuring-user-provisioning-to-thousandeyes"></a>Inrichten van gebruikers naar ThousandEyes configureren 

In deze sectie helpt u bij uw Azure AD verbinden met de ThousandEyes gebruikersaccount Inrichtings-API, en configureren van de provisioning-service voor het maken, bijwerken en uitschakelen van toegewezen gebruikersaccounts in ThousandEyes op basis van gebruikers en groepen in Azure AD .

> [!TIP]
> U kunt ook op SAML gebaseerde eenmalige aanmelding ingeschakeld voor ThousandEyes, vindt u de instructies te volgen in [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies een fraaie aanvulling in elkaar.


### <a name="configure-automatic-user-account-provisioning-to-thousandeyes-in-azure-ad"></a>Automatisch inrichten van gebruikersaccounts aan ThousandEyes in Azure AD configureren


1. In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory > Bedrijfsapps > alle toepassingen** sectie.

2. Als u al ThousandEyes hebt geconfigureerd voor eenmalige aanmelding, zoeken naar uw exemplaar van ThousandEyes met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar de **ThousandEyes** in de toepassingengalerie. Selecteer ThousandEyes in de resultaten voor zoeken en toe te voegen aan uw lijst met toepassingen.

3. Selecteer uw exemplaar van ThousandEyes en selecteer vervolgens de **Provisioning** tabblad.

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![ThousandEyes Provisioning](./media/thousandeyes-provisioning-tutorial/ThousandEyes1.png)

5. Onder de **beheerdersreferenties** sectie, voer de **OAuth Bearer Token** die worden gegenereerd door de account van uw ThousandEyes (u kunt vinden en of een token genereren voor uw account ThousandEyes  **Profiel** sectie).

    ![ThousandEyes Provisioning](./media/thousandeyes-provisioning-tutorial/ThousandEyes2.png)

6. Klik in de Azure-portal op **testverbinding** om te controleren of Azure AD kunt verbinden met uw app ThousandEyes. Als de verbinding is mislukt, zorg ervoor dat uw account ThousandEyes beheerdersmachtigingen heeft en probeer het opnieuw stap 5.

7. Voer het e-mailadres van een persoon of groep die inrichting fout meldingen moet ontvangen de **e-mailmelding** veld en schakel het selectievakje "Een e-mailmelding verzenden wanneer een fout optreedt."

8. Klik op **Opslaan**. 

9. Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory: gebruikers aan ThousandEyes**.

10. In de **kenmerktoewijzingen** sectie, controleert u de kenmerken van de gebruiker die van Azure AD worden gesynchroniseerd naar ThousandEyes. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in ThousandEyes voor update-bewerkingen. Selecteer de knop Opslaan om door te voeren van eventuele wijzigingen.

11. Wijzigen zodat de Azure AD-inrichtingsservice voor ThousandEyes de **Inrichtingsstatus** naar **op** in de **instellingen** sectie

12. Klik op **Opslaan**. 

Met deze bewerking wordt gestart voor de initiële synchronisatie van alle gebruikers en/of groepen die zijn toegewezen aan ThousandEyes in de sectie gebruikers en groepen. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de service wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van activiteitenlogboeken, waarin alle acties die worden uitgevoerd door de inrichtingsservice worden beschreven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).


## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../manage-apps/check-status-user-account-provisioning.md)
