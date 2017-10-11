---
title: 'Zelfstudie: Azure Active Directory-integratie met AnswerHub | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en AnswerHub.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 818b91d7-01df-4b36-9706-f167c710a73c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 3a1c9cc5d7a2ebe28e9fb7e0e6ed8e3d393873ae
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-answerhub"></a>Zelfstudie: Azure Active Directory-integratie met AnswerHub

In deze zelfstudie leert u hoe AnswerHub integreren met Azure Active Directory (Azure AD).

AnswerHub integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot AnswerHub heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij AnswerHub (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met AnswerHub, moet u de volgende items:

- Een Azure AD-abonnement
- Een AnswerHub eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. AnswerHub uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-answerhub-from-the-gallery"></a>AnswerHub uit de galerie toevoegen
Voor het configureren van de integratie van AnswerHub in Azure AD, moet u AnswerHub uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen AnswerHub uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **AnswerHub**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_search.png)

5. Selecteer in het deelvenster resultaten **AnswerHub**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie configureert en test eenmalige aanmelding Azure AD met AnswerHub op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in AnswerHub is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in AnswerHub tot stand worden gebracht.

Wijs in AnswerHub, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met AnswerHub, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker AnswerHub](#creating-an-answerhub-test-user)**  - AnswerHub die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing AnswerHub configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met AnswerHub, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **AnswerHub** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_samlbase.png)

3. Op de **AnswerHub domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<company>.answerhub.com`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://<company>.answerhub.com`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met het werkelijke aanmeldings-URL en de id. Neem contact op met [AnswerHub Client ondersteuningsteam](mailto:success@answerhub.com) ophalen van deze waarden. 
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-answerhub-tutorial/tutorial_general_400.png)

6. Op de **AnswerHub configuratie** sectie, klikt u op **configureren AnswerHub** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out URL's, en SAML Single Sign-On Service** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_configure.png) 

7. In een ander browservenster, meld u bij uw bedrijf AnswerHub site als beheerder.
   
    >[!NOTE]
    >Als u nodig hebt bij het configureren van AnswerHub, neem dan contact op met [ondersteuningsteam van AnswerHub](mailto:success@answerhub.com.).
   
8. Ga naar **beheer**.

9. Klik op de **gebruikers en groepen** tabblad.

10. Klik in het navigatievenster aan de linkerkant in de **sociale instellingen** sectie, klikt u op **SAML Setup**.

11. Klik op **IDP Config** tabblad.

12. Op de **IDP Config** tabblad, voert u de volgende stappen uit:

     ![Setup van SAML](./media/active-directory-saas-answerhub-tutorial/ic785172.png "SAML Setup")  
  
     a. In **IDP aanmeldings-URL** textbox plakken **SAML Single Sign-On Service-URL** die u hebt gekopieerd vanuit Azure-portal.
  
     b. In **IDP afmelding URL** textbox plakken **Sign-Out URL** waarde die u hebt gekopieerd vanuit Azure-portal.
     
     c. In **IDP-naamnotatie id** textbox, voert u de gebruiker id dezelfde waarde als de geselecteerde in Azure-portal op **gebruikerskenmerken** sectie.
  
     d. Klik op **sleutels en certificaten**.

13. Voer de volgende stappen uit op het tabblad sleutels en certificaten:
    
     ![Sleutels en certificaten](./media/active-directory-saas-answerhub-tutorial/ic785173.png "sleutels en certificaten")  
 
     a. Open uw base-64 gecodeerde certificaat dat u hebt gedownload vanuit Azure-portal in Kladblok, Kopieer de inhoud ervan naar het Klembord en plakt u deze naar de **IDP openbare sleutel (x 509-indeling)** textbox.
  
     b. Klik op **Opslaan**.

14. Op de **IDP Config** tabblad **opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-answerhub-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-answerhub-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-answerhub-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-answerhub-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-an-answerhub-test-user"></a>Een testgebruiker AnswerHub maken

Om Azure AD-gebruikers zich aanmelden bij AnswerHub, moeten ze worden ingericht in AnswerHub.  
In het geval van AnswerHub is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **AnswerHub** bedrijf site als administrator.

2. Ga naar **beheer**.

3. Klik op de **gebruikers en groepen** tabblad.

4. Klik in het navigatievenster aan de linkerkant in de **gebruikers beheren** sectie, klikt u op **maken of importeren gebruikers**.
   
   ![Gebruikers en groepen](./media/active-directory-saas-answerhub-tutorial/ic785175.png "gebruikers en groepen")

5. Typ de **e-mailadres**, **gebruikersnaam** en **wachtwoord** van een geldige Azure Active Directory-account die u wilt inrichten in de bijbehorende tekstvakken en klik vervolgens op **opslaan**.

>[!NOTE]
>U kunt andere AnswerHub gebruiker account hulpmiddelen voor het maken of API's die is geleverd door AnswerHub aan inrichten AAD-gebruikersaccounts.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan AnswerHub.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen AnswerHub, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **AnswerHub**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel AnswerHub in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing AnswerHub.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_203.png

