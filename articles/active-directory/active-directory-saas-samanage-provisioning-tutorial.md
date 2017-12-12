---
title: 'Zelfstudie: Samanage configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en gebruikersaccounts aan Samanage ongedaan in te richten.
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
ms.openlocfilehash: 9e6e4fbccc225f0a457bb1fab53b75fe59074576
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-configuring-samanage-for-automatic-user-provisioning"></a>Zelfstudie: Samanage configureren voor het automatisch gebruikers inrichten


Het doel van deze zelfstudie is zodat u de stappen die u uitvoeren in Samanage en Azure AD wilt om automatisch in te richten en inrichten van gebruikersaccounts vanuit Azure AD naar Samanage ongedaan. 

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure Active directory-tenant
*   Een tenant Samanage met de [Professional plan](https://www.samanage.com/pricing/) of beter ingeschakeld 
*   Een gebruikersaccount in Samanage met beheerdersmachtigingen 

> [!NOTE]
> De Azure AD integratie inrichting is afhankelijk van de [Samanage REST-API](https://www.samanage.com/api/), die beschikbaar zijn voor Samanage teams op het plan Professional of een betere is.

## <a name="assigning-users-to-samanage"></a>Gebruikers toewijzen aan Samanage

Azure Active Directory gebruikt een concept 'toewijzingen' genoemd om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van automatische gebruikers account inrichten, alleen de gebruikers en groepen die '' tot een toepassing in Azure AD toegewezen zijn gesynchroniseerd. 

Voordat u configureren en inschakelen van de inrichting service, moet u om te bepalen welke gebruikers en/of groepen in Azure AD vertegenwoordigen de gebruikers die toegang tot uw app Samanage nodig hebben. Als besloten, kunt u deze gebruikers toewijzen aan uw app Samanage door de volgende instructies te volgen:

[Een gebruiker of groep toewijzen aan een enterprise-app](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-samanage"></a>Belangrijke tips voor het toewijzen van gebruikers aan Samanage

*   Het is raadzaam om één Azure AD-gebruiker is toegewezen aan Samanage voor het testen van de configuratie van de inrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Wanneer een gebruiker aan Samanage toewijzen, moet u ofwel de **gebruiker** functie of een andere geldige toepassingsspecifieke-rol (indien beschikbaar) in het dialoogvenster toewijzing. De **standaardtoegang** rol werkt niet voor het inrichten en deze gebruikers worden overgeslagen.

> [!NOTE]
> Als een toegevoegde functie en de inrichting service eventuele aangepaste rollen gedefinieerd in Samanage leest en importeert ze in Azure AD waarin ze kunnen worden geselecteerd in het dialoogvenster rol selecteren. Deze rollen zijn zichtbaar in de Azure portal nadat de inrichting-service is ingeschakeld en een synchronisatiecyclus is voltooid.

## <a name="configuring-user-provisioning-to-samanage"></a>Gebruikersaanvragen voor Samanage configureren 

Deze sectie helpt u bij het verbinding maken met uw Azure AD Samanage van gebruikersaccount inrichten API en configureren van de inrichting service te maken, bijwerken en uitschakelen van toegewezen gebruikersaccounts in Samanage op basis van gebruikers en groepen toewijzen in Azure AD.

> [!TIP]
> U kunt ook op basis van SAML eenmalige aanmelding is ingeschakeld voor Samanage, vindt u de instructies te volgen in [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies aanvulling van elkaar.


### <a name="configure-automatic-user-account-provisioning-to-samanage-in-azure-ad"></a>Configureer Automatische account gebruikersaanvragen naar Samanage in Azure AD:


1. In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory > zakelijke Apps > alle toepassingen** sectie.

2. Als u al Samanage voor eenmalige aanmelding hebt geconfigureerd, kunt u zoeken naar uw exemplaar van Samanage met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar **Samanage** in de galerie met toepassingen. Samanage selecteert in de zoekresultaten en toe te voegen aan uw lijst met toepassingen.

3. Selecteer uw exemplaar van Samanage en selecteer vervolgens de **inrichten** tabblad.

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Samanage inrichten](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage1.png)

5. Onder de **beheerdersreferenties** sectie, voer de **Admin Username & beheerderswachtwoord** van uw Samanage-account. 

6. Klik in de Azure-portal op **testverbinding** om te controleren of Azure AD, kan verbinding maken met uw app Samanage. Als de verbinding is mislukt, zorg ervoor dat uw account Samanage beheerdersmachtigingen heeft en probeer het opnieuw stap 5.

7. Voer het e-mailadres van een persoon of groep die in inrichting fout meldingen moet ontvangen de **e-mailmelding** veld en schakel het selectievakje "Een e-mailmelding verzenden wanneer een fout optreedt."

8. Klik op **Opslaan**. 

9. Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory-gebruikers Samanage**.

10. In de **kenmerktoewijzingen** sectie, moet u de kenmerken van de gebruiker is gesynchroniseerd vanuit Azure AD Samanage controleren. De kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen overeenkomen met de gebruikersaccounts in Samanage voor update-bewerkingen worden gebruikt. Selecteer de knop Opslaan eventuele wijzigingen doorvoeren.

11. Om de Azure AD-service voor Samanage inricht, wijzigen de **inrichting Status** naar **op** in de **instellingen** sectie

12. Klik op **Opslaan**. 

Deze bewerking begint de initiële synchronisatie van gebruikers en/of groepen die zijn toegewezen aan Samanage in de sectie gebruikers en groepen. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer 20 minuten optreden als de service wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en volg de koppelingen voor het inrichten van de activiteitsrapporten, waarin alle acties die worden uitgevoerd door de inrichting service beschrijven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over automatische account gebruikersaanvragen](https://docs.microsoft.com/azure/active-directory/active-directory-saas-provisioning-reporting).


## <a name="additional-resources"></a>Aanvullende bronnen

* [Het beheren van gebruikers account inrichten voor zakelijke Apps](active-directory-enterprise-apps-manage-provisioning.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Volgende stappen

* [Informatie over het bekijken van Logboeken en rapporten over het inrichten van de activiteit ophalen](active-directory-saas-provisioning-reporting.md)
