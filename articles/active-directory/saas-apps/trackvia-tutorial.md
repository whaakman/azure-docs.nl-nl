---
title: 'Zelfstudie: Azure Active Directory-integratie met TrackVia | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en TrackVia.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: e7010023-bdda-4a19-a335-19904e75b813
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: jeedes
ms.openlocfilehash: 1c4cc0517ad55cd3be862e974a0a707ef871d805
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55196375"
---
# <a name="tutorial-azure-active-directory-integration-with-trackvia"></a>Zelfstudie: Azure Active Directory-integratie met TrackVia

In deze zelfstudie leert u hoe u TrackVia integreren met Azure Active Directory (Azure AD).

TrackVia integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot TrackVia heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij TrackVia (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met TrackVia, moet u de volgende items:

- Een Azure AD-abonnement
- Een TrackVia eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. TrackVia uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-trackvia-from-the-gallery"></a>TrackVia uit de galerie toe te voegen
Voor het configureren van de integratie van TrackVia in Azure AD, moet u TrackVia uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen TrackVia uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **TrackVia**, selecteer **TrackVia** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![TrackVia in de lijst met resultaten](./media/trackvia-tutorial/tutorial_trackvia_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met TrackVia op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in TrackVia is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in TrackVia tot stand worden gebracht.

In TrackVia, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met TrackVia, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker TrackVia](#create-a-trackvia-test-user)**  : als u wilt een equivalent van Britta Simon in TrackVia die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing TrackVia.

**Voor het configureren van Azure AD eenmalige aanmelding met TrackVia, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **TrackVia** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/trackvia-tutorial/tutorial_trackvia_samlbase.png)

1. Op de **TrackVia domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![TrackVia domein en URL's, eenmalige aanmelding informatie](./media/trackvia-tutorial/tutorial_trackvia_url.png)

    In de **id** tekstvak typt u de waarde: `TrackVia`

1. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![TrackVia domein en URL's, eenmalige aanmelding informatie](./media/trackvia-tutorial/tutorial_trackvia_url1.png)

    Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://companyname.trackvia.com`
     
    > [!NOTE] 
    > Aanmeldings-URL-waarde is niet echt. Deze waarde bijwerken met de werkelijke aanmeldings-URL. Neem contact op met [TrackVia Client ondersteuningsteam](mailto:support@trackvia.com) deze waarde op te halen.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/trackvia-tutorial/tutorial_trackvia_certificate.png) 

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/trackvia-tutorial/tutorial_general_400.png)

1. Op de **TrackVia configuratie** sectie, klikt u op **configureren TrackVia** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID** uit de **Naslaggids sectie.**

    ![TrackVia configuratie](./media/trackvia-tutorial/tutorial_trackvia_configure.png)
    
1. In een ander browservenster en meld u aan bij uw bedrijf TrackVia site als beheerder.

1. Klik op Trackvia **Mijn Account** instellingen en selecteer vervolgens **Single Sign On** tabblad, voert u de volgende stappen uit:

    ![TrackVia configuratie](./media/trackvia-tutorial/configure1.png)

    a. In de **Identity Provider entiteit-ID** tekstvak plakken **SAML entiteit-ID** waarde die u hebt gekopieerd vanuit Azure portal.

    b. Selecteer de **bestand kiezen** voor het uploaden van het bestand met metagegevens die u hebt gedownload van de Azure-portal.

    c. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/trackvia-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/trackvia-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/trackvia-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/trackvia-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-trackvia-test-user"></a>Maak een testgebruiker TrackVia

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in TrackVia. TrackVia biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot TrackVia als deze nog niet bestaat.
>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [TrackVia ondersteuningsteam](mailto:support@trackvia.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan TrackVia.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan TrackVia toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **TrackVia**.

    ![De koppeling TrackVia in de lijst met toepassingen](./media/trackvia-tutorial/tutorial_trackvia_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel TrackVia in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing TrackVia.
Zie [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster. 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/trackvia-tutorial/tutorial_general_01.png
[2]: ./media/trackvia-tutorial/tutorial_general_02.png
[3]: ./media/trackvia-tutorial/tutorial_general_03.png
[4]: ./media/trackvia-tutorial/tutorial_general_04.png

[100]: ./media/trackvia-tutorial/tutorial_general_100.png

[200]: ./media/trackvia-tutorial/tutorial_general_200.png
[201]: ./media/trackvia-tutorial/tutorial_general_201.png
[202]: ./media/trackvia-tutorial/tutorial_general_202.png
[203]: ./media/trackvia-tutorial/tutorial_general_203.png
