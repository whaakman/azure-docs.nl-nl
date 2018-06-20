---
title: 'Zelfstudie: Azure Active Directory-integratie met Vidyard | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Vidyard.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: bed7df23-6e13-4e7c-b4cc-53ed4804664d
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2018
ms.author: jeedes
ms.openlocfilehash: 0934ec2443d240234b2120056894921f2e30c81c
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36213162"
---
# <a name="tutorial-azure-active-directory-integration-with-vidyard"></a>Zelfstudie: Azure Active Directory-integratie met Vidyard

In deze zelfstudie leert u hoe Vidyard integreren met Azure Active Directory (Azure AD).

Vidyard integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Vidyard heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Vidyard (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Vidyard, moet u de volgende items:

- Een Azure AD-abonnement
- Een Vidyard eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Vidyard uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-vidyard-from-the-gallery"></a>Vidyard uit de galerie toevoegen
Voor het configureren van de integratie van Vidyard in Azure AD, moet u Vidyard uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Vidyard uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Vidyard**, selecteer **Vidyard** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Vidyard in de lijst met resultaten](./media/vidyard-tutorial/tutorial_vidyard_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met Vidyard op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Vidyard is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Vidyard tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Vidyard, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Vidyard](#create-a-vidyard-test-user)**  - Vidyard die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Vidyard configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Vidyard, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Vidyard** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/vidyard-tutorial/tutorial_vidyard_samlbase.png)

3. Op de **Vidyard domein en de URL's** sectie, voert u de volgende stappen uit als u wilt configureren, de toepassing in **IDP** modus gestart:

    ![URL's en Vidyard domein eenmalige aanmelding informatie](./media/vidyard-tutorial/tutorial_vidyard_url2.png)

    a. In de **id** textbox, typ een URL met het volgende patroon volgen: `https://secure.vidyard.com/sso/saml/<unique id>/metadata`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen: `https://secure.vidyard.com/sso/saml/<unique id>/consume`

4. Controleer **weergeven geavanceerde instellingen voor URL** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![URL's en Vidyard domein eenmalige aanmelding informatie](./media/vidyard-tutorial/tutorial_vidyard_url1.png)

    In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen: `https://secure.vidyard.com/sso/saml/<unique id>/login`

    > [!NOTE]
    > Deze waarden zijn niet echt. U kunt deze waarden wordt bijgewerkt met de werkelijke id, antwoord-URL en aanmeldings-URL, die verderop in de zelfstudie wordt beschreven

1. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/vidyard-tutorial/tutorial_vidyard_certificate.png) 

6. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/vidyard-tutorial/tutorial_general_400.png)

7. Op de **Vidyard configuratie** sectie, klikt u op **configureren Vidyard** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Vidyard configuratie](./media/vidyard-tutorial/tutorial_vidyard_configure.png)

8. In een ander browservenster, meld u aan bij uw bedrijf Vidyard Software site als beheerder.

9. Selecteer in het dashboard Vidyard **groep** > **beveiliging**

    ![Vidyard configuratie](./media/vidyard-tutorial/configure1.png)

1. Klik op **nieuw profiel** tabblad.

    ![Vidyard configuratie](./media/vidyard-tutorial/configure2.png)

11. In de **SAML-configuratie** sectie, voert u de volgende stappen uit:

    ![Vidyard configuratie](./media/vidyard-tutorial/configure3.png)

    a. Geef algemene profielnaam in de **profielnaam** textbox.

    b. Kopiëren **aanmeldingspagina voor eenmalige aanmelding gebruiker** waarde en plak deze in **URL aanmelden** textbox in **Vidyard domein en de URL's sectie** op Azure-portal.

    c. Kopiëren **ACS URL** waarde en plak deze in **antwoord-URL** textbox in **Vidyard domein en de URL's sectie** op Azure-portal.

    d. Kopiëren **verlener/metagegevens-URL** waarde en plak deze in **id** textbox in **Vidyard domein en de URL's sectie** op Azure-portal.

    e. Het gedownloade certificaatbestand openen vanuit Azure-portal in Kladblok en plak deze in de **X.509-certificaat** textbox.

    f. In de **eindpunt-URL van SAML** textbox, plak de waarde van **SAML Single Sign-On Service-URL** gekopieerd vanuit Azure-portal.

    g. Klik op **bevestigen**.

12. Selecteer in het tabblad eenmalige aanmelding **toewijzen** naast een bestaand profiel

    ![Vidyard configuratie](./media/vidyard-tutorial/configure4.png)

    > [!NOTE]
    > Nadat u een profiel voor eenmalige aanmelding hebt gemaakt, deze toewijzen aan alle gebruikers waarvoor toegang via Azure moet groep(en). Als de gebruiker niet in de groep waaraan ze zijn toegewezen bestaat, wordt Vidyard automatisch een gebruikersaccount maken en toewijzen van hun rol in realtime.

13. Selecteer de groep van uw organisatie, dat zichtbaar is in de **beschikbare groepen toe te wijzen**.

    ![Vidyard configuratie](./media/vidyard-tutorial/configure5.png)

14. Ziet u de toegewezen groepen onder de **groepen toegewezen**. Selecteer een rol voor de groep aan de hand van uw organisatie en klik op **bevestigen**.

    ![Vidyard configuratie](./media/vidyard-tutorial/configure6.png)

    > [!NOTE]
    > Raadpleeg voor meer informatie [deze doc](https://knowledge.vidyard.com/saml-single-sign-on-authentication/saml-based-single-sign-on-sso-in-vidyard).

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/vidyard-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/vidyard-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/vidyard-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/vidyard-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-vidyard-test-user"></a>Een testgebruiker Vidyard maken

Het doel van deze sectie is het maken van een gebruiker Britta Simon in Vidyard genoemd. Vidyard ondersteunt just-in-time-inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot Vidyard als deze nog niet bestaat.
>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [Vidyard ondersteuningsteam](mailto:support@vidyard.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Vidyard.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen Vidyard, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Vidyard**.

    ![De koppeling Vidyard in de lijst met toepassingen](./media/vidyard-tutorial/tutorial_vidyard_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Vidyard in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Vidyard.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/vidyard-tutorial/tutorial_general_01.png
[2]: ./media/vidyard-tutorial/tutorial_general_02.png
[3]: ./media/vidyard-tutorial/tutorial_general_03.png
[4]: ./media/vidyard-tutorial/tutorial_general_04.png

[100]: ./media/vidyard-tutorial/tutorial_general_100.png

[200]: ./media/vidyard-tutorial/tutorial_general_200.png
[201]: ./media/vidyard-tutorial/tutorial_general_201.png
[202]: ./media/vidyard-tutorial/tutorial_general_202.png
[203]: ./media/vidyard-tutorial/tutorial_general_203.png

