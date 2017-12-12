---
title: 'Zelfstudie: Azure Active Directory-integratie met TINFOIL SECURITY | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en TINFOIL SECURITY.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: da02da92-e3b0-4c09-ad6c-180882b0f9f8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 7794564396c2ade8ba45b9f2b1238f304f6b3921
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tinfoil-security"></a>Zelfstudie: Azure Active Directory-integratie met TINFOIL SECURITY

In deze zelfstudie leert u hoe TINFOIL SECURITY integreren met Azure Active Directory (Azure AD).

TINFOIL SECURITY integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot TINFOIL SECURITY heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij TINFOIL SECURITY (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met TINFOIL SECURITY, moet u de volgende items:

- Een Azure AD-abonnement
- Een TINFOIL SECURITY eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. TINFOIL SECURITY uit de galerie toevoegen
2. Configureren en testen eenmalige aanmelding Azure AD

## <a name="add-tinfoil-security-from-the-gallery"></a>TINFOIL SECURITY uit de galerie toevoegen
Voor het configureren van de integratie van TINFOIL SECURITY in Azure AD, moet u TINFOIL SECURITY uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen TINFOIL SECURITY uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **TINFOIL SECURITY**, selecteer **TINFOIL SECURITY** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![TINFOIL SECURITY vanuit galerie](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD
In deze sectie configureert en test eenmalige aanmelding Azure AD met TINFOIL SECURITY op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in TINFOIL SECURITY is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de verwante TINFOIL SECURITY-gebruiker worden gemaakt.

Wijs in TINFOIL SECURITY de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met TINFOIL SECURITY, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker TINFOIL SECURITY](#create-a-tinfoil-security-test-user)**  - TINFOIL SECURITY die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In deze sectie maakt u Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing TINFOIL SECURITY.

**Voor het configureren van Azure AD eenmalige aanmelding met TINFOIL SECURITY, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **TINFOIL SECURITY** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![SAML op basis van eenmalige aanmelding](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_samlbase.png)

3. Op de **TINFOIL SECURITY domein en de URL's** sectie, de gebruiker heeft geen alle stappen uitvoeren als de app al vooraf is geïntegreerd met Azure.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_url.png)


4. Op de **SAML-certificaat voor ondertekening van** sectie, Kopieer de **VINGERAFDRUK** waarde.

    ![Certificaat voor ondertekening van SAML-sectie](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_certificate.png) 

5. Als u wilt de vereiste kenmerktoewijzingen toevoegen, moet u de volgende stappen uitvoeren:
    
    ![Kenmerken](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_attribute1.png "kenmerken")
    
    | Kenmerknaam    |   Waarde kenmerk |
    | ------------------- | -------------------- |
    | AccountId | UXXXXXXXXXXXXX |
    
    a. Klik op **gebruikerskenmerk toevoegen**.
    
    ![Kenmerk toevoegen](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_attribute.png "kenmerken")
    
    ![Kenmerk toevoegen](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_addatt.png "kenmerken")
    
    b. In de **kenmerknaam** textbox type **accountid**.
    
    c. In de **kenmerkwaarde** textbox, plak de account-ID-waarde die u later in de zelfstudie krijgt.
    
    d. Klik op **OK**.    

6. Klik op **opslaan** knop.

    ![knop Opslaan](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_400.png)

7. Op de **TINFOIL SECURITY Configuration** sectie, klikt u op **TINFOIL SECURITY configureren** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![TINFOIL SECURITY Configuration](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_configure.png) 

8. In een ander browservenster, meld u bij uw bedrijf TINFOIL SECURITY site als beheerder.

9. Klik in de werkbalk bovenaan op **Mijn Account**.
   
    ![Dashboard](./media/active-directory-saas-tinfoil-security-tutorial/ic798971.png "Dashboard")

10. Klik op **beveiliging**.
   
    ![Beveiliging](./media/active-directory-saas-tinfoil-security-tutorial/ic798972.png "beveiliging")

11. Op de **Single Sign-On** configuratie pagina, voert u de volgende stappen uit:
   
    ![Eenmalige aanmelding](./media/active-directory-saas-tinfoil-security-tutorial/ic798973.png "eenmalige aanmelding")
   
    a. Selecteer **SAML inschakelen**.
   
    b. Klik op **handmatige configuratie**.
   
    c. In **SAML Post URL** textbox, plak de waarde van **SAML Single Sign-On Service-URL** die u hebt gekopieerd vanuit Azure-portal
   
    d. In **SAML certificaat vingerafdruk** textbox, plak de waarde van **vingerafdruk** die u hebt gekopieerd uit **certificaat voor ondertekening van SAML** sectie.
  
    e. Kopiëren **uw Account-ID** waarde en plak de waarde in **kenmerkwaarde** textbox onder **kenmerk toevoegen** sectie in Azure-portal.
   
    f. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Gebruikers en groepen -> alle gebruikers ](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Gebruiker](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="create-a-tinfoil-security-test-user"></a>Een testgebruiker TINFOIL SECURITY maken

Om in te schakelen gebruikers van Azure AD aan te melden bij TINFOIL SECURITY, moeten ze worden ingericht in TINFOIL SECURITY. In het geval van TINFOIL SECURITY is inrichting een handmatige taak.

**Als u een gebruiker die zijn ingericht, kunt u de volgende stappen uitvoeren:**

1. Als de gebruiker deel van een Enterprise-account uitmaakt, moet u [Neem contact op met het ondersteuningsteam TINFOIL SECURITY](https://www.tinfoilsecurity.com/contact) ophalen van het gebruikersaccount dat is gemaakt.

2. Als de gebruiker een gewone TINFOIL SECURITY SaaS-gebruiker is, kunnen de gebruiker kan een samenwerker toevoegen aan een van de sites van de gebruiker. Dit activeert een proces voor het verzenden van een uitnodiging voor het opgegeven e-mailbericht voor het maken van een nieuw TINFOIL SECURITY-gebruikersaccount.

> [!NOTE]
> U kunt geen andere hulpprogramma's van TINFOIL SECURITY gebruiker-account maken of API's die worden geleverd door TINFOIL SECURITY voor het inrichten van Azure AD-gebruikersaccounts gebruiken.
> 
> 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan TINFOIL SECURITY.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen TINFOIL SECURITY, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **TINFOIL SECURITY**.

    ![TINFOIL SECURITY selecteren](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel TINFOIL SECURITY in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing TINFOIL SECURITY. Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_203.png

