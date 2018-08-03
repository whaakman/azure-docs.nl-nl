---
title: 'Zelfstudie: ServiceNow configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Meer informatie over het automatisch inrichten en de inrichting van gebruikersaccounts uit Azure AD met ServiceNow ongedaan maken.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 4d6f06dd-a798-4c22-b84f-8a11f1b8592a
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: 8d05e314cb31aaba96e7db79e0e4dd287e6d2184
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39426901"
---
# <a name="tutorial-configure-servicenow-for-automatic-user-provisioning-with-azure-active-directory"></a>Zelfstudie: ServiceNow configureren voor het automatisch gebruikers inrichten met Azure Active Directory

Het doel van deze zelfstudie is om weer te geven u de stappen die u uitvoeren in ServiceNow en Azure AD wilt voor het automatisch inrichten en inrichting van gebruikersaccounts uit Azure AD met ServiceNow ongedaan maken.

> [!NOTE]
> Deze zelfstudie beschrijft een connector die is gebaseerd op de Provisioning-Service van Azure AD-gebruiker. Zie voor belangrijke informatie over wat deze service biedt, hoe het werkt en veelgestelde vragen [automatiseren van gebruikersinrichting en -opheffing in SaaS-toepassingen met Azure Active Directory](./../active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Vereisten

Voor het Azure AD-integratie configureren met ServiceNow, moet u de volgende items:

- Een Azure AD-abonnement
- Voor ServiceNow, een exemplaar of een tenant van ServiceNow, Calgary versie of hoger
- Voor snelle ServiceNow, een exemplaar van ServiceNow Express, Helsinki versie of hoger

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).


## <a name="assigning-users-to-servicenow"></a>Gebruikers toewijzen aan ServiceNow

Azure Active Directory maakt gebruik van een concept genaamd "toewijzingen" om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het inrichten van automatische gebruikersaccounts, wordt alleen de gebruikers en groepen die '' aan een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureren en inschakelen van de inrichtingsservice, moet u bepalen welke gebruikers en/of groepen in Azure AD vertegenwoordigen de gebruikers die toegang nodig tot uw ServiceNow-app. Als besloten, kunt u deze gebruikers toewijzen aan uw ServiceNow-app door de instructies hier: [een gebruiker of groep toewijzen aan een enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)


> [!IMPORTANT]
>*   Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan ServiceNow voor het testen van de configuratie van de inrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.
>*   Bij het toewijzen van een gebruiker met ServiceNow, moet u een geldige gebruikersrol selecteren. De rol 'standaardtoegang' werkt niet voor het inrichten.

## <a name="enable-automated-user-provisioning"></a>Geautomatiseerde gebruikersinrichting inschakelen

In deze sectie helpt u bij uw Azure AD verbinden met de ServiceNow-gebruikersaccount Inrichtings-API en configureren van de provisioning-service voor het maken, bijwerken en uitschakelen van toegewezen gebruikersaccounts in ServiceNow op basis van gebruikers en groepen in Azure AD.

> [!TIP]
>U kunt ook op SAML gebaseerde eenmalige aanmelding ingeschakeld voor ServiceNow, vindt u de instructies te volgen in [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies een fraaie aanvulling in elkaar.

### <a name="configure-automatic-user-account-provisioning"></a>Het inrichten van automatische gebruikersaccounts configureren

1. In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory > Bedrijfsapps > alle toepassingen** sectie.

1. Als u al ServiceNow hebt geconfigureerd voor eenmalige aanmelding, zoeken naar uw exemplaar van ServiceNow met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar de **ServiceNow** in de toepassingengalerie. ServiceNow selecteren in de resultaten voor zoeken en toe te voegen aan uw lijst met toepassingen.

1. Selecteer uw exemplaar van ServiceNow, en selecteer vervolgens de **Provisioning** tabblad.

1. Stel de **inrichting** modus waarnaar moet worden **automatische**. 

    ![inrichten](./media/servicenow-provisioning-tutorial/provisioning.png)

1. In de sectie beheerdersreferenties, moet u de volgende stappen uitvoeren:
   
    a. In de **ServiceNow-exemplaarnaam** tekstvak typt u de naam van het ServiceNow-exemplaar.

    b. In de **ServiceNow-Beheerdersgebruikersnaam** tekstvak, typ de naam van de gebruiker van een beheerder.

    c. In de **ServiceNow beheerderswachtwoord** tekstvak, het administrator wachtwoord.

1. Klik in de Azure-portal op **testverbinding** om te controleren of Azure AD kunt verbinden met uw ServiceNow-app. Als de verbinding is mislukt, zorg ervoor dat uw ServiceNow-account Team beheerdersmachtigingen heeft en probeer de **"Beheerdersreferenties"** stap opnieuw uit.

1. Voer het e-mailadres van een persoon of groep die inrichting fout meldingen moet ontvangen de **e-mailmelding** veld en schakel het selectievakje in.

1. Klik op **opslaan.**

1. Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory: gebruikers met ServiceNow.**

1. In de **kenmerktoewijzingen** sectie, controleert u de kenmerken van de gebruiker die worden gesynchroniseerd vanuit Azure AD met ServiceNow. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in ServiceNow voor update-bewerkingen. Selecteer de knop Opslaan om door te voeren van eventuele wijzigingen.

1. Als wilt inschakelen in de Azure AD-inrichtingsservice voor ServiceNow, wijzigt de **Inrichtingsstatus** naar **op** in de sectie instellingen

1. Klik op **opslaan.**

De initiële synchronisatie van alle gebruikers en/of groepen die zijn toegewezen met ServiceNow in de sectie gebruikers en groepen wordt gestart. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de service wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van activiteitenlogboeken, waarin alle acties die worden uitgevoerd door de provisioning-service op de ServiceNow-app worden beschreven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
* [Eenmalige aanmelding configureren](servicenow-tutorial.md)


