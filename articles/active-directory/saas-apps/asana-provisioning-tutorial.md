---
title: 'Zelfstudie: Asana configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en gebruikersaccounts aan Asana ongedaan in te richten.
services: active-directory
documentationcenter: ''
author: asmalser-msft
writer: asmalser-msft
manager: sakula
ms.assetid: 0b38ee73-168b-42cb-bd8b-9c5e5126d648
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: asmalser
ms.reviewer: asmalser
ms.openlocfilehash: 2abd10f90d2e6d86d7f82aa43016a31bfdbd324b
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36231420"
---
# <a name="tutorial-configure-asana-for-automatic-user-provisioning"></a>Zelfstudie: Asana configureren voor het automatisch gebruikers inrichten

Het doel van deze zelfstudie is zodat u de stappen die u uitvoeren in Asana en Azure Active Directory (Azure AD wilt) om automatisch in te richten en inrichten van gebruikersaccounts vanuit Azure AD naar Asana ongedaan.

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure AD-tenant
*   Een Asana-tenant met een [Enterprise](https://www.asana.com/pricing) plannen of beter ingeschakeld 
*   Een gebruikersaccount in Asana met beheerdersmachtigingen 

> [!NOTE] 
> Azure AD integratie inrichting is afhankelijk van de [Asana API](https://app.asana.com/api/1.0/scim/Users), die voor Asana beschikbaar is.

## <a name="assign-users-to-asana"></a>Gebruikers toewijzen aan Asana

Azure AD gebruikt een concept 'toewijzingen' genoemd om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van automatische gebruikers account inrichten, worden alleen de gebruikers die zijn toegewezen aan een toepassing in Azure AD gesynchroniseerd. 

Voordat u configureert en de inrichting-service inschakelen, moet u bepalen welke gebruikers in Azure AD moeten toegang hebben tot uw app Asana. Vervolgens kunt u deze gebruikers toewijzen aan uw app Asana door de volgende instructies te volgen:

[Een gebruiker toewijzen aan een enterprise-app](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-asana"></a>Belangrijke tips voor het toewijzen van gebruikers aan Asana

Het is raadzaam dat u één toewijzen aan Asana voor het testen van de configuratie van de inrichting Azure AD-gebruiker. Extra gebruikers kunnen later worden toegewezen.

## <a name="configure-user-provisioning-to-asana"></a>Gebruikersaanvragen voor Asana configureren 

Deze sectie helpt u bij het verbinding maken met uw Azure AD Asana gebruikersaccount inrichten API. U ook configureren de inrichting service te maken, bijwerken en uitschakelen van toegewezen gebruikersaccounts in Asana op basis van de toewijzingen van gebruikers in Azure AD.

> [!TIP]
> Als u wilt inschakelen op basis van SAML eenmalige aanmelding voor Asana, volg de instructies in de [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies elkaar aanvullen.

### <a name="to-configure-automatic-user-account-provisioning-to-asana-in-azure-ad"></a>Voor het configureren van automatische account gebruikersaanvragen naar Asana in Azure AD

1. In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory** > **zakelijke Apps** > **alle toepassingen** sectie.

2. Als u al Asana geconfigureerd voor eenmalige aanmelding, zoekt u uw exemplaar van Asana met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar **Asana** in de galerie met toepassingen. Selecteer **Asana** uit de lijst met zoekresultaten en toe te voegen aan de lijst met toepassingen.

3. Selecteer uw exemplaar van Asana, en selecteer vervolgens de **inrichten** tabblad.

4. Stel **Inrichtingsmodus** naar **automatische**.

    ![Asana inrichten](./media/asana-provisioning-tutorial/asanaazureprovisioning.png)

5. Onder de **beheerdersreferenties** opgevolgd, volgt u deze instructies voor het genereren van het token en voer deze in **geheim Token**:

    a. Aanmelden bij [Asana](https://app.asana.com) met behulp van uw beheerdersaccount.

    b. Selecteer de profielfoto in de bovenste balk en selecteer uw huidige instellingen voor de naam van de organisatie.

    c. Ga naar de **serviceaccounts** tabblad.

    d. Selecteer **Service-Account toevoegen**.

    e. Update **naam** en **over** en de profielfoto indien nodig. Kopieer het token in **Token**, en selecteer deze in **wijzigingen opslaan**.

6. Selecteer in de Azure-portal **testverbinding** om ervoor te zorgen dat Azure AD verbinding met uw app Asana maken kan. Als de verbinding is mislukt, zorg ervoor dat uw account Asana beheerdersmachtigingen heeft en probeer de **testverbinding** stap opnieuw.

7. Voer het e-mailadres van een persoon of groep die u meldingen wilt ontvangen inrichting fout in **e-mailmelding**. Schakel het selectievakje onder.

8. Selecteer **Opslaan**. 

9. Onder de **toewijzingen** sectie **synchroniseren Azure Active Directory-gebruikers Asana**.

10. In de **kenmerktoewijzingen** sectie, moet u de kenmerken van de gebruiker moeten worden gesynchroniseerd vanuit Azure AD naar Asana controleren. De kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen overeenkomen met de gebruikersaccounts in Asana voor update-bewerkingen worden gebruikt. Selecteer **opslaan** eventuele wijzigingen doorvoeren. Zie voor meer informatie [aanpassen gebruiker inrichten kenmerktoewijzingen](../active-directory-saas-customizing-attribute-mappings.md).

11. Inschakelen van de Azure AD-service voor Asana, inrichten in de **instellingen** sectie, wijzigt u **inrichting Status** naar **op**.

12. Selecteer **Opslaan**. 

Nu de initiële synchronisatie wordt uitgevoerd voor alle gebruikers die zijn toegewezen aan Asana in de **gebruikers** sectie. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de service wordt uitgevoerd. Gebruik de **synchronisatiedetails** sectie voortgang en volg de koppelingen voor het inrichten van activiteitenlogboeken. De controlelogboeken beschrijven alle acties die worden uitgevoerd door de inrichting van uw app Asana-service.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapporteren over automatische account gebruikersaanvragen](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Account gebruikersinrichting voor zakelijke Apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Eenmalige aanmelding configureren](asana-tutorial.md)
