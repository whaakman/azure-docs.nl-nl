---
title: 'Zelfstudie: Salesforce configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Salesforce.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 49384b8b-3836-4eb1-b438-1c46bb9baf6f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: a35682c1a647039fbb946c0ea79d92e0d3806d0c
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44347219"
---
# <a name="tutorial-configure-salesforce-for-automatic-user-provisioning"></a>Zelfstudie: Salesforce configureren voor het automatisch inrichten van gebruikers

Het doel van deze zelfstudie is de stappen die nodig zijn om uit te voeren in Salesforce en Azure AD voor het automatisch inrichten en ongedaan maken inrichting gebruikersaccounts van Azure AD met Salesforce weer te geven.

## <a name="prerequisites"></a>Vereiste onderdelen

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure Active directory-tenant
*   Een Salesforce.com-tenant

>[!IMPORTANT] 
>Als u van een evaluatieversie Salesforce.com-account gebruikmaakt, klikt u vervolgens zich kunt u kan geen geautomatiseerde gebruikersinrichting configureren. Proefaccounts hoeft niet de vereiste API-toegang ingeschakeld totdat ze worden aangeschaft. U kunt deze beperking omzeilen met behulp van een gratis [ontwikkelaarsaccount](https://developer.salesforce.com/signup) om deze zelfstudie te voltooien.

Als u van een Salesforce-Sandbox-omgeving gebruikmaakt, raadpleegt u de [zelfstudie voor integratie van Salesforce Sandbox](https://go.microsoft.com/fwLink/?LinkID=521879).

## <a name="assigning-users-to-salesforce"></a>Gebruikers toewijzen aan Salesforce

Azure Active Directory maakt gebruik van een concept genaamd "toewijzingen" om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het inrichten van automatische gebruikersaccounts, wordt alleen de gebruikers en groepen die '' aan een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureren en inschakelen van de inrichtingsservice, moet u bepalen welke gebruikers of groepen in Azure AD toegang hebben tot uw Salesforce-app moeten. Nadat u deze beslissing hebt genomen, kunt u deze gebruikers toewijzen aan uw Salesforce-app door de instructies in [een gebruiker of groep toewijzen aan een enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-salesforce"></a>Belangrijke tips voor het toewijzen van gebruikers met Salesforce

*   Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan Salesforce voor het testen van de configuratie van de inrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*  Bij het toewijzen van een gebruiker met Salesforce, moet u een geldige gebruikersrol selecteren. De rol 'standaardtoegang' werkt niet voor het inrichten van

    > [!NOTE]
    > Deze app aangepaste rollen de invoer van Salesforce worden als onderdeel van het inrichtingsproces, de klant u wilt mogelijk bij het toewijzen van gebruikers

## <a name="enable-automated-user-provisioning"></a>Geautomatiseerde gebruikersinrichting inschakelen

In deze sectie helpt u bij uw Azure AD verbinden met de Salesforce-gebruikersaccount Inrichtings-API en configureren van de provisioning-service voor het maken, bijwerken en uitschakelen van toegewezen gebruikersaccounts in Salesforce op basis van gebruikers en groepen in Azure AD.

>[!Tip]
>U kunt ook op SAML gebaseerde eenmalige aanmelding ingeschakeld voor Salesforce, vindt u de instructies te volgen in [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies een fraaie aanvulling in elkaar.

### <a name="configure-automatic-user-account-provisioning"></a>Het inrichten van automatische gebruikersaccounts configureren

Het doel van deze sectie is om te beschrijven hoe u om in te schakelen met het inrichten van gebruikers van Active Directory-gebruikersaccounts met Salesforce.

1. In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory > Bedrijfsapps > alle toepassingen** sectie.

1. Als u al Salesforce hebt geconfigureerd voor eenmalige aanmelding, zoeken naar uw exemplaar van Salesforce met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar de **Salesforce** in de toepassingengalerie. Selecteer Salesforce in de resultaten voor zoeken en toe te voegen aan uw lijst met toepassingen.

1. Selecteer uw exemplaar van Salesforce en selecteer vervolgens de **Provisioning** tabblad.

1. Stel de **Inrichtingsmodus** naar **automatische**.

    ![inrichten](./media/salesforce-provisioning-tutorial/provisioning.png)

1. Onder de **beheerdersreferenties** sectie, geeft u de volgende configuratie-instellingen:
   
    a. In de **Admin Username** tekstvak, een Salesforce-account met de naam type de **systeembeheerder** profiel in Salesforce.com toegewezen.
   
    b. In de **beheerderswachtwoord** tekstvak typt u het wachtwoord voor dit account.

1. Als u de beveiliging van uw Salesforce-token, opent u een nieuw tabblad en meld u in de dezelfde Salesforce-beheerdersaccount. In de rechterbovenhoek van de pagina, klikt u op uw naam en klik vervolgens op **instellingen**.

     ![Automatische inrichting inschakelen](./media/salesforce-provisioning-tutorial/sf-my-settings.png "automatische inrichting inschakelen")

1. Klik in het linkernavigatiedeelvenster op **My Personal Information** de gerelateerde sectie uitvouwen en klik vervolgens op **opnieuw instellen van mijn Security Token**.
  
    ![Automatische inrichting inschakelen](./media/salesforce-provisioning-tutorial/sf-personal-reset.png "automatische inrichting inschakelen")

1. Op de **Security Token opnieuw** pagina, klikt u op **Security Token opnieuw** knop.

    ![Automatische inrichting inschakelen](./media/salesforce-provisioning-tutorial/sf-reset-token.png "automatische inrichting inschakelen")

1. Controleer het postvak in die zijn gekoppeld aan dit beheerdersaccount. Zoek naar een e-mailbericht van Salesforce.com met het nieuwe beveiligingstoken.

1. Kopieer het token, gaat u naar uw Azure AD-venster en plak deze in de **geheim Token** veld.

1. De **Tenant-URL** moet worden opgegeven als het exemplaar van Salesforce op de Salesforce Government-Cloud. Anders is optioneel. Voer de tenant-URL met behulp van de indeling van ' https://\<uw exemplaar\>. my.salesforce.com, "vervangen \<uw exemplaar\> met de naam van uw Salesforce-exemplaar.

1. Klik in de Azure-portal op **testverbinding** om te controleren of Azure AD kunt verbinden met uw Salesforce-app.

1. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die moet inrichten fout meldingen ontvangen, en schakel het onderstaande selectievakje in.

1. Klik op **opslaan.**  
    
1.  Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory: gebruikers met Salesforce.**

1. In de **kenmerktoewijzingen** sectie, controleert u de kenmerken van de gebruiker die worden gesynchroniseerd vanuit Azure AD met Salesforce. Houd er rekening mee dat de kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in Salesforce voor update-bewerkingen. Selecteer de knop Opslaan om door te voeren van eventuele wijzigingen.

1. Als wilt inschakelen in de Azure AD-inrichtingsservice voor Salesforce, wijzigt de **Inrichtingsstatus** naar **op** in de sectie instellingen

1. Klik op **opslaan.**

Hiermee start u de initiële synchronisatie van alle gebruikers en/of groepen die zijn toegewezen aan Salesforce in de sectie gebruikers en groepen. Houd er rekening mee dat de eerste synchronisatie langer dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden duurt als de service wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van activiteitenlogboeken, waarin alle acties die worden uitgevoerd door de provisioning-service op uw Salesforce-app worden beschreven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
* [Eenmalige aanmelding configureren](https://docs.microsoft.com/azure/active-directory/active-directory-saas-salesforce-tutorial)
