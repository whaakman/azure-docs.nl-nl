---
title: 'Zelfstudie: Azure Active Directory-integratie met Optimizely | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Optimizely.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28ef03e1-9aad-4301-af97-d94e853edc74
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 4d6f6da6bace09fbd6ab105530a1162653675c99
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-optimizely"></a>Zelfstudie: Azure Active Directory-integratie met Optimizely

In deze zelfstudie leert u hoe Optimizely integreren met Azure Active Directory (Azure AD).

Optimizely integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Optimizely heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Optimizely (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Optimizely, moet u de volgende items:

- Een Azure AD-abonnement
- Een Optimizely eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Optimizely uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-optimizely-from-the-gallery"></a>Optimizely uit de galerie toevoegen
Voor het configureren van de integratie van Optimizely in Azure AD, moet u Optimizely uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Optimizely uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Optimizely**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_search.png)

5. Selecteer in het deelvenster resultaten **Optimizely**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met Optimizely op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Optimizely is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Optimizely tot stand worden gebracht.

Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Optimizely.

Om te configureren en testen van Azure AD eenmalige aanmelding met Optimizely, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Optimizely](#creating-an-optimizely-test-user)**  - Optimizely die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Optimizely configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Optimizely, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Optimizely** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_samlbase.png)

3. Op de **Optimizely domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://app.optimizely.net/<instance name>`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:`urn:auth0:optimizely:contoso`

    > [!NOTE] 
    > Deze waarden zijn niet de werkelijke. U kunt de waarde wordt bijgewerkt met de werkelijke aanmeldings-URL en de id, die verderop in de zelfstudie wordt uitgelegd. 

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-optimizely-tutorial/tutorial_general_400.png)

6. Op de **Optimizely configuratie** sectie, klikt u op **configureren Optimizely** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_configure.png) 

7. Eenmalige aanmelding configureren op **Optimizely** zijde, neem contact op met uw accountmanager Optimizely en bieden de gedownloade **certificaat (Base64)**, en **SAML Single Sign-On Service-URL**. 

8. In antwoord op uw e-mailadres biedt Optimizely u de aanmelding op URL (Serviceprovider geïnitieerde SSO) en de id (Service Provider entiteit-ID)-waarden.

    a. Kopiëren de **Serviceprovider geïnitieerde eenmalige aanmelding URL** opgegeven door Optimizely en plakken in de **aanmelding op URL** textbox in **Optimizely domein en de URL's** sectie in Azure portal 

    b. Kopieer de **Service Provider entiteit-ID** opgegeven door Optimizely en plakken in de **id** textbox in **Optimizely domein en de URL's** sectie in Azure portal 

9. In een ander browservenster eenmalige aanmelding voor uw toepassing Optimizely.

10. Klikt u op dat u de accountnaam in de rechterbovenhoek rechterbovenhoek en vervolgens **Accountinstellingen**.
   
    ![Azure AD voor eenmalige aanmelding](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_09.png)

11. Schakel het selectievakje in op het tabblad Account **eenmalige aanmelding inschakelen** onder eenmalige aanmelding in de **overzicht** sectie.
   
    ![Azure AD voor eenmalige aanmelding](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_10.png)
    
12. Klik op **opslaan**

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-optimizely-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-optimizely-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-optimizely-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-optimizely-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van Britta Simon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-an-optimizely-test-user"></a>Een testgebruiker Optimizely maken

In deze sectie kunt u een gebruiker Britta Simon aangeroepen in Optimizely maken.

1. Selecteer op de startpagina **deelnemers** tabblad.

2. Nieuwe medewerker toevoegen aan het project: klik op **nieuwe medewerker**.
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-optimizely-tutorial/create_aaduser_10.png)

3. Vul in het e-mailadres en een rol toewijzen. Klik op **uitnodigen**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-optimizely-tutorial/create_aaduser_11.png)

4. Ze ontvangen een e-uitnodiging. Met behulp van het e-mailadres, hebben ze zich aanmelden bij Optimizely.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Optimizely.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Optimizely, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Optimizely**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Optimizely in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Optimizely. 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_203.png

