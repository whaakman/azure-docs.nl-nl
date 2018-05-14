---
title: Quick Start - Multi-factor authentication (MFA) vereisen voor specifieke apps met voorwaardelijke toegang van Azure Active Directory | Microsoft Docs
description: In deze snelstartgids leert u hoe u uw verificatievereisten naar het type van de gebruikte cloud-app met behulp van voorwaardelijke toegang van Azure Active Directory (Azure AD) kunt verbinden.
services: active-directory
keywords: voorwaardelijke toegang tot apps, voorwaardelijke toegang met Azure AD, beveiligde toegang tot bedrijfsresources, beleidsregels voor voorwaardelijke toegang
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/11/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: d675236f36840858f0f011484392186d355ac6df
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2018
---
# <a name="quickstart-require-mfa-for-specific-apps-with-azure-active-directory-conditional-access"></a>Snelstartgids: MFA vereisen voor specifieke apps met voorwaardelijke toegang van Azure Active Directory 

Om te vereenvoudigen de aanmeldingservaring van uw gebruikers, is het raadzaam zodat ze aan te melden bij uw cloud-apps met een gebruikersnaam en een wachtwoord. Veel omgevingen hebben echter ten minste enkele apps waarvoor u wordt aangeraden om te vereisen een sterkere vorm van accountverificatie,, zoals multi-factor authentication (MFA). Dit kan zijn, voor de voorbeeld-waar, voor toegang tot e-mailsysteem van uw organisatie of uw HR-apps. In Azure Active Directory (Azure AD), u kunt dit te bereiken met een beleid voor voorwaardelijke toegang.    

Deze snelstartgids toont hoe u configureert een [beleid voor voorwaardelijke toegang van Azure AD](active-directory-conditional-access-azure-portal.md) waarvoor multi-factor authentication-server is vereist voor een set met geselecteerde cloud-apps in uw omgeving.


## <a name="scenario-description"></a>Scenariobeschrijving

Het scenario in dit artikel wordt de Azure-portal als tijdelijke aanduiding voor een cloud-app die MFA is vereist voor een specifieke gebruiker. Isabella Simonsen is een gebruiker in uw organisatie. Wanneer zij zich heeft aangemeld bij uw Azure-portal, wilt u haar verder haar account verifiëren met MFA.

![Multi-Factor Authentication](./media/active-directory-conditional-access-app-based-mfa/22.png)



## <a name="prerequisites"></a>Vereisten 

Voor het voltooien van het scenario in deze snelstartgids hebt u het volgende nodig:

- **Toegang tot een Azure AD Premium-editie** -voorwaardelijke toegang van Azure AD is een Azure AD Premium-functie. Als u geen toegang tot een Azure AD Premium edition hebt, kunt u [aanmelden voor een proefversie](https://azure.microsoft.com/trial/get-started-active-directory/).

- **Een testaccount genoemd Isabella Simonsen** : als u niet hoe u een testaccount maken weet, lezen [deze instructies](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory).



## <a name="create-your-conditional-access-policy"></a>Maken van uw beleid voor voorwaardelijke toegang 

Deze sectie toont het maken van beleid voor voorwaardelijke toegang vereist.  
U instellen in uw beleid:

|Instelling |Waarde|
|---     | --- |
|Gebruikers en groepen | Isabella Simonsen |
|Cloud-apps | Beheer van Microsoft Azure |
|Verlenen | Meervoudige verificatie vereisen |
 

![Beleid maken](./media/active-directory-conditional-access-app-based-mfa/31.png)




**Uw beleid voor voorwaardelijke toegang configureren:**

1. Aanmelden bij uw [Azure-portal](https://portal.azure.com) als globale beheerder.

2. In de Azure-portal op de navigatiebalk links, klikt u op **Azure Active Directory**. 

    ![Azure Active Directory](./media/active-directory-conditional-access-app-based-mfa/02.png)

3. Op de **Azure Active Directory** pagina in de **beheren** sectie, klikt u op **voorwaardelijke toegang**.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-app-based-mfa/03.png)
 
4. Op de **voorwaardelijke toegang** pagina in de werkbalk bovenaan, klikt u op **toevoegen**.

    ![Toevoegen](./media/active-directory-conditional-access-app-based-mfa/04.png)

5. Op de **nieuw** pagina in de **naam** textbox type **MFA vereisen voor toegang tot de Azure portal**.

    ![Naam](./media/active-directory-conditional-access-app-based-mfa/05.png)

6. In de **toewijzing** sectie, klikt u op **gebruikers en groepen**.

    ![Gebruikers en groepen](./media/active-directory-conditional-access-app-based-mfa/06.png)

7. Op de **gebruikers en groepen** pagina, voert u de volgende stappen uit:

    ![Gebruikers en groepen](./media/active-directory-conditional-access-app-based-mfa/24.png)

    a. Klik op **gebruikers en groepen selecteren**, en selecteer vervolgens **gebruikers en groepen**.

    b. Klik op **Selecteren**.

    c. Op de **Selecteer** pagina **Isabella Simonsen**, en klik vervolgens op **Selecteer**.

    d. Op de **gebruikers en groepen** pagina, klikt u op **gedaan**.

8. Klik op **Cloud-apps**.

    ![Cloud-apps](./media/active-directory-conditional-access-app-based-mfa/08.png)

9. Op de **Cloud-apps** pagina, voert u de volgende stappen uit:

    ![Selecteer cloud-apps](./media/active-directory-conditional-access-app-based-mfa/26.png)

    a. Klik op **apps selecteren**.

    b. Klik op **Selecteren**.

    c. Op de **Selecteer** pagina **Microsoft Azure Management**, en klik vervolgens op **Selecteer**.

    d. Op de **Cloud-apps** pagina, klikt u op **gedaan**.


10. In de **toegangscontroles** sectie, klikt u op **Grant**.

    ![Besturingselementen voor toegang](./media/active-directory-conditional-access-app-based-mfa/10.png)

11. Op de **Grant** pagina, voert u de volgende stappen uit:

    ![Verlenen](./media/active-directory-conditional-access-app-based-mfa/11.png)

    a. Selecteer **toegang verlenen**.

    a. Selecteer **meervoudige authenticatie**.

    b. Klik op **Selecteren**.

12. In de **beleid inschakelen** sectie, klikt u op **op**.

    ![Beleid inschakelen](./media/active-directory-conditional-access-app-based-mfa/18.png)

13. Klik op **Create**.


## <a name="evaluate-your-conditional-access-policy"></a>Uw beleid voor voorwaardelijke toegang evalueren

Nu dat u uw beleid voor voorwaardelijke toegang hebt geconfigureerd, wilt u waarschijnlijk weet of deze werkt zoals verwacht. Als een eerste stap kunt u de [voorwaardelijke toegang tot wat als hulpprogramma beleid](active-directory-conditional-access-whatif.md) om te simuleren een aanmeldingspagina van een gebruiker. Wanneer u het hulpprogramma met configureert **Isabella Simonsen** als gebruiker en **Microsoft Azure Management** als cloud-app, ziet u het hulpprogramma **MFA vereisen voor toegang tot de Azure portal** onder **Beleidsregels die van toepassing** en **meervoudige authenticatie** als **Grant besturingselementen**.

![Wat gebeurt er als hulpprogramma-beleid](./media/active-directory-conditional-access-app-based-mfa/23.png)



**Evalueren van uw beleid voor voorwaardelijke toegang:**

1. Op de [voorwaardelijke toegang - beleid](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) pagina in het menu bovenaan, klikt u op **wat gebeurt er als**.  
 
    ![What If](./media/active-directory-conditional-access-app-based-mfa/14.png)

2. Klik op **gebruikers**, selecteer **Isabella Simonsen**, en klik vervolgens op **Selecteer**.

    ![Gebruiker](./media/active-directory-conditional-access-app-based-mfa/15.png)

2. Selecteer een cloud-app, moet u de volgende stappen uitvoeren:

    ![Cloud-apps](./media/active-directory-conditional-access-app-based-mfa/16.png)

    a. Klik op **Cloud-apps**.

    b. Op de **pagina Cloud-apps**, klikt u op **apps selecteren**.

    c. Klik op **Selecteren**.

    d. Op de **Selecteer** pagina, selecteert u Microsoft Azure Management ** en klik vervolgens op **Selecteer**.

    e. Klik op de pagina van de cloud-apps op **gedaan**.

3. Klik op **wat gebeurt er als**.


## <a name="test-your-conditional-access-policy"></a>Testen van uw beleid voor voorwaardelijke toegang

De vorige sectie In de vorige sectie u 

Als u wilt testen van uw beleid, willen aanmelden bij uw [Azure-portal](https://portal.azure.com) met behulp van uw **Isabella Simonsen** test-account. U ziet een dialoogvenster dat moet worden ingesteld van uw account voor aanvullende beveiligingsverificatie.

![Multi-Factor Authentication](./media/active-directory-conditional-access-app-based-mfa/22.png)


## <a name="next-steps"></a>Volgende stappen

Als u meer informatie over voorwaardelijke toegang wilt, Zie [voorwaardelijke toegang van Azure Active Directory](active-directory-conditional-access-azure-portal.md).

