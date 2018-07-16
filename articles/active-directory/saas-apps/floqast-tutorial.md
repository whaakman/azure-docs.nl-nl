---
title: 'Zelfstudie: Azure Active Directory-integratie met FloQast | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en FloQast.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 013cb57d-567c-44d0-a119-e6ba6e607153
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2018
ms.author: jeedes
ms.openlocfilehash: b37c62bbdf0eb6c232c47ef257bb7c0da75e1948
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39042500"
---
# <a name="tutorial-azure-active-directory-integration-with-floqast"></a>Zelfstudie: Azure Active Directory-integratie met FloQast

In deze zelfstudie leert u hoe u FloQast integreren met Azure Active Directory (Azure AD).

FloQast integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot FloQast heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij FloQast (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met FloQast, moet u de volgende items:

- Een Azure AD-abonnement
- Een FloQast eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. FloQast uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-floqast-from-the-gallery"></a>FloQast uit de galerie toe te voegen
Voor het configureren van de integratie van FloQast in Azure AD, moet u FloQast uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen FloQast uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **FloQast**, selecteer **FloQast** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![FloQast in de lijst met resultaten](./media/floqast-tutorial/tutorial_floqast_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met FloQast op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in FloQast is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in FloQast tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met FloQast, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker FloQast](#create-a-floqast-test-user)**  : als u wilt een equivalent van Britta Simon in FloQast die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing FloQast.

**Voor het configureren van Azure AD eenmalige aanmelding met FloQast, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **FloQast** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/floqast-tutorial/tutorial_floqast_samlbase.png)

3. Op de **FloQast domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![FloQast domein en URL's, eenmalige aanmelding informatie](./media/floqast-tutorial/tutorial_floqast_url.png)

     In de **id** tekstvak, een URL typen: `https://go.floqast.com/`

4. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![FloQast domein en URL's, eenmalige aanmelding informatie](./media/floqast-tutorial/tutorial_floqast_url1.png)

     In de **aanmeldings-URL** tekstvak, een URL typen: `https://go.floqast.com/login/sso`
     
5. FloQast toepassing verwacht het SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de **gebruikerskenmerken** sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze.
    
    ![Single Sign-On kenmerk configureren](./media/floqast-tutorial/tutorial_floqast_attribute.png)
    
6. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding en voer de volgende stappen uit:
    
    | Naam kenmerk | Waarde kenmerk |
    | ------------------- | -------------------- |    
    | FirstName           | User.givenName |
    | LastName        | User.surname |
    | Email       | User.mail    |

    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Configureren van eenmalige aanmelding toevoegen](./media/floqast-tutorial/tutorial_attribute_04.png)

    ![Single Sign-On Addattb configureren](./media/floqast-tutorial/tutorial_attribute_05.png)

    b. In de **naam** tekstvak typt u de naam van het kenmerk wordt weergegeven voor die rij.

    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Laat de **Namespace** leeg.
    
    e. Klik op **OK**.

7. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer en voer de volgende stappen uit:

    ![De downloadkoppeling certificaat](./media/floqast-tutorial/tutorial_floqast_certificate.png)

    a. Controleer **geavanceerde instellingen voor het ondertekenen van certificaat weergeven**.

    ![De certificaat-verklaring](./media/floqast-tutorial/tutorial_floqast_certificateassertion.png)

    b. Selecteer **ondertekening optie** als **aanmelding SAML-antwoord en -bewering**.

8. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/floqast-tutorial/tutorial_general_400.png)
    
9. Het configureren van eenmalige aanmelding op **FloQast** zijde, moet u voor het verzenden van de gedownloade **Metadata XML** naar [FloQast ondersteuningsteam](mailto:support@floqast.com). Ze stelt u deze optie om de SAML SSO-verbinding instellen goed aan beide zijden.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/floqast-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/floqast-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/floqast-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/floqast-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-floqast-test-user"></a>Maak een testgebruiker FloQast

In deze sectie maakt u een gebruiker met de naam van Britta Simon in FloQast. Werken met [FloQast ondersteuningsteam](mailto:support@floqast.com) om toe te voegen de gebruikers in het FloQast-platform. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken. 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan FloQast.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan FloQast toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **FloQast**.

    ![De koppeling FloQast in de lijst met toepassingen](./media/floqast-tutorial/tutorial_floqast_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel FloQast in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing FloQast.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/floqast-tutorial/tutorial_general_01.png
[2]: ./media/floqast-tutorial/tutorial_general_02.png
[3]: ./media/floqast-tutorial/tutorial_general_03.png
[4]: ./media/floqast-tutorial/tutorial_general_04.png

[100]: ./media/floqast-tutorial/tutorial_general_100.png

[200]: ./media/floqast-tutorial/tutorial_general_200.png
[201]: ./media/floqast-tutorial/tutorial_general_201.png
[202]: ./media/floqast-tutorial/tutorial_general_202.png
[203]: ./media/floqast-tutorial/tutorial_general_203.png

