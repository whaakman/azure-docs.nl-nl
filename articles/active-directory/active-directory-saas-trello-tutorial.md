---
title: 'Zelfstudie: Azure Active Directory-integratie met Trello | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Trello.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: cd5ae365-9ed6-43a6-920b-f7814b993949
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2017
ms.author: jeedes
ms.openlocfilehash: 39ea5cc19e73cb6e9ea6da9757f5e8013d1a5ffe
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-trello"></a>Zelfstudie: Azure Active Directory-integratie met Trello

In deze zelfstudie leert u hoe Trello integreren met Azure Active Directory (Azure AD).

Trello integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Trello heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Trello (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Trello, moet u de volgende items:

- Een Azure AD-abonnement
- Een Trello eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Trello uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-trello-from-the-gallery"></a>Trello uit de galerie toevoegen
Voor het configureren van de integratie van Trello in Azure AD, moet u Trello uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Trello uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Trello**, selecteer **Trello** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Trello in de lijst met resultaten](./media/active-directory-saas-trello-tutorial/tutorial_trello_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met Trello op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Trello is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Trello tot stand worden gebracht.

Wijs in Trello, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Trello, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Trello](#create-a-trello-test-user)**  - Trello die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In deze sectie maakt u Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing Trello.

>[!NOTE]
>Deze krijgt u de  **\<enterprise\>**  slug van Trello. Als u de slug-waarde niet hebt, neem dan contact op met [Trello ondersteuningsteam](mailto:support@trello.com) ophalen van de slug voor u enterprise.
    > 

**Voor het configureren van Azure AD eenmalige aanmelding met Trello, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Trello** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-trello-tutorial/tutorial_trello_samlbase.png)

3. Op de **Trello domein en de URL's** sectie als u wilt configureren van de toepassing in **IDP geïnitieerd modus**, voer de volgende stappen uit:

    ![URL's en Trello domein eenmalige aanmelding informatie](./media/active-directory-saas-trello-tutorial/tutorial_trello_url.png)
    
    a. In de **id** textbox, typ de volgende URL:`https://trello.com/auth/saml/metadata`
    
    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://trello.com/auth/saml/consume/<enterprise>`

4. Als u wilt configureren van de toepassing in **SP geïnitieerd modus**, voer de volgende stappen uit:

    ![URL's en Trello domein eenmalige aanmelding informatie](./media/active-directory-saas-trello-tutorial/tutorial_trello_url1.png)

    a. Controleer **weergeven geavanceerde instellingen voor URL**.

    b. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://trello.com/auth/saml/login/<enterprise>` 

5. Trello toepassing verwacht de SAML-asserties naar specifieke kenmerken bevatten. Configureer de volgende kenmerken voor deze toepassing. U kunt de waarden van deze kenmerken van beheren de **'Gebruikerskenmerken'** van de toepassing. De volgende Schermafbeelding toont een voorbeeld voor deze.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-trello-tutorial/tutorial_trello_attribute.png)

6. Op de **SAML-token kenmerken** dialoogvenster uitvoeren voor elke rij in de onderstaande tabel wordt weergegeven op de volgende stappen uit:
 
    | Kenmerknaam | Waarde kenmerk |
    | --- | --- |
    | User.Email | User.mail |
    | User.FirstName | User.givenName |
    | User.LastName | User.surname |

    a. Klik op **toevoegen kenmerk** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-trello-tutorial/tutorial_officespace_04.png)

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-trello-tutorial/tutorial_attribute_05.png)

    b. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij. 

    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    d. Klik op **OK**. 

7. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-trello-tutorial/tutorial_trello_certificate.png) 

8. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-trello-tutorial/tutorial_general_400.png)
    
9. Op de **Trello configuratie** sectie, klikt u op **configureren Trello** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Trello configuratie](./media/active-directory-saas-trello-tutorial/tutorial_trello_configure.png) 

10. Eenmalige aanmelding configureren op **Trello** zijde, moet u gaat u naar de [Trello SSO Ondernemingsconfiguratie](https://trello.com/sso-configuration) pagina voor het verzenden van de gedownloade **certificaat (Base64)** en  **SAML Single Sign-On Service-URL** naar [Trello ondersteuningsteam](mailto:support@trello.com). Ze deze instelling zodat de SAML SSO-verbinding juist is ingesteld op beide zijden ingesteld.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-trello-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-trello-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-trello-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-trello-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-trello-test-user"></a>Een testgebruiker Trello maken

Het doel van deze sectie is het maken van een gebruiker Britta Simon aangeroepen in Trello. Trello ondersteunt just-in-time-inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot Trello als deze nog niet bestaat.

>[!Note]
>Als u wilt een gebruiker handmatig maken, neem contact op met [Trello ondersteuningsteam](mailto:support@trello.com).


### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Trello.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen Trello, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Trello**.

    ![De koppeling Trello in de lijst met toepassingen](./media/active-directory-saas-trello-tutorial/tutorial_trello_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Trello in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Trello.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-trello-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-trello-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-trello-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-trello-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-trello-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-trello-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-trello-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-trello-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-trello-tutorial/tutorial_general_203.png

