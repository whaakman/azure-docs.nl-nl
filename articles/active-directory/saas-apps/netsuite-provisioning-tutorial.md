---
title: 'Zelfstudie: Netsuite configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Netsuite.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 8a6d3994-ee33-4a6f-b0a2-9d0389467f16
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: e14f74f3dd6d49b882dedcb2ae01029a50a459a3
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44346743"
---
# <a name="tutorial-configuring-netsuite-for-automatic-user-provisioning"></a>Zelfstudie: Netsuite configureren voor het automatisch inrichten van gebruikers

Het doel van deze zelfstudie is om weer te geven u de stappen die u uitvoeren in Netsuite en Azure AD wilt voor het automatisch inrichten en inrichting van gebruikersaccounts vanuit Azure AD naar Netsuite ongedaan maken.

## <a name="prerequisites"></a>Vereiste onderdelen

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure Active directory-tenant.
*   Een Netsuite eenmalige aanmelding ingeschakeld abonnement.
*   Een gebruikersaccount in Netsuite met Team beheerdersmachtigingen.

## <a name="assigning-users-to-netsuite"></a>Gebruikers toewijzen aan Netsuite

Azure Active Directory maakt gebruik van een concept genaamd "toewijzingen" om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het inrichten van automatische gebruikersaccounts, worden alleen de gebruikers en groepen die '' aan een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureren en inschakelen van de inrichtingsservice, moet u om te bepalen welke gebruikers en/of groepen in Azure AD de gebruikers die toegang nodig tot uw app Netsuite vertegenwoordigen. Als besloten, kunt u deze gebruikers toewijzen aan uw app Netsuite door de instructies hier:

[Een gebruiker of groep toewijzen aan een enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-netsuite"></a>Belangrijke tips voor het toewijzen van gebruikers aan Netsuite

*   Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan Netsuite voor het testen van de configuratie van de inrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Wanneer een gebruiker aan Netsuite toewijzen, moet u een geldige gebruikersrol selecteren. De rol 'standaardtoegang' werkt niet voor het inrichten.

## <a name="enable-user-provisioning"></a>Inrichting van de gebruiker inschakelen

In deze sectie helpt u bij uw Azure AD verbinden met de Netsuite gebruikersaccount Inrichtings-API en configureren van de provisioning-service voor het maken, bijwerken en uitschakelen van toegewezen gebruikersaccounts in Netsuite op basis van gebruikers en groepen in Azure AD.

> [!TIP] 
> U kunt ook op SAML gebaseerde eenmalige aanmelding voor Netsuite is ingeschakeld, vindt u de instructies te volgen in [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies een fraaie aanvulling in elkaar.

### <a name="to-configure-user-account-provisioning"></a>Het configureren van het inrichten van gebruikersaccounts:

Het doel van deze sectie is om een overzicht van het inschakelen van het inrichten van gebruikers van Active Directory-gebruikersaccounts met Netsuite te.

1. In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory > Bedrijfsapps > alle toepassingen** sectie.

1. Als u al Netsuite hebt geconfigureerd voor eenmalige aanmelding, zoeken naar uw exemplaar van Netsuite met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar de **Netsuite** in de toepassingengalerie. Netsuite selecteren in de resultaten voor zoeken en toe te voegen aan uw lijst met toepassingen.

1. Selecteer uw exemplaar van Netsuite en selecteer vervolgens de **Provisioning** tabblad.

1. Stel de **Inrichtingsmodus** naar **automatische**. 

    ![inrichten](./media/netsuite-provisioning-tutorial/provisioning.png)

1. Onder de **beheerdersreferenties** sectie, geeft u de volgende configuratie-instellingen:
   
    a. In de **beheerdersnaam** tekstvak, type een Netsuite accountnaam waarvoor de **systeembeheerder** profiel in Netsuite.com toegewezen.
   
    b. In de **beheerderswachtwoord** tekstvak typt u het wachtwoord voor dit account.
      
1. Klik in de Azure-portal op **testverbinding** om te controleren of Azure AD kunt verbinden met uw app Netsuite.

1. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die moet inrichten fout meldingen ontvangen, en schakel het selectievakje in.

1. Klik op **opslaan.**

1. Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory: gebruikers aan Netsuite.**

1. In de **kenmerktoewijzingen** sectie, controleert u de kenmerken van de gebruiker die van Azure AD worden gesynchroniseerd naar Netsuite. Houd er rekening mee dat de kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in Netsuite voor update-bewerkingen. Selecteer de knop Opslaan om door te voeren van eventuele wijzigingen.

1. Wijzigen zodat de Azure AD-inrichtingsservice voor Netsuite de **Inrichtingsstatus** naar **op** in de sectie instellingen

1. Klik op **opslaan.**

De initiële synchronisatie van alle gebruikers en/of groepen die zijn toegewezen aan Netsuite in de sectie gebruikers en groepen wordt gestart. Houd er rekening mee dat de eerste synchronisatie langer dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden duurt als de service wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van activiteitenlogboeken, waarin alle acties die worden uitgevoerd door de provisioning-service op uw app Netsuite worden beschreven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
* [Eenmalige aanmelding configureren](netsuite-tutorial.md)