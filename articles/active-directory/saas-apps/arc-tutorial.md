---
title: 'Zelfstudie: Azure Active Directory-integratie met Publishing boog - SSO | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Publishing boog - eenmalige aanmelding.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ae609583-f875-4cb8-b68e-1b0b7938e9a7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2018
ms.author: jeedes
ms.openlocfilehash: 9fe3740bf6ad9fe0fd7847f4e1ab0764ea5e4a72
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2018
ms.locfileid: "35929665"
---
# <a name="tutorial-azure-active-directory-integration-with-arc-publishing---sso"></a>Zelfstudie: Azure Active Directory-integratie met Publishing boog - SSO

In deze zelfstudie leert u hoe integreren Publishing boog - SSO met Azure Active Directory (Azure AD).

Boog publicatie - SSO met Azure AD integreren biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Publishing boog - eenmalige aanmelding heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij publicatie boog - SSO (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Publishing boog - SSO, moet u de volgende items:

- Een Azure AD-abonnement
- De publicatie van een boog - eenmalige aanmelding SSO-abonnement is ingeschakeld

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toevoegen van boog publicatie - SSO uit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-arc-publishing---sso-from-the-gallery"></a>Toevoegen van boog publicatie - SSO uit de galerie
Om de integratie van Publishing boog - SSO met Azure AD te configureren die u wilt toevoegen Publishing boog - SSO uit de galerie aan de lijst met beheerde SaaS-apps.

**Voer de volgende stappen uit als u wilt publiceren boog - SSO uit de galerie toevoegen:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Publishing boog - SSO**, selecteer **Publishing boog - SSO** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Boog publicatie - eenmalige aanmelding in de lijst met resultaten](./media/arc-tutorial/tutorial_arc_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Publishing boog - eenmalige aanmelding op basis van een testgebruiker 'Britta Simon' genoemd.

Azure AD moet weten wat de equivalente gebruiker in Publishing boog - eenmalige aanmelding is voor een gebruiker in Azure AD voor eenmalige aanmelding werkt. Met andere woorden, een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Publishing boog - SSO moet tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Publishing boog - SSO, moet u de volgende elementen voltooid:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een Publishing boog - SSO testgebruiker](#create-an-arc-publishing---sso-test-user)**  - bevatten een equivalent van Britta Simon Publishing boog - eenmalige aanmelding die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw boog publicatie - SSO-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met Publishing boog - SSO, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Publishing boog - SSO** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/arc-tutorial/tutorial_arc_samlbase.png)

3. Op de **Publishing boog - domein met eenmalige aanmelding en URL's** sectie, voert u de volgende stappen uit als u wilt configureren, de toepassing in **IDP** modus gestart:

    ![Boog publicatie - SSO-domein en één URL's aanmelding informatie](./media/arc-tutorial/tutorial_arc_url.png)

    a. In de **id** textbox, typ een URL met het volgende patroon volgen: `https://www.okta.com/saml2/service-provider/<Unique ID>`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen: `https://arcpublishing-<Customer>.okta.com/sso/saml2/<Unique ID>`

4. Controleer **weergeven geavanceerde instellingen voor URL** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Boog publicatie - SSO-domein en één URL's aanmelding informatie](./media/arc-tutorial/tutorial_arc_url1.png)

    In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen: `https://arcpublishing-<Customer>.okta.com/sso/saml2/<Unique ID>`
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke id, antwoord-URL en aanmeldings-URL. Neem contact op met [Publishing boog - ondersteuningsteam SSO-Client](mailto:inf@washpost.com) ophalen van deze waarden. 

5. ARC publiceren - SSO-toepassing de SAML-asserties verwacht in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken van beheren de **gebruikerskenmerken** sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze.
    
    ![Eenmalige aanmelding configureren](./media/arc-tutorial/tutorial_arc_attribute.png)

6. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding hierboven en voer de volgende stappen uit:
    
    | Naam kenmerk | Waarde kenmerk |
    | ---------------| --------------- |    
    | Voornaam | User.givenName |
    | lastName | User.surname |
    | e-mailen | User.mail |
    | groepen | User.assignedroles |

    a. Klik op **toevoegen kenmerk** openen de **kenmerk toevoegen** dialoogvenster.

     ![Eenmalige aanmelding configureren](./media/arc-tutorial/tutorial_attribute_04.png)

     ![Eenmalige aanmelding configureren](./media/arc-tutorial/tutorial_attribute_05.png)
    
    b. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.
    
    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Laat de **Namespace** leeg.
    
    d. Klik op **Ok**

    > [!NOTE]
    > Hier de **groepen** kenmerk wordt toegewezen aan **user.assignedroles**. Dit zijn aangepaste rollen in Azure AD om toe te wijzen de namen in de toepassing hebt gemaakt. U vindt meer richtlijnen [hier](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-enterprise-app-role-management) voor het maken van aangepaste rollen in Azure AD. 

7. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/arc-tutorial/tutorial_arc_certificate.png) 

8. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/arc-tutorial/tutorial_general_400.png)
    
9. Op de **Publishing boog - configuratie voor eenmalige aanmelding** sectie, klikt u op **configureren boog publicatie - SSO** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Boog publicatie - configuratie voor eenmalige aanmelding](./media/arc-tutorial/tutorial_arc_configure.png) 

10. Eenmalige aanmelding configureren op **Publishing boog - SSO** zijde, moet u de gedownloade verzenden **certificaat (Base64), Sign-Out URL SAML entiteit-ID en SAML Single Sign-On Service-URL** naar [boog Publicatie - ondersteuningsteam SSO](mailto:inf@washpost.com). Ze deze instelling zodat de SAML SSO-verbinding juist is ingesteld op beide zijden ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/arc-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/arc-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/arc-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/arc-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-arc-publishing---sso-test-user"></a>Een boog publicatie - SSO testgebruiker maken

Het doel van deze sectie is het maken van een gebruiker Britta Simon aangeroepen in Publishing boog - eenmalige aanmelding. ARC publiceren - eenmalige aanmelding ondersteunt just-in-time-inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot Publishing boog - eenmalige aanmelding als deze nog niet bestaat.

>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [Publishing boog - ondersteuningsteam SSO](mailto:inf@washpost.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Publishing boog - eenmalige aanmelding.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen Publishing boog - SSO, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Publishing boog - SSO**.

    ![Het publiceren van de boog - SSO-koppeling in de lijst met toepassingen](./media/arc-tutorial/tutorial_arc_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Wanneer u klikt op de boog publicatie - tegel in het deelvenster toegang SSO-u moet ophalen automatisch aangemeld bij uw boog publicatie - SSO-toepassing.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/arc-tutorial/tutorial_general_01.png
[2]: ./media/arc-tutorial/tutorial_general_02.png
[3]: ./media/arc-tutorial/tutorial_general_03.png
[4]: ./media/arc-tutorial/tutorial_general_04.png

[100]: ./media/arc-tutorial/tutorial_general_100.png

[200]: ./media/arc-tutorial/tutorial_general_200.png
[201]: ./media/arc-tutorial/tutorial_general_201.png
[202]: ./media/arc-tutorial/tutorial_general_202.png
[203]: ./media/arc-tutorial/tutorial_general_203.png

