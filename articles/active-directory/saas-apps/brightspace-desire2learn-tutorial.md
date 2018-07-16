---
title: 'Zelfstudie: Azure Active Directory-integratie met Brightspace door Desire2Learn | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Brightspace door Desire2Learn.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: e2d3065b-1f6c-4c45-af78-0d5da3266999
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 2387cb6517675e59bc84c6283d31575c9a0aff60
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39043584"
---
# <a name="tutorial-azure-active-directory-integration-with-brightspace-by-desire2learn"></a>Zelfstudie: Azure Active Directory-integratie met Brightspace door Desire2Learn

In deze zelfstudie leert u hoe u Brightspace door Desire2Learn integreren met Azure Active Directory (Azure AD).

Brightspace door Desire2Learn integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Brightspace door Desire2Learn heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Brightspace door Desire2Learn inschakelen (Single Sign-On) met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Brightspace door Desire2Learn, moet u de volgende items:

- Een Azure AD-abonnement
- Een Brightspace door Desire2Learn eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Brightspace door Desire2Learn uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-brightspace-by-desire2learn-from-the-gallery"></a>Brightspace door Desire2Learn uit de galerie toe te voegen
Voor het configureren van de integratie van Brightspace door Desire2Learn in Azure AD, moet u Brightspace door Desire2Learn uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Brightspace door Desire2Learn uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Brightspace door Desire2Learn**.

    ![Het maken van een Azure AD-testgebruiker](./media/brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_search.png)

5. Selecteer in het deelvenster resultaten **Brightspace door Desire2Learn**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Brightspace door Desire2Learn op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Brightspace door Desire2Learn is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Brightspace door Desire2Learn tot stand worden gebracht.

In de Brightspace door Desire2Learn, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Brightspace door Desire2Learn, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een Brightspace door Desire2Learn testgebruiker](#creating-a-brightspace-by-desire2learn-test-user)**  : als u wilt een equivalent van Britta Simon in Brightspace door Desire2Learn die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw Brightspace door Desire2Learn toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met Brightspace door Desire2Learn, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Brightspace door Desire2Learn** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_samlbase.png)

3. Op de **Brightspace Desire2Learn domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_url.png)

    a. In de **id** tekstvak, een URL met behulp van het volgende patroon:
    | |
    |--|
    | `https://<companyname>.tenants.brightspace.com/samlLogin`|
    | `https://<companyname>.desire2learn.com/shibboleth-sp`|

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<companyname>.desire2learn.com/d2l/lp/auth/login/samlLogin.d2l`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke id en de antwoord-URL. Neem contact op met [Brightspace door het ondersteuningsteam Desire2Learn](https://www.d2l.com/contact/) om deze waarden te verkrijgen.
 


4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/brightspace-desire2learn-tutorial/tutorial_general_400.png)

6. Het configureren van eenmalige aanmelding op **Brightspace door Desire2Learn** zijde, moet u voor het verzenden van de gedownloade **Metadata XML** naar [Brightspace door het ondersteuningsteam Desire2Learn](https://www.d2l.com/contact/).

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/brightspace-desire2learn-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/brightspace-desire2learn-tutorial/create_aaduser_02.png) 

3. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/brightspace-desire2learn-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/brightspace-desire2learn-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-brightspace-by-desire2learn-test-user"></a>Het maken van een Brightspace door Desire2Learn testgebruiker

Als u wilt inschakelen in Azure AD-gebruikers zich aanmelden bij Brightspace door Desire2Learn, moeten deze in Brightspace worden ingericht door Desire2Learn.  

In het geval van Brightspace door Desire2Learn, de gebruikersaccounts moeten worden gemaakt door uw [Brightspace door het ondersteuningsteam Desire2Learn](https://www.d2l.com/contact/).

>[!NOTE]
>U kunt elke andere Brightspace door Desire2Learn gebruiker-account maken-hulpprogramma's of API's geleverd door Brightspace door Desire2Learn voor het inrichten van Azure Active Directory gebruikersaccounts. 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Brightspace door Desire2Learn.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan Brightspace door Desire2Learn toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Brightspace door Desire2Learn**.

    ![Eenmalige aanmelding configureren](./media/brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de Brightspace door Desire2Learn tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw Brightspace door Desire2Learn toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/brightspace-desire2learn-tutorial/tutorial_general_01.png
[2]: ./media/brightspace-desire2learn-tutorial/tutorial_general_02.png
[3]: ./media/brightspace-desire2learn-tutorial/tutorial_general_03.png
[4]: ./media/brightspace-desire2learn-tutorial/tutorial_general_04.png

[100]: ./media/brightspace-desire2learn-tutorial/tutorial_general_100.png

[200]: ./media/brightspace-desire2learn-tutorial/tutorial_general_200.png
[201]: ./media/brightspace-desire2learn-tutorial/tutorial_general_201.png
[202]: ./media/brightspace-desire2learn-tutorial/tutorial_general_202.png
[203]: ./media/brightspace-desire2learn-tutorial/tutorial_general_203.png

