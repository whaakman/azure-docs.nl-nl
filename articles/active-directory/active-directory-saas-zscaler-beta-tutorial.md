---
title: 'Zelfstudie: Azure Active Directory-integratie met Zscaler Beta | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Zscaler Beta.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 56b846ae-a1e7-45ae-a79d-992a87f075ba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 01e76ba6e89fd04fb48e7eb2a3965b2928ba72ff
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-beta"></a>Zelfstudie: Azure Active Directory-integratie met Zscaler Beta

In deze zelfstudie leert u hoe Zscaler Beta integreren met Azure Active Directory (Azure AD).

Zscaler Beta integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Zscaler Beta heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Zscaler Beta (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Zscaler Bèta, moet u de volgende items:

- Een Azure AD-abonnement
- Een Zscaler Beta eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand hier downloaden: [proefversie aanbieding](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Zscaler Beta uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-zscaler-beta-from-the-gallery"></a>Zscaler Beta uit de galerie toevoegen
Voor het configureren van de integratie van Zscaler Beta in Azure AD, moet u Zscaler Beta uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Zscaler Beta uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Zscaler Beta**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_search.png)

5. Selecteer in het deelvenster resultaten **Zscaler Beta**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test eenmalige aanmelding Azure AD met Zscaler Beta op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Zscaler Beta is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker Zscaler bètaversie tot stand worden gebracht.

Wijs in Zscaler Bèta, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Zscaler Beta, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Proxy-instellingen configureren](#configuring-proxy-settings)**  : als u wilt de proxy-instellingen in Internet Explorer configureren
3. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
4. **[Maken van een testgebruiker Zscaler Beta](#creating-a-zscaler-beta-test-user)**  - Zscaler Beta die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
5. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
6. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing Zscaler Beta.

**Voor het configureren van Azure AD eenmalige aanmelding met Zscaler Bèta, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Zscaler Beta** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_samlbase.png)

3. Op de **Zscaler Beta domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_url.png)

    Typ in het tekstvak aanmeldings-URL de URL die wordt gebruikt door uw gebruikers aan te melden bij uw toepassing Zscaler Beta.

    > [!NOTE] 
    > U moet deze waarde bijwerken met de werkelijke URL voor eenmalige aanmelding. Neem contact op met [Zscaler bèta-Client-ondersteuningsteam](https://www.zscaler.com/company/contact) deze waarde op te halen. 

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_400.png)

6. Op de **Zscaler Beta configuratie** sectie, klikt u op **configureren Zscaler Beta** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_configure.png) 

7. In een ander browservenster, meld u aan bij uw bedrijf Zscaler Beta site als beheerder.

8. Klik in het menu bovenaan op **beheer**.
   
    ![Beheer](./media/active-directory-saas-zscaler-beta-tutorial/ic800206.png "beheer")

9. Onder **beheerders beheren en rollen**, klikt u op **gebruikers beheren & verificatie**.   
            
    ![Gebruikers & verificatie beheren](./media/active-directory-saas-zscaler-beta-tutorial/ic800207.png "gebruikers & verificatie beheren")

10. In de **verificatie-opties kiezen voor uw organisatie** sectie, voert u de volgende stappen uit:   
                
    ![Verificatie](./media/active-directory-saas-zscaler-beta-tutorial/ic800208.png "verificatie")
   
    a. Selecteer **verificatie met eenmalige aanmelding SAML**.

    b. Klik op **één SAML aanmelding Parameters configureren**.

11. Op de **configureren SAML Single Sign-On Parameters** dialoogvenster pagina de volgende stappen uit en klik vervolgens op **gedaan**

    ![Eenmalige aanmelding](./media/active-directory-saas-zscaler-beta-tutorial/ic800209.png "eenmalige aanmelding")
    
    a. Plak de **SAML Single Sign-On Service-URL** waarde, die u hebt gekopieerd vanuit de Azure-portal in de **URL van de SAML-Portal waarmee gebruikers worden verzonden voor verificatie** textbox.
    
    b. In de **kenmerk met naam van de aanmelding** textbox type **NameID**.
    
    c. Als u wilt uw gedownloade certificaat uploaden, klikt u op **Zscaler pem**.
    
    d. Selecteer **SAML automatische inrichting inschakelen**.

12. Op de **gebruikersverificatie configureren** dialoogvenster pagina, voert u de volgende stappen uit:

    ![Beheer](./media/active-directory-saas-zscaler-beta-tutorial/ic800210.png "beheer")
    
    a. Klik op **Opslaan**.

    b. Klik op **nu activeren**.

## <a name="configuring-proxy-settings"></a>Proxy-instellingen configureren
### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a>De proxy-instellingen in Internet Explorer configureren

1. Start **Internet Explorer**.

2. Selecteer **Internetopties** van de **extra** menu voor open de **Internetopties** dialoogvenster.   
    
     ![Internetopties](./media/active-directory-saas-zscaler-beta-tutorial/ic769492.png "Internetopties")

3. Klik op de **verbindingen** tabblad.   
  
     ![Verbindingen](./media/active-directory-saas-zscaler-beta-tutorial/ic769493.png "verbindingen")

4. Klik op **LAN-instellingen** openen de **LAN-instellingen** dialoogvenster.

5. Voer de volgende stappen uit in de sectie Proxy-server:   
   
    ![Proxyserver](./media/active-directory-saas-zscaler-beta-tutorial/ic769494.png "proxyserver")

    a. Selecteer **een proxyserver gebruiken voor uw LAN**.

    b. Typ in het tekstvak adres **gateway.zscalerbeta.net**.

    c. Typ in het tekstvak poort **80**.

    d. Selecteer **proxyserver niet gebruiken voor lokale adressen**.

    e. Klik op **OK** sluiten de **Local Area Network (LAN)-instellingen** dialoogvenster.

6. Klik op **OK** sluiten de **Internetopties** dialoogvenster.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-zscaler-beta-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-zscaler-beta-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-zscaler-beta-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-zscaler-beta-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-zscaler-beta-test-user"></a>Maken van een testgebruiker Zscaler Beta

Om Azure AD-gebruikers zich aanmelden bij Zscaler Bèta, moeten ze worden ingericht op Zscaler Beta. In het geval van Zscaler bèta is inrichting een handmatige taak.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren voor gebruikers inrichten, moet u de volgende stappen uitvoeren:

1. Meld u aan bij uw **Zscaler Beta** tenant.

2. Klik op **beheer**.   
   
    ![Beheer](./media/active-directory-saas-zscaler-beta-tutorial/ic781035.png "beheer")

3. Klik op **Gebruikersbeheer**.   
        
     ![Voeg](./media/active-directory-saas-zscaler-beta-tutorial/ic781036.png "toevoegen")

4. In de **gebruikers** tabblad **toevoegen**.
      
    ![Voeg](./media/active-directory-saas-zscaler-beta-tutorial/ic781037.png "toevoegen")

5. Voer de volgende stappen uit in de sectie gebruiker toevoegen:
        
    ![Gebruiker toevoegen](./media/active-directory-saas-zscaler-beta-tutorial/ic781038.png "gebruiker toevoegen")
   
    a. Typ de **UserID**, **weergavenaam gebruiker**, **wachtwoord**, **wachtwoord bevestigen**, en selecteer vervolgens **groepen** en de **afdeling** van een geldig Azure AD-account die u inrichten wilt.

    b. Klik op **Opslaan**.

> [!NOTE]
> U kunt geen andere hulpprogramma's voor Zscaler Beta gebruiker-account maken of geleverd door Zscaler Beta-API's voor het inrichten van Azure AD-gebruikersaccounts gebruiken.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Zscaler Beta.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Zscaler Bèta, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Zscaler Beta**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Zscaler Beta in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Zscaler Beta.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_203.png

