---
title: 'Zelfstudie: Pingboard configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en inrichting ongedaan maken-gebruikersaccounts met Pingboard.
services: active-directory
documentationcenter: ''
author: asmalser-msft
writer: asmalser-msft
manager: sakula
ms.assetid: 0b38ee73-168b-42cb-bd8b-9c5e5126d648
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: asmalser
ms.reviewer: asmalser
ms.openlocfilehash: 4daf52318fc707de2ae365a6c12b676226ef7fcd
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55188402"
---
# <a name="tutorial-configure-pingboard-for-automatic-user-provisioning"></a>Zelfstudie: Pingboard configureren voor het automatisch inrichten van gebruikers

Het doel van deze zelfstudie is om u de stappen die u moet volgen om in te schakelen van automatische inrichting en ongedaan maken van gebruikersaccounts via Azure Active Directory (Azure AD) in te richten Pingboard weer te geven.

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure AD-tenant
*   Een tenant Pingboard [Pro-account](https://pingboard.com/pricing) 
*   Een gebruikersaccount in Pingboard met beheerdersmachtigingen 

> [!NOTE] 
> Azure AD integratie inrichting is afhankelijk van de [Pingboard API](https://pingboard.docs.apiary.io/#), die beschikbaar zijn voor uw account is.

## <a name="assign-users-to-pingboard"></a>Gebruikers toewijzen aan Pingboard

Azure AD maakt gebruik van een concept genaamd "toewijzingen" om te bepalen welke gebruikers krijgen toegang tot de geselecteerde toepassingen. In de context van het inrichten van automatische gebruikersaccounts, worden alleen de gebruikers die zijn toegewezen aan een toepassing in Azure AD gesynchroniseerd. 

Voordat u configureren en inschakelen van de inrichtingsservice, moet u bepalen welke gebruikers in Azure AD moeten toegang hebben tot uw app Pingboard. Vervolgens kunt u deze gebruikers toewijzen aan uw app Pingboard door de instructies hier:

[Een gebruiker toewijzen aan een enterprise-app](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-pingboard"></a>Belangrijke tips voor het toewijzen van gebruikers aan Pingboard

Het is raadzaam dat u één Azure AD-gebruiker te Pingboard voor het testen van de configuratie van de inrichting. Extra gebruikers kunnen later worden toegewezen.

## <a name="configure-user-provisioning-to-pingboard"></a>Inrichten van gebruikers aan Pingboard configureren 

Deze sectie helpt u bij uw Azure AD verbinden met het gebruikersaccount voor Pingboard Inrichtings-API. U ook configureren de provisioning-service voor het maken, bijwerken en uitschakelen van toegewezen gebruikersaccounts in Pingboard die zijn gebaseerd op de toewijzingen van gebruikers in Azure AD.

> [!TIP]
> Als u wilt inschakelen op basis van SAML eenmalige aanmelding voor Pingboard, volg de instructies de [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies elkaar aanvullen.

### <a name="to-configure-automatic-user-account-provisioning-to-pingboard-in-azure-ad"></a>Het configureren van automatische inrichten van gebruikersaccounts aan Pingboard in Azure AD

1. In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory** > **Bedrijfsapps** > **alle toepassingen** sectie.

1. Als u al Pingboard geconfigureerd voor eenmalige aanmelding, zoekt u uw exemplaar van Pingboard met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar de **Pingboard** in de toepassingengalerie. Selecteer **Pingboard** in de resultaten voor zoeken en toe te voegen aan uw lijst met toepassingen.

1. Selecteer uw exemplaar van Pingboard, en selecteer vervolgens de **Provisioning** tabblad.

1. Stel **Inrichtingsmodus** naar **automatische**.

    ![Pingboard inrichten](./media/pingboard-provisioning-tutorial/pingboardazureprovisioning.png)
    
1. Onder de **beheerdersreferenties** sectie, gebruikt u de volgende stappen uit:

    a. In **Tenant-URL**, voer `https://your_domain.pingboard.com/scim/v2`, en "uw_domein" vervangen door uw werkelijke domein.

    b. Aanmelden bij [Pingboard](https://pingboard.com/) met behulp van uw beheerdersaccount.

    c. Selecteer **invoegtoepassingen** > **integraties** > **Azure Active Directory**.

    d. Ga naar de **configureren** tabblad, en selecteer **inschakelen inrichten van gebruikers van Azure**.

    e. Kopieer het token in **OAuth Bearer Token**, en voer deze in **geheim Token**.

1. Selecteer in de Azure portal, **verbinding testen** voor het testen van Azure AD kunnen verbinding maken met uw app Pingboard. Als de verbinding is mislukt, testen of uw account Pingboard beheerdersmachtigingen heeft en probeer de **verbinding testen** stap opnieuw uit.

1. Voer het e-mailadres van een persoon of groep die u meldingen wilt ontvangen inrichting fout in **e-mailmelding**. Schakel het selectievakje onder.

1. Selecteer **Opslaan**. 

1. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory: gebruikers aan Pingboard**.

1. In de **kenmerktoewijzingen** sectie, controleert u de kenmerken van de gebruiker uit Azure AD worden gesynchroniseerd met Pingboard. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in Pingboard voor update-bewerkingen. Selecteer **opslaan** doorvoeren van wijzigingen. Zie voor meer informatie, [Kenmerktoewijzingen van gebruikersinrichting aanpassen](../manage-apps/customize-application-attributes.md).

1. Om in te schakelen van de Azure AD-inrichtingsservice voor Pingboard, in de **instellingen** sectie, wijzigt u **Inrichtingsstatus** naar **op**.

1. Selecteer **opslaan** starten van de initiële synchronisatie van gebruikers die zijn toegewezen aan Pingboard.

De initiële synchronisatie duurt langer dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de service wordt uitgevoerd. Gebruik de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van activiteitenlogboeken. De logboeken worden alle acties die door de provisioning-service op uw app Pingboard beschreven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapporteren over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
* [Eenmalige aanmelding configureren](pingboard-tutorial.md)
