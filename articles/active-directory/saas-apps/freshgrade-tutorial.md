---
title: 'Zelfstudie: Azure Active Directory-integratie met FreshGrade | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en FreshGrade.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1055bba6-f4df-462e-bc9b-1ad5ada0f638
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: jeedes
ms.openlocfilehash: 7a24f69e19d492647a24284a13342795559e0542
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39047158"
---
# <a name="tutorial-azure-active-directory-integration-with-freshgrade"></a>Zelfstudie: Azure Active Directory-integratie met FreshGrade

In deze zelfstudie leert u hoe u FreshGrade integreren met Azure Active Directory (Azure AD).

FreshGrade integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot FreshGrade heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij FreshGrade (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met FreshGrade, moet u de volgende items:

- Een Azure AD-abonnement
- Een FreshGrade eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. FreshGrade uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-freshgrade-from-the-gallery"></a>FreshGrade uit de galerie toe te voegen
Voor het configureren van de integratie van FreshGrade in Azure AD, moet u FreshGrade uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen FreshGrade uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **FreshGrade**.

    ![Het maken van een Azure AD-testgebruiker](./media/freshgrade-tutorial/tutorial_freshgrade_search.png)

5. Selecteer in het deelvenster resultaten **FreshGrade**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/freshgrade-tutorial/tutorial_freshgrade_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met FreshGrade op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in FreshGrade is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in FreshGrade tot stand worden gebracht.

In FreshGrade, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met FreshGrade, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker FreshGrade](#creating-a-freshgrade-test-user)**  : als u wilt een equivalent van Britta Simon in FreshGrade die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing FreshGrade.

**Voor het configureren van Azure AD eenmalige aanmelding met FreshGrade, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **FreshGrade** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](./media/freshgrade-tutorial/tutorial_freshgrade_samlbase.png)

3. Op de **FreshGrade domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/freshgrade-tutorial/tutorial_freshgrade_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van de volgende patronen:
      | |
      |--|
      | `https://<subdomain>.freshgrade.com/login` |
      | `https://<subdomain>.onboarding.freshgrade.com/login` |

    b. In de **id** tekstvak, een URL met behulp van de volgende patronen:
      | |
      |--|
      | `https://login.onboarding.freshgrade.com:443/saml/metadata/alias/<instancename>` |
      | `https://login.freshgrade.com:443/saml/metadata/alias/<instancename>` |

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL en -id. Neem contact op met [FreshGrade Client ondersteuningsteam](mailTo:support@freshgrade.com) om deze waarden te verkrijgen.

4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op de knop kopiëren om te kopiëren **App-Url voor federatieve metagegevens** en plak deze in Kladblok.
    
    ![Eenmalige aanmelding configureren](./media/freshgrade-tutorial/tutorial_metadataurl.png)
     
5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/freshgrade-tutorial/tutorial_general_400.png)

6. Op de **FreshGrade configuratie** sectie, klikt u op **configureren FreshGrade** openen **aanmelding configureren** venster. Kopiëren de **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/freshgrade-tutorial/tutorial_freshgrade_configure.png)

7. Het configureren van eenmalige aanmelding op **FreshGrade** zijde, moet u voor het verzenden van de **App-Url voor federatieve metagegevens** en **Single Sign-On Service URL voor SAML** naar [ Het ondersteuningsteam FreshGrade](mailTo:support@freshgrade.com). Ze stelt u deze optie om de SAML SSO-verbinding instellen goed aan beide zijden.

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/freshgrade-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/freshgrade-tutorial/create_aaduser_02.png) 

3. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/freshgrade-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/freshgrade-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-freshgrade-test-user"></a>Het maken van een testgebruiker FreshGrade

In deze sectie maakt u een gebruiker met de naam van Britta Simon in FreshGrade. Neem contact op met [FreshGrade ondersteuningsteam](mailTo:support@freshgrade.com) om toe te voegen de gebruikers in het FreshGrade-platform.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan FreshGrade.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan FreshGrade toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **FreshGrade**.

    ![Eenmalige aanmelding configureren](./media/freshgrade-tutorial/tutorial_freshgrade_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel FreshGrade in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing FreshGrade.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/freshgrade-tutorial/tutorial_general_01.png
[2]: ./media/freshgrade-tutorial/tutorial_general_02.png
[3]: ./media/freshgrade-tutorial/tutorial_general_03.png
[4]: ./media/freshgrade-tutorial/tutorial_general_04.png

[100]: ./media/freshgrade-tutorial/tutorial_general_100.png

[200]: ./media/freshgrade-tutorial/tutorial_general_200.png
[201]: ./media/freshgrade-tutorial/tutorial_general_201.png
[202]: ./media/freshgrade-tutorial/tutorial_general_202.png
[203]: ./media/freshgrade-tutorial/tutorial_general_203.png

