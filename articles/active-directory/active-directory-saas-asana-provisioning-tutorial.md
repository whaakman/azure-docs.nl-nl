---
title: 'Zelfstudie: Asana configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en gebruikersaccounts aan Asana ongedaan in te richten.
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: sakula
ms.assetid: 0b38ee73-168b-42cb-bd8b-9c5e5126d648
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: asmalser
ms.reviewer: asmalser
ms.openlocfilehash: 4f3bd11a99f43d6405ea285a7a283179d561f92a
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2018
---
# <a name="tutorial-configure-asana-for-automatic-user-provisioning"></a>Zelfstudie: Asana configureren voor het automatisch gebruikers inrichten

Het doel van deze zelfstudie is zodat u de stappen die u uitvoeren in Asana en Azure AD wilt om automatisch in te richten en inrichten van gebruikersaccounts vanuit Azure AD naar Asana ongedaan.

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure Active Active directory-tenant
*   Een tenant Asana met [Enterprise](https://www.asana.com/pricing) plannen of beter ingeschakeld 
*   Een gebruikersaccount in Asana met beheerdersmachtigingen 

> [!NOTE] 
> De Azure AD integratie inrichting is afhankelijk van de [Asana API](https://app.asana.com/api/1.0/scim/Users) die voor Asana beschikbaar is.

## <a name="assigning-users-to-asana"></a>Gebruikers toewijzen aan Asana

Azure Active Directory gebruikt een concept 'toewijzingen' genoemd om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van automatische gebruikers account inrichten, worden alleen de gebruikers die '' tot een toepassing in Azure AD toegewezen zijn gesynchroniseerd. 

Voordat u configureren en inschakelen van de inrichting service, moet u bepalen welke gebruikers in Azure AD vertegenwoordigen de gebruikers die toegang tot uw app Asana nodig hebben. Als besloten, kunt u deze gebruikers toewijzen aan uw app Asana door de volgende instructies te volgen:

[Een gebruiker toewijzen aan een enterprise-app](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-asana"></a>Belangrijke tips voor het toewijzen van gebruikers aan Asana

*   Het is raadzaam om één Azure AD-gebruiker worden toegewezen aan Asana voor het testen van de configuratie van de inrichting. Extra gebruikers kunnen later worden toegewezen.

## <a name="configuring-user-provisioning-to-asana"></a>Gebruikersaanvragen voor Asana configureren 

In deze sectie helpt u bij het verbinding maken met uw Azure AD Asana gebruikersaccount inrichten API en de inrichting service maken, bijwerken en uitschakelen configureren toegewezen gebruikersaccounts in Asana op basis van de gebruiker is toegewezen in Azure AD.

> [!TIP]
> U kunt ook op basis van SAML eenmalige aanmelding is ingeschakeld voor Asana, vindt u de instructies te volgen in [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies aanvulling van elkaar.

### <a name="to-configure-automatic-user-account-provisioning-to-asana-in-azure-ad"></a>Voor het configureren van automatische account gebruikersaanvragen naar Asana in Azure AD:

1)  In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory > zakelijke Apps > alle toepassingen** sectie.

2) Als u al Asana voor eenmalige aanmelding hebt geconfigureerd, kunt u zoeken naar uw exemplaar van Asana met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar **Asana** in de galerie met toepassingen. Selecteer **Asana** uit de lijst met zoekresultaten en toe te voegen aan de lijst met toepassingen.

3)  Selecteer uw exemplaar van Asana en selecteer vervolgens de **inrichten** tabblad.

4)  Stel de **Inrichtingsmodus** naar **automatische**.

![Asana inrichten](./media/active-directory-saas-asana-provisioning-tutorial/asanaazureprovisioning.png)

5) Onder de sectie beheerdersreferenties volgt u onderstaande instructies voor het genereren van het token en voer deze in de **geheim Token** textbox.

* Meld u aan bij [Asana](https://app.asana.com) met Administrator-account
* Klik op de profielfoto van de bovenste balk en selecteer de huidige instellingen voor de naam van de organisatie
* Navigeer naar het tabblad serviceaccounts
* Klik op Service-Account toevoegen
* Werk de naam, punt en profiel foto's, indien nodig, Kopieer **Token** en klik op de wijzigingen opslaan

6) Klik in de Azure-portal op **testverbinding** om te controleren of Azure AD, kan verbinding maken met uw app Asana. Als de verbinding is mislukt, zorg ervoor dat uw account Asana heeft beheerdersmachtigingen en probeer de **'Testverbinding '** stap opnieuw.

7) Voer het e-mailadres van een persoon of groep die in inrichting fout meldingen moet ontvangen de **e-mailmelding** veld en schakel het selectievakje hieronder in.

8) Klik op **Opslaan**. 

9) Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory-gebruikers Asana**.

10) In de **kenmerktoewijzingen** sectie, moet u de kenmerken van de gebruiker van Azure AD worden gesynchroniseerd met Asana controleren. Let op de kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen overeenkomen met de gebruikersaccounts in Asana voor update-bewerkingen worden gebruikt. Selecteer de **opslaan** knop eventuele wijzigingen doorvoeren. Zie [gebruiker inrichting kenmerktoewijzingen aanpassen](active-directory-saas-customizing-attribute-mappings.md) voor meer informatie

11) Om de Azure AD-service voor Asana inricht, wijzigen de **inrichting Status** naar **op** in de **instellingen** sectie

12) Klik op **Opslaan**. 

Hiermee start u de initiële synchronisatie van alle gebruikers die zijn toegewezen aan Asana in de sectie gebruikers. De eerste duurt langer dan het volgende wordt gesynchroniseerd, die ongeveer 20 minuten optreden als de service wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en volg de koppelingen voor het inrichten van de activiteitsrapporten, waarin alle acties die worden uitgevoerd door de inrichting service op uw app Asana beschrijven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over automatische account gebruikersaanvragen](active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Het beheren van gebruikers account inrichten voor zakelijke Apps](active-directory-enterprise-apps-manage-provisioning.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Eenmalige aanmelding configureren](active-directory-saas-asana-tutorial.md)
