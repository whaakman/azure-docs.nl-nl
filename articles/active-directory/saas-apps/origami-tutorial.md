---
title: 'Zelfstudie: Azure Active Directory-integratie met Origami | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Origami.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: a28bb0ba-b564-46ba-accc-e587699295d4
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: e8c7dbb2b50e6c6c00b857e419f4e238e318a537
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54809221"
---
# <a name="tutorial-azure-active-directory-integration-with-origami"></a>Zelfstudie: Azure Active Directory-integratie met Origami

In deze zelfstudie leert u hoe u Origami integreren met Azure Active Directory (Azure AD).

Origami integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Origami heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Origami (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Origami, moet u de volgende items:

- Een Azure AD-abonnement
- Een Origami eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Origami uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-origami-from-the-gallery"></a>Origami uit de galerie toe te voegen
Voor het configureren van de integratie van Origami in Azure AD, moet u Origami uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Origami uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **Origami**.

    ![Het maken van een Azure AD-testgebruiker](./media/origami-tutorial/tutorial_origami_search.png)

1. Selecteer in het deelvenster resultaten **Origami**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/origami-tutorial/tutorial_origami_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Origami op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Origami is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Origami tot stand worden gebracht.

In Origami, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Origami, u nodig hebt voor de volgende bouwstenen:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker Origami](#creating-an-origami-test-user)**  : als u wilt een equivalent van Britta Simon in Origami die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Origami.

**Voor het configureren van Azure AD eenmalige aanmelding met Origami, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Origami** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/origami-tutorial/tutorial_origami_samlbase.png)

1. Op de **Origami domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/origami-tutorial/tutorial_origami_url.png)

    Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://live.origamirisk.com/origami/account/login?account=<companyname>`

    > [!NOTE] 
    > De waarde is niet echt. Werk de waarde bij met de werkelijke aanmeldings-URL. Neem contact op met [Origami Client ondersteuningsteam](https://wordpress.org/support/theme/origami) om de waarde. 
 
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/origami-tutorial/tutorial_origami_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/origami-tutorial/tutorial_general_400.png)

1. Op de **Origami configuratie** sectie, klikt u op **configureren Origami** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/origami-tutorial/tutorial_origami_configure.png) 

1. Meld u aan bij de Origami-account met beheerdersrechten.

1. Klik in het menu bovenaan op **Admin**.
   
    ![Eenmalige aanmelding configureren](./media/origami-tutorial/tutorial_origami_51.png)

1. Voer de volgende stappen uit op de pagina van het dialoogvenster één teken op instellingen:
   
    ![Eenmalige aanmelding configureren](./media/origami-tutorial/tutorial_origami_531.png)

    a. Selecteer **eenmalige aanmelding inschakelen**.

    b. In de **id-Provider aanmelden pagina-URL** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML**, die u hebt gekopieerd vanuit Azure portal.

    c. In de **URL van id-Provider afmelding** tekstvak, plak de waarde van **afmelding URL**, die u hebt gekopieerd vanuit Azure portal.

    d. Klik op **Bladeren** voor het uploaden van het certificaat dat u hebt gedownload vanuit Azure portal.

    e. Klik op **Wijzigingen opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/origami-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/origami-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/origami-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/origami-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-an-origami-test-user"></a>Het maken van een testgebruiker Origami

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Origami. 

1. Meld u aan bij de Origami-account met beheerdersrechten.

1. Klik in het menu bovenaan op **Admin**.
   
    ![Eenmalige aanmelding configureren](./media/origami-tutorial/tutorial_origami_51.png)

1. Op de **gebruikers en beveiliging** dialoogvenster, klikt u op **gebruikers**.
   
    ![Eenmalige aanmelding configureren](./media/origami-tutorial/tutorial_origami_54.png)

1. Klik op **Add New User**.
   
    ![Eenmalige aanmelding configureren](./media/origami-tutorial/tutorial_origami_55.png)

1. In het dialoogvenster Nieuwe gebruiker toevoegen de volgende stappen uitvoeren:
   
    ![Eenmalige aanmelding configureren](./media/origami-tutorial/tutorial_origami_56.png)

    a. In de **gebruikersnaam** tekstvak, voer het e-mailadres van gebruiker, zoals **brittasimon@contoso.com**.

    b. In de **wachtwoord** tekstvak, typ een wachtwoord.

    c. In de **wachtwoord bevestigen** tekstvak typt u het wachtwoord opnieuw.

    d. In de **voornaam** tekstvak, geef de voornaam van de gebruiker, zoals **Julia**.

    e. In de **achternaam** tekstvak, geef de achternaam van de gebruiker, zoals **Simon**.

    f. Klik op **Opslaan**.
   
    ![Eenmalige aanmelding configureren](./media/origami-tutorial/tutorial_origami_57.png)

1. Toewijzen **gebruikersrollen** en **clienttoegang** aan de gebruiker. 
   
    ![Eenmalige aanmelding configureren](./media/origami-tutorial/tutorial_origami_58.png)

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Origami.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan Origami toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Origami**.

    ![Eenmalige aanmelding configureren](./media/origami-tutorial/tutorial_origami_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Origami in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Origami.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/origami-tutorial/tutorial_general_01.png
[2]: ./media/origami-tutorial/tutorial_general_02.png
[3]: ./media/origami-tutorial/tutorial_general_03.png
[4]: ./media/origami-tutorial/tutorial_general_04.png

[100]: ./media/origami-tutorial/tutorial_general_100.png

[200]: ./media/origami-tutorial/tutorial_general_200.png
[201]: ./media/origami-tutorial/tutorial_general_201.png
[202]: ./media/origami-tutorial/tutorial_general_202.png
[203]: ./media/origami-tutorial/tutorial_general_203.png

