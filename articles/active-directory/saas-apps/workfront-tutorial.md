---
title: 'Zelfstudie: Azure Active Directory-integratie met Workfront | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Workfront.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: aab8bd2f-f9dd-42da-a18e-d707865687d7
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: d68db3f1b1e6993676ee342d84b7ad1f2e52fffe
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39050704"
---
# <a name="tutorial-azure-active-directory-integration-with-workfront"></a>Zelfstudie: Azure Active Directory-integratie met Workfront

In deze zelfstudie leert u hoe u Workfront integreren met Azure Active Directory (Azure AD).

Workfront integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Workfront heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Workfront (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Workfront, moet u de volgende items:

- Een Azure AD-abonnement
- Een Workfront eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Workfront uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-workfront-from-the-gallery"></a>Workfront uit de galerie toe te voegen
Voor het configureren van de integratie van Workfront in Azure AD, moet u Workfront uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Workfront uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Workfront**.

    ![Het maken van een Azure AD-testgebruiker](./media/workfront-tutorial/tutorial_workfront_search.png)

5. Selecteer in het deelvenster resultaten **Workfront**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/workfront-tutorial/tutorial_workfront_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Workfront op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Workfront is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Workfront tot stand worden gebracht.

In Workfront, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Workfront, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker Workfront](#creating-a-workfront-test-user)**  : als u wilt een equivalent van Britta Simon in Workfront die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Workfront.

**Voor het configureren van Azure AD eenmalige aanmelding met Workfront, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Workfront** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/workfront-tutorial/tutorial_workfront_samlbase.png)

3. Op de **Workfront domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/workfront-tutorial/tutorial_workfront_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<companyname>.attask-ondemand.com`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<companyname>.attasksandbox.com/SAML2`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL en -id. Neem contact op met [Workfront Client ondersteuningsteam](https://www.workfront.com/contact-us/) om deze waarden te verkrijgen. 
 
4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/workfront-tutorial/tutorial_workfront_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/workfront-tutorial/tutorial_general_400.png)

6. Op de **Workfront configuratie** sectie, klikt u op **configureren Workfront** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/workfront-tutorial/tutorial_workfront_configure.png) 

7. Aanmelding bij uw bedrijf Workfront site als administrator.

8. Ga naar **eenmalige aanmelding van de configuratie**.

9. Op de **Single Sign-On** dialoogvenster de volgende stappen uitvoeren
    
    ![Eenmalige aanmelding configureren][23]
   
    a. Als **Type**, selecteer **SAML 2.0**.
   
    b. Selecteer **Service Provider-ID**.
   
    c. Plak de **Single Sign-On Service URL voor SAML** in de **aanmeldings-URL van Portal** tekstvak.
   
    d. Plakken **Service-URL voor eenmalige afmelding** in de **afmelding URL** tekstvak.
   
    e. Plakken **URL van wijzigen wachtwoord** in de **URL van wijzigen wachtwoord** tekstvak.
   
    f. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/workfront-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/workfront-tutorial/create_aaduser_02.png) 

3. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/workfront-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/workfront-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-workfront-test-user"></a>Het maken van een testgebruiker Workfront

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Workfront.

**Voor het maken van een gebruiker met de naam van Britta Simon in Workfront, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf Workfront site als administrator.
2. Klik in het menu aan de bovenkant op **mensen**.
3. Klik op **nieuwe persoon**. 
4. In het dialoogvenster nieuwe persoon, moet u de volgende stappen uitvoeren:
   
    ![Maak een testgebruiker Workfront][21] 
   
    a. In de **voornaam** tekstvak typt u "Julia."
   
    b. In de **achternaam** tekstvak typt u "Simon."
   
    c. In de **e-mailadres** tekstvak typt u Britta Simon van e-mailadres in Azure Active Directory.
   
    d. Klik op **persoon**.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Workfront.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan Workfront toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Workfront**.

    ![Eenmalige aanmelding configureren](./media/workfront-tutorial/tutorial_workfront_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Workfront in het toegangsvenster, krijgt u de aanmeldingspagina van Workfront toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/workfront-tutorial/tutorial_general_01.png
[2]: ./media/workfront-tutorial/tutorial_general_02.png
[3]: ./media/workfront-tutorial/tutorial_general_03.png
[4]: ./media/workfront-tutorial/tutorial_general_04.png
[21]:./media/workfront-tutorial/tutorial_attask_08.png
[23]:./media/workfront-tutorial/tutorial_attask_06.png
[100]: ./media/workfront-tutorial/tutorial_general_100.png

[200]: ./media/workfront-tutorial/tutorial_general_200.png
[201]: ./media/workfront-tutorial/tutorial_general_201.png
[202]: ./media/workfront-tutorial/tutorial_general_202.png
[203]: ./media/workfront-tutorial/tutorial_general_203.png

