---
title: 'Zelfstudie: Azure Active Directory-integratie met Samanage | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Samanage.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f0db4fb0-7eec-48c2-9c7a-beab1ab49bc2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: c54dbe407145a29a712acc3c0fb549a38ac26bed
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-samanage"></a>Zelfstudie: Azure Active Directory-integratie met Samanage

In deze zelfstudie leert u hoe Samanage integreren met Azure Active Directory (Azure AD).

Samanage integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Samanage heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Samanage (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Samanage, moet u de volgende items:

- Een Azure AD-abonnement
- Een Samanage eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Samanage uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-samanage-from-the-gallery"></a>Samanage uit de galerie toevoegen
Voor het configureren van de integratie van Samanage in Azure AD, moet u Samanage uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Samanage uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Samanage**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_search.png)

5. Selecteer in het deelvenster resultaten **Samanage**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie configureert en test eenmalige aanmelding Azure AD met Samanage op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Samanage is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Samanage tot stand worden gebracht.

Wijs in Samanage, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Samanage, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Samanage](#creating-a-samanage-test-user)**  - Samanage die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Samanage configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Samanage, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Samanage** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_samlbase.png)

3. Op de **Samanage domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<Company Name>.samanage.com/saml_login/<Company Name>`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://<Company Name>.samanage.com`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke aanmeldings-URL en de id, die verderop in de zelfstudie wordt uitgelegd. Voor meer informatie contact op met [Samanage Client ondersteuningsteam](https://www.samanage.com/support).    
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-samanage-tutorial/tutorial_general_400.png)

6. Op de **Samanage configuratie** sectie, klikt u op **configureren Samanage** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL en SAML entiteit-ID** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_configure.png) 

7. In een ander browservenster, meld u bij uw bedrijf Samanage site als beheerder.

8. Klik op **Dashboard** en selecteer **Setup** in het navigatiedeelvenster links.
   
    ![Dashboard](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_001.png "Dashboard")

9. Klik op **Single Sign-On**.
   
    ![Eenmalige aanmelding](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_002.png "eenmalige aanmelding")

10. Navigeer naar **aanmelding via SAML** sectie, voert u de volgende stappen uit:
   
    ![Aanmelding via SAML](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_003.png "aanmelding via SAML")
 
    a. Klik op **eenmalige aanmelding met SAML inschakelen**.  
 
    b. In de **identiteit Provider URL** textbox, plak de waarde van **SAML entiteit-ID** die u hebt gekopieerd vanuit Azure-portal.    
 
    c. Bevestig de **aanmeldings-URL** overeenkomt met de **aanmelding op URL** van **Samanage domein en de URL's** sectie in Azure-portal.
 
    d. In de **afmelding URL** textbox, voer de waarde van **Sign-Out URL** die u hebt gekopieerd vanuit Azure-portal.
 
    e. In de **SAML verlener** textbox, type URI van de app-id moet worden ingesteld in de id-provider.
 
    f. Open uw base-64 gecodeerde certificaat gedownload vanuit Azure-portal in Kladblok, Kopieer de inhoud ervan naar het Klembord en plakt u deze naar de **plak uw id-Provider-x.509-certificaat onderstaande** textbox.
 
    g. Klik op **gebruikers maken als ze bestaan niet in Samanage**.
 
    h. Klik op **Update**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
 
### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-samanage-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-samanage-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-samanage-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-samanage-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-samanage-test-user"></a>Een testgebruiker Samanage maken

Om Azure AD-gebruikers zich aanmelden bij Samanage, moeten ze worden ingericht in Samanage.  
In het geval van Samanage is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf Samanage site als beheerder.

2. Klik op **Dashboard** en selecteer **Setup** in het linkernavigatievenster pan.
   
    ![Setup](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_001.png "Setup")

3. Klik op de **gebruikers** tabblad
   
    ![Gebruikers](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_006.png "gebruikers")

4. Klik op **nieuwe gebruiker**.
   
    ![Nieuwe gebruiker](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_007.png "nieuwe gebruiker")

5. Typ de **naam** en de **e-mailadres** van een Azure Active Directory-account dat u wilt richten en klik op **gebruiker maken**.
   
    ![Gebruiker maken](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_008.png "gebruiker maken")
   
   >[!NOTE]
   >De accounthouder Azure Active Directory wordt een e-mailbericht ontvangen en Ga als volgt een koppeling om hun account te bevestigen voordat deze geactiveerd wordt. U kunt andere Samanage gebruiker account hulpmiddelen voor het maken of API's geleverd door Samanage om in te richten Azure Active Directory-gebruikersaccounts.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Samanage.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Samanage, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Samanage**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Samanage in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Samanage.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_203.png

