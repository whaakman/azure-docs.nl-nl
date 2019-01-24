---
title: 'Zelfstudie: Azure Active Directory-integratie met kleine verbeteringen | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory- en kleine verbeteringen.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 59c8a112-41e1-4337-9ef3-3d7029780d61
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 27f94ef7fb41315ea11f87adf8a4f7f026d584af
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54818656"
---
# <a name="tutorial-azure-active-directory-integration-with-small-improvements"></a>Zelfstudie: Azure Active Directory-integratie met kleine verbeteringen

In deze zelfstudie leert u hoe u kleine verbeteringen integreren met Azure Active Directory (Azure AD).

Kleine verbeteringen integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot kleine verbeteringen heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij kleine verbeteringen (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met kleine verbeteringen, moet u de volgende items:

- Een Azure AD-abonnement
- Een kleine verbeteringen eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand hier [proefversie aanbieding](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Kleine verbeteringen in de galerie toevoegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-small-improvements-from-the-gallery"></a>Kleine verbeteringen in de galerie toevoegen
Voor het configureren van de integratie van kleine verbeteringen in Azure AD, moet u kleine verbeteringen in de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen kleine verbeteringen in de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **kleine verbeteringen**.

    ![Het maken van een Azure AD-testgebruiker](./media/smallimprovements-tutorial/tutorial_smallimprovements_search.png)

1. Selecteer in het deelvenster resultaten **kleine verbeteringen**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/smallimprovements-tutorial/tutorial_smallimprovements_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met kleine verbeteringen op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in kleine verbeteringen is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in kleine verbeteringen tot stand worden gebracht.

In kleine verbeteringen, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met kleine verbeteringen, u nodig hebt voor de volgende bouwstenen:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker kleine verbeteringen](#creating-a-small-improvements-test-user)**  : als u wilt een equivalent van Britta Simon in kleine verbeteringen die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u Azure AD eenmalige aanmelding in de Azure-portal inschakelen en configureren van eenmalige aanmelding in uw toepassing kleine verbeteringen.

**Voor het configureren van Azure AD eenmalige aanmelding met kleine verbeteringen, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **kleine verbeteringen** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/smallimprovements-tutorial/tutorial_smallimprovements_samlbase.png)

1. Op de **kleine verbeteringen domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/smallimprovements-tutorial/tutorial_smallimprovements_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<subdomain>.small-improvements.com`

    b. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<subdomain>.small-improvements.com`

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de daadwerkelijke aanmeldings-URL en id. Neem contact op met [kleine verbeteringen Client ondersteuningsteam](mailto:support@small-improvements.com) om deze waarden te verkrijgen. 
 
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/smallimprovements-tutorial/tutorial_smallimprovements_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/smallimprovements-tutorial/tutorial_general_400.png)

1. Op de **kleine verbeteringen configuratie** sectie, klikt u op **kleine verbeteringen configureren** openen **aanmelding configureren** venster. Kopiëren de **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/smallimprovements-tutorial/tutorial_smallimprovements_configure.png) 

1. In een ander browservenster, meld u aan bij uw bedrijf kleine verbeteringen site als beheerder.

1. Klik op de pagina hoofddashboard **beheer** knop aan de linkerkant.
   
    ![Eenmalige aanmelding configureren](./media/smallimprovements-tutorial/tutorial_smallimprovements_06.png) 

1. Klik op de **SAML SSO** knop van **integraties** sectie.
   
    ![Eenmalige aanmelding configureren](./media/smallimprovements-tutorial/tutorial_smallimprovements_07.png) 

1. Voer de volgende stappen uit op de pagina instellingen voor eenmalige aanmelding:
   
    ![Eenmalige aanmelding configureren](./media/smallimprovements-tutorial/tutorial_smallimprovements_08.png)  

    a. In de **HTTP-eindpunt** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML**, die u hebt gekopieerd vanuit Azure portal.

    b. Open het gedownloade certificaat in Kladblok, Kopieer de inhoud en plak deze in de **x509 certificaat** tekstvak. 

    c. Als u dat eenmalige aanmelding en meld u aan formulier optie voor verificatie beschikbaar voor gebruikers wilt, controleert u de **toegang via aanmelding en wachtwoord te inschakelen** optie.  

    d. Voer de juiste waarde als naam van de knop Aanmelden voor eenmalige aanmelding in de **SAML vragen** tekstvak.  

    e. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/smallimprovements-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/smallimprovements-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/smallimprovements-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/smallimprovements-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-small-improvements-test-user"></a>Het maken van een testgebruiker kleine verbeteringen

Als u wilt dat Azure AD-gebruikers zich aanmelden bij kleine verbeteringen, moeten ze worden ingericht voor kleine verbeteringen. In het geval van kleine verbeteringen is inrichting een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Aanmelding bij uw bedrijf kleine verbeteringen site als beheerder.

1. Vanaf de startpagina, gaat u naar het menu aan de linkerkant, klik op **beheer**.

1. Klik op de **gebruikerslijst** knop in de sectie beheer van gebruikers. 
   
    ![Het maken van een Azure AD-testgebruiker](./media/smallimprovements-tutorial/tutorial_smallimprovements_10.png) 

1. Klik op **gebruikers toevoegen**.

    ![Het maken van een Azure AD-testgebruiker](./media/smallimprovements-tutorial/tutorial_smallimprovements_11.png) 

1. Op de **gebruikers toevoegen** dialoogvenster, voer de volgende stappen uit: 

    ![Het maken van een Azure AD-testgebruiker](./media/smallimprovements-tutorial/tutorial_smallimprovements_12.png)
    
    a. Voer de **voornaam** van gebruiker, zoals **Julia**.

    b. Voer de **achternaam** van gebruiker, zoals **Simon**.

    c. Voer de **e** van gebruiker, zoals <strong>brittasimon@contoso.com</strong>. 

    d. U kunt er ook voor kiezen om in te voeren van het persoonlijke bericht in de **e-mailmelding verzenden** vak. Als u niet verzenden van de melding wilt, schakel dit selectievakje in.

    e. Klik op **maken gebruikers**.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding toegang verlenen tot kleine verbeteringen.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon kleine verbeteringen, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **kleine verbeteringen**.

    ![Eenmalige aanmelding configureren](./media/smallimprovements-tutorial/tutorial_smallimprovements_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van de configuratie van uw Azure AD-eenmalige aanmelding via het toegangsvenster.  

Wanneer u op de tegel kleine verbeteringen in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing kleine verbeteringen.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/smallimprovements-tutorial/tutorial_general_01.png
[2]: ./media/smallimprovements-tutorial/tutorial_general_02.png
[3]: ./media/smallimprovements-tutorial/tutorial_general_03.png
[4]: ./media/smallimprovements-tutorial/tutorial_general_04.png

[100]: ./media/smallimprovements-tutorial/tutorial_general_100.png

[200]: ./media/smallimprovements-tutorial/tutorial_general_200.png
[201]: ./media/smallimprovements-tutorial/tutorial_general_201.png
[202]: ./media/smallimprovements-tutorial/tutorial_general_202.png
[203]: ./media/smallimprovements-tutorial/tutorial_general_203.png

