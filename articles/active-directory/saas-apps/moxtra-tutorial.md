---
title: 'Zelfstudie: Azure Active Directory-integratie met Moxtra | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Moxtra.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 2aed2d4b-1dcd-4839-8fed-9419d107c61c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: ee8931f1f9121f3e645b2f94eece919ae6b19075
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54808847"
---
# <a name="tutorial-azure-active-directory-integration-with-moxtra"></a>Zelfstudie: Azure Active Directory-integratie met Moxtra

In deze zelfstudie leert u hoe u Moxtra integreren met Azure Active Directory (Azure AD).

Moxtra integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Moxtra heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Moxtra (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Moxtra, moet u de volgende items:

- Een Azure AD-abonnement
- Een Moxtra eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Moxtra uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-moxtra-from-the-gallery"></a>Moxtra uit de galerie toe te voegen
Voor het configureren van de integratie van Moxtra in Azure AD, moet u Moxtra uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Moxtra uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **Moxtra**.

    ![Het maken van een Azure AD-testgebruiker](./media/moxtra-tutorial/tutorial_moxtra_search.png)

1. Selecteer in het deelvenster resultaten **Moxtra**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/moxtra-tutorial/tutorial_moxtra_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Moxtra op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Moxtra is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Moxtra tot stand worden gebracht.

In Moxtra, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Moxtra, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker Moxtra](#creating-a-moxtra-test-user)**  : als u wilt een equivalent van Britta Simon in Moxtra die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Moxtra.

**Voor het configureren van Azure AD eenmalige aanmelding met Moxtra, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Moxtra** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/moxtra-tutorial/tutorial_moxtra_samlbase.png)

1. Op de **Moxtra domein en URL's** sectie, voert u de volgende stap:

    ![Eenmalige aanmelding configureren](./media/moxtra-tutorial/tutorial_moxtra_url.png)

    In de **aanmeldings-URL** tekstvak, een URL als: `https://www.moxtra.com/service/#login`

1. Moxtra toepassing verwacht het SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de "**gebruikerskenmerken**" sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze configuratie. 

    ![Eenmalige aanmelding configureren](./media/moxtra-tutorial/tutorial_moxtra_attributes.png)
    
1. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding en voer de volgende stappen uit:
    
    | Naam kenmerk | Waarde kenmerk |
    | ------------------- | -------------------- |    
    | firstname | user.givenname |
    | lastname | user.surname |
    | idpid    | < SAML entiteit-ID > 

    > [!Note]
    > De waarde van **idpid** kenmerk is niet echt. Krijgt u de werkelijke waarde van **Naslaggids** sectie onder **Moxtra configuratie**.
    
    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/moxtra-tutorial/tutorial_attribute_04.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    ![Eenmalige aanmelding configureren](./media/moxtra-tutorial/tutorial_attribute_05.png)

    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Klik op **OK**.
    
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/moxtra-tutorial/tutorial_moxtra_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/moxtra-tutorial/tutorial_general_400.png)

1. Op de **Moxtra configuratie** sectie, klikt u op **configureren Moxtra** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/moxtra-tutorial/tutorial_moxtra_configure.png) 

1. In een ander browservenster, meld u aan bij uw bedrijf Moxtra site als beheerder.

1. Klik in de werkbalk aan de linkerkant op **-beheerconsole > SAML Single Sign-on**, en klik vervolgens op **nieuw**.
   
    ![Eenmalige aanmelding configureren](./media/moxtra-tutorial/tutorial_moxtra_06.png) 

1. Op de **SAML** pagina, voert u de volgende stappen uit:
   
    ![Eenmalige aanmelding configureren](./media/moxtra-tutorial/tutorial_moxtra_08.png)   
 
    a. In de **naam** tekstvak, typ een naam voor uw configuratie (bijvoorbeeld: *SAML*). 
  
    b. In de **IdP entiteit-ID** tekstvak, plak de waarde van **SAML entiteit-ID** die u hebt gekopieerd vanuit Azure portal. 
 
    c. In **aanmeldings-URL** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML** die u hebt gekopieerd vanuit Azure portal. 
 
    d. In de **AuthnContextClassRef** tekstvak, type **urn: oasis: namen: tc: SAML:2.0:ac:classes:Password**. 
 
    e. In de **NameID-indeling** tekstvak, type **urn: oasis: namen: tc: SAML:1.1:nameid-indeling: emailAddress**. 
 
    f. Open-certificaat dat u hebt gedownload vanuit Azure portal in Kladblok en kopieer de inhoud en plak deze in de **certificaat** tekstvak.    
 
    g. Typ uw e-maildomein van SAML in het tekstvak SAML e-domein.    
  
    >[!NOTE]
    >Voor de stappen om te controleren of het domein, klikt u op de '**ik**' hieronder.

    h. Klik op **Update**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/moxtra-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/moxtra-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/moxtra-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/moxtra-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-moxtra-test-user"></a>Het maken van een testgebruiker Moxtra

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Moxtra.

**Voor het maken van een gebruiker met de naam van Britta Simon in Moxtra, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf Moxtra site als beheerder.

1. Klik in de werkbalk aan de linkerkant op **-beheerconsole > Gebruikersbeheer**, en vervolgens **gebruiker toevoegen**.
   
    ![Eenmalige aanmelding configureren](./media/moxtra-tutorial/tutorial_moxtra_10.png) 

1. Op de **gebruiker toevoegen** dialoogvenster, voer de volgende stappen uit:
  
    a. In de **voornaam** tekstvak, type **Julia**.
  
    b. In de **achternaam** tekstvak, type **Simon**.
  
    c. In de **e** tekstvak, type Julia van e-mailadres dat hetzelfde als in Azure portal.
  
    d. In de **deling** tekstvak, type **Dev**.
  
    e. In de **afdeling** tekstvak, type **IT**.
  
    f. Selecteer **beheerder**.
  
    g. Klik op **Add**.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Moxtra.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan Moxtra toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Moxtra**.

    ![Eenmalige aanmelding configureren](./media/moxtra-tutorial/tutorial_moxtra_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Moxtra in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Moxtra.
Zie [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/moxtra-tutorial/tutorial_general_01.png
[2]: ./media/moxtra-tutorial/tutorial_general_02.png
[3]: ./media/moxtra-tutorial/tutorial_general_03.png
[4]: ./media/moxtra-tutorial/tutorial_general_04.png

[100]: ./media/moxtra-tutorial/tutorial_general_100.png

[200]: ./media/moxtra-tutorial/tutorial_general_200.png
[201]: ./media/moxtra-tutorial/tutorial_general_201.png
[202]: ./media/moxtra-tutorial/tutorial_general_202.png
[203]: ./media/moxtra-tutorial/tutorial_general_203.png

