---
title: 'Zelfstudie: Azure Active Directory-integratie met Picturepark | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Picturepark.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 31c21cd4-9c00-4cad-9538-a13996dc872f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: jeedes
ms.openlocfilehash: 87b910b627b6d9549348851fe98e2037e87317c0
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54821597"
---
# <a name="tutorial-azure-active-directory-integration-with-picturepark"></a>Zelfstudie: Azure Active Directory-integratie met Picturepark

In deze zelfstudie leert u hoe u Picturepark integreren met Azure Active Directory (Azure AD).

Picturepark integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Picturepark heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Picturepark (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Picturepark, moet u de volgende items:

- Een Azure AD-abonnement
- Een Picturepark eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Picturepark uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-picturepark-from-the-gallery"></a>Picturepark uit de galerie toe te voegen
Voor het configureren van de integratie van Picturepark in Azure AD, moet u Picturepark uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Picturepark uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **Picturepark**.

    ![Het maken van een Azure AD-testgebruiker](./media/picturepark-tutorial/tutorial_picturepark_search.png)

1. Selecteer in het deelvenster resultaten **Picturepark**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/picturepark-tutorial/tutorial_picturepark_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Picturepark op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Picturepark is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Picturepark tot stand worden gebracht.

In Picturepark, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Picturepark, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker Picturepark](#creating-a-picturepark-test-user)**  : als u wilt een equivalent van Britta Simon in Picturepark die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Picturepark.

**Voor het configureren van Azure AD eenmalige aanmelding met Picturepark, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Picturepark** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/picturepark-tutorial/tutorial_picturepark_samlbase.png)

1. Op de **Picturepark domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/picturepark-tutorial/tutorial_picturepark_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<companyname>.picturepark.com`

    b. In het tekstvak **Id** typt u een URL met het volgende patroon: 
    
    |  |
    |--|
    | `https://<companyname>.current-picturepark.com`|
    | `https://<companyname>.picturepark.com`|
    | `https://<companyname>.next-picturepark.com`|
    | |

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de daadwerkelijke aanmeldings-URL en id. Neem contact op met [Picturepark Client ondersteuningsteam](https://picturepark.com/about/contact/) om deze waarden te verkrijgen. 
 
1. Op de **SAML-handtekeningcertificaat** sectie, Kopieer de **VINGERAFDRUK** waarde van het certificaat.

    ![Eenmalige aanmelding configureren](./media/picturepark-tutorial/tutorial_picturepark_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/picturepark-tutorial/tutorial_general_400.png)

1. Op de **Picturepark configuratie** sectie, klikt u op **configureren Picturepark** openen **aanmelding configureren** venster. Kopiëren de **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/picturepark-tutorial/tutorial_picturepark_configure.png) 

1. Meld u in een ander browservenster in uw bedrijf Picturepark site als beheerder.

1. Klik in de werkbalk bovenaan op **Systeembeheer**, en klik vervolgens op **beheerconsole**.
   
    ![Beheerconsole](./media/picturepark-tutorial/ic795062.png "-beheerconsole")

1. Klik op **verificatie**, en klik vervolgens op **id-providers**.
   
    ![Authentication](./media/picturepark-tutorial/ic795063.png "Authentication")

1. In de **identiteit providerconfiguratie** sectie, voert u de volgende stappen uit:
   
    ![ID-providerconfiguratie](./media/picturepark-tutorial/ic795064.png "Identity provider configureren")
   
    a. Klik op **Add**.
  
    b. Typ een naam voor uw configuratie.
   
    c. Selecteer **ingesteld als standaard**.
   
    d. In **verlener URI** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML** die u hebt gekopieerd vanuit Azure portal.
   
    e. In **vertrouwde verlener Duimafdruk** tekstvak, plak de waarde van **vingerafdruk** die u hebt gekopieerd uit **SAML-handtekeningcertificaat** sectie. 

1. Klik op **JoinDefaultUsersGroup**.

1. Om in te stellen de **Emailaddress** kenmerk in de **Claim** tekstvak, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` en klikt u op **opslaan**.

      ![Configuratie](./media/picturepark-tutorial/ic795065.png "configuratie")

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/picturepark-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/picturepark-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/picturepark-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/picturepark-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-picturepark-test-user"></a>Het maken van een testgebruiker Picturepark

Als u wilt inschakelen in Azure AD-gebruikers zich aanmelden bij Picturepark, moeten ze worden ingericht voor Picturepark. In het geval van Picturepark is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Meld u aan bij uw **Picturepark** tenant.

1. Klik in de werkbalk bovenaan op **Systeembeheer**, en klik vervolgens op **gebruikers**.
   
    ![Gebruikers](./media/picturepark-tutorial/ic795067.png "Gebruikers")

1. In de **overzicht van gebruikers** tabblad **nieuw**.
   
    ![Gebruikersbeheer](./media/picturepark-tutorial/ic795068.png "Gebruikersbeheer")

1. Op de **Create User** dialoogvenster, voer de volgende stappen uit een geldige Azure Active Directory-gebruiker u wilt om in te richten:
   
    ![Create User](./media/picturepark-tutorial/ic795069.png "Create User")
   
    a. In de **e-mailadres** tekstvak, type de **e-mailadres** van de gebruiker **BrittaSimon@contoso.com**.  
   
    b. In de **wachtwoord** en **wachtwoord bevestigen** tekstvakken, type de **wachtwoord** van BrittaSimon. 
   
    c. In de **voornaam** tekstvak, type de **voornaam** van de gebruiker **Julia**. 
   
    d. In de **achternaam** tekstvak, type de **achternaam** van de gebruiker **Simon**.
   
    e. In de **bedrijf** tekstvak, type de **bedrijfsnaam** van de gebruiker. 
   
    f. In de **land** tekstvak, selecteer de **land** van de gebruiker.
  
    g. In de **ZIP** tekstvak, type de **postcode** van de plaats.
   
    h. In de **plaats** tekstvak, type de **plaatsnaam** van de gebruiker.

    i. Selecteer een **taal**.
   
    j. Klik op **Create**.

>[!NOTE]
>U kunt alle andere Picturepark gebruiker-account maken van hulpprogramma's of API's geleverd door Picturepark voor het inrichten van gebruikersaccounts van de Azure AD.
> 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Picturepark.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan Picturepark toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Picturepark**.

    ![Eenmalige aanmelding configureren](./media/picturepark-tutorial/tutorial_picturepark_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Picturepark in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Picturepark. Zie [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/picturepark-tutorial/tutorial_general_01.png
[2]: ./media/picturepark-tutorial/tutorial_general_02.png
[3]: ./media/picturepark-tutorial/tutorial_general_03.png
[4]: ./media/picturepark-tutorial/tutorial_general_04.png

[100]: ./media/picturepark-tutorial/tutorial_general_100.png

[200]: ./media/picturepark-tutorial/tutorial_general_200.png
[201]: ./media/picturepark-tutorial/tutorial_general_201.png
[202]: ./media/picturepark-tutorial/tutorial_general_202.png
[203]: ./media/picturepark-tutorial/tutorial_general_203.png

