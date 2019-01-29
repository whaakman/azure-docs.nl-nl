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
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2018
ms.author: jeedes
ms.openlocfilehash: 6f2d869f345aeb8f50d42de6b1533b849ffb2182
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55197565"
---
# <a name="tutorial-azure-active-directory-integration-with-signalfx"></a>Zelfstudie: Azure Active Directory-integratie met SignalFx

In deze zelfstudie leert u hoe u SignalFx integreren met Azure Active Directory (Azure AD).

SignalFx integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot SignalFx heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij SignalFx (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SignalFx, moet u de volgende items:

- Een Azure AD-abonnement
- Een SignalFx eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SignalFx uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-signalfx-from-the-gallery"></a>SignalFx uit de galerie toe te voegen
Voor het configureren van de integratie van SignalFx in Azure AD, moet u SignalFx uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SignalFx uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **SignalFx**, selecteer **SignalFx** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![SignalFx in de lijst met resultaten](./media/signalfx-tutorial/tutorial_signalfx_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met SignalFx op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in SignalFx is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in SignalFx tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met SignalFx, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker SignalFx](#create-a-signalfx-test-user)**  : als u wilt een equivalent van Britta Simon in SignalFx die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing SignalFx.

**Voor het configureren van Azure AD eenmalige aanmelding met SignalFx, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **SignalFx** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/signalfx-tutorial/tutorial_signalfx_samlbase.png)

1. Op de **SignalFx domein en URL's** sectie, voert u de volgende stappen uit:

    ![SignalFx domein en URL's, eenmalige aanmelding informatie](./media/signalfx-tutorial/tutorial_signalfx_url.png)

    a. In de **id** tekstvak, een URL typen: `https://api.signalfx.com/v1/saml/metadata`

    b. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://api.signalfx.com/v1/saml/acs/<integration ID>`

    > [!NOTE] 
    > De bovenstaande waarde is geen echte waarde. U werkt de waarde bij met de werkelijke antwoord-URL, die later in de zelfstudie wordt uitgelegd.

1. SignalFx toepassing verwacht het SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit de sectie **Gebruikerskenmerken** op de integratiepagina van de toepassing-beheren. In de volgende schermopname ziet u een voorbeeld hiervan.   

    ![Eenmalige aanmelding configureren](./media/signalfx-tutorial/tutorial_signalfx_attribute.png)

1. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding en voer de volgende stappen uit:
    
    | Naam kenmerk | Waarde kenmerk |
    | ------------------- | -------------------- |    
    | User.FirstName          | user.givenname |
    | User.email          | user.mail |
    | PersonImmutableID       | user.userprincipalname    |
    | User.LastName       | user.surname    |

    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Configureren van eenmalige aanmelding toevoegen](./media/signalfx-tutorial/tutorial_attribute_04.png)

    ![Single Sign-On Addattb configureren](./media/signalfx-tutorial/tutorial_attribute_05.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Laat **Naamruimte** leeg.
    
    e. Klik op **OK**.
 
1. Op de **SAML-handtekeningcertificaat** sectie, voert u de volgende stappen uit: 

    ![De link om het certificaat te downloaden](./media/signalfx-tutorial/tutorial_signalfx_certificate.png)

    a. Klik op de knop kopiëren om te kopiëren **App-Url voor federatieve metagegevens** en plak deze in Kladblok.

    b. Klik op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

1. Klik op de knop **Save**.

    ![De knop voor enkelvoudige aanmelding configureren](./media/signalfx-tutorial/tutorial_general_400.png)

1. Op de **SignalFx configuratie** sectie, klikt u op **configureren SignalFx** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID** uit de **Naslaggids sectie.**

    ![SignalFx configuratie](./media/signalfx-tutorial/tutorial_signalfx_configure.png) 

1. Aanmelding bij uw bedrijf SignalFx site als administrator.

1. In SignalFx op Klik bovenaan op **integraties** om de pagina integraties te openen.

    ![SignalFx-integratie](./media/signalfx-tutorial/tutorial_signalfx_intg.png)

1. Klik op **Azure Active Directory** tegel onder **aanmeldingsfuncties** sectie.
 
    ![SignalFx saml](./media/signalfx-tutorial/tutorial_signalfx_saml.png)

1. Klik op **nieuwe integratie** en klikt u onder de **installeren** tabblad de volgende stappen uitvoeren:
 
    ![SignalFx samlintgpage](./media/signalfx-tutorial/tutorial_signalfx_azure.png)

    a. In de **naam** tekstvak type, de naam van een nieuwe integratie, zoals **OurOrgName SAML SSO**.

    b. Kopieer de **integratie-ID** waarde en toevoegen met de **antwoord-URL** , zoals `https://api.signalfx.com/v1/saml/acs/<integration ID>` in de **antwoord-URL** tekstvak van **SignalFx domein en URL's** sectie in Azure portal.

    c. Klik op **-bestand uploaden** het uploaden van de **Base64-gecodeerd certificaat** gedownload vanuit Azure portal in de **certificaat** tekstvak.

    d. In de **URL-verlener** tekstvak, plak de waarde van **SAML entiteit-ID**, die u hebt gekopieerd vanuit Azure portal.

    e. In de **metagegevens-URL** tekstvak, plak de **App-Url voor federatieve metagegevens** die u hebt gekopieerd vanuit Azure portal.

    f. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/signalfx-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/signalfx-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/signalfx-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/signalfx-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
  
### <a name="create-a-signalfx-test-user"></a>Maak een testgebruiker SignalFx

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in SignalFx. SignalFx biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot SignalFx als deze nog niet bestaat.

Wanneer een gebruiker zich aanmeldt bij SignalFx van de SAML SSO voor de eerste keer [SignalFx ondersteuningsteam](mailto:kmazzola@signalfx.com) stuurt een e-mailbericht met een koppeling waarop ze via klikken moeten om te verifiëren. Dit gebeurt alleen de eerste keer dat de gebruiker zich aanmeldt; verdere aanmeldingspogingen moet niet worden gevalideerd e-mailbericht.

>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [SignalFx-ondersteuningsteam](mailto:kmazzola@signalfx.com)

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan SignalFx.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan SignalFx toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **SignalFx**.

    ![De koppeling SignalFx in de lijst met toepassingen](./media/signalfx-tutorial/tutorial_signalfx_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel SignalFx in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing SignalFx.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



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

