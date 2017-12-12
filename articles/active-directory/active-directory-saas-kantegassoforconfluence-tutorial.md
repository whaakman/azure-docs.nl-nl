---
title: 'Zelfstudie: Azure Active Directory-integratie met Kantega SSO voor samenloop | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Kantega SSO voor samenloop.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: d0d99c14-a6ca-45f2-bb84-633126095e7a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: d98aad7d2b4bb26dbd04088ffd133fe96ffe0dd5
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-confluence"></a>Zelfstudie: Azure Active Directory-integratie met Kantega SSO voor samenloop

In deze zelfstudie leert u hoe Kantega SSO voor samenloop integreren met Azure Active Directory (Azure AD).

Kantega SSO voor samenloop integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Kantega SSO voor samenloop heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Kantega SSO voor samenloop (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Kantega SSO voor samenloop, moet u de volgende items:

- Een Azure AD-abonnement
- Een Kantega SSO voor eenmalige aanmelding samenloop abonnement ingeschakeld

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Kantega SSO voor samenloop uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-kantega-sso-for-confluence-from-the-gallery"></a>Kantega SSO voor samenloop uit de galerie toevoegen
Voor het configureren van de integratie van Kantega SSO voor samenloop met Azure AD, moet u Kantega SSO voor samenloop uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Kantega SSO voor samenloop uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Kantega SSO voor samenloop**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_search.png)

5. Selecteer in het deelvenster resultaten **Kantega SSO voor samenloop**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Kantega SSO voor samenloop op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Kantega SSO voor samenloop is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Kantega SSO voor samenloop tot stand worden gebracht.

Wijs in Kantega SSO voor samenloop, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Kantega SSO voor samenloop, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een SSO Kantega voor de testgebruiker samenloop](#creating-a-kantega-sso-for-confluence-test-user)**  - Kantega SSO voor samenloop die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw Kantega SSO voor samenloop toepassing configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Kantega SSO voor samenloop, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Kantega SSO voor samenloop** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_samlbase.png)

3. In **IDP** modus gestart op de **Kantega SSO voor samenloop domein en URL's** sectie de volgende stap uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_url1.png)

    a. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

4. In **SP** geïnitieerd modus selectievakje **weergeven geavanceerde instellingen voor URL** en voer de volgende stap:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_url2.png)

    In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke id, antwoord-URL en aanmeldings-URL. Deze waarden worden ontvangen tijdens de configuratie van de invoegtoepassing samenloop, die verderop in de zelfstudie wordt beschreven.

5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_certificate.png) 

6. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_400.png)
    
7. In een ander browservenster geopend, moet u zich aanmelden bij uw **samenloop beheerportal** als beheerder.

8. Beweeg de muisaanwijzer op het tandwiel en klik op de **invoegtoepassingen**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon1.png)

9. Onder **ATLASSIAN MARKETPLACE** tabblad **vinden van nieuwe invoegtoepassingen**. 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon.png)

10. Search **Kantega SSO voor samenloop SAML Kerberos** en klik op **installeren** knop voor het installeren van de nieuwe SAML-invoegtoepassing.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon2.png)

11. De installatie van de invoegtoepassing wordt gestart.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon3.png)

12. Zodra de installatie voltooid is. Klik op **Sluiten**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon33.png)

13. Klik op **Beheren**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon34.png)
    
14. Klik op **configureren** voor het configureren van de nieuwe invoegtoepassing.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon35.png)

15. Deze nieuwe invoegtoepassing kunt u vinden onder **gebruikers & beveiliging** tabblad.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon36.png)
    
16. In de **SAML** sectie. Selecteer **Azure Active Directory (Azure AD)** van de **toevoegen identiteitsprovider** vervolgkeuzelijst.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon4.png)

17. Abonnement als selecteren **Basic**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon5.png)       

18. Op de **App-eigenschappen** sectie, voert u de volgende stappen: 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon6.png)

    a. Kopieer de **App ID URI** waarde en deze gebruiken als **id, de antwoord-URL en de aanmeldings-URL** op de **Kantega SSO voor samenloop domein en URL's** sectie in Azure-portal.

    b. Klik op **Volgende**.

19. Op de **metagegevens importeren** sectie, voert u de volgende stappen: 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon7.png)

    a. Selecteer **metagegevensbestand op mijn computer**, en de metagegevens-uploadbestand, die u hebt gedownload vanuit Azure-portal.

    b. Klik op **Volgende**.

20. Op de **en SSO** sectie, voert u de volgende stappen:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon8.png)
    
    a. Naam van de identiteitsprovider in toevoegen **identiteit providernaam** textbox (bijvoorbeeld Azure AD).

    b. Klik op **Volgende**.

21. Controleer of het certificaat voor ondertekening en klikt u op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon9.png)

22. Op de **samenloop gebruikersaccounts** sectie, voert u de volgende stappen:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon10.png)

    a. Selecteer **gebruikers in de samenloop interne Directory maken, indien nodig** en voer de juiste naam van de groep voor gebruikers (kan meerdere Nee. van groepen van elkaar gescheiden door komma's).

    b. Klik op **Volgende**.

23. Klik op **Voltooien**.   

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon11.png)

24. Op de **bekend domeinen voor Azure AD** sectie, voert u de volgende stappen: 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon12.png)

    a. Selecteer **domeinen bekend** in het linkerpaneel van de pagina.

    b. Geef de domeinnaam in de **domeinen bekend** textbox.

    c. Klik op **Opslaan**. 

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-kantegassoforconfluence-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-kantegassoforconfluence-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-kantegassoforconfluence-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-kantegassoforconfluence-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-kantega-sso-for-confluence-test-user"></a>Maken van een SSO Kantega voor samenloop testgebruiker

Om Azure AD-gebruikers zich aanmelden bij samenloop, moeten ze worden ingericht in samenloop. In het geval van Kantega SSO voor samenloop is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Aanmelden bij uw Kantega SSO voor samenloop bedrijf site als beheerder.

2. Beweeg de muisaanwijzer op het tandwiel en klik op de **Gebruikersbeheer**.

    ![Werknemer toevoegen](./media/active-directory-saas-kantegassoforconfluence-tutorial/user1.png) 

3. Klik onder de sectie gebruikers op **gebruikers toevoegen** tabblad. Op de **'Gebruiker toevoegen'** dialoogvenster pagina, voert u de volgende stappen uit:

    ![Werknemer toevoegen](./media/active-directory-saas-kantegassoforconfluence-tutorial/user2.png) 

    a. In de **gebruikersnaam** textbox, typ het e-mailadres van gebruiker, zoals Brittasimon@contoso.com.

    b. In de **volledige naam** textbox, typ de volledige naam van gebruiker zoals Britta Simon.

    c. In de **e** textbox, typ het e-mailadres van gebruiker, zoals Brittasimon@contoso.com.

    d. In de **wachtwoord** textbox, typt u het wachtwoord voor de gebruiker.

    e. Klik op **wachtwoord bevestigen** het wachtwoord opnieuw invoeren.
    
    f. Klik op **toevoegen** knop.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Kantega SSO voor samenloop.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Kantega SSO voor samenloop, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Kantega SSO voor samenloop**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de Kantega SSO samenloop tegel in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw Kantega SSO voor samenloop toepassing.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_203.png

