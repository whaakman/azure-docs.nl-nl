---
title: 'Zelfstudie: Azure Active Directory-integratie met Hornbill | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Hornbill.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 173061e4-ac1d-458f-bb9b-e9a2493aab0e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 86f23a1520175827f775553e1ba949c62567cf83
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56201921"
---
# <a name="tutorial-azure-active-directory-integration-with-hornbill"></a>Zelfstudie: Azure Active Directory-integratie met Hornbill

In deze zelfstudie leert u hoe u Hornbill integreren met Azure Active Directory (Azure AD).

Hornbill integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Hornbill heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Hornbill (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Hornbill, moet u de volgende items:

- Een Azure AD-abonnement
- Een Hornbill eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Hornbill uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-hornbill-from-the-gallery"></a>Hornbill uit de galerie toe te voegen
Voor het configureren van de integratie van Hornbill in Azure AD, moet u Hornbill uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Hornbill uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

4. Typ in het zoekvak **Hornbill**, selecteer **Hornbill** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Hornbill in de lijst met resultaten](./media/hornbill-tutorial/tutorial_hornbill_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Hornbill op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Hornbill is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Hornbill tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Hornbill, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
3. **[Maak een testgebruiker Hornbill](#create-a-hornbill-test-user)**  : als u wilt een equivalent van Britta Simon in Hornbill die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Hornbill.

**Voor het configureren van Azure AD eenmalige aanmelding met Hornbill, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Hornbill** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/hornbill-tutorial/tutorial_hornbill_samlbase.png)

3. Op de **Hornbill domein en URL's** sectie, voert u de volgende stappen uit:

    ![Hornbill domein en URL's, eenmalige aanmelding informatie](./media/hornbill-tutorial/tutorial_hornbill_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<SUBDOMAIN>.hornbill.com/<INSTANCE_NAME>/`

    b. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<SUBDOMAIN>.hornbill.com/<INSTANCE_NAME>/lib/saml/auth/simplesaml/module.php/saml/sp/metadata.php/saml`

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de daadwerkelijke aanmeldings-URL en id. Neem contact op met [Hornbill Client ondersteuningsteam](https://www.hornbill.com/support/?request/) om deze waarden te verkrijgen. 

4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op de knop kopiëren om te kopiëren **App-Url voor federatieve metagegevens** en plak deze in Kladblok.

    ![De link om het certificaat te downloaden](./media/hornbill-tutorial/tutorial_hornbill_certificate.png) 

5. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/hornbill-tutorial/tutorial_general_400.png)
 
6. In een ander browservenster aanmelden bij Hornbill als een beveiligingsbeheerder.

7. Klik op de startpagina op **System**.

    ![Hornbill system](./media/hornbill-tutorial/tutorial_hornbill_system.png)

8. Navigeer naar **Security**.

    ![Hornbill beveiliging](./media/hornbill-tutorial/tutorial_hornbill_security.png)

9. Klik op **SSO profielen**.

    ![Hornbill één](./media/hornbill-tutorial/tutorial_hornbill_sso.png)

10. Aan de rechterkant van de pagina, klikt u op **logo toevoegen**.

    ![Hornbill toevoegen](./media/hornbill-tutorial/tutorial_hornbill_addlogo.png)

11. Op de **profielgegevens** balk, klikt u op **SAML-metagegevens importeren logo**.

    ![Hornbill-logo](./media/hornbill-tutorial/tutorial_hornbill_logo.png)

12. Op de pop-pagina in de **URL** in het tekstvak, plak de **App-Url voor federatieve metagegevens**, die u hebt gekopieerd vanuit Azure portal en klik op **proces**.

    ![Hornbill proces](./media/hornbill-tutorial/tutorial_hornbill_process.png)

13. Nadat het proces te klikken op de waarden ophalen automatisch ingevuld automatisch onder **profielgegevens** sectie.

    ![Hornbill pagina 1](./media/hornbill-tutorial/tutorial_hornbill_ssopage.png)

    ![Hornbill pagina 2](./media/hornbill-tutorial/tutorial_hornbill_ssopage1.png)

    ![Hornbill Pagina3](./media/hornbill-tutorial/tutorial_hornbill_ssopage2.png)

14. Klik op **Wijzigingen opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/hornbill-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/hornbill-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/hornbill-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/hornbill-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-hornbill-test-user"></a>Maak een testgebruiker Hornbill

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Hornbill. Hornbill biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot Hornbill als deze nog niet bestaat.

> [!Note]
> Als u maken van een gebruiker handmatig wilt, neem dan contact op met [Hornbill Client ondersteuningsteam](https://www.hornbill.com/support/?request/).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Hornbill.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan Hornbill toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Hornbill**.

    ![De koppeling Hornbill in de lijst met toepassingen](./media/hornbill-tutorial/tutorial_hornbill_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Hornbill in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Hornbill.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/hornbill-tutorial/tutorial_general_01.png
[2]: ./media/hornbill-tutorial/tutorial_general_02.png
[3]: ./media/hornbill-tutorial/tutorial_general_03.png
[4]: ./media/hornbill-tutorial/tutorial_general_04.png

[100]: ./media/hornbill-tutorial/tutorial_general_100.png

[200]: ./media/hornbill-tutorial/tutorial_general_200.png
[201]: ./media/hornbill-tutorial/tutorial_general_201.png
[202]: ./media/hornbill-tutorial/tutorial_general_202.png
[203]: ./media/hornbill-tutorial/tutorial_general_203.png

