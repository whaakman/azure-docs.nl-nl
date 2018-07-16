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
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2018
ms.author: jeedes
ms.openlocfilehash: 24a5c1a260cf86f0860e0292c2eb9527f976a363
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39040507"
---
# <a name="tutorial-azure-active-directory-integration-with-signalfx"></a>Zelfstudie: Azure Active Directory-integratie met SignalFx

In deze zelfstudie leert u hoe u SignalFx integreren met Azure Active Directory (Azure AD).

SignalFx integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot SignalFx heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij SignalFx (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SignalFx, moet u de volgende items:

- Een Azure AD-abonnement
- Een SignalFx eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SignalFx uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-signalfx-from-the-gallery"></a>SignalFx uit de galerie toe te voegen
Voor het configureren van de integratie van SignalFx in Azure AD, moet u SignalFx uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SignalFx uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **SignalFx**, selecteer **SignalFx** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![SignalFx in de lijst met resultaten](./media/signalfx-tutorial/tutorial_signalfx_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met SignalFx op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in SignalFx is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in SignalFx tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met SignalFx, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker SignalFx](#create-a-signalfx-test-user)**  : als u wilt een equivalent van Britta Simon in SignalFx die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing SignalFx.

**Voor het configureren van Azure AD eenmalige aanmelding met SignalFx, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **SignalFx** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/signalfx-tutorial/tutorial_signalfx_samlbase.png)

3. Op de **SignalFx domein en URL's** sectie, voert u de volgende stappen uit:

    ![SignalFx domein en URL's, eenmalige aanmelding informatie](./media/signalfx-tutorial/tutorial_signalfx_url.png)

    a. In de **id** tekstvak, een URL typen: `https://api.signalfx.com/v1/saml/metadata`

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://api.signalfx.com/v1/saml/acs/<integration ID>`

    > [!NOTE] 
    > De bovenstaande waarde is geen echte waarde. U werkt de waarde bij met de werkelijke antwoord-URL, die later in de zelfstudie wordt uitgelegd.

4. SignalFx toepassing verwacht het SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de **gebruikerskenmerken** sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze.   

    ![Eenmalige aanmelding configureren](./media/signalfx-tutorial/tutorial_signalfx_attribute.png)

5. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding en voer de volgende stappen uit:
    
    | Naam kenmerk | Waarde kenmerk |
    | ------------------- | -------------------- |    
    | User.FirstName          | User.givenName |
    | User.email          | User.mail |
    | PersonImmutableID       | User.userPrincipalName    |
    | User.LastName       | User.surname    |

    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Configureren van eenmalige aanmelding toevoegen](./media/signalfx-tutorial/tutorial_attribute_04.png)

    ![Single Sign-On Addattb configureren](./media/signalfx-tutorial/tutorial_attribute_05.png)

    b. In de **naam** tekstvak typt u de naam van het kenmerk wordt weergegeven voor die rij.

    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Laat de **Namespace** leeg.
    
    e. Klik op **OK**.
 
6. Op de **SAML-handtekeningcertificaat** sectie, voert u de volgende stappen uit: 

    ![De downloadkoppeling certificaat](./media/signalfx-tutorial/tutorial_signalfx_certificate.png)

    a. Klik op de knop kopiëren om te kopiëren **App-Url voor federatieve metagegevens** en plak deze in Kladblok.

    b. Klik op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

7. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/signalfx-tutorial/tutorial_general_400.png)

8. Op de **SignalFx configuratie** sectie, klikt u op **configureren SignalFx** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID** uit de **Naslaggids sectie.**

    ![SignalFx configuratie](./media/signalfx-tutorial/tutorial_signalfx_configure.png) 

9. Aanmelding bij uw bedrijf SignalFx site als administrator.

10. In SignalFx op Klik bovenaan op **integraties** om de pagina integraties te openen.

    ![SignalFx-integratie](./media/signalfx-tutorial/tutorial_signalfx_intg.png)

11. Klik op **Azure Active Directory** tegel onder **aanmeldingsfuncties** sectie.
 
    ![SignalFx saml](./media/signalfx-tutorial/tutorial_signalfx_saml.png)

12. Klik op **nieuwe integratie** en klikt u onder de **installeren** tabblad de volgende stappen uitvoeren:
 
    ![SignalFx samlintgpage](./media/signalfx-tutorial/tutorial_signalfx_azure.png)

    a. In de **naam** tekstvak type, de naam van een nieuwe integratie, zoals **OurOrgName SAML SSO**.

    b. Kopieer de **integratie-ID** waarde en toevoegen met de **antwoord-URL** , zoals `https://api.signalfx.com/v1/saml/acs/<integration ID>` in de **antwoord-URL** tekstvak van **SignalFx domein en URL's** sectie in Azure portal.

    c. Klik op **-bestand uploaden** het uploaden van de **Base64-gecodeerd certificaat** gedownload vanuit Azure portal in de **certificaat** tekstvak.

    d. In de **URL-verlener** tekstvak, plak de waarde van **SAML entiteit-ID**, die u hebt gekopieerd vanuit Azure portal.

    e. In de **metagegevens-URL** tekstvak, plak de **App-Url voor federatieve metagegevens** die u hebt gekopieerd vanuit Azure portal.

    f. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/signalfx-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/signalfx-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/signalfx-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/signalfx-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
  
### <a name="create-a-signalfx-test-user"></a>Maak een testgebruiker SignalFx

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in SignalFx. SignalFx biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot SignalFx als deze nog niet bestaat.

Wanneer een gebruiker zich aanmeldt bij SignalFx van de SAML SSO voor de eerste keer [SignalFx ondersteuningsteam](mailto:kmazzola@signalfx.com) stuurt een e-mailbericht met een koppeling waarop ze via klikken moeten om te verifiëren. Dit gebeurt alleen de eerste keer dat de gebruiker zich aanmeldt; verdere aanmeldingspogingen moet niet worden gevalideerd e-mailbericht.

>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [SignalFx-ondersteuningsteam](mailto:kmazzola@signalfx.com)

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan SignalFx.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan SignalFx toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **SignalFx**.

    ![De koppeling SignalFx in de lijst met toepassingen](./media/signalfx-tutorial/tutorial_signalfx_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel SignalFx in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing SignalFx.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



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

