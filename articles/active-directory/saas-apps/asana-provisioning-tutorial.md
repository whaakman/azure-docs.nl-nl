---
title: 'Zelfstudie: Asana configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en inrichting van gebruikersaccounts aan Asana ongedaan maken.
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
ms.openlocfilehash: 26642fefbb86b2709e110b13d782286fd18d5e60
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44348052"
---
# <a name="tutorial-configure-asana-for-automatic-user-provisioning"></a>Zelfstudie: Asana configureren voor het automatisch inrichten van gebruikers

Het doel van deze zelfstudie is om u de stappen die u uitvoeren in Asana en Azure Active Directory (Azure AD wilt) voor het automatisch inrichten en verwijdering van gebruikersaccounts vanuit Azure AD naar Asana weer te geven.

## <a name="prerequisites"></a>Vereiste onderdelen

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure AD-tenant
*   Een Asana-tenant met een [Enterprise](https://www.asana.com/pricing) plannen of beter ingeschakeld 
*   Een gebruikersaccount in Asana met beheerdersmachtigingen 

> [!NOTE] 
> Azure AD integratie inrichting is afhankelijk van de [Asana API](https://asana.com/developers/api-reference/users), die voor Asana beschikbaar is.

## <a name="assign-users-to-asana"></a>Gebruikers toewijzen aan Asana

Azure AD maakt gebruik van een concept genaamd "toewijzingen" om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het inrichten van automatische gebruikersaccounts, worden alleen de gebruikers die zijn toegewezen aan een toepassing in Azure AD gesynchroniseerd. 

Voordat u configureren en inschakelen van de inrichtingsservice, moet u bepalen welke gebruikers in Azure AD moeten toegang hebben tot uw app in Asana. U kunt deze gebruikers toewijzen aan uw app in Asana en volg de instructies hier:

[Een gebruiker toewijzen aan een enterprise-app](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-asana"></a>Belangrijke tips voor het toewijzen van gebruikers aan Asana

Het is raadzaam dat u één Azure AD-gebruiker aan Asana voor het testen van de configuratie van de inrichting. Extra gebruikers kunnen later worden toegewezen.

## <a name="configure-user-provisioning-to-asana"></a>Inrichten van gebruikers aan Asana configureren 

Deze sectie helpt u bij het verbinden van uw Azure AD aan Asana-gebruikersaccount Inrichtings-API. U ook configureren de provisioning-service voor het maken, bijwerken en uitschakelen van toegewezen gebruikersaccounts in Asana op basis van de toewijzingen van gebruikers in Azure AD.

> [!TIP]
> Als u wilt inschakelen op basis van SAML eenmalige aanmelding voor Asana, volg de instructies de [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies elkaar aanvullen.

### <a name="to-configure-automatic-user-account-provisioning-to-asana-in-azure-ad"></a>Het configureren van automatische inrichten van gebruikersaccounts aan Asana in Azure AD

1. In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory** > **Bedrijfsapps** > **alle toepassingen** sectie.

1. Als u al Asana geconfigureerd voor eenmalige aanmelding, zoekt u uw exemplaar van Asana met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar de **Asana** in de toepassingengalerie. Selecteer **Asana** in de resultaten voor zoeken en toe te voegen aan uw lijst met toepassingen.

1. Selecteer uw exemplaar van Asana en selecteer vervolgens de **Provisioning** tabblad.

1. Stel **Inrichtingsmodus** naar **automatische**.

    ![Asana-inrichting](./media/asana-provisioning-tutorial/asanaazureprovisioning.png)

1. Onder de **beheerdersreferenties** sectie, volgt u deze instructies voor het genereren van het token en geef deze op in **geheim Token**:

    a. Aanmelden bij [Asana](https://app.asana.com) met behulp van uw beheerdersaccount.

    b. Selecteer de profielfoto van de bovenste balk en selecteer de huidige instellingen voor de naam van de organisatie.

    c. Ga naar de **serviceaccounts** tabblad.

    d. Selecteer **Service-Account toevoegen**.

    e. Update **naam** en **over** en de profielfoto indien nodig. Kopieer het token in **Token**, en selecteer deze in **wijzigingen opslaan**.

1. Selecteer in de Azure portal, **testverbinding** om ervoor te zorgen dat Azure AD verbinding met uw app in Asana maken kunt. Als de verbinding is mislukt, zorg ervoor dat uw account Asana beheerdersmachtigingen heeft en probeer de **testverbinding** stap opnieuw uit.

1. Voer het e-mailadres van een persoon of groep die u meldingen wilt ontvangen inrichting fout in **e-mailmelding**. Schakel het selectievakje onder.

1. Selecteer **Opslaan**. 

1. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory: gebruikers aan Asana**.

1. In de **kenmerktoewijzingen** sectie, controleert u de kenmerken van de gebruiker uit Azure AD worden gesynchroniseerd met Asana. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in Asana voor update-bewerkingen. Selecteer **opslaan** doorvoeren van wijzigingen. Zie voor meer informatie, [gebruiker inrichten kenmerktoewijzingen aanpassen](../manage-apps/customize-application-attributes.md).

1. Om in te schakelen van de Azure AD-inrichtingsservice voor Asana, in de **instellingen** sectie, wijzigt u **Inrichtingsstatus** naar **op**.

1. Selecteer **Opslaan**. 

Nu de initiële synchronisatie wordt gestart voor alle gebruikers die zijn toegewezen aan Asana in de **gebruikers** sectie. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de service wordt uitgevoerd. Gebruik de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van activiteitenlogboeken. De auditlogboeken worden alle acties die worden uitgevoerd door de provisioning-service op uw app in Asana beschreven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapporteren over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
* [Eenmalige aanmelding configureren](asana-tutorial.md)
