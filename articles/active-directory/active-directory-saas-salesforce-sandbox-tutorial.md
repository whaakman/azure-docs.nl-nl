---
title: 'Zelfstudie: Azure Active Directory-integratie met Salesforce Sandbox | Microsoft Docs'
description: Informatie over het gebruik van Salesforce Sandbox met Azure Active Directory om te schakelen eenmalige aanmelding, geautomatiseerde inrichting en meer!.
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/21/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 32835e79188806bb2ff319eea23b1b52ab585ab1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Zelfstudie: Azure Active Directory-integratie met Salesforce Sandbox

Het doel van deze zelfstudie is om de integratie van Azure en Salesforce Sandbox weer te geven.  

>[!TIP]
>Zie voor feedback over de [Azure ondersteuningspagina](http://go.microsoft.com/fwlink/?LinkId=521878). 
> 

Sandboxes bieden u de mogelijkheid meerdere kopieën van uw organisatie in een afzonderlijke omgeving voor verschillende doeleinden, zoals ontwikkeling, testen en training, zonder verlies van gegevens en toepassingen in uw productieomgeving Salesforce maken de organisatie.  

Zie voor meer informatie [sandbox-overzicht](https://help.salesforce.com/HTViewHelpDoc?id=create_test_instance.htm&language=en_US)

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

* Een geldige Azure-abonnement
* Een sandbox in Salesforce.com

Als u nog een geldige sandbox in Salesforce.com hebt, moet u contact opnemen met Salesforce.

Het scenario in deze zelfstudie bestaat uit de volgende elementen:

1. De integratie van toepassingen voor Salesforce Sandbox inschakelen
2. Configureren van eenmalige aanmelding (SSO)
3. Uw domein inschakelen
4. Configuratie van gebruikers inrichten
5. Gebruikers toewijzen

![Scenario](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769571.png "Scenario")

## <a name="enable-the-application-integration-for-salesforce-sandbox"></a>De integratie van toepassingen voor Salesforce Sandbox inschakelen
Het doel van deze sectie is het inschakelen van de integratie van toepassingen voor Salesforce sandbox overzicht.

**Schakel de integratie van toepassingen voor Salesforce sandbox door de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure-portal op het navigatiedeelvenster links op **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700993.png "Active Directory")
2. Van de **Directory** , selecteert u de map waarvoor u wilt inschakelen van Active directory-integratie.
3. De weergave toepassingen in de directoryweergave, klikt u op **toepassingen** in het menu bovenaan.
   
   ![Toepassingen](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700994.png "toepassingen")
4. Openen van de **Toepassingsgalerie**, klikt u op **een App toevoegen**, en klik vervolgens op **toevoegen van een toepassing voor mijn organisatie te gebruiken**.
   
   ![Wat wilt u doen? ] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700995.png "Wat wilt u doen?")
5. In de **zoekvak**, type **Salesforce Sandbox**.
   
   ![Toepassingsgalerie](./media/active-directory-saas-salesforce-sandbox-tutorial/IC710978.png "-Toepassingsgalerie")
6. Selecteer in het deelvenster met resultaten **Salesforce Sandbox**, en klik vervolgens op **Complete** de toepassing toevoegen.
   
   ![SalesForce Sandbox](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746474.png "Salesforce Sandbox")
   
## <a name="configur-single-sign-on-sso"></a>Configur eenmalige aanmelding (SSO)

Het doel van deze sectie is om een overzicht van gebruikers te verifiëren bij Salesforce met hun account in Azure AD dat gebruikmaakt van Federatie op basis van het SAML-protocol in staat te.

**Voor het configureren van eenmalige aanmelding, moet u de volgende stappen uitvoeren:**

1. In de klassieke Azure portal op de **Salesforce Sandbox** toepassing Integratiepagina, klikt u op **eenmalige aanmelding configureren** openen de **configureren eenmalige aanmelding** dialoogvenster.
   
   ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforce-sandbox-tutorial/IC749323.png "eenmalige aanmelding configureren")
2. Op de **hoe wilt u dat gebruikers zich aanmelden op voor de Salesforce Sandbox** pagina **Microsoft Azure AD Single Sign-On**, en klik vervolgens op **volgende**.
   
   ![SalesForce Sandbox](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746479.png "Salesforce Sandbox")
3. Op de **App-URL configureren** pagina in de **aanmelding op URL** textbox, typ uw URL met het volgende patroon volgen `http://company.my.salesforce.com`, en klik vervolgens op **volgende**.
   
   ![App-URL configureren](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781022.png "App-URL configureren")
4. Als u al eenmalige aanmelding voor een ander exemplaar van Salesforce Sandbox hebt geconfigureerd in uw directory, dan moet u ook configureren de **id** hebben dezelfde waarde als de **aanmelden URL**. 
 * De **id** veld kan worden gevonden door het controleren van de **weergeven geavanceerde instellingen** selectievakje op het **App-URL configureren** pagina van het dialoogvenster.
5. Op de **eenmalige aanmelding configureren op de Salesforce Sandbox** pagina, klikt u op **certificaat downloaden**, en sla het certificaatbestand op uw computer.
   
   ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781023.png "eenmalige aanmelding configureren")
6. In een ander browservenster, meld u bij uw Salesforce-sandbox als beheerder.
7. Klik in het menu bovenaan op **Setup**.
   
   ![Setup](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781024.png "Setup")
8. Klik in het navigatievenster aan de linkerkant op **beveiligingsmechanismen**, en klik vervolgens op **instellingen voor eenmalige aanmelding**.
   
   ![Eenmalige aanmelding instellingen](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781025.png "eenmalige aanmelding-instellingen")
9. Voer de volgende stappen uit in de sectie instellingen voor eenmalige aanmelding:
   
   ![Eenmalige aanmelding instellingen](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781026.png "eenmalige aanmelding-instellingen")  
 1.  Selecteer **SAML ingeschakeld**. 
 2.  Klik op **Nieuw**.
10. Voer de volgende stappen uit in de sectie SAML Single Sign-On-instellingen:
    
    ![SAML eenmalige aanmelding-instellingen](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781027.png "SAML eenmalige aanmelding-instellingen")  
 1. Typ de naam van de configuratie in het tekstvak naam (bijvoorbeeld: *SPSSOWAAD\_Test*). 
 2. In de klassieke Azure-portal op de **op Salesforce Sandbox eenmalige aanmelding configureren** dialoog pagina, Kopieer de **URL-verlener** waarde en plak deze in de **verlener** tekstvak.
 3. In de **entiteit-Id** textbox type **https://test.salesforce.com** als dit het eerste exemplaar van Salesforce Sandbox die u aan uw directory toevoegen wilt. Als u al hebt toegevoegd een exemplaar van Salesforce-Sandbox vervolgens voor de **entiteit-ID** typt u in de **aanmelding op URL**, die moet worden in deze indeling:`http://company.my.salesforce.com`   
 4. Klik op **Bladeren** om de gedownloade certificaat te uploaden.  
 5. Als **SAML identiteitstype**, selecteer **Assertion bevat de Federation-ID van het gebruikersobject**. 
 6. Als **SAML identiteit locatie**, selecteer **identiteit is in het element NameIdentifier van het onderwerp overzicht**.
 7. In de klassieke Azure-portal op de **op Salesforce Sandbox eenmalige aanmelding configureren** dialoog pagina, Kopieer de **externe aanmeldings-URL** waarde en plak deze in de **identiteit Provider aanmeldings-URL**  textbox. 
 8. SFDC biedt geen ondersteuning voor SAML-afmelden.  Als een tijdelijke oplossing 'https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0' te plakken in de **identiteit Provider afmelding URL** textbox.
 9. Als **Provider geïnitieerd aanvragen servicebinding**, selecteer **HTTP POST**. 
 10. Klik op **Opslaan**.
11. Bij de klassieke Azure portal, selecteert u de configuratie voor één aanmelding bevestiging en klik op **Complete** sluiten de **configureren eenmalige aanmelding** dialoogvenster.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781028.png "eenmalige aanmelding configureren")

## <a name="enable-your-domain"></a>Uw domein inschakelen
Deze sectie wordt ervan uitgegaan dat u al hebt gemaakt een domein.  Zie voor meer informatie [definiëren van uw domeinnaam](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).

**Als u wilt maken voor uw domein, moet u de volgende stappen uitvoeren:**

1. Klik in het navigatiedeelvenster links op **domeinbeheer**, en klik vervolgens op **mijn domein.**
   
   ![Mijn domein](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781029.png "mijn domein")
   
   >[!NOTE]
   >Zorg dat uw domein correct is geconfigureerd. 
   > 
2. In de **pagina aanmeldingsinstellingen** sectie, klikt u op **bewerken**, naarmate **verificatieservice**, selecteert u de naam van de SAML Single Sign-On-instelling van de vorige sectie en klik tot slot **opslaan**.
   
   ![Mijn domein](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781030.png "mijn domein")

Als u een domein geconfigureerd hebt, moeten uw gebruikers de domein-URL voor aanmelding bij de sandbox Salesforce gebruiken.  

Als u de waarde van de URL, klikt u op de SSO-profiel dat u hebt gemaakt in de vorige sectie.

## <a name="configure-user-provisioning"></a>Gebruikers inrichten configureren
Het doel van deze sectie is het inschakelen van de gebruiker het inrichten van Active Directory-gebruikersaccounts met Salesforce Sandbox overzicht.

**Als u wilt configureren voor gebruikers inrichten, moet u de volgende stappen uitvoeren:**

1. Selecteer uw naam om uw gebruikersmenu te openen in de bovenste navigatiebalk de Salesforce-Portal:
   
   ![Mijn instellingen](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698773.png "Mijn instellingen")
2. Selecteer in het gebruikersmenu **Mijn instellingen** openen uw **Mijn instellingen** pagina.
3. Klik in het linkerdeelvenster op **persoonlijke** Vouw het persoonlijke gedeelte uit en klik vervolgens op **opnieuw mijn beveiligingstoken**:
   
   ![Mijn instellingen](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698774.png "Mijn instellingen")
4. Op de **opnieuw mijn beveiligingstoken** pagina, klikt u op **Security Token opnieuw** om aan te vragen van een e-mailbericht met uw beveiligingstoken Salesforce.com.
   
   ![Nieuw Token](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698776.png "nieuw Token")
5. Controleer uw postvak voor een e-mailbericht van Salesforce.com met '**salesforce.com.com beveiligingsgegevens**' als onderwerp.
6. Bekijk dit e-mailbericht en kopieer de waarde voor het token.
7. In de klassieke Azure portal op de **salesforce Sandbox** pagina van de integratie van toepassing, klikt u op **configureren gebruikersaanvragen** openen de **gebruikersaanvragen configureren** het dialoogvenster.
   
   ![Configureer gebruikersaanvragen](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769573.png "gebruikers inrichten configureren")
8. Op de **Voer uw referenties in Salesforce Sandbox zodat automatisch gebruikers inrichten** pagina, bieden de volgende configuratie-instellingen:
   
   ![SalesForce Sandbox](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746476.png "Salesforce Sandbox")   
 1. In de **Salesforce Sandbox-Beheerdersgebruikersnaam** textbox type een sandbox met Salesforce-accountnaam met de **systeembeheerder** profiel in Salesforce.com toegewezen.
 2. In de **Salesforce Sandbox beheerderswachtwoord** textbox, typt u het wachtwoord voor dit account.
 3. In de **gebruiker beveiligingstoken** textbox, plak de security token waarde.
 4. Klik op **valideren** om te controleren of uw configuratie.
 5. Klik op de **volgende** te openen de **bevestiging** pagina.
9. Op de **bevestiging** pagina, klikt u op **Complete** aan uw configuratie op te slaan.
   
## <a name="assigning-users"></a>Gebruikers toewijzen

Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat met behulp van uw toepassing toegang tot deze door toe te wijzen.

**Als u gebruikers toewijzen aan de Salesforce Sandbox, wilt u de volgende stappen uitvoeren:**

1. In de klassieke Azure portal een testaccount te maken.
2. Op de ** Salesforce Sandbox ** toepassing Integratiepagina, klikt u op **gebruikers toewijzen**.
   
   ![Gebruikers toewijzen](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769574.png "gebruikers toewijzen")
3. Selecteer uw testgebruiker, klik op **toewijzen**, en klik vervolgens op **Ja** om te bevestigen dat de toewijzing.
   
   ![Ja](./media/active-directory-saas-salesforce-sandbox-tutorial/IC767830.png "Ja")

U moet nu wacht tien minuten en controleer of dat het account bij Salesforce Sandbox is gesynchroniseerd.

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het toegangsvenster. Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](https://msdn.microsoft.com/library/dn308586).

