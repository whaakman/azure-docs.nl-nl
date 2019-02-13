---
title: 'Zelfstudie: DocuSign configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en DocuSign.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 294cd6b8-74d7-44bc-92bc-020ccd13ff12
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 121d147a3f8c91f17e955120b2c14f7dbd3da592
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56191840"
---
# <a name="tutorial-configure-docusign-for-automatic-user-provisioning"></a>Zelfstudie: DocuSign voor het automatisch inrichten van gebruikers configureren

Het doel van deze zelfstudie is om u de stappen die u uitvoeren in de DocuSign en Azure AD wilt voor het automatisch inrichten en verwijdering van gebruikersaccounts vanuit Azure AD naar DocuSign weer te geven.

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure Active directory-tenant.
*   Een DocuSign eenmalige aanmelding ingeschakeld abonnement.
*   Een gebruikersaccount in DocuSign met Team beheerdersmachtigingen.

## <a name="assigning-users-to-docusign"></a>Gebruikers toewijzen aan DocuSign

Azure Active Directory maakt gebruik van een concept genaamd "toewijzingen" om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het inrichten van automatische gebruikersaccounts, worden alleen de gebruikers en groepen die '' aan een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureren en inschakelen van de inrichtingsservice, moet u om te bepalen welke gebruikers en/of groepen in Azure AD de gebruikers die toegang nodig tot uw app DocuSign vertegenwoordigen. Als besloten, kunt u deze gebruikers toewijzen aan uw app DocuSign en volg de instructies hier:

[Een gebruiker of groep toewijzen aan een enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-docusign"></a>Belangrijke tips voor het toewijzen van gebruikers aan DocuSign

*   Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan DocuSign voor het testen van de configuratie van de inrichting. Extra gebruikers kunnen later worden toegewezen.

*   Wanneer een gebruiker aan DocuSign toewijzen, moet u een geldige gebruikersrol selecteren. De rol 'standaardtoegang' werkt niet voor het inrichten.

> [!NOTE]
> Azure AD biedt geen ondersteuning voor het inrichten van de groep met de Docusign-toepassing, kunnen alleen gebruikers worden ingericht.

## <a name="enable-user-provisioning"></a>Inrichting van de gebruiker inschakelen

In deze sectie helpt u bij uw Azure AD verbinden met de DocuSign-gebruikersaccount Inrichtings-API en configureren van de provisioning-service voor het maken, bijwerken en uitschakelen van toegewezen gebruikersaccounts in DocuSign op basis van gebruikers en groepen in Azure AD.

> [!Tip]
> U kunt ook op SAML gebaseerde eenmalige aanmelding ingeschakeld voor DocuSign, vindt u de instructies te volgen in [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies een fraaie aanvulling in elkaar.

### <a name="to-configure-user-account-provisioning"></a>Het configureren van het inrichten van gebruikersaccounts:

Het doel van deze sectie is om een overzicht van het inschakelen van het inrichten van gebruikers van Active Directory-gebruikersaccounts met DocuSign te.

1. In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory > Bedrijfsapps > alle toepassingen** sectie.

1. Als u al DocuSign hebt geconfigureerd voor eenmalige aanmelding, zoeken naar uw exemplaar van DocuSign met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar de **DocuSign** in de toepassingengalerie. Selecteer DocuSign in de resultaten voor zoeken en toe te voegen aan uw lijst met toepassingen.

1. Selecteer uw exemplaar van DocuSign en selecteer vervolgens de **Provisioning** tabblad.

1. Stel de **Inrichtingsmodus** naar **automatische**. 

    ![inrichten](./media/docusign-provisioning-tutorial/provisioning.png)

1. Onder de **beheerdersreferenties** sectie, geeft u de volgende configuratie-instellingen:
   
    a. In de **beheerdersnaam** tekstvak, type een DocuSign-accountnaam waarvoor de **systeembeheerder** profiel in DocuSign.com toegewezen.
   
    b. In de **beheerderswachtwoord** tekstvak typt u het wachtwoord voor dit account.

1. Klik in de Azure-portal op **testverbinding** om te controleren of Azure AD kunt verbinden met uw app in DocuSign.

1. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die moet inrichten fout meldingen ontvangen, en schakel het selectievakje in.

1. Klik op **opslaan.**

1. Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory: gebruikers aan DocuSign.**

1. In de **kenmerktoewijzingen** sectie, controleert u de kenmerken van de gebruiker die van Azure AD worden gesynchroniseerd naar DocuSign. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in DocuSign voor update-bewerkingen. Selecteer de knop Opslaan om door te voeren van eventuele wijzigingen.

1. Om in te schakelen in de Azure AD-inrichtingsservice voor DocuSign, wijzigen de **Inrichtingsstatus** naar **op** in de sectie instellingen

1. Klik op **opslaan.**

De initiële synchronisatie van alle gebruikers die zijn toegewezen aan DocuSign in de sectie gebruikers en groepen wordt gestart. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de service wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van activiteitenlogboeken, waarin alle acties die worden uitgevoerd door de provisioning-service in de DocuSign-app worden beschreven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
* [Eenmalige aanmelding configureren](docusign-tutorial.md)
