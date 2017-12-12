---
title: 'Zelfstudie: Azure Active Directory-integratie met Salesforce Sandbox | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Salesforce Sandbox.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: bab73fda-6754-411d-9288-f73ecdaa486d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: jeedes
ms.openlocfilehash: d0efcae50b18dc2626af5510bd47ff36a27ba718
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-configuring-salesforce-sandbox-for-automatic-user-provisioning"></a>Zelfstudie: Sandbox-Salesforce configureren voor het automatisch gebruikers inrichten

Het doel van deze zelfstudie is zodat u de stappen die u uitvoeren in Salesforce Sandbox en Azure AD wilt om automatisch inrichten en de gebruikersaccounts van Azure AD bij Salesforce Sandbox ongedaan in te richten.

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure Active directory-tenant.
*   Een geldige tenant voor Salesforce Sandbox voor werk of Salesforce Sandbox voor onderwijs. U kunt een gratis proefaccount voor de service.
*   Een gebruikersaccount in Salesforce Sandbox met beheerdersmachtigingen Team.

## <a name="assigning-users-to-salesforce-sandbox"></a>Gebruikers toewijzen aan Salesforce Sandbox

Azure Active Directory gebruikt een concept 'toewijzingen' genoemd om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van automatische gebruikers account inrichten, worden alleen de gebruikers en groepen die '' tot een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureren en inschakelen van de inrichting service, moet u bepalen welke gebruikers of groepen in Azure AD toegang hebben tot uw Salesforce-Sandbox-app moeten. Nadat u deze beslissing hebt genomen, kunt u deze gebruikers toewijzen aan uw Salesforce-Sandbox-app door de instructies in [een gebruiker of groep toewijzen aan een enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-salesforce-sandbox"></a>Belangrijke tips voor het toewijzen van gebruikers aan de Salesforce Sandbox

* Het is raadzaam om één Azure AD-gebruiker is toegewezen aan de Salesforce Sandbox voor het testen van de configuratie van de inrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

* Wanneer een gebruiker aan de Salesforce Sandbox toewijzen, moet u een geldige gebruikersrol selecteren. De rol 'Default toegang' werkt niet voor het inrichten.

> [!NOTE]
> Deze app importeert aangepaste rollen van Salesforce Sandbox als onderdeel van het inrichtingsproces, de klant selecteren kunt bij het toewijzen van gebruikers.

## <a name="enable-automated-user-provisioning"></a>Geautomatiseerde gebruikersinrichting inschakelen

Deze sectie helpt u bij het verbinding maken met uw Azure AD van Salesforce Sandbox gebruikersaccount inrichten API en configureren van de inrichting service te maken, bijwerken en uitschakelen van de toegewezen gebruiker accounts in het Salesforce-Sandbox op basis van gebruikers en groepen toewijzen in Azure AD.

>[!Tip]
>U kunt ook op basis van SAML eenmalige aanmelding is ingeschakeld voor Salesforce Sandbox, vindt u de instructies te volgen in [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies aanvulling van elkaar.

### <a name="configure-automatic-user-account-provisioning"></a>Automatisch gebruikers inrichten van account configureren

Het doel van deze sectie is het inschakelen van de gebruiker het inrichten van Active Directory-gebruikersaccounts met Salesforce Sandbox overzicht.

1. In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory > zakelijke Apps > alle toepassingen** sectie.

2. Als u al Salesforce Sandbox voor eenmalige aanmelding hebt geconfigureerd, kunt u zoeken naar uw exemplaar van Salesforce Sandbox met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar **Salesforce Sandbox** in de galerie met toepassingen. Salesforce Sandbox selecteert in de zoekresultaten en toe te voegen aan uw lijst met toepassingen.

3. Selecteer uw exemplaar van Salesforce Sandbox en selecteer vervolgens de **inrichten** tabblad.

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Inrichting](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/provisioning.png)

5. Onder de **beheerdersreferenties** sectie, bieden de volgende configuratie-instellingen:
   
    a. In de **Admin Username** textbox type een Sandbox met Salesforce-accountnaam met de **systeembeheerder** profiel in Salesforce.com toegewezen.
   
    b. In de **beheerderswachtwoord** textbox, typt u het wachtwoord voor dit account.

6. Als u de beveiliging van uw Salesforce-sandbox-token, open een nieuw tabblad en meld u aan dezelfde Salesforce Sandbox admin-account. In de rechterbovenhoek van de pagina, klikt u op uw naam en klik vervolgens op **instellingen**.

     ![Schakel Automatische gebruikersaanvragen](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/sf-my-settings.png "automatische gebruikersinrichting inschakelen")

7. Klik in het navigatiedeelvenster links op **mijn persoonlijke gegevens** Vouw de bijbehorende sectie en klik vervolgens op **opnieuw mijn beveiligingstoken**.
  
    ![Schakel Automatische gebruikersaanvragen](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/sf-personal-reset.png "automatische gebruikersinrichting inschakelen")

8. Op de **Security Token opnieuw** pagina, klikt u op de **Security Token opnieuw** knop.

    ![Schakel Automatische gebruikersaanvragen](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/sf-reset-token.png "automatische gebruikersinrichting inschakelen")

9. Controleer de Postvak die zijn gekoppeld aan dit admin-account. Zoek naar een e-mailbericht van Salesforce Sandbox.com waarin het nieuwe beveiligingstoken.

10. Kopiëren van het token, Ga naar uw Azure AD-venster en plak deze in de **geheim Token** veld.

11. Klik in de Azure-portal op **testverbinding** om te controleren of Azure AD, kan verbinding maken met uw Salesforce-Sandbox-app.

12. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet inrichting fout meldingen ontvangen en schakel het selectievakje in.

13. Klik op **opslaan.**  
    
14.  Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory: gebruikers aan de Salesforce Sandbox.**

15. In de **kenmerktoewijzingen** sectie, controleert u de kenmerken van de gebruiker die zijn gesynchroniseerd vanuit Azure AD bij Salesforce Sandbox. De kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen overeenkomen met de gebruikersaccounts in Salesforce Sandbox voor de update-bewerkingen worden gebruikt. Selecteer de knop Opslaan eventuele wijzigingen doorvoeren.

16. Om de Azure AD-service voor Salesforce Sandbox inricht, wijzigen de **inrichting Status** naar **op** in de sectie instellingen

17. Klik op **opslaan.**

De initiële synchronisatie van gebruikers en/of groepen die zijn toegewezen aan de Salesforce Sandbox in de sectie gebruikers en groepen wordt gestart. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer 20 minuten optreden als de service wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en volg de koppelingen voor het inrichten van de activiteitsrapporten, waarin alle acties die worden uitgevoerd door de inrichting service op de Salesforce Sandbox-app.

U kunt nu een testaccount maken. Wacht 20 minuten duren om te verifiëren dat het account is gesynchroniseerd met salesforce.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Het beheren van gebruikers account inrichten voor zakelijke Apps](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Eenmalige aanmelding configureren](https://docs.microsoft.com/azure/active-directory/active-directory-saas-salesforce-sandbox-tutorial)
