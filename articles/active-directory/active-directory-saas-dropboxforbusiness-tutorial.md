---
title: 'Zelfstudie: Azure Active Directory-integratie met Dropbox voor bedrijven | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Dropbox voor bedrijven.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 63502412-758b-4b46-a580-0e8e130791a1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: a56a5af171eaca259db29f25fee4331a77313420
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-dropbox-for-business"></a>Zelfstudie: Azure Active Directory-integratie met Dropbox voor bedrijven

In deze zelfstudie leert u hoe Dropbox voor bedrijven integreren met Azure Active Directory (Azure AD).

Dropbox voor bedrijven integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Dropbox voor bedrijven heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Dropbox voor bedrijven (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Dropbox voor bedrijven, moet u de volgende items:

- Een Azure AD-abonnement
- Een Dropbox voor eenmalige aanmelding Business abonnement ingeschakeld

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Dropbox voor bedrijven uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-dropbox-for-business-from-the-gallery"></a>Dropbox voor bedrijven uit de galerie toevoegen
Voor het configureren van de integratie van Dropbox voor bedrijven met Azure AD, moet u Dropbox voor bedrijven uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Dropbox voor bedrijven uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Klik op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Dropbox voor bedrijven**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_search.png)

5. Selecteer in het deelvenster resultaten **Dropbox voor bedrijven**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Dropbox voor bedrijven op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Dropbox voor bedrijven is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Dropbox voor bedrijven tot stand worden gebracht.

Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Dropbox voor bedrijven.

Om te configureren en testen van Azure AD eenmalige aanmelding met Dropbox voor bedrijven, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een Dropbox voor zakelijke testgebruiker](#creating-a-dropbox-for-business-test-user)**  - Dropbox voor bedrijven die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw dropbox geplaatst voor Business-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met Dropbox voor bedrijven, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Dropbox voor bedrijven** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_samlbase.png)

3. Op de **Dropbox voor Business-domein en URL's** sectie, voert u de volgende stappen uit:

    a. Meld u aan op uw Dropbox voor bedrijven-tenant. 
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769509.png "eenmalige aanmelding configureren")
   
    b. Klik in het navigatievenster aan de linkerkant op **beheerconsole**. 
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769510.png "eenmalige aanmelding configureren")
   
    c. Op de **beheerconsole**, klikt u op **verificatie** in het navigatiedeelvenster links. 
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769511.png "eenmalige aanmelding configureren")
   
    d. In de **eenmalige aanmelding** sectie **eenmalige aanmelding inschakelen**, en klik vervolgens op **meer** uit te breiden in deze sectie.  
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769512.png "eenmalige aanmelding configureren")
   
    e. Kopieer de URL naast **kunnen gebruikers zich aanmelden door te voeren van het e-mailadres of kunnen ze rechtstreeks naar**. 
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769513.png)
    
    f. Op de Azure-portal in de **aanmeldings-URL** textbox, plak de URL.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_url.png)

     In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://www.dropbox.com/sso/<id>`

    > [!NOTE] 
    > Deze waarde is geen echte waarde. Werk de waarde met de werkelijke aanmeldings-URL u via een sectie voor eenmalige aanmelding krijgen. Neem contact op met [Dropbox voor Business Client ondersteuningsteam](https://www.dropbox.com/business/contact) deze waarde op te halen. 
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_400.png)

6. Op de **Dropbox voor zakelijke configuratie** sectie, klikt u op **Dropbox configureren voor bedrijven** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_configure.png) 

7. Eenmalige aanmelding configureren op **Dropbox voor bedrijven** zijde, gaat u in uw Dropbox voor bedrijven-tenant, het **eenmalige aanmelding** sectie van de **verificatie** pagina de volgende stappen uit: 
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769516.png "eenmalige aanmelding configureren")
   
    a. Klik op **vereist**.
   
    b. In de Azure-portal op de **eenmalige aanmelding configureren** venster, Kopieer de **SAML Single Sign-On Service-URL** waarde en plak deze in de **aanmelden URL** textbox.

    c. Klik op **certificaat kiezen**, en blader vervolgens naar uw **Base64-gecodeerde certificaatbestand**.

    d. Klik op **wijzigingen opslaan** om de configuratie op uw DropBox voor zakelijke tenant te voltooien.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_01.png) 

2.  Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_02.png) 

3. Aan de bovenkant van het dialoogvenster, klikt u op **toevoegen** openen de **gebruiker** dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-dropbox-for-business-test-user"></a>Een Dropbox voor zakelijke testgebruiker maken

In deze sectie wordt een gebruiker met de naam Britta Simon gemaakt in Dropbox voor bedrijven. Dropbox voor bedrijven ondersteunt just-in-time-inrichting, die standaard is ingeschakeld.

Er is geen actie-item voor u in deze sectie. Als een gebruiker niet al in Dropbox voor bedrijven bestaat, wordt een nieuw gemaakt wanneer u probeert te krijgen tot Dropbox voor bedrijven.

>[!Note]
>Als u wilt een gebruiker handmatig maken, neem contact op met [Dropbox voor Business Client ondersteuningsteam](https://www.dropbox.com/business/contact) 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Dropbox voor bedrijven.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Dropbox voor bedrijven, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Dropbox voor bedrijven**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u klikt op de Dropbox voor bedrijven-tegel in het deelvenster toegang, krijgt u de aanmeldingspagina van uw Dropbox voor Business-toepassing.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Gebruikers inrichten configureren](active-directory-saas-dropboxforbusiness-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_203.png

