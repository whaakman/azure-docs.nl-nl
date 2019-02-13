---
title: 'Zelfstudie: Azure Active Directory-integratie met SpaceIQ | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SpaceIQ.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 5b55ae29-491f-401f-9299-d3a6b64a1b99
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/04/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6695fe4854c6d91d3d2ba671104d1b481721356c
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56162268"
---
# <a name="tutorial-azure-active-directory-integration-with-spaceiq"></a>Zelfstudie: Azure Active Directory-integratie met SpaceIQ

In deze zelfstudie leert u hoe u SpaceIQ integreren met Azure Active Directory (Azure AD).

SpaceIQ integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot SpaceIQ heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij SpaceIQ (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SpaceIQ, moet u de volgende items:

- Een Azure AD-abonnement
- Een SpaceIQ eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SpaceIQ uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-spaceiq-from-the-gallery"></a>SpaceIQ uit de galerie toe te voegen
Voor het configureren van de integratie van SpaceIQ in Azure AD, moet u SpaceIQ uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SpaceIQ uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **SpaceIQ**, selecteer **SpaceIQ** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![SpaceIQ in de lijst met resultaten](./media/spaceiq-tutorial/tutorial_spaceiq_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met SpaceIQ op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in SpaceIQ is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in SpaceIQ tot stand worden gebracht.

In SpaceIQ, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met SpaceIQ, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker SpaceIQ](#create-a-spaceiq-test-user)**  : als u wilt een equivalent van Britta Simon in SpaceIQ die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing SpaceIQ.

**Voor het configureren van Azure AD eenmalige aanmelding met SpaceIQ, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **SpaceIQ** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/spaceiq-tutorial/tutorial_spaceiq_samlbase.png)

1. Op de **SpaceIQ domein en URL's** sectie, voert u de volgende stappen uit:

    ![SpaceIQ domein en URL's, eenmalige aanmelding informatie](./media/spaceiq-tutorial/tutorial_spaceiq_url.png)

    a. In de **id** tekstvak typt u de URL: `https://api.spaceiq.com`

    b. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://api.spaceiq.com/saml/<instanceid>/callback`

    > [!NOTE] 
    > Werk deze waarden met de werkelijke antwoord-URL en -id in die later in de zelfstudie wordt uitgelegd.
 
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **(Base64-certificaat)** en slaat u het certificaatbestand op uw computer.

    ![De link om het certificaat te downloaden](./media/spaceiq-tutorial/tutorial_spaceiq_certificate.png) 

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/spaceiq-tutorial/tutorial_general_400.png)

1. Op de **SpaceIQ configuratie** sectie, klikt u op **configureren SpaceIQ** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID** uit de **Naslaggids sectie.**

    ![SpaceIQ configuratie](./media/spaceiq-tutorial/tutorial_spaceiq_configure.png) 

1.  Open een nieuw browservenster en meld u aan uw omgeving SpaceIQ als beheerder.

1. Nadat u bent aangemeld, klikt u op het teken puzzel in de rechterbovenhoek en klik op **"Integraties"**

    ![Accountinstellingen](./media/spaceiq-tutorial/setting1.png) 

1. Onder **alle inrichting & SSO**, klikt u op de **Azure** tegel toevoegen een exemplaar van Azure als id-provider.

    ![SAML-pictogram](./media/spaceiq-tutorial/setting2.png)

1. In de **SSO** dialoogvenster vak, voer de volgende stappen uit:

    ![SAML-verificatie-instellingen](./media/spaceiq-tutorial/setting3.png)

    a. In de **URL voor SAML-verlener** vak, plak de **SAML entiteit-ID** waarde opgehaald uit het Configuratievenster van Azure AD-toepassing.
    
    b. Kopieer de **SAML CallBack eindpunt-URL (alleen-lezen)** waarde en plak de waarde in de **antwoord-URL** vak in de Azure-portal onder SpaceIQ **domein en URL's** sectie.
    
    c. Kopieer de **SAML doelgroep-URI (alleen-lezen)** waarde en plak de waarde in de **id** vak in de Azure-portal onder SpaceIQ **domein en URL's** sectie.

    d. Open het gedownloade certificaat-bestand in Kladblok, Kopieer de inhoud en plak deze in de **X.509-certificaat** vak.
    
    e. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/spaceiq-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/spaceiq-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/spaceiq-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/spaceiq-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
  
### <a name="create-a-spaceiq-test-user"></a>Maak een testgebruiker SpaceIQ

In deze sectie maakt u een gebruiker met de naam van Britta Simon in SpaceIQ. Werk [SpaceIQ ondersteuningsteam](mailto:eng@spaceiq.com) om toe te voegen de gebruikers in het SpaceIQ-platform. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan SpaceIQ.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan SpaceIQ toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **SpaceIQ**.

    ![De koppeling SpaceIQ in de lijst met toepassingen](./media/spaceiq-tutorial/tutorial_spaceiq_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel SpaceIQ in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing SpaceIQ.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/spaceiq-tutorial/tutorial_general_01.png
[2]: ./media/spaceiq-tutorial/tutorial_general_02.png
[3]: ./media/spaceiq-tutorial/tutorial_general_03.png
[4]: ./media/spaceiq-tutorial/tutorial_general_04.png

[100]: ./media/spaceiq-tutorial/tutorial_general_100.png

[200]: ./media/spaceiq-tutorial/tutorial_general_200.png
[201]: ./media/spaceiq-tutorial/tutorial_general_201.png
[202]: ./media/spaceiq-tutorial/tutorial_general_202.png
[203]: ./media/spaceiq-tutorial/tutorial_general_203.png

