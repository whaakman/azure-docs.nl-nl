---
title: 'Zelfstudie: Azure Active Directory-integratie met Supermood | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Supermood.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: afc04efa-2eba-4e47-8ce4-b71eb293cd09
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2018
ms.author: jeedes
ms.openlocfilehash: ebd8e28e8adc357f0e6a0582f422085ddf50749f
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39041021"
---
# <a name="tutorial-azure-active-directory-integration-with-supermood"></a>Zelfstudie: Azure Active Directory-integratie met Supermood

In deze zelfstudie leert u hoe u Supermood integreren met Azure Active Directory (Azure AD).

Supermood integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Supermood heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Supermood (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Supermood, moet u de volgende items:

- Een Azure AD-abonnement
- Een Supermood eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Supermood uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-supermood-from-the-gallery"></a>Supermood uit de galerie toe te voegen
Voor het configureren van de integratie van Supermood in Azure AD, moet u Supermood uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Supermood uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **Supermood**, selecteer **Supermood** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Supermood in de lijst met resultaten](./media/supermood-tutorial/tutorial_supermood_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Supermood op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Supermood is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Supermood tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Supermood, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Supermood](#create-a-supermood-test-user)**  : als u wilt een equivalent van Britta Simon in Supermood die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Supermood.

**Voor het configureren van Azure AD eenmalige aanmelding met Supermood, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Supermood** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/supermood-tutorial/tutorial_supermood_samlbase.png)

3. Op de **Supermood domein en URL's** sectie, voert u de volgende stappen uit:

    ![Supermood domein en URL's, eenmalige aanmelding informatie](./media/supermood-tutorial/tutorial_supermood_url.png)

    a. Controleer **geavanceerde URL-instellingen weergeven**.

    b. Als u wilt configureren van de toepassing in **IDP** modus gestart de **Relaystatus** tekstvak, een URL typen: `https://supermood.co/auth/sso/saml20`

    c. Als u wilt configureren van de toepassing in **SP** modus gestart de **aanmeldings-URL** tekstvak, een URL typen: `https://supermood.co/app/#!/loginv2`

4. Supermood toepassing verwacht het SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de **gebruikerskenmerken** sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze.
    
    ![Eenmalige aanmelding configureren](./media/supermood-tutorial/tutorial_supermood_attribute.png)

5. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de bovenstaande afbeelding en voer de volgende stappen uit:
    
    | Naam kenmerk | Waarde kenmerk |
    | ---------------| --------------- |    
    | Voornaam | User.givenName |
    | lastName | User.surname |

    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/supermood-tutorial/tutorial_attribute_04.png)

    ![Eenmalige aanmelding configureren](./media/supermood-tutorial/tutorial_attribute_05.png)
    
    b. In de **naam** tekstvak typt u de naam van het kenmerk wordt weergegeven voor die rij.
    
    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Laat de **Namespace** leeg.
    
    d. Klik op **Ok**

6. Op de **SAML-handtekeningcertificaat** sectie, klikt u op de knop kopiëren om te kopiëren **App-Url voor federatieve metagegevens** en plak deze in Kladblok.

    ![De downloadkoppeling certificaat](./media/supermood-tutorial/tutorial_supermood_certificate.png) 

7. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/supermood-tutorial/tutorial_general_400.png)

8. Ga naar het deelvenster van de beheerder Supermood.co als beveiligingsbeheerder.

9. Klik op **Mijn account** (onderaan links) en **eenmalige aanmelding (SSO)**.

    ![Het certificaat dat één](./media/supermood-tutorial/tutorial_supermood_single.png)
10. Op **uw SAML 2.0-configuraties**, klikt u op **toevoegen van een SAML 2.0-configuratie voor een e-maildomein**.

    ![Het certificaat toevoegen](./media/supermood-tutorial/tutorial_supermood_add.png)

11. Op **toevoegen van een SAML 2.0-configuratie voor een e-maildomein**. sectie, voert u de volgende stappen uit:

    ![De certificaat-saml](./media/supermood-tutorial/tutorial_supermood_saml.png)

    a. In de **e-maildomein voor deze id-provider** tekstvak typt u uw domein.

    b. In de **een metagegevens-URL gebruiken** tekstvak, plak de **App-Url voor federatieve metagegevens** die u hebt gekopieerd vanuit Azure portal.

    c. Klik op **Add**.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/supermood-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/supermood-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/supermood-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/supermood-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-supermood-test-user"></a>Maak een testgebruiker Supermood

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Supermood. Supermood biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld voor gebruikers met e-mailberichten deel uitmaken van de domeinen die worden toegevoegd tijdens de configuratie aan Supermood einde. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot Supermood als deze nog niet bestaat.

>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [Supermood ondersteuningsteam](mailto:hello@supermood.fr).


### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Supermood.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan Supermood toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Supermood**.

    ![De koppeling Supermood in de lijst met toepassingen](./media/supermood-tutorial/tutorial_supermood_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Supermood in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Supermood.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/supermood-tutorial/tutorial_general_01.png
[2]: ./media/supermood-tutorial/tutorial_general_02.png
[3]: ./media/supermood-tutorial/tutorial_general_03.png
[4]: ./media/supermood-tutorial/tutorial_general_04.png

[100]: ./media/supermood-tutorial/tutorial_general_100.png

[200]: ./media/supermood-tutorial/tutorial_general_200.png
[201]: ./media/supermood-tutorial/tutorial_general_201.png
[202]: ./media/supermood-tutorial/tutorial_general_202.png
[203]: ./media/supermood-tutorial/tutorial_general_203.png

