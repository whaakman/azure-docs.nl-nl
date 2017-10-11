---
title: 'Zelfstudie: Azure Active Directory-integratie met Qualtrics | Microsoft Docs'
description: Meer informatie over het gebruik van Qualtrics met Azure Active Directory voor het inschakelen van eenmalige aanmelding, geautomatiseerde inrichting en meer!
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 4df889ab-2685-4d15-a163-1ba26567eeda
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: 2fcde595a40dafda7549f5bccb582b57585b314e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-qualtrics"></a>Zelfstudie: Azure Active Directory-integratie met Qualtrics
Het doel van deze zelfstudie is om de integratie van Azure en Qualtrics weer te geven.  

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

* Een geldige Azure-abonnement
* Een Qualtrics eenmalige aanmelding (SSO) ingeschakeld abonnement

Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan Qualtrics worden kunnen eenmalige aanmelding in de toepassing op uw Qualtrics bedrijf site (serviceprovider geïnitieerd aanmelding) of met behulp van de [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

Het scenario in deze zelfstudie bestaat uit de volgende elementen:

1. De integratie van toepassingen voor Qualtrics inschakelen
2. Configureren van eenmalige aanmelding (SSO)
3. Configuratie van gebruikers inrichten
4. Gebruikers toewijzen

![Scenario](./media/active-directory-saas-qualtrics-tutorial/IC789542.png "Scenario")

## <a name="enabling-the-application-integration-for-qualtrics"></a>De integratie van toepassingen voor Qualtrics inschakelen
Het doel van deze sectie is het inschakelen van de integratie van toepassingen voor Qualtrics overzicht.

**Als u wilt de integratie van toepassingen voor Qualtrics inschakelen, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure-portal op het navigatiedeelvenster links op **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-qualtrics-tutorial/IC700993.png "Active Directory")
2. Van de **Directory** , selecteert u de map waarvoor u wilt inschakelen van Active directory-integratie.
3. De weergave toepassingen in de directoryweergave, klikt u op **toepassingen** in het menu bovenaan.
   
   ![Toepassingen](./media/active-directory-saas-qualtrics-tutorial/IC700994.png "toepassingen")
4. Klik op **toevoegen** aan de onderkant van de pagina.
   
   ![Toepassing toevoegen](./media/active-directory-saas-qualtrics-tutorial/IC749321.png "toepassing toevoegen")
5. Op de **wat wilt u doen** dialoogvenster, klikt u op **toevoegen van een toepassing uit de galerie**.
   
   ![Een toepassing toevoegen van gallerry](./media/active-directory-saas-qualtrics-tutorial/IC749322.png "een toepassing van gallerry toevoegen")
6. In de **zoekvak**, type **Qualtrics**.
   
   ![Toepassingsgalerie](./media/active-directory-saas-qualtrics-tutorial/IC789543.png "-Toepassingsgalerie")
7. Selecteer in het deelvenster met resultaten **Qualtrics**, en klik vervolgens op **Complete** de toepassing toevoegen.
   
   ![Qualtrics](./media/active-directory-saas-qualtrics-tutorial/IC789544.png "Qualtrics")
   
## <a name="configure-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van deze sectie is om een overzicht van gebruikers om te verifiëren Qualtrics aan hun account in Azure AD dat gebruikmaakt van Federatie op basis van het SAML-protocol in staat te.

**Voor het configureren van eenmalige aanmelding, moet u de volgende stappen uitvoeren:**

1. In de klassieke Azure-portal op de **Qualtrics** toepassing Integratiepagina, klikt u op **eenmalige aanmelding configureren** openen de **configureren eenmalige aanmelding** dialoogvenster.
   
   ![Eenmalige aanmelding configureren](./media/active-directory-saas-qualtrics-tutorial/IC789545.png "eenmalige aanmelding configureren")
2. Op de **hoe wilt u dat gebruikers zich aanmelden op Qualtrics** pagina **Microsoft Azure AD Single Sign-On**, en klik vervolgens op **volgende**.
   
   ![Eenmalige aanmelding configureren](./media/active-directory-saas-qualtrics-tutorial/IC789546.png "eenmalige aanmelding configureren")
3. Op de **App-URL configureren** pagina de **Qualtrics aanmelding op URL** textbox, typ de URL (bijvoorbeeld: '*https://ssotest2ut1.qualtrics.com*'), en klik vervolgens op **Volgende**.
   
   ![App-URL configureren](./media/active-directory-saas-qualtrics-tutorial/IC789547.png "App-URL configureren")
4. Op de **eenmalige aanmelding configureren op Qualtrics** pagina, klikt u op **metagegevens downloaden**, en sla het bestand met metagegevens op uw computer.
   
   ![Eenmalige aanmelding configureren](./media/active-directory-saas-qualtrics-tutorial/IC789548.png "eenmalige aanmelding configureren")
5. Het metagegevensbestand verzenden naar het ondersteuningsteam Qualtrics.
   
   >[!NOTE]
   >De SSO-configuratie is door het ondersteuningsteam Qualtrics worden uitgevoerd. U ontvangt een melding zodra de configuratie is voltooid.
   > 
   > 
6. Bij de klassieke Azure portal, selecteert u de configuratie voor één aanmelding bevestiging en klik op **Complete** sluiten de **configureren eenmalige aanmelding** dialoogvenster.
   
   ![Eenmalige aanmelding configureren](./media/active-directory-saas-qualtrics-tutorial/IC789549.png "eenmalige aanmelding configureren")
   
## <a name="configure-user-provisioning"></a>Gebruikers inrichten configureren

Er is geen actie-item voor gebruikers inrichten voor Qualtrics configuratie. Wanneer een toegewezen gebruiker probeert aan te melden bij met het toegangsvenster Qualtrics, Qualtrics u controleert of de gebruiker bestaat.  

Als er nog geen gebruikersaccount beschikbaar is, wordt het automatisch gemaakt door Qualtrics.

## <a name="assign-users"></a>Gebruikers toewijzen
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat met behulp van uw toepassing toegang tot deze door toe te wijzen.

**Gebruikers om aan te wijzen Qualtrics, moet u de volgende stappen uitvoeren:**

1. In de klassieke Azure portal een testaccount te maken.
2. Op de **Qualtrics** toepassing Integratiepagina, klikt u op **gebruikers toewijzen**.
   
   ![Gebruikers toewijzen](./media/active-directory-saas-qualtrics-tutorial/IC789550.png "gebruikers toewijzen")
3. Selecteer uw testgebruiker, klik op **toewijzen**, en klik vervolgens op **Ja** om te bevestigen dat de toewijzing.
   
   ![Ja](./media/active-directory-saas-qualtrics-tutorial/IC767830.png "Ja")

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het toegangsvenster. Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

