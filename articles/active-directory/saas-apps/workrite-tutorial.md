---
title: 'Zelfstudie: Azure Active Directory-integratie met Workrite | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Workrite.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 2a5c2956-a011-4d5c-877b-80679b6587b5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 1e593d66e7f293a5e84200eff0980b1cdf6d55dd
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55203127"
---
# <a name="tutorial-azure-active-directory-integration-with-workrite"></a>Zelfstudie: Azure Active Directory-integratie met Workrite

In deze zelfstudie leert u hoe u Workrite integreren met Azure Active Directory (Azure AD).

Workrite integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Workrite heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Workrite (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Workrite, moet u de volgende items:

- Een Azure AD-abonnement
- Een Workrite eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Workrite uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-workrite-from-the-gallery"></a>Workrite uit de galerie toe te voegen
Voor het configureren van de integratie van Workrite in Azure AD, moet u Workrite uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Workrite uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **Workrite**, selecteer **Workrite** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Workrite in de lijst met resultaten](./media/workrite-tutorial/tutorial_workrite_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Workrite op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Workrite is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Workrite tot stand worden gebracht.

In Workrite, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Workrite, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker Workrite](#create-a-workrite-test-user)**  : als u wilt een equivalent van Britta Simon in Workrite die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Workrite.

**Voor het configureren van Azure AD eenmalige aanmelding met Workrite, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Workrite** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/workrite-tutorial/tutorial_workrite_samlbase.png)

1. Op de **Workrite domein en URL's** sectie, voert u de volgende stappen uit:

    ![Workrite domein en URL's, eenmalige aanmelding informatie](./media/workrite-tutorial/tutorial_workrite_url.png)

    Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://app.workrite.co.uk/securelogin/samlgateway.aspx?id=<uniqueid>`

    > [!NOTE] 
    > Deze waarde is niet echt. Deze waarde bijwerken met de werkelijke aanmeldings-URL. Neem contact op met [Workrite Client ondersteuningsteam](mailto:support@workrite.co.uk) deze waarde op te halen.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![De link om het certificaat te downloaden](./media/workrite-tutorial/tutorial_workrite_certificate.png) 

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/workrite-tutorial/tutorial_general_400.png)

1. Op de **Workrite configuratie** sectie, klikt u op **configureren Workrite** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Workrite configuratie](./media/workrite-tutorial/tutorial_workrite_configure.png) 

1. Het configureren van eenmalige aanmelding op **Workrite** zijde, moet u voor het verzenden van de gedownloade **Certificate(Base64), URL van de afmelding, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** naar [Workrite ondersteuning voor team](mailto:support@workrite.co.uk).

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/workrite-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/workrite-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/workrite-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/workrite-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-workrite-test-user"></a>Maak een testgebruiker Workrite

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Workrite.

**Voor het maken van een gebruiker met de naam van Britta Simon in Workrite, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf workrite site als administrator.

1. Klik in het navigatievenster op **Admin**.
   
    ![-Beheer][400]

1. Ga naar Quick Links en klik vervolgens op **maken van een gebruiker**.
   
    ![Sectie van de gebruiker maken][401]

1. Op de **Create User** dialoogvenster, voer de volgende stappen uit:
   
    ![Gebruiker Dailog maken][402]
    
    a. Typ in het tekstvak **Email** het e-mailadres van de gebruiker, bijvoorbeeld Brittasimon@contoso.com.

    b. In de **voornaam** tekstvak typt u de voornaam van de gebruiker, zoals Julia.

    c. In de **achternaam** tekstvak typt u de achternaam van de gebruiker, zoals Simon.
    
    d. Selecteer **Clientbeheerder** als **Kies rol**.
    
    e. Klik op **Opslaan**.   

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Workrite.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan Workrite toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Workrite**.

    ![De koppeling Workrite in de lijst met toepassingen](./media/workrite-tutorial/tutorial_workrite_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van de configuratie van uw Azure AD-eenmalige aanmelding via het toegangsvenster.

Wanneer u op de tegel Workrite in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Workrite.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/workrite-tutorial/tutorial_general_01.png
[2]: ./media/workrite-tutorial/tutorial_general_02.png
[3]: ./media/workrite-tutorial/tutorial_general_03.png
[4]: ./media/workrite-tutorial/tutorial_general_04.png

[100]: ./media/workrite-tutorial/tutorial_general_100.png

[200]: ./media/workrite-tutorial/tutorial_general_200.png
[201]: ./media/workrite-tutorial/tutorial_general_201.png
[202]: ./media/workrite-tutorial/tutorial_general_202.png
[203]: ./media/workrite-tutorial/tutorial_general_203.png
[400]: ./media/workrite-tutorial/tutorial_workrite_400.png
[401]: ./media/workrite-tutorial/tutorial_workrite_401.png
[402]: ./media/workrite-tutorial/tutorial_workrite_402.png

