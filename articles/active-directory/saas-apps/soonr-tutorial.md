---
title: 'Zelfstudie: Azure Active Directory-integratie met Soonr werkplek | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Soonr werkplek.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: b75f5f00-ea8b-4850-ae2e-134e5d678d97
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: jeedes
ms.openlocfilehash: 7dd02aee83d42421cdda57f4feb54dde790db68c
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54809150"
---
# <a name="tutorial-azure-active-directory-integration-with-soonr-workplace"></a>Zelfstudie: Azure Active Directory-integratie met Soonr werkplek

In deze zelfstudie leert u hoe werkplek Soonr integreren met Azure Active Directory (Azure AD).

Soonr werkplek integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Soonr werkplek heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Soonr werkplek (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Soonr werkplek, moet u de volgende items:

- Een Azure AD-abonnement
- Een Soonr werkplek eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Soonr werkplek toevoegen vanuit de galerie
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-soonr-workplace-from-the-gallery"></a>Soonr werkplek toevoegen vanuit de galerie
Voor het configureren van de integratie van Soonr werkplek in Azure AD, moet u Soonr werkplek toevoegen uit de galerie aan de lijst met beheerde SaaS-apps.

**Als u wilt Soonr werkplek toevoegen vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **Soonr werkplek**.

    ![Het maken van een Azure AD-testgebruiker](./media/soonr-tutorial/tutorial_soonr_search.png)

1. Selecteer in het deelvenster resultaten **Soonr werkplek**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/soonr-tutorial/tutorial_soonr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie configureert u en test Azure AD eenmalige aanmelding met Soonr werkplek op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Soonr werkplek is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Soonr werkplek tot stand worden gebracht.

In de Soonr werkplek, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Soonr werkplek, u nodig hebt voor de volgende bouwstenen:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker Soonr werkplek](#creating-a-soonr-workplace-test-user)**  : als u wilt een equivalent van Britta Simon in Soonr werkplek die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Soonr werkplek.

**Voor het configureren van Azure AD eenmalige aanmelding met Soonr werkplek, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Soonr werkplek** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/soonr-tutorial/tutorial_soonr_samlbase.png)

1. Op de **Soonr werkplek domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/soonr-tutorial/tutorial_soonr_url.png)

    a. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<servername>.soonr.com/singlesignon/saml/metadata`

    b. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://<servername>.soonr.com/singlesignon/saml/SSO`

1. Op de **Soonr werkplek domein en URL's** sectie, als u wilt configureren van de toepassing in **SP geïnitieerde modus**, voer de volgende stappen uit:
    
    ![Eenmalige aanmelding configureren](./media/soonr-tutorial/tutorial_soonr_url1.png)

    a. Klik op de **geavanceerde URL-instellingen weergeven**.

    b. In de **aanmelding URL** tekstvak, een URL met behulp van het volgende patroon: `https://<servername>.soonr.com/singlesignon/saml/SSO`

    > [!NOTE] 
    > Dit zijn geen echte waarden. Deze waarden bijwerken met de werkelijke-id en meld u aan bij de URL en antwoord-URL. Neem contact op met [Soonr werkplek ondersteuningsteam](https://awp.autotask.net/help/) om deze waarden te verkrijgen.
 
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/soonr-tutorial/tutorial_soonr_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/soonr-tutorial/tutorial_general_400.png)

1. Op de **Soonr werkplek configuratie** sectie, klikt u op **configureren Soonr werkplek** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/soonr-tutorial/tutorial_soonr_configure.png) 

1. Het configureren van eenmalige aanmelding op **Soonr werkplek** zijde, moet u voor het verzenden van de gedownloade **Metadata XML**, **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** naar [Soonr werkplek ondersteuningsteam](https://awp.autotask.net/help/). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

    >[!Note]
    >Als u hulp bij het configureren van Autotask werkplek nodig hebt, raadpleegt u [deze pagina](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) hulp met uw werkplek te krijgen.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/soonr-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/soonr-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/soonr-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/soonr-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-soonr-workplace-test-user"></a>Het maken van een testgebruiker Soonr werkplek

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Soonr werkplek. Werken met [Soonr werkplek ondersteuningsteam](https://awp.autotask.net/help/) te maken van een gebruiker in het platform. U kunt de ondersteuningsticket met Soonr van verhogen <a href="https://na01.safelinks.protection.outlook.com/?url=http%3A%2F%2Fsoonr.com%2FAWPHelp%2FContent%2F0_HOME%2FSupport_for_End_Clients.htm&data=01%7C01%7Cv-saikra%40microsoft.com%7Ccbb4367ab09b4dacaac408d3eebe3f42%7C72f988bf86f141af91ab2d7cd011db47%7C1&sdata=FB92qtE6m%2Fd8yox7AnL2f1h%2FGXwSkma9x9H8Pz0955M%3D&reserved=0/">hier</a>.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan de werkplek Soonr.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon Soonr werkplek koppelen, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Soonr werkplek**.

    ![Eenmalige aanmelding configureren](./media/soonr-tutorial/tutorial_soonr_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie via het toegangsvenster.  

Wanneer u op de tegel Soonr werkplek in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Soonr werkplek.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/soonr-tutorial/tutorial_general_01.png
[2]: ./media/soonr-tutorial/tutorial_general_02.png
[3]: ./media/soonr-tutorial/tutorial_general_03.png
[4]: ./media/soonr-tutorial/tutorial_general_04.png

[100]: ./media/soonr-tutorial/tutorial_general_100.png

[200]: ./media/soonr-tutorial/tutorial_general_200.png
[201]: ./media/soonr-tutorial/tutorial_general_201.png
[202]: ./media/soonr-tutorial/tutorial_general_202.png
[203]: ./media/soonr-tutorial/tutorial_general_203.png

