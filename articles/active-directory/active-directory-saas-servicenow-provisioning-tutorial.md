---
title: 'Zelfstudie: ServiceNow configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het automatisch inrichten en gebruikersaccounts vanuit Azure AD naar ServiceNow ongedaan in te richten.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 4d6f06dd-a798-4c22-b84f-8a11f1b8592a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: jeedes
ms.openlocfilehash: 06772c9e21936d224f721413ccdc1c859b07b940
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-configuring-servicenow-for-automatic-user-provisioning-with-azure-active-directory"></a>Zelfstudie: ServiceNow configureren voor automatisch gebruikers inrichten met Azure Active Directory

Het doel van deze zelfstudie is zodat u de stappen die u uitvoeren in ServiceNow en Azure AD wilt om automatisch in te richten en inrichten van gebruikersaccounts vanuit Azure AD naar ServiceNow ongedaan.

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure Active directory-tenant.
*   U moet een geldige tenant voor ServiceNow voor werk- of ServiceNow voor onderwijs hebben. U kunt een gratis proefaccount voor de service.
*   Een gebruikersaccount in ServiceNow met beheerdersmachtigingen Team.

## <a name="assigning-users-to-servicenow"></a>Gebruikers toewijzen aan ServiceNow

Azure Active Directory gebruikt een concept 'toewijzingen' genoemd om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van automatische gebruikers account inrichten, alleen de gebruikers en groepen die '' tot een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureren en inschakelen van de inrichting service, moet u bepalen welke gebruikers en/of groepen in Azure AD vertegenwoordigen de gebruikers die toegang nodig tot uw ServiceNow-app. Als u had besloten, kunt u deze gebruikers toewijzen aan uw ServiceNow-app door de instructies volgen hier: [een gebruiker of groep toewijzen aan een enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)


> [!IMPORTANT]
>*   Het is raadzaam om één Azure AD-gebruiker is toegewezen aan ServiceNow voor het testen van de configuratie van de inrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.
>*   Wanneer een gebruiker aan ServiceNow toewijzen, moet u een geldige gebruikersrol selecteren. De rol 'Default toegang' werkt niet voor het inrichten.

## <a name="enable-automated-user-provisioning"></a>Geautomatiseerde gebruikersinrichting inschakelen

Deze sectie helpt u bij het verbinding maken met uw Azure AD ServiceNow van gebruikersaccount inrichten API en configureren van de inrichting service te maken, bijwerken en uitschakelen van toegewezen gebruikersaccounts in ServiceNow op basis van gebruikers en groepen toewijzen in Azure AD.

> [!TIP]
>U kunt ook op basis van SAML eenmalige aanmelding is ingeschakeld voor ServiceNow, vindt u de instructies te volgen in [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies aanvulling van elkaar.

### <a name="configure-automatic-user-account-provisioning"></a>Automatisch gebruikers inrichten van account configureren

1. In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory > zakelijke Apps > alle toepassingen** sectie.

2. Als u al ServiceNow hebt geconfigureerd voor eenmalige aanmelding, zoekt u uw exemplaar van ServiceNow met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar **ServiceNow** in de galerie met toepassingen. ServiceNow selecteert in de zoekresultaten en toe te voegen aan uw lijst met toepassingen.

3. Selecteer uw ServiceNow-exemplaar en selecteer vervolgens de **inrichten** tabblad.

4. Stel de **inrichting** modus **automatische**. 

    ![Inrichting](./media/active-directory-saas-servicenow-provisioning-tutorial/provisioning.png)

5. In de sectie beheerdersreferenties, moet u de volgende stappen uitvoeren:
   
    a. In de **ServiceNow-exemplaarnaam** textbox, typ de naam van het ServiceNow-exemplaar.

    b. In de **ServiceNow-Beheerdersgebruikersnaam** textbox, typ de gebruikersnaam van een beheerder.

    c. In de **ServiceNow-beheerderswachtwoord** textbox, het administrator wachtwoord.

6. Klik in de Azure-portal op **testverbinding** om te controleren of Azure AD, kan verbinding maken met uw ServiceNow-app. Als de verbinding is mislukt, Controleer uw ServiceNow-account Team beheerdersmachtigingen heeft en probeer de **'Beheerdersreferenties'** stap opnieuw.

7. Voer het e-mailadres van een persoon of groep die in inrichting fout meldingen moet ontvangen de **e-mailmelding** veld en schakel het selectievakje in.

8. Klik op **opslaan.**

9. Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory-gebruikers ServiceNow.**

10. In de **kenmerktoewijzingen** sectie, moet u de kenmerken van de gebruiker is gesynchroniseerd vanuit Azure AD ServiceNow controleren. De kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen overeenkomen met de gebruikersaccounts in ServiceNow voor update-bewerkingen worden gebruikt. Selecteer de knop Opslaan eventuele wijzigingen doorvoeren.

11. Wijzigen om de Azure AD-service voor ServiceNow-inrichting, de **inrichting Status** naar **op** in de sectie instellingen

12. Klik op **opslaan.**

De initiële synchronisatie van gebruikers en/of groepen die zijn toegewezen aan ServiceNow in de sectie gebruikers en groepen wordt gestart. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer 20 minuten optreden als de service wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en volg de koppelingen voor het inrichten van de activiteitsrapporten, waarin alle acties die worden uitgevoerd door de inrichting service op uw ServiceNow-app.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Het beheren van gebruikers account inrichten voor zakelijke Apps](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Eenmalige aanmelding configureren](active-directory-saas-servicenow-tutorial.md)


