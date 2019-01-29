---
title: 'Zelfstudie: Azure Active Directory-integratie met Front | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en de voorzijde.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 88270b6d-2571-434a-b139-b6ccc3a2b19f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: jeedes
ms.openlocfilehash: 95b6565885435aeb964dd37bd49ae5a05c06be94
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55175465"
---
# <a name="tutorial-azure-active-directory-integration-with-front"></a>Zelfstudie: Azure Active Directory-integratie met front-end

In deze zelfstudie leert u over het integreren van voorgrond met Azure Active Directory (Azure AD).

Integratie van voorgrond in Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de front-end heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld naar voorgrond (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met front-end, moet u de volgende items:

- Een Azure AD-abonnement
- Een front-end eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toevoegen van voorgrond uit de galerie
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-front-from-the-gallery"></a>Toevoegen van voorgrond uit de galerie
Voor het configureren van de integratie van voorgrond in Azure AD, moet u Front uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen voor uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **Front**, selecteer **Front** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Begin in de lijst met resultaten](./media/front-tutorial/tutorial_front_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met front-end op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat het equivalent van de gebruiker op de voorgrond is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker op de voorgrond tot stand worden gebracht.

Op de voorgrond, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met front-end, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker van voorgrond](#create-a-front-test-user)**  : als u wilt een equivalent van Britta Simon voorkant die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw Front-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met front-end, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Front** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/front-tutorial/tutorial_front_samlbase.png)

1. Op de **Front domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/front-tutorial/tutorial_front_url1.png)

    a. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<companyname>.frontapp.com`

    b. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://<companyname>.frontapp.com/sso/saml/callback`
     
    > [!NOTE] 
    > Dit zijn geen echte waarden. Deze waarden bijwerken met de werkelijke id en de antwoord-URL die worden beschreven later in de zelfstudie of neem contact op met [Front-Client-ondersteuningsteam](mailto:support@frontapp.com) om deze waarden te verkrijgen. 

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/front-tutorial/tutorial_front_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/front-tutorial/tutorial_general_400.png)
    
1. Op de **Front configuratie** sectie, klikt u op **Front configureren** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/front-tutorial/tutorial_front_configure.png) 

1. Aanmelding met uw Front-tenant als beheerder.

1. Ga naar **instellingen (tandwielpictogram aan de onderkant van de linkerzijbalk) > Voorkeuren**.
   
    ![Configureren van eenmalige aanmelding op App-zijde](./media/front-tutorial/tutorial_front_000.png)

1. Klik op **Single Sign On** koppeling.
   
    ![Configureren van eenmalige aanmelding op App-zijde](./media/front-tutorial/tutorial_front_001.png)

1. Selecteer **SAML** in de vervolgkeuzelijst van **Single Sign On**.
   
    ![Configureren van eenmalige aanmelding op App-zijde](./media/front-tutorial/tutorial_front_002.png)

1. In de **toegangspunt** tekstvak plaatst de waarde van **Single Sign-on Service URL** van Azure AD-wizard voor het configureren van toepassing.
    
    ![Configureren van eenmalige aanmelding op App-zijde](./media/front-tutorial/tutorial_front_003.png)

1. Open uw gedownloade **Certificate(Base64)** -bestand in Kladblok, Kopieer de inhoud ervan in het Klembord en plakt u deze naar de **handtekeningcertificaat** tekstvak.
    
    ![Configureren van eenmalige aanmelding op App-zijde](./media/front-tutorial/tutorial_front_004.png)

1. Op de **Service-Providerinstellingen** sectie, voert u de volgende stappen uit:

    ![Configureren van eenmalige aanmelding op App-zijde](./media/front-tutorial/tutorial_front_005.png)

    a. Kopieer de waarde van **entiteit-ID** en plak deze in de **id** -tekstvak in **Front domein en URL's** sectie in Azure portal.

    b. Kopieer de waarde van **ACS URL** en plak deze in de **antwoord-URL** -tekstvak in **Front domein en URL's** sectie in Azure portal.
    
1. Klik op de knop **Save**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/front-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/front-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/front-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/front-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-front-test-user"></a>Maak een testgebruiker van voorgrond

In deze sectie maakt u een gebruiker met de naam Britta Simon op de voorgrond. Werken met [Front-Client-ondersteuningsteam](mailto:support@frontapp.com) om toe te voegen de gebruikers in de Front-platform. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan begin.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon naar voorgrond, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Front**.

    ![De Front-koppeling in de lijst met toepassingen](./media/front-tutorial/tutorial_front_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van uw Azure AD-SSOconfiguration via het toegangsvenster.

Wanneer u op de Front-tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw Front-toepassing. 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/front-tutorial/tutorial_general_01.png
[2]: ./media/front-tutorial/tutorial_general_02.png
[3]: ./media/front-tutorial/tutorial_general_03.png
[4]: ./media/front-tutorial/tutorial_general_04.png

[100]: ./media/front-tutorial/tutorial_general_100.png

[200]: ./media/front-tutorial/tutorial_general_200.png
[201]: ./media/front-tutorial/tutorial_general_201.png
[202]: ./media/front-tutorial/tutorial_general_202.png
[203]: ./media/front-tutorial/tutorial_general_203.png

