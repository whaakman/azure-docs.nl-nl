---
title: 'Zelfstudie: Azure Active Directory-integratie met Jive | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Jive.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6fbfdbe7-d66c-4305-9fea-76d6a6a92830
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 957b152fdd40d08a867e788b0cb9f7d57ed481e4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-jive-for-user-provisioning"></a>Zelfstudie: Jive configureren voor gebruikers inrichten

Het doel van deze zelfstudie is zodat u de stappen die u wilt uitvoeren in Jive en Azure AD aan automatisch leveren en intrekken gebruikersaccounts vanuit Azure AD te Jive.

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure Active directory-tenant.
*   Een Jive eenmalige aanmelding ingeschakeld abonnement.
*   Een gebruikersaccount in Jive met beheerdersmachtigingen Team.

## <a name="assigning-users-to-jive"></a>Gebruikers toewijzen aan Jive

Azure Active Directory gebruikt een concept 'toewijzingen' genoemd om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van automatische gebruikers account inrichten, alleen de gebruikers en groepen die '' tot een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureren en inschakelen van de inrichting service, moet u om te bepalen welke gebruikers en/of groepen in Azure AD vertegenwoordigen de gebruikers die toegang tot uw app Jive nodig hebben. Als besloten, kunt u deze gebruikers toewijzen aan uw app Jive door de volgende instructies te volgen:

[Een gebruiker of groep toewijzen aan een enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-jive"></a>Belangrijke tips voor het toewijzen van gebruikers aan Jive

*   Het is raadzaam om één Azure AD-gebruiker worden toegewezen aan Jive voor het testen van de configuratie van de inrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Wanneer een gebruiker aan Jive toewijzen, moet u een geldige gebruikersrol selecteren. De rol 'Default toegang' werkt niet voor het inrichten.

## <a name="enable-user-provisioning"></a>Gebruikersinrichting inschakelen

Deze sectie helpt u bij het verbinding maken met uw Azure AD Jive van gebruikersaccount inrichten API en configureren van de inrichting service te maken, bijwerken en uitschakelen van toegewezen gebruikersaccounts in Jive op basis van gebruikers en groepen toewijzen in Azure AD.

> [!TIP]
> U kunt ook op basis van SAML Single Sign-On for Jive is ingeschakeld, vindt u de instructies te volgen in [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies aanvulling van elkaar.

### <a name="to-configure-user-account-provisioning"></a>Configureren voor het inrichten van het account:

Het doel van deze sectie is het inschakelen van de gebruiker het inrichten van Active Directory-gebruikersaccounts met Jive overzicht.
Als onderdeel van deze procedure moet zijn u vereist voor het beveiligingstoken van een gebruiker moet u aanvragen via Jive.com.

1. In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory > zakelijke Apps > alle toepassingen** sectie.

2. Als u al Jive voor eenmalige aanmelding hebt geconfigureerd, kunt u zoeken naar uw exemplaar van Jive met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar **Jive** in de galerie met toepassingen. Jive selecteert in de zoekresultaten en toe te voegen aan uw lijst met toepassingen.

3. Selecteer uw exemplaar van Jive en selecteer vervolgens de **inrichten** tabblad.

4. Stel de **Inrichtingsmodus** naar **automatische**. 

    ![Inrichting](./media/active-directory-saas-jive-provisioning-tutorial/provisioning.png)

5. Onder de **beheerdersreferenties** sectie, bieden de volgende configuratie-instellingen:
   
    a. In de **Jive Beheerdersgebruikersnaam** textbox type een Jive accountnaam met de **systeembeheerder** profiel in Jive.com toegewezen.
   
    b. In de **Jive beheerderswachtwoord** textbox, typt u het wachtwoord voor dit account.
   
    c. In de **Jive Tenant-URL** textbox, typ de URL van de tenant Jive.
      
      > [!NOTE]
      > De URL van de tenant Jive is de URL die wordt gebruikt door uw organisatie zich aanmelden bij Jive.  
      > Normaal gesproken de URL heeft de volgende indeling: **www.\< organisatie\>. jive.com**.          

6. Klik in de Azure-portal op **testverbinding** om te controleren of Azure AD, kan verbinding maken met uw app Jive.

7. Voer het e-mailadres van een persoon of groep die in inrichting fout meldingen moet ontvangen de **e-mailmelding** veld en schakel het selectievakje hieronder in.

8. Klik op **opslaan.**

9. Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory-gebruikers Jive.**

10. In de **kenmerktoewijzingen** sectie, moet u de kenmerken van de gebruiker is gesynchroniseerd vanuit Azure AD Jive controleren. De kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen overeenkomen met de gebruikersaccounts in Jive voor update-bewerkingen worden gebruikt. Selecteer de knop Opslaan eventuele wijzigingen doorvoeren.

11. Om de Azure AD-service voor Jive inricht, wijzigen de **inrichting Status** naar **op** in de sectie instellingen

12. Klik op **opslaan.**

De initiële synchronisatie van gebruikers en/of groepen die zijn toegewezen aan Jive in de sectie gebruikers en groepen wordt gestart. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer 20 minuten optreden als de service wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en volg de koppelingen voor het inrichten van de activiteitsrapporten, waarin alle acties die worden uitgevoerd door de inrichting service op uw app Jive beschrijven.

U kunt nu een testaccount maken. Wacht 20 minuten duren om te verifiëren dat het account is gesynchroniseerd voor Jive.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Het beheren van gebruikers account inrichten voor zakelijke Apps](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Eenmalige aanmelding configureren](active-directory-saas-jive-tutorial.md)