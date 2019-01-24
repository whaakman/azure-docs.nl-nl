---
title: 'Zelfstudie: Azure Active Directory-integratie met UserEcho | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en UserEcho.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: bedd916b-8f69-4b50-9b8d-56f4ee3bd3ed
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 3846a015e519d5b3ac848be7d55a6dbc18b2afbe
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54814967"
---
# <a name="tutorial-azure-active-directory-integration-with-userecho"></a>Zelfstudie: Azure Active Directory-integratie met UserEcho

In deze zelfstudie leert u hoe u UserEcho integreren met Azure Active Directory (Azure AD).

UserEcho integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot UserEcho heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij UserEcho (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met UserEcho, moet u de volgende items:

- Een Azure AD-abonnement
- Een UserEcho eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u niet beschikt over een evaluatieomgeving in Azure AD, kunt u hier een gratis proefversie van één maand krijgen: [Proefversie](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. UserEcho uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-userecho-from-the-gallery"></a>UserEcho uit de galerie toe te voegen
Voor het configureren van de integratie van UserEcho in Azure AD, moet u UserEcho uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen UserEcho uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **UserEcho**.

    ![Het maken van een Azure AD-testgebruiker](./media/userecho-tutorial/tutorial_userecho_search.png)

1. Selecteer in het deelvenster resultaten **UserEcho**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/userecho-tutorial/tutorial_userecho_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met UserEcho op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in UserEcho is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in UserEcho tot stand worden gebracht.

In UserEcho, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met UserEcho, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker UserEcho](#creating-a-userecho-test-user)**  : als u wilt een equivalent van Britta Simon in UserEcho die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing UserEcho.

**Voor het configureren van Azure AD eenmalige aanmelding met UserEcho, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **UserEcho** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/userecho-tutorial/tutorial_userecho_samlbase.png)

1. Op de **UserEcho domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/userecho-tutorial/tutorial_userecho_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<companyname>.userecho.com/`

    b. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<companyname>.userecho.com/saml/metadata/`

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de daadwerkelijke aanmeldings-URL en id. Neem contact op met [UserEcho Client ondersteuningsteam](https://feedback.userecho.com/) om deze waarden te verkrijgen. 

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/userecho-tutorial/tutorial_userecho_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/userecho-tutorial/tutorial_general_400.png)

1. Op de **UserEcho configuratie** sectie, klikt u op **configureren UserEcho** openen **aanmelding configureren** venster. Kopiëren de **afmelding URL's en SAML Single Sign-On Service** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/userecho-tutorial/tutorial_userecho_configure.png) 

1. In een ander browservenster, meld u aan bij uw bedrijf UserEcho site als beheerder.

1. Klik op de naam van de gebruiker om het menu te openen in de werkbalk in de rechterbovenhoek en klik vervolgens op **Setup**.
   
    ![Eenmalige aanmelding configureren](./media/userecho-tutorial/tutorial_userecho_06.png) 

1. Klik op **integraties**.
   
    ![Eenmalige aanmelding configureren](./media/userecho-tutorial/tutorial_userecho_07.png) 

1. Klik op **Website**, en klik vervolgens op **eenmalige aanmelding (SAML2)**.
   
    ![Eenmalige aanmelding configureren](./media/userecho-tutorial/tutorial_userecho_08.png) 

1. Op de **eenmalige aanmelding (SAML)** pagina, voert u de volgende stappen uit:
   
    ![Eenmalige aanmelding configureren](./media/userecho-tutorial/tutorial_userecho_09.png)
    
    a. Als **SAML-functionaliteit**, selecteer **Ja**.
    
    b. Plakken **Single Sign-On Service URL voor SAML**, die u hebt gekopieerd vanuit de Azure portal in de **URL voor SAML SSO-** tekstvak.
    
    c. Plakken **afmelding URL**, die u hebt gekopieerd vanuit de Azure portal in de **externe logoout URL** tekstvak.
    
    d. Open het gedownloade certificaat in Kladblok, Kopieer de inhoud en plak deze in de **X.509-certificaat** tekstvak.
    
    e. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/userecho-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/userecho-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/userecho-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/userecho-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-userecho-test-user"></a>Het maken van een testgebruiker UserEcho

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in UserEcho.

**Voor het maken van een gebruiker met de naam van Britta Simon in UserEcho, moet u de volgende stappen uitvoeren:**

1. Aanmelding bij uw bedrijf UserEcho site als beheerder.

1. Klik op de naam van de gebruiker om het menu te openen in de werkbalk in de rechterbovenhoek en klik vervolgens op **Setup**.
   
    ![Eenmalige aanmelding configureren](./media/userecho-tutorial/tutorial_userecho_06.png)

1. Klik op **gebruikers**, om uit te breiden de **gebruikers** sectie.
   
    ![Eenmalige aanmelding configureren](./media/userecho-tutorial/tutorial_userecho_10.png)

1. Klik op **Users**.
   
    ![Eenmalige aanmelding configureren](./media/userecho-tutorial/tutorial_userecho_11.png)

1. Klik op **uitnodigen een nieuwe gebruiker**.
   
    ![Eenmalige aanmelding configureren](./media/userecho-tutorial/tutorial_userecho_12.png)

1. Op de **uitnodigen een nieuwe gebruiker** dialoogvenster, voer de volgende stappen uit:
   
    ![Eenmalige aanmelding configureren](./media/userecho-tutorial/tutorial_userecho_13.png)

    a. In de **naam** tekstvak, de typenaam van de gebruiker, zoals Britta Simon.
    
    b.  Typ in het tekstvak **Email** het e-mailadres van de gebruiker, bijvoorbeeld Brittasimon@contoso.com.
    
    c. Klik op **uitnodigen**.

Een uitnodiging is verzonden naar Julia waarmee haar UserEcho met aan de slag. 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan UserEcho.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan UserEcho toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **UserEcho**.

    ![Eenmalige aanmelding configureren](./media/userecho-tutorial/tutorial_userecho_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van de configuratie van uw Azure AD-eenmalige aanmelding via het toegangsvenster.  

Wanneer u op de tegel UserEcho in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing UserEcho.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/userecho-tutorial/tutorial_general_01.png
[2]: ./media/userecho-tutorial/tutorial_general_02.png
[3]: ./media/userecho-tutorial/tutorial_general_03.png
[4]: ./media/userecho-tutorial/tutorial_general_04.png

[100]: ./media/userecho-tutorial/tutorial_general_100.png

[200]: ./media/userecho-tutorial/tutorial_general_200.png
[201]: ./media/userecho-tutorial/tutorial_general_201.png
[202]: ./media/userecho-tutorial/tutorial_general_202.png
[203]: ./media/userecho-tutorial/tutorial_general_203.png

