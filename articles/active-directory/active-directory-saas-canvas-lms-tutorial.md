---
title: 'Zelfstudie: Azure Active Directory-integratie met Canvas Lms | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Canvas LMS.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: bfed291c-a33e-410d-b919-5b965a631d45
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: jeedes
ms.openlocfilehash: ae007ee34f1dc997db2d7ca8267ba2522e0eb687
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-canvas-lms"></a>Zelfstudie: Azure Active Directory-integratie met Canvas LMS

In deze zelfstudie leert u het Canvas integreren met Azure Active Directory (Azure AD).

Canvas integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot het Canvas heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Canvas (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Canvas, moet u de volgende items:

- Een Azure AD-abonnement
- Een Canvas eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Canvas uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-canvas-from-the-gallery"></a>Canvas uit de galerie toevoegen
Voor het configureren van de integratie van Canvas in Azure AD, moet u Canvas uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Canvas uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Canvas**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_search.png)

5. Selecteer in het deelvenster resultaten **Canvas**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met Canvas op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Canvas is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in het Canvas tot stand worden gebracht.

Wijs in het Canvas de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Canvas, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Canvas](#creating-a-canvas-test-user)**  - Canvas die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing Canvas.

**Voor het configureren van Azure AD eenmalige aanmelding met Canvas, kunt u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Canvas** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_samlbase.png)

3. Op de **Canvas domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<tenant-name>.instructure.com`

    b. In de **id** textbox, typ de waarde met het volgende patroon volgen:`https://<tenant-name>.instructure.com/saml2`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met het werkelijke aanmeldings-URL en de id. Neem contact op met [Canvas Client ondersteuningsteam](https://community.canvaslms.com/community/help) ophalen van deze waarden. 
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, Kopieer de **VINGERAFDRUK** waarde van het certificaat.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_400.png)

6. Op de **configuratie tekenpapier** sectie, klikt u op **Canvas configureren** openen **eenmalige aanmelding configureren** venster. Kopieer de **URL van wijzigen wachtwoord, Sign-Out URL SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_configure.png) 
 
7. In een ander browservenster, meld u aan bij uw bedrijf Canvas site als beheerder.

8. Ga naar **cursussen \> beheerde Accounts \> Microsoft**.
   
    ![Canvas](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Canvas")

9. Selecteer in het navigatievenster aan de linkerkant **verificatie**, en klik vervolgens op **nieuwe SAML-configuratie toevoegen**.
   
    ![Verificatie](./media/active-directory-saas-canvas-lms-tutorial/IC775991.png "verificatie")

10. Voer de volgende stappen uit op de pagina huidige integratie:
   
    ![Huidige integratie](./media/active-directory-saas-canvas-lms-tutorial/IC775992.png "huidige integratie")

    a. In **IdP entiteit-ID** textbox, plak de waarde van **SAML entiteit-ID** die u hebt gekopieerd vanuit Azure-portal.

    b. In **logboek URL** textbox, plak de waarde van **SAML Single Sign-On Service-URL** die u hebt gekopieerd vanuit Azure-portal.

    c. In **afmeldings-URL** textbox, plak de waarde van **Sign-Out URL** die u hebt gekopieerd vanuit Azure-portal.

    d. In **koppeling van wijzigen wachtwoord** textbox, plak de waarde van **URL van wijzigen wachtwoord** die u hebt gekopieerd vanuit Azure-portal. 

    e. In **vingerafdruk van certificaat** textbox, plak de **vingerafdruk** waarde van het certificaat dat u hebt gekopieerd vanuit Azure-portal.      
        
    f. Van de **aanmelding kenmerk** selecteert **NameID**.

    g. Van de **id indeling** selecteert **emailAddress**.

    h. Klik op **verificatie-instellingen opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-canvas-lms-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-canvas-lms-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-canvas-lms-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-canvas-lms-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-canvas-test-user"></a>Maken van een testgebruiker Canvas

Om Azure AD-gebruikers zich aanmelden bij Canvas, moeten ze worden ingericht in Canvas.

In geval van een Canvas is gebruikersaanvragen een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **Canvas** tenant.

2. Ga naar **cursussen \> beheerde Accounts \> Microsoft**.
   
   ![Canvas](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Canvas")

3. Klik op **gebruikers**.
   
   ![Gebruikers](./media/active-directory-saas-canvas-lms-tutorial/IC775995.png "gebruikers")

4. Klik op **nieuwe gebruiker toevoegen**.
   
   ![Gebruikers](./media/active-directory-saas-canvas-lms-tutorial/IC775996.png "gebruikers")

5. Voer de volgende stappen uit op de pagina dialoogvenster Nieuwe gebruiker toevoegen:
   
   ![Gebruiker toevoegen](./media/active-directory-saas-canvas-lms-tutorial/IC775997.png "gebruiker toevoegen")
   
   a. In de **volledige naam** textbox, voer de naam van gebruiker zoals **BrittaSimon**.

   b. In de **e** textbox, voer het e-mailadres van de gebruiker zoals  **brittasimon@contoso.com** .

   c. In de **aanmelding** textbox, voer de gebruiker Azure AD e-mailadres zoals  **brittasimon@contoso.com** .

   d. Selecteer **de gebruiker over het maken van dit account een E-mail**.

   e. Klik op **gebruiker toevoegen**.

>[!NOTE]
>U kunt andere Canvas gebruiker account hulpmiddelen voor het maken of API's die is geleverd door Canvas aan inrichten AAD-gebruikersaccounts.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Canvas.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen papier, kunt u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Canvas**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Canvas in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Canvas.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_203.png

