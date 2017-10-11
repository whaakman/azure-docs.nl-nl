---
title: 'Zelfstudie: Azure Active Directory-integratie met MOVEit overdracht - Azure AD-integratie | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en MOVEit overdracht - Azure AD-integratie.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8ff7102d-be73-4888-ae81-d8e3d01dd534
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: d35aceb9be2d0ff49f86a00cc84f5deb198d88f0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-moveit-transfer---azure-ad-integration"></a>Zelfstudie: Azure Active Directory-integratie met MOVEit overdracht - Azure AD-integratie

In deze zelfstudie leert u het integreren van MOVEit overdracht - Azure AD-integratie met Azure Active Directory (Azure AD).

Integratie van MOVEit overdracht - Azure AD-integratie met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot MOVEit overdracht - Azure AD-integratie heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij MOVEit overdracht - Azure AD-integratie met hun Azure AD-accounts (Single Sign-On) inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met MOVEit overdracht - integratie van Azure AD, moet u de volgende items:

- Een Azure AD-abonnement
- De overdracht van een MOVEit - Azure AD-integratie eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Overdracht van MOVEit - Azure AD-integratie uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-moveit-transfer---azure-ad-integration-from-the-gallery"></a>Overdracht van MOVEit - Azure AD-integratie uit de galerie toevoegen
Om de integratie van MOVEit overdracht - Azure AD-integratie met Azure AD te configureren die u wilt toevoegen MOVEit overdracht - Azure AD-integratie uit de galerie aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen MOVEit overdracht - Azure AD-integratie uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **MOVEit overdracht - Azure AD-integratie**, selecteer **MOVEit overdracht - Azure AD-integratie** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Overdracht van MOVEit - Azure AD-integratie in de lijst met resultaten](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met MOVEit overdracht - Azure AD-integratie op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in MOVEit overdracht - Azure AD-integratie is voor een gebruiker in Azure AD. Met andere woorden, een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in overdrachts-MOVEit - Azure AD-integratie moet tot stand worden gebracht.

Wijs in overdrachts-MOVEit - Azure AD-integratie, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en testen Azure AD eenmalige aanmelding met MOVEit Transfer - Azure AD-integratie moet u de volgende elementen voltooid:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een overdracht MOVEit - Azure AD-integratie testgebruiker](#create-a-moveit-transfer---azure-ad-integration-test-user)**  - MOVEit overdracht - Azure AD-integratie die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in de overdracht van uw MOVEit - toepassing voor Azure AD-integratie.

**Voer de volgende stappen uit voor het configureren van Azure AD eenmalige aanmelding met MOVEit overdracht - Azure AD-integratie:**

1. In de Azure-portal op de **MOVEit overdracht - Azure AD-integratie** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_samlbase.png)

3. Op de **MOVEit overdracht - URL's en Azure AD-integratie domein** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://contoso.com`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://contoso.com/<tenatid>`

    c. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://contoso.com/<tenatid>/SAML/SSO/HTTP-Post`    
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke id, antwoord-URL en aanmeldings-URL. U kunt deze waarden later in verwijzen **metagegevens-URL voor Service Provider** sectie of neem contact op met [MOVEit overdracht - ondersteuningsteam van Azure AD-integratie Client](https://community.ipswitch.com/s/support) ophalen van deze waarden.

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_certificate.png) 

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_400.png)
    
6. Meld u aan op uw tenant MOVEit overdracht als beheerder.

7. Klik in het navigatiedeelvenster links op **instellingen**.

    ![Instellingen sectie op App aan clientzijde](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_000.png)

8. Klik op **eenmalige aanmelding** koppeling, die zich onder **beveiligingsbeleid-gebruikersverificatie >**.

    ![Security-beleid op App aan clientzijde](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_001.png)

9. Klik op de koppeling van de metagegevens-URL voor het downloaden van het metagegevensdocument.

    ![Serviceprovider metagegevens-URL](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_002.png)
    
    * Controleer of **id van de entiteit** overeenkomt met **id** in de **MOVEit overdracht - URL's en Azure AD-integratie domein** sectie.
    * Controleer of **AssertionConsumerService** locatie-URL overeenkomt met **antwoord-URL** in de **MOVEit overdracht - URL's en Azure AD-integratie domein** sectie.
    
    ![Eenmalige aanmelding op App aan clientzijde configureren](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_007.png)

10. Klik op **identiteitsprovider toevoegen** knop een nieuwe federatieve identiteitsprovider toevoegen.

    ![ID-Provider toevoegen](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_003.png)

11. Klik op **bladeren...**  om te selecteren in het bestand met metagegevens die u hebt gedownload van Azure-portal, klikt u vervolgens op **identiteitsprovider toevoegen** het gedownloade bestand te uploaden.

    ![SAML-identiteitsprovider](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_004.png)

12. Selecteer "**Ja**' als **ingeschakeld** in de **bewerken federatieve identiteit Providerinstellingen...**  pagina en klik op **opslaan**.

    ![Federatieve identiteiten Providerinstellingen](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_005.png)

13. In de **bewerken federatieve identiteit Provider gebruikersinstellingen** pagina, de volgende acties uitvoeren:
    
    ![Federatieve identiteiten Providerinstellingen bewerken](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_006.png)
    
    a. Selecteer **SAML NameID** als **aanmeldingsnaam**.
    
    b. Selecteer **andere** als **volledige naam** en in de **kenmerknaam** textbox plaatsen de waarde: `http://schemas.microsoft.com/identity/claims/displayname`.
    
    c. Selecteer **andere** als **e** en in de **kenmerknaam** textbox plaatsen de waarde: `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.
    
    d. Selecteer **Ja** als **automatisch maken van account aanmeldt**.
    
    e. Klik op **opslaan** knop.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-moveit-transfer---azure-ad-integration-test-user"></a>Een MOVEit Transfer - Azure AD-integratie testgebruiker maken

Het doel van deze sectie is het maken van een gebruiker Britta Simon aangeroepen in MOVEit overdracht - Azure AD-integratie. Overdracht van MOVEit - Azure AD-integratie ondersteunt just-in-time-inrichting, die u hebt ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot MOVEit overdracht - Azure AD-integratie als deze nog niet bestaat.

>[!NOTE]
>Als u een gebruiker handmatig maken wilt, moet u contact op met de [MOVEit overdracht - ondersteuningsteam van Azure AD-integratie Client](https://community.ipswitch.com/s/support).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang op de overdracht van MOVEit - Azure AD-integratie.

![Toewijzen van de gebruikersrol][200] 

**Voer de volgende stappen uit om toe te wijzen Britta Simon op de overdracht van MOVEit - Azure AD-integratie:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **MOVEit overdracht - Azure AD-integratie**.

    ![De overdracht MOVEit - Azure AD-integratie koppeling in de lijst met toepassingen](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

Het doel van deze sectie is het testen van uw Azure AD SSO-configuratie met behulp van het toegangsvenster.

Wanneer u klikt op de overdracht MOVEit - tegel Azure AD-integratie in het deelvenster toegang u moet ophalen automatisch aangemeld bij de overdracht van uw MOVEit - toepassing voor Azure AD-integratie. 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_203.png

