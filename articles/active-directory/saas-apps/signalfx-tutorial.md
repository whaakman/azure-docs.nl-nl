---
title: 'Zelfstudie: Azure Active Directory-integratie met SignalFx | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SignalFx.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 6d5ab4b0-29bc-4b20-8536-d64db7530f32
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2018
ms.author: jeedes
ms.openlocfilehash: 3fe91df742ab09a95902a349e797eb89c9a84db1
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2018
ms.locfileid: "35914831"
---
# <a name="tutorial-azure-active-directory-integration-with-signalfx"></a>Zelfstudie: Azure Active Directory-integratie met SignalFx

In deze zelfstudie leert u hoe SignalFx integreren met Azure Active Directory (Azure AD).

SignalFx integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot SignalFx heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij SignalFx (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SignalFx, moet u de volgende items:

- Een Azure AD-abonnement
- Een SignalFx eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SignalFx uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-signalfx-from-the-gallery"></a>SignalFx uit de galerie toevoegen
Voor het configureren van de integratie van SignalFx in Azure AD, moet u SignalFx uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SignalFx uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **SignalFx**, selecteer **SignalFx** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![SignalFx in de lijst met resultaten](./media/signalfx-tutorial/tutorial_signalfx_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met SignalFx op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in SignalFx is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in SignalFx tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met SignalFx, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker SignalFx](#create-a-signalfx-test-user)**  - SignalFx die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing SignalFx configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met SignalFx, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **SignalFx** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/signalfx-tutorial/tutorial_signalfx_samlbase.png)

3. Op de **SignalFx domein en de URL's** sectie, voert u de volgende stappen uit:

    ![URL's en SignalFx domein eenmalige aanmelding informatie](./media/signalfx-tutorial/tutorial_signalfx_url.png)

    a. In de **id** textbox, typ een URL: `https://api.signalfx.com/v1/saml/metadata`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen: `https://api.signalfx.com/v1/saml/acs/<integration ID>`

    > [!NOTE] 
    > De voorgaande waarde is geen echte waarde. U kunt de waarde bijwerken met de werkelijke antwoord-URL, die verderop in de zelfstudie wordt beschreven.

4. De SAML-asserties verwacht SignalFx toepassing in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken van beheren de **gebruikerskenmerken** sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze.   

    ![Eenmalige aanmelding configureren](./media/signalfx-tutorial/tutorial_signalfx_attribute.png)

5. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding en de volgende stappen uitvoeren:
    
    | Naam kenmerk | Waarde kenmerk |
    | ------------------- | -------------------- |    
    | User.FirstName          | User.givenName |
    | User.email          | User.mail |
    | PersonImmutableID       | User.userPrincipalName    |
    | User.LastName       | User.surname    |

    a. Klik op **toevoegen kenmerk** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren toevoegen](./media/signalfx-tutorial/tutorial_attribute_04.png)

    ![Eenmalige aanmelding Addattb configureren](./media/signalfx-tutorial/tutorial_attribute_05.png)

    b. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.

    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Laat de **Namespace** leeg.
    
    e. Klik op **OK**.
 
6. Op de **certificaat voor ondertekening van SAML** sectie, voert u de volgende stappen uit: 

    ![De downloadkoppeling certificaat](./media/signalfx-tutorial/tutorial_signalfx_certificate.png)

    a. Klik op de knop kopiëren om te kopiëren **App-Url voor federatieve metagegevens** en plak deze in Kladblok.

    b. Klik op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

7. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/signalfx-tutorial/tutorial_general_400.png)

8. Op de **SignalFx configuratie** sectie, klikt u op **configureren SignalFx** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML entiteit-ID** van de **Naslaggids punt.**

    ![SignalFx configuratie](./media/signalfx-tutorial/tutorial_signalfx_configure.png) 

9. Aanmelding bij uw bedrijf SignalFx site als administrator.

10. In SignalFx op bovenaan op **integraties** om de pagina integraties te openen.

    ![SignalFx-integratie](./media/signalfx-tutorial/tutorial_signalfx_intg.png)

11. Klik op **Azure Active Directory** tegel onder **aanmeldingsfuncties** sectie.
 
    ![SignalFx saml](./media/signalfx-tutorial/tutorial_signalfx_saml.png)

12. Klik op **nieuwe integratie** en klikt u onder de **installeren** tabblad de volgende stappen uitvoeren:
 
    ![SignalFx samlintgpage](./media/signalfx-tutorial/tutorial_signalfx_azure.png)

    a. In de **naam** textbox type, een nieuwe naam voor integratie, zoals **OurOrgName SAML SSO**.

    b. Kopieer de **integratie-ID** waarde en toevoegen aan de **antwoord-URL** zoals `https://api.signalfx.com/v1/saml/acs/<integration ID>` in de **antwoord-URL** textbox van **SignalFx domein en de URL's** sectie in Azure-portal.

    c. Klik op **-bestand uploaden** voor het uploaden van de **met Base64 versleuteld certificaat** gedownload van Azure-portal op de **certificaat** textbox.

    d. In de **URL-verlener** textbox, plak de waarde van **SAML entiteit-ID**, die u hebt gekopieerd uit de Azure portal.

    e. In de **metagegevens-URL** textbox, plak de **App-Url voor federatieve metagegevens** die u hebt gekopieerd uit de Azure portal.

    f. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/signalfx-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/signalfx-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/signalfx-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/signalfx-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
  
### <a name="create-a-signalfx-test-user"></a>Een testgebruiker SignalFx maken

Het doel van deze sectie is het maken van een gebruiker Britta Simon in SignalFx genoemd. SignalFx ondersteunt just-in-time-inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot SignalFx als deze nog niet bestaat.

Wanneer een gebruiker zich bij SignalFx van SAML met SSO voor het eerst aanmeldt [SignalFx ondersteuningsteam](mailto:kmazzola@signalfx.com) verzendt deze een e-mail met een koppeling waarop ze via klikken moeten om te verifiëren. Dit gebeurt alleen de eerste keer dat de gebruiker zich aanmeldt; daaropvolgende aanmeldingspogingen wordt geen e-validatie is vereist.

>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [ondersteuningsteam SignalFx](mailto:kmazzola@signalfx.com)

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan SignalFx.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen SignalFx, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **SignalFx**.

    ![De koppeling SignalFx in de lijst met toepassingen](./media/signalfx-tutorial/tutorial_signalfx_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel SignalFx in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing SignalFx.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/signalfx-tutorial/tutorial_general_01.png
[2]: ./media/signalfx-tutorial/tutorial_general_02.png
[3]: ./media/signalfx-tutorial/tutorial_general_03.png
[4]: ./media/signalfx-tutorial/tutorial_general_04.png

[100]: ./media/signalfx-tutorial/tutorial_general_100.png

[200]: ./media/signalfx-tutorial/tutorial_general_200.png
[201]: ./media/signalfx-tutorial/tutorial_general_201.png
[202]: ./media/signalfx-tutorial/tutorial_general_202.png
[203]: ./media/signalfx-tutorial/tutorial_general_203.png

