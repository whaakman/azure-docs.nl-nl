---
title: 'Zelfstudie: Azure Active Directory-integratie met Citrix GoToMeeting | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Citrix GoToMeeting.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0f59fedb-2cf8-48d2-a5fb-222ed943ff78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 1ddfcd991431a11e5c3e306bd5905003d094ac18
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-citrix-gotomeeting-for-automatic-user-provisioning"></a>Zelfstudie: Citrix GoToMeeting configureren voor het automatisch gebruikers inrichten

Het doel van deze zelfstudie is zodat u de stappen die u wilt uitvoeren in Citrix GoToMeeting en Azure AD automatisch inrichten en de gebruikersaccounts van Azure AD naar Citrix GoToMeeting inrichten.

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure Active directory-tenant.
*   Een Citrix GoToMeeting eenmalige aanmelding ingeschakeld abonnement.
*   Een gebruikersaccount in Citrix GoToMeeting met beheerdersmachtigingen Team.

## <a name="assigning-users-to-citrix-gotomeeting"></a>Gebruikers toewijzen aan Citrix GoToMeeting

Azure Active Directory gebruikt een concept 'toewijzingen' genoemd om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van automatische gebruikers account inrichten, alleen de gebruikers en groepen die '' tot een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureren en inschakelen van de inrichting service, moet u om te bepalen welke gebruikers en/of groepen in Azure AD vertegenwoordigen de gebruikers die toegang tot uw app Citrix GoToMeeting nodig hebben. Als besloten, kunt u deze gebruikers toewijzen aan uw app Citrix GoToMeeting door de volgende instructies te volgen:

[Een gebruiker of groep toewijzen aan een enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-citrix-gotomeeting"></a>Belangrijke tips voor het toewijzen van gebruikers aan Citrix GoToMeeting

*   Het is raadzaam om één Azure AD-gebruiker is toegewezen aan Citrix GoToMeeting voor het testen van de configuratie van de inrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Wanneer een gebruiker aan Citrix GoToMeeting toewijzen, moet u een geldige gebruikersrol selecteren. De rol 'Default toegang' werkt niet voor het inrichten.

## <a name="enable-automated-user-provisioning"></a>Geautomatiseerde Gebruikersinrichting inschakelen

Deze sectie helpt u bij het verbinden van uw Azure AD met Citrix GoToMeeting van gebruikersaccount inrichten API en configureren van de inrichting service te maken, bijwerken en uitschakelen van de toegewezen gebruiker accounts in Citrix GoToMeeting op basis van gebruikers en groepen toewijzen in Azure AD.

> [!TIP]
> U kunt ook op basis van SAML eenmalige aanmelding is ingeschakeld voor Citrix GoToMeeting, vindt u de instructies te volgen in [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies aanvulling van elkaar.

### <a name="to-configure-automatic-user-account-provisioning"></a>Voor het configureren van automatische account gebruikersaanvragen:

1. In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory > zakelijke Apps > alle toepassingen** sectie.

2. Als u Citrix GoToMeeting al hebt geconfigureerd voor eenmalige aanmelding, zoekt u uw exemplaar van Citrix GoToMeeting met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar **Citrix GoToMeeting** in de galerie met toepassingen. Citrix GoToMeeting selecteert in de zoekresultaten en toe te voegen aan uw lijst met toepassingen.

3. Selecteer uw exemplaar van Citrix GoToMeeting en selecteer vervolgens de **inrichten** tabblad.

4. Stel de **inrichting** modus **automatische**. 

    ![Inrichting](./media/active-directory-saas-citrixgotomeeting-provisioning-tutorial/provisioning.png)

5. In de sectie beheerdersreferenties, moet u de volgende stappen uitvoeren:
   
    a. In de **Citrix GoToMeeting Beheerdersgebruikersnaam** textbox, typ de gebruikersnaam van een beheerder.

    b. In de **Citrix GoToMeeting beheerderswachtwoord** textbox, het administrator wachtwoord.

6. Klik in de Azure-portal op **testverbinding** om te controleren of Azure AD, kan verbinding maken met uw app Citrix GoToMeeting. Als de verbinding is mislukt, zorg je account Citrix GoToMeeting Team beheerdersmachtigingen heeft en probeer de **'Beheerdersreferenties'** stap opnieuw.

7. Voer het e-mailadres van een persoon of groep die in inrichting fout meldingen moet ontvangen de **e-mailmelding** veld en schakel het selectievakje in.

8. Klik op **opslaan.**

9. Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory-gebruikers Citrix GoToMeeting.**

10. In de **kenmerktoewijzingen** sectie, moet u de kenmerken van de gebruiker van Azure AD worden gesynchroniseerd met Citrix GoToMeeting controleren. De kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen overeenkomen met de gebruikersaccounts in Citrix GoToMeeting voor update-bewerkingen worden gebruikt. Selecteer de knop Opslaan eventuele wijzigingen doorvoeren.

11. Om de Azure AD-service voor Citrix GoToMeeting inricht, wijzigen de **inrichting Status** naar **op** in de sectie instellingen

12. Klik op **opslaan.**

De initiële synchronisatie van gebruikers en/of groepen die zijn toegewezen aan Citrix GoToMeeting in de sectie gebruikers en groepen wordt gestart. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer 20 minuten optreden als de service wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en volg de koppelingen voor het inrichten van de activiteitsrapporten, waarin alle acties die worden uitgevoerd door de inrichting service op uw app Citrix GoToMeeting beschrijven.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Het beheren van gebruikers account inrichten voor zakelijke Apps](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Eenmalige aanmelding configureren](active-directory-saas-citrix-gotomeeting-tutorial.md)


