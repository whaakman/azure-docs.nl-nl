---
title: 'Zelfstudie: GoToMeeting configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en GoToMeeting.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 0f59fedb-2cf8-48d2-a5fb-222ed943ff78
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: 3d2dafd409c6c7b2be06a6f18c3d392aff681efe
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44345774"
---
# <a name="tutorial-configure-gotomeeting-for-automatic-user-provisioning"></a>Zelfstudie: GoToMeeting configureren voor het automatisch inrichten van gebruikers

Het doel van deze zelfstudie is om weer te geven u de stappen die u uitvoeren in GoToMeeting en Azure AD wilt voor het automatisch inrichten en inrichting van gebruikersaccounts vanuit Azure AD naar GoToMeeting ongedaan maken.

## <a name="prerequisites"></a>Vereiste onderdelen

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure Active directory-tenant.
*   Een GoToMeeting eenmalige aanmelding ingeschakeld abonnement.
*   Een gebruikersaccount in GoToMeeting met Team beheerdersmachtigingen.

## <a name="assigning-users-to-gotomeeting"></a>Gebruikers toewijzen aan GoToMeeting

Azure Active Directory maakt gebruik van een concept genaamd "toewijzingen" om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het inrichten van automatische gebruikersaccounts, wordt alleen de gebruikers en groepen die '' aan een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureren en inschakelen van de inrichtingsservice, moet u om te bepalen welke gebruikers en/of groepen in Azure AD de gebruikers die toegang nodig tot uw app GoToMeeting vertegenwoordigen. Als besloten, kunt u deze gebruikers toewijzen aan uw app GoToMeeting door de instructies hier:

[Een gebruiker of groep toewijzen aan een enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-gotomeeting"></a>Belangrijke tips voor het toewijzen van gebruikers aan GoToMeeting

*   Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan GoToMeeting voor het testen van de configuratie van de inrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Wanneer een gebruiker toewijzen aan GoToMeeting, moet u een geldige gebruikersrol selecteren. De rol 'standaardtoegang' werkt niet voor het inrichten.

## <a name="enable-automated-user-provisioning"></a>Geautomatiseerde Gebruikersinrichting inschakelen

In deze sectie helpt u bij uw Azure AD verbinden met de GoToMeeting gebruikersaccount Inrichtings-API en configureren van de provisioning-service voor het maken, bijwerken en uitschakelen van toegewezen gebruikersaccounts in GoToMeeting op basis van gebruikers en groepen in Azure AD.

> [!TIP]
> U kunt ook op SAML gebaseerde eenmalige aanmelding ingeschakeld voor GoToMeeting, vindt u de instructies te volgen in [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies een fraaie aanvulling in elkaar.

### <a name="to-configure-automatic-user-account-provisioning"></a>Het configureren van het inrichten van automatische gebruikersaccounts:

1. In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory > Bedrijfsapps > alle toepassingen** sectie.

1. Als u al GoToMeeting hebt geconfigureerd voor eenmalige aanmelding, zoeken naar uw exemplaar van GoToMeeting met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar de **GoToMeeting** in de toepassingengalerie. Selecteer GoToMeeting in de resultaten voor zoeken en toe te voegen aan uw lijst met toepassingen.

1. Selecteer uw exemplaar van GoToMeeting, en selecteer vervolgens de **Provisioning** tabblad.

1. Stel de **inrichting** modus waarnaar moet worden **automatische**. 

    ![inrichten](./media/citrixgotomeeting-provisioning-tutorial/provisioning.png)

1. In de sectie beheerdersreferenties, moet u de volgende stappen uitvoeren:
   
    a. In de **GoToMeeting beheerdersnaam** tekstvak, typ de naam van de gebruiker van een beheerder.

    b. In de **GoToMeeting beheerderswachtwoord** tekstvak, het administrator wachtwoord.

1. Klik in de Azure-portal op **testverbinding** om te controleren of Azure AD kunt verbinden met uw app GoToMeeting. Als de verbinding is mislukt, zorg ervoor dat uw account GoToMeeting Teambeheerder machtigingen heeft en probeer de **"Beheerdersreferenties"** stap opnieuw uit.

1. Voer het e-mailadres van een persoon of groep die inrichting fout meldingen moet ontvangen de **e-mailmelding** veld en schakel het selectievakje in.

1. Klik op **opslaan.**

1. Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory: gebruikers aan GoToMeeting.**

1. In de **kenmerktoewijzingen** sectie, controleert u de kenmerken van de gebruiker die van Azure AD worden gesynchroniseerd naar GoToMeeting. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in GoToMeeting voor update-bewerkingen. Selecteer de knop Opslaan om door te voeren van eventuele wijzigingen.

1. Als wilt inschakelen in de Azure AD-inrichtingsservice voor GoToMeeting, wijzigt de **Inrichtingsstatus** naar **op** in de sectie instellingen

1. Klik op **opslaan.**

De initiële synchronisatie van alle gebruikers en/of groepen die zijn toegewezen aan GoToMeeting in de sectie gebruikers en groepen wordt gestart. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de service wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van activiteitenlogboeken, waarin alle acties die worden uitgevoerd door de provisioning-service op uw app GoToMeeting worden beschreven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
* [Eenmalige aanmelding configureren](https://docs.microsoft.com/azure/active-directory/active-directory-saas-citrix-gotomeeting-tutorial)


