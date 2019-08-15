---
title: 'Zelfstudie: ServiceNow configureren voor het automatisch inrichten van gebruikers met Azure Active Directory | Microsoft Docs'
description: Meer informatie over het automatisch inrichten en ongedaan maken van de inrichting van gebruikers accounts van Azure AD naar ServiceNow.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 4d6f06dd-a798-4c22-b84f-8a11f1b8592a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/12/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 85783339c7d1348f598f924f14d9b40cd0c8cd22
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2019
ms.locfileid: "68967167"
---
# <a name="tutorial-configure-servicenow-for-automatic-user-provisioning-with-azure-active-directory"></a>Zelfstudie: ServiceNow configureren voor het automatisch inrichten van gebruikers met Azure Active Directory

Het doel van deze zelf studie is om u te laten zien welke stappen u moet uitvoeren in ServiceNow en Azure AD om gebruikers accounts van Azure AD automatisch in te richten en te deactiveren naar ServiceNow.

> [!NOTE]
> In deze zelf studie wordt een connector beschreven die boven op de Azure AD User Provisioning-Service is gebouwd. Zie [Gebruikers inrichten en de inrichting ongedaan maken voor SaaS-toepassingen met Azure Active Directory](../manage-apps/user-provisioning.md)voor belang rijke informatie over de werking van deze service, hoe deze werkt en veelgestelde vragen.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met ServiceNow hebt u het volgende nodig:

- Een Azure AD-abonnement
- Voor ServiceNow: een instantie of tenant van ServiceNow, versie Calgary of hoger
- Voor ServiceNow Express: een instantie van ServiceNow Express, versie Helsinki of hoger

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen Azure AD-proef omgeving hebt, kunt u een [gratis account](https://azure.microsoft.com/free/)aanvragen.

## <a name="assigning-users-to-servicenow"></a>Gebruikers toewijzen aan ServiceNow

Azure Active Directory gebruikt een concept met de naam ' toewijzingen ' om te bepalen welke gebruikers toegang moeten krijgen tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers accounts worden alleen de gebruikers en groepen die zijn toegewezen aan een toepassing in azure AD gesynchroniseerd.

Voordat u de inrichtings service configureert en inschakelt, moet u bepalen welke gebruikers en/of groepen in azure AD de gebruikers vertegenwoordigen die toegang nodig hebben tot uw ServiceNow-app. Nadat u hebt besloten, kunt u deze gebruikers toewijzen aan uw ServiceNow-app door de volgende instructies te volgen: [Een gebruiker of groep toewijzen aan een bedrijfs-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)


> [!IMPORTANT]
>*   U wordt aangeraden één Azure AD-gebruiker toe te wijzen aan ServiceNow om de inrichtings configuratie te testen. Extra gebruikers en/of groepen kunnen later worden toegewezen.
>*   Wanneer u een gebruiker toewijst aan ServiceNow, moet u een geldige gebruikersrol selecteren. De rol ' standaard toegang ' werkt niet voor het inrichten.
>*   Voor meer informatie over het maken en configureren van rollen in azure AD verwijzen we u naar deze [koppeling](https://docs.microsoft.com/azure/active-directory/develop/active-directory-enterprise-app-role-management)

## <a name="enable-automated-user-provisioning"></a>Automatische gebruikers inrichting inschakelen

In deze sectie vindt u instructies voor het verbinden van uw Azure AD-ServiceNow en het configureren van de inrichtings service om toegewezen gebruikers accounts in ServiceNow te maken, bij te werken en uit te scha kelen op basis van de gebruikers-en groeps toewijzing in azure AD.

> [!TIP]
>U kunt er ook voor kiezen om eenmalige aanmelding op basis van SAML in te scha kelen voor ServiceNow, volgens de instructies in [Azure Portal](https://portal.azure.com). Eenmalige aanmelding kan onafhankelijk van automatische inrichting worden geconfigureerd, maar deze twee functies gelden voor elkaar.

### <a name="configure-automatic-user-account-provisioning"></a>Automatisch inrichten van gebruikers accounts configureren

1. Blader in het [Azure Portal](https://portal.azure.com)naar het gedeelte **Azure Active Directory > Enter prise-apps > alle toepassingen** .

1. Als u ServiceNow al hebt geconfigureerd voor eenmalige aanmelding, zoekt u naar uw instantie van ServiceNow met behulp van het zoek veld. Als dat niet het geval is, selecteert u **toevoegen** en zoeken naar **ServiceNow** in de toepassings galerie. Selecteer ServiceNow in de zoek resultaten en voeg deze toe aan uw lijst met toepassingen.

1. Selecteer uw exemplaar van ServiceNow en selecteer vervolgens het tabblad **inrichten** .

1. Stel de inrichtings modus in op **automatisch**. 

    ![inrichten](./media/servicenow-provisioning-tutorial/provisioning.png)

1. Voer de volgende stappen uit in de sectie beheerders referenties:
   
    a. Typ de naam van het ServiceNow-exemplaar in het tekstvak **naam van ServiceNow-exemplaar** .

    b. Typ in het tekstvak **ServiceNow beheer gebruikers naam** de gebruikers naam van een beheerder.

    c. Het wacht woord van de beheerder in het tekstvak **ServiceNow Administrator-wacht woord** .

1. Klik in het Azure Portal op **verbinding testen** om ervoor te zorgen dat Azure AD verbinding kan maken met uw ServiceNow-app. Als de verbinding mislukt, zorgt u ervoor dat uw ServiceNow-account beschikt over team beheerders machtigingen en voert u de stap **beheerders referenties** opnieuw uit.

1. Voer het e-mail adres in van een persoon of groep die inrichtings fout meldingen moet ontvangen in het veld **e-mail melding** en schakel het selectie vakje in.

1. Klik op **opslaan.**

1. Selecteer in de sectie toewijzingen de optie **Azure Active Directory gebruikers synchroniseren met ServiceNow.**

1. Controleer in de sectie **kenmerk toewijzingen** de gebruikers kenmerken die zijn gesynchroniseerd vanuit Azure AD naar ServiceNow. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt om te voldoen aan de gebruikers accounts in ServiceNow voor bijwerk bewerkingen. Selecteer de knop Opslaan om door te voeren van eventuele wijzigingen.

1. Als u de Azure AD-inrichtings service voor ServiceNow wilt inschakelen, wijzigt u de inrichtings **status** in in het gedeelte instellingen

1. Klik op **opslaan.**

Hiermee start u de initiële synchronisatie van gebruikers en/of groepen die zijn toegewezen aan ServiceNow in de sectie gebruikers en groepen. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de service wordt uitgevoerd. U kunt de sectie **synchronisatie Details** gebruiken om de voortgang te bewaken en koppelingen te volgen voor het inrichtings logboek, waarin alle acties worden beschreven die worden uitgevoerd door de inrichtings service in uw ServiceNow-app.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Inrichten van gebruikers accounts voor zakelijke apps beheren](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
* [Eenmalige aanmelding configureren](servicenow-tutorial.md)


