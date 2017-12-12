---
title: 'Zelfstudie: Azure Active Directory-integratie met Symantec Web Security Service (WSS) | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Symantec Web Security Service (WSS).
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: d6e4d893-1f14-4522-ac20-0c73b18c72a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 35427941a084e6750d66ccd3e135d7eef1767c6c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-symantec-web-security-service-wss"></a>Zelfstudie: Azure Active Directory-integratie met Symantec Web Security Service (WSS)

In deze zelfstudie leert u het integreren van uw account Symantec Web Security Service (WSS) aan uw account voor Azure Active Directory (Azure AD), zodat WSS kunnen verifiëren van een eindgebruiker ingericht in de Azure AD dat gebruikmaakt van SAML-verificatie en afdwingen van de gebruiker of groep niveau beleidsregels.

Symantec Web Security Service (WSS) integreren met Azure AD biedt de volgende voordelen:

- Alle gebruikers en groepen die worden gebruikt door uw WSS-account van uw Azure AD-portal beheren. 

- De gebruikers om zich te authentiseren in WSS met hun Azure AD-referenties toestaan.

- Schakel het afdwingen van de gebruiker en groep niveau beleidsregels die zijn gedefinieerd in uw WSS-account.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Symantec Web Security Service (WSS), moet u de volgende items:

- Een Azure AD-abonnement
- Een account Symantec Web Security Service (WSS)

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met een WSS-account dat momenteel wordt gebruikt voor productie-doel.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw WSS-account dat momenteel wordt gebruikt voor productie-doel voor deze test als dat nodig is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie configureert u uw Azure AD om in te schakelen eenmalige aanmelding voor WSS met behulp van de referenties van de gebruiker gedefinieerd in uw Azure AD-account.
Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. De app Symantec Web Security Service (WSS) toevoegen uit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-symantec-web-security-service-wss-from-the-gallery"></a>Symantec Web Security Service (WSS) uit de galerie toevoegen
Voor het configureren van de integratie van Symantec Web Security Service (WSS) in Azure AD, moet u Symantec Web Security Service (WSS) uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Symantec Web Security Service (WSS) uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Symantec Web Security Service (WSS)**, selecteer **Symantec Web Security Service (WSS)** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen de de toepassing.

    ![Symantec Web Security Service (WSS) in de lijst met resultaten](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met Symantec Web Security Service (WSS) hebt op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Symantec Web Security Service (WSS) is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Symantec Web Security Service (WSS) tot stand worden gebracht.

In Symantec Web Security Service (WSS), wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Symantec Web Security Service (WSS), moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Symantec Web Security Service (WSS)](#create-a-symantec-web-security-service-wss-test-user)**  - hebben een equivalent van Britta Simon in Symantec Web Security Service (WSS) die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing Symantec Web Security Service (WSS).

**Voor het configureren van Azure AD eenmalige aanmelding met Symantec Web Security Service (WSS), moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Symantec Web Security Service (WSS)** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_samlbase.png)

3. Op de **Symantec Web Service (WSS) beveiligingsdomein en URL's** sectie, voert u de volgende stappen uit:

    ![Symantec Web Security Service (WSS)-domein en de URL's van eenmalige aanmelding informatie](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_url.png)

    a. In de **id** textbox, typ de URL:`https://saml.threatpulse.net:8443/saml/saml_realm`

    b. In de **antwoord-URL** textbox, typ de URL:`https://saml.threatpulse.net:8443/saml/saml_realm/bcsamlpost`

    > [!NOTE]
    > Neem contact op met de [Symantec Web Security Service (WSS) Client ondersteuningsteam](https://www.symantec.com/contact-us) als de waarden voor de **id** en **antwoord-URL** zijn voor een bepaalde reden niet werkt.

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_certificate.png) 

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-symantec-tutorial/tutorial_general_400.png)
    
6. Raadpleeg de onlinedocumentatie WSS voor het configureren van eenmalige aanmelding aan de kant van Symantec Web Security Service (WSS). De gedownloade **Metadata XML** bestand moet worden geïmporteerd in de WSS-portal. Neem contact op met de [Symantec Web Security Service (WSS) ondersteuningsteam](https://www.symantec.com/contact-us) als u hulp nodig hebt bij de configuratie op de WSS-portal.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-symantec-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-symantec-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-symantec-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-symantec-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-symantec-web-security-service-wss-test-user"></a>Maak een testgebruiker Symantec Web Security Service (WSS)

In deze sectie maakt u een gebruiker met de naam Britta Simon in Symantec Web Security Service (WSS). De bijbehorende end gebruikersnaam kan handmatig worden gemaakt in de WSS-portal of u kunt wachten op de gebruikers/groepen ingericht in de Azure AD moeten worden gesynchroniseerd naar de portal WSS na een paar minuten (~ 15 minuten). Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken. Het openbare IP-adres van de eindgebruiker-machine die wordt gebruikt om te bladeren websites moet ook worden ingericht in de portal Symantec Web Security Service (WSS).

> [!NOTE]
> Neem [Klik hier](http://www.bing.com/search?q=my+ip+address&qs=AS&pq=my+ip+a&sc=8-7&cvid=29A720C95C78488CA3F9A6BA0B3F98C5&FORM=QBLH&sp=1) ophalen van de computer het openbare IP-adres.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding toegang verleent aan Symantec Web Security Service (WSS) gebruiken.

![Toewijzen van de gebruikersrol][200] 

**Als u wilt toewijzen Britta Simon naar Symantec Web Security Service (WSS), moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Symantec Web Security Service (WSS)**.

    ![De koppeling Symantec Web Security Service (WSS) in de lijst met toepassingen](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie test u de functionaliteit voor eenmalige aanmelding nu dat u uw WSS-account voor het gebruik van uw Azure AD voor SAML-verificatie hebt geconfigureerd.

Nadat u hebt uw webbrowser om proxy het verkeer naar WSS, geconfigureerd wanneer u uw webbrowser Open en probeert om te bladeren naar een site en vervolgens wordt u omgeleid naar de pagina voor Azure. Geef de referenties van de eindgebruiker test die is ingericht in de Azure AD (dat wil zeggen, BrittaSimon) en het wachtwoord dat is gekoppeld. Eenmaal is geverifieerd, kunt u zult kunnen bladeren naar de website die u hebt gekozen. U moet een beleidsregel maken aan de kant WSS BrittaSimon van bladeren naar een bepaalde site, en vervolgens u de pagina WSS-blok ziet wanneer u probeert om te bladeren naar die site als gebruiker BrittaSimon blokkeren.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_203.png

