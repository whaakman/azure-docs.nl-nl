---
title: "Zelfstudie: Azure Active Directory-integratie met N2F - onkostennota's | Microsoft Docs"
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en N2F - onkostennota's.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: f56d53d7-5a08-490a-bfb9-78fefc2751ec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2018
ms.author: jeedes
ms.openlocfilehash: d8d3d9c14ee1eaa5e5d6dc3245c0d4071306f12a
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55811627"
---
# <a name="tutorial-azure-active-directory-integration-with-n2f---expense-reports"></a>Zelfstudie: Azure Active Directory-integratie met N2F - onkostennota 's

In deze zelfstudie leert u hoe u integreert N2F - onkostennota's met Azure Active Directory (Azure AD).

Integratie N2F - onkostennota's met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot N2F heeft - 's onkostennota.
- U kunt uw gebruikers automatisch ophalen aangemeld bij N2F - onkostennota's (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met N2F - onkostennota's, moet u de volgende items:

- Een Azure AD-abonnement
- Een N2F - onkostennota eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. N2F - onkostennota's uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-n2f---expense-reports-from-the-gallery"></a>N2F - onkostennota's uit de galerie toevoegen

Als u wilt configureren van de integratie van N2F - onkostennota's in Azure AD, moet u toevoegen N2F - onkostennota's uit de galerie aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen, N2F - onkostennota's uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

4. Typ in het zoekvak **N2F - onkostennota**, selecteer **N2F - onkostennota** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![N2F - onkostennota's in de lijst met resultaten](./media/n2f-expensereports-tutorial/tutorial_n2f-expensereports_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie configureert u en test Azure AD eenmalige aanmelding met N2F - onkostennota's op basis van een testgebruiker met de naam 'Britta Simon'.

Voor eenmalige aanmelding voor het werk, Azure AD moet weten wat de equivalente-gebruiker in N2F - onkostennota is aan een gebruiker in Azure AD. Met andere woorden, een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in N2F - onkostennota's moet tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met N2F - onkostennota's, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
3. **Maken van een N2F - onkostennota testgebruiker** - dat een equivalent van Britta Simon in N2F - onkostennota's dat is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u Azure AD eenmalige aanmelding in de Azure-portal inschakelen en configureren van eenmalige aanmelding in uw N2F - onkostennota's toepassing.

**Voor het configureren van Azure AD Voer eenmalige aanmelding met N2F - onkostennota's, de volgende stappen uit:**

1. In de Azure-portal op de **N2F - onkostennota** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![In het dialoogvenster voor eenmalige aanmelding](./media/n2f-expensereports-tutorial/tutorial_n2f-expensereports_samlbase.png)

3. Op de **N2F - onkostennota's domein en URL's** sectie, als u wilt configureren van de toepassing in **IDP** gestart modus wordt de gebruiker hoeft niet te werk als de app al vooraf geïntegreerd met is Azure.

    ![N2F - onkostennota domein en URL's, eenmalige aanmelding informatie](./media/n2f-expensereports-tutorial/tutorial_n2f-expensereports_url1.png)

4. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![N2F - onkostennota domein en URL's, eenmalige aanmelding informatie](./media/n2f-expensereports-tutorial/tutorial_n2f-expensereports_url2.png)

    In de **aanmeldings-URL** tekstvak typt u de URL: `https://www.n2f.com/app/`

5. Op de **SAML-handtekeningcertificaat** sectie, klikt u op de knop kopiëren om te kopiëren **App-Url voor federatieve metagegevens** en plak deze in Kladblok.

    ![De link om het certificaat te downloaden](./media/n2f-expensereports-tutorial/tutorial_n2f-expensereports_certificate.png)

6. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/n2f-expensereports-tutorial/tutorial_general_400.png)

7. Op de **N2F - onkostennota's configuratie** sectie, klikt u op **configureren N2F - onkostennota** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID** uit de **Naslaggids sectie.**

    ![N2F - onkostennota configuratie](./media/n2f-expensereports-tutorial/tutorial_n2f-expensereports_configure.png)

8. In een ander browservenster, moet u zich aanmelden bij uw N2F - onkosten rapporten bedrijf site als beheerder.

9. Klik op **instellingen** en selecteer vervolgens **geavanceerde instellingen** in de vervolgkeuzelijst.

    ![N2F - onkostennota configuratie](./media/n2f-expensereports-tutorial/configure1.png)

10. Selecteer **Accountinstellingen** tabblad.

    ![N2F - onkostennota configuratie](./media/n2f-expensereports-tutorial/configure2.png)

11. Selecteer **verificatie** en selecteer vervolgens **+ toevoegen van een verificatiemethode** tabblad.

    ![N2F - onkostennota configuratie](./media/n2f-expensereports-tutorial/configure3.png)

12. Selecteer **SAML Microsoft Office 365** als verificatiemethode.

    ![N2F - onkostennota configuratie](./media/n2f-expensereports-tutorial/configure4.png)

13. Op de **verificatiemethode** sectie, voert u de volgende stappen uit:

    ![N2F - onkostennota configuratie](./media/n2f-expensereports-tutorial/configure5.png)

    a. In de **entiteit-ID** tekstvak, plak de **SAML entiteit-ID** waarde die u hebt gekopieerd vanuit Azure portal.

    b. In de **metagegevens-URL** tekstvak, plak de **App-Url voor federatieve metagegevens** waarde die u hebt gekopieerd vanuit Azure portal.

    c. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/n2f-expensereports-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/n2f-expensereports-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/n2f-expensereports-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/n2f-expensereports-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.

### <a name="create-a-n2f---expense-reports-test-user"></a>Maken van een N2F - onkostennota gebruiker testen

Om Azure AD-gebruikers zich aanmelden bij N2F - onkostennota's, moeten ze worden ingericht voor N2F - onkostennota's. In het geval van N2F - onkostennota's, is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Meld u aan bij uw N2F - onkosten rapporten bedrijf site als beheerder.

2. Klik op **instellingen** en selecteer vervolgens **geavanceerde instellingen** in de vervolgkeuzelijst.

   ![N2F - gebruiker onkosten toevoegen](./media/n2f-expensereports-tutorial/configure1.png)

3. Selecteer **gebruikers** tabblad in het linkernavigatievenster.

    ![N2F - onkostennota configuratie](./media/n2f-expensereports-tutorial/user1.png)

4. Selecteer **+ nieuwe gebruiker** tabblad.

   ![N2F - onkostennota configuratie](./media/n2f-expensereports-tutorial/user2.png)

5. Op de **gebruiker** sectie, voert u de volgende stappen uit:

    ![N2F - onkostennota configuratie](./media/n2f-expensereports-tutorial/user3.png)

    a. In de **e-mailadres** tekstvak, voer het e-mailadres van gebruiker, zoals **brittasimon@contoso.com**.

    b. In de **voornaam** tekstvak, geef de voornaam van de gebruiker, zoals **Julia**.

    c. In de **naam** tekstvak, voer de naam van gebruiker, zoals **BrittaSimon**.

    d. Kies **rol (N + 1) de directe manager**, en **deling** volgens de vereisten van uw organisatie.

    e. Klik op **valideren en te verzenden uitnodiging**.

    > [!NOTE]
    > Als u problemen ondervindt tijdens het toevoegen van de gebruiker, contact op met [N2F - ondersteuningsteam onkostennota](mailto:support@n2f.com)

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan N2F inschakelen - onkostennota's.

![De de gebruikersrol toewijzen][200]

**Britta Simon om aan te wijzen N2F - onkostennota's, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **N2F - onkostennota**.

    ![De N2F - onkostennota's koppeling in de lijst met toepassingen](./media/n2f-expensereports-tutorial/tutorial_n2f-expensereports_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u klikt op de N2F - onkostennota tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw N2F: onkostennota's toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/n2f-expensereports-tutorial/tutorial_general_01.png
[2]: ./media/n2f-expensereports-tutorial/tutorial_general_02.png
[3]: ./media/n2f-expensereports-tutorial/tutorial_general_03.png
[4]: ./media/n2f-expensereports-tutorial/tutorial_general_04.png

[100]: ./media/n2f-expensereports-tutorial/tutorial_general_100.png

[200]: ./media/n2f-expensereports-tutorial/tutorial_general_200.png
[201]: ./media/n2f-expensereports-tutorial/tutorial_general_201.png
[202]: ./media/n2f-expensereports-tutorial/tutorial_general_202.png
[203]: ./media/n2f-expensereports-tutorial/tutorial_general_203.png

