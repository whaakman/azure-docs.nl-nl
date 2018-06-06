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
ms.component: protection
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/11/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 1501ca1c036a8db1d53b9b27170d9ae05d41f797
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34724112"
---
# <a name="quickstart-require-mfa-for-specific-apps-with-azure-active-directory-conditional-access"></a>Snelstartgids: MFA vereisen voor specifieke apps met voorwaardelijke toegang van Azure Active Directory 

Om te vereenvoudigen de aanmeldingservaring van uw gebruikers, is het raadzaam zodat ze aan te melden bij uw cloud-apps met een gebruikersnaam en een wachtwoord. Veel omgevingen hebben echter ten minste enkele apps waarvoor u wordt aangeraden om te vereisen een sterkere vorm van accountverificatie,, zoals multi-factor authentication (MFA). Dit kan zijn, voor de voorbeeld-waar, voor toegang tot e-mailsysteem van uw organisatie of uw HR-apps. In Azure Active Directory (Azure AD), u kunt dit te bereiken met een beleid voor voorwaardelijke toegang.    

Deze snelstartgids toont hoe u configureert een [beleid voor voorwaardelijke toegang van Azure AD](active-directory-conditional-access-azure-portal.md) waarvoor multi-factor authentication-server is vereist voor een geselecteerde cloud-app in uw omgeving.

![Beleid maken](./media/active-directory-conditional-access-app-based-mfa/32.png)


Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.



## <a name="prerequisites"></a>Vereisten 

Voor het voltooien van het scenario in deze snelstartgids hebt u het volgende nodig:

- **Toegang tot een Azure AD Premium-editie** -voorwaardelijke toegang van Azure AD is een Azure AD Premium-functie. 

- **Een testaccount genoemd Isabella Simonsen** : als u niet hoe een testaccount maken weet, raadpleeg dan [cloud-gebaseerde gebruikers toevoegen](add-users-azure-active-directory.md#add-cloud-based-users).



## <a name="create-your-conditional-access-policy"></a>Maken van uw beleid voor voorwaardelijke toegang 

Deze sectie toont het maken van beleid voor voorwaardelijke toegang vereist. Maakt gebruik van het scenario in deze snelstartgids:

- De Azure-portal als tijdelijke aanduiding voor een cloud-app die MFA is vereist. 
- Uw voorbeeldgebruiker voor het testen van beleid voor voorwaardelijke toegang.  

In uw beleid instellen:

|Instelling |Waarde|
|---     | --- |
|Gebruikers en groepen | Isabella Simonsen |
|Cloud-apps | Beheer van Microsoft Azure |
|Toegang verlenen | Meervoudige verificatie vereisen |
 

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


## <a name="evaluate-a-simulated-sign-in"></a>Een gesimuleerde aanmelden evalueren

Nu dat u uw beleid voor voorwaardelijke toegang hebt geconfigureerd, wilt u waarschijnlijk weet of deze werkt zoals verwacht. Gebruik als een eerste stap de voorwaardelijke toegang wat gebeurt er als beleid hulpprogramma om te simuleren een aanmeldingspagina van uw testgebruiker. De simulatie maakt een schatting van de impact dit aanmelden heeft op uw beleid en een simulatierapport genereert.  

Initialisatie van de wat als hulpprogramma voor het evalueren van beleid is ingesteld:

- **Isabella Simonsen** als gebruiker 
- **Microsoft Azure Management** als cloud-app

 Te klikken op **wat gebeurt er als** maakt u een simulatierapport:

- **MFA vereisen voor toegang tot de Azure portal** onder **beleidsregels die van toepassing** 
- **Meervoudige authenticatie** als **Grant besturingselementen**.

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

In de vorige sectie hebt u geleerd hoe een gesimuleerde aanmelden evalueren. Naast een simulatie, moet u ook testen van uw beleid voor voorwaardelijke toegang om ervoor te zorgen dat deze werkt zoals verwacht. 

Als u wilt testen van uw beleid, willen aanmelden bij uw [Azure-portal](https://portal.azure.com) met behulp van uw **Isabella Simonsen** test-account. U ziet een dialoogvenster dat moet worden ingesteld van uw account voor aanvullende beveiligingsverificatie.

![Multi-Factor Authentication](./media/active-directory-conditional-access-app-based-mfa/22.png)


## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, verwijder de testgebruiker en beleid voor voorwaardelijke toegang:

- Als u het verwijderen van een Azure AD-gebruiker niet weet, Zie [gebruikers verwijderen van Azure AD](add-users-azure-active-directory.md#delete-users-from-azure-ad).

- Selecteer uw beleid voor het verwijderen van uw beleid en klik vervolgens op **verwijderen** in de werkbalk Snelle toegang.

    ![Multi-Factor Authentication](./media/active-directory-conditional-access-app-based-mfa/33.png)


## <a name="next-steps"></a>Volgende stappen

Als u meer informatie over voorwaardelijke toegang wilt, Zie [voorwaardelijke toegang van Azure Active Directory](active-directory-conditional-access-azure-portal.md).

