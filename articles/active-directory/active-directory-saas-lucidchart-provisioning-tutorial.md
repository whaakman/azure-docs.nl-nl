---
title: 'Zelfstudie: LucidChart configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en gebruikersaccounts aan LucidChart ongedaan in te richten.
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: mtillman
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: asmalser-msft
ms.openlocfilehash: be93059111202deca2e0582112ab8f09508d6c43
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-configuring-lucidchart-for-automatic-user-provisioning"></a>Zelfstudie: LucidChart configureren voor het automatisch gebruikers inrichten


Het doel van deze zelfstudie is zodat u de stappen die u uitvoeren in LucidChart en Azure AD wilt om automatisch in te richten en inrichten van gebruikersaccounts vanuit Azure AD naar LucidChart ongedaan. 

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure Active directory-tenant
*   Een tenant LucidChart met de [enterpriseplan](https://www.lucidchart.com/user/117598685#/subscriptionLevel) of beter ingeschakeld 
*   Een gebruikersaccount in LucidChart met beheerdersmachtigingen 

## <a name="assigning-users-to-lucidchart"></a>Gebruikers toewijzen aan LucidChart

Azure Active Directory gebruikt een concept 'toewijzingen' genoemd om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van automatische gebruikers account inrichten, alleen de gebruikers en groepen die '' tot een toepassing in Azure AD toegewezen zijn gesynchroniseerd. 

Voordat u configureren en inschakelen van de inrichting service, moet u om te bepalen welke gebruikers en/of groepen in Azure AD vertegenwoordigen de gebruikers die toegang tot uw app LucidChart nodig hebben. Als besloten, kunt u deze gebruikers toewijzen aan uw app LucidChart door de volgende instructies te volgen:

[Een gebruiker of groep toewijzen aan een enterprise-app](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-lucidchart"></a>Belangrijke tips voor het toewijzen van gebruikers aan LucidChart

*   Het is raadzaam om één Azure AD-gebruiker is toegewezen aan LucidChart voor het testen van de configuratie van de inrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Wanneer een gebruiker aan LucidChart toewijzen, moet u ofwel de **gebruiker** functie of een andere geldige toepassingsspecifieke-rol (indien beschikbaar) in het dialoogvenster toewijzing. De **standaardtoegang** rol werkt niet voor het inrichten en deze gebruikers worden overgeslagen.


## <a name="configuring-user-provisioning-to-lucidchart"></a>Gebruikersaanvragen voor LucidChart configureren 

Deze sectie helpt u bij het verbinding maken met uw Azure AD LucidChart van gebruikersaccount inrichten API en configureren van de inrichting service te maken, bijwerken en uitschakelen van toegewezen gebruikersaccounts in LucidChart op basis van gebruikers en groepen toewijzen in Azure AD.

> [!TIP]
> U kunt ook op basis van SAML eenmalige aanmelding is ingeschakeld voor LucidChart, vindt u de instructies te volgen in [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies aanvulling van elkaar.


### <a name="configure-automatic-user-account-provisioning-to-lucidchart-in-azure-ad"></a>Automatische account gebruikersaanvragen naar LucidChart in Azure AD configureren


1. In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory > zakelijke Apps > alle toepassingen** sectie.

2. Als u al LucidChart voor eenmalige aanmelding hebt geconfigureerd, kunt u zoeken naar uw exemplaar van LucidChart met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar **LucidChart** in de galerie met toepassingen. LucidChart selecteert in de zoekresultaten en toe te voegen aan uw lijst met toepassingen.

3. Selecteer uw exemplaar van LucidChart en selecteer vervolgens de **inrichten** tabblad.

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![LucidChart inrichten](./media/active-directory-saas-lucidchart-provisioning-tutorial/LucidChart1.png)

5. Onder de **beheerdersreferenties** sectie, voer de **geheim Token** die worden gegenereerd door uw LucidChart account (u vindt het token onder uw account: **Team** > **App-integratie** > **SCIM**). 

    ![LucidChart inrichten](./media/active-directory-saas-lucidchart-provisioning-tutorial/LucidChart2.png)

6. Klik in de Azure-portal op **testverbinding** om te controleren of Azure AD, kan verbinding maken met uw app LucidChart. Als de verbinding is mislukt, zorg ervoor dat uw account LucidChart beheerdersmachtigingen heeft en probeer het opnieuw stap 5.

7. Voer het e-mailadres van een persoon of groep die in inrichting fout meldingen moet ontvangen de **e-mailmelding** veld en schakel het selectievakje "Een e-mailmelding verzenden wanneer een fout optreedt."

8. Klik op **Opslaan**. 

9. Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory-gebruikers LucidChart**.

10. In de **kenmerktoewijzingen** sectie, moet u de kenmerken van de gebruiker is gesynchroniseerd vanuit Azure AD LucidChart controleren. De kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen overeenkomen met de gebruikersaccounts in LucidChart voor update-bewerkingen worden gebruikt. Selecteer de knop Opslaan eventuele wijzigingen doorvoeren.

11. Om de Azure AD-service voor LucidChart inricht, wijzigen de **inrichting Status** naar **op** in de **instellingen** sectie

12. Klik op **Opslaan**. 

Deze bewerking begint de initiële synchronisatie van gebruikers en/of groepen die zijn toegewezen aan LucidChart in de sectie gebruikers en groepen. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer 20 minuten optreden als de service wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en volg de koppelingen voor het inrichten van de activiteitsrapporten, waarin alle acties die worden uitgevoerd door de inrichting service beschrijven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over automatische account gebruikersaanvragen](https://docs.microsoft.com/azure/active-directory/active-directory-saas-provisioning-reporting).


## <a name="additional-resources"></a>Aanvullende bronnen

* [Het beheren van gebruikers account inrichten voor zakelijke Apps](active-directory-enterprise-apps-manage-provisioning.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Volgende stappen

* [Informatie over het bekijken van Logboeken en rapporten over het inrichten van de activiteit ophalen](active-directory-saas-provisioning-reporting.md)
