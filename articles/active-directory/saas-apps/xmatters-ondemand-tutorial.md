---
title: 'Zelfstudie: Azure Active Directory-integratie met xMatters OnDemand | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en xMatters OnDemand.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: ca0633db-4f95-432e-b3db-0168193b5ce9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: 5eab595c12fda25768a3bf8a04650870e1c4f57e
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55153536"
---
# <a name="tutorial-azure-active-directory-integration-with-xmatters-ondemand"></a>Zelfstudie: Azure Active Directory-integratie met xMatters OnDemand

In deze zelfstudie leert u hoe u xMatters OnDemand integreren met Azure Active Directory (Azure AD).

XMatters OnDemand integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot xMatters OnDemand heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij xMatters OnDemand (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met xMatters OnDemand, moet u de volgende items:

- Een Azure AD-abonnement
- Een xMatters OnDemand eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. XMatters OnDemand uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-xmatters-ondemand-from-the-gallery"></a>XMatters OnDemand uit de galerie toe te voegen
Voor het configureren van de integratie van xMatters OnDemand in Azure AD, moet u xMatters OnDemand uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen xMatters OnDemand uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **xMatters OnDemand**.

    ![Het maken van een Azure AD-testgebruiker](./media/xmatters-ondemand-tutorial/tutorial_xmattersondemand_search.png)

1. Selecteer in het deelvenster resultaten **xMatters OnDemand**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/xmatters-ondemand-tutorial/tutorial_xmattersondemand_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met xMatters die OnDemand op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD weten wat de equivalente-gebruiker in xMatters OnDemand aan een gebruiker in Azure AD is. Met andere woorden, een koppeling de relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in xMatters OnDemand moet tot stand worden gebracht.

In xMatters OnDemand, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met xMatters OnDemand, u nodig hebt voor de volgende bouwstenen:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker van xMatters OnDemand](#creating-a-xmatters-ondemand-test-user)**  : als u wilt een equivalent van Britta Simon in xMatters OnDemand die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw xMatters OnDemand-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met xMatters OnDemand, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **xMatters OnDemand** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](./media/xmatters-ondemand-tutorial/tutorial_xmattersondemand_samlbase.png)

1. Op de **xMatters OnDemand domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/xmatters-ondemand-tutorial/tutorial_xmattersondemand_url.png)
    
    a. In het tekstvak **Id** typt u een URL met het volgende patroon:
    | |
    |--|
    | `https://<companyname>.au1.xmatters.com.au/`|
    | `https://<companyname>.cs1.xmatters.com/`|
    | `https://<companyname>.xmatters.com/`|
    | `https://www.xmatters.com`|
    | `https://<companyname>.xmatters.com.au/`|

    b. In het tekstvak **Antwoord-URL** typt u een URL met de volgende notatie:
    | |
    |--|
    | `https://<companyname>.au1.xmatters.com.au`|
    | `https://<companyname>.xmatters.com/sp/<instancename>`|
    | `https://<companyname>.cs1.xmatters.com/sp/<instancename>`|
    | `https://<companyname>.au1.xmatters.com.au/<instancename>`|

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke id en antwoord-URL. Neem contact op met [xMatters OnDemand ondersteuningsteam](https://www.xmatters.com/company/contact-us/) om deze waarden te verkrijgen.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand lokaal als **c:\\XMatters OnDemand.cer**.

    ![Eenmalige aanmelding configureren](./media/xmatters-ondemand-tutorial/tutorial_xmattersondemand_certificate.png)

    > [!IMPORTANT]
    > U moet voor het doorsturen van het certificaat naar de [xMatters OnDemand ondersteuningsteam](https://www.xmatters.com/company/contact-us/). Het certificaat moet worden geüpload door het ondersteuningsteam xMatters voordat u de configuratie voor eenmalige aanmelding kunt voltooien. 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/xmatters-ondemand-tutorial/tutorial_general_400.png)

1. Op de **xMatters OnDemand configuratie** sectie, klikt u op **xMatters OnDemand configureren** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/xmatters-ondemand-tutorial/tutorial_xmattersondemand_configure.png) 

1. In een ander browservenster aanmelden bij uw bedrijf XMatters OnDemand site als beheerder.

1. Klik in de werkbalk bovenaan op **Admin**, en klik vervolgens op **bedrijfsgegevens** in de navigatiebalk aan de linkerkant.

    ![Admin](./media/xmatters-ondemand-tutorial/IC776795.png "Admin")

1. Op de **SAML-configuratie** pagina, voert u de volgende stappen uit:

    ![De SAML-configuratie](./media/xmatters-ondemand-tutorial/IC776796.png "SAML-configuratie")

    a. Selecteer **SAML inschakelen**.

    b. In de **Identity Provider-ID** tekstvak plakken **SAML entiteit-ID** waarde die u hebt gekopieerd vanuit Azure portal.

    c. In de **eenmalige aanmelding op URL** tekstvak plakken **Single Sign-On Service URL voor SAML** waarde die u hebt gekopieerd vanuit Azure portal.

    d. In de **URL voor eenmalige afmelding** tekstvak plakken **afmelding URL**, die u hebt gekopieerd vanuit Azure portal.

    e. Klik op de pagina Details van het bedrijf aan de bovenkant op **wijzigingen opslaan**.

    ![Details van de bedrijfsportal](./media/xmatters-ondemand-tutorial/IC776797.png "details van de bedrijfsportal")

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/xmatters-ondemand-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.

    ![Het maken van een Azure AD-testgebruiker](./media/xmatters-ondemand-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.

    ![Het maken van een Azure AD-testgebruiker](./media/xmatters-ondemand-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:

    ![Het maken van een Azure AD-testgebruiker](./media/xmatters-ondemand-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.

### <a name="creating-a-xmatters-ondemand-test-user"></a>Het maken van een gebruiker xMatters OnDemand testen

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in xMatters OnDemand.

**Als u de gebruiker handmatig moet maken, voert u de volgende stappen uit:**

1. Meld u aan bij uw **XMatters OnDemand** tenant.

1.  Klik op **gebruikers** tabblad en klik vervolgens op **gebruiker toevoegen**.

    ![Gebruikers](./media/xmatters-ondemand-tutorial/IC781048.png "Gebruikers")

1. In de **toevoegen van een gebruiker** sectie, voert u de volgende stappen uit:

    ![Een gebruiker toevoegen](./media/xmatters-ondemand-tutorial/IC781049.png "een gebruiker toevoegen")

    a. Selecteer **Active**.

    b. In de **gebruikers-ID** tekstvak, type de gebruikers-id van gebruiker, zoals Brittasimon@contoso.com.

    c. In de **voornaam** tekstvak, type de voornaam van de gebruiker, zoals Julia.

    d. In de **achternaam** tekstvak Achternaam van de gebruiker, zoals Simon type.

    e. In de **Site** tekstvak, voer de geldige site van een geldige Azure AD-account die u inrichten wilt.

    f. Klik op **Opslaan**.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot xMatters OnDemand.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan xMatters OnDemand toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **xMatters OnDemand**.

    ![Eenmalige aanmelding configureren](./media/xmatters-ondemand-tutorial/tutorial_xmattersondemand_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de xMatters OnDemand-tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw xMatters OnDemand-toepassing.
Zie [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/xmatters-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/xmatters-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/xmatters-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/xmatters-ondemand-tutorial/tutorial_general_04.png

[100]: ./media/xmatters-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/xmatters-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/xmatters-ondemand-tutorial/tutorial_general_201.png
[202]: ./media/xmatters-ondemand-tutorial/tutorial_general_202.png
[203]: ./media/xmatters-ondemand-tutorial/tutorial_general_203.png
