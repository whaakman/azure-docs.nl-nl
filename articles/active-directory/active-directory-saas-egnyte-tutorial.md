---
title: 'Zelfstudie: Azure Active Directory-integratie met Egnyte | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Egnyte.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8c2101d4-1779-4b36-8464-5c1ff780da18
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: 62d01333b61e73c83588d2d1701c0c300df4ab1c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-egnyte"></a>Zelfstudie: Azure Active Directory-integratie met Egnyte

In deze zelfstudie leert u hoe Egnyte integreren met Azure Active Directory (Azure AD).

Egnyte integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Egnyte heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Egnyte (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Egnyte, moet u de volgende items:

- Een Azure AD-abonnement
- Een Egnyte eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Egnyte uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-egnyte-from-the-gallery"></a>Egnyte uit de galerie toevoegen
Voor het configureren van de integratie van Egnyte in Azure AD, moet u Egnyte uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Egnyte uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Egnyte**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_search.png)

5. Selecteer in het deelvenster resultaten **Egnyte**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met Egnyte op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Egnyte is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Egnyte tot stand worden gebracht.

Wijs in Egnyte, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Egnyte, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Egnyte](#creating-an-egnyte-test-user)**  - Egnyte die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Egnyte configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Egnyte, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Egnyte** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_samlbase.png)

3. Op de **Egnyte domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_url.png)

    In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<companyname>.egnyte.com`

    > [!NOTE] 
    > Deze waarde is geen echte. Deze waarde bijwerken met de werkelijke URL voor eenmalige aanmelding. Neem contact op met [Egnyte Client ondersteuningsteam](https://www.egnyte.com/corp/contact_egnyte.html) deze waarde op te halen. 
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-egnyte-tutorial/tutorial_general_400.png)

6. Op de **Egnyte configuratie** sectie, klikt u op **configureren Egnyte** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_configure.png) 

7. In een ander browservenster, meld u aan bij uw bedrijf Egnyte site als beheerder.

8. Klik op **instellingen**.
   
   ![Instellingen](./media/active-directory-saas-egnyte-tutorial/ic787819.png "instellingen")

9. Klik in het menu **instellingen**.

   ![Instellingen](./media/active-directory-saas-egnyte-tutorial/ic787820.png "instellingen")

10. Klik op de **configuratie** tabblad en klik vervolgens op **beveiliging**.

    ![Beveiliging](./media/active-directory-saas-egnyte-tutorial/ic787821.png "beveiliging")

11. In de **eenmalige aanmelding verificatie** sectie, voert u de volgende stappen uit:

    ![Eenmalige verificatie](./media/active-directory-saas-egnyte-tutorial/ic787822.png "eenmalige-verificatie")   
    
    a. Als **eenmalige aanmelding verificatie**, selecteer **SAML 2.0**.
   
    b. Als **identiteitsprovider**, selecteer **AzureAD**.
   
    c. Plakken **SAML Single Sign-On Service-URL** gekopieerd vanuit Azure-portal in de **identiteit provider aanmeldings-URL** textbox.
   
    d. Plakken **SAML entiteit-ID** die u hebt gekopieerd vanuit Azure-portal in de **identiteit provider entiteit-ID** textbox.
      
    e. De base-64 gecodeerde certificaat openen in Kladblok van Azure portal hebt gedownload, Kopieer de inhoud ervan naar het Klembord en plakt u deze naar de **provider identiteitscertificaat** textbox.
   
    f. Als **standaard Gebruikerskoppeling**, selecteer **e-mailadres**.
   
    g. Als **domeinspecifieke verlener waarde**, selecteer **uitgeschakeld**.
   
    h. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-egnyte-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-egnyte-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-egnyte-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-egnyte-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-an-egnyte-test-user"></a>Een testgebruiker Egnyte maken

Om Azure AD-gebruikers zich aanmelden bij Egnyte, moeten ze worden ingericht in Egnyte. In het geval van Egnyte is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **Egnyte** bedrijf site als administrator.

2. Ga naar **instellingen \> gebruikers en groepen**.

3. Klik op **nieuwe gebruiker toevoegen**, en selecteer vervolgens het type van de gebruiker die u wilt toevoegen.
   
   ![Gebruikers](./media/active-directory-saas-egnyte-tutorial/ic787824.png "gebruikers")

4. In de **nieuwe standaardgebruiker** sectie, voert u de volgende stappen uit:
   
   ![Nieuwe standaardgebruiker](./media/active-directory-saas-egnyte-tutorial/ic787825.png "nieuwe standaardgebruiker")   

   a. Typ de **e**, **gebruikersnaam**, en andere details van een geldig Azure Active Directory-account dat u inrichten wilt.
   
   b. Klik op **Opslaan**.
    
    >[!NOTE]
    >De houder van Azure Active Directory-account ontvangt een melding per e-mail.
    >

>[!NOTE]
>U kunt andere Egnyte gebruiker account hulpmiddelen voor het maken of API's die is geleverd door Egnyte aan inrichten AAD-gebruikersaccounts.
> 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Egnyte.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Egnyte, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Egnyte**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Egnyte in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Egnyte.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_203.png

