---
title: 'Zelfstudie: Salesforce configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Salesforce.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 49384b8b-3836-4eb1-b438-1c46bb9baf6f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: 3997d913525b44f154ca1e989ee1880308b82096
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
ms.locfileid: "34344249"
---
# <a name="tutorial-configure-salesforce-for-automatic-user-provisioning"></a>Zelfstudie: Salesforce configureren voor het automatisch gebruikers inrichten

Het doel van deze zelfstudie is om de stappen die nodig zijn om uit te voeren in Salesforce en Azure AD om automatisch te stellen en deactivering inrichten gebruikersaccounts vanuit Azure AD bij Salesforce.

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure Active directory-tenant
*   Een Salesforce.com-tenant

>[!IMPORTANT] 
>Als u van een Salesforce.com-proefaccount gebruikmaakt, klikt u vervolgens kunt u zich kan niet worden geconfigureerd geautomatiseerde gebruikersinrichting. Proefaccounts hoeft niet de benodigde API-toegang ingeschakeld totdat ze zijn aangeschaft. U kunt deze beperking omzeilen met behulp van een gratis [ontwikkelaarsaccount](https://developer.salesforce.com/signup) om deze zelfstudie te voltooien.

Als u van een Salesforce Sandbox-omgeving gebruikmaakt, raadpleegt u de [Salesforce Sandbox integratie zelfstudie](https://go.microsoft.com/fwLink/?LinkID=521879).

## <a name="assigning-users-to-salesforce"></a>Gebruikers toewijzen aan Salesforce

Azure Active Directory gebruikt een concept 'toewijzingen' genoemd om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van automatische gebruikers account inrichten, alleen de gebruikers en groepen die '' tot een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureren en inschakelen van de inrichting service, moet u bepalen welke gebruikers of groepen in Azure AD toegang hebben tot uw Salesforce-app moeten. Nadat u deze beslissing hebt genomen, kunt u deze gebruikers toewijzen aan uw Salesforce-app door de instructies in [een gebruiker of groep toewijzen aan een enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-salesforce"></a>Belangrijke tips voor het toewijzen van gebruikers aan Salesforce

*   Het is raadzaam om één Azure AD-gebruiker is toegewezen aan Salesforce voor het testen van de configuratie van de inrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*  Wanneer een gebruiker aan Salesforce toewijzen, moet u een geldige gebruikersrol selecteren. De rol 'Default toegang' werkt niet voor inrichting

    > [!NOTE]
    > Deze app kunt u aangepaste rollen uit Salesforce als onderdeel van het inrichtingsproces, de klant selecteren kunt bij het toewijzen van gebruikers importeren

## <a name="enable-automated-user-provisioning"></a>Geautomatiseerde gebruikersinrichting inschakelen

Deze sectie helpt u bij uw Azure AD verbinden met Salesforce van gebruikersaccount inrichten API en configureren van de inrichting service te maken, bijwerken en uitschakelen van toegewezen gebruikersaccounts in Salesforce op basis van gebruikers en groepen toewijzen in Azure AD.

>[!Tip]
>U kunt ook op basis van SAML eenmalige aanmelding is ingeschakeld voor Salesforce, vindt u de instructies te volgen in [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies aanvulling van elkaar.

### <a name="configure-automatic-user-account-provisioning"></a>Automatisch gebruikers inrichten van account configureren

Het doel van deze sectie is het inschakelen van de gebruiker het inrichten van Active Directory-gebruikersaccounts met Salesforce overzicht.

1. In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory > zakelijke Apps > alle toepassingen** sectie.

2. Als u al Salesforce voor eenmalige aanmelding hebt geconfigureerd, kunt u zoeken naar uw exemplaar van Salesforce met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar **Salesforce** in de galerie met toepassingen. Salesforce selecteert in de zoekresultaten en toe te voegen aan uw lijst met toepassingen.

3. Selecteer uw exemplaar van Salesforce en selecteer vervolgens de **inrichten** tabblad.

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![inrichten](./media/active-directory-saas-salesforce-provisioning-tutorial/provisioning.png)

5. Onder de **beheerdersreferenties** sectie, bieden de volgende configuratie-instellingen:
   
    a. In de **Admin Username** textbox type een Salesforce-accountnaam met de **systeembeheerder** profiel in Salesforce.com toegewezen.
   
    b. In de **beheerderswachtwoord** textbox, typt u het wachtwoord voor dit account.

6. Als u de beveiliging van uw Salesforce-token, open een nieuw tabblad en meld u aan dezelfde Salesforce-admin-account. In de rechterbovenhoek van de pagina, klikt u op uw naam en klik vervolgens op **instellingen**.

     ![Schakel Automatische gebruikersaanvragen](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-my-settings.png "automatische gebruikersinrichting inschakelen")

7. Klik in het navigatiedeelvenster links op **mijn persoonlijke gegevens** Vouw de bijbehorende sectie en klik vervolgens op **opnieuw mijn beveiligingstoken**.
  
    ![Schakel Automatische gebruikersaanvragen](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-personal-reset.png "automatische gebruikersinrichting inschakelen")

8. Op de **Security Token opnieuw** pagina, klikt u op **Security Token opnieuw** knop.

    ![Schakel Automatische gebruikersaanvragen](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-reset-token.png "automatische gebruikersinrichting inschakelen")

9. Controleer de Postvak die zijn gekoppeld aan dit admin-account. Zoek naar een e-mailbericht van Salesforce.com waarin het nieuwe beveiligingstoken.

10. Kopiëren van het token, Ga naar uw Azure AD-venster en plak deze in de **geheim Token** veld.

11. De **Tenant-URL** moet worden opgegeven als het exemplaar van Salesforce in de Cloud van de overheid Salesforce. Anders is optioneel. Voer de URL van de tenant met behulp van de indeling van https://your-instance.my.salesforce.com, uw instantie vervangen door de naam van uw Salesforce-exemplaar.

12. Klik in de Azure-portal op **testverbinding** om te controleren of Azure AD, kan verbinding maken met uw Salesforce-app.

13. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet inrichting fout meldingen ontvangen en schakel het selectievakje hieronder in.

14. Klik op **opslaan.**  
    
15.  Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory-gebruikers Salesforce.**

16. In de **kenmerktoewijzingen** sectie, controleert u de kenmerken van de gebruiker van Azure AD worden gesynchroniseerd met Salesforce. Let op de kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen overeenkomen met de gebruikersaccounts in Salesforce voor update-bewerkingen worden gebruikt. Selecteer de knop Opslaan eventuele wijzigingen doorvoeren.

17. Om de Azure AD-service voor Salesforce inricht, wijzigen de **inrichting Status** naar **op** in de sectie instellingen

18. Klik op **opslaan.**

Hiermee start u de initiële synchronisatie van gebruikers en/of groepen die zijn toegewezen aan Salesforce in de sectie gebruikers en groepen. Houd er rekening mee dat de eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de service wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en volg de koppelingen voor het inrichten van activiteitenlogboeken waarin alle acties die worden uitgevoerd door de inrichting service op uw Salesforce-app.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over automatische account gebruikersaanvragen](active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Het beheren van gebruikers account inrichten voor zakelijke Apps](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](manage-apps/what-is-single-sign-on.md)
* [Eenmalige aanmelding configureren](https://docs.microsoft.com/azure/active-directory/active-directory-saas-salesforce-tutorial)
