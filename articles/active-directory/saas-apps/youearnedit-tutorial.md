---
title: 'Zelfstudie: Azure Active Directory-integratie met YouEarnedIt | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en YouEarnedIt.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 3011d44d-dfcf-4061-888f-cff90fbc8150
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2018
ms.author: jeedes
ms.openlocfilehash: 3a394c13092547991bf7f8ae98e5c69e92077701
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43344781"
---
# <a name="tutorial-azure-active-directory-integration-with-youearnedit"></a>Zelfstudie: Azure Active Directory-integratie met YouEarnedIt

In deze zelfstudie leert u hoe u YouEarnedIt integreren met Azure Active Directory (Azure AD).

YouEarnedIt integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot YouEarnedIt heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij YouEarnedIt (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met YouEarnedIt, moet u de volgende items:

- Een Azure AD-abonnement
- Een YouEarnedIt eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. YouEarnedIt uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-youearnedit-from-the-gallery"></a>YouEarnedIt uit de galerie toe te voegen

Voor het configureren van de integratie van YouEarnedIt in Azure AD, moet u YouEarnedIt uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen YouEarnedIt uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **YouEarnedt**, selecteer **YouEarnedt** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![YouEarnedIt in de lijst met resultaten](./media/youearnedit-tutorial/tutorial_youearnedit_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met YouEarnedIt op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in YouEarnedIt is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in YouEarnedIt tot stand worden gebracht.

In YouEarnedIt, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met YouEarnedIt, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker YouEarnedIt](#create-a-youearnedit-test-user)**  : als u wilt een equivalent van Britta Simon in YouEarnedIt die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing YouEarnedIt.

**Voor het configureren van Azure AD eenmalige aanmelding met YouEarnedIt, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **YouEarnedIt** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/youearnedit-tutorial/tutorial_youearnedit_samlbase.png)

3. Op de **YouEarnedIt domein en URL's** sectie, voert u de volgende stappen uit:

    ![YouEarnedIt domein en URL's, eenmalige aanmelding informatie](./media/youearnedit-tutorial/tutorial_youearnedit_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van de volgende patronen: 
    | Omgeving  | Patroon  |
    |:--- |:--- |
    | Productie | `https://<company name>.youearnedit.com/users/sign_in` |
    | Sandbox  |`https://<company name>.sandbox.youearnedit.com/users/sign_in` |

    b. In de **id** tekstvak, een URL met behulp van de volgende patronen:
    | Omgeving  | Patroon  |
    |:--- |:--- |
    | Productie | `https://<company name>.youearnedit.com` |
    | Sandbox  |`https://<company name>.sandbox.youearnedit.com` |

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL en -id. Neem contact op met uw toegewezen YouEarnedIt Klantsucces manager als u deze waarden.

4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/youearnedit-tutorial/tutorial_youearnedit_certificate.png) 

5. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/youearnedit-tutorial/tutorial_general_400.png)

6. Op de **YouEarnedIt configuratie** sectie, klikt u op **configureren YouEarnedIt** openen **aanmelding configureren** venster. Kopiëren de **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![YouEarnedIt configuratie](./media/youearnedit-tutorial/tutorial_youearnedit_configure.png) 

7. Het configureren van eenmalige aanmelding op de **YouEarnedIt** zijde, moet u voor het verzenden van de gedownloade ***Certificate(Base64)*** en ***Single Sign-On Service URL voor SAML*** aan uw toegewezen **YouEarnedIt** Klantsucces manager. Ze stelt u deze optie om de SAML SSO-verbinding instellen goed aan beide zijden.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/youearnedit-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/youearnedit-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/youearnedit-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/youearnedit-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.

### <a name="create-a-youearnedit-test-user"></a>Maak een testgebruiker YouEarnedIt

In deze sectie maakt u een gebruiker met de naam van Britta Simon in YouEarnedIt. Neem contact op met uw toegewezen YouEarnedIt Klantsucces manager om toe te voegen de gebruikers in het YouEarnedIt-platform.

>[!NOTE]
>YouEarnedIt verwachten dat de id-Provider op te geven van een EmailAddress of de gebruikersnaam in het kenmerk van NameID. Verificatie mislukt als er een overeenkomende gebruikersnaam of EmailAddress niet in de database gevonden is of komt niet exact overeen. Dit vereist dat de accounts worden geïmporteerd in het systeem YouEarnedIt voordat de integratie van eenmalige aanmelding (meestal ofwel via een API- of CSV-import).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan YouEarnedIt.

![De de gebruikersrol toewijzen][200]

**Als u wilt Britta Simon aan YouEarnedIt toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **YouEarnedIt**.

    ![De koppeling YouEarnedIt in de lijst met toepassingen](./media/youearnedit-tutorial/tutorial_youearnedit_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel YouEarnedIt in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing YouEarnedIt.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/youearnedit-tutorial/tutorial_general_01.png
[2]: ./media/youearnedit-tutorial/tutorial_general_02.png
[3]: ./media/youearnedit-tutorial/tutorial_general_03.png
[4]: ./media/youearnedit-tutorial/tutorial_general_04.png

[100]: ./media/youearnedit-tutorial/tutorial_general_100.png

[200]: ./media/youearnedit-tutorial/tutorial_general_200.png
[201]: ./media/youearnedit-tutorial/tutorial_general_201.png
[202]: ./media/youearnedit-tutorial/tutorial_general_202.png
[203]: ./media/youearnedit-tutorial/tutorial_general_203.png