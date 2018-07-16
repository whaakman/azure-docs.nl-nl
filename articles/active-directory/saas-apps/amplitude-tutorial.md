---
title: 'Zelfstudie: Azure Active Directory-integratie met Amplitude | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Amplitude.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 496c9ffa-c833-41fa-8d17-2dc3044954d1
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2018
ms.author: jeedes
ms.openlocfilehash: a2815b60799f98071915a0f06908fd92ff3fb2f2
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39043928"
---
# <a name="tutorial-azure-active-directory-integration-with-amplitude"></a>Zelfstudie: Azure Active Directory-integratie met Amplitude

In deze zelfstudie leert u hoe u Amplitude integreren met Azure Active Directory (Azure AD).

Amplitude integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Amplitude heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Amplitude (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Amplitude, moet u de volgende items:

- Een Azure AD-abonnement
- Een Amplitude eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Amplitude uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-amplitude-from-the-gallery"></a>Amplitude uit de galerie toe te voegen
Voor het configureren van de integratie van Amplitude in Azure AD, moet u Amplitude uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Amplitude uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **Amplitude**, selecteer **Amplitude** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Waarde tijdens de lijst met resultaten](./media/amplitude-tutorial/tutorial_amplitude_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Amplitude op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Amplitude is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Amplitude tot stand worden gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Amplitude, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Amplitude](#create-an-amplitude-test-user)**  : als u wilt een equivalent van Britta Simon in Amplitude die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Amplitude.

**Voor het configureren van Azure AD eenmalige aanmelding met Amplitude, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Amplitude** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/amplitude-tutorial/tutorial_amplitude_samlbase.png)

3. Op de **Amplitude domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Amplitude domein en URL's, eenmalige aanmelding informatie](./media/amplitude-tutorial/tutorial_amplitude_url.png)

    a. In de **id** tekstvak typt u de URL: `https://amplitude.com/saml/sso/metadata`

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://analytics.amplitude.com/saml/sso/<uniqueid>`

    > [!NOTE]
    > De antwoord-URL-waarde is niet echt. Verderop in deze zelfstudie krijgt u de antwoord-URL-waarde.

4. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Amplitude domein en URL's, eenmalige aanmelding informatie](./media/amplitude-tutorial/tutorial_amplitude_url1.png)

    In de **aanmeldings-URL** tekstvak typt u de URL: `https://analytics.amplitude.com/sso`

5. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/amplitude-tutorial/tutorial_amplitude_certificate.png) 

6. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/amplitude-tutorial/tutorial_general_400.png)
    
7. Aanmelding bij uw bedrijf Amplitude site als administrator.

8. Klik op de **plannen Admin** uit de linker navigatiebalk.

    ![Eenmalige aanmelding configureren](./media/amplitude-tutorial/configure1.png)

9. Selecteer **Microsoft Azure Active Directory-metagegevens** uit de **SSO-integratie**.

    ![Eenmalige aanmelding configureren](./media/amplitude-tutorial/configure2.png)

10. Op de **instellen van Single Sign-On** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/amplitude-tutorial/configure3.png)

    a. Open het gedownloade **Metadata Xml** vanuit Azure portal in Kladblok, plak de inhoud in de **Microsoft Azure Active Directory Metadata** tekstvak.

    b. Kopieer de **antwoord-URL (ACS)** waarde en plak deze in de **antwoord-URL** tekstvak van sectie Amplitude domein en URL's in Azure portal.

    c. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/amplitude-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/amplitude-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/amplitude-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/amplitude-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-amplitude-test-user"></a>Maak een testgebruiker Amplitude

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Amplitude. Amplitude biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot Amplitude als deze nog niet bestaat.
>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [Amplitude ondersteuningsteam](https://amplitude.zendesk.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Amplitude.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan Amplitude toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Amplitude**.

    ![De koppeling Amplitude in de lijst met toepassingen](./media/amplitude-tutorial/tutorial_amplitude_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Amplitude in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Amplitude.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/amplitude-tutorial/tutorial_general_01.png
[2]: ./media/amplitude-tutorial/tutorial_general_02.png
[3]: ./media/amplitude-tutorial/tutorial_general_03.png
[4]: ./media/amplitude-tutorial/tutorial_general_04.png

[100]: ./media/amplitude-tutorial/tutorial_general_100.png

[200]: ./media/amplitude-tutorial/tutorial_general_200.png
[201]: ./media/amplitude-tutorial/tutorial_general_201.png
[202]: ./media/amplitude-tutorial/tutorial_general_202.png
[203]: ./media/amplitude-tutorial/tutorial_general_203.png

