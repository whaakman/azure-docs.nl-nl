---
title: 'Zelfstudie: Pingboard configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en gebruikersaccounts aan Pingboard ongedaan in te richten.
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
ms.date: 10/19/2017
ms.author: asmalser
ms.reviewer: asmalser
ms.openlocfilehash: 6d9562697c8135ba4b92b1bbf304a41af8c426c0
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36220380"
---
# <a name="tutorial-configure-pingboard-for-automatic-user-provisioning"></a>Zelfstudie: Pingboard configureren voor het automatisch gebruikers inrichten

Het doel van deze zelfstudie is om u de stappen die u uitvoeren moet om automatische inrichting en het opheffen van provisioning van gebruikersaccounts van Azure Active Directory (Azure AD) voor Pingboard weer te geven.

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure AD-tenant
*   Een tenant Pingboard [Pro-account](https://pingboard.com/pricing) 
*   Een gebruikersaccount in Pingboard met beheerdersmachtigingen 

> [!NOTE] 
> Azure AD integratie inrichting is afhankelijk van de [Pingboard API](`https://your_domain.pingboard.com/scim/v2`), die voor uw account beschikbaar is.

## <a name="assign-users-to-pingboard"></a>Gebruikers toewijzen aan Pingboard

Azure AD gebruikt een concept 'toewijzingen' genoemd om te bepalen welke gebruikers krijgen toegang tot de geselecteerde toepassingen. In de context van automatische gebruikers account inrichten, worden alleen de gebruikers die zijn toegewezen aan een toepassing in Azure AD gesynchroniseerd. 

Voordat u configureert en de inrichting-service inschakelen, moet u bepalen welke gebruikers in Azure AD moeten toegang hebben tot uw app Pingboard. Vervolgens kunt u deze gebruikers toewijzen aan uw app Pingboard door de volgende instructies te volgen:

[Een gebruiker toewijzen aan een enterprise-app](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-pingboard"></a>Belangrijke tips voor het toewijzen van gebruikers aan Pingboard

Het is raadzaam dat u één toewijzen aan Pingboard voor het testen van de configuratie van de inrichting Azure AD-gebruiker. Extra gebruikers kunnen later worden toegewezen.

## <a name="configure-user-provisioning-to-pingboard"></a>Gebruikersaanvragen voor Pingboard configureren 

In deze sectie helpt u bij uw Azure AD verbinden met het inrichten van de API Pingboard-gebruikersaccount. U ook configureren de inrichting service te maken, bijwerken en uitschakelen van toegewezen gebruikersaccounts in Pingboard op basis van de toewijzingen van gebruikers in Azure AD.

> [!TIP]
> Als u wilt inschakelen op basis van SAML eenmalige aanmelding voor Pingboard, volg de instructies in de [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies elkaar aanvullen.

### <a name="to-configure-automatic-user-account-provisioning-to-pingboard-in-azure-ad"></a>Voor het configureren van automatische account gebruikersaanvragen naar Pingboard in Azure AD

1. In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory** > **zakelijke Apps** > **alle toepassingen** sectie.

2. Als u al Pingboard geconfigureerd voor eenmalige aanmelding, zoekt u uw exemplaar van Pingboard met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar **Pingboard** in de galerie met toepassingen. Selecteer **Pingboard** uit de lijst met zoekresultaten en toe te voegen aan de lijst met toepassingen.

3. Selecteer uw exemplaar van Pingboard, en selecteer vervolgens de **inrichten** tabblad.

4. Stel **Inrichtingsmodus** naar **automatische**.

    ![Pingboard inrichten](./media/pingboard-provisioning-tutorial/pingboardazureprovisioning.png)
    
5. Onder de **beheerdersreferenties** sectie, voert u de volgende stappen uit:

    a. In **Tenant-URL**, voer `https://your_domain.pingboard.com/scim/v2`, en "uw_domein" vervangen door uw werkelijke domein.

    b. Aanmelden bij [Pingboard](https://pingboard.com/) met behulp van uw beheerdersaccount.

    c. Selecteer **invoegtoepassingen** > **integraties** > **Azure Active Directory**.

    d. Ga naar de **configureren** tabblad en selecteer **gebruikers inrichten van Azure inschakelen**.

    e. Kopieer het token in **OAuth Bearer-Token**, en voer deze in **geheim Token**.

6. Selecteer in de Azure-portal **testverbinding** om te controleren of Azure AD, kan verbinding maken met uw app Pingboard. Als de verbinding is mislukt, zorg ervoor dat uw account Pingboard beheerdersmachtigingen heeft en probeer de **testverbinding** stap opnieuw.

7. Voer het e-mailadres van een persoon of groep die u meldingen wilt ontvangen inrichting fout in **e-mailmelding**. Schakel het selectievakje onder.

8. Selecteer **Opslaan**. 

9. Onder de **toewijzingen** sectie **synchroniseren Azure Active Directory-gebruikers Pingboard**.

10. In de **kenmerktoewijzingen** sectie, moet u de kenmerken van de gebruiker moeten worden gesynchroniseerd vanuit Azure AD naar Pingboard controleren. De kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen overeenkomen met de gebruikersaccounts in Pingboard voor update-bewerkingen worden gebruikt. Selecteer **opslaan** eventuele wijzigingen doorvoeren. Zie voor meer informatie [aanpassen gebruikersinrichting, kenmerktoewijzingen](../active-directory-saas-customizing-attribute-mappings.md).

11. Inschakelen van de Azure AD-service voor Pingboard, inrichten in de **instellingen** sectie, wijzigt u **inrichting Status** naar **op**.

12. Selecteer **opslaan** starten van de initiële synchronisatie van gebruikers die zijn toegewezen aan Pingboard.

De initiële synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de service wordt uitgevoerd. Gebruik de **synchronisatiedetails** sectie voortgang en volg de koppelingen voor het inrichten van activiteitenlogboeken. De logboeken worden alle acties die worden uitgevoerd door de inrichting service op uw app Pingboard beschreven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapporteren over automatische account gebruikersaanvragen](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Account gebruikersinrichting voor zakelijke apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Eenmalige aanmelding configureren](pingboard-tutorial.md)
