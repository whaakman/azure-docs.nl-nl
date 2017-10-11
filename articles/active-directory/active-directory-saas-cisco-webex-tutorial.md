---
title: 'Zelfstudie: Azure Active Directory-integratie met Cisco Webex | Microsoft Docs'
description: Meer informatie over het gebruik van Cisco Webex met Azure Active Directory voor het inschakelen van eenmalige aanmelding, geautomatiseerde inrichting en meer!
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 26704ca7-13ed-4261-bf24-fd6252e2072b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: b44b1a5b3e988a51db3325ec8a181651fa84e768
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a>Zelfstudie: Azure Active Directory-integratie met Cisco Webex
Het doel van deze zelfstudie is om de integratie van Azure en Cisco Webex weer te geven.  
Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

* Een geldige Azure-abonnement
* Een tenant Cisco Webex

Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan Cisco Webex worden kunnen eenmalige aanmelding in de toepassing op uw Cisco Webex bedrijf site (serviceprovider geïnitieerd aanmelding) of met behulp van de [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

Het scenario in deze zelfstudie bestaat uit de volgende elementen:

* De integratie van toepassingen voor Cisco Webex inschakelen
* Configureren van eenmalige aanmelding (SSO)
* Configuratie van gebruikers inrichten
* Gebruikers toewijzen

![Scenario](./media/active-directory-saas-cisco-webex-tutorial/IC777614.png "Scenario")

## <a name="enable-the-application-integration-for-cisco-webex"></a>De integratie van toepassingen voor Cisco Webex inschakelen
Het doel van deze sectie is het inschakelen van de integratie van toepassingen voor Cisco Webex overzicht.

**Als u wilt de integratie van toepassingen voor Cisco Webex inschakelen, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure-portal op het navigatiedeelvenster links op **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-cisco-webex-tutorial/IC700993.png "Active Directory")
2. Van de **Directory** , selecteert u de map waarvoor u wilt inschakelen van Active directory-integratie.
3. De weergave toepassingen in de directoryweergave, klikt u op **toepassingen** in het menu bovenaan.
   
   ![Toepassingen](./media/active-directory-saas-cisco-webex-tutorial/IC700994.png "toepassingen")
4. Klik op **toevoegen** aan de onderkant van de pagina.
   
   ![Toepassing toevoegen](./media/active-directory-saas-cisco-webex-tutorial/IC749321.png "toepassing toevoegen")
5. Op de **wat wilt u doen** dialoogvenster, klikt u op **toevoegen van een toepassing uit de galerie**.
   
   ![Een toepassing toevoegen van gallerry](./media/active-directory-saas-cisco-webex-tutorial/IC749322.png "een toepassing van gallerry toevoegen")
6. In de **zoekvak**, type **Cisco Webex**.
   
   ![Toepassingsgalerie](./media/active-directory-saas-cisco-webex-tutorial/IC777615.png "-Toepassingsgalerie")
7. Selecteer in het deelvenster met resultaten **Cisco Webex**, en klik vervolgens op **Complete** de toepassing toevoegen.
   
   ![Cisco Webex](./media/active-directory-saas-cisco-webex-tutorial/IC777616.png "Cisco Webex")
   
## <a name="configure-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van deze sectie is om een overzicht van gebruikers om te verifiëren Cisco Webex aan hun account in Azure AD dat gebruikmaakt van Federatie op basis van het SAML-protocol in staat te.  

Als onderdeel van deze procedure moet zijn u vereist voor het maken van een Base64-gecodeerde certificaat. Als u niet bekend met deze procedure bent, raadpleegt u [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

**Voor het configureren van eenmalige aanmelding, moet u de volgende stappen uitvoeren:**

1. In de klassieke Azure-portal op de **Cisco Webex** toepassing Integratiepagina, klikt u op **eenmalige aanmelding configureren** openen de **configureren eenmalige aanmelding** dialoogvenster.
   
   ![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-webex-tutorial/IC777617.png "eenmalige aanmelding configureren")
2. Op de **hoe wilt u dat gebruikers zich aanmelden op voor Cisco Webex** pagina **Microsoft Azure AD Single Sign-On**, en klik vervolgens op **volgende**.
   
   ![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-webex-tutorial/IC777618.png "eenmalige aanmelding configureren")
3. Op de **App-URL configureren** pagina, voer de volgende stappen uit en klik vervolgens op **volgende**.
   
   ![App-URL configureren](./media/active-directory-saas-cisco-webex-tutorial/IC777619.png "app-URL configureren")   
   1. In de **Sing op URL** textbox, typt u uw Cisco Webex tenant-URL (bijvoorbeeld: *http://contoso.webex.com*).
   2. In de **Cisco Webex antwoord-URL** textbox type uw **Cisco Webex AssertionConsumerService URL** (bijvoorbeeld: *https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company*).
4. Op de **eenmalige aanmelding configureren op Cisco Webex** pagina om uw certificaat te downloaden, klikt u op **certificaat downloaden**, en sla het certificaatbestand op uw computer.
   
   ![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-webex-tutorial/IC777620.png "eenmalige aanmelding configureren")
5. In een ander browservenster, meld u bij uw bedrijf Cisco Webex site als beheerder.
6. Klik in het menu bovenaan op **Sitebeheer**.
   
   ![Sitebeheer](./media/active-directory-saas-cisco-webex-tutorial/IC777621.png "Sitebeheer")
7. In de **-Site beheren** sectie, klikt u op **SSO configuratie**.
   
   ![Configuratie van de SSO-](./media/active-directory-saas-cisco-webex-tutorial/IC777622.png "SSO-configuratie")
8. Voer de volgende stappen uit in de sectie federatief Web SSO-configuratie:
   
   ![Federatieve SSO configuratie](./media/active-directory-saas-cisco-webex-tutorial/IC777623.png "federatieve SSO-configuratie")  
   1. Van de **Protocol Federation** selecteert **SAML 2.0**.
   2. Maak een **Base 64-codering** bestand van uw gedownloade certificaat.  
    >[!TIP]
    >Zie voor meer informatie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)
    >  
   3. Open uw base-64 gecodeerde certificaat in Kladblok en kopieer de inhoud ervan.
   4. Klik op **SAML-metagegevens importeren**, en plak de base-64 gecodeerde certificaat.
   5. In de klassieke Azure-portal op de **eenmalige aanmelding configureren op Cisco Webex** dialoogvenster pagina-, Kopieer de **URL-verlener** waarde en plak deze in de **verlener voor SAML (IdP-ID)** textbox.
   6. In de klassieke Azure-portal op de **op Cisco Webex eenmalige aanmelding configureren** dialoogvenster pagina-, Kopieer de **externe aanmeldings-URL** waarde en plak deze in de **aanmeldings-URL van de klant SSO Service** textbox.
   7. Van de **NameID indeling** selecteert **e-mailadres**.
   8. In de **AuthnContextClassRef** textbox type **urn: oasis: namen: tc: SAML:2.0:ac:classes:Password**.
   9. In de klassieke Azure-portal op de **op Cisco Webex eenmalige aanmelding configureren** dialoogvenster pagina-, Kopieer de **externe URL voor afmelden** waarde en plak deze in de **klant SSO-Service-URL voor afmelden** textbox.
   10. Klik op **Update**.
9. In de klassieke Azure-portal op de **eenmalige aanmelding configureren op Cisco Webex** dialoogvenster pagina, selecteert u de configuratie voor één aanmelding bevestiging en klik vervolgens op **Complete**.
   
   ![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-webex-tutorial/IC777624.png "eenmalige aanmelding configureren")
   
## <a name="configure-user-provisioning"></a>Gebruikers inrichten configureren

Om in te schakelen gebruikers van Azure AD aan te melden bij Cisco Webex, moeten ze worden ingericht in Cisco Webex.  

* In het geval van een Cisco-Webex is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **Cisco Webex** tenant.
2. Ga naar **gebruikers beheren \> gebruiker toevoegen**.
   
   ![Gebruikers toevoegen](./media/active-directory-saas-cisco-webex-tutorial/IC777625.png "gebruikers toevoegen")
3. Voer de volgende stappen uit in de sectie gebruiker toevoegen:
   
   ![Gebruiker toevoegen](./media/active-directory-saas-cisco-webex-tutorial/IC777626.png "gebruiker toevoegen")   
   1. Als **accounttype**, selecteer **Host**.
   2. Typ de gegevens van een bestaande Azure AD-gebruiker in de volgende tekstvakken: **voornaam, achternaam**, **gebruikersnaam**, **e**, **wachtwoord**, **wachtwoord bevestigen**.
   3. Klik op **Add**.

>[!NOTE]
>U kunt andere Cisco Webex gebruiker account hulpmiddelen voor het maken of API's die is geleverd door Cisco Webex aan inrichten AAD-gebruikersaccounts. 
> 

## <a name="assign-users"></a>Gebruikers toewijzen
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat met behulp van uw toepassing toegang tot deze door toe te wijzen.

**Gebruikers om aan te wijzen Cisco Webex, moet u de volgende stappen uitvoeren:**

1. In de klassieke Azure portal een testaccount te maken.
2. Op de **Cisco Webex** toepassing Integratiepagina, klikt u op **gebruikers toewijzen**.
   
   ![Gebruikers toewijzen](./media/active-directory-saas-cisco-webex-tutorial/IC777627.png "gebruikers toewijzen")
3. Selecteer uw testgebruiker, klik op **toewijzen**, en klik vervolgens op **Ja** om te bevestigen dat de toewijzing.
   
   ![Ja](./media/active-directory-saas-cisco-webex-tutorial/IC767830.png "Ja")

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het toegangsvenster. Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

