---
title: 'Zelfstudie: Sandbox-Salesforce configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Salesforce Sandbox.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: bab73fda-6754-411d-9288-f73ecdaa486d
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: b1ad53c4ba1b79a1918177ce73862b603d157153
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44346500"
---
# <a name="tutorial-configure-salesforce-sandbox-for-automatic-user-provisioning"></a>Zelfstudie: Sandbox-Salesforce configureren voor het automatisch inrichten van gebruikers

Het doel van deze zelfstudie is om weer te geven u de stappen die u uitvoeren in de Sandbox voor Salesforce en Azure AD wilt voor automatisch inrichten en verwijdering van gebruikersaccounts vanuit Azure AD naar Salesforce Sandbox.

## <a name="prerequisites"></a>Vereiste onderdelen

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure Active directory-tenant.
*   Een geldige tenant voor Salesforce-Sandbox voor werk- of Salesforce Sandbox voor onderwijs. U kunt een gratis proefaccount voor beide services.
*   Een gebruikersaccount in Salesforce Sandbox met Team beheerdersmachtigingen.

## <a name="assigning-users-to-salesforce-sandbox"></a>Gebruikers toewijzen aan Salesforce-Sandbox

Azure Active Directory maakt gebruik van een concept genaamd "toewijzingen" om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het inrichten van automatische gebruikersaccounts, worden alleen de gebruikers en groepen die '' aan een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureren en inschakelen van de inrichtingsservice, moet u bepalen welke gebruikers of groepen in Azure AD toegang hebben tot uw Salesforce-Sandbox-app moeten. Nadat u deze beslissing hebt genomen, kunt u deze gebruikers toewijzen aan uw Salesforce-Sandbox-app door de instructies in [een gebruiker of groep toewijzen aan een enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-salesforce-sandbox"></a>Belangrijke tips voor het toewijzen van gebruikers aan Salesforce-Sandbox

* Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan Salesforce Sandbox voor het testen van de configuratie van de inrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

* Wanneer een gebruiker aan Salesforce Sandbox toewijzen, moet u een geldige gebruikersrol selecteren. De rol 'standaardtoegang' werkt niet voor het inrichten.

> [!NOTE]
> Deze app importeert aangepaste rollen van Salesforce Sandbox als onderdeel van het inrichtingsproces, de klant u wilt mogelijk bij het toewijzen van gebruikers.

## <a name="enable-automated-user-provisioning"></a>Geautomatiseerde gebruikersinrichting inschakelen

In deze sectie helpt u bij uw Azure AD verbinding te maken met Salesforce-Sandbox-gebruikersaccount Inrichtings-API, en configureren van de provisioning-service voor het maken, bijwerken en uitschakelen van de toegewezen gebruiker-accounts in Salesforce-Sandbox op basis van gebruikers en groepen toewijzing in Azure AD.

>[!Tip]
>U kunt ook op SAML gebaseerde eenmalige aanmelding ingeschakeld voor Salesforce-Sandbox, vindt u de instructies te volgen in [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies een fraaie aanvulling in elkaar.

### <a name="configure-automatic-user-account-provisioning"></a>Het inrichten van automatische gebruikersaccounts configureren

Het doel van deze sectie is om een overzicht van het inrichten van gebruikers van Active Directory-gebruikersaccounts aan Salesforce Sandbox inschakelen te.

1. In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory > Bedrijfsapps > alle toepassingen** sectie.

1. Als u Salesforce Sandbox voor eenmalige aanmelding al hebt geconfigureerd, kunt u zoeken naar uw exemplaar van Salesforce-Sandbox met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar de **Salesforce Sandbox** in de toepassingengalerie. Salesforce-sandbox-Selecteer in de resultaten voor zoeken en toe te voegen aan uw lijst met toepassingen.

1. Selecteer uw exemplaar van Salesforce Sandbox, en selecteer vervolgens de **Provisioning** tabblad.

1. Stel de **Inrichtingsmodus** naar **automatische**.

    ![inrichten](./media/salesforce-sandbox-provisioning-tutorial/provisioning.png)

1. Onder de **beheerdersreferenties** sectie, geeft u de volgende configuratie-instellingen:
   
    a. In de **Admin Username** tekstvak, type een Salesforce-Sandbox accountnaam waarvoor de **systeembeheerder** profiel in Salesforce.com toegewezen.
   
    b. In de **beheerderswachtwoord** tekstvak typt u het wachtwoord voor dit account.

1. Als u de beveiliging van uw Salesforce-sandbox-token, open een nieuw tabblad en meld u bij het dezelfde Salesforce sandbox-admin-account. In de rechterbovenhoek van de pagina, klikt u op uw naam en klik vervolgens op **instellingen**.

     ![Automatische inrichting inschakelen](./media/salesforce-sandbox-provisioning-tutorial/sf-my-settings.png "automatische inrichting inschakelen")

1. Klik in het linkernavigatiedeelvenster op **My Personal Information** de gerelateerde sectie uitvouwen en klik vervolgens op **opnieuw instellen van mijn Security Token**.
  
    ![Automatische inrichting inschakelen](./media/salesforce-sandbox-provisioning-tutorial/sf-personal-reset.png "automatische inrichting inschakelen")

1. Op de **Security Token opnieuw** pagina, klikt u op de **Security Token opnieuw** knop.

    ![Automatische inrichting inschakelen](./media/salesforce-sandbox-provisioning-tutorial/sf-reset-token.png "automatische inrichting inschakelen")

1. Controleer het postvak in die zijn gekoppeld aan dit beheerdersaccount. Zoek naar een e-mailbericht uit Salesforce Sandbox.com met het nieuwe beveiligingstoken.

1. Kopieer het token, gaat u naar uw Azure AD-venster en plak deze in de **geheim Token** veld.

1. Klik in de Azure-portal op **testverbinding** om te controleren of Azure AD kunt verbinden met uw Salesforce-Sandbox-app.

1. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die moet inrichten fout meldingen ontvangen, en schakel het selectievakje in.

1. Klik op **opslaan.**  
    
1.  Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory: gebruikers aan Salesforce Sandbox.**

1. In de **kenmerktoewijzingen** sectie, controleert u de kenmerken van de gebruiker die worden gesynchroniseerd vanuit Azure AD naar Salesforce Sandbox. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in Salesforce Sandbox voor update-bewerkingen. Selecteer de knop Opslaan om door te voeren van eventuele wijzigingen.

1. Om in te schakelen in de Azure AD-inrichtingsservice voor Salesforce-Sandbox, wijzigen de **Inrichtingsstatus** naar **op** in de sectie instellingen

1. Klik op **opslaan.**

De initiële synchronisatie van alle gebruikers en/of groepen die zijn toegewezen aan de Salesforce Sandbox in de sectie gebruikers en groepen wordt gestart. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de service wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van activiteitenlogboeken, waarin alle acties die worden uitgevoerd door de inrichtingsservice in Salesforce Sandbox-app worden beschreven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
* [Eenmalige aanmelding configureren](https://docs.microsoft.com/azure/active-directory/active-directory-saas-salesforce-sandbox-tutorial)
