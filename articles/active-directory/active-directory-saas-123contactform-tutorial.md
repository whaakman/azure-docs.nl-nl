---
title: 'Zelfstudie: Azure Active Directory-integratie met 123ContactForm | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en 123ContactForm.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 5211910a-ab96-4709-959a-524c4d57c43e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 70955676e78642e6c8a6eb85f8165b327baece3f
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-123contactform"></a>Zelfstudie: Azure Active Directory-integratie met 123ContactForm

In deze zelfstudie leert u hoe 123ContactForm integreren met Azure Active Directory (Azure AD).

123ContactForm integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot 123ContactForm heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij 123ContactForm inschakelen (Single Sign-On) met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met 123ContactForm, moet u de volgende items:

- Een Azure AD-abonnement
- Een 123ContactForm eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. 123ContactForm uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-123contactform-from-the-gallery"></a>123ContactForm uit de galerie toevoegen
Voor het configureren van de integratie van 123ContactForm in Azure AD, moet u 123ContactForm uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen 123ContactForm uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **123ContactForm**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_search.png)

5. Selecteer in het deelvenster resultaten **123ContactForm**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met 123ContactForm op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in 123ContactForm is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in 123ContactForm tot stand worden gebracht.

Wijs in 123ContactForm, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met 123ContactForm, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker 123ContactForm](#creating-a-123contactform-test-user)**  - hebben een equivalent van Britta Simon in 123ContactForm die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing 123ContactForm configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met 123ContactForm, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **123ContactForm** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_samlbase.png)

3. Op de **123ContactForm domein en de URL's** sectie als u wilt configureren van de toepassing in **IDP geïnitieerd modus**, voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-123contactform-tutorial/url1.png)

    a. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://www.123contactform.com/saml/azure_ad/<tenant_id>/metadata`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://www.123contactform.com/saml/azure_ad/<tenant_id>/acs`

4. Als u wilt configureren van de toepassing in **SP geïnitieerd modus**, voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-123contactform-tutorial/url2.png)

    a. Klik op de **weergeven geavanceerde instellingen voor URL** optie

    b. In de **aanmelding op URL** textbox, typ een URL als:`https://www.123contactform.com/saml/azure_ad/<tenant_id>/sso`

    > [!NOTE] 
    > Deze waarden zijn niet echt. U moet deze waarde van de werkelijke URL's en -id die verderop in de zelfstudie wordt uitgelegd bijwerken.
    
5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_certificate.png) 

6. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-123contactform-tutorial/tutorial_general_400.png)

7. Eenmalige aanmelding configureren op **123ContactForm** aan clientzijde, gaat u naar [https://www.123contactform.com/form-2709121/](https://www.123contactform.com/form-2709121/) en voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-123contactform-tutorial/submit.png) 

    a. In de **e** textbox, typ de e-mailadres van de gebruiker eenledige **BrittaSimon@Contoso.com**.

    b. Klik op **uploaden** en blader naar het Metadata XML-bestand dat u hebt gedownload vanuit Azure-portal.

    c. Klik op **indienen formulier**.

8. Op de **instellingen App van Microsoft Azure AD eenmalige aanmelding -** de volgende stappen uitvoeren:
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-123contactform-tutorial/url3.png)

    a. Als u wilt configureren van de toepassing in **IDP geïnitieerd modus**, Kopieer de **id** waarde voor uw exemplaar en plak deze in **id** textbox in  **123ContactForm domein en de URL's** sectie op Azure-portal.
    
    b. Als u wilt configureren van de toepassing in **IDP geïnitieerd modus**, Kopieer de **antwoord-URL** waarde voor uw exemplaar en plak deze in **antwoord-URL** textbox in  **123ContactForm domein en de URL's** sectie op Azure-portal.

    c. Als u wilt configureren van de toepassing in **SP geïnitieerd modus**, exemplaar de **SIGN-ON-URL** waarde voor uw exemplaar en plak deze in **aanmelding op URL** textbox in  **123ContactForm domein en de URL's** sectie op Azure-portal.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-123contactform-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-123contactform-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-123contactform-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-123contactform-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-123contactform-test-user"></a>Een testgebruiker 123ContactForm maken

Toepassing ondersteunt Just in tijd gebruikers inrichten en na verificatie gebruikers wordt in de toepassing automatisch gemaakt.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot 123ContactForm.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen 123ContactForm, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **123ContactForm**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel 123ContactForm in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing 123ContactForm.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_203.png

