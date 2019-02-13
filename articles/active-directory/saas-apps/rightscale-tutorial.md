---
title: 'Zelfstudie: Azure Active Directory-integratie met Rightscale | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Rightscale.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 3a8d376d-95fb-4dd7-832a-4fdd4dd7c87c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 301199667d2307bc81da7ef42f3e4f7daa750ee2
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56199689"
---
# <a name="tutorial-azure-active-directory-integration-with-rightscale"></a>Zelfstudie: Azure Active Directory-integratie met Rightscale

In deze zelfstudie leert u hoe u Rightscale integreren met Azure Active Directory (Azure AD).

Rightscale integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Rightscale heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Rightscale (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Rightscale, moet u de volgende items:

- Een Azure AD-abonnement
- Een Rightscale eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Rightscale uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-rightscale-from-the-gallery"></a>Rightscale uit de galerie toe te voegen
Voor het configureren van de integratie van Rightscale in Azure AD, moet u Rightscale uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Rightscale uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **Rightscale**.

    ![Het maken van een Azure AD-testgebruiker](./media/rightscale-tutorial/tutorial_rightscale_search.png)

1. Selecteer in het deelvenster resultaten **Rightscale**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/rightscale-tutorial/tutorial_rightscale_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Rightscale op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Rightscale is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Rightscale tot stand worden gebracht.

In Rightscale, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Rightscale, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker Rightscale](#creating-a-rightscale-test-user)**  : als u wilt een equivalent van Britta Simon in Rightscale die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Rightscale.

**Voor het configureren van Azure AD eenmalige aanmelding met Rightscale, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Rightscale** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/rightscale-tutorial/tutorial_rightscale_samlbase.png)

1. Op de **Rightscale domein en URL's** sectie, als u wilt configureren van de toepassing in **IDP gestart door modus** u hoeft niet te werk als de app al vooraf geïntegreerd in Azure is.

    ![Eenmalige aanmelding configureren](./media/rightscale-tutorial/tutorial_rightscale_url.png)

1. Op de **Rightscale domein en URL's** sectie, als u wilt configureren van de toepassing in **SP geïnitieerde modus**, voer de volgende stappen uit:
    
    ![Eenmalige aanmelding configureren](./media/rightscale-tutorial/tutorial_rightscale_url1.png)

    a. Klik op de **geavanceerde URL-instellingen weergeven**.

    b. In de **aanmelding URL** tekstvak typt u de URL: `https://login.rightscale.com/`

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/rightscale-tutorial/tutorial_rightscale_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/rightscale-tutorial/tutorial_general_400.png)

1. Op de **Rightscale configuratie** sectie, klikt u op **configureren Rightscale** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/rightscale-tutorial/tutorial_rightscale_configure.png) 
<CS>
1. Als u eenmalige aanmelding configureren voor uw toepassing, moet u aan te melden bij uw tenant RightScale als beheerder.

    a. Klik in het menu bovenaan op de **instellingen** tabblad en selecteer **Single Sign-On**.
   
    ![Eenmalige aanmelding configureren](./media/rightscale-tutorial/tutorial_rightscale_001.png) 

    b. Klik op de '**nieuwe**' om toe te voegen **uw id-Providers van SAML**.
   
    ![Eenmalige aanmelding configureren](./media/rightscale-tutorial/tutorial_rightscale_002.png) 
 
    c. In het tekstvak van **weergavenaam**, voer de naam van uw bedrijf.
   
    ![Eenmalige aanmelding configureren](./media/rightscale-tutorial/tutorial_rightscale_003.png)
 
    d. Selecteer **RightScale toestaan geïnitieerde eenmalige aanmelding met behulp van een detectiehint** en invoer uw **domeinnaam** in de onderstaande tekstvak.
   
    ![Eenmalige aanmelding configureren](./media/rightscale-tutorial/tutorial_rightscale_004.png)

    e. Plak de waarde van **Single Sign-On Service URL voor SAML** die u hebt gekopieerd vanuit Azure portal in **SAML SSO-eindpunt** in RightScale.
   
    ![Eenmalige aanmelding configureren](./media/rightscale-tutorial/tutorial_rightscale_006.png)

    f. Plak de waarde van **SAML entiteit-ID** die u hebt gekopieerd vanuit Azure portal in **SAML EntityID** in RightScale.
   
    ![Eenmalige aanmelding configureren](./media/rightscale-tutorial/tutorial_rightscale_008.png)

    g. Klik op **Browser** knop voor het uploaden van het certificaat dat u hebt gedownload van Azure portal.
   
    ![Eenmalige aanmelding configureren](./media/rightscale-tutorial/tutorial_rightscale_009.png)

    h. Klik op **Opslaan**.
<CE>
> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/rightscale-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/rightscale-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/rightscale-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/rightscale-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-rightscale-test-user"></a>Het maken van een testgebruiker Rightscale

In deze sectie maakt u een gebruiker met de naam van Britta Simon in RightScale. Werken met [Rightscale Client ondersteuningsteam](mailto:support@rightscale.com) om toe te voegen de gebruikers in het RightScale-platform.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Rightscale.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan Rightscale toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Rightscale**.

    ![Eenmalige aanmelding configureren](./media/rightscale-tutorial/tutorial_rightscale_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van de configuratie van uw Azure AD-eenmalige aanmelding via het toegangsvenster.  

Wanneer u op de tegel RightScale in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing RightScale.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/rightscale-tutorial/tutorial_general_01.png
[2]: ./media/rightscale-tutorial/tutorial_general_02.png
[3]: ./media/rightscale-tutorial/tutorial_general_03.png
[4]: ./media/rightscale-tutorial/tutorial_general_04.png

[100]: ./media/rightscale-tutorial/tutorial_general_100.png

[200]: ./media/rightscale-tutorial/tutorial_general_200.png
[201]: ./media/rightscale-tutorial/tutorial_general_201.png
[202]: ./media/rightscale-tutorial/tutorial_general_202.png
[203]: ./media/rightscale-tutorial/tutorial_general_203.png

