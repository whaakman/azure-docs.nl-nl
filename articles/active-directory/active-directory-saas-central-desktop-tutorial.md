---
title: 'Zelfstudie: Azure Active Directory-integratie met centraal bureaublad | Microsoft Docs'
description: Meer informatie over het centrale bureaublad gebruiken met Azure Active Directory voor het inschakelen van eenmalige aanmelding, geautomatiseerde inrichting en meer!
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: b805d485-93db-49b4-807a-18d446c7090e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: fe32c1d68040ceb9d9de2ad6c4a6dc9ea93f5aef
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-central-desktop"></a>Zelfstudie: Azure Active Directory-integratie met centraal bureaublad
Het doel van deze zelfstudie is om de integratie van Azure en centrale bureaublad weer te geven. Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

* Een geldige Azure-abonnement
* Een centrale bureaublad eenmalige aanmelding ingeschakeld abonnement / tenant-centrale bureaublad

Het scenario in deze zelfstudie bestaat uit de volgende elementen:

* De integratie van toepassingen voor centrale bureaublad inschakelen
* Configureren van eenmalige aanmelding (SSO)
* Configuratie van gebruikers inrichten
* Gebruikers toewijzen

![Scenario](./media/active-directory-saas-central-desktop-tutorial/IC769558.png "Scenario")

## <a name="enable-the-application-integration-for-central-desktop"></a>De toepassingsintegratie van de voor centrale bureaublad inschakelen
Het doel van deze sectie is het inschakelen van de integratie van toepassingen voor centrale bureaublad overzicht.

**Schakel de integratie van toepassingen voor centrale bureaublad door de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure-portal op het navigatiedeelvenster links op **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-central-desktop-tutorial/IC700993.png "Active Directory")
2. Van de **Directory** , selecteert u de map waarvoor u wilt inschakelen van Active directory-integratie.
3. De weergave toepassingen in de directoryweergave, klikt u op **toepassingen** in het menu bovenaan.
   
   ![Toepassingen](./media/active-directory-saas-central-desktop-tutorial/IC700994.png "toepassingen")
4. Klik op **toevoegen** aan de onderkant van de pagina.
   
   ![Toepassing toevoegen](./media/active-directory-saas-central-desktop-tutorial/IC749321.png "toepassing toevoegen")
5. Op de **wat wilt u doen** dialoogvenster, klikt u op **toevoegen van een toepassing uit de galerie**.
   
   ![Een toepassing toevoegen van gallerry](./media/active-directory-saas-central-desktop-tutorial/IC749322.png "een toepassing van gallerry toevoegen")
6. In de **zoekvak**, type **centrale bureaublad**.
   
   ![Toepassingsgalerie](./media/active-directory-saas-central-desktop-tutorial/IC769559.png "-toepassingsgalerie")
7. Selecteer in het deelvenster met resultaten **centrale bureaublad**, en klik vervolgens op **Complete** de toepassing toevoegen.
   
   ![Centrale bureaublad](./media/active-directory-saas-central-desktop-tutorial/IC769560.png "centrale bureaublad")
   
## <a name="configure-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van deze sectie is om een overzicht van gebruikers te verifiëren naar centrale bureaublad met hun account in Azure AD dat gebruikmaakt van Federatie op basis van het SAML-protocol in staat te.

Als onderdeel van deze procedure zich u moet een Base64-gecodeerde certificaat uploaden naar uw tenant centrale bureaublad.  
Als u niet bekend met deze procedure bent, Zie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

**Voor het configureren van eenmalige aanmelding, moet u de volgende stappen uitvoeren:**

1. In de klassieke Azure-portal op de **centrale bureaublad** toepassing Integratiepagina, klikt u op **eenmalige aanmelding configureren** openen de ** configureren eenmalige aanmelding ** dialoogvenster.
   
   ![Eenmalige aanmelding configureren](./media/active-directory-saas-central-desktop-tutorial/IC749323.png "eenmalige aanmelding configureren")
2. Op de **hoe wilt u dat gebruikers zich aanmelden op voor de centrale bureaublad** pagina **Microsoft Azure AD Single Sign-On**, en klik vervolgens op **volgende**.
   
   ![Eenmalige aanmelding configureren](./media/active-directory-saas-central-desktop-tutorial/IC777628.png "eenmalige aanmelding configureren")
3. Op de **App-URL configureren** pagina, voer de volgende stappen uit en klik vervolgens op **volgende**: 
   
   1. In de **centrale bureaublad aanmelding In URL** textbox, typ de URL van uw tenant centrale bureaublad (bijvoorbeeld: *http://contoso.centraldesktop.com*).
   2. Typ in het tekstvak voor de centrale bureaublad antwoord-URL, uw centrale bureaublad AssertionConsumerService-URL (bijvoorbeeld: https://contoso.centraldesktop.com/saml2-assertion.php).
   
   >[!NOTE]
   >U kunt de waarde niet ophalen uit de centrale bureaublad metagegevens (bijvoorbeeld: *http://contoso.centraldesktop.com*).
   >  
   
   ![App-URL configureren](./media/active-directory-saas-central-desktop-tutorial/IC769561.png "app-URL configureren")
4. Op de **eenmalige aanmelding configureren op de centrale bureaublad** pagina om uw certificaat te downloaden, klikt u op **certificaat downloaden**, en sla het certificaatbestand op uw computer.
   
  ![Eenmalige aanmelding configureren](./media/active-directory-saas-central-desktop-tutorial/IC769562.png "eenmalige aanmelding configureren")
5. Meld u aan bij uw **centrale bureaublad** tenant.
6. Ga naar **instellingen**, klikt u op **Geavanceerd**, en klik vervolgens op **eenmalige aanmelding**.
   
  ![Setup - geavanceerde](./media/active-directory-saas-central-desktop-tutorial/IC769563.png "Setup - geavanceerde")
7. Op de **eenmalige aanmelding op instellingen** pagina, voert u de volgende stappen uit:
   
  ![Eenmalige aanmelding instellingen](./media/active-directory-saas-central-desktop-tutorial/IC769564.png "eenmalige van instellingen")
   
  1. Selecteer **inschakelen SAML v2 voor eenmalige aanmelding**.
  2. In de klassieke Azure-portal op de **op centrale bureaublad eenmalige aanmelding configureren** pagina, Kopieer de **URL-verlener** waarde en plak deze in de **URL SSO** textbox.
  3. In de klassieke Azure-portal op de **eenmalige aanmelding configureren op de centrale bureaublad** pagina, Kopieer de **externe aanmeldings-URL** waarde en plak deze in de **aanmeldings-URL voor eenmalige aanmelding** textbox .
  4. In de klassieke Azure-portal op de **eenmalige aanmelding configureren op de centrale bureaublad** pagina, Kopieer de **Service-URL met eenmalige Sign-Out** waarde en plak deze in de **SSO afmelding URL**textbox.
8. In de **bericht handtekening verificatiemethode** sectie, voert u de volgende stappen uit:
   
   ![Handtekening verificatiemethode bericht](./media/active-directory-saas-central-desktop-tutorial/IC769565.png "bericht handtekening verificatiemethode")
   
  1. Selecteer **certificaat**.
  2. Van de **SSO certificaat** selecteert **RSH SHA256**.
  3. Maak een tekstbestand uit het gedownloade certificaat, Kopieer de inhoud van het tekstbestand en plakt u deze in de **SSO certificaat** veld.  
     >[!TIP]
     >Zie voor meer informatie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)
      >  
   4. Selecteer **een koppeling weergegeven naar de aanmeldingspagina van uw SAMLv2**.
9. Klik op **Update**.
10. Bij de klassieke Azure portal, selecteert u de configuratie voor één aanmelding bevestiging en klik op **Complete** sluiten de **configureren eenmalige aanmelding** dialoogvenster.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-central-desktop-tutorial/IC769566.png "eenmalige aanmelding configureren")
    
## <a name="configure-user-provisioning"></a>Gebruikers inrichten configureren

AAD-gebruikers moeten kunnen aanmelden, als ze worden ingericht voor de toepassing centrale bureaublad. Deze sectie beschrijft het maken van gebruikersaccounts van AAD in centrale bureaublad.

**Voor het inrichten van gebruikersaccounts naar centrale bureaublad:**
1. Aanmelden bij uw tenant centrale bureaublad.
2. Ga naar **mensen \> interne leden**.
3. Klik op **interne leden toevoegen**.
   
  ![Mensen](./media/active-directory-saas-central-desktop-tutorial/IC781051.png "personen")
4. In de **e-mailadres van nieuwe leden** textbox, typt u een AAD-account u wilt inrichten, en klik vervolgens op **volgende**.
   
  ![E-mailadressen van nieuwe leden](./media/active-directory-saas-central-desktop-tutorial/IC781052.png "e-mailadressen van nieuwe leden")
5. Klik op **interne toevoegen leden**.
   
  ![Interne lid toevoegen](./media/active-directory-saas-central-desktop-tutorial/IC781053.png "interne lid toevoegen")
   
   >[!NOTE]
   >De gebruikers die u hebt toegevoegd, ontvangt een e-mailbericht met een bevestigingskoppeling moet worden geklikt om de account te activeren.
   > 

>[!NOTE]
>U kunt andere centrale bureaublad gebruiker account hulpmiddelen voor het maken of API's die is geleverd door centrale bureaublad aan inrichten AAD-gebruikersaccounts
>  

## <a name="assign-users"></a>Gebruikers toewijzen
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat met behulp van uw toepassing toegang tot deze door toe te wijzen.

**Gebruikers om aan te wijzen centrale bureaublad, moet u de volgende stappen uitvoeren:**

1. In de klassieke Azure portal een testaccount te maken.
2. Op de **centrale bureaublad** toepassing Integratiepagina, klikt u op **gebruikers toewijzen**.
   
   ![Gebruikers toewijzen](./media/active-directory-saas-central-desktop-tutorial/IC769567.png "gebruikers toewijzen")
3. Selecteer uw testgebruiker, klik op **toewijzen**, en klik vervolgens op **Ja** om te bevestigen dat de toewijzing.
   
   ![Ja](./media/active-directory-saas-central-desktop-tutorial/IC767830.png "Ja")

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het toegangsvenster. Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

