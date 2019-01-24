---
title: 'Zelfstudie: Azure Active Directory-integratie met Questetra BPM Suite | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Questetra BPM Suite.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: fb6d5b73-e491-4dd2-92d6-94e5aba21465
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2017
ms.author: jeedes
ms.openlocfilehash: cc2d88bfc6b8ce57cebc2e35e3a9f2e3b826e9cd
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54816769"
---
# <a name="tutorial-azure-active-directory-integration-with-questetra-bpm-suite"></a>Zelfstudie: Azure Active Directory-integratie met Questetra BPM Suite

In deze zelfstudie leert u hoe u Questetra BPM Suite integreren met Azure Active Directory (Azure AD).

Questetra BPM Suite integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Questetra BPM Suite heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Questetra BPM Suite (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Questetra BPM Suite, moet u de volgende items:

- Een Azure AD-abonnement
- Een Questetra BPM Suite eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Questetra BPM Suite uit de galerie toevoegen
1. Azure AD-eenmalige aanmelding configureren en testen

## <a name="add-questetra-bpm-suite-from-the-gallery"></a>Questetra BPM Suite uit de galerie toevoegen
Voor het configureren van de integratie van Questetra BPM Suite in Azure AD, moet u Questetra BPM Suite uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Questetra BPM Suite uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **Questetra BPM Suite**, selecteer **Questetra BPM Suite** van resultaat deelvenster en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Uit de galerie toevoegen](./media/questetra-bpm-suite-tutorial/tutorial_questetra-bpm-suite_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Questetra BPM Suite op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Questetra BPM Suite is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Questetra BPM Suite tot stand worden gebracht.

In Questetra BPM Suite, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Questetra BPM Suite, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker Questetra BPM Suite](#create-a-questetra-bpm-suite-test-user)**  : als u wilt een equivalent van Britta Simon in Questetra BPM Suite die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Questetra BPM Suite.

**Voor het configureren van Azure AD eenmalige aanmelding met Questetra BPM Suite, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Questetra BPM Suite** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Op SAML gebaseerde aanmelding](./media/questetra-bpm-suite-tutorial/tutorial_questetra-bpm-suite_samlbase.png)

1. Op de **Questetra BPM Suite domein en URL's** sectie, voert u de volgende stappen uit:

    ![Sectie Questetra BPM Suite-domein en URL 's](./media/questetra-bpm-suite-tutorial/tutorial_questetra-bpm-suite_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<subdomain>.questetra.net/saml/SSO/alias/bpm`

    b. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<subdomain>.questetra.net/`

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de daadwerkelijke aanmeldings-URL en id. U kunt deze waarden ophalen uit **SP informatie** sectie op uw **Questetra BPM Suite** bedrijf site, die verderop in de zelfstudie of neem contact op met wordt [clientondersteuning Questetra BPM Suite team](https://www.questetra.com/contact/). 
 
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base 64)** en slaat u het certificaatbestand op uw computer.

    ![Sectie voor SAML-handtekeningcertificaat](./media/questetra-bpm-suite-tutorial/tutorial_questetra-bpm-suite_certificate.png) 

1. Klik op de knop **Save**.

    ![De knop Opslaan](./media/questetra-bpm-suite-tutorial/tutorial_general_400.png)

1. Op de **configuratie van de Suite Questetra BPM** sectie, klikt u op **Questetra BPM-pakket configureren** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Configuratiesectie Questetra BPM Suite](./media/questetra-bpm-suite-tutorial/tutorial_questetra-bpm-suite_configure.png) 

1. In een ander browservenster, meld u aan bij uw **Questetra BPM Suite** bedrijf site als beheerder.

1. Klik in het menu aan de bovenkant op **systeeminstellingen**. 
   
    ![Azure AD voor eenmalige aanmelding][10]

1. Om te openen de **SingleSignOnSAML** pagina, klikt u op **SSO (SAML)**. 
   
    ![Azure AD voor eenmalige aanmelding][11]

1. Op uw **Questetra BPM Suite** bedrijf site, in de **SP informatie** sectie, voert u de volgende stappen uit:

    a. Kopiëren de **ACS URL**, en plak deze in de **aanmelding URL** -tekstvak in de **Questetra BPM Suite domein en URL's** sectie in Azure portal.
    
    b. Kopiëren de **entiteit-ID**, en plak deze in de **id** -tekstvak in de **Questetra BPM Suite domein en URL's** sectie in Azure portal.

1. Op uw **Questetra BPM Suite** site van de bedrijfsportal, voert u de volgende stappen uit: 
   
    ![Eenmalige aanmelding configureren][15]
   
    a. Selecteer **eenmalige aanmelding inschakelen**.
   
    b. In **entiteit-ID** tekstvak, plak de waarde van **SAML entiteit-ID** die u hebt gekopieerd vanuit Azure portal.
    
    c. In **aanmelden pagina-URL** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML** die u hebt gekopieerd vanuit Azure portal.
    
    d. In **URL voor afmelden pagina** tekstvak, plak de waarde van **afmelding URL** die u hebt gekopieerd vanuit Azure portal.
    
    e. In de **NameID-indeling** tekstvak, type `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`.

    f. Open uw **Base-64** gecodeerd certificaat in Kladblok gedownload vanuit Azure portal, de inhoud van het kopiëren naar het Klembord en plak deze in de **validatiecertificaat** tekstvak. 

    g. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/questetra-bpm-suite-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/questetra-bpm-suite-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/questetra-bpm-suite-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/questetra-bpm-suite-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="create-a-questetra-bpm-suite-test-user"></a>Maak een testgebruiker Questetra BPM Suite

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Questetra BPM Suite.

**Voor het maken van een gebruiker met de naam van Britta Simon in Questetra BPM Suite, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf Questetra BPM Suite site als beheerder.
1. Ga naar **systeeminstellingen > lijst met gebruikers > nieuwe gebruiker**. 
1. Voer in het dialoogvenster Nieuwe gebruiker de volgende stappen uit: 
   
    ![Testgebruiker maken][300] 
   
    a. In de **naam** tekstvak, type **naam** van de gebruiker **britta.simon@contoso.com**.
   
    b. In de **e** tekstvak, type **e** van de gebruiker **britta.simon@contoso.com**
   
    c. In de **wachtwoord** tekstvak, typ een **wachtwoord** van de gebruiker.
    
    d. Klik op **nieuwe gebruiker toevoegen**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot Questetra BPM Suite.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon Questetra BPM Suite, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Questetra BPM Suite**.

    ![Questetra BPM Suite in de lijst met apps](./media/questetra-bpm-suite-tutorial/tutorial_questetra-bpm-suite_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Questetra BPM Suite in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Questetra BPM Suite.
Zie [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/questetra-bpm-suite-tutorial/tutorial_general_01.png
[2]: ./media/questetra-bpm-suite-tutorial/tutorial_general_02.png
[3]: ./media/questetra-bpm-suite-tutorial/tutorial_general_03.png
[4]: ./media/questetra-bpm-suite-tutorial/tutorial_general_04.png
[10]: ./media/questetra-bpm-suite-tutorial/questera_bpm_suite_03.png
[11]: ./media/questetra-bpm-suite-tutorial/questera_bpm_suite_04.png
[15]: ./media/questetra-bpm-suite-tutorial/questera_bpm_suite_08.png

[100]: ./media/questetra-bpm-suite-tutorial/tutorial_general_100.png

[200]: ./media/questetra-bpm-suite-tutorial/tutorial_general_200.png
[201]: ./media/questetra-bpm-suite-tutorial/tutorial_general_201.png
[202]: ./media/questetra-bpm-suite-tutorial/tutorial_general_202.png
[203]: ./media/questetra-bpm-suite-tutorial/tutorial_general_203.png
[300]: ./media/questetra-bpm-suite-tutorial/questera_bpm_suite_11.png 

