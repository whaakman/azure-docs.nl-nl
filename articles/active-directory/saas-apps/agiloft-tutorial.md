---
title: 'Zelfstudie: Azure Active Directory-integratie met Agiloft | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Agiloft.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: aca13814-cdbd-46b8-93dc-1578099c5ee4
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/09/2017
ms.author: jeedes
ms.openlocfilehash: f11d705cceb05c9e9cd0b340a680684eecf4f5d9
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39054196"
---
# <a name="tutorial-azure-active-directory-integration-with-agiloft"></a>Zelfstudie: Azure Active Directory-integratie met Agiloft

In deze zelfstudie leert u hoe u Agiloft integreren met Azure Active Directory (Azure AD).

Agiloft integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Agiloft heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Agiloft (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Agiloft, moet u de volgende items:

- Een Azure AD-abonnement
- Een Agiloft eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Agiloft uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-agiloft-from-the-gallery"></a>Agiloft uit de galerie toe te voegen
Voor het configureren van de integratie van Agiloft in Azure AD, moet u Agiloft uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Agiloft uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **Agiloft**, selecteer **Agiloft** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Agiloft in de lijst met resultaten](./media/agiloft-tutorial/tutorial_agiloft_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Agiloft op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Agiloft is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Agiloft tot stand worden gebracht.

In Agiloft, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Agiloft, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Agiloft](#create-an-agiloft-test-user)**  : als u wilt een equivalent van Britta Simon in Agiloft die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Agiloft.

**Voor het configureren van Azure AD eenmalige aanmelding met Agiloft, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Agiloft** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/agiloft-tutorial/tutorial_agiloft_samlbase.png)

3. Op de **Agiloft domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in de modus voor IDP gestart:

    ![Agiloft domein en URL's, eenmalige aanmelding informatie](./media/agiloft-tutorial/tutorial_agiloft_url.png)

    a. In de **id** tekstvak, een URL met behulp van het volgende patroon: 
    | |
    |-|-|
    | `https://<subdomain>.saas.enterprisewizard.com/project/<KB_NAME>` |
    | `https://<subdomain>.agiloft.com/project/<KB_NAME>` |

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon:
    | |
    |-|-|
    | `https://<subdomain>.saas.enterprisewizard.com:443/gui2/spsamlsso?project=<KB_NAME>` |
    | `https://<subdomain>.agiloft.com:443/gui2/spsamlsso?project=<KB_NAME>` |

4. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Agiloft domein en URL's, eenmalige aanmelding informatie](./media/agiloft-tutorial/tutorial_agiloft_url1.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: 
    | |
    |-|-|
    | `https://<subdomain>.saas.enterprisewizard.com/gui2/samlssologin.jsp?project=<KB_NAME>` |
    | `https://<subdomain>.agiloft.com/gui2/samlssologin.jsp?project=<KB_NAME>` |
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke-id, de antwoord-URL en aanmeldings-URL. Neem contact op met [Agiloft Client ondersteuningsteam](https://www.agiloft.com/support-login.htm) om deze waarden te verkrijgen. 

5. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/agiloft-tutorial/tutorial_agiloft_certificate.png) 

6. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/agiloft-tutorial/tutorial_general_400.png)
    
7. Op de **Agiloft configuratie** sectie, klikt u op **configureren Agiloft** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Agiloft configuratie](./media/agiloft-tutorial/tutorial_agiloft_configure.png) 

8. In een ander browservenster aanmelden bij uw bedrijf Agiloft site als beheerder.

9. Klik op **Setup** (in het linkerdeelvenster) en vervolgens **toegang**.

    ![Agiloft configuratie](./media/agiloft-tutorial/setup1.png) 

10. Klik op de knop **' configureren van SAML 2.0 Single Sign-On '**. 
    
    ![Agiloft configuratie](./media/agiloft-tutorial/setup2.png) 

11. Er wordt een dialoogvenster wizard weergegeven. In het dialoogvenster, klikt u op het tabblad **'Identity Provider Details'** en vult u de volgende velden:  
    
    ![Agiloft configuratie](./media/agiloft-tutorial/setup4.png) 

    a. In **IdP entiteit-Id / verlener** tekstvak, plak de waarde van **SAML entiteit-ID**, die u hebt gekopieerd vanuit Azure portal.

    b. In **IdP aanmeldings-URL** tekstvak, plak de waarde van **Single Sign-On Service URL**, die u hebt gekopieerd vanuit Azure portal.

    c. In **afmeldings-URL van id-provider** tekstvak, plak de waarde van **afmelding URL**, die u hebt gekopieerd vanuit Azure portal.

    d. Open uw **base-64 gecodeerde certificaat** in Kladblok gedownload vanuit Azure portal, kopieert u de inhoud ervan naar het Klembord en plakt u deze naar de **IdP opgegeven x.509-certificaatinhoud** tekstvak.

    e. Klik op **Voltooien**.


> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/agiloft-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/agiloft-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/agiloft-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/agiloft-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-agiloft-test-user"></a>Maak een testgebruiker Agiloft

Toepassing biedt ondersteuning voor Just in time gebruikersinrichting en na-verificatiegebruikers, in de toepassing automatisch worden gemaakt. Er is geen actie voor u in deze sectie.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Agiloft.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan Agiloft toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Agiloft**.

    ![De koppeling Agiloft in de lijst met toepassingen](./media/agiloft-tutorial/tutorial_agiloft_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Agiloft in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Agiloft.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/agiloft-tutorial/tutorial_general_01.png
[2]: ./media/agiloft-tutorial/tutorial_general_02.png
[3]: ./media/agiloft-tutorial/tutorial_general_03.png
[4]: ./media/agiloft-tutorial/tutorial_general_04.png

[100]: ./media/agiloft-tutorial/tutorial_general_100.png

[200]: ./media/agiloft-tutorial/tutorial_general_200.png
[201]: ./media/agiloft-tutorial/tutorial_general_201.png
[202]: ./media/agiloft-tutorial/tutorial_general_202.png
[203]: ./media/agiloft-tutorial/tutorial_general_203.png

