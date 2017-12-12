---
title: 'Zelfstudie: Azure Active Directory-integratie met Mimecast-beheerconsole | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Mimecast-beheerconsole.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 81c50614-f49b-4bbc-97d5-3cf77154305f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: jeedes
ms.openlocfilehash: 64bb18caa046f8a08be8f229e4c82a34306a8f72
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-admin-console"></a>Zelfstudie: Azure Active Directory-integratie met Mimecast-beheerconsole

In deze zelfstudie leert u hoe de beheerconsole Mimecast integreren met Azure Active Directory (Azure AD).

Beheerconsole Mimecast integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Mimecast-beheerconsole heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij de beheerconsole Mimecast (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Mimecast-beheerconsole, moet u de volgende items:

- Een Azure AD-abonnement
- Een beheerconsole Mimecast eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Mimecast-beheerconsole uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-mimecast-admin-console-from-the-gallery"></a>Mimecast-beheerconsole uit de galerie toevoegen
Voor het configureren van de integratie van Mimecast-beheerconsole in Azure AD, moet u Mimecast-beheerconsole uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Mimecast-beheerconsole uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Mimecast-beheerconsole**, selecteer **Mimecast-beheerconsole** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Mimecast-beheerconsole in de lijst met resultaten](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met Mimecast-beheerconsole op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in de beheerconsole Mimecast is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in de beheerconsole Mimecast tot stand worden gebracht.

In de beheerconsole Mimecast, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Mimecast-beheerconsole, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker beheerconsole Mimecast](#create-a-mimecast-admin-console-test-user)**  - Mimecast-beheerconsole die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In deze sectie maakt u Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing Mimecast-beheerconsole.

**Voor het configureren van Azure AD eenmalige aanmelding met Mimecast-beheerconsole, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Mimecast-beheerconsole** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_samlbase.png)

3. Op de **Mimecast Admin Consoledomein en de URL's** sectie, voert u de volgende stappen uit:

    ![Domein Mimecast Administrator-Console en URL's van eenmalige aanmelding informatie](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_url.png)

    In de **aanmeldings-URL** textbox, typ de URL:
    | |
    | -- |
    | `https://webmail-uk.mimecast.com`|
    | `https://webmail-us.mimecast.com`|

    > [!NOTE] 
    > Het teken op de URL voor is specifieke regio.

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_certificate.png) 

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_400.png)

6. Op de **Mimecast Console beheerdersconfiguratie** sectie, klikt u op **Mimecast beheerconsole configureren** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Configuratie van de Console Mimecast Admin](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_configure.png) 

7. In een ander browservenster, meld u bij uw Mimecast-beheerconsole als beheerder.

8. Ga naar **Services \> toepassing**.

    ![Services](./media/active-directory-saas-mimecast-admin-console-tutorial/ic794998.png "Services")

9. Klik op **verificatie profielen**.

    ![Verificatie profielen](./media/active-directory-saas-mimecast-admin-console-tutorial/ic794999.png "verificatie-profielen")
    
10. Klik op **nieuwe Authentication profiel**.

    ![Nieuwe verificatie profielen](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795000.png "nieuwe verificatie-profielen")

11. In de **Authentication profiel** sectie, voert u de volgende stappen uit:

    ![Authentication profiel](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795015.png "Authentication profiel")
    
    a. In de **beschrijving** textbox, typ een naam voor uw configuratie.
    
    b. Selecteer **afdwingen van SAML-verificatie voor de beheerconsole Mimecast**.
    
    c. Als **Provider**, selecteer **Azure Active Directory**.
    
    d. Plakken **SAML entiteit-ID**, die u hebt gekopieerd vanuit de Azure-portal in de **URL-verlener** textbox.
    
    e. Plakken **SAML Single Sign-On Service-URL**, die u hebt gekopieerd vanuit de Azure-portal in de **aanmeldings-URL** textbox.

    f. Plakken **SAML Single Sign-On Service-URL**, die u hebt gekopieerd vanuit de Azure-portal in de **afmelding URL** textbox.
    
    >[!NOTE]
    >De waarde van de aanmeldings-URL en de afmelding URL-waarde zijn voor de beheerconsole Mimecast hetzelfde.
    
    g. Open uw base 64-certificaat gedownload vanuit Azure-portal in Kladblok, verwijdert u de eerste regel ('*--*') en de laatste regel ('*--*'), kopieert u de resterende inhoud van dit in uw Klembord en plakt u deze naar de **Provider identiteitscertificaat (metagegevens)** textbox.
    
    h. Selecteer **toestaan voor eenmalige op**.
    
    ik. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985) 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-mimecast-admin-console-test-user"></a>Maak een testgebruiker Mimecast-beheerconsole

Om in te schakelen gebruikers van Azure AD aan te melden bij Mimecast-beheerconsole, moeten ze worden ingericht in Mimecast-beheerconsole. Inrichting is een handmatige taak in het geval van Mimecast-beheerconsole.

* U moet een domein registreren voordat u gebruikers kunt maken.

**Als u wilt configureren voor gebruikers inrichten, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **Mimecast-beheerconsole** als administrator.
2. Ga naar **mappen \> interne**.
   
   ![Mappen](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795003.png "mappen")
3. Klik op **nieuwe domein registreren**.
   
   ![Nieuwe domein registreren](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795004.png "nieuwe domein registreren")
4. Nadat het nieuwe domein is gemaakt, klikt u op **nieuw adres**.
   
   ![Nieuw adres](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795005.png "nieuw adres")
5. Voer de volgende stappen uit in het dialoogvenster Nieuw adres:
   
   ![Sla](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795006.png "opslaan")
   
   a. Typ de **e-mailadres**, **globale naam**, **wachtwoord**, en **wachtwoord bevestigen** kenmerken van een geldige Azure AD-account u wilt in te richten in de bijbehorende tekstvakken.

   b. Klik op **Opslaan**.

>[!NOTE]
>U kunt geen andere hulpprogramma's voor Mimecast-beheerconsole gebruiker-account maken of API's die worden geleverd door Mimecast-beheerconsole gebruiken voor het inrichten van Azure AD-gebruikersaccounts. 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang te verlenen aan Mimecast-beheerconsole.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen Mimecast-beheerconsole, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Mimecast-beheerconsole**.

    ![De koppeling Mimecast-beheerconsole in de lijst met toepassingen](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Mimecast-beheerconsole in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Mimecast-beheerconsole.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_203.png

