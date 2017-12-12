---
title: 'Zelfstudie: Azure Active Directory-integratie met Yonyx interactieve gidsen | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en de interactieve gidsen Yonyx.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 07db4e01-319b-4cb6-9b93-4577bffd3cbc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/16/2017
ms.author: jeedes
ms.openlocfilehash: e221959a9997c44bbcb1fe97273b2e40b1eec06c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-yonyx-interactive-guides"></a>Zelfstudie: Azure Active Directory-integratie met Yonyx interactieve handleidingen

In deze zelfstudie leert u hoe de interactieve gidsen Yonyx integreren met Azure Active Directory (Azure AD).

Interactieve gidsen Yonyx integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Yonyx interactieve handleidingen heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij de interactieve gidsen Yonyx (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Yonyx interactieve gidsen, moet u de volgende items:

- Een Azure AD-abonnement
- Een interactieve gidsen Yonyx eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Interactieve gidsen Yonyx uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-yonyx-interactive-guides-from-the-gallery"></a>Interactieve gidsen Yonyx uit de galerie toevoegen
Voor het configureren van de integratie van Yonyx interactieve handleidingen in Azure AD, moet u interactieve gidsen Yonyx uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u de interactieve hulplijnen Yonyx uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Yonyx interactieve gidsen**, selecteer **Yonyx interactieve gidsen** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Yonyx interactieve handleidingen in de lijst met resultaten](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyxinteractiveguides_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met Yonyx interactieve gidsen op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in de interactieve handleidingen Yonyx is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in de interactieve handleidingen Yonyx tot stand worden gebracht.

In de interactieve handleidingen Yonyx, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en testen Azure AD eenmalige aanmelding met Yonyx interactieve gidsen, moet u voltooien van de volgende elementen:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Yonyx interactieve gidsen](#create-a-yonyx-interactive-guides-test-user)**  - Yonyx interactieve gidsen die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing Yonyx interactieve gidsen.

**Voor het configureren van Azure AD eenmalige aanmelding met Yonyx interactieve gidsen, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Yonyx interactieve gidsen** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyxinteractiveguides_samlbase.png)

3. Op de **Yonyx interactieve handleidingen domein en de URL's** sectie, voert u de volgende stappen uit:

    ![URL's en Yonyx interactieve handleidingen domein eenmalige aanmelding informatie](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyxinteractiveguides_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<company name>.yonyx.com/y/conversation/?id=<guid number>`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://<company name>.yonyx.com`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met het werkelijke aanmeldings-URL en de id. Neem contact op met [Yonyx interactieve handleidingen Client ondersteuningsteam](mailto:support@yonyx.com) ophalen van deze waarden. 
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyxinteractiveguides_certificate.png) 

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-yonyx-tutorial/tutorial_general_400.png)

6. Op de **Yonyx interactieve handleidingen configuratie** sectie, klikt u op **Yonyx interactieve gidsen configureren** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Yonyx interactieve gidsen configuratie](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyxinteractiveguides_configure.png) 

7. Eenmalige aanmelding configureren op **Yonyx interactieve gidsen** zijde, moet u de gedownloade verzenden **Certificate(Base64)**, **Sign-Out URL**, **SAML één Service-URL aanmelding** **SAML entiteit-ID** naar [Yonyx interactieve gidsen ondersteuningsteam](mailto:support@yonyx.com). Ze deze instelling zodat de SAML SSO-verbinding juist is ingesteld op beide zijden ingesteld.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

  ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![De Azure Active Directory-knop](./media/active-directory-saas-yonyx-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-yonyx-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![De knop toevoegen](./media/active-directory-saas-yonyx-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het dialoogvenster gebruiker](./media/active-directory-saas-yonyx-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="create-a-yonyx-interactive-guides-test-user"></a>Maak een testgebruiker Yonyx interactieve handleidingen

Het doel van deze sectie is het maken van een gebruiker Britta Simon aangeroepen in de interactieve gidsen Yonyx. Interactieve gidsen Yonyx ondersteunt just-in-time-inrichting, dit is standaard ingeschakeld.

Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot Yonyx interactieve gidsen als deze nog niet bestaat.

>[!NOTE]
>Als u een gebruiker handmatig maken moet, moet u contact op met het ondersteuningsteam Yonyx interactieve gidsen via < mailto:support@yonyx.com >. 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Yonyx interactieve gidsen.

![Toewijzen van de gebruikersrol][200]

**Britta Simon om aan te wijzen Yonyx interactieve gidsen, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Yonyx interactieve gidsen**.

    ![De koppeling Yonyx interactieve handleidingen in de lijst met toepassingen](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyxinteractiveguides_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Yonyx interactieve handleidingen in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Yonyx interactieve gidsen.

Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_203.png

