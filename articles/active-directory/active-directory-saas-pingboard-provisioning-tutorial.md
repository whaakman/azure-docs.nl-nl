---
title: 'Zelfstudie: Pingboard configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en gebruikersaccounts aan Pingboard ongedaan in te richten.
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
ms.date: 10/19/2017
ms.author: asmalser
ms.reviewer: asmalser
ms.openlocfilehash: 0492a59852a6a2dd424c98e44969eab1ac4697e8
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="tutorial-configuring-pingboard-for-automatic-user-provisioning"></a>Zelfstudie: Pingboard configureren voor het automatisch gebruikers inrichten

Het doel van deze zelfstudie is zodat u de stappen die u uitvoeren moet om automatische inrichting en de inrichting van gebruikersaccounts vanuit Azure Active Directory naar Pingboard.

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure Active Directory-tenant
*   Een tenant Pingboard [Pro-account](https://pingboard.com/pricing) 
*   Een gebruikersaccount in Pingboard met beheerdersmachtigingen 

> [!NOTE] 
> De Azure Active Directory provisioning integratie is afhankelijk van de API Pingboard (`https://your_domain.pingboard.com/scim/v2`) die voor uw account beschikbaar is.

## <a name="assigning-users-to-pingboard"></a>Gebruikers toewijzen aan Pingboard

Azure Active Directory gebruikt een concept 'toewijzingen' genoemd om te bepalen welke gebruikers krijgen toegang tot de geselecteerde toepassingen. In de context van automatische gebruikers account inrichten, worden alleen de gebruikers die '' tot een toepassing in Azure Active Directory toegewezen zijn gesynchroniseerd. 

Voordat u configureren en inschakelen van de inrichting service, moet u bepalen welke gebruikers in Azure Active Directory vertegenwoordigen de gebruikers die toegang tot uw app Pingboard nodig hebben. Als besloten, kunt u deze gebruikers toewijzen aan uw app Pingboard door de volgende instructies te volgen:

[Een gebruiker toewijzen aan een enterprise-app](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-pingboard"></a>Belangrijke tips voor het toewijzen van gebruikers aan Pingboard

*   Het is raadzaam dat u een enkele Azure Active Directory-gebruiker aan Pingboard toewijzen voor het testen van de configuratie van de inrichting. Extra gebruikers kunnen later worden toegewezen.

## <a name="configuring-user-provisioning-to-pingboard"></a>Gebruikersaanvragen voor Pingboard configureren 

Deze sectie helpt u bij het verbinding maken met uw Azure Active Directory Pingboard gebruikersaccount API-inrichting, en configureren van de inrichting service te maken, bijwerken en uitschakelen van toegewezen gebruikersaccounts in Pingboard op basis van de gebruiker is toegewezen in Azure Active Directory.

> [!TIP]
> U kunt ook op basis van SAML eenmalige aanmelding is ingeschakeld voor Pingboard, vindt u de instructies te volgen in [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies aanvulling van elkaar.

### <a name="to-configure-automatic-user-account-provisioning-to-pingboard-in-azure-active-directory"></a>Voor het configureren van automatische account gebruikersaanvragen naar Pingboard in Azure Active Directory:

1)  In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory > zakelijke Apps > alle toepassingen** sectie.

2) Als u al Pingboard voor eenmalige aanmelding hebt geconfigureerd, kunt u zoeken naar uw exemplaar van Pingboard met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar **Pingboard** in de galerie met toepassingen. Selecteer **Pingboard** uit de lijst met zoekresultaten en toe te voegen aan de lijst met toepassingen.

3)  Selecteer uw exemplaar van Pingboard en selecteer vervolgens de **inrichten** tabblad.

4)  Stel de **Inrichtingsmodus** naar **automatische**.

![Pingboard inrichten](./media/active-directory-saas-pingboard-provisioning-tutorial/pingboardazureprovisioning.png)
    
5) In de sectie beheerdersreferenties, moet u de volgende stappen uitvoeren:

* In de **Tenant-URL** textbox Voer `https://your_domain.pingboard.com/scim/v2` en uw_domein vervangt door uw werkelijke domein
* Meld u aan bij [Pingboard](https://pingboard.com/) met Administrator-account.
* Klik op invoegtoepassingen > integraties > Azure Active Directory.
* Klik op het tabblad configureren en selecteer **gebruikers inrichten van Azure inschakelen**.
* Kopieer de **OAuth Bearer-Token** toe en voer in **geheim Token** textbox.

6) Klik in de Azure-portal op **testverbinding** om te controleren of Azure Active Directory kunnen verbinding maken met uw app Pingboard. Als de verbinding is mislukt, zorg ervoor dat uw account Pingboard heeft beheerdersmachtigingen en probeer de **'Testverbinding '** stap opnieuw.

7) Voer het e-mailadres van een persoon of groep die in inrichting fout meldingen moet ontvangen de **e-mailmelding** veld en controleer het volgende selectievakje in.

8) Klik op **Opslaan**. 

9) Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory-gebruikers Pingboard**.

10) In de **kenmerktoewijzingen** sectie, controleert u de kenmerken van de gebruiker naar Pingboard van Azure Active Directory worden gesynchroniseerd. De kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen overeenkomen met de gebruikersaccounts in Pingboard voor update-bewerkingen worden gebruikt. Selecteer de **opslaan** knop eventuele wijzigingen doorvoeren. Zie voor meer informatie [gebruiker inrichting kenmerktoewijzingen aanpassen](active-directory-saas-customizing-attribute-mappings.md).

11) Om de Azure Active Directory-service voor Pingboard inricht, wijzigen de **inrichting Status** naar **op** in de **instellingen** sectie

12) Klik op **opslaan** starten van de initiële synchronisatie van gebruikers die zijn toegewezen aan Pingboard.

De eerste duurt langer dan het volgende wordt gesynchroniseerd, die ongeveer 20 minuten optreden als de service wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en volg de koppelingen voor het inrichten van de activiteitsrapporten, waarin alle acties die worden uitgevoerd door de inrichting service op uw app Pingboard beschrijven.

Zie voor meer informatie over het lezen van de Azure Active Directory provisioning logboeken [rapportage over automatische account gebruikersaanvragen](active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Het beheren van gebruikers account inrichten voor zakelijke Apps](active-directory-enterprise-apps-manage-provisioning.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Eenmalige aanmelding configureren](active-directory-saas-pingboard-tutorial.md)
