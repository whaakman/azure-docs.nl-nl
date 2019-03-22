---
title: 'Zelfstudie: Azure Active Directory-integratie met interactieve handleidingen Yonyx | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Yonyx interactieve gidsen.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 07db4e01-319b-4cb6-9b93-4577bffd3cbc
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/16/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: fce7653770d0fb3134f266137e167125b9161865
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58097590"
---
# <a name="tutorial-azure-active-directory-integration-with-yonyx-interactive-guides"></a>Zelfstudie: Azure Active Directory-integratie met interactieve Yonyx-handleidingen

In deze zelfstudie leert u hoe u interactieve handleidingen Yonyx integreren met Azure Active Directory (Azure AD).

Interactieve handleidingen Yonyx integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Yonyx interactieve handleidingen heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij interactieve handleidingen Yonyx (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Yonyx interactieve handleidingen, moet u de volgende items:

- Een Azure AD-abonnement
- Een interactieve handleidingen Yonyx eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Interactieve handleidingen Yonyx uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-yonyx-interactive-guides-from-the-gallery"></a>Interactieve handleidingen Yonyx uit de galerie toe te voegen
Voor het configureren van de integratie van Yonyx interactieve handleidingen in Azure AD, moet u interactieve handleidingen Yonyx uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u interactieve hulplijnen Yonyx uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **Yonyx interactieve handleidingen**, selecteer **Yonyx interactieve handleidingen** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Yonyx interactieve handleidingen in de lijst met resultaten](./media/yonyx-tutorial/tutorial_yonyxinteractiveguides_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Yonyx interactieve handleidingen op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in de interactieve handleidingen Yonyx is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in de interactieve handleidingen Yonyx tot stand worden gebracht.

In de interactieve handleidingen Yonyx, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Yonyx interactieve handleidingen, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker Yonyx interactieve handleidingen](#create-a-yonyx-interactive-guides-test-user)**  : als u wilt een equivalent van Britta Simon in Yonyx interactieve handleidingen die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Yonyx interactieve gidsen.

**Voor het configureren van Azure AD eenmalige aanmelding met Yonyx interactieve handleidingen, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Yonyx interactieve handleidingen** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/yonyx-tutorial/tutorial_yonyxinteractiveguides_samlbase.png)

1. Op de **Yonyx interactieve handleidingen domein en URL's** sectie, voert u de volgende stappen uit:

    ![Yonyx interactieve handleidingen domein en URL's, eenmalige aanmelding informatie](./media/yonyx-tutorial/tutorial_yonyxinteractiveguides_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<company name>.yonyx.com/y/conversation/?id=<guid number>`

    b. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<company name>.yonyx.com`

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de daadwerkelijke aanmeldings-URL en id. Neem contact op met [Yonyx interactieve handleidingen Client ondersteuningsteam](mailto:support@yonyx.com) om deze waarden te verkrijgen. 
 
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![De link om het certificaat te downloaden](./media/yonyx-tutorial/tutorial_yonyxinteractiveguides_certificate.png) 

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/yonyx-tutorial/tutorial_general_400.png)

1. Op de **Yonyx interactieve handleidingen configuratie** sectie, klikt u op **Yonyx interactieve handleidingen configureren** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Configuratie van Yonyx interactieve handleidingen](./media/yonyx-tutorial/tutorial_yonyxinteractiveguides_configure.png) 

1. Het configureren van eenmalige aanmelding op **Yonyx interactieve handleidingen** zijde, moet u voor het verzenden van de gedownloade **Certificate(Base64)**, **afmelding URL**, **SAML-eenmalige Aanmeldings-URL van Service** **SAML entiteit-ID** naar [Yonyx interactieve handleidingen ondersteuningsteam](mailto:support@yonyx.com). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

  ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](./media/yonyx-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![De koppelingen Gebruikers en groepen en Alle gebruikers](./media/yonyx-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![De knop toevoegen](./media/yonyx-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het dialoogvenster Gebruiker](./media/yonyx-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="create-a-yonyx-interactive-guides-test-user"></a>Maak een testgebruiker Yonyx interactieve handleidingen

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Yonyx interactieve handleidingen. Yonyx interactieve handleidingen biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld.

Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot Yonyx interactieve handleidingen als deze nog niet bestaat.

> [!NOTE]
> Als u een gebruiker handmatig hebt gemaakt wilt, moet u contact op met het ondersteuningsteam van het interactieve handleidingen Yonyx via <mailto:support@yonyx.com>. 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Yonyx interactieve hulplijnen.

![De de gebruikersrol toewijzen][200]

**Als u wilt toewijzen Britta Simon Yonyx interactieve handleidingen, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Yonyx interactieve handleidingen**.

    ![De koppeling Yonyx interactieve handleidingen in de lijst met toepassingen](./media/yonyx-tutorial/tutorial_yonyxinteractiveguides_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Yonyx interactieve handleidingen in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Yonyx interactieve gidsen.

Zie [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/yonyx-tutorial/tutorial_general_01.png
[2]: ./media/yonyx-tutorial/tutorial_general_02.png
[3]: ./media/yonyx-tutorial/tutorial_general_03.png
[4]: ./media/yonyx-tutorial/tutorial_general_04.png

[100]: ./media/yonyx-tutorial/tutorial_general_100.png

[200]: ./media/yonyx-tutorial/tutorial_general_200.png
[201]: ./media/yonyx-tutorial/tutorial_general_201.png
[202]: ./media/yonyx-tutorial/tutorial_general_202.png
[203]: ./media/yonyx-tutorial/tutorial_general_203.png

