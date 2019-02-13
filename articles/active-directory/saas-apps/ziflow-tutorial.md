---
title: 'Zelfstudie: Azure Active Directory-integratie met Ziflow | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Ziflow.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 84e60fa4-36fb-49c4-a642-95538c78f926
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 175e678365016bafd3d18f590a5434c32ac9fadd
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56176472"
---
# <a name="tutorial-azure-active-directory-integration-with-ziflow"></a>Zelfstudie: Azure Active Directory-integratie met Ziflow

In deze zelfstudie leert u hoe u Ziflow integreren met Azure Active Directory (Azure AD).

Ziflow integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Ziflow heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Ziflow (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Ziflow, moet u de volgende items:

- Een Azure AD-abonnement
- Een Ziflow eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Ziflow uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-ziflow-from-the-gallery"></a>Ziflow uit de galerie toe te voegen
Voor het configureren van de integratie van Ziflow in Azure AD, moet u Ziflow uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Ziflow uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

4. Typ in het zoekvak **Ziflow**, selecteer **Ziflow** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Ziflow in de lijst met resultaten](./media/ziflow-tutorial/tutorial_ziflow_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Ziflow op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Ziflow is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Ziflow tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Ziflow, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
3. **[Maak een testgebruiker Ziflow](#create-a-ziflow-test-user)**  : als u wilt een equivalent van Britta Simon in Ziflow die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Ziflow.

**Voor het configureren van Azure AD eenmalige aanmelding met Ziflow, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Ziflow** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![In het dialoogvenster voor eenmalige aanmelding](./media/ziflow-tutorial/tutorial_ziflow_samlbase.png)

3. Op de **Ziflow domein en URL's** sectie, voert u de volgende stappen uit:

    ![Ziflow domein en URL's, eenmalige aanmelding informatie](./media/ziflow-tutorial/tutorial_ziflow_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://ziflow-production.auth0.com/login/callback?connection=<UniqueID>`

    b. Typ in het tekstvak **Id** een URL met het volgende patroon: `urn:auth0:ziflow-production:<UniqueID>`

    > [!NOTE]
    > De bovenstaande waarden zijn niet echt. U kunt de unieke id-waarde in de id en de aanmeldings-URL wordt bijgewerkt met de werkelijke waarde, die later in de zelfstudie wordt uitgelegd.

4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![De link om het certificaat te downloaden](./media/ziflow-tutorial/tutorial_ziflow_certificate.png) 

5. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/ziflow-tutorial/tutorial_general_400.png)

6. Op de **Ziflow configuratie** sectie, klikt u op **configureren Ziflow** openen **aanmelding configureren** venster. Kopiëren de **afmelding URL's en SAML Single Sign-On Service** uit de **Naslaggids sectie.**

    ![Ziflow configuratie](./media/ziflow-tutorial/tutorial_ziflow_configure.png) 

7. In een ander browservenster, meld u aan bij Ziflow als een beveiligingsbeheerder.

8. Klik op het Avatar in de rechterbovenhoek en klik vervolgens op **-account beheren**.

    ![Ziflow configuratie beheren](./media/ziflow-tutorial/tutorial_ziflow_manage.png)

9. In de linkerbovenhoek, klikt u op **Single Sign-On**.

    ![Meld u Ziflow-configuratie](./media/ziflow-tutorial/tutorial_ziflow_signon.png)

10. Op de **Single Sign-On** pagina, voert u de volgende stappen uit:

    ![Ziflow configuratie één](./media/ziflow-tutorial/tutorial_ziflow_page.png)

    a. Selecteer **Type** als **SAML2.0**.

    b. In de **Sign In URL** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML**, die u hebt gekopieerd vanuit Azure portal.

    c. Upload het base-64 gecodeerde certificaat dat u hebt gedownload vanuit Azure portal, in de **X509 certificaat voor ondertekening**.

    d. In de **afmeldings-URL** tekstvak, plak de waarde van **afmelding URL**, die u hebt gekopieerd vanuit Azure portal.

    e. Uit de **configuratie-instellingen voor uw id-Provider** sectie, kopieert u de gemarkeerde unieke id-waarde en voegt u deze met de id en de aanmeldings-URL in de **Ziflow domein en URL's sectie** op Azure-portal.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/ziflow-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/ziflow-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/ziflow-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/ziflow-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
  
### <a name="create-a-ziflow-test-user"></a>Maak een testgebruiker Ziflow

Als u wilt dat Azure AD-gebruikers zich aanmelden bij Ziflow, moeten ze worden ingericht voor Ziflow. In Ziflow is inrichten een handmatige taak.

Voer de volgende stappen uit als u een gebruikersaccount wilt inrichten:

1. Meld u aan bij Ziflow als een beveiligingsbeheerder.

2. Navigeer naar **mensen** in de rechterbovenhoek.

    ![Configuratie van Ziflow personen](./media/ziflow-tutorial/tutorial_ziflow_people.png)

3. Klik op **toevoegen** en klik vervolgens op **gebruiker toevoegen**.

    ![Configuratie van Ziflow toevoegen van gebruiker](./media/ziflow-tutorial/tutorial_ziflow_add.png)

4. Op de **toevoegen van een gebruiker** pop-upvenster de volgende stappen uitvoeren:

    ![Configuratie van Ziflow toevoegen van gebruiker](./media/ziflow-tutorial/tutorial_ziflow_adduser.png)

    a. In **e** tekst vak, voer het e-mailadres van gebruiker, zoals brittasimon@contoso.com.

    b. In **voornaam** tekst voert u de voornaam van de gebruiker, zoals Julia.

    c. In **achternaam** tekst voert u de achternaam van de gebruiker, zoals Simon.

    d. Selecteer uw rol Ziflow.

    e. Klik op **toevoegen 1 gebruiker**.

    > [!NOTE]
    > De houder van Azure Active Directory-account ontvangt een e-mailbericht en volgt een koppeling om te bevestigen van hun account voordat deze geactiveerd wordt.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Ziflow.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan Ziflow toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Ziflow**.

    ![De koppeling Ziflow in de lijst met toepassingen](./media/ziflow-tutorial/tutorial_ziflow_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Ziflow in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Ziflow.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/ziflow-tutorial/tutorial_general_01.png
[2]: ./media/ziflow-tutorial/tutorial_general_02.png
[3]: ./media/ziflow-tutorial/tutorial_general_03.png
[4]: ./media/ziflow-tutorial/tutorial_general_04.png

[100]: ./media/ziflow-tutorial/tutorial_general_100.png

[200]: ./media/ziflow-tutorial/tutorial_general_200.png
[201]: ./media/ziflow-tutorial/tutorial_general_201.png
[202]: ./media/ziflow-tutorial/tutorial_general_202.png
[203]: ./media/ziflow-tutorial/tutorial_general_203.png

