---
title: Snelstartgids - toegang blokkeren wanneer het risico van een sessie wordt gedetecteerd met voorwaardelijke toegang van Azure Active Directory | Microsoft Docs
description: In deze snelstartgids leert u hoe u een beleid voor voorwaardelijke toegang van Azure Active Directory (Azure AD) voor het blokkeren van aanmeldingen op basis van de sessie risico's kunt configureren.
services: active-directory
keywords: voorwaardelijke toegang tot apps, voorwaardelijke toegang met Azure AD, beveiligde toegang tot bedrijfsresources, beleid voor voorwaardelijke toegang
documentationcenter: ''
author: MarkusVi
manager: daveba
ms.assetid: ''
ms.service: active-directory
ms.component: conditional-access
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/14/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 899fc10d4b5a4c9bdbfd46cc1ff71f5232cffbc6
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54450282"
---
# <a name="quickstart-block-access-when-a-session-risk-is-detected-with-azure-active-directory-conditional-access"></a>Quickstart: Toegang blokkeren als er een risico voor de sessie wordt gedetecteerd met voorwaardelijke toegang van Azure Active Directory  

Om te voorkomen dat uw omgeving beveiligd, is het raadzaam om verdachte gebruikers zich insign in activiteiten te blokkeren. [Azure Active Directory (Azure AD) Identity Protection](../active-directory-identityprotection.md) analyseert elke aanmelding en berekent de kans dat een aanmelding bij poging is niet uitgevoerd door de rechtmatige eigenaar van een gebruikersaccount. De kans (laag, Gemiddeld, hoog) wordt aangegeven in de vorm van een berekende waarde met de naam [aanmelden risiconiveaus](conditions.md#sign-in-risk). Door in te stellen de voorwaarde voor aanmeldingsrisico, kunt u een beleid voor voorwaardelijke toegang om te reageren op specifieke aanmeldingsrisico niveaus configureren. 

Deze quickstart laat zien hoe u configureert een [beleid voor voorwaardelijke toegang](../active-directory-conditional-access-azure-portal.md) die een aanmelding wordt geblokkeerd als het niveau van een geconfigureerde aanmeldingsrisico is gedetecteerd. 

![Beleid maken](./media/app-sign-in-risk/1000.png)


Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.



## <a name="prerequisites"></a>Vereisten 

Voor het voltooien van het scenario in deze zelfstudie hebt u het volgende nodig:

- **Toegang tot een Azure AD Premium P2-editie** -voorwaardelijke toegang is de mogelijkheid van een Azure AD Premium P1, u moet een P2-editie omdat het scenario in deze snelstartgids Identity Protection is vereist. 

- **Identity Protection** -Identity Protection wordt ingeschakeld door het scenario in deze Quick Start is vereist. Als u niet hoe u Identity Protection inschakelen weet, raadpleegt u [inschakelen van Azure Active Directory Identity Protection](../identity-protection/enable.md).

- **Tor Browser** : de [Tor Browser](https://www.torproject.org/projects/torbrowser.html.en) is ontworpen om u te helpen u uw privacy online beschermen. Identity Protection detecteert een aanmelding vanuit een Tor Browser als **aanmeldingen vanaf anonieme IP-adressen**, die is voorzien van een gemiddeld risico-niveau. Zie [Risicogebeurtenissen in Azure Active Directory](../reports-monitoring/concept-risk-events.md) voor meer informatie.  

- **Een testaccount met de naam Alain Charon** : als u niet hoe ik een testaccount maakt weet, Zie [cloudgebruikers toevoegen](../fundamentals/add-users-azure-active-directory.md#add-a-new-user).


## <a name="test-your-sign-in"></a>Test de aanmelding 

Het doel van deze stap is om ervoor te zorgen dat uw testaccount toegang heeft tot uw tenant met behulp van de Tor-Browser.

**Voor het testen van de aanmelding:**

1. Aanmelden bij uw [Azure-portal](https://portal.azure.com) als **Alain Charon**.

2. Meld u af. 


## <a name="create-your-conditional-access-policy"></a>Uw beleid voor voorwaardelijke toegang maken 

Het scenario in deze snelstartgids wordt een aanmelding vanuit een Tor-Browser gebruikt voor het genereren van een gedetecteerde **aanmeldingen vanaf anonieme IP-adressen** risicogebeurtenis. Het risiconiveau van deze risicogebeurtenis is normaal. Om te reageren op deze risicogebeurtenis, kunt u de voorwaarde voor aanmeldingsrisico instellen op Gemiddeld. In een productieomgeving, moet u de voorwaarde voor aanmeldingsrisico instellen op hoge of op Gemiddeld en hoog.     

Deze sectie wordt beschreven hoe u de vereiste voorwaardelijk toegangsbeleid maken. Stel in het beleid:

|Instelling |Waarde|
|---     | --- |
| Gebruikers en groepen | Alain Charon  |
| Cloud-apps | Alle cloud-apps |
| Aanmeldingsrisico | Middelgroot |
| Verlenen | Toegang blokkeren |
 

![Beleid maken](./media/app-sign-in-risk/130.png)

 


**Uw beleid voor voorwaardelijke toegang configureren:**

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) als hoofdbeheerder, beveiligingsbeheerder of beheerder voor voorwaardelijke toegang.

2. Klik in de Azure-portal op de navigatiebalk links op **Azure Active Directory**. 

    ![Azure Active Directory](./media/app-sign-in-risk/02.png)

3. Op de **Azure Active Directory** pagina, in de **Security** sectie, klikt u op **voorwaardelijke toegang**.

    ![Voorwaardelijke toegang](./media/app-sign-in-risk/03.png)
 
4. Op de **voorwaardelijke toegang** in de werkbalk bovenaan op de pagina, klikt u op **toevoegen**.

    ![Name](./media/app-sign-in-risk/108.png)

5. Op de **nieuw** pagina, in de **naam** tekstvak, type **toegang blokkeren voor middelgrote risiconiveau**.

    ![Name](./media/app-sign-in-risk/104.png)

6. In de **toewijzing** sectie, klikt u op **gebruikers en groepen**.

    ![Gebruikers en groepen](./media/app-sign-in-risk/06.png)

7. Op de **gebruikers en groepen** pagina:

    ![Voorwaardelijke toegang](./media/app-sign-in-risk/107.png)

    a. Klik op **gebruikers en groepen selecteren**, en selecteer vervolgens **gebruikers en groepen**.

    b. Klik op **Selecteren**.

    c. Op de **Selecteer** pagina, selecteert u **Alain Charon**, en klik vervolgens op **Selecteer**.

    d. Op de **gebruikers en groepen** pagina, klikt u op **gedaan**.

8. Klik op **Cloud-apps**.

    ![Cloud-apps](./media/app-sign-in-risk/08.png)

9. Op de **Cloud-apps** pagina:

    ![Voorwaardelijke toegang](./media/app-sign-in-risk/109.png)

    a. Klik op **alle cloud-apps**.

    b. Klik op **Gereed**.

10. Klik op **voorwaarden**. 

    ![Besturingselementen voor toegang](./media/app-sign-in-risk/19.png)

11. Op de **voorwaarden** pagina:

    ![Niveau van aanmeldingsrisico](./media/app-sign-in-risk/21.png)

    a. Klik op **aanmeldingsrisico**.
 
    b. Als **configureren**, klikt u op **Ja**.

    c. Niveau van aanmeldingsrisico selecteren **gemiddeld**.

    d. Klik op **Selecteren**.

    e. Op de **voorwaarden** pagina, klikt u op **gedaan**.



10. In de **besturingselementen voor toegang** sectie, klikt u op **verlenen**.

    ![Besturingselementen voor toegang](./media/app-sign-in-risk/10.png)

11. Op de **verlenen** pagina:

    ![Voorwaardelijke toegang](./media/app-sign-in-risk/105.png)

    a. Selecteer **toegang blokkeren**.

    b. Klik op **Selecteren**.

12. In de **beleid inschakelen** sectie, klikt u op **op**.

    ![Beleid inschakelen](./media/app-sign-in-risk/18.png)

13. Klik op **Create**.


## <a name="evaluate-a-simulated-sign-in"></a>Een gesimuleerde aanmelding evalueren

Nu u uw beleid voor voorwaardelijke toegang hebt geconfigureerd, wilt u weten of het werkt zoals u wilt. Als een eerste stap gebruikt u de voorwaardelijke toegang **wat gebeurt er als beleid hulpprogramma** voor het simuleren van een aanmelding van uw testgebruiker. De simulatie schat de impact van deze aanmelding op uw beleid in en genereert een simulatierapport.  

Bij het uitvoeren van de **wat gebeurt er als beleid hulpprogramma** voor dit scenario, de **toegang blokkeren voor middelgrote risiconiveau** moet worden weergegeven onder **beleidsregels die worden toegepast**. 

![Gebruiker](./media/app-sign-in-risk/117.png)


**Uw beleid voor voorwaardelijke toegang evalueren:**

1. Op de [voorwaardelijke toegang - beleid](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) in het menu bovenaan op de pagina, klikt u op **wat gebeurt er als**.  
 
    ![What If](./media/app-sign-in-risk/14.png)

2. Klik op **gebruiker**, selecteer **Alan Charon** op de **gebruikers** pagina en klik vervolgens op **Selecteer**.

    ![Gebruiker](./media/app-sign-in-risk/116.png)

3. Als **aanmeldingsrisico**, selecteer **gemiddeld**.

    ![Gebruiker](./media/app-sign-in-risk/119.png)


3. Klik op **wat gebeurt er als**.


## <a name="test-your-conditional-access-policy"></a>Uw voorwaardelijke toegangsbeleid testen

In de vorige sectie hebt u geleerd hoe u een gesimuleerde aanmelding evalueren. Naast een simulatie, moet u ook testen van uw beleid voor voorwaardelijke toegang om ervoor te zorgen dat deze naar verwachting werkt. 

Als u wilt testen van uw beleid, willen aanmelden bij uw [Azure-portal](https://portal.azure.com) als **Alan Charon** met behulp van de Tor-Browser. Uw aanmeldingspoging moet worden geblokkeerd door uw beleid voor voorwaardelijke toegang.

![Multi-Factor Authentication](./media/app-sign-in-risk/118.png)


## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u niet meer nodig hebt, verwijdert u de testgebruiker, de Tor-Browser en het beleid voor voorwaardelijke toegang:

- Als u niet hoe u een Azure AD-gebruiker verwijdert weet, Zie [gebruikers verwijderen uit Azure AD](../fundamentals/add-users-azure-active-directory.md#delete-a-user).

- Als u wilt verwijderen van uw beleid, selecteert u uw beleid en klik vervolgens op **verwijderen** in de werkbalk Snelle toegang.

    ![Multi-Factor Authentication](./media/app-sign-in-risk/33.png)

- Zie voor instructies voor het verwijderen van de Tor Browser, [verwijderen](https://tb-manual.torproject.org/uninstalling/).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Vereisen dat de gebruiksrechtovereenkomst moet zijn geaccepteerd](require-tou.md)
> [MFA vereisen voor specifieke apps](app-based-mfa.md)

