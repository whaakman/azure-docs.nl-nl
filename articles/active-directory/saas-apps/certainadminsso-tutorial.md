---
title: 'Zelfstudie: Azure Active Directory-integratie met bepaalde beheerder SSO | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en bepaalde beheerder eenmalige aanmelding.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 98ba0174-be02-408a-8634-c8113b12dedb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2018
ms.author: jeedes
ms.openlocfilehash: e465518f5526dcf581afab77e30d0af33aaae88e
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39054036"
---
# <a name="tutorial-azure-active-directory-integration-with-certain-admin-sso"></a>Zelfstudie: Azure Active Directory-integratie met bepaalde beheerder eenmalige aanmelding

In deze zelfstudie leert u hoe u bepaalde beheerder SSO integreren met Azure Active Directory (Azure AD).

Bepaalde beheerder SSO integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot bepaalde beheerder eenmalige aanmelding heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij bepaalde beheerder SSO (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met bepaalde beheerder eenmalige aanmelding, moet u de volgende items:

- Een Azure AD-abonnement
- Een bepaalde beheerder SSO eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Bepaalde beheerder SSO uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-certain-admin-sso-from-the-gallery"></a>Bepaalde beheerder SSO uit de galerie toe te voegen
Voor het configureren van de integratie van bepaalde beheerder SSO in Azure AD, moet u bepaalde beheerder SSO uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen bepaalde beheerder SSO uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **bepaalde beheerder SSO**, selecteer **bepaalde beheerder SSO** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Bepaalde SSO Admin in de lijst met resultaten](./media/certainadminsso-tutorial/tutorial_certainadminsso_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met bepaalde beheerder eenmalige aanmelding op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in bepaalde beheerder SSO is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in een bepaalde beheerder eenmalige aanmelding tot stand worden gebracht.

Als u wilt configureren en Azure AD eenmalige aanmelding met bepaalde beheerder eenmalige aanmelding testen, moet u uitvoeren van de volgende bouwstenen:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker bepaalde beheerder SSO](#create-a-certain-admin-sso-test-user)**  : als u wilt een equivalent van Britta Simon hebt in bepaalde Admin-eenmalige aanmelding die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing bepaalde beheerder eenmalige aanmelding.

**Voor het configureren van Azure AD eenmalige aanmelding met bepaalde beheerder eenmalige aanmelding, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **bepaalde beheerder SSO** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/certainadminsso-tutorial/tutorial_certainadminsso_samlbase.png)

3. Op de **bepaalde beheerder SSO-domein en URL's** sectie, voert u de volgende stappen uit:

    ![Bepaalde beheerder SSO-domein en URL's eenmalige aanmelding informatie](./media/certainadminsso-tutorial/tutorial_certainadminsso_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<YOUR DOMAIN URL>/svcs/sso_admin_login/handleRequest/<ID>`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<SUBDOMAIN>.certain.com`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL en -id. Neem contact op met [bepaalde beheerder SSO-Client-ondersteuningsteam](mailto:integrations@certain.com) om deze waarden te verkrijgen. 
 
4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Raw)** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/certainadminsso-tutorial/tutorial_certainadminsso_certificate.png) 

5. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/certainadminsso-tutorial/tutorial_general_400.png)

6. Op de **bepaalde SSO beheerconfiguratie** sectie, klikt u op **bepaalde beheerder SSO configureren** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Bepaalde beheerder SSO-configuratie](./media/certainadminsso-tutorial/tutorial_certainadminsso_configure.png) 

7. Het configureren van eenmalige aanmelding op **bepaalde beheerder SSO** zijde, moet u voor het verzenden van de gedownloade **certificaat (Raw)**, **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** naar [bepaalde beheerder SSO-ondersteuningsteam](mailto:integrations@certain.com). Ze stelt u deze optie om de SAML SSO-verbinding instellen goed aan beide zijden.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/certainadminsso-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/certainadminsso-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/certainadminsso-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/certainadminsso-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-certain-admin-sso-test-user"></a>Maak een testgebruiker bepaalde beheerder SSO

In deze sectie maakt u een gebruiker met de naam van Britta Simon in bepaalde beheerder eenmalige aanmelding. Werken met [bepaalde beheerder SSO-ondersteuningsteam](mailto:integrations@certain.com) om toe te voegen de gebruikers in de bepaalde beheerder SSO-platform. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan een bepaalde beheerder eenmalige aanmelding.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon aan bepaalde beheerder SSO, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **bepaalde beheerder SSO**.

    ![De koppeling bepaalde beheerder eenmalige aanmelding in de lijst met toepassingen](./media/certainadminsso-tutorial/tutorial_certainadminsso_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel bepaalde beheerder eenmalige aanmelding in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing bepaalde beheerder eenmalige aanmelding.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/certainadminsso-tutorial/tutorial_general_01.png
[2]: ./media/certainadminsso-tutorial/tutorial_general_02.png
[3]: ./media/certainadminsso-tutorial/tutorial_general_03.png
[4]: ./media/certainadminsso-tutorial/tutorial_general_04.png

[100]: ./media/certainadminsso-tutorial/tutorial_general_100.png

[200]: ./media/certainadminsso-tutorial/tutorial_general_200.png
[201]: ./media/certainadminsso-tutorial/tutorial_general_201.png
[202]: ./media/certainadminsso-tutorial/tutorial_general_202.png
[203]: ./media/certainadminsso-tutorial/tutorial_general_203.png

