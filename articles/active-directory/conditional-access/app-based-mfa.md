---
title: 'Snelstart: multi-factor authentication (MFA) voor specifieke apps met voorwaardelijke toegang van Azure Active Directory vereisen | Microsoft Docs'
description: In deze snelstartgids leert u hoe u uw verificatievereisten voor het type gebruikte cloud-app met behulp van voorwaardelijke toegang van Azure Active Directory (Azure AD) kunt verbinden.
services: active-directory
keywords: voorwaardelijke toegang tot apps, voorwaardelijke toegang met Azure AD, beveiligde toegang tot bedrijfsresources, beleid voor voorwaardelijke toegang
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: conditional-access
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/13/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: eee4d73042232aabd995a749b7848306be0ef655
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39628253"
---
# <a name="quickstart-require-mfa-for-specific-apps-with-azure-active-directory-conditional-access"></a>Snelstartgids: MFA vereisen voor specifieke apps met voorwaardelijke toegang van Azure Active Directory 

Ter vereenvoudiging van de aanmeldingservaring van uw gebruikers, is het raadzaam om toe te staan dat ze zich aanmelden bij uw cloud-apps met behulp van een gebruikersnaam en wachtwoord. Veel omgevingen hebben echter ten minste een aantal apps waarvoor u wordt aangeraden een sterkere vorm van verificatie-account, zoals multi-factor authentication (MFA) vereist. Dit kan zijn, voor de voorbeeld-waar, voor toegang tot e-mailsysteem van uw organisatie of uw HR-apps. In Azure Active Directory (Azure AD), kunt u dit doel met beleid voor voorwaardelijke toegang uitvoeren.    

Deze quickstart laat zien hoe u configureert een [beleid voor voorwaardelijke toegang van Azure AD](../active-directory-conditional-access-azure-portal.md) waarvoor multi-factor Authentication-verificatie is vereist voor een geselecteerde cloud-app in uw omgeving.

![Beleid maken](./media/app-based-mfa/32.png)


Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.



## <a name="prerequisites"></a>Vereisten 

Als u wilt het scenario in deze snelstartgids hebt voltooid, hebt u het volgende nodig:

- **Toegang tot een Azure AD Premium-editie** -voorwaardelijke toegang van Azure AD is een Azure AD Premium-capaciteit. 

- **Een testaccount met de naam Isabella Simonsen** : als u niet hoe ik een testaccount maakt weet, Zie [cloudgebruikers toevoegen](../fundamentals/add-users-azure-active-directory.md#add-cloud-based-users).


## <a name="test-your-sign-in"></a>Test de aanmelding

Het doel van deze stap is om een indruk van de ervaring aanmelden zonder een beleid voor voorwaardelijke toegang.

**Uw omgeving initialiseren:**

1. Meld u aan uw Azure-portal als Isabella Simonsen.

2. Meld u af.


## <a name="create-your-conditional-access-policy"></a>Uw beleid voor voorwaardelijke toegang maken 

Deze sectie wordt beschreven hoe u de vereiste voorwaardelijk toegangsbeleid maken. Maakt gebruik van het scenario in deze Quick Start:

- De Azure-portal als tijdelijke aanduiding voor een cloud-app die MFA vereist. 
- De voorbeeldgebruiker in voor het testen van het beleid voor voorwaardelijke toegang.  

Stel in het beleid:

|Instelling |Waarde|
|---     | --- |
|Gebruikers en groepen | Isabella Simonsen |
|Cloud-apps | Beheer van Microsoft Azure |
|Toegang verlenen | Meervoudige verificatie vereisen |
 

![Beleid maken](./media/app-based-mfa/31.png)

 


**Uw beleid voor voorwaardelijke toegang configureren:**

1. Aanmelden bij uw [Azure-portal](https://portal.azure.com) als hoofdbeheerder, beveiligingsbeheerder of een beheerder van voorwaardelijke toegang.

2. Klik in de Azure-portal op de navigatiebalk links op **Azure Active Directory**. 

    ![Azure Active Directory](./media/app-based-mfa/02.png)

3. Op de **Azure Active Directory** pagina, in de **beheren** sectie, klikt u op **voorwaardelijke toegang**.

    ![Voorwaardelijke toegang](./media/app-based-mfa/03.png)
 
4. Op de **voorwaardelijke toegang** in de werkbalk bovenaan op de pagina, klikt u op **toevoegen**.

    ![Toevoegen](./media/app-based-mfa/04.png)

5. Op de **nieuw** pagina, in de **naam** tekstvak, type **MFA vereisen voor toegang tot Azure portal**.

    ![Naam](./media/app-based-mfa/05.png)

6. In de **toewijzing** sectie, klikt u op **gebruikers en groepen**.

    ![Gebruikers en groepen](./media/app-based-mfa/06.png)

7. Op de **gebruikers en groepen** pagina, voert u de volgende stappen uit:

    ![Gebruikers en groepen](./media/app-based-mfa/24.png)

    a. Klik op **gebruikers en groepen selecteren**, en selecteer vervolgens **gebruikers en groepen**.

    b. Klik op **Selecteren**.

    c. Op de **Selecteer** pagina, selecteert u **Isabella Simonsen**, en klik vervolgens op **Selecteer**.

    d. Op de **gebruikers en groepen** pagina, klikt u op **gedaan**.

8. Klik op **Cloud-apps**.

    ![Cloud-apps](./media/app-based-mfa/08.png)

9. Op de **Cloud-apps** pagina, voert u de volgende stappen uit:

    ![Cloud-apps selecteren](./media/app-based-mfa/26.png)

    a. Klik op **apps selecteren**.

    b. Klik op **Selecteren**.

    c. Op de **Selecteer** pagina, selecteert u **Microsoft Azure Management**, en klik vervolgens op **Selecteer**.

    d. Op de **Cloud-apps** pagina, klikt u op **gedaan**.


10. In de **besturingselementen voor toegang** sectie, klikt u op **verlenen**.

    ![Besturingselementen voor toegang](./media/app-based-mfa/10.png)

11. Op de **verlenen** pagina, voert u de volgende stappen uit:

    ![Verlenen](./media/app-based-mfa/11.png)

    a. Selecteer **toegang verlenen**.

    a. Selecteer **meervoudige verificatie vereisen**.

    b. Klik op **Selecteren**.

12. In de **beleid inschakelen** sectie, klikt u op **op**.

    ![Beleid inschakelen](./media/app-based-mfa/18.png)

13. Klik op **Create**.


## <a name="evaluate-a-simulated-sign-in"></a>Een gesimuleerde aanmelding evalueren

Nu dat u uw beleid voor voorwaardelijke toegang hebt geconfigureerd, wilt u waarschijnlijk weet of deze werkt zoals verwacht. Gebruik als een eerste stap de voorwaardelijke toegang beleid hulpprogramma what-if om te simuleren een aanmelding van uw testgebruiker. De simulatie maakt een schatting van het effect dat aanmelding heeft op uw beleid en genereert een simulatierapport.  

Initialiseren van de wat als hulpprogramma voor het evalueren van beleid is ingesteld:

- **Isabella Simonsen** als gebruiker 
- **Microsoft Azure Management** als cloud-app

 Te klikken op **wat gebeurt er als** maakt u een simulatierapport waarin wordt weergegeven:

- **MFA vereisen voor toegang tot Azure portal** onder **beleidsregels die worden toegepast** 
- **Meervoudige verificatie vereisen** als **besturingselementen verlenen**.

![Wat gebeurt er als hulpprogramma voor beleid](./media/app-based-mfa/23.png)



**Uw beleid voor voorwaardelijke toegang evalueren:**

1. Op de [voorwaardelijke toegang - beleid](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) in het menu bovenaan op de pagina, klikt u op **wat gebeurt er als**.  
 
    ![What If](./media/app-based-mfa/14.png)

2. Klik op **gebruikers**, selecteer **Isabella Simonsen**, en klik vervolgens op **Selecteer**.

    ![Gebruiker](./media/app-based-mfa/15.png)

2. Selecteer een cloud-app, moet u de volgende stappen uitvoeren:

    ![Cloud-apps](./media/app-based-mfa/16.png)

    a. Klik op **Cloud-apps**.

    b. Op de **pagina voor Cloud-apps**, klikt u op **apps selecteren**.

    c. Klik op **Selecteren**.

    d. Op de **Selecteer** pagina, selecteert u **Microsoft Azure Management**, en klik vervolgens op **Selecteer**.

    e. Klik op de pagina van de cloud-apps op **gedaan**.

3. Klik op **wat gebeurt er als**.


## <a name="test-your-conditional-access-policy"></a>Testen van uw beleid voor voorwaardelijke toegang

In de vorige sectie hebt u geleerd hoe u een gesimuleerde aanmelding evalueren. Naast een simulatie, moet u ook testen van uw beleid voor voorwaardelijke toegang om ervoor te zorgen dat deze naar verwachting werkt. 

Als u wilt testen van uw beleid, willen aanmelden bij uw [Azure-portal](https://portal.azure.com) met behulp van uw **Isabella Simonsen** test-account. U ziet een dialoogvenster waarin u uw account voor aanvullende beveiligingsverificatie instellen is vereist.

![Multi-Factor Authentication](./media/app-based-mfa/22.png)


## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u niet meer nodig hebt, verwijdert u de testgebruiker en het beleid voor voorwaardelijke toegang:

- Als u niet hoe u een Azure AD-gebruiker verwijdert weet, Zie [gebruikers verwijderen uit Azure AD](../fundamentals/add-users-azure-active-directory.md#delete-users-from-azure-ad).

- Als u wilt verwijderen van uw beleid, selecteert u uw beleid en klik vervolgens op **verwijderen** in de werkbalk Snelle toegang.

    ![Multi-Factor Authentication](./media/app-based-mfa/33.png)


## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Vereisen dat de gebruiksrechtovereenkomst moet zijn geaccepteerd](require-tou.md)
> [toegang blokkeren als er een risico voor de sessie wordt gedetecteerd](app-sign-in-risk.md)
