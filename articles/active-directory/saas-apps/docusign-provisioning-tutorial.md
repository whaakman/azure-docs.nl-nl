---
title: 'Zelfstudie: DocuSign configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en DocuSign.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 294cd6b8-74d7-44bc-92bc-020ccd13ff12
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: 18004d7b7f40d8461346f7f529f54ef5f982d8de
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36213624"
---
# <a name="tutorial-configure-docusign-for-automatic-user-provisioning"></a>Zelfstudie: DocuSign configureren voor het automatisch gebruikers inrichten

Het doel van deze zelfstudie is zodat u de stappen die u uitvoeren in DocuSign en Azure AD wilt om automatisch in te richten en inrichten van gebruikersaccounts vanuit Azure AD naar DocuSign ongedaan.

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure Active directory-tenant.
*   Een DocuSign eenmalige aanmelding ingeschakeld abonnement.
*   Een gebruikersaccount in DocuSign met beheerdersmachtigingen Team.

## <a name="assigning-users-to-docusign"></a>Gebruikers toewijzen aan DocuSign

Azure Active Directory gebruikt een concept 'toewijzingen' genoemd om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van automatische gebruikers account inrichten, worden alleen de gebruikers en groepen die '' tot een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureren en inschakelen van de inrichting service, moet u om te bepalen welke gebruikers en/of groepen in Azure AD vertegenwoordigen de gebruikers die toegang tot uw app DocuSign nodig hebben. Als besloten, kunt u deze gebruikers toewijzen aan uw app DocuSign door de volgende instructies te volgen:

[Een gebruiker of groep toewijzen aan een enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-docusign"></a>Belangrijke tips voor het toewijzen van gebruikers aan DocuSign

*   Het is raadzaam om één Azure AD-gebruiker is toegewezen aan DocuSign voor het testen van de configuratie van de inrichting. Extra gebruikers kunnen later worden toegewezen.

*   Wanneer een gebruiker aan DocuSign toewijzen, moet u een geldige gebruikersrol selecteren. De rol 'Default toegang' werkt niet voor het inrichten.

> [!NOTE]
> Azure AD biedt geen ondersteuning voor het inrichten van de groep met de toepassing Docusign, alleen gebruikers kunnen worden ingericht.

## <a name="enable-user-provisioning"></a>Gebruikersinrichting inschakelen

Deze sectie helpt u bij het verbinding maken met uw Azure AD DocuSign van gebruikersaccount inrichten API en configureren van de inrichting service te maken, bijwerken en uitschakelen van toegewezen gebruikersaccounts in DocuSign op basis van gebruikers en groepen toewijzen in Azure AD.

> [!Tip]
> U kunt ook op basis van SAML eenmalige aanmelding is ingeschakeld voor DocuSign, vindt u de instructies te volgen in [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies aanvulling van elkaar.

### <a name="to-configure-user-account-provisioning"></a>Configureren voor het inrichten van het account:

Het doel van deze sectie is het inschakelen van de gebruiker het inrichten van Active Directory-gebruikersaccounts met DocuSign overzicht.

1. In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory > zakelijke Apps > alle toepassingen** sectie.

2. Als u al DocuSign voor eenmalige aanmelding hebt geconfigureerd, kunt u zoeken naar uw exemplaar van DocuSign met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar **DocuSign** in de galerie met toepassingen. DocuSign selecteert in de zoekresultaten en toe te voegen aan uw lijst met toepassingen.

3. Selecteer uw exemplaar van DocuSign en selecteer vervolgens de **inrichten** tabblad.

4. Stel de **Inrichtingsmodus** naar **automatische**. 

    ![inrichten](./media/docusign-provisioning-tutorial/provisioning.png)

5. Onder de **beheerdersreferenties** sectie, bieden de volgende configuratie-instellingen:
   
    a. In de **Beheerdersgebruikersnaam** textbox type een DocuSign accountnaam met de **systeembeheerder** profiel in DocuSign.com toegewezen.
   
    b. In de **beheerderswachtwoord** textbox, typt u het wachtwoord voor dit account.

6. Klik in de Azure-portal op **testverbinding** om te controleren of Azure AD, kan verbinding maken met uw app DocuSign.

7. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet inrichting fout meldingen ontvangen en schakel het selectievakje in.

8. Klik op **opslaan.**

9. Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory-gebruikers DocuSign.**

10. In de **kenmerktoewijzingen** sectie, moet u de kenmerken van de gebruiker is gesynchroniseerd vanuit Azure AD DocuSign controleren. De kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen overeenkomen met de gebruikersaccounts in DocuSign voor update-bewerkingen worden gebruikt. Selecteer de knop Opslaan eventuele wijzigingen doorvoeren.

11. Om de Azure AD-service voor DocuSign inricht, wijzigen de **inrichting Status** naar **op** in de sectie instellingen

12. Klik op **opslaan.**

De initiële synchronisatie van alle gebruikers die zijn toegewezen aan DocuSign in de sectie gebruikers en groepen wordt gestart. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de service wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en volg de koppelingen voor het inrichten van activiteitenlogboeken waarin alle acties die worden uitgevoerd door de inrichting van uw app DocuSign-service.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over automatische account gebruikersaanvragen](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Het beheren van gebruikers account inrichten voor zakelijke Apps](tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Eenmalige aanmelding configureren](docusign-tutorial.md)