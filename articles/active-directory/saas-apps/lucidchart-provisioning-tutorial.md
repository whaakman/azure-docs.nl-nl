---
title: 'Zelfstudie: LucidChart configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en inrichting ongedaan maken-gebruikersaccounts met LucidChart.
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
ms.openlocfilehash: f9e037e1be36a38631028e5e179e20720d3476a1
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56190854"
---
# <a name="tutorial-configure-lucidchart-for-automatic-user-provisioning"></a>Zelfstudie: LucidChart configureren voor het automatisch inrichten van gebruikers


Het doel van deze zelfstudie is om weer te geven u de stappen die u uitvoeren in LucidChart en Azure AD wilt voor het automatisch inrichten en inrichting van gebruikersaccounts vanuit Azure AD naar LucidChart ongedaan maken. 

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure Active directory-tenant
*   Een tenant LucidChart met de [Enterprise-plan](https://www.lucidchart.com/user/117598685#/subscriptionLevel) of beter ingeschakeld 
*   Een gebruikersaccount in LucidChart met beheerdersmachtigingen 

## <a name="assigning-users-to-lucidchart"></a>Gebruikers toewijzen aan LucidChart

Azure Active Directory maakt gebruik van een concept genaamd "toewijzingen" om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het inrichten van automatische gebruikersaccounts, wordt alleen de gebruikers en groepen die '' aan een toepassing in Azure AD toegewezen zijn gesynchroniseerd. 

Voordat u configureren en inschakelen van de inrichtingsservice, moet u om te bepalen welke gebruikers en/of groepen in Azure AD de gebruikers die toegang nodig tot uw app LucidChart vertegenwoordigen. Als besloten, kunt u deze gebruikers toewijzen aan uw app LucidChart door de instructies hier:

[Een gebruiker of groep toewijzen aan een enterprise-app](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-lucidchart"></a>Belangrijke tips voor het toewijzen van gebruikers aan LucidChart

*   Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan LucidChart voor het testen van de configuratie van de inrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Wanneer een gebruiker aan LucidChart toewijzen, moet u ofwel de **gebruiker** functie of een andere geldige toepassingsspecifieke-rol (indien beschikbaar) in het dialoogvenster toewijzing. De **standaardtoegang** rol werkt niet voor het inrichten en deze gebruikers worden overgeslagen.


## <a name="configuring-user-provisioning-to-lucidchart"></a>Inrichten van gebruikers naar LucidChart configureren 

In deze sectie helpt u bij uw Azure AD verbinden met de LucidChart gebruikersaccount Inrichtings-API en configureren van de provisioning-service voor het maken, bijwerken en uitschakelen van toegewezen gebruikersaccounts in LucidChart op basis van gebruikers en groepen in Azure AD.

> [!TIP]
> U kunt ook op SAML gebaseerde eenmalige aanmelding ingeschakeld voor LucidChart, vindt u de instructies te volgen in [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies een fraaie aanvulling in elkaar.


### <a name="configure-automatic-user-account-provisioning-to-lucidchart-in-azure-ad"></a>Automatisch inrichten van gebruikersaccounts aan LucidChart in Azure AD configureren


1. In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory > Bedrijfsapps > alle toepassingen** sectie.

2. Als u al LucidChart hebt geconfigureerd voor eenmalige aanmelding, zoeken naar uw exemplaar van LucidChart met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar de **LucidChart** in de toepassingengalerie. Selecteer LucidChart in de resultaten voor zoeken en toe te voegen aan uw lijst met toepassingen.

3. Selecteer uw exemplaar van LucidChart en selecteer vervolgens de **Provisioning** tabblad.

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![LucidChart Provisioning](./media/lucidchart-provisioning-tutorial/LucidChart1.png)

5. Onder de **beheerdersreferenties** sectie, voer de **geheim Token** die worden gegenereerd door de account van uw LucidChart (u vindt het token voor uw account: **Team** > **App-integratie** > **SCIM**). 

    ![LucidChart Provisioning](./media/lucidchart-provisioning-tutorial/LucidChart2.png)

6. Klik in de Azure-portal op **testverbinding** om te controleren of Azure AD kunt verbinden met uw app LucidChart. Als de verbinding is mislukt, zorg ervoor dat uw account LucidChart beheerdersmachtigingen heeft en probeer het opnieuw stap 5.

7. Voer het e-mailadres van een persoon of groep die inrichting fout meldingen moet ontvangen de **e-mailmelding** veld en schakel het selectievakje "Een e-mailmelding verzenden wanneer een fout optreedt."

8. Klik op **Opslaan**. 

9. Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory: gebruikers aan LucidChart**.

10. In de **kenmerktoewijzingen** sectie, controleert u de kenmerken van de gebruiker die van Azure AD worden gesynchroniseerd naar LucidChart. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in LucidChart voor update-bewerkingen. Selecteer de knop Opslaan om door te voeren van eventuele wijzigingen.

11. Wijzigen zodat de Azure AD-inrichtingsservice voor LucidChart de **Inrichtingsstatus** naar **op** in de **instellingen** sectie

12. Klik op **Opslaan**. 

Met deze bewerking wordt gestart voor de initiële synchronisatie van alle gebruikers en/of groepen die zijn toegewezen aan LucidChart in de sectie gebruikers en groepen. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de service wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van activiteitenlogboeken, waarin alle acties die worden uitgevoerd door de inrichtingsservice worden beschreven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).


## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../manage-apps/check-status-user-account-provisioning.md)
