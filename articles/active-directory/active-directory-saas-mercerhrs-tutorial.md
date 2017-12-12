---
title: 'Zelfstudie: Azure Active Directory-integratie met Mercer BenefitsCentral (MBC) | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Mercer BenefitsCentral (MBC).
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 3788b28c-49aa-4208-9acd-630362008e89
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/21/2017
ms.author: jeedes
ms.openlocfilehash: dfc963be9d087c6ad1c56dcdaf922ff5dbf5b284
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mercer-benefitscentral-mbc"></a>Zelfstudie: Azure Active Directory-integratie met Mercer BenefitsCentral (MBC)

In deze zelfstudie leert u hoe Mercer BenefitsCentral (MBC) integreren met Azure Active Directory (Azure AD).

Mercer BenefitsCentral (MBC) integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die Mercer BenefitsCentral (MBC) toegang heeft.
- U kunt uw gebruikers automatisch ophalen aangemelde naar Mercer BenefitsCentral (MBC) (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Mercer BenefitsCentral (MBC), moet u de volgende items:

- Een Azure AD-abonnement
- Een Mercer BenefitsCentral (MBC) eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Mercer BenefitsCentral (MBC) uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-mercer-benefitscentral-mbc-from-the-gallery"></a>Mercer BenefitsCentral (MBC) uit de galerie toevoegen
Voor het configureren van de integratie van Mercer BenefitsCentral (MBC) in Azure AD, moet u Mercer BenefitsCentral (MBC) uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Mercer BenefitsCentral (MBC) uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Mercer BenefitsCentral (MBC)**, selecteer **Mercer BenefitsCentral (MBC)** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Mercer BenefitsCentral (MBC) in de lijst met resultaten](./media/active-directory-saas-mercerhrs-tutorial/tutorial_mercerhrs_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met Mercer BenefitsCentral (MBC) op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Mercer BenefitsCentral (MBC) is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Mercer BenefitsCentral (MBC) tot stand worden gebracht.

In Mercer BenefitsCentral (MBC), wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Mercer BenefitsCentral (MBC), moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Mercer BenefitsCentral (MBC)](#create-a-mercer-benefitscentral-mbc-test-user)**  - hebben een equivalent van Britta Simon in Mercer BenefitsCentral (MBC) die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In deze sectie maakt u Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing Mercer BenefitsCentral (MBC).

**Voor het configureren van Azure AD eenmalige aanmelding met Mercer BenefitsCentral (MBC), moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Mercer BenefitsCentral (MBC)** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-mercerhrs-tutorial/tutorial_mercerhrs_samlbase.png)

3. Op de **Mercer BenefitsCentral (MBC)-domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Mercer BenefitsCentral (MBC)-domein en de URL's van eenmalige aanmelding informatie](./media/active-directory-saas-mercerhrs-tutorial/tutorial_mercerhrs_url.png)

    a. In de **id** textbox, typ een URL met het volgende patroon volgen:`stg.mercerhrs.com/saml2.0`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://ssous-stg.mercerhrs.com/SP2/Saml2AssertionConsumer.aspx`

    > [!NOTE] 
    > Antwoord-URL-waarde is geen echte. Deze waarde bijwerken met de werkelijke antwoord-URL. Neem contact op met [Mercer BenefitsCentral (MBC) ondersteuningsteam](https://www.mercer.com/contact-us.html) deze waarde op te halen.

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-mercerhrs-tutorial/tutorial_mercerhrs_certificate.png) 

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-mercerhrs-tutorial/tutorial_general_400.png)

6. Op de **Mercer BenefitsCentral (MBC) configuratie** sectie, klikt u op **configureren Mercer BenefitsCentral (MBC)** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Configuratie van Mercer BenefitsCentral (MBC)](./media/active-directory-saas-mercerhrs-tutorial/tutorial_mercerhrs_configure.png) 

7. Eenmalige aanmelding configureren op **Mercer BenefitsCentral (MBC)** zijde, moet u de gedownloade verzenden **Metadata XML** en **SAML Single Sign-On Service-URL** naar [Mercer BenefitsCentral (MBC) ondersteuningsteam](https://www.mercer.com/contact-us.html). Ze deze instelling zodat de SAML SSO-verbinding juist is ingesteld op beide zijden ingesteld.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-mercerhrs-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-mercerhrs-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-mercerhrs-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-mercerhrs-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
  
### <a name="create-a-mercer-benefitscentral-mbc-test-user"></a>Maak een testgebruiker Mercer BenefitsCentral (MBC)

In deze sectie maakt maken u een gebruiker Britta Simon aangeroepen in Mercer uur. Werken met [Mercer BenefitsCentral (MBC) ondersteuningsteam](https://www.mercer.com/contact-us.html) om toe te voegen de gebruikers van het platform Mercer uur. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding toegang verleent Mercer BenefitsCentral (MBC) gebruiken.

![Toewijzen van de gebruikersrol][200] 

**Als u wilt toewijzen Britta Simon Mercer BenefitsCentral (MBC), moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Mercer BenefitsCentral (MBC)**.

    ![De koppeling Mercer BenefitsCentral (MBC) in de lijst met toepassingen](./media/active-directory-saas-mercerhrs-tutorial/tutorial_mercerhrs_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Mercer BenefitsCentral (MBC) in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Mercer BenefitsCentral (MBC).
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-mercerhrs-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mercerhrs-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mercerhrs-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mercerhrs-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mercerhrs-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mercerhrs-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mercerhrs-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mercerhrs-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mercerhrs-tutorial/tutorial_general_203.png

