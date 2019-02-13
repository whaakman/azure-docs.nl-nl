---
title: 'Zelfstudie: Azure Active Directory-integratie met Leapsome | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Leapsome.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: cb523e97-add8-4289-b106-927bf1a02188
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 37549cc76e1490b0758de8e296523b0e70c98dbf
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56191228"
---
# <a name="tutorial-azure-active-directory-integration-with-leapsome"></a>Zelfstudie: Azure Active Directory-integratie met Leapsome

In deze zelfstudie leert u hoe u Leapsome integreren met Azure Active Directory (Azure AD).

Leapsome integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Leapsome heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Leapsome (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Leapsome, moet u de volgende items:

- Een Azure AD-abonnement
- Een Leapsome eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Leapsome uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-leapsome-from-the-gallery"></a>Leapsome uit de galerie toe te voegen
Voor het configureren van de integratie van Leapsome in Azure AD, moet u Leapsome uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Leapsome uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **Leapsome**, selecteer **Leapsome** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Leapsome in de lijst met resultaten](./media/leapsome-tutorial/tutorial_leapsome_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Leapsome op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Leapsome is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Leapsome tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Leapsome, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker Leapsome](#create-a-leapsome-test-user)**  : als u wilt een equivalent van Britta Simon in Leapsome die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Leapsome.

**Voor het configureren van Azure AD eenmalige aanmelding met Leapsome, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Leapsome** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/leapsome-tutorial/tutorial_leapsome_samlbase.png)

1. Op de **Leapsome domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Leapsome domein en URL's, eenmalige aanmelding informatie](./media/leapsome-tutorial/tutorial_leapsome_url.png)

    a. In de **id** tekstvak, een URL typen: `https://www.leapsome.com`

    b. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://www.leapsome.com/api/users/auth/saml/<CLIENTID>/assert`

1. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Leapsome domein en URL's, eenmalige aanmelding informatie](./media/leapsome-tutorial/tutorial_leapsome_url1.png)

    Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://www.leapsome.com/api/users/auth/saml/<CLIENTID>/login`
     
    > [!NOTE] 
    > De voorgaande antwoord-URL en de aanmeldings-URL-waarde is geen echte waarde. U werkt met de werkelijke waarden, die later in de zelfstudie wordt uitgelegd.

1. Leapsome toepassing verwacht het SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit de sectie **Gebruikerskenmerken** op de integratiepagina van de toepassing-beheren. De volgende Schermafbeelding toont een voorbeeld.
    
    ![Eenmalige aanmelding configureren](./media/leapsome-tutorial/tutorial_Leapsome_attribute.png)

1. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de bovenstaande afbeelding en voer de volgende stappen uit:
    
    | Naam kenmerk | Waarde kenmerk | Naamruimte |
    | ---------------| --------------- | --------- |   
    | firstname | user.givenname | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | lastname | user.surname | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | titel | user.jobtitle | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | afbeelding | URL van afbeelding van de werknemer | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |

    > [!Note]
    > De waarde van afbeelding kenmerk is niet echt. Deze waarde bijwerken met de werkelijke afbeeldings-URL. Om op te halen deze waarde contact op met [Leapsome Client ondersteuningsteam](mailto:support@leapsome.com).
    
    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/leapsome-tutorial/tutorial_attribute_04.png)

    ![Eenmalige aanmelding configureren](./media/leapsome-tutorial/tutorial_attribute_05.png)
    
    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.
    
    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. In de **Namespace** tekstvak typt u de naamruimte-uri voor die rij.
    
    e. Klik op **OK**.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![De link om het certificaat te downloaden](./media/leapsome-tutorial/tutorial_leapsome_certificate.png) 

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/leapsome-tutorial/tutorial_general_400.png)
    
1. Op de **Leapsome configuratie** sectie, klikt u op **configureren Leapsome** openen **aanmelding configureren** venster. Kopiëren de **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![Leapsome configuratie](./media/leapsome-tutorial/tutorial_leapsome_configure.png)

1. In een ander browservenster aanmelden bij Leapsome als een beveiligingsbeheerder.

1. Klik op instellingen logo in de rechterbovenhoek, en klik vervolgens op **beheerdersinstellingen**. 

    ![Leapsome instellen](./media/leapsome-tutorial/tutorial_leapsome_admin.png)

1. Klik op de linkermenubalk **eenmalige aanmelding (SSO)**, en klik op de **SAML gebaseerde eenmalige aanmelding (SSO)** pagina de volgende stappen uitvoeren:
    
    ![Leapsome saml](./media/leapsome-tutorial/tutorial_leapsome_samlsettings.png)

    a. Selecteer **inschakelen SAML gebaseerde eenmalige aanmelding**.

    b. Kopieer de **aanmeldings-URL (uw gebruikers hier punt voor het starten van aanmelding)** waarde en plak deze in de **aanmeldings-URL** -tekstvak in **Leapsome domein en URL's** sectie in Azure portal.

    c. Kopieer de **antwoord-URL (antwoord ontvangt van uw id-provider)** waarde en plak deze in de **antwoord-URL** -tekstvak in **Leapsome domein en URL's** sectie in Azure portal.

    d. In de **SSO-aanmeldings-URL (geleverd door de id-provider)** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML**, die u hebt gekopieerd vanuit Azure portal.

    e. Kopieer het certificaat dat u hebt gedownload vanuit Azure portal zonder--certificaat BEGIN en einde certificaat--opmerkingen en plak deze in de **certificaat (geleverd door de id-provider)** tekstvak.

    f. Klik op **UPDATE SSO-instellingen**.
    
### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/leapsome-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/leapsome-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/leapsome-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/leapsome-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-leapsome-test-user"></a>Maak een testgebruiker Leapsome

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Leapsome. Werken met [Leapsome Client ondersteuningsteam](mailto:support@leapsome.com) om toe te voegen de gebruikers of het domein dat moet worden opgenomen in de whitelist in het Leapsome-platform. Als het domein is toegevoegd door het team, krijgen automatisch gebruikers ingericht voor het Leapsome-platform. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken. 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Leapsome.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan Leapsome toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Leapsome**.

    ![De koppeling Leapsome in de lijst met toepassingen](./media/leapsome-tutorial/tutorial_leapsome_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Leapsome in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Leapsome.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/leapsome-tutorial/tutorial_general_01.png
[2]: ./media/leapsome-tutorial/tutorial_general_02.png
[3]: ./media/leapsome-tutorial/tutorial_general_03.png
[4]: ./media/leapsome-tutorial/tutorial_general_04.png

[100]: ./media/leapsome-tutorial/tutorial_general_100.png

[200]: ./media/leapsome-tutorial/tutorial_general_200.png
[201]: ./media/leapsome-tutorial/tutorial_general_201.png
[202]: ./media/leapsome-tutorial/tutorial_general_202.png
[203]: ./media/leapsome-tutorial/tutorial_general_203.png

