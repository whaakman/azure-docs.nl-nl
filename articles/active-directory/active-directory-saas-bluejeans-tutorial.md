---
title: 'Zelfstudie: Azure Active Directory-integratie met BlueJeans | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en BlueJeans.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: dfc634fd-1b55-4ba8-94a8-b8288429b6a9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: jeedes
ms.openlocfilehash: 03bf65852b8d3cf14aebf155891a028db86e78d0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bluejeans"></a>Zelfstudie: Azure Active Directory-integratie met BlueJeans

In deze zelfstudie leert u hoe BlueJeans integreren met Azure Active Directory (Azure AD).

BlueJeans integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot BlueJeans heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij BlueJeans (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met BlueJeans, moet u de volgende items:

- Een Azure AD-abonnement
- Een BlueJeans eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. BlueJeans uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-bluejeans-from-the-gallery"></a>BlueJeans uit de galerie toevoegen
Voor het configureren van de integratie van BlueJeans in Azure AD, moet u BlueJeans uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen BlueJeans uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **BlueJeans**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_search.png)

5. Selecteer in het deelvenster resultaten **BlueJeans**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met BlueJeans op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in BlueJeans is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in BlueJeans tot stand worden gebracht.

Wijs in BlueJeans, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met BlueJeans, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker BlueJeans](#creating-a-bluejeans-test-user)**  - hebben een equivalent van Britta Simon in BlueJeans die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing BlueJeans configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met BlueJeans, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **BlueJeans** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_samlbase.png)

3. Op de **BlueJeans domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<companyname>.BlueJeans.com`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://<companyname>.BlueJeans.com`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met het werkelijke aanmeldings-URL en de id. Neem contact op met [BlueJeans Client ondersteuningsteam](https://support.bluejeans.com/contact) ophalen van deze waarden. 
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-bluejeans-tutorial/tutorial_general_400.png)

6. Op de **BlueJeans configuratie** sectie, klikt u op **configureren BlueJeans** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, de URL van wijzigen wachtwoord en de SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_configure.png) 

7. In een ander browservenster geopend, moet u zich aanmelden bij uw **BlueJeans** bedrijf site als beheerder.

8. Ga naar **ADMIN \> groepsinstellingen \> beveiliging**.
   
   ![Beheerder](./media/active-directory-saas-bluejeans-tutorial/IC785868.png "Admin")

9. In de **beveiliging** sectie, voert u de volgende stappen uit:
   
   ![SAML voor eenmalige aanmelding](./media/active-directory-saas-bluejeans-tutorial/IC785869.png "SAML voor eenmalige aanmelding")   
   
   a. Selecteer **SAML voor eenmalige aanmelding**.
  
   b. Selecteer **automatische inrichting inschakelen**.

10. Gaan met de volgende stappen uit:

    ![Pad van het certificaat](./media/active-directory-saas-bluejeans-tutorial/IC785870.png "pad van het certificaat")
    
    a. Klik op **bestand kiezen**, en vervolgens het gedownloade certificaat te uploaden.
   
    b. Plakken **SAML Single Sign-On Service-URL** in de **aanmeldings-URL** textbox.
   
    c. Plakken **URL van wijzigen wachtwoord** in de **URL voor wachtwoord wijzigen** textbox.
   
    d. Plakken **Sign-Out URL** in de **afmelding URL** textbox.

11. Gaan met de volgende stappen uit:
    
    ![Wijzigingen opslaan](./media/active-directory-saas-bluejeans-tutorial/IC785874.png "wijzigingen opslaan")
    
    a. In de **gebruikers-id** textbox type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.
   
    b. In de **e** textbox type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.
   
    c. Klik op **wijzigingen opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-bluejeans-test-user"></a>Een testgebruiker BlueJeans maken

Om Azure AD-gebruikers zich aanmelden bij BlueJeans, moeten ze worden ingericht in BlueJeans.  

In geval van een BlueJeans is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **BlueJeans** bedrijf site als beheerder.

2. Ga naar **ADMIN \> gebruikers beheren \> gebruiker toevoegen**.
   
   ![Beheerder](./media/active-directory-saas-bluejeans-tutorial/IC785877.png "Admin")
   
   >[!IMPORTANT]
   >De **gebruiker toevoegen** tabblad is alleen beschikbaar als in de **tabblad Beveiliging**, **automatische inrichting inschakelen** is uitgeschakeld. 
   
3. In de **gebruiker toevoegen** sectie, voert u de volgende stappen uit:

    ![Gebruiker toevoegen](./media/active-directory-saas-bluejeans-tutorial/IC785886.png "gebruiker toevoegen")
    
    a. Typ een **BlueJeans gebruikersnaam**, een **e-mailadres**, een **BlueJeans vergadering ID**, een **beheerder wachtwoordcode**, een **volledige naam**, wordt de **bedrijf** van een geldige AAD-account dat u inrichten in de bijbehorende tekstvakken wilt.
    
    b. Klik op **gebruiker toevoegen**.

>[!NOTE]
>U kunt andere BlueJeans gebruiker account hulpmiddelen voor het maken of API's die is geleverd door BlueJeans aan inrichten AAD-gebruikersaccounts. 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan BlueJeans.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen BlueJeans, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **BlueJeans**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel BlueJeans in het deelvenster toegang, krijgt u de aanmeldingspagina van BlueJeans toepassing.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_203.png

