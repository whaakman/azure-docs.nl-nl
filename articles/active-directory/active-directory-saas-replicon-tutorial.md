---
title: 'Zelfstudie: Azure Active Directory-integratie met Replicon | Microsoft Docs'
description: Meer informatie over het gebruik van Replicon met Azure Active Directory voor het inschakelen van eenmalige aanmelding, geautomatiseerde inrichting en meer!
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 02a62f15-917c-417c-8d80-fe685e3fd601
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: 2aeeceb61191962b62892b8409218684f76c6fa8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-replicon"></a>Zelfstudie: Azure Active Directory-integratie met Replicon
Het doel van deze zelfstudie is om de integratie van Azure en Replicon weer te geven. Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

* Een geldige Azure-abonnement
* Een tenant Replicon

Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan Replicon worden kunnen eenmalige aanmelding in de toepassing op uw Replicon bedrijf site (serviceprovider geïnitieerd aanmelding) of met behulp van de [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

Het scenario in deze zelfstudie bestaat uit de volgende elementen:

1. De integratie van toepassingen voor Replicon inschakelen
2. Configureren van eenmalige aanmelding (SSO)
3. Configuratie van gebruikers inrichten
4. Gebruikers toewijzen

![Scenario](./media/active-directory-saas-replicon-tutorial/IC777798.png "Scenario")

## <a name="enable-the-application-integration-for-replicon"></a>De integratie van toepassingen voor Replicon inschakelen
Het doel van deze sectie is het inschakelen van de integratie van toepassingen voor Replicon overzicht.

**Als u wilt de integratie van toepassingen voor Replicon inschakelen, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure-portal op het navigatiedeelvenster links op **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-replicon-tutorial/IC700993.png "Active Directory")
2. Van de **Directory** , selecteert u de map waarvoor u wilt inschakelen van Active directory-integratie.
3. De weergave toepassingen in de directoryweergave, klikt u op **toepassingen** in het menu bovenaan.
   
    ![Toepassingen](./media/active-directory-saas-replicon-tutorial/IC700994.png "toepassingen")
4. Klik op **toevoegen** aan de onderkant van de pagina.
   
    ![Toepassing toevoegen](./media/active-directory-saas-replicon-tutorial/IC749321.png "toepassing toevoegen")
5. Op de **wat wilt u doen** dialoogvenster, klikt u op **toevoegen van een toepassing uit de galerie**.
   
    ![Een toepassing toevoegen van gallerry](./media/active-directory-saas-replicon-tutorial/IC749322.png "een toepassing van gallerry toevoegen")
6. In de **zoekvak**, type **Replicon**.
   
    ![Toepassingsgalerie](./media/active-directory-saas-replicon-tutorial/IC777799.png "-toepassingsgalerie")
7. Selecteer in het deelvenster met resultaten **Replicon**, en klik vervolgens op **Complete** de toepassing toevoegen.
   
    ![Replicon](./media/active-directory-saas-replicon-tutorial/IC777800.png "Replicon")
   
## <a name="configure-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van deze sectie is om een overzicht van gebruikers om te verifiëren Replicon aan hun account in Azure AD dat gebruikmaakt van Federatie op basis van het SAML-protocol in staat te.

**Voor het configureren van eenmalige aanmelding, moet u de volgende stappen uitvoeren:**

1. In de klassieke Azure-portal op de **Replicon** toepassing Integratiepagina, klikt u op **eenmalige aanmelding configureren** openen de **configureren eenmalige aanmelding** dialoogvenster.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-replicon-tutorial/IC777801.png "eenmalige aanmelding configureren")
2. Op de **hoe wilt u dat gebruikers zich aanmelden op Replicon** pagina **Microsoft Azure AD Single Sign-On**, en klik vervolgens op **volgende**.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-replicon-tutorial/IC777802.png "eenmalige aanmelding configureren")
3. Op de **App-URL configureren** pagina, voert u de volgende stappen uit:
   
    ![App-URL configureren](./media/active-directory-saas-replicon-tutorial/IC777803.png "app-URL configureren")
  1. In de **Replicon aanmelding op URL** textbox, typt u uw Replicon tenant-URL (bijvoorbeeld: *https://na2.replicon.com/company/saml2/sp-sso/post*).
  2. In de **Replicon antwoord-URL** textbox, typt u uw Replicon **AssertionConsumerService** URL (bijvoorbeeld: *https://global.replicon.com/! / saml2/bedrijf/sso/post*).  
      
     >[!NOTE]
     >U kunt de URL van de metagegevens Replicon op ophalen: **https://global.replicon.com/! /saml2/\<YourCompanyKey\>**.
     > 
     > 
 
  3. Klik op **Volgende**.

4. Op de **eenmalige aanmelding configureren op Replicon** pagina voor het downloaden van de metagegevens, klikt u op **metagegevens downloaden**, en sla de metagegevens op uw computer.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-replicon-tutorial/IC777804.png "eenmalige aanmelding configureren")
5. In een ander browservenster, meld u bij uw bedrijf Replicon site als beheerder.

6. Voor het configureren van SAML 2.0, moet u de volgende stappen uitvoeren:
   
    ![SAML-verificatie inschakelen](./media/active-directory-saas-replicon-tutorial/IC777805.png "inschakelen SAML-verificatie")
  
  1. Om weer te geven de **EnableSAML Authentication2** dialoogvenster het volgende toevoegen aan de URL na de sleutel van uw bedrijf: **/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**  
    * Hieronder ziet u het schema van de volledige URL:  
   **https://Na2.replicon.com/\<YourCompanyKey\>/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**
   2. Klik op de  **+**  uit te breiden de **v20Configuration** sectie.
   3. Klik op de  **+**  uit te breiden de **metaDataConfiguration** sectie.
   4. Klik op **bestand kiezen**, zodat uw identiteit provider metagegevens-XML-bestand selecteren en op **indienen**.

7. Bij de klassieke Azure portal, selecteert u de configuratie voor één aanmelding bevestiging en klik op **Complete** sluiten de **configureren eenmalige aanmelding** dialoogvenster.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-replicon-tutorial/IC778418.png "eenmalige aanmelding configureren")
   
## <a name="configure-user-provisioning"></a>Gebruikers inrichten configureren

Om in te schakelen gebruikers van Azure AD aan te melden bij Replicon, moeten ze worden ingericht in Replicon.  

In het geval van Replicon is inrichting een handmatige taak.

**Als u wilt configureren voor gebruikers inrichten, moet u de volgende stappen uitvoeren:**

1. In een browservenster geopend, meld u bij uw bedrijf Replicon site als beheerder.
2. Ga naar **beheer \> gebruikers**.
   
    ![Gebruikers](./media/active-directory-saas-replicon-tutorial/IC777806.png "gebruikers")
3. Klik op **+ gebruiker toevoegen**.
   
    ![Gebruiker toevoegen](./media/active-directory-saas-replicon-tutorial/IC777807.png "gebruiker toevoegen")
4. In de **gebruikersprofiel** sectie, voert u de volgende stappen uit:
   
    ![Gebruikersprofiel](./media/active-directory-saas-replicon-tutorial/IC777808.png "gebruikersprofiel")
   
  1. In de **aanmeldingsnaam** textbox type de Azure AD-e-mailadres van de Azure AD-gebruiker die u inrichten wilt.
  2. Als **verificatietype**, selecteer **SSO**.
  3. In de **afdeling** textbox, typ de afdeling van de gebruiker.
  4. Als **werknemertype**, selecteer **beheerder**.
  5. Klik op **gebruikersprofiel opslaat**.

>[!NOTE]
>U kunt andere Replicon gebruiker account hulpmiddelen voor het maken of API's die is geleverd door Replicon aan inrichten AAD-gebruikersaccounts.
> 
> 

## <a name="assign-users"></a>Gebruikers toewijzen
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat met behulp van uw toepassing toegang tot deze door toe te wijzen.

**Gebruikers om aan te wijzen Replicon, moet u de volgende stappen uitvoeren:**

1. In de klassieke Azure portal een testaccount te maken.

2. Op de **Replicon** toepassing Integratiepagina, klikt u op **gebruikers toewijzen**.
   
    ![Gebruikers toewijzen](./media/active-directory-saas-replicon-tutorial/IC777809.png "gebruikers toewijzen")

3. Selecteer uw testgebruiker, klik op **toewijzen**, en klik vervolgens op **Ja** om te bevestigen dat de toewijzing.
   
    ![Ja](./media/active-directory-saas-replicon-tutorial/IC767830.png "Ja")

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het toegangsvenster. Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

