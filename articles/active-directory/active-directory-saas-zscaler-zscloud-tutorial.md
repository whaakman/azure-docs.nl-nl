---
title: 'Zelfstudie: Azure Active Directory-integratie met Zscaler ZSCloud | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Zscaler ZSCloud.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 411d5684-a780-410a-9383-59f92cf569b5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: 44fa15a3057975617116a10f044ddba2298feba2
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-zscloud"></a>Zelfstudie: Azure Active Directory-integratie met Zscaler ZSCloud

In deze zelfstudie leert u hoe Zscaler ZSCloud integreren met Azure Active Directory (Azure AD).

Zscaler ZSCloud integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Zscaler ZSCloud heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Zscaler ZSCloud (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Zscaler ZSCloud, moet u de volgende items:

- Een Azure AD-abonnement
- Een Zscaler ZSCloud eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Zscaler ZSCloud uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-zscaler-zscloud-from-the-gallery"></a>Zscaler ZSCloud uit de galerie toevoegen
Voor het configureren van de integratie van Zscaler ZSCloud in Azure AD, moet u Zscaler ZSCloud uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Zscaler ZSCloud uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Zscaler ZSCloud**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_search.png)

5. Selecteer in het deelvenster resultaten **Zscaler ZSCloud**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en test eenmalige aanmelding Azure AD met Zscaler ZSCloud op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Zscaler ZSCloud is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Zscaler ZSCloud tot stand worden gebracht.

Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Zscaler ZSCloud.

Om te configureren en testen van Azure AD eenmalige aanmelding met Zscaler ZSCloud, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Proxy-instellingen configureren](#configuring-proxy-settings)**  : als u wilt de proxy-instellingen in Internet Explorer configureren
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Zscaler ZSCloud](#creating-a-zscaler-zscloud-test-user)**  - Zscaler ZSCloud die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Zscaler ZSCloud configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Zscaler ZSCloud, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Zscaler ZSCloud** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_samlbase.png)

3. Op de **Zscaler ZSCloud domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_url.png)

     In de **aanmeldings-URL** textbox, typ de URL moet worden gebruikt door uw gebruikers aan te melden bij uw toepassing ZScaler ZSCloud.
    
    > [!NOTE] 
    > U moet deze waarde bijwerken met de werkelijke URL voor eenmalige aanmelding. Neem contact op met [Zscaler ZSCloud Client ondersteuningsteam](https://support.zscaler.com/hc/articles/210172606-Zscaler-is-Expanding-Its-Zscloud-Global-Footprint) deze waarde op te halen. 
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_400.png)

6. Op de **Zscaler ZSCloud configuratie** sectie, klikt u op **configureren Zscaler ZSCloud** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_configure.png) 

7. In een ander browservenster, meld u aan bij uw bedrijf ZScaler ZSCloud site als beheerder.

8. Klik in het menu bovenaan op **beheer**.
   
    ![Beheer](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800206.png "beheer")

9. Onder **beheerders beheren en rollen**, klikt u op **gebruikers beheren & verificatie**.   
            
    ![Gebruikers & verificatie beheren](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800207.png "gebruikers & verificatie beheren")

10. In de **verificatie-opties kiezen voor uw organisatie** sectie, voert u de volgende stappen uit:   
                
    ![Verificatie](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800208.png "verificatie")
   
    a. Selecteer **verificatie met eenmalige aanmelding SAML**.

    b. Klik op **één SAML aanmelding Parameters configureren**.

11. Op de **configureren SAML Single Sign-On Parameters** dialoogvenster pagina de volgende stappen uit en klik vervolgens op **gedaan**

    ![Eenmalige aanmelding](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800209.png "eenmalige aanmelding")
    
    a. Plak de **SAML Single Sign-On Service-URL** waarde in de **URL van de SAML-Portal waarmee gebruikers worden verzonden voor verificatie** textbox.
    
    b. In de **kenmerk met naam van de aanmelding** textbox type **NameID**.
    
    c. Als u wilt uw gedownloade certificaat uploaden, klikt u op **Zscaler pem**.
    
    d. Selecteer **SAML automatische inrichting inschakelen**.

12. Op de **gebruikersverificatie configureren** dialoogvenster pagina, voert u de volgende stappen uit:

    ![Beheer](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800210.png "beheer")
    
    a. Klik op **Opslaan**.

    b. Klik op **nu activeren**.

## <a name="configuring-proxy-settings"></a>Proxy-instellingen configureren
### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a>De proxy-instellingen in Internet Explorer configureren

1. Start **Internet Explorer**.

2. Selecteer **Internetopties** van de **extra** menu voor open de **Internetopties** dialoogvenster.   
    
     ![Internetopties](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769492.png "Internetopties")

3. Klik op de **verbindingen** tabblad.   
  
     ![Verbindingen](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769493.png "verbindingen")

4. Klik op **LAN-instellingen** openen de **LAN-instellingen** dialoogvenster.

5. Voer de volgende stappen uit in de sectie Proxy-server:   
   
    ![Proxyserver](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769494.png "proxyserver")

    a. Selecteer **een proxyserver gebruiken voor uw LAN**.

    b. Typ in het tekstvak adres **gateway.zscalerone.net**.

    c. Typ in het tekstvak poort **80**.

    d. Selecteer **proxyserver niet gebruiken voor lokale adressen**.

    e. Klik op **OK** sluiten de **Local Area Network (LAN)-instellingen** dialoogvenster.

6. Klik op **OK** sluiten de **Internetopties** dialoogvenster.

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-zscaler-zscloud-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-zscaler-zscloud-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-zscaler-zscloud-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-zscaler-zscloud-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.

### <a name="creating-a-zscaler-zscloud-test-user"></a>Een testgebruiker Zscaler ZSCloud maken

Om Azure AD-gebruikers zich aanmelden bij ZScaler ZSCloud, moeten ze worden ingericht op ZScaler ZSCloud.  
In het geval van ZScaler ZSCloud is inrichting een handmatige taak.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren voor gebruikers inrichten, moet u de volgende stappen uitvoeren:

1. Meld u aan bij uw **Zscaler** tenant.

2. Klik op **beheer**.   
   
    ![Beheer](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781035.png "beheer")

3. Klik op **Gebruikersbeheer**.   
        
     ![Voeg](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781037.png "toevoegen")

4. In de **gebruikers** tabblad **toevoegen**.
      
    ![Voeg](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781037.png "toevoegen")

5. Voer de volgende stappen uit in de sectie gebruiker toevoegen:
        
    ![Gebruiker toevoegen](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781038.png "gebruiker toevoegen")
   
    a. Typ de **UserID**, **weergavenaam gebruiker**, **wachtwoord**, **wachtwoord bevestigen**, en selecteer vervolgens **groepen**en de **afdeling** van een geldige AAD-account dat u inrichten wilt.

    b. Klik op **Opslaan**.

> [!NOTE]
> U kunt andere ZScaler ZSCloud gebruiker account hulpmiddelen voor het maken of API's die is geleverd door ZScaler ZSCloud aan inrichten AAD-gebruikersaccounts.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Zscaler ZSCloud.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Zscaler ZSCloud, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Zscaler ZSCloud**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het toegangsvenster.

Als u op de tegel Zscaler ZSCloud in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Zscaler ZSCloud.

Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_203.png

