---
title: 'Zelfstudie: Azure Active Directory-integratie met DocuSign | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en DocuSign.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: a691288b-84c1-40fb-84bd-5b06878865f0
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: 5d8aef1edf4d7a02686db48d3e788e4f9493c398
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39448482"
---
# <a name="tutorial-azure-active-directory-integration-with-docusign"></a>Zelfstudie: Azure Active Directory-integratie met DocuSign

In deze zelfstudie leert u hoe DocuSign integreren met Azure Active Directory (Azure AD).

DocuSign integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot DocuSign heeft
- U kunt uw gebruikers automatisch ophalen ondertekend in DocuSign (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met DocuSign, moet u de volgende items:

- Een Azure AD-abonnement
- Een DocuSign eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. DocuSign uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-docusign-from-the-gallery"></a>DocuSign uit de galerie toe te voegen
Voor het configureren van de integratie van DocuSign in Azure AD, moet u DocuSign uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen DocuSign uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
1. Klik op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

1. Typ in het zoekvak **DocuSign**.

    ![Het maken van een Azure AD-testgebruiker](./media/docusign-tutorial/tutorial_docusign_search.png)

1. Selecteer in het deelvenster resultaten **DocuSign**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/docusign-tutorial/tutorial_docusign_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met DocuSign op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in DocuSign is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in DocuSign tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door toe te wijzen de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in DocuSign.

Om te configureren en testen van Azure AD eenmalige aanmelding met DocuSign, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker DocuSign](#creating-a-docusign-test-user)**  : als u wilt een equivalent van Britta Simon in DocuSign die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing DocuSign.

**Voor het configureren van Azure AD eenmalige aanmelding met DocuSign, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **DocuSign** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/docusign-tutorial/tutorial_docusign_samlbase.png)

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base 64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/docusign-tutorial/tutorial_docusign_certificate.png) 

1. Op de **DocuSign configuratie** sectie in Azure portal, klik op **DocuSign configureren** om configureren aanmeldings-venster te openen. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**
    
    ![Eenmalige aanmelding configureren](./media/docusign-tutorial/tutorial_docusign_configure.png)

1. In een ander browservenster, meld u aan bij uw **DocuSign-beheerportal** als beheerder.

1. Klik in het navigatiemenu aan de linkerkant op **domeinen**.
   
    ![Eenmalige aanmelding configureren][51]

1. Klik in het rechter deelvenster op **Claim domein**.
   
    ![Eenmalige aanmelding configureren][52]

1. Op de **claimen van een domein** dialoogvenster in de **domeinnaam** tekstvak typt u het domein van uw bedrijf, en klik vervolgens op **Claim**. Zorg ervoor dat u het domein controleren en de status actief is.
   
    ![Eenmalige aanmelding configureren][53]

1. Klik in het menu aan de linkerkant op **id-Providers**  
   
    ![Eenmalige aanmelding configureren][54]
1. Klik in het rechter deelvenster op **id-Provider toevoegen**. 
   
    ![Eenmalige aanmelding configureren][55]

1. Op de **instellingen van de identiteit** pagina, voert u de volgende stappen uit:
   
    ![Eenmalige aanmelding configureren][56]

    a. In de **naam** tekstvak typt u een unieke naam voor uw configuratie. Gebruik geen spaties.

    b. Plakken **SAML entiteit-ID** in de **Identity Provider Issuer** tekstvak.

    c. Plakken **Single Sign-On Service URL voor SAML** in de **aanmeldings-URL van id-Provider** tekstvak.

    d. Plakken **afmelding URL** in de **afmeldings-URL van id-Provider** tekstvak.

    e. Selecteer **AuthN aanvraag ondertekenen**.

    f. Als **AuthN verzenden aanvraag door**, selecteer **POST**.

    g. Als **afmeldingsaanvraag verzenden door**, selecteer **ophalen**.

1. In de **toewijzing van aangepast kenmerk** sectie, kiest u het veld dat u wilt toewijzen met Azure AD-Claim. In dit voorbeeld wordt de **emailaddress** claim is toegewezen door de waarde van **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**. Het is de standaardnaam van de claim van Azure AD voor e-claim. 
   
    > [!NOTE]
    > Gebruik het juiste **gebruikers-id** om toe te wijzen van de gebruiker uit Azure AD DocuSign gebruiker toewijzen. Selecteer het juiste veld en voer de juiste waarde op basis van de instellingen van uw organisatie.
          
    ![Eenmalige aanmelding configureren][57]

1. In de **Provider identiteitscertificaat** sectie, klikt u op **certificaat toevoegen**, en upload het certificaat dat u hebt gedownload van Azure AD-portal.   
   
    ![Eenmalige aanmelding configureren][58]

1. Klik op **Opslaan**.

1. In de **id-Providers** sectie, klikt u op **acties**, en klik vervolgens op **eindpunten**.   
   
    ![Eenmalige aanmelding configureren][59]
 
1. In de **SAML 2.0-eindpunten weergeven** sectie op **DocuSign-beheerportal**, voer de volgende stappen uit:
   
    ![Eenmalige aanmelding configureren][60]
   
    a. Kopiëren de **URL voor Service Provider-verlener**, en plak in het **id** tekstvak op **DocuSign-domein en URL's** sectie van de Azure portal het patroon volgen: `https://<subdomain>.docusign.com/organization/<uniqueID>/saml2/login/sp/<uniqueID>`.
   
    b. Kopiëren de **aanmeldings-URL voor Service Provider**, en plak in het **aanmelding URL** tekstvak op **DocuSign-domein en URL's** sectie van de Azure-portal volgen het patroon: `https://<subdomain>.docusign.com/organization/<uniqueID>/saml2/` .

    ![Eenmalige aanmelding configureren](./media/docusign-tutorial/tutorial_docusign_url.png)
      
    c.  Klik op **sluiten**
    
1. Klik op de Azure-portal **opslaan**.
    
    ![Eenmalige aanmelding configureren](./media/docusign-tutorial/tutorial_general_400.png)

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/docusign-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/docusign-tutorial/create_aaduser_02.png) 

1. Aan de bovenkant van het dialoogvenster, klikt u op **toevoegen** openen de **gebruiker** dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/docusign-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/docusign-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-docusign-test-user"></a>Het maken van een testgebruiker DocuSign

Toepassing ondersteunt **Just-in-time gebruikersinrichting** en na-verificatiegebruikers automatisch in de toepassing gemaakt worden.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door haar toegang verlenen tot DocuSign.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan DocuSign toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **DocuSign**.

    ![Eenmalige aanmelding configureren](./media/docusign-tutorial/tutorial_docusign_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel DocuSign in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing DocuSign.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
* [Inrichten van gebruikers configureren](docusign-provisioning-tutorial.md)


<!--Image references-->

[1]: ./media/docusign-tutorial/tutorial_general_01.png
[2]: ./media/docusign-tutorial/tutorial_general_02.png
[3]: ./media/docusign-tutorial/tutorial_general_03.png
[4]: ./media/docusign-tutorial/tutorial_general_04.png
[51]: ./media/docusign-tutorial/tutorial_docusign_21.png
[52]: ./media/docusign-tutorial/tutorial_docusign_22.png
[53]: ./media/docusign-tutorial/tutorial_docusign_23.png
[54]: ./media/docusign-tutorial/tutorial_docusign_19.png
[55]: ./media/docusign-tutorial/tutorial_docusign_20.png
[56]: ./media/docusign-tutorial/tutorial_docusign_24.png
[57]: ./media/docusign-tutorial/tutorial_docusign_25.png
[58]: ./media/docusign-tutorial/tutorial_docusign_26.png
[59]: ./media/docusign-tutorial/tutorial_docusign_27.png
[60]: ./media/docusign-tutorial/tutorial_docusign_28.png
[61]: ./media/docusign-tutorial/tutorial_docusign_29.png
[100]: ./media/docusign-tutorial/tutorial_general_100.png

[200]: ./media/docusign-tutorial/tutorial_general_200.png
[201]: ./media/docusign-tutorial/tutorial_general_201.png
[202]: ./media/docusign-tutorial/tutorial_general_202.png
[203]: ./media/docusign-tutorial/tutorial_general_203.png

