---
title: 'Zelfstudie: Azure Active Directory-integratie met Samanage | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Samanage.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: f0db4fb0-7eec-48c2-9c7a-beab1ab49bc2
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 118ab72c9afc13c5792f229f9c7bc61d226553d5
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39420571"
---
# <a name="tutorial-azure-active-directory-integration-with-samanage"></a>Zelfstudie: Azure Active Directory-integratie met Samanage

In deze zelfstudie leert u hoe u Samanage integreren met Azure Active Directory (Azure AD).

Samanage integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Samanage heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Samanage (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Samanage, moet u de volgende items:

- Een Azure AD-abonnement
- Een Samanage eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Samanage uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-samanage-from-the-gallery"></a>Samanage uit de galerie toe te voegen
Voor het configureren van de integratie van Samanage in Azure AD, moet u Samanage uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Samanage uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

1. Typ in het zoekvak **Samanage**.

    ![Het maken van een Azure AD-testgebruiker](./media/samanage-tutorial/tutorial_samanage_search.png)

1. Selecteer in het deelvenster resultaten **Samanage**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/samanage-tutorial/tutorial_samanage_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Samanage op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Samanage is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Samanage tot stand worden gebracht.

In Samanage, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Samanage, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker Samanage](#creating-a-samanage-test-user)**  : als u wilt een equivalent van Britta Simon in Samanage die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Samanage.

**Voor het configureren van Azure AD eenmalige aanmelding met Samanage, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Samanage** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/samanage-tutorial/tutorial_samanage_samlbase.png)

1. Op de **Samanage domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/samanage-tutorial/tutorial_samanage_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<Company Name>.samanage.com/saml_login/<Company Name>`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<Company Name>.samanage.com`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL en de id, die later in de zelfstudie wordt uitgelegd. Voor meer informatie contact op met [Samanage Client ondersteuningsteam](https://www.samanage.com/support).    
 
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/samanage-tutorial/tutorial_samanage_certificate.png) 

1. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/samanage-tutorial/tutorial_general_400.png)

1. Op de **Samanage configuratie** sectie, klikt u op **configureren Samanage** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL en SAML entiteit-ID** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/samanage-tutorial/tutorial_samanage_configure.png) 

1. Meld u in een ander browservenster in uw bedrijf Samanage site als beheerder.

1. Klik op **Dashboard** en selecteer **Setup** in het navigatiedeelvenster links.
   
    ![Dashboard](./media/samanage-tutorial/tutorial_samanage_001.png "Dashboard")

1. Klik op **Single Sign-On**.
   
    ![Single Sign-On](./media/samanage-tutorial/tutorial_samanage_002.png "Single Sign-On")

1. Navigeer naar **Meld u aan met SAML** sectie, voert u de volgende stappen uit:
   
    ![Meld u aan met SAML](./media/samanage-tutorial/tutorial_samanage_003.png "Meld u aan met SAML")
 
    a. Klik op **eenmalige aanmelding met SAML inschakelen**.  
 
    b. In de **URL van de id-Provider** tekstvak, plak de waarde van **SAML entiteit-ID** die u hebt gekopieerd vanuit Azure portal.    
 
    c. Controleer of de **aanmeldings-URL** komt overeen met de **aanmelding URL** van **Samanage domein en URL's** sectie in Azure portal.
 
    d. In de **afmeldings-URL van** tekstvak, voer de waarde van **afmelding URL** die u hebt gekopieerd vanuit Azure portal.
 
    e. In de **SAML verlener** tekstvak, type de URI van de app-id instellen in uw id-provider.
 
    f. Open uw base-64 gecodeerde certificaat gedownload vanuit Azure portal in Kladblok, Kopieer de inhoud ervan in het Klembord en plakt u deze naar de **plak uw id-Provider x.509 certificaat onderstaande** tekstvak.
 
    g. Klik op **gebruikers maken als deze nog niet bestaan in Samanage**.
 
    h. Klik op **Update**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
 
### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/samanage-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/samanage-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/samanage-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/samanage-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-samanage-test-user"></a>Het maken van een testgebruiker Samanage

Als u wilt dat Azure AD-gebruikers zich aanmelden bij Samanage, moeten ze worden ingericht voor Samanage.  
In het geval van Samanage is inrichten een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u in uw bedrijf Samanage site als beheerder.

1. Klik op **Dashboard** en selecteer **Setup** in de navigatiebalk aan de linkerkant pannen.
   
    ![Setup](./media/samanage-tutorial/tutorial_samanage_001.png "Setup")

1. Klik op de **gebruikers** tabblad
   
    ![Gebruikers](./media/samanage-tutorial/tutorial_samanage_006.png "gebruikers")

1. Klik op **nieuwe gebruiker**.
   
    ![Nieuwe gebruiker](./media/samanage-tutorial/tutorial_samanage_007.png "nieuwe gebruiker")

1. Type de **naam** en de **e-mailadres** van een Azure Active Directory-account dat u wilt inrichten en klikt u op **gebruiker maken**.
   
    ![Gebruiker maken](./media/samanage-tutorial/tutorial_samanage_008.png "gebruiker maken")
   
   >[!NOTE]
   >De houder van Azure Active Directory-account ontvangt een e-mailbericht en gaat u als volgt een koppeling om te bevestigen van hun account voordat deze geactiveerd wordt. U kunt elke andere Samanage gebruiker account hulpmiddelen voor het maken of API's geleverd door Samanage voor het inrichten van Azure Active Directory-gebruikersaccounts.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Samanage.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan Samanage toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Samanage**.

    ![Eenmalige aanmelding configureren](./media/samanage-tutorial/tutorial_samanage_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Samanage in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Samanage.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/samanage-tutorial/tutorial_general_01.png
[2]: ./media/samanage-tutorial/tutorial_general_02.png
[3]: ./media/samanage-tutorial/tutorial_general_03.png
[4]: ./media/samanage-tutorial/tutorial_general_04.png

[100]: ./media/samanage-tutorial/tutorial_general_100.png

[200]: ./media/samanage-tutorial/tutorial_general_200.png
[201]: ./media/samanage-tutorial/tutorial_general_201.png
[202]: ./media/samanage-tutorial/tutorial_general_202.png
[203]: ./media/samanage-tutorial/tutorial_general_203.png

