---
title: 'Zelfstudie: Salesforce configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Ontdek hoe u eenmalige aanmelding configureert tussen Azure Active Directory en Salesforce.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 49384b8b-3836-4eb1-b438-1c46bb9baf6f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 712cc5ce62225987f8cc3ea13b5e4fd10a7d5eaf
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56172710"
---
# <a name="tutorial-configure-salesforce-for-automatic-user-provisioning"></a>Zelfstudie: Salesforce voor het automatisch inrichten van gebruikers configureren

Het doel van deze zelfstudie is de stappen die nodig zijn om uit te voeren in Salesforce en Azure AD voor het automatisch inrichten en ongedaan maken inrichting gebruikersaccounts van Azure AD met Salesforce weer te geven.

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

* Een Azure Active directory-tenant
* Een Salesforce.com-tenant

> [!IMPORTANT]
> Als u van een evaluatieversie Salesforce.com-account gebruikmaakt, klikt u vervolgens zich kunt u kan geen geautomatiseerde gebruikersinrichting configureren. Proefaccounts hoeft niet de vereiste API-toegang ingeschakeld totdat ze worden aangeschaft. U kunt deze beperking omzeilen met behulp van een gratis [ontwikkelaarsaccount](https://developer.salesforce.com/signup) om deze zelfstudie te voltooien.

Als u van een Salesforce-Sandbox-omgeving gebruikmaakt, raadpleegt u de [zelfstudie voor integratie van Salesforce Sandbox](https://go.microsoft.com/fwLink/?LinkID=521879).

## <a name="assigning-users-to-salesforce"></a>Gebruikers toewijzen aan Salesforce

Azure Active Directory maakt gebruik van een concept genaamd "toewijzingen" om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het inrichten van automatische gebruikersaccounts, wordt alleen de gebruikers en groepen die '' aan een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureren en inschakelen van de inrichtingsservice, moet u bepalen welke gebruikers of groepen in Azure AD toegang hebben tot uw Salesforce-app moeten. Nadat u deze beslissing hebt genomen, kunt u deze gebruikers toewijzen aan uw Salesforce-app door de instructies in [een gebruiker of groep toewijzen aan een enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-salesforce"></a>Belangrijke tips voor het toewijzen van gebruikers met Salesforce

* Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan Salesforce voor het testen van de configuratie van de inrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

* Bij het toewijzen van een gebruiker met Salesforce, moet u een geldige gebruikersrol selecteren. De rol 'standaardtoegang' werkt niet voor het inrichten van

    > [!NOTE]
    > Deze app importeert profielen van Salesforce worden als onderdeel van het inrichtingsproces, de klant u wilt mogelijk bij het toewijzen van gebruikers in Azure AD. Houd er rekening mee dat de profielen die van Salesforce worden geïmporteerd als rollen in Azure AD weergeven.

## <a name="enable-automated-user-provisioning"></a>Geautomatiseerde gebruikersinrichting inschakelen

In deze sectie helpt u bij uw Azure AD verbinden met de Salesforce-gebruikersaccount Inrichtings-API en configureren van de provisioning-service voor het maken, bijwerken en uitschakelen van toegewezen gebruikersaccounts in Salesforce op basis van gebruikers en groepen in Azure AD.

> [!Tip]
> U kunt ook op SAML gebaseerde eenmalige aanmelding ingeschakeld voor Salesforce, vindt u de instructies te volgen in [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies een fraaie aanvulling in elkaar.

### <a name="configure-automatic-user-account-provisioning"></a>Het inrichten van automatische gebruikersaccounts configureren

Het doel van deze sectie is om te beschrijven hoe u om in te schakelen met het inrichten van gebruikers van Active Directory-gebruikersaccounts met Salesforce.

1. In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory > Bedrijfsapps > alle toepassingen** sectie.

2. Als u al Salesforce hebt geconfigureerd voor eenmalige aanmelding, zoeken naar uw exemplaar van Salesforce met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar de **Salesforce** in de toepassingengalerie. Selecteer Salesforce in de resultaten voor zoeken en toe te voegen aan uw lijst met toepassingen.

3. Selecteer uw exemplaar van Salesforce en selecteer vervolgens de **Provisioning** tabblad.

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![inrichten](./media/salesforce-provisioning-tutorial/provisioning.png)

5. Onder de **beheerdersreferenties** sectie, geeft u de volgende configuratie-instellingen:

    a. In de **Admin Username** tekstvak, een Salesforce-account met de naam type de **systeembeheerder** profiel in Salesforce.com toegewezen.

    b. In de **beheerderswachtwoord** tekstvak typt u het wachtwoord voor dit account.

6. Als u de beveiliging van uw Salesforce-token, opent u een nieuw tabblad en meld u in de dezelfde Salesforce-beheerdersaccount. In de rechterbovenhoek van de pagina, klikt u op uw naam en klik vervolgens op **instellingen**.

    ![Automatische inrichting inschakelen](./media/salesforce-provisioning-tutorial/sf-my-settings.png "automatische inrichting inschakelen")

7. Klik in het linkernavigatiedeelvenster op **My Personal Information** de gerelateerde sectie uitvouwen en klik vervolgens op **opnieuw instellen van mijn Security Token**.
  
    ![Automatische inrichting inschakelen](./media/salesforce-provisioning-tutorial/sf-personal-reset.png "automatische inrichting inschakelen")

8. Op de **Security Token opnieuw** pagina, klikt u op **Security Token opnieuw** knop.

    ![Automatische inrichting inschakelen](./media/salesforce-provisioning-tutorial/sf-reset-token.png "automatische inrichting inschakelen")

9. Controleer het postvak in die zijn gekoppeld aan dit beheerdersaccount. Zoek naar een e-mailbericht van Salesforce.com met het nieuwe beveiligingstoken.

10. Kopieer het token, gaat u naar uw Azure AD-venster en plak deze in de **geheim Token** veld.

11. De **Tenant-URL** moet worden opgegeven als het exemplaar van Salesforce op de Salesforce Government-Cloud. Anders is optioneel. Voer de tenant-URL met behulp van de indeling van ' https://\<uw exemplaar\>. my.salesforce.com, "vervangen \<uw exemplaar\> met de naam van uw Salesforce-exemplaar.

12. Klik in de Azure-portal op **testverbinding** om te controleren of Azure AD kunt verbinden met uw Salesforce-app.

13. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die moet inrichten fout meldingen ontvangen, en schakel het onderstaande selectievakje in.

14. Klik op **opslaan.**  

15. Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory: gebruikers met Salesforce.**

16. In de **kenmerktoewijzingen** sectie, controleert u de kenmerken van de gebruiker die worden gesynchroniseerd vanuit Azure AD met Salesforce. Houd er rekening mee dat de kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in Salesforce voor update-bewerkingen. Selecteer de knop Opslaan om door te voeren van eventuele wijzigingen.

17. Als wilt inschakelen in de Azure AD-inrichtingsservice voor Salesforce, wijzigt de **Inrichtingsstatus** naar **op** in de sectie instellingen

18. Klik op **opslaan.**

Hiermee start u de initiële synchronisatie van alle gebruikers en/of groepen die zijn toegewezen aan Salesforce in de sectie gebruikers en groepen. Houd er rekening mee dat de eerste synchronisatie langer dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden duurt als de service wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van activiteitenlogboeken, waarin alle acties die worden uitgevoerd door de provisioning-service op uw Salesforce-app worden beschreven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
* [Eenmalige aanmelding configureren](https://docs.microsoft.com/azure/active-directory/active-directory-saas-salesforce-tutorial)
