---
title: 'Zelfstudie: Azure Active Directory-integratie met EBSCO | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en EBSCO.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 144f7f65-69e9-4016-a151-fe1104fd6ba8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: jeedes
ms.openlocfilehash: 984c5ceab0115d464633094d20deb35761eb80f0
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55178644"
---
# <a name="tutorial-azure-active-directory-integration-with-ebsco"></a>Zelfstudie: Azure Active Directory-integratie met EBSCO

In deze zelfstudie leert u hoe u EBSCO integreren met Azure Active Directory (Azure AD).

EBSCO integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot EBSCO heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij EBSCO (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met EBSCO, moet u de volgende items:

- Een Azure AD-abonnement
- Een EBSCO eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. EBSCO uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-ebsco-from-the-gallery"></a>EBSCO uit de galerie toe te voegen
Voor het configureren van de integratie van EBSCO in Azure AD, moet u EBSCO uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen EBSCO uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **EBSCO**, selecteer **EBSCO** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![EBSCO in de lijst met resultaten](./media/ebsco-tutorial/tutorial_ebsco_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met EBSCO op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in EBSCO is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in EBSCO tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met EBSCO, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker EBSCO](#create-an-ebsco-test-user)**  -u EBSCOhost inrichting/persoonlijke instellingen kunt automatiseren. EBSCO biedt ondersteuning voor Just-In-Time inrichting van gebruikers.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing EBSCO.

**Voor het configureren van Azure AD eenmalige aanmelding met EBSCO, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **EBSCO** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/ebsco-tutorial/tutorial_ebsco_samlbase.png)

1. Op de **EBSCO domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![EBSCO domein en URL's, eenmalige aanmelding informatie](./media/ebsco-tutorial/tutorial_ebsco_url.png)

    In de **id** tekstvak, een URL typen: `pingsso.ebscohost.com`

1. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![EBSCO domein en URL's, eenmalige aanmelding informatie](./media/ebsco-tutorial/tutorial_ebsco_url1.png)

    Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `http://search.ebscohost.com/login.aspx?authtype=sso&custid=<unique EBSCO customer ID>&profile=<profile ID>`
     
    > [!NOTE] 
    > De waarde voor de aanmeldings-URL is niet echt. Werk de waarde bij met de werkelijke aanmeldings-URL. Neem contact op met [EBSCO Client ondersteuningsteam](mailto:sso@ebsco.com) om de waarde. 

    o **unieke elementen:**  

    o **Custid** Enter unieke EBSCO klant-ID = 

    o **profiel** = Clients de koppeling om de gebruikers aan een specifiek profiel (afhankelijk van wat ze bij EBSCO aanschaffen) kunnen aanpassen. Ze kunnen een specifiek profiel-ID invoeren De belangrijkste-id's zijn eds (EBSCO Discovery-Service) en ehost (EBSOCOhost databases). Instructies voor dezelfde krijgen [hier](https://help.ebsco.com/interfaces/EBSCOhost/EBSCOhost_FAQs/How_do_I_set_up_direct_links_to_EBSCOhost_profiles_and_or_databases#profile).

1. EBSCO toepassing verwacht het SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de "**gebruikerskenmerken**" sectie op de pagina van de toepassing-integratie. In de volgende schermopname ziet u een voorbeeld hiervan.
    
    ![Eenmalige aanmelding configureren](./media/ebsco-tutorial/tutorial_ebsco_attribute.png)

    > [!Note]
    > De **naam** kenmerk verplicht is en is toegewezen met **gebruikers-id** in EBSCO toepassing. Dit is standaard toegevoegd zodat u niet hoeft op dit handmatig toevoegen.
    
1. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de bovenstaande afbeelding en voer de volgende stappen uit:
    
    | Naam kenmerk | Waarde kenmerk |
    | ---------------| --------------- |    
    | FirstName   | user.givenname |
    | LastName   | user.surname |
    | Email   | user.mail |

    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/ebsco-tutorial/tutorial_officespace_04.png)

    ![Eenmalige aanmelding configureren](./media/ebsco-tutorial/tutorial_attribute_05.png)
    
    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.
    
    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    d. Klik op **OK**.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/ebsco-tutorial/tutorial_ebsco_certificate.png) 

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/ebsco-tutorial/tutorial_general_400.png)
    
1. Het configureren van eenmalige aanmelding op **EBSCO** zijde, moet u voor het verzenden van de gedownloade **Metadata XML** naar [EBSCO ondersteuningsteam](mailto:sso@ebsco.com). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/ebsco-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/ebsco-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/ebsco-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/ebsco-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-ebsco-test-user"></a>Maak een testgebruiker EBSCO

In het geval van EBSCO wordt het automatisch inrichten van gebruikers.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

Azure AD wordt de vereiste gegevens doorgegeven aan EBSCO toepassing. EBSCO van gebruikersinrichting kan worden automatische of vereisen dat een eenmalige formulier. Dat hangt ervan af of de client heeft een groot aantal bestaande EBSCOhost accounts met persoonlijke instellingen die zijn opgeslagen. Hetzelfde kan worden besproken met de [EBSCO ondersteuningsteam](mailto:sso@ebsco.com) tijdens de implementatie. In beide gevallen de client hoeft te maken van accounts EBSCOhost vóór testen.

   >[!Note]
   >U kunt EBSCOhost inrichting/persoonlijke instellingen automatiseren. Neem contact op met [EBSCO ondersteuningsteam](mailto:sso@ebsco.com) over Just-In-Time inrichting van gebruikers. 
 
### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan EBSCO.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan EBSCO toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **EBSCO**.

    ![De koppeling EBSCO in de lijst met toepassingen](./media/ebsco-tutorial/tutorial_ebsco_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

1. Wanneer u op de tegel EBSCO in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing EBSCO.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

1. Als u zich bij de toepassing aanmelden, klikt u op de **aanmelden** knop in de rechterbovenhoek.

    ![De aanmelding EBSCO in de lijst met toepassingen](./media/ebsco-tutorial/tutorial_ebsco_signin.png)
 
1. Ontvangt u een eenmalige prompt om te koppelen van de institutionele/SAML-aanmelding met een **nu uw bestaand MyEBSCOhost-account koppelt aan uw account instelling** of **een nieuw MyEBSCOhost-account maken en een koppeling naar uw instelling account**. Het account wordt gebruikt voor persoonlijke instellingen op de toepassing EBSCOhost. Selecteer de optie **een nieuw account maken** en u ziet dat het formulier voor persoonlijke instellingen vooraf ingevuld met de waarden van het saml-antwoord, is zoals wordt weergegeven in de onderstaande schermafbeelding. Klik op **'Doorgaan'** om op te slaan deze selectie.
    
     ![De gebruiker EBSCO in de lijst met toepassingen](./media/ebsco-tutorial/tutorial_ebsco_user.png)

1. Na het voltooien van de bovenstaande instellingen, cookies/cache leegmaken en aanmelding opnieuw. U hoeft te handmatig aanmelding opnieuw en de persoonlijke instellingen worden onthouden

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/ebsco-tutorial/tutorial_general_01.png
[2]: ./media/ebsco-tutorial/tutorial_general_02.png
[3]: ./media/ebsco-tutorial/tutorial_general_03.png
[4]: ./media/ebsco-tutorial/tutorial_general_04.png

[100]: ./media/ebsco-tutorial/tutorial_general_100.png

[200]: ./media/ebsco-tutorial/tutorial_general_200.png
[201]: ./media/ebsco-tutorial/tutorial_general_201.png
[202]: ./media/ebsco-tutorial/tutorial_general_202.png
[203]: ./media/ebsco-tutorial/tutorial_general_203.png

