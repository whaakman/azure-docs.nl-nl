---
title: 'Zelfstudie: Azure Active Directory-integratie met menselijkheid | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en menselijkheid.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 6aa771e9-31c6-48d1-8dde-024bebc06943
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: 7b9730d7d0935e5ee31437f3024588618e12b95e
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-humanity"></a>Zelfstudie: Azure Active Directory-integratie met menselijkheid

In deze zelfstudie leert u hoe menselijkheid integreren met Azure Active Directory (Azure AD).

Menselijkheid integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot menselijkheid heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij menselijkheid (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met menselijkheid, moet u de volgende items:

- Een Azure AD-abonnement
- Een menselijkheid eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Menselijkheid uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-humanity-from-the-gallery"></a>Menselijkheid uit de galerie toevoegen
Voor het configureren van de integratie van menselijkheid met Azure AD, moet u menselijkheid uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen menselijkheid uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **menselijkheid**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_search.png)

5. Selecteer in het deelvenster resultaten **menselijkheid**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met menselijkheid op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in menselijkheid is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in menselijkheid tot stand worden gebracht.

Wijs in menselijkheid, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen eenmalige aanmelding Azure AD met menselijkheid, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker menselijkheid](#creating-a-humanity-test-user)**  - menselijkheid die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing menselijkheid configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met menselijkheid, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **menselijkheid** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_samlbase.png)

3. Op de **menselijkheid domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://company.humanity.com/includes/saml/`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://company.humanity.com/app/`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met het werkelijke aanmeldings-URL en de id. Neem contact op met [menselijkheid Client ondersteuningsteam](https://www.humanity.com/support/) ophalen van deze waarden. 
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_400.png)

6. Op de **menselijkheid configuratie** sectie, klikt u op **menselijkheid configureren** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML Single Sign-On Service-URL en Sign-Out URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_configure.png) 

7. In een ander browservenster geopend, moet u zich aanmelden bij uw **menselijkheid** bedrijf site als beheerder.

8. Klik in het menu bovenaan op **Admin**.
   
    ![Beheerder](./media/active-directory-saas-shiftplanning-tutorial/iC786619.png "Admin")

9. Onder **integratie**, klikt u op **Single Sign-On**.
   
    ![Eenmalige aanmelding](./media/active-directory-saas-shiftplanning-tutorial/iC786620.png "eenmalige aanmelding")

10. In de **Single Sign-On** sectie, voert u de volgende stappen uit:
   
    ![Eenmalige aanmelding](./media/active-directory-saas-shiftplanning-tutorial/iC786905.png "eenmalige aanmelding")
   
    a. Selecteer **SAML ingeschakeld**.

    b. Selecteer **wachtwoord aanmelding toestaan**.

    c. Plak de **SAML Single Sign-On Service-URL** waarde in de **URL-verlener SAML** textbox.

    d. Plak de **Sign-Out URL** waarde in de **externe URL voor afmelden** textbox.
   
    e. Open uw base-64 gecodeerde certificaat in Kladblok, Kopieer de inhoud ervan naar het Klembord en plakt u deze naar de **X.509-certificaat** textbox.

11. Klik op **instellingen opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-shiftplanning-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-shiftplanning-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-shiftplanning-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-shiftplanning-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-humanity-test-user"></a>Een testgebruiker menselijkheid maken

Om Azure AD-gebruikers zich aanmelden bij menselijkheid inschakelt, moeten ze worden ingericht in menselijkheid. In het geval van menselijkheid is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **menselijkheid** bedrijf site als beheerder.

2. Klik op **Admin**.
   
    ![Beheerder](./media/active-directory-saas-shiftplanning-tutorial/iC786619.png "Admin")

3. Klik op **personeel**.
   
    ![Personeel](./media/active-directory-saas-shiftplanning-tutorial/ic786623.png "personeel")

4. Onder **acties**, klikt u op **werknemers toevoegen**.
   
    ![Werknemers toevoegen](./media/active-directory-saas-shiftplanning-tutorial/iC786624.png "werknemers toevoegen")

5. In de **werknemers toevoegen** sectie, voert u de volgende stappen uit:
   
    ![Opslaan van werknemers](./media/active-directory-saas-shiftplanning-tutorial/iC786625.png "werknemers opslaan")
   
    a. Typ de **voornaam**, **achternaam**, en **e** van een geldige AAD-account dat u inrichten in de bijbehorende tekstvakken wilt.

    b. Klik op **opslaan werknemers**.

>[!NOTE]
>U kunt andere menselijkheid gebruiker account hulpmiddelen voor het maken of API's die is geleverd door menselijkheid aan inrichten AAD-gebruikersaccounts.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan menselijkheid.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen menselijkheid, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **menselijkheid**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel menselijkheid in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing menselijkheid.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_203.png

