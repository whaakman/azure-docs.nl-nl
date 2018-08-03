---
title: 'Zelfstudie: Jive configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Jive.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 6fbfdbe7-d66c-4305-9fea-76d6a6a92830
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: c38623bba4c15add9abae289fae97af33be4f393
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39443463"
---
# <a name="tutorial-configure-jive-for-automatic-user-provisioning"></a>Zelfstudie: Jive configureren voor het automatisch inrichten van gebruikers

Het doel van deze zelfstudie is om u de stappen die u wilt uitvoeren in Jive en Azure AD aan automatisch inrichten en ongedaan maken inrichting gebruikersaccounts uit Azure AD naar Jive weer te geven.

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure Active directory-tenant.
*   Een Jive eenmalige aanmelding ingeschakeld abonnement.
*   Een gebruikersaccount in Jive met Team beheerdersmachtigingen.

## <a name="assigning-users-to-jive"></a>Gebruikers toewijzen aan Jive

Azure Active Directory maakt gebruik van een concept genaamd "toewijzingen" om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het inrichten van automatische gebruikersaccounts, wordt alleen de gebruikers en groepen die '' aan een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureren en inschakelen van de inrichtingsservice, moet u om te bepalen welke gebruikers en/of groepen in Azure AD de gebruikers die toegang nodig tot uw app Jive vertegenwoordigen. Als besloten, kunt u deze gebruikers toewijzen aan uw app Jive door de instructies hier:

[Een gebruiker of groep toewijzen aan een enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-jive"></a>Belangrijke tips voor het toewijzen van gebruikers aan Jive

*   Het wordt aanbevolen dat één Azure AD-gebruiker worden toegewezen aan Jive voor het testen van de configuratie van de inrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Wanneer een gebruiker aan Jive toewijzen, moet u een geldige gebruikersrol selecteren. De rol 'standaardtoegang' werkt niet voor het inrichten.

## <a name="enable-user-provisioning"></a>Inrichting van de gebruiker inschakelen

In deze sectie helpt u bij het verbinden van uw Azure AD aan de gebruikersaccount van Jive Inrichtings-API en configureren van de provisioning-service voor het maken, bijwerken en uitschakelen van toegewezen gebruikersaccounts in Jive op basis van gebruikers en groepen in Azure AD.

> [!TIP]
> U kunt ook op SAML gebaseerde eenmalige aanmelding voor Jive is ingeschakeld, vindt u de instructies te volgen in [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies een fraaie aanvulling in elkaar.

### <a name="to-configure-user-account-provisioning"></a>Het configureren van het inrichten van gebruikersaccounts:

Het doel van deze sectie is om een overzicht van het inschakelen van het inrichten van gebruikers van Active Directory-gebruikersaccounts met Jive te.
Als onderdeel van deze procedure bent u op te geven van een gebruiker beveiligingstoken die u nodig hebt om aan te vragen van Jive.com.

1. In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory > Bedrijfsapps > alle toepassingen** sectie.

1. Als u al Jive hebt geconfigureerd voor eenmalige aanmelding, zoeken naar uw exemplaar van Jive met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar de **Jive** in de toepassingengalerie. Selecteer Jive in de resultaten voor zoeken en toe te voegen aan uw lijst met toepassingen.

1. Selecteer uw exemplaar van Jive en selecteer vervolgens de **Provisioning** tabblad.

1. Stel de **Inrichtingsmodus** naar **automatische**. 

    ![inrichten](./media/jive-provisioning-tutorial/provisioning.png)

1. Onder de **beheerdersreferenties** sectie, geeft u de volgende configuratie-instellingen:
   
    a. In de **Jive beheerdersnaam** tekstvak, type een Jive accountnaam waarvoor de **systeembeheerder** profiel in Jive.com toegewezen.
   
    b. In de **Jive beheerderswachtwoord** tekstvak typt u het wachtwoord voor dit account.
   
    c. In de **Tenant-URL van Jive** tekstvak typt u de tenant-URL van Jive.
      
      > [!NOTE]
      > De tenant-URL van Jive is de URL die wordt gebruikt door uw organisatie zich aanmelden bij Jive.  
      > Normaal gesproken de URL heeft de volgende indeling: **www.\< organisatie\>. jive.com**.          

1. Klik in de Azure-portal op **testverbinding** om te controleren of Azure AD kunt verbinden met uw app Jive.

1. Voer het e-mailadres van een persoon of groep die inrichting fout meldingen moet ontvangen de **e-mailmelding** veld en schakel het onderstaande selectievakje in.

1. Klik op **opslaan.**

1. Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory: gebruikers aan Jive.**

1. In de **kenmerktoewijzingen** sectie, controleert u de kenmerken van de gebruiker die van Azure AD worden gesynchroniseerd naar Jive. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in Jive voor update-bewerkingen. Selecteer de knop Opslaan om door te voeren van eventuele wijzigingen.

1. Wijzigen zodat de Azure AD-inrichtingsservice voor Jive de **Inrichtingsstatus** naar **op** in de sectie instellingen

1. Klik op **opslaan.**

De initiële synchronisatie van alle gebruikers en/of groepen die zijn toegewezen aan Jive in de sectie gebruikers en groepen wordt gestart. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de service wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van activiteitenlogboeken, waarin alle acties die worden uitgevoerd door de provisioning-service op uw app Jive worden beschreven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
* [Eenmalige aanmelding configureren](jive-tutorial.md)