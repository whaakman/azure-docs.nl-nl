---
title: 'Zelfstudie: Azure Active Directory-integratie met Concur | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Concur.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: df47f55f-a894-4e01-a82e-0dbf55fc8af1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: f39013785f000c7055aaa28f4c6f41998644cb6a
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-configuring-concur-for-user-provisioning"></a>Zelfstudie: Configureren voor gebruikers inrichten instemming

Het doel van deze zelfstudie is zodat u de stappen die u uitvoeren in Concur en Azure AD wilt om automatisch in te richten en inrichten van gebruikersaccounts vanuit Azure AD naar Concur ongedaan.

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure Active directory-tenant.
*   Een Concur eenmalige aanmelding ingeschakeld abonnement.
*   Een gebruikersaccount in Concur met beheerdersmachtigingen Team.

## <a name="assigning-users-to-concur"></a>Gebruikers toewijzen aan Concur

Azure Active Directory gebruikt een concept 'toewijzingen' genoemd om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van automatische gebruikers account inrichten, alleen de gebruikers en groepen die '' tot een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureren en inschakelen van de inrichting service, moet u om te bepalen welke gebruikers en/of groepen in Azure AD vertegenwoordigen de gebruikers die toegang tot uw app Concur nodig hebben. Als besloten, kunt u deze gebruikers toewijzen aan uw app Concur door de volgende instructies te volgen:

[Een gebruiker of groep toewijzen aan een enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-concur"></a>Belangrijke tips voor het toewijzen van gebruikers aan Concur

*   Het is raadzaam om één Azure AD-gebruiker worden toegewezen aan Concur voor het testen van de configuratie van de inrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Wanneer een gebruiker aan Concur toewijzen, moet u een geldige gebruikersrol selecteren. De rol 'Default toegang' werkt niet voor het inrichten.

## <a name="enable-user-provisioning"></a>Gebruikersinrichting inschakelen

Deze sectie helpt u bij het verbinding maken met uw Azure AD Concur van gebruikersaccount inrichten API en configureren van de inrichting service te maken, bijwerken en uitschakelen van toegewezen gebruikersaccounts in Concur op basis van gebruikers en groepen toewijzen in Azure AD.

> [!Tip] 
> U kunt ook op basis van SAML eenmalige aanmelding is ingeschakeld voor Concur, vindt u de instructies te volgen in [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies aanvulling van elkaar.

### <a name="to-configure-user-account-provisioning"></a>Configureren voor het inrichten van het account:

Het doel van deze sectie is het inschakelen van de inrichting van Active Directory-gebruikersaccounts met Concur overzicht.

Heeft zodat apps in de Service onkosten, er moet een juiste installatie en het gebruik van een profiel voor een Web-servicebeheerder. Voeg de WS-beheerrol niet aan uw bestaande administrator-profiel dat u voor T & en beheerfuncties gebruikt.

Adviseurs instemming of de beheerder van de client moet een afzonderlijke Service webbeheerder-profiel maken en de beheerder van de Client Gebruik dit profiel voor de functies van de beheerder van de Web-Services (bijvoorbeeld inschakelen apps). Deze profielen moeten worden gescheiden gehouden van de client dagelijkse d & E admin beheerdersprofiel (het profiel van de beheerder T & en mag geen rol WSAdmin).

Wanneer u het profiel moet worden gebruikt voor het inschakelen van de app maakt, voert u naam van de beheerder van de client naar de gebruiker profiel velden. Hiermee wijst u het eigendom toe aan het profiel. Zodra een of meer profielen is gemaakt, de client moet aanmelden met dit profiel te klikken op de '*inschakelen*' knop voor een App-Partner in het menu Web Services.

De volgende oorzaken hebben, moet deze actie niet aan het profiel dat ze voor het normale beheer van d & E gebruiken worden gedaan.

* De client moet zijn die klikt op '*Ja*' in het venster dialoog die wordt weergegeven nadat een app is ingeschakeld. Klikt u op bevestigt dat de client is bereid voor de Partner-toepassing voor toegang tot hun gegevens, zodat u of de Partner kan niet daarop Ja klikken.

* Als de beheerder van een client die een app is ingeschakeld met behulp van de beheerder d & E profiel het bedrijf verlaat (wat resulteert in het profiel wordt uitgeschakeld), alle apps die zijn ingeschakeld met dat profiel niet werkt totdat de app met een andere actieve WS-Admin-profiel is ingeschakeld. Dit is de reden waarom u moet afzonderlijke WS-beheerder om profielen te maken.

* Als een beheerder het bedrijf verlaat, kan de naam gekoppeld aan het profiel WS-beheer worden gewijzigd in de beheerder van de vervangende indien gewenst zonder enige impact op dat de ingeschakelde app omdat dit profiel niet hoeft deactiveren.

**Als u wilt configureren voor gebruikers inrichten, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **Concur** tenant.

2. Van de **beheer** selecteert u **webservices**.
   
    ![Concur tenant](./media/active-directory-saas-concur-provisioning-tutorial/IC721729.png "Concur tenant")

3. Aan de linkerkant van de **webservices** deelvenster **partnertoepassing inschakelen**.
   
    ![Partnertoepassing inschakelen](./media/active-directory-saas-concur-provisioning-tutorial/ic721730.png "partnertoepassing inschakelen")

4. Van de **toepassing inschakelen** selecteert **Azure Active Directory**, en klik vervolgens op **inschakelen**.
   
    ![Microsoft Azure Active Directory](./media/active-directory-saas-concur-provisioning-tutorial/ic721731.png "Microsoft Azure Active Directory")

5. Klik op **Ja** sluiten de **bevestigen actie** dialoogvenster.
   
    ![Bevestig de actie](./media/active-directory-saas-concur-provisioning-tutorial/ic721732.png "Bevestig de actie")

6. In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory > zakelijke Apps > alle toepassingen** sectie.

7. Als u al Concur voor eenmalige aanmelding hebt geconfigureerd, kunt u zoeken naar uw exemplaar van Concur met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar **Concur** in de galerie met toepassingen. Concur selecteert in de zoekresultaten en toe te voegen aan uw lijst met toepassingen.

8. Selecteer uw exemplaar van Concur en selecteer vervolgens de **inrichten** tabblad.

9. Stel de **Inrichtingsmodus** naar **automatische**. 
 
    ![Inrichting](./media/active-directory-saas-concur-provisioning-tutorial/provisioning.png)

10. Onder de **beheerdersreferenties** sectie, voert u de **gebruikersnaam** en de **wachtwoord** van uw beheerder Concur.

11. Klik in de Azure-portal op **testverbinding** om te controleren of Azure AD, kan verbinding maken met uw app Concur. Als de verbinding is mislukt, Controleer of uw account Concur Team beheerder machtigingen.

12. Voer het e-mailadres van een persoon of groep die in inrichting fout meldingen moet ontvangen de **e-mailmelding** veld en schakel het selectievakje in.

13. Klik op **opslaan.**

14. Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory-gebruikers Concur.**

15. In de **kenmerktoewijzingen** sectie, moet u de kenmerken van de gebruiker die gesynchroniseerd zijn van Azure AD naar Concur controleren. De kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen overeenkomen met de gebruikersaccounts in Concur voor update-bewerkingen worden gebruikt. Selecteer de knop Opslaan eventuele wijzigingen doorvoeren.

16. Om de Azure AD-service voor Concur inricht, wijzigen de **inrichting Status** naar **op** in de **instellingen** sectie

17. Klik op **opslaan.**

U kunt nu een testaccount maken. Wacht 20 minuten duren om te verifiëren dat het account is gesynchroniseerd met Concur.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Het beheren van gebruikers account inrichten voor zakelijke Apps](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Eenmalige aanmelding configureren](active-directory-saas-concur-tutorial.md)

