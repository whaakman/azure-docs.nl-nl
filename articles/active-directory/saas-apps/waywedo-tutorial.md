---
title: 'Zelfstudie: Azure Active Directory-integratie met de manier waarop We doen | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en de manier waarop We doen.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 84fc4f36-ecd1-42c6-8a70-cb0f3dc15655
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5dc6d8e2cf7ac4786f30484325406a1fe696dff3
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56165124"
---
# <a name="tutorial-azure-active-directory-integration-with-way-we-do"></a>Zelfstudie: Azure Active Directory-integratie met de manier waarop We doen

In deze zelfstudie leert u over het integreren van de manier waarop We doen met Azure Active Directory (Azure AD).

We doen integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de manier waarop We doen heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld op een manier die We doen (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met de manier waarop We doen, moet u de volgende items:

- Een Azure AD-abonnement
- Een manier die We eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toe te voegen We doen vanuit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-way-we-do-from-the-gallery"></a>Toe te voegen We doen vanuit de galerie
Voor het configureren van de integratie van de manier waarop We doen in Azure AD, moet u de manier waarop We doen vanuit de galerie aan de lijst met beheerde SaaS-apps toevoegen.

**Om toe te voegen We doen vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

4. Typ in het zoekvak **manier doen We**, selecteer **manier doen We** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![We doen in de lijst met resultaten](./media/waywedo-tutorial/tutorial_waywedo_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met de manier waarop We doen op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in de manier waarop We doen is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in de manier waarop We doen tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met de manier waarop We doen, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
3. **[Maak een testgebruiker manier doen We](#create-a-way-we-do-test-user)**  : als u wilt een equivalent van Britta Simon hebben in de manier waarop We doen dat is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing manier waarop We doen.

**Voor het configureren van Azure AD eenmalige aanmelding met de manier waarop We doen, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **manier doen We** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/waywedo-tutorial/tutorial_waywedo_samlbase.png)

3. Op de **manier waarop We doen domein en URL's** sectie, voert u de volgende stappen uit:

    ![Hoe We doen domein en URL's eenmalige aanmelding informatie](./media/waywedo-tutorial/tutorial_waywedo_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<SUBDOMAIN>.waywedo.com/Authentication/ExternalSignIn`

    b. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<SUBDOMAIN>.waywedo.com`

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de daadwerkelijke aanmeldings-URL en id. Neem contact op met [manier waarop We doen Client ondersteuningsteam](mailto:support@waywedo.com) om deze waarden te verkrijgen. 
 
4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Raw)** en slaat u het certificaatbestand op uw computer.

    ![De link om het certificaat te downloaden](./media/waywedo-tutorial/tutorial_waywedo_certificate.png) 

5. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/waywedo-tutorial/tutorial_general_400.png)

6. Op de **manier waarop We doen configuratie** sectie, klikt u op **configureren We doen** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Manier waarop die wij configuratie doen](./media/waywedo-tutorial/tutorial_waywedo_configure.png) 

7. In een ander browservenster aanmelden bij de manier waarop We doen als een beveiligingsbeheerder.

8. Klik op de **persoon pictogram** in de rechterbovenhoek van een willekeurige pagina in de manier waarop We doen, en klik op **Account** in het vervolgkeuzemenu.

    ![We doen manier-account](./media/waywedo-tutorial/tutorial_waywedo_account.png) 

9. Klik op de **menupictogram** de push-navigatiemenu en klikt u op openen **Single Sign On**.

    ![We doen één](./media/waywedo-tutorial/tutorial_waywedo_single.png)

10. Op de **setup voor eenmalige aanmelding** pagina, voert u de volgende stappen uit:

    ![We doen opslaan](./media/waywedo-tutorial/tutorial_waywedo_save.png)

    a. Klik op de **eenmalige aanmelding inschakelen** overzet naar **Ja** eenmalige aanmelding inschakelen.

    b. In de **één aanmelding naam** tekstvak, Voer uw naam in.

    c. In de **entiteit-ID** tekstvak, plak de waarde van **SAML entiteit-ID**, die u hebt gekopieerd vanuit Azure portal.

    d. In de **URL voor SAML SSO-** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML**, die u hebt gekopieerd vanuit Azure portal.

    e. Upload het certificaat door te klikken op de **Selecteer** naast **certificaat**.

    f. **Optionele instellingen** -
    
    * Wachtwoorden - inschakelen wanneer deze optie is uitgeschakeld, het normale wachtwoord fungeert voor de manier waarop We doen zodat gebruikers alleen eenmalige aanmelding kunnen gebruiken.

    * Automatische inrichting - inschakelen wanneer deze optie is ingeschakeld, het e-mailadres dat is gebruikt voor aanmelding automatisch wordt vergeleken met de lijst met gebruikers in de manier waarop We doen. Als het e-mailadres komt niet overeen met een actieve gebruiker in de manier waarop We doen, worden automatisch een nieuw gebruikersaccount voor de persoon aanmelden, vraagt eventueel ontbrekende informatie toegevoegd.

      > [!NOTE]
      > Gebruikers die zijn toegevoegd via eenmalige aanmelding als algemene gebruikers zijn toegevoegd en een rol niet is toegewezen in het systeem. Een beheerder is in te gaan en hun beveiligingsrol als een editor of een beheerder wijzigen en kunt ook een of meer organigram rollen toewijzen. 

    g. Klik op **opslaan** om vast te leggen van de instellingen.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/waywedo-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/waywedo-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/waywedo-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/waywedo-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-way-we-do-test-user"></a>Maak een testgebruiker manier waarop We doen

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in de manier waarop We doen. We doen biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot manier waarop We doen als deze nog niet bestaat.

> [!Note]
> Als u maken van een gebruiker handmatig wilt, neem dan contact op met [manier waarop We doen Client ondersteuningsteam](mailto:support@waywedo.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan de manier waarop We doen.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon manier waarop We doen, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **manier doen We**.

    ![De manier waarop We doen koppeling in de lijst met toepassingen](./media/waywedo-tutorial/tutorial_waywedo_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel manier waarop We doen in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing manier waarop We doen.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/waywedo-tutorial/tutorial_general_01.png
[2]: ./media/waywedo-tutorial/tutorial_general_02.png
[3]: ./media/waywedo-tutorial/tutorial_general_03.png
[4]: ./media/waywedo-tutorial/tutorial_general_04.png

[100]: ./media/waywedo-tutorial/tutorial_general_100.png

[200]: ./media/waywedo-tutorial/tutorial_general_200.png
[201]: ./media/waywedo-tutorial/tutorial_general_201.png
[202]: ./media/waywedo-tutorial/tutorial_general_202.png
[203]: ./media/waywedo-tutorial/tutorial_general_203.png

