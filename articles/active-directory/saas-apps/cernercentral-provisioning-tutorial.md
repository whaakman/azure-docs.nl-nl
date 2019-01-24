---
title: 'Zelfstudie: Cerner centraal configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory om automatisch gebruikers inrichten op een schema Cerner centraal-India.
services: active-directory
documentationcenter: ''
author: asmalser-msft
writer: asmalser-msft
manager: daveba
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: asmalser-msft
ms.openlocfilehash: eb34a6e7ded4c31c623031104aabbee16b5c4f1c
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54819472"
---
# <a name="tutorial-configure-cerner-central-for-automatic-user-provisioning"></a>Zelfstudie: Cerner centraal configureren voor het automatisch inrichten van gebruikers

Het doel van deze zelfstudie is om weer te geven u de stappen die u uitvoeren in de Cerner centraal en Azure AD wilt voor het automatisch inrichten en verwijdering van gebruikersaccounts vanuit Azure AD naar een gebruiker Lesrooster Cerner centraal-India. 


## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure Active Directory-tenant
*   Een tenant Cerner-centraal 

> [!NOTE]
> Azure Active Directory kan worden geïntegreerd met Cerner Central met behulp van de [SCIM](http://www.simplecloud.info/) protocol.

## <a name="assigning-users-to-cerner-central"></a>Gebruikers toewijzen aan Cerner-centraal

Azure Active Directory maakt gebruik van een concept genaamd "toewijzingen" om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het inrichten van automatische gebruikersaccounts, worden alleen de gebruikers en groepen die '' aan een toepassing in Azure AD toegewezen zijn gesynchroniseerd. 

Voordat u configureren en inschakelen van de inrichtingsservice, moet u bepalen welke gebruikers en/of groepen in Azure AD de gebruikers die toegang nodig tot Cerner centraal staan. Als besloten, kunt u deze gebruikers toewijzen aan Cerner centraal door de instructies hier:

[Een gebruiker of groep toewijzen aan een enterprise-app](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-cerner-central"></a>Belangrijke tips voor het toewijzen van gebruikers aan Cerner-centraal

*   Het wordt aanbevolen dat één Azure AD-gebruiker worden toegewezen aan Cerner centraal voor het testen van de configuratie van de inrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

* Zodra de initiële test is voltooid voor één gebruiker, Cerner centraal raadt aan om de volledige lijst met gebruikers die bestemd zijn voor toegang tot een Cerner-oplossing (niet alleen Cerner centraal) in te richten op de Cerner gebruiker schema toe te wijzen.  Andere oplossingen Cerner gebruikmaken van deze lijst met gebruikers in de gebruikerslijst.

*   Bij het toewijzen van een gebruiker bij Cerner Central, moet u de **gebruiker** rol in het dialoogvenster toewijzing. Gebruikers met de rol 'standaardtoegang' worden uitgesloten van het inrichten.


## <a name="configuring-user-provisioning-to-cerner-central"></a>Inrichten van gebruikers naar Cerner centraal configureren

In deze sectie helpt u bij uw Azure AD verbinden met de Cerner centraal gebruiker schema met behulp van Cerner SCIM-gebruikersaccount Inrichtings-API, en configureren van de provisioning-service voor het maken, bijwerken en uitschakelen van de toegewezen gebruiker gebruikersaccounts Cerner centraal-India op basis van toewijzing van gebruikers en groepen in Azure AD.

> [!TIP]
> U kunt ook ingeschakeld SAML gebaseerde eenmalige aanmelding voor Cerner centraal, vindt u de instructies te volgen in [Azure-portal (https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies elkaar aanvullen. Zie voor meer informatie de [één centrale Cerner aanmeldings-zelfstudie](cernercentral-tutorial.md).


### <a name="to-configure-automatic-user-account-provisioning-to-cerner-central-in-azure-ad"></a>Het configureren van automatische inrichten van gebruikersaccounts bij Cerner Central in Azure AD:


Om gebruikersaccounts bij Cerner Central inricht, moet u het systeemaccount van een centrale Cerner aanvragen bij Cerner en het genereren van een OAuth-bearer-token die Azure AD gebruiken kunt om verbinding maken met de Cerner SCIM eindpunt. Het verdient ook dat de integratie in een Cerner sandbox-omgeving worden uitgevoerd voordat het in productie wilt nemen.

1.  De eerste stap is om te controleren of het beheren van de Cerner personen en Azure AD-integratie een CernerCare-account is vereist voor toegang tot de documentatie die nodig zijn voor de instructies. Indien nodig, gebruikt u de onderstaande URL's voor het maken van CernerCare accounts in elke omgeving van toepassing.

   * Sandbox:  https://sandboxcernercare.com/accounts/create

   * Productie:  https://cernercare.com/accounts/create  

2.  Vervolgens moet een systeem-account worden gemaakt voor Azure AD. Volg de onderstaande instructies om aan te vragen van een systeem-Account voor de sandbox- en productieomgevingen.

   * Instructies:  https://wiki.ucern.com/display/CernerCentral/Requesting+A+System+Account

   * Sandbox: https://sandboxcernercentral.com/system-accounts/

   * Productie:  https://cernercentral.com/system-accounts/

3.  Vervolgens genereert een bearer-token van OAuth voor elk van de systeemaccounts van uw. U doet dit door de onderstaande instructies uit te voeren.

   * Instructies:  https://wiki.ucern.com/display/public/reference/Accessing+Cerner%27s+Web+Services+Using+A+System+Account+Bearer+Token

   * Sandbox: https://sandboxcernercentral.com/system-accounts/

   * Productie:  https://cernercentral.com/system-accounts/

4. Tot slot moet u aan te schaffen gebruikers Lesrooster Realm-id's voor de sandbox en de productie-omgevingen in Cerner om de configuratie te voltooien. Zie voor meer informatie over het verkrijgen van dit: https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+SCIM. 

5. U kunt nu Azure AD voor het inrichten van gebruikersaccounts aan Cerner configureren. Aanmelden bij de [Azure-portal](https://portal.azure.com), en blader naar de **Azure Active Directory > Bedrijfsapps > alle toepassingen** sectie.

6. Als u al Cerner centraal hebt geconfigureerd voor eenmalige aanmelding, zoeken naar uw exemplaar van Cerner centraal met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar de **Cerner centraal** in de toepassingengalerie. Selecteer Cerner centraal in de resultaten voor zoeken en toe te voegen aan uw lijst met toepassingen.

7.  Selecteer uw exemplaar van de centrale Cerner en selecteer vervolgens de **Provisioning** tabblad.

8.  Stel de **Inrichtingsmodus** naar **automatische**.

   ![Cerner centraal inrichten](./media/cernercentral-provisioning-tutorial/Cerner.PNG)

9.  Vul de volgende velden onder **beheerdersreferenties**:

   * In de **Tenant-URL** en voer een URL in de notatie, "Gebruiker-schema-Realm-ID" vervangen door de realm-ID die u hebt verkregen in stap 4 #.

> Sandbox: https://user-roster-api.sandboxcernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/ 

> Productie: https://user-roster-api.cernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/ 

   * In de **geheim Token** veld, het OAuth-bearer-token die u in stap 3 # invoeren en op **testverbinding**.

   * U ziet een melding succes upperright aan van de portal.

10. Voer het e-mailadres van een persoon of groep die inrichting fout meldingen moet ontvangen de **e-mailmelding** veld en schakel het onderstaande selectievakje in.

11. Klik op **Opslaan**. 

12. In de **kenmerktoewijzingen** sectie, controleert u de gebruikers- en groepskenmerken van Azure AD worden gesynchroniseerd met Cerner centraal. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts en groepen Cerner centraal-India voor update-bewerkingen. Selecteer de knop Opslaan om door te voeren van eventuele wijzigingen.

13. Wijzigen zodat de Azure AD-inrichtingsservice voor Cerner centraal de **Inrichtingsstatus** naar **op** in de **instellingen** sectie

14. Klik op **Opslaan**. 

Hiermee start u de initiële synchronisatie van alle gebruikers en/of groepen die zijn toegewezen aan de centrale Cerner in de sectie gebruikers en groepen. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de Azure AD-inrichtingsservice wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van activiteitenlogboeken, waarin alle acties die worden uitgevoerd door de provisioning-service op uw app Cerner centraal worden beschreven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Cerner centraal: Publiceren van identiteitsgegevens met behulp van Azure AD](https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+Azure+AD)
* [Zelfstudie: Cerner centraal configureren voor eenmalige aanmelding met Azure Active Directory](cernercentral-tutorial.md)
* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](https://docs.microsoft.com/azure/active-directory/active-directory-saas-provisioning-reporting).
