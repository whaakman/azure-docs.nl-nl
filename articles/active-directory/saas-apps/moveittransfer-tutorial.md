---
title: 'Zelfstudie: Azure Active Directory-integratie met MOVEit overdracht - Azure AD-integratie | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en MOVEit overdracht - Azure AD-integratie.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 8ff7102d-be73-4888-ae81-d8e3d01dd534
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: 671d3c9954321bf19d0fd56057d05d8b4475d8d2
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55166064"
---
# <a name="tutorial-azure-active-directory-integration-with-moveit-transfer---azure-ad-integration"></a>Zelfstudie: Azure Active Directory-integratie met MOVEit overdracht - Azure AD-integratie

In deze zelfstudie leert u hoe u om te integreren, overdracht MOVEit - Azure AD-integratie met Azure Active Directory (Azure AD).

Integratie van MOVEit overdracht - Azure AD-integratie met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot MOVEit overdracht - Azure AD-integratie heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij MOVEit overdracht - Azure AD-integratie met hun Azure AD-accounts (Single Sign-On) inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met MOVEit overdracht - integratie van Azure AD, moet u de volgende items:

- Een Azure AD-abonnement
- Een overdracht MOVEit - Azure AD-integratie eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. MOVEit overdracht - Azure AD-integratie uit de galerie toevoegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-moveit-transfer---azure-ad-integration-from-the-gallery"></a>MOVEit overdracht - Azure AD-integratie uit de galerie toevoegen
Als u wilt configureren van de integratie van MOVEit overdracht - Azure AD-integratie met Azure AD, moet u MOVEit overdracht - Azure AD-integratie uit de galerie aan de lijst van beheerde SaaS-apps toevoegen.

**Als u wilt toevoegen MOVEit overdracht - Azure AD-integratie uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **MOVEit overdracht - Azure AD-integratie**, selecteer **MOVEit overdracht - Azure AD-integratie** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen de de toepassing.

    ![MOVEit overdracht - Azure AD-integratie in de lijst met resultaten](./media/moveittransfer-tutorial/tutorial_moveittransfer_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met MOVEit overdracht - Azure AD-integratie op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in MOVEit overdracht - Azure AD-integratie is aan een gebruiker in Azure AD. Met andere woorden, een koppeling de relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in MOVEit overdracht - Azure AD-integratie moet tot stand worden gebracht.

In MOVEit overdracht - Azure AD-integratie, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met MOVEit overdracht - de Azure AD-integratie, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maken van een overdracht MOVEit - gebruiker van Azure AD-integratie testen](#create-a-moveit-transfer---azure-ad-integration-test-user)**  : als u wilt een equivalent van Britta Simon in MOVEit overdracht - integratie van Azure AD die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw MOVEit overdracht - toepassing voor Azure AD-integratie.

**Voor het configureren van Azure AD eenmalige aanmelding met MOVEit overdracht - integratie van Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **MOVEit overdracht - Azure AD-integratie** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/moveittransfer-tutorial/tutorial_moveittransfer_samlbase.png)

1. Op de **MOVEit overdracht - URL's en integratie van Azure AD Domain** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/moveittransfer-tutorial/tutorial_moveittransfer_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://contoso.com`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://contoso.com/<tenatid>`

    c. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://contoso.com/<tenatid>/SAML/SSO/HTTP-Post`    
     
    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke id, antwoord-URL en aanmeldings-URL. U kunt deze waarden later in verwijzen **metagegevens-URL van Service Provider** sectie of neem contact op met [MOVEit overdracht - ondersteuningsteam voor Azure AD-integratie-Client](https://community.ipswitch.com/s/support) om deze waarden te verkrijgen.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/moveittransfer-tutorial/tutorial_moveittransfer_certificate.png) 

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/moveittransfer-tutorial/tutorial_general_400.png)
    
1. Meld u aan bij uw tenant MOVEit overdracht als beheerder.

1. Klik in het linkernavigatiedeelvenster op **instellingen**.

    ![Instellingen voor sectie op App aan clientzijde](./media/moveittransfer-tutorial/tutorial_moveittransfer_000.png)

1. Klik op **eenmalige aanmelding** koppeling naar deze bevindt zich onder **beveiligingsbeleid -> verificatie van de gebruiker**.

    ![Beveiliging-beleid op App aan clientzijde](./media/moveittransfer-tutorial/tutorial_moveittransfer_001.png)

1. Klik op de koppeling van de metagegevens-URL om de metadata-document te downloaden.

    ![Serviceprovider metagegevens-URL](./media/moveittransfer-tutorial/tutorial_moveittransfer_002.png)
    
    * Controleer of **entityID** komt overeen met **id** in de **MOVEit overdracht - URL's en integratie van Azure AD Domain** sectie.
    * Controleer of **AssertionConsumerService** locatie-URL overeenkomt met **antwoord-URL** in de **MOVEit overdracht - URL's en integratie van Azure AD Domain** sectie.
    
    ![Configureren van eenmalige aanmelding op App-zijde](./media/moveittransfer-tutorial/tutorial_moveittransfer_007.png)

1. Klik op **id-Provider toevoegen** om toe te voegen een nieuwe federatieve id-Provider.

    ![Id-provider toevoegen](./media/moveittransfer-tutorial/tutorial_moveittransfer_003.png)

1. Klik op **bladeren...**  om te selecteren in het bestand met metagegevens die u hebt gedownload van Azure portal, klik vervolgens op **id-Provider toevoegen** het gedownloade bestand te uploaden.

    ![SAML-identiteitsprovider](./media/moveittransfer-tutorial/tutorial_moveittransfer_004.png)

1. Selecteer "**Ja**' als **ingeschakeld** in de **bewerken voor federatieve identiteit Providerinstellingen...**  pagina en klik op **opslaan**.

    ![Instellingen van de federatieve identiteit](./media/moveittransfer-tutorial/tutorial_moveittransfer_005.png)

1. In de **bewerken voor federatieve identiteit Provider gebruikersinstellingen** pagina, de volgende acties uitvoeren:
    
    ![Instellingen van de federatieve identiteit bewerken](./media/moveittransfer-tutorial/tutorial_moveittransfer_006.png)
    
    a. Selecteer **NameID van SAML** als **aanmeldingsnaam**.
    
    b. Selecteer **andere** als **volledige naam** en in de **kenmerknaam** tekstvak plaatst de waarde: `http://schemas.microsoft.com/identity/claims/displayname`.
    
    c. Selecteer **andere** als **e** en in de **kenmerknaam** tekstvak plaatst de waarde: `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.
    
    d. Selecteer **Ja** als **-account maken met een automatisch aanmeldt**.
    
    e. Klik op de knop **Save**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/moveittransfer-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/moveittransfer-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/moveittransfer-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/moveittransfer-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-moveit-transfer---azure-ad-integration-test-user"></a>Maken van een overdracht MOVEit - gebruiker van Azure AD-integratie testen

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in MOVEit overdracht - Azure AD-integratie. MOVEit overdracht - integratie van Azure AD biedt ondersteuning voor just-in-time inrichting, die u hebt ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot MOVEit overdracht - integratie van Azure AD als deze nog niet bestaat.

>[!NOTE]
>Als u een gebruiker handmatig hebt gemaakt wilt, moet u contact op met de [MOVEit overdracht - ondersteuningsteam voor Azure AD-integratie Client](https://community.ipswitch.com/s/support).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan MOVEit overdracht - Azure AD-integratie.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon MOVEit overdracht - integratie van Azure AD, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **MOVEit overdracht - Azure AD-integratie**.

    ![De overdracht MOVEit - Azure AD-integratie koppeling in de lijst met toepassingen](./media/moveittransfer-tutorial/tutorial_moveittransfer_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van de configuratie van uw Azure AD-eenmalige aanmelding via het toegangsvenster.

Wanneer u klikt op de overdracht MOVEit - tegel Azure AD-integratie in het toegangsvenster, u moet u automatisch aangemeld bij de overdracht van uw MOVEit - toepassing voor Azure AD-integratie. 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)


<!--Image references-->

[1]: ./media/moveittransfer-tutorial/tutorial_general_01.png
[2]: ./media/moveittransfer-tutorial/tutorial_general_02.png
[3]: ./media/moveittransfer-tutorial/tutorial_general_03.png
[4]: ./media/moveittransfer-tutorial/tutorial_general_04.png

[100]: ./media/moveittransfer-tutorial/tutorial_general_100.png

[200]: ./media/moveittransfer-tutorial/tutorial_general_200.png
[201]: ./media/moveittransfer-tutorial/tutorial_general_201.png
[202]: ./media/moveittransfer-tutorial/tutorial_general_202.png
[203]: ./media/moveittransfer-tutorial/tutorial_general_203.png

