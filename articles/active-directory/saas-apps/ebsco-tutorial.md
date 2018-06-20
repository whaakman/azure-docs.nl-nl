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
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: jeedes
ms.openlocfilehash: 993d03e05c7d97634b7ed12c50d248643ce9e51d
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36221281"
---
# <a name="tutorial-azure-active-directory-integration-with-ebsco"></a>Zelfstudie: Azure Active Directory-integratie met EBSCO

In deze zelfstudie leert u hoe EBSCO integreren met Azure Active Directory (Azure AD).

EBSCO integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot EBSCO heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij EBSCO (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met EBSCO, moet u de volgende items:

- Een Azure AD-abonnement
- Een EBSCO eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. EBSCO uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-ebsco-from-the-gallery"></a>EBSCO uit de galerie toevoegen
Voor het configureren van de integratie van EBSCO in Azure AD, moet u EBSCO uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen EBSCO uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **EBSCO**, selecteer **EBSCO** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![EBSCO in de lijst met resultaten](./media/ebsco-tutorial/tutorial_ebsco_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met EBSCO op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in EBSCO is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in EBSCO tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met EBSCO, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker EBSCO](#create-an-ebsco-test-user)**  -EBSCOhost inrichting/persoonlijke instellingen kan worden geautomatiseerd. EBSCO ondersteunt Just-In-Time gebruikers inrichten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing EBSCO configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met EBSCO, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **EBSCO** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/ebsco-tutorial/tutorial_ebsco_samlbase.png)

3. Op de **EBSCO domein en de URL's** sectie, voert u de volgende stappen uit als u wilt configureren, de toepassing in **IDP** modus gestart:

    ![URL's en EBSCO domein eenmalige aanmelding informatie](./media/ebsco-tutorial/tutorial_ebsco_url.png)

    In de **id** textbox, typ een URL: `pingsso.ebscohost.com`

4. Controleer **weergeven geavanceerde instellingen voor URL** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![URL's en EBSCO domein eenmalige aanmelding informatie](./media/ebsco-tutorial/tutorial_ebsco_url1.png)

    In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen: `http://search.ebscohost.com/login.aspx?authtype=sso&custid=<unique EBSCO customer ID>&profile=<profile ID>`
     
    > [!NOTE] 
    > De waarde van de aanmeldings-URL is geen echte. Werk de waarde met de werkelijke URL voor eenmalige aanmelding. Neem contact op met [EBSCO Client ondersteuningsteam](mailto:sso@ebsco.com) de waarde op te halen. 

    o **unieke elementen:**  

    o **Custid** Enter unieke EBSCO klant-ID = 

    o **profiel** = Clients de koppeling om te leiden gebruikers naar een bepaald profiel (afhankelijk van wat ze bij EBSCO aanschaffen) kunnen aanpassen. Ze kunnen een bepaald profiel-ID invoeren De belangrijkste-id's zijn eds (EBSCO Discovery-Service) en ehost (EBSOCOhost databases). Instructies voor dezelfde krijgen [hier](https://help.ebsco.com/interfaces/EBSCOhost/EBSCOhost_FAQs/How_do_I_set_up_direct_links_to_EBSCOhost_profiles_and_or_databases#profile).

5. De SAML-asserties verwacht EBSCO toepassing in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt beheren de waarden van deze kenmerken van de '**gebruikerskenmerken**' sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze.
    
    ![Eenmalige aanmelding configureren](./media/ebsco-tutorial/tutorial_ebsco_attribute.png)

    > [!Note]
    > De **naam** kenmerk is verplicht en is toegewezen met **gebruikers-id** in EBSCO toepassing. Dit is standaard toegevoegd zodat u niet hoeft dit handmatig toevoegen.
    
6. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding hierboven en voer de volgende stappen uit:
    
    | Naam kenmerk | Waarde kenmerk |
    | ---------------| --------------- |    
    | Voornaam   | User.givenName |
    | LastName   | User.surname |
    | Email   | User.mail |

    a. Klik op **toevoegen kenmerk** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/ebsco-tutorial/tutorial_officespace_04.png)

    ![Eenmalige aanmelding configureren](./media/ebsco-tutorial/tutorial_attribute_05.png)
    
    b. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.
    
    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    d. Klik op **Ok**

7. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/ebsco-tutorial/tutorial_ebsco_certificate.png) 

8. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/ebsco-tutorial/tutorial_general_400.png)
    
9. Eenmalige aanmelding configureren op **EBSCO** zijde, moet u de gedownloade verzenden **Metadata XML** naar [EBSCO ondersteuningsteam](mailto:sso@ebsco.com). Ze deze instelling zodat de SAML SSO-verbinding juist is ingesteld op beide zijden ingesteld.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/ebsco-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/ebsco-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/ebsco-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/ebsco-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-ebsco-test-user"></a>Een testgebruiker EBSCO maken

In het geval van EBSCO is het automatisch gebruikers inrichten.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

Azure AD geeft de vereiste gegevens aan EBSCO toepassing. EBSCO van gebruikersinrichting worden automatische of een eenmalig formulier vereisen. Dit is afhankelijk van of de client een groot heeft aantal reeds bestaande EBSCOhost accounts met persoonlijke instellingen die zijn opgeslagen. Hetzelfde kan worden besproken met de [EBSCO ondersteuningsteam](mailto:sso@ebsco.com) tijdens de implementatie. In beide gevallen moet de client hoeft te maken van accounts EBSCOhost vóór het testen.

   >[!Note]
   >U kunt EBSCOhost inrichting/persoonlijke instellingen automatiseren. Neem contact op met [EBSCO ondersteuningsteam](mailto:sso@ebsco.com) over Just-In-Time gebruikers inrichten. 
 
### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan EBSCO.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen EBSCO, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **EBSCO**.

    ![De koppeling EBSCO in de lijst met toepassingen](./media/ebsco-tutorial/tutorial_ebsco_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

1. Als u op de tegel EBSCO in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing EBSCO.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md).

2. Zodra u zich bij de toepassing aanmelden, klik op de **aanmelden** knop in de rechterbovenhoek.

    ![De aanmelding EBSCO in de lijst met toepassingen](./media/ebsco-tutorial/tutorial_ebsco_signin.png)
 
3. U wordt gevraagd eenmalige te koppelen van de institutionele/SAML-aanmelding met een **nu uw bestaande MyEBSCOhost-account koppelen aan uw account instelling** of **een nieuw MyEBSCOhost-account maken en dit koppelen aan uw account van de instelling**. Het account wordt gebruikt voor persoonlijke instellingen voor de toepassing EBSCOhost. Selecteer de optie **Maak een nieuw account** en u ziet dat het formulier voor persoonlijke instellingen vooraf ingevuld met de waarden uit de saml-reactie is, zoals wordt weergegeven in de onderstaande schermafbeelding. Klik op **'Doorgaan'** deze selectie opslaan.
    
     ![De gebruiker EBSCO in de lijst met toepassingen](./media/ebsco-tutorial/tutorial_ebsco_user.png)

4. Na het voltooien van de bovenstaande instellingen, schakelt u cookies cache/en meld u opnieuw. U hoeft te handmatig aanmelding opnieuw en de instellingen voor personalisatie worden onthouden

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



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

