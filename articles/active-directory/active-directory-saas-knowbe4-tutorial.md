---
title: 'Zelfstudie: Azure Active Directory-integratie met KnowBe4 Awareness beveiligingstraining | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en KnowBe4 Awareness beveiligingstraining.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: b80d2212-cc5f-4adb-836c-570640810c39
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/31/2017
ms.author: jeedes
ms.openlocfilehash: 8153c0824de2cda61fecf1da822c1ffa70d8f0d0
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-knowbe4-security-awareness-training"></a>Zelfstudie: Azure Active Directory-integratie met KnowBe4 Awareness beveiligingstraining

In deze zelfstudie leert u hoe KnowBe4 Awareness beveiligingstraining integreren met Azure Active Directory (Azure AD).

KnowBe4 Awareness beveiligingstraining integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de beveiligingstraining Awareness KnowBe4 heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij KnowBe4 beveiligingstraining Awareness (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met KnowBe4 Awareness beveiligingstraining, moet u de volgende items:

- Een Azure AD-abonnement
- Een KnowBe4 Awareness beveiligingstraining eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. KnowBe4 Awareness beveiligingstraining uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-knowbe4-security-awareness-training-from-the-gallery"></a>KnowBe4 Awareness beveiligingstraining uit de galerie toevoegen
Voor het configureren van de integratie van KnowBe4 Awareness beveiligingstraining in Azure AD, moet u KnowBe4 Awareness beveiligingstraining uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen KnowBe4 Awareness beveiligingstraining uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **KnowBe4 Awareness beveiligingstraining**, selecteer **KnowBe4 Awareness beveiligingstraining** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen de de toepassing.

    ![KnowBe4 beveiligingstraining inzicht in de lijst met resultaten](./media/active-directory-saas-knowbe4-tutorial/tutorial_knowbe4_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met KnowBe4 Awareness beveiligingstraining op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in KnowBe4 Awareness beveiligingstraining is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in KnowBe4 Awareness beveiligingstraining tot stand worden gebracht.

Wijs in KnowBe4 Awareness beveiligingstraining, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met KnowBe4 Awareness beveiligingstraining, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker KnowBe4 Awareness beveiligingstraining](#create-a-knowbe4-security-awareness-training-test-user)**  - KnowBe4 Awareness beveiligingstraining die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing KnowBe4 Awareness beveiligingstraining configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met KnowBe4 Awareness beveiligingstraining, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **KnowBe4 Awareness beveiligingstraining** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-knowbe4-tutorial/tutorial_knowbe4_samlbase.png)

3. Op de **Awareness KnowBe4-Training beveiligingsdomein en URL's** sectie, voert u de volgende stappen uit:

    ![URL's en Awareness KnowBe4-Training beveiligingsdomein één aanmelding informatie](./media/active-directory-saas-knowbe4-tutorial/tutorial_knowbe4_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<companyname>.KnowBe4.com/auth/saml/<instancename>`

    > [!NOTE] 
    > Eenmalige aanmelding URL-waarde is geen echte. Deze waarde bijwerken met de werkelijke URL voor eenmalige aanmelding. Neem contact op met [KnowBe4 beveiliging Awareness Training Client ondersteuningsteam](mailto:support@KnowBe4.com) deze waarde op te halen. 

    b. In de **id** textbox, typt u de string-waarde:`KnowBe4`

    > [!NOTE]
    >Dit is hoofdlettergevoelig

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Raw)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-knowbe4-tutorial/tutorial_knowbe4_certificate.png) 

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-knowbe4-tutorial/tutorial_general_400.png)

6. Op de **Awareness KnowBe4-Training Beveiligingsconfiguratie** sectie, klikt u op **configureren KnowBe4 Awareness beveiligingstraining** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Bewustzijn KnowBe4-Training Beveiligingsconfiguratie](./media/active-directory-saas-knowbe4-tutorial/tutorial_knowbe4_configure.png) 

7. Eenmalige aanmelding configureren op **KnowBe4 Awareness beveiligingstraining** zijde, moet u de gedownloade verzenden **certificaat (Raw)**, **Sign-Out-URL, SAML entiteit-ID en eenmalige aanmelding SAML Service-URL** naar [KnowBe4 beveiliging Awareness Training Client ondersteuningsteam](mailto:support@KnowBe4.com).

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-knowbe4-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-knowbe4-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-knowbe4-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-knowbe4-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-knowbe4-security-awareness-training-test-user"></a>Een testgebruiker KnowBe4 Awareness beveiligingstraining maken

Het doel van deze sectie is het maken van een gebruiker Britta Simon aangeroepen in KnowBe4 Awareness beveiligingstraining. KnowBe4 Awareness beveiligingstraining ondersteunt just-in-time-inrichting, dit is standaard ingeschakeld.

Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot de beveiligingstraining Awareness KnowBe4 als deze nog niet bestaat. 

>[!NOTE]
>Als u een gebruiker handmatig maken wilt, moet u contact op met de [KnowBe4 Awareness beveiligingstraining ondersteuningsteam](mailto:support@KnowBe4.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan KnowBe4 Awareness beveiligingstraining.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen KnowBe4 Awareness beveiligingstraining, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **KnowBe4 Awareness beveiligingstraining**.

    ![De koppeling KnowBe4 Awareness beveiligingstraining in de lijst met toepassingen](./media/active-directory-saas-knowbe4-tutorial/tutorial_knowbe4_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.
  
Als u op de tegel KnowBe4 Awareness beveiligingstraining in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing KnowBe4 Awareness beveiligingstraining. 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_203.png

