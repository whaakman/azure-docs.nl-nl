---
title: 'Zelfstudie: Azure Active Directory-integratie met SCC LifeCycle | Microsoft Docs'
description: Meer informatie over het SCC LifeCycle gebruiken met Azure Active Directory om te schakelen van eenmalige aanmelding, geautomatiseerde inrichting en meer!
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 9748bf38-ffc3-4d51-a1ae-207ce57104fa
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: 9a30bcca720ff135d0180d73f46e78403e9bca43
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-scc-lifecycle"></a>Zelfstudie: Azure Active Directory-integratie met SCC levenscyclus
Het doel van deze zelfstudie is om de integratie van Azure en SCC LifeCycle weer te geven.  

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

* Een geldige Azure-abonnement
* Een SCC LifeCycle eenmalige aanmelding (SSO) ingeschakeld abonnement

Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan de levenscyclus van SCC worden kunnen eenmalige aanmelding in de toepassing op uw site in de levenscyclus van SCC bedrijf (serviceprovider geïnitieerd aanmelding) of met behulp van de [Inleiding tot de toegang Deelvenster](active-directory-saas-access-panel-introduction.md).

Het scenario in deze zelfstudie bestaat uit de volgende elementen:

1. Inschakelen van de integratie van toepassingen voor SCC levenscyclus
2. Configureren van eenmalige aanmelding (SSO)
3. Configuratie van gebruikers inrichten
4. Gebruikers toewijzen

![Scenario](./media/active-directory-saas-scc-lifecycle-tutorial/IC794120.png "Scenario")

## <a name="enable-the-application-integration-for-scc-lifecycle"></a>De integratie van toepassingen voor SCC levenscyclus inschakelen
Het doel van deze sectie is het inschakelen van de integratie van toepassingen voor SCC levenscyclus overzicht.

**Om de integratie van toepassingen voor SCC levenscyclus inschakelt, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure-portal op het navigatiedeelvenster links op **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-scc-lifecycle-tutorial/IC700993.png "Active Directory")
2. Van de **Directory** , selecteert u de map waarvoor u wilt inschakelen van Active directory-integratie.
3. De weergave toepassingen in de directoryweergave, klikt u op **toepassingen** in het menu bovenaan.
   
    ![Toepassingen](./media/active-directory-saas-scc-lifecycle-tutorial/IC700994.png "toepassingen")
4. Klik op **toevoegen** aan de onderkant van de pagina.
   
    ![Toepassing toevoegen](./media/active-directory-saas-scc-lifecycle-tutorial/IC749321.png "toepassing toevoegen")
5. Op de **wat wilt u doen** dialoogvenster, klikt u op **toevoegen van een toepassing uit de galerie**.
   
    ![Een toepassing toevoegen van gallerry](./media/active-directory-saas-scc-lifecycle-tutorial/IC749322.png "een toepassing van gallerry toevoegen")
6. In de **zoekvak**, type **SCC LifeCycle**.
   
    ![Toepassingsgalerie](./media/active-directory-saas-scc-lifecycle-tutorial/IC794121.png "-Toepassingsgalerie")
7. Selecteer in het deelvenster met resultaten **SCC LifeCycle**, en klik vervolgens op **Complete** de toepassing toevoegen.
   
    ![De levenscyclus van SCC](./media/active-directory-saas-scc-lifecycle-tutorial/IC795082.png "SCC levenscyclus")
   
## <a name="configure-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van deze sectie is het overzicht van het inschakelen van gebruikers worden geverifieerd bij de levenscyclus van SCC aan hun account in Azure AD dat gebruikmaakt van Federatie op basis van het SAML-protocol.

**Voor het configureren van eenmalige aanmelding, moet u de volgende stappen uitvoeren:**

1. In de klassieke Azure portal op de **SCC LifeCycle** toepassing Integratiepagina, klikt u op **eenmalige aanmelding configureren** openen de ** configureren eenmalige aanmelding ** dialoogvenster.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-scc-lifecycle-tutorial/IC794122.png "eenmalige aanmelding configureren")
2. Op de **hoe wilt u dat gebruikers zich aanmelden op voor de levenscyclus van SCC** pagina **Microsoft Azure AD Single Sign-On**, en klik vervolgens op **volgende**.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-scc-lifecycle-tutorial/IC794123.png "eenmalige aanmelding configureren")
3. Op de **App-URL configureren** pagina in de **aanmelding op URL** textbox, typ de URL gebruikt door uw gebruikers aanmelden bij uw SCC LifeCycle-toepassing met het volgende patroon volgen '*https://bs1.scc.com/ lc7/Welcome/Customer/PICTtest.aspx*', en klik vervolgens op **volgende**.
   
    ![App-URL configureren](./media/active-directory-saas-scc-lifecycle-tutorial/IC794124.png "App-URL configureren")
4. Op de **eenmalige aanmelding configureren op de levenscyclus van SCC** pagina, klikt u op **metagegevens downloaden**, en sla het bestand met metagegevens lokaal op uw computer.
   
   ![Eenmalige aanmelding configureren](./media/active-directory-saas-scc-lifecycle-tutorial/IC795083.png "eenmalige aanmelding configureren")
5. Doorsturen die metagegevensbestand aan SCC LifeCycle Support team.
   
   >[!NOTE]
   >Eenmalige aanmelding moet worden ingeschakeld door het ondersteuningsteam SCC levenscyclus.
   > 
   > 

6. Bij de klassieke Azure portal, selecteert u de configuratie voor één aanmelding bevestiging en klik op **Complete** sluiten de **configureren eenmalige aanmelding** dialoogvenster.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-scc-lifecycle-tutorial/IC794125.png "eenmalige aanmelding configureren")
   
## <a name="configure-user-provisioning"></a>Gebruikers inrichten configureren

Zodat gebruikers van Azure AD aan te melden bij de levenscyclus van SCC moeten ze worden ingericht in de levenscyclus van SCC. Er is geen actie-item voor configuratie gebruikersaanvragen naar SCC levenscyclus.

Wanneer een toegewezen gebruiker probeert aan te melden bij SCC levenscyclus, wordt automatisch een SCC LifeCycle-account gemaakt, indien nodig.

>[!NOTE]
>U kunt andere SCC LifeCycle gebruiker account hulpmiddelen voor het maken of API's geleverd door SCC LifeCycle aan inrichten AAD-gebruikersaccounts.
> 
> 

## <a name="assign-users"></a>Gebruikers toewijzen
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat met behulp van uw toepassing toegang tot deze door toe te wijzen.

**Gebruikers om aan te wijzen SCC levenscyclus, moet u de volgende stappen uitvoeren:**

1. In de klassieke Azure portal een testaccount te maken.
2. Op de ** SCC LifeCycle ** toepassing Integratiepagina, klikt u op **gebruikers toewijzen**.
   
    ![Gebruikers toewijzen](./media/active-directory-saas-scc-lifecycle-tutorial/IC794126.png "gebruikers toewijzen")
3. Selecteer uw testgebruiker, klik op **toewijzen**, en klik vervolgens op **Ja** om te bevestigen dat de toewijzing.
   
    ![Ja](./media/active-directory-saas-scc-lifecycle-tutorial/IC767830.png "Ja")

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het toegangsvenster. Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

