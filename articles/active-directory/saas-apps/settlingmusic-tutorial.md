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
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2018
ms.author: jeedes
ms.openlocfilehash: 07533670a188c7f50a2848e76ed9d83a96c54cf7
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36211979"
---
# <a name="tutorial-azure-active-directory-integration-with-settling-music"></a>Zelfstudie: Azure Active Directory-integratie met muziek vereffenen

In deze zelfstudie leert u hoe Settling muziek integreren met Azure Active Directory (Azure AD).

Settling muziek integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang heeft tot vereffenen muziek.
- U kunt uw gebruikers automatisch ophalen aangemeld bij vereffenen muziek (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met vereffenen muziek, moet u de volgende items:

- Een Azure AD-abonnement
- Een Settling muziek eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Settling muziek toe te voegen uit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-settling-music-from-the-gallery"></a>Settling muziek toe te voegen uit de galerie
Voor het configureren van de integratie van Settling muziek in Azure AD, moet u Settling muziek uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Settling muziek uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **vereffenen muziek**, selecteer **vereffenen muziek** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Music vereffenen in de lijst met resultaten](./media/settlingmusic-tutorial/tutorial_settlingmusic_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met vereffenen muziek op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Settling muziek is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker bij het vereffenen muziek tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Settling muziek, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een gebruiker Settling muziek test](#create-a-settling-music-test-user)**  - vereffenen muziek die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw Settling music-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met vereffenen muziek, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **vereffenen muziek** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/settlingmusic-tutorial/tutorial_settlingmusic_samlbase.png)

3. Op de **vereffenen van URL's en muziek domein** sectie, voert u de volgende stappen uit:

    ![Music domein en de URL's eenmalige aanmelding informatie vereffenen](./media/settlingmusic-tutorial/tutorial_settlingmusic_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen: `https://<SUBDOMAIN>.rakurakuseisan.jp/<USERACCOUNT>/`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen: `https://<SUBDOMAIN>.rakurakuseisan.jp/<USERACCOUNT>/`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met het werkelijke aanmeldings-URL en de id. Neem contact op met [vereffenen muziek Client ondersteuningsteam](https://rakurakuseisan.jp/) ophalen van deze waarden. 
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/settlingmusic-tutorial/tutorial_settlingmusic_certificate.png) 

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/settlingmusic-tutorial/tutorial_general_400.png)

6. Op de **vereffenen muziek configuratie** sectie, klikt u op **configureren vereffenen muziek** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out URL's en SAML Single Sign-On Service** van de **Naslaggids punt.**

    ![Music configuratie vereffenen](./media/settlingmusic-tutorial/tutorial_settlingmusic_configure.png) 

7. In een ander browservenster, meld u aan bij vereffenen muziek als beveiligingsbeheerder.

8. Klik boven op de pagina **management** tabblad.

    ![Stap 1 muziek vereffenen](./media/settlingmusic-tutorial/tutorial_settlingmusic_step1.png)

9. Klik op **systeeminstelling** tabblad.

    ![Stap 2 muziek vereffenen](./media/settlingmusic-tutorial/tutorial_settlingmusic_step2.png)

10. Overschakelen naar **beveiliging** tabblad.

    ![Stap 3 muziek vereffenen](./media/settlingmusic-tutorial/tutorial_settlingmusic_step3.png)

11. Op de **eenmalige aanmelding instelling** sectie, voert u de volgende stappen uit:

    ![Music step5 vereffenen](./media/settlingmusic-tutorial/tutorial_settlingmusic_step4.png)

    a. Klik op **om in te schakelen**.

    b. In de **aanmeldings-URL van de ID-provider** textbox, plak de waarde van **SAML Single Sign-On Service-URL** die u hebt gekopieerd vanuit Azure-portal.

    c. In de **ID-provider afmelding URL** textbox, plak de waarde van **Sign-Out URL** die u hebt gekopieerd vanuit Azure-portal.

    d. Klik op **bestand kiezen** voor het uploaden van de **certificaat (Base64)** die u hebt gedownload vormen van de Azure-portal.

    e. Klik op de knop **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/settlingmusic-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/settlingmusic-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/settlingmusic-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/settlingmusic-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-settling-music-test-user"></a>Maken van een gebruiker Settling muziek testen

In deze sectie maakt maken u een gebruiker met de naam van Britta Simon in Settling muziek. Werken met [vereffenen muziek Client ondersteuningsteam](https://rakurakuseisan.jp/) toevoegen van de gebruikers in het Settling music-platform. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot muziek vereffenen.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen Settling muziek, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **vereffenen muziek**.

    ![De Settling music-koppeling in de lijst met toepassingen](./media/settlingmusic-tutorial/tutorial_settlingmusic_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Settling muziek in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw Settling music-toepassing.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



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

