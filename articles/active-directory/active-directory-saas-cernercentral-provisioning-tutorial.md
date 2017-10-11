---
title: 'Zelfstudie: Cerner centraal configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory automatisch inrichten-beheerders kunnen een schema in Cerner centraal.
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: stevenpo
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: asmalser-msft
ms.openlocfilehash: 84613b7f8d7bd031d492a62da0bc53be96ac45a3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="tutorial-configuring-cerner-central-for-automatic-user-provisioning"></a>Zelfstudie: Cerner centraal configureren voor het automatisch gebruikers inrichten

Het doel van deze zelfstudie is zodat u de stappen die u uitvoeren in Cerner centraal en Azure AD wilt om automatisch in te richten en inrichten van gebruikersaccounts vanuit Azure AD naar een schema van de gebruiker in Cerner centrale ongedaan. 


## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure Active Directory-tenant
*   Een tenant Cerner-centraal 

> [!NOTE]
> Azure Active Directory is geïntegreerd met Cerner centraal met behulp van de [SCIM](http://www.simplecloud.info/) protocol.

## <a name="assigning-users-to-cerner-central"></a>Gebruikers toewijzen aan Cerner-centraal

Azure Active Directory gebruikt een concept 'toewijzingen' genoemd om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van automatische gebruikers account inrichten, worden alleen de gebruikers en groepen die '' tot een toepassing in Azure AD toegewezen zijn gesynchroniseerd. 

Voordat u configureren en inschakelen van de inrichting service, moet u bepalen welke gebruikers en/of groepen in Azure AD de gebruikers die toegang nodig tot Cerner centraal vertegenwoordigen. Als besloten, kunt u deze gebruikers toewijzen aan Cerner centraal door de volgende instructies te volgen:

[Een gebruiker of groep toewijzen aan een enterprise-app](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-cerner-central"></a>Belangrijke tips voor het toewijzen van gebruikers aan Cerner-centraal

*   Het is raadzaam om één Azure AD-gebruiker worden toegewezen aan Cerner centraal voor het testen van de configuratie van de inrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

* Zodra het eerste testen is voltooid voor één gebruiker Cerner centraal raadt aan om de volledige lijst met gebruikers die bestemd zijn voor toegang tot een Cerner-oplossing (niet alleen Cerner centraal) om te worden ingericht op de gebruikerslijst van Cerner toewijzen.  Andere oplossingen Cerner gebruikmaken van deze lijst van gebruikers in de gebruikerslijst.

*   Wanneer een gebruiker aan Cerner centraal toewijzen, moet u de **gebruiker** rol in het dialoogvenster toewijzing. Gebruikers met de rol 'Default toegang' zijn uitgesloten van de inrichting.


## <a name="configuring-user-provisioning-to-cerner-central"></a>Gebruikersaanvragen voor Cerner centraal configureren

Deze sectie helpt u bij het verbinding maken met uw Azure AD Cerner centraal van gebruiker schema van Cerner SCIM gebruikersaccount gebruikt API-inrichting, en configureren van de inrichting service te maken, bijwerken en uitschakelen van de toegewezen gebruiker accounts in Cerner centrale op basis van toewijzing van gebruikers en groepen in Azure AD.

> [!TIP]
> U kunt ook op basis van SAML eenmalige aanmelding is ingeschakeld voor Cerner centraal, volgt de instructies in [Azure-portal (https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies elkaar aanvullen. Zie voor meer informatie de [één centrale Cerner aanmelding zelfstudie](active-directory-saas-cernercentral-tutorial.md).


### <a name="to-configure-automatic-user-account-provisioning-to-cerner-central-in-azure-ad"></a>Voor het configureren van automatische account gebruikersaanvragen naar Cerner centraal in Azure AD:


Gebruikersaccounts aan Cerner centraal inricht, hebt u nodig voor een systeemaccount Cerner centraal aanvragen bij Cerner en het genereren van een OAuth bearer-token die Azure AD gebruiken kunt voor het verbinding maken met de Cerner SCIM eindpunt. Het is ook raadzaam de integratie worden uitgevoerd in een Cerner sandbox-omgeving voordat u implementeert naar productie.

1.  De eerste stap is om te controleren of de mensen die het beheer van de Cerner en Azure AD-integratie hebben een CernerCare-account is vereist voor toegang tot de documentatie die nodig zijn voor de instructies. Gebruik de onderstaande URL's CernerCare accounts maken in elke omgeving van toepassing, indien nodig.

   * Sandbox: https://sandboxcernercare.com/accounts/create

   * Productie: https://cernercare.com/accounts/create  

2.  Vervolgens moet een systeem-account worden gemaakt voor Azure AD. Volg de onderstaande instructies om aan te vragen van een systeem-Account voor uw sandbox en productie-omgevingen.

   * Instructies: https://wiki.ucern.com/display/CernerCentral/Requesting+A+System+Account

   * Sandbox: https://sandboxcernercentral.com/system-accounts/

   * Productie: https://cernercentral.com/system-accounts/

3.  Vervolgens een OAuth bearer-token genereren voor elk van uw systeemaccounts. U doet dit door de onderstaande instructies uit te voeren.

   * Instructies: https://wiki.ucern.com/display/public/reference/Accessing+Cerner%27s+Web+Services+Using+A+System+Account+Bearer+Token

   * Sandbox: https://sandboxcernercentral.com/system-accounts/

   * Productie: https://cernercentral.com/system-accounts/

4. Tot slot moet u aan te schaffen gebruikersnamen schema Realm voor de sandbox en de productie-omgevingen in Cerner om de configuratie te voltooien. Zie voor meer informatie over het verkrijgen van dit: https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+SCIM. 

5. Nu kunt u Azure AD om gebruikersaccounts in inrichten op Cerner te configureren. Aanmelden bij de [Azure-portal](https://portal.azure.com), en blader naar de **Azure Active Directory > zakelijke Apps > alle toepassingen** sectie.

6. Als u al Cerner centraal hebt geconfigureerd voor eenmalige aanmelding, zoekt u uw exemplaar van Cerner centraal met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar **Cerner centraal** in de galerie met toepassingen. Cerner centraal selecteert in de zoekresultaten en toe te voegen aan uw lijst met toepassingen.

7.  Selecteer uw exemplaar van de centrale Cerner en selecteer vervolgens de **inrichten** tabblad.

8.  Stel de **Inrichtingsmodus** naar **automatische**.

   ![Cerner centraal inrichten](./media/active-directory-saas-cernercentral-provisioning-tutorial/Cerner.PNG)

9.  Vul de volgende velden onder **beheerdersreferenties**:

   * In de **Tenant-URL** en voer een URL in de notatie, "Gebruiker-schema-Realm-ID" vervangen door de realm-ID die u hebt verkregen in stap &#4;.

> Sandbox: https://user-roster-api.sandboxcernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/ 

> Productie: https://user-roster-api.cernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/ 

   * In de **geheim Token** veld, voert u het OAuth bearer-token die u in stap &#3; gegenereerd en klikt u op **testverbinding**.

   * U ziet een melding met succes de upperright-zijde van de portal.

10. Voer het e-mailadres van een persoon of groep die in inrichting fout meldingen moet ontvangen de **e-mailmelding** veld en schakel het selectievakje hieronder in.

11. Klik op **Opslaan**. 

12. In de **kenmerktoewijzingen** sectie, moet u de gebruikers- en groepskenmerken moeten worden gesynchroniseerd vanuit Azure AD naar Cerner centraal controleren. De kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen overeenkomen met de gebruikersaccounts en groepen in de centrale Cerner voor update-bewerkingen worden gebruikt. Selecteer de knop Opslaan eventuele wijzigingen doorvoeren.

13. Om de Azure AD-service voor het hoofdkantoor Cerner inricht, wijzigen de **inrichting Status** naar **op** in de **instellingen** sectie

14. Klik op **Opslaan**. 

Hiermee start u de initiële synchronisatie van gebruikers en/of groepen die zijn toegewezen aan de centrale Cerner in de sectie gebruikers en groepen. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer 20 minuten optreden als de Azure AD-service inricht wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en volg de koppelingen voor het inrichten van de activiteitsrapporten, waarin alle acties die worden uitgevoerd door de inrichting service op uw app Cerner centraal beschrijven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over automatische account gebruikersaanvragen](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Cerner centraal: Publiceren van identiteitsgegevens met behulp van Azure AD](https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+Azure+AD)
* [Zelfstudie: Cerner centraal configureren voor eenmalige aanmelding bij Azure Active Directory](active-directory-saas-cernercentral-tutorial.md)
* [Het beheren van gebruikers account inrichten voor zakelijke Apps](active-directory-enterprise-apps-manage-provisioning.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Volgende stappen
* [Informatie over het bekijken van Logboeken en rapporten over het inrichten van de activiteit](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).
