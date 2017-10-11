---
title: 'Zelfstudie: Azure Active Directory-integratie met TOPdesk - beveiligde | Microsoft Docs'
description: Informatie over het gebruik van TOPdesk - beveiligen met Azure Active Directory om het inschakelen van eenmalige aanmelding, geautomatiseerde inrichting en meer!.
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 8e149d2d-7849-48ec-9993-31f4ade5fdb4
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: 28f0542dbe87bb34c83a7852db7c3a9fef055ce9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---secure"></a>Zelfstudie: Azure Active Directory-integratie met TOPdesk - beveiligen
Het doel van deze zelfstudie is het weergeven van de integratie van Azure en TOPdesk - veilig.  
Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

* Een geldige Azure-abonnement
* Een TOPdesk - beveiligde eenmalige aanmelding ingeschakeld abonnement

Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan TOPdesk - veilig kan worden één Meld u aan bij de toepassing op uw TOPdesk - beveiligd bedrijf site (serviceprovider geïnitieerd aanmelding) of met behulp van de [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

Het scenario in deze zelfstudie bestaat uit de volgende elementen:

1. De integratie van toepassingen voor TOPdesk - beveiligde inschakelen
2. Eenmalige aanmelding configureren
3. Configuratie van gebruikers inrichten
4. Gebruikers toewijzen

![Scenario](./media/active-directory-saas-topdesk-secure-tutorial/IC790596.png "Scenario")

## <a name="enabling-the-application-integration-for-topdesk---secure"></a>De integratie van toepassingen voor TOPdesk - beveiligde inschakelen
Het doel van deze sectie is het inschakelen van de integratie van toepassingen voor TOPdesk - beveiligde overzicht.

### <a name="to-enable-the-application-integration-for-topdesk---secure-perform-the-following-steps"></a>Om in te schakelen van de integratie van toepassingen voor TOPdesk - beveiligen, voert u de volgende stappen uit:
1. Klik in de klassieke Azure-portal op het navigatiedeelvenster links op **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-topdesk-secure-tutorial/IC700993.png "Active Directory")

2. Van de **Directory** , selecteert u de map waarvoor u wilt inschakelen van Active directory-integratie.

3. De weergave toepassingen in de directoryweergave, klikt u op **toepassingen** in het menu bovenaan.
   
    ![Toepassingen](./media/active-directory-saas-topdesk-secure-tutorial/IC700994.png "toepassingen")

4. Klik op **toevoegen** aan de onderkant van de pagina.
   
    ![Toepassing toevoegen](./media/active-directory-saas-topdesk-secure-tutorial/IC749321.png "toepassing toevoegen")

5. Op de **wat wilt u doen** dialoogvenster, klikt u op **toevoegen van een toepassing uit de galerie**.
   
    ![Een toepassing toevoegen van gallerry](./media/active-directory-saas-topdesk-secure-tutorial/IC749322.png "een toepassing van gallerry toevoegen")

6. In de **zoekvak**, type **TOPdesk - beveiligde**.
   
    ![Toepassingsgalerie](./media/active-directory-saas-topdesk-secure-tutorial/IC790597.png "-Toepassingsgalerie")

7. Selecteer in het deelvenster met resultaten **TOPdesk - beveiligde**, en klik vervolgens op **Complete** de toepassing toevoegen.
   
    ![TOPdesk - beveiligde](./media/active-directory-saas-topdesk-secure-tutorial/IC791933.png "TOPdesk - beveiligen")

## <a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
Het doel van deze sectie is om een overzicht van gebruikers te verifiëren bij TOPdesk - in staat te beveiligen met een account in Azure AD dat gebruikmaakt van Federatie op basis van het SAML-protocol.  
Configureren eenmalige aanmelding voor TOPdesk - beveiligen, moet u een pictogram logobestand te uploaden. Als u het pictogrambestand, contact op met het ondersteuningsteam TOPdesk.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voor het configureren van eenmalige aanmelding, moet u de volgende stappen uitvoeren:
1. Meld u aan bij uw **TOPdesk - beveiligde** bedrijf site als beheerder.
2. In de **TOPdesk** menu, klikt u op **instellingen**.
   
    ![Instellingen](./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "instellingen")

3. Klik op **aanmeldingsinstellingen**.
   
    ![Instellingen voor aanmelding](./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "aanmeldingsinstellingen")

4. Vouw de **aanmeldingsinstellingen** menu en klik vervolgens op **algemene**.
   
    ![Algemene](./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "algemeen")

5. In de **Secure** sectie van de **SAML aanmelding** configuratie sectie, voert u de volgende stappen uit:
   
    ![Instellingen voor technische](./media/active-directory-saas-topdesk-secure-tutorial/IC790855.png "technische instellingen")
   
    a. Klik op **downloaden** downloaden van het metagegevensbestand van de openbare en lokaal opslaan op uw computer.
   
    b. Open het metagegevensbestand en Ga naar de **AssertionConsumerService** knooppunt.
    
    ![Verklaring Consumer Service](./media/active-directory-saas-topdesk-secure-tutorial/IC790856.png "Assertion Consumer-Service")
   
    c. Kopieer de **AssertionConsumerService** waarde.  
      
    > [!NOTE]
    > U moet de waarde in de **App-URL configureren** verderop in deze zelfstudie.
    > 
    > 

6. In een ander browservenster, meld u aan bij uw **klassieke Azure-portal** als beheerder.

7. Op de **TOPdesk - beveiligde** toepassing Integratiepagina, klikt u op **eenmalige aanmelding configureren** openen de ** configureren eenmalige aanmelding ** dialoogvenster.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-topdesk-secure-tutorial/IC790602.png "eenmalige aanmelding configureren")

8. Op de **hoe wilt u dat gebruikers zich aanmelden op TOPdesk - beveiligde** pagina **Microsoft Azure AD Single Sign-On**, en klik vervolgens op **volgende**.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-topdesk-secure-tutorial/IC790603.png "eenmalige aanmelding configureren")

9. Op de **App-URL configureren** pagina, voert u de volgende stappen uit:
   
    ![App-URL configureren](./media/active-directory-saas-topdesk-secure-tutorial/IC790604.png "App-URL configureren")
   
    a. In de **TOPdesk - beveiligde aanmelding op URL** textbox, typ de URL gebruikt door uw gebruikers zich aanmeldt bij uw TOPdesk - beveiligde toepassing (bijvoorbeeld: '*https://qssolutions.topdesk.net*').
   
    b. In de **TOPdesk – openbare antwoord-URL** textbox, plak de **TOPdesk - beveiligde AssertionConsumerService URL** (bijvoorbeeld: '*https://qssolutions.topdesk.net/tas/public/login/saml*")
   
    c. Klik op **Volgende**.

10. Op de **eenmalige aanmelding configureren op TOPdesk - beveiligde** pagina voor het downloaden van het metagegevensbestand, klikt u op **metagegevens downloaden**, en sla het bestand lokaal op uw computer.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-topdesk-secure-tutorial/IC790605.png "eenmalige aanmelding configureren")

11. Voer de volgende stappen uit voor het maken van een certificaatbestand:
    
    ![Certificaat](./media/active-directory-saas-topdesk-secure-tutorial/IC790606.png "certificaat")
    
    a. Open het metagegevensbestand gedownloade.
    b. Vouw de **RoleDescriptor** knooppunt met een **xsi: type** van **ingevoerd: ApplicationServiceType**.
    c. Kopieer de waarde van de **X509Certificate** knooppunt.
    d. Opslaan van de gekopieerde **X509Certificate** waarde lokaal op uw computer in een bestand.

12. Beveiligd op uw TOPdesk - site, bedrijf, in de **TOPdesk** menu, klikt u op **instellingen**.
    
    ![Instellingen](./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "instellingen")

13. Klik op **aanmeldingsinstellingen**.
    
    ![Instellingen voor aanmelding](./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "aanmeldingsinstellingen")

14. Vouw de **aanmeldingsinstellingen** menu en klik vervolgens op **algemene**.
    
    ![Algemene](./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "algemeen")

15. In de **openbare** sectie, klikt u op **toevoegen**.
    
    ![Voeg](./media/active-directory-saas-topdesk-secure-tutorial/IC790607.png "toevoegen")

16. Op de **SAML-configuratie-assistent** dialoogvenster pagina, voert u de volgende stappen uit:
    
    ![SAML-configuratie-assistent](./media/active-directory-saas-topdesk-secure-tutorial/IC790608.png "assistent voor SAML-configuratie")
    
    a. Voor het uploaden van uw gedownloade metagegevensbestand onder **Federatiemetagegevens**, klikt u op **Bladeren**.

    b. Voor het uploaden van het certificaatbestand onder **certificaat (RSA)**, klikt u op **Bladeren**.

    c. Het logobestand dat u het ondersteuningsteam TOPdesk onder gekregen te uploaden **Logo pictogram**, klikt u op **Bladeren**.

    d. In de **naam gebruikerskenmerk** textbox type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    e. In de **weergavenaam** textbox, typ een naam voor uw configuratie.

    f. Klik op **Opslaan**.

17. Bij de klassieke Azure portal, selecteert u de configuratie voor één aanmelding bevestiging en klik op **Complete** sluiten de **configureren eenmalige aanmelding** dialoogvenster.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-topdesk-secure-tutorial/IC790609.png "eenmalige aanmelding configureren")

## <a name="configuring-user-provisioning"></a>Configuratie van gebruikers inrichten
Om in te schakelen van Azure AD-gebruikers aan te melden bij TOPdesk - wilt beveiligen, ze moeten worden ingericht in TOPdesk - veilig.  
In het geval van TOPdesk - veilige, inrichting is een handmatige taak.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren voor gebruikers inrichten, moet u de volgende stappen uitvoeren:
1. Meld u aan bij uw **TOPdesk - beveiligde** bedrijf site als administrator.
2. Klik in het menu bovenaan op **TOPdesk \> nieuw \> ondersteuningsbestanden \> Operator**.
   
    ![Operator](./media/active-directory-saas-topdesk-secure-tutorial/IC790610.png "Operator")

3. Op de **Operator New** dialoogvenster de volgende stappen uitvoeren:
   
    ![Nieuwe Operator](./media/active-directory-saas-topdesk-secure-tutorial/IC790611.png "nieuwe Operator")
   
    a. Klik op het tabblad Algemeen.
   
    b. In de **achternaam** textbox van de **algemene** sectie, typt u de achternaam van een geldig Azure Active Directory-account dat u inrichten wilt.
   
    c. Selecteer een **Site** voor het account in de **locatie** sectie.
   
    d. In de **aanmeldingsnaam** textbox van de **TOPdesk aanmelding** sectie, typt u een aanmeldingsnaam voor de gebruiker.
   
    e. Klik op **Opslaan**.

> [!NOTE]
> U kunt andere TOPdesk - hulpmiddelen voor het maken van account beveiligde gebruiker of geleverd door TOPdesk - veilig voor het inrichten van accounts voor gebruikers met AAD-API's gebruiken.
> 
> 

## <a name="assigning-users"></a>Gebruikers toewijzen
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat met behulp van uw toepassing toegang tot deze door toe te wijzen.

### <a name="to-assign-users-to-topdesk---secure-perform-the-following-steps"></a>Als u gebruikers toewijzen aan TOPdesk - wilt beveiligen, de volgende stappen uitvoeren:
1. In de klassieke Azure portal een testaccount te maken.
2. Op de ** TOPdesk - beveiligde ** toepassing Integratiepagina, klikt u op **gebruikers toewijzen**.
   
    ![Gebruikers toewijzen](./media/active-directory-saas-topdesk-secure-tutorial/IC790612.png "gebruikers toewijzen")

3. Selecteer uw testgebruiker, klik op **toewijzen**, en klik vervolgens op **Ja** om te bevestigen dat de toewijzing.
   
    ![Ja](./media/active-directory-saas-topdesk-secure-tutorial/IC767830.png "Ja")

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het toegangsvenster. Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

