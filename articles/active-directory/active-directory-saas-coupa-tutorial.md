---
title: 'Zelfstudie: Azure Active Directory-integratie met Coupa | Microsoft Docs'
description: Meer informatie over het gebruik van Coupa met Azure Active Directory voor het inschakelen van eenmalige aanmelding, geautomatiseerde inrichting en meer!
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 47f27746-9057-4b9c-991e-3abf77710f73
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: c952975919cfd948f1b9ea93ff2ac2641a53f923
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-coupa"></a>Zelfstudie: Azure Active Directory-integratie met Coupa
Het doel van deze zelfstudie is om de integratie van Azure en Coupa weer te geven.  
Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

* Een geldige Azure-abonnement
* Een Coupa eenmalige aanmelding (SSO) ingeschakeld abonnement

Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan Coupa kan worden één Meld u aan bij de toepassing met behulp van de [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

Het scenario in deze zelfstudie bestaat uit de volgende elementen:

* De integratie van toepassingen voor Coupa inschakelen
* Eenmalige aanmelding configureren
* Configuratie van gebruikers inrichten
* Gebruikers toewijzen

![Scenario](./media/active-directory-saas-coupa-tutorial/IC791897.png "Scenario")

## <a name="enable-the-application-integration-for-coupa"></a>De integratie van toepassingen voor Coupa inschakelen
Het doel van deze sectie is het inschakelen van de integratie van toepassingen voor Coupa overzicht.

**Als u wilt de integratie van toepassingen voor Coupa inschakelen, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure-portal op het navigatiedeelvenster links op **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-coupa-tutorial/IC700993.png "Active Directory")
2. Van de **Directory** , selecteert u de map waarvoor u wilt inschakelen van Active directory-integratie.
3. De weergave toepassingen in de directoryweergave, klikt u op **toepassingen** in het menu bovenaan.
   
   ![Toepassingen](./media/active-directory-saas-coupa-tutorial/IC700994.png "toepassingen")
4. Klik op **toevoegen** aan de onderkant van de pagina.
   
   ![Toepassing toevoegen](./media/active-directory-saas-coupa-tutorial/IC749321.png "toepassing toevoegen")
5. Op de **wat wilt u doen** dialoogvenster, klikt u op **toevoegen van een toepassing uit de galerie**.
   
   ![Een toepassing toevoegen van gallerry](./media/active-directory-saas-coupa-tutorial/IC749322.png "een toepassing van gallerry toevoegen")
6. In de **zoekvak**, type **Coupa**.
   
   ![Toepassingsgalerie](./media/active-directory-saas-coupa-tutorial/IC791898.png "-Toepassingsgalerie")
7. Selecteer in het deelvenster met resultaten **Coupa**, en klik vervolgens op **Complete** de toepassing toevoegen.
   
   ![Coupa](./media/active-directory-saas-coupa-tutorial/IC791899.png "Coupa")
   
## <a name="configure-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van deze sectie is om een overzicht van gebruikers om te verifiëren Coupa aan hun account in Azure AD dat gebruikmaakt van Federatie op basis van het SAML-protocol in staat te.  

Eenmalige aanmelding voor Coupa configureren, moet u een vingerafdrukwaarde wordt opgehaald uit een certificaat. Als u niet bekend met deze procedure bent, Zie [hoe op te halen van een certificaat vingerafdrukwaarde](http://youtu.be/YKQF266SAxI).

**Voor het configureren van eenmalige aanmelding, moet u de volgende stappen uitvoeren:**

1. Meld u op met uw bedrijf Coupa site als een beheerder.
2. Ga naar **Setup \> beveiligingscontrole**.
   
   ![Beveiligingsmechanismen](./media/active-directory-saas-coupa-tutorial/IC791900.png "beveiligingsmechanismen")
3. Het bestand te downloaden Coupa metagegevens op uw computer, klikt u op **downloaden en importeren SP metagegevens**.
   
   ![Coupa SP metagegevens](./media/active-directory-saas-coupa-tutorial/IC791901.png "Coupa SP metagegevens")
4. In een ander browservenster, moet u zich aanmelden bij de klassieke Azure portal.
5. Op de **Coupa** toepassing Integratiepagina, klikt u op **eenmalige aanmelding configureren** openen de **configureren eenmalige aanmelding** dialoogvenster.
   
   ![Eenmalige aanmelding configureren](./media/active-directory-saas-coupa-tutorial/IC791902.png "eenmalige aanmelding configureren")
6. Op de **hoe wilt u dat gebruikers zich aanmelden op Coupa** pagina **Microsoft Azure AD Single Sign-On**, en klik vervolgens op **volgende**.
   
   ![Eenmalige aanmelding configureren](./media/active-directory-saas-coupa-tutorial/IC791903.png "eenmalige aanmelding configureren")
7. Op de **App-URL configureren** pagina, voert u de volgende stappen uit:
   
   ![App-URL configureren](./media/active-directory-saas-coupa-tutorial/IC791904.png "App-URL configureren")   
   1. In de **aanmelding op URL** textbox, typ de URL die wordt gebruikt door uw gebruikers aanmelden bij uw toepassing Coupa (bijvoorbeeld: '*http://company.Coupa.com*').
   2. Open het gedownloade Coupa metagegevens-bestand en kopieer de **AssertionConsumerService index/URL**.
   3. In de **Coupa antwoord-URL** textbox, plak de **AssertionConsumerService index/URL** waarde.
   4. Klik op **Volgende**.
8. Op de **eenmalige aanmelding configureren op Coupa** pagina voor het downloaden van het metagegevensbestand, klikt u op **metagegevens downloaden**, en sla het bestand lokaal op uw computer.
   
   ![Eenmalige aanmelding configureren](./media/active-directory-saas-coupa-tutorial/IC791905.png "eenmalige aanmelding configureren")
9. Op de site van het bedrijf Coupa, gaat u naar **Setup \> beveiligingscontrole**.
   
   ![Beveiligingsmechanismen](./media/active-directory-saas-coupa-tutorial/IC791900.png "beveiligingsmechanismen")
10. In de **Meld u aan met referenties Coupa** sectie, voert u de volgende stappen uit:  

   ![Meld u aan met referenties Coupa](./media/active-directory-saas-coupa-tutorial/IC791906.png "Meld u aan met referenties Coupa") 
   1. Selecteer **aanmelden via SAML**.
   2. Klik op **Bladeren** uw gedownloade Azure Active-metagegevens-bestand te uploaden.
   3. Klik op **Opslaan**.
11. Bij de klassieke Azure portal, selecteert u de configuratie voor één aanmelding bevestiging en klik op **Complete** sluiten de **configureren eenmalige aanmelding** dialoogvenster.
    
   ![Eenmalige aanmelding configureren](./media/active-directory-saas-coupa-tutorial/IC791907.png "eenmalige aanmelding configureren")
    
## <a name="configure-user-provisioning"></a>Gebruikers inrichten configureren

Om in te schakelen gebruikers van Azure AD aan te melden bij Coupa, moeten ze worden ingericht in Coupa.  

* In het geval van Coupa is inrichting een handmatige taak.

**Als u wilt configureren voor gebruikers inrichten, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **Coupa** bedrijf site als administrator.
2. Klik in het menu bovenaan op **Setup**, en klik vervolgens op **gebruikers**.
   
   ![Gebruikers](./media/active-directory-saas-coupa-tutorial/IC791908.png "gebruikers")
3. Klik op **Create**.
   
   ![Gebruikers maken](./media/active-directory-saas-coupa-tutorial/IC791909.png "gebruikers maken")
4. In de **gebruiker maken** sectie, voert u de volgende stappen uit:
   
   ![Details van gebruiker](./media/active-directory-saas-coupa-tutorial/IC791910.png "Gebruikersdetails")
   
   1. Typ de **aanmelding**, **voornaam**, **achternaam**, **Single Sign-On-ID**, **e** kenmerken van een geldige Azure Active Directory-account die u inrichten in de bijbehorende tekstvakken wilt.
   2. Klik op **Create**.   
   >[!NOTE]
   >De houder van Azure Active Directory-account ontvangt een e-mailbericht met een koppeling naar het account te bevestigen voordat deze geactiveerd wordt. 
   > 

>[!NOTE]
>U kunt andere Coupa gebruiker account hulpmiddelen voor het maken of API's die is geleverd door Coupa aan inrichten AAD-gebruikersaccounts. 
> 

## <a name="assign-users"></a>Gebruikers toewijzen
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat met behulp van uw toepassing toegang tot deze door toe te wijzen.

**Gebruikers om aan te wijzen Coupa, moet u de volgende stappen uitvoeren:**

1. In de klassieke Azure portal een testaccount te maken.
2. Op de ** Coupa ** toepassing Integratiepagina, klikt u op **gebruikers toewijzen**.
   
   ![Gebruikers toewijzen](./media/active-directory-saas-coupa-tutorial/IC791911.png "gebruikers toewijzen")
3. Selecteer uw testgebruiker, klik op **toewijzen**, en klik vervolgens op **Ja** om te bevestigen dat de toewijzing.
   
   ![Ja](./media/active-directory-saas-coupa-tutorial/IC767830.png "Ja")

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het toegangsvenster. Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

