---
title: 'Zelfstudie: Azure Active Directory-integratie met muziek vereffenen | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Settling muziek.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 6f86a8a2-4bd0-40cc-b1b4-752fce123328
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2018
ms.author: jeedes
ms.openlocfilehash: 900254e42410aafa0d8e58048c7d88efa94dfed5
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55178542"
---
# <a name="tutorial-azure-active-directory-integration-with-settling-music"></a>Zelfstudie: Azure Active Directory-integratie met muziek vereffenen

In deze zelfstudie leert u hoe u Settling muziek integreren met Azure Active Directory (Azure AD).

Settling muziek integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang heeft tot vereffenen muziek.
- U kunt uw gebruikers automatisch ophalen aangemeld bij vereffenen muziek (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met vereffenen muziek, moet u de volgende items:

- Een Azure AD-abonnement
- Een Settling muziek eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Settling muziek toe te voegen uit de galerie
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-settling-music-from-the-gallery"></a>Settling muziek toe te voegen uit de galerie
Voor het configureren van de integratie van Settling muziek in Azure AD, moet u Settling muziek uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Settling muziek uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **vereffenen muziek**, selecteer **vereffenen muziek** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Muziek vereffenen in de lijst met resultaten](./media/settlingmusic-tutorial/tutorial_settlingmusic_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met vereffenen muziek op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Settling muziek is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in vereffenen muziek tot stand worden gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Settling muziek, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker van Settling muziek](#create-a-settling-music-test-user)**  : als u wilt een equivalent van Britta Simon in vereffenen muziek die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw Settling muziek-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met vereffenen muziek, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **vereffenen muziek** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/settlingmusic-tutorial/tutorial_settlingmusic_samlbase.png)

1. Op de **vereffenen muziek domein en URL's** sectie, voert u de volgende stappen uit:

    ![Muziek domein en URL's één aanmelding informatie vereffenen](./media/settlingmusic-tutorial/tutorial_settlingmusic_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<SUBDOMAIN>.rakurakuseisan.jp/<USERACCOUNT>/`

    b. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<SUBDOMAIN>.rakurakuseisan.jp/<USERACCOUNT>/`

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de daadwerkelijke aanmeldings-URL en id. Neem contact op met [vereffenen muziek Client ondersteuningsteam](https://rakurakuseisan.jp/) om deze waarden te verkrijgen. 
 
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![De link om het certificaat te downloaden](./media/settlingmusic-tutorial/tutorial_settlingmusic_certificate.png) 

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/settlingmusic-tutorial/tutorial_general_400.png)

1. Op de **vereffenen muziek configuratie** sectie, klikt u op **configureren vereffenen muziek** openen **aanmelding configureren** venster. Kopiëren de **afmelding URL's en SAML Single Sign-On Service** uit de **Naslaggids sectie.**

    ![Muziek configuratie vereffenen](./media/settlingmusic-tutorial/tutorial_settlingmusic_configure.png) 

1. In een ander browservenster, meld u aan bij vereffenen muziek als een beveiligingsbeheerder.

1. Boven op de pagina, klikt u op **management** tabblad.

    ![Stap 1 muziek vereffenen](./media/settlingmusic-tutorial/tutorial_settlingmusic_step1.png)

1. Klik op **systeeminstelling** tabblad.

    ![Stap 2 muziek vereffenen](./media/settlingmusic-tutorial/tutorial_settlingmusic_step2.png)

1. Schakel over naar **Security** tabblad.

    ![Stap 3 muziek vereffenen](./media/settlingmusic-tutorial/tutorial_settlingmusic_step3.png)

1. Op de **eenmalige aanmelding instelling** sectie, voert u de volgende stappen uit:

    ![Muziek step5 vereffenen](./media/settlingmusic-tutorial/tutorial_settlingmusic_step4.png)

    a. Klik op **om in te schakelen**.

    b. In de **aanmeldings-URL van de ID-provider** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML** die u hebt gekopieerd vanuit Azure portal.

    c. In de **afmeldings-URL van ID-provider** tekstvak, plak de waarde van **afmelding URL** die u hebt gekopieerd vanuit Azure portal.

    d. Klik op **bestand kiezen** het uploaden van de **certificaat (Base64)** die u hebt gedownload vormen van Azure portal.

    e. Klik op de knop **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/settlingmusic-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/settlingmusic-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/settlingmusic-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/settlingmusic-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-settling-music-test-user"></a>Maak een testgebruiker van Settling muziek

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Settling muziek. Werken met [vereffenen muziek Client ondersteuningsteam](https://rakurakuseisan.jp/) om toe te voegen de gebruikers in het Settling muziek-platform. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot muziek vereffenen.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon Settling muziek, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **vereffenen muziek**.

    ![De koppeling Settling muziek in de lijst met toepassingen](./media/settlingmusic-tutorial/tutorial_settlingmusic_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Settling muziek in het toegangsvenster, u moet u automatisch aangemeld bij uw Settling muziek-toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/settlingmusic-tutorial/tutorial_general_01.png
[2]: ./media/settlingmusic-tutorial/tutorial_general_02.png
[3]: ./media/settlingmusic-tutorial/tutorial_general_03.png
[4]: ./media/settlingmusic-tutorial/tutorial_general_04.png

[100]: ./media/settlingmusic-tutorial/tutorial_general_100.png

[200]: ./media/settlingmusic-tutorial/tutorial_general_200.png
[201]: ./media/settlingmusic-tutorial/tutorial_general_201.png
[202]: ./media/settlingmusic-tutorial/tutorial_general_202.png
[203]: ./media/settlingmusic-tutorial/tutorial_general_203.png

