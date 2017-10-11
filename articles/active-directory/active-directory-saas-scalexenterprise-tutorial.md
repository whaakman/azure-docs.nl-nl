---
title: 'Zelfstudie: Azure Active Directory-integratie met ScaleX Enterprise | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en ScaleX Enterprise.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c2379a8d-a659-45f1-87db-9ba156d83183
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2017
ms.author: jeedes
ms.openlocfilehash: 0ebed0c2605862426384c0e219e52c9d626b6246
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-scalex-enterprise"></a>Zelfstudie: Azure Active Directory-integratie met ScaleX Enterprise

In deze zelfstudie leert u hoe ScaleX-onderneming integreren met Azure Active Directory (Azure AD).

ScaleX-onderneming integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot ScaleX Enterprise heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij ScaleX Enterprise (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, zien. Wat is er toegang tot toepassingen en eenmalige aanmelding met [Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met ScaleX voor ondernemingen, moet u de volgende items:

- Een Azure AD-abonnement
- Een ScaleX Enterprise eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij dit noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. ScaleX Enterprise uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-scalex-enterprise-from-the-gallery"></a>ScaleX Enterprise uit de galerie toevoegen
Voor het configureren van de integratie van ScaleX Enterprise in Azure AD, moet u ScaleX Enterprise uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen ScaleX Enterprise uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Klik op **toevoegen** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **ScaleX Enterprise**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_search.png)

5. Selecteer in het deelvenster resultaten **ScaleX Enterprise**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en test eenmalige aanmelding Azure AD met ScaleX Enterprise op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in de onderneming ScaleX is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in de onderneming ScaleX tot stand worden gebracht.

Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in ScaleX onderneming.

Om te configureren en testen van Azure AD eenmalige aanmelding met ScaleX voor ondernemingen, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker ScaleX Enterprise](#creating-a-scalex-enterprise-test-user)**  - ScaleX Enterprise die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing ScaleX Enterprise.

**Voor het configureren van Azure AD eenmalige aanmelding met ScaleX voor ondernemingen, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **ScaleX Enterprise** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster als **modus** Selecteer **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_samlbase.png)

3. Op de **ScaleX Enterprise domein en de URL's** sectie, voert u de volgende stappen uit als u wilt configureren, de toepassing in **IDP** modus gestart:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_url1.png)

    a. In de **id** textbox, typ de waarde met het volgende patroon volgen:`https://platform.rescale.com/saml2/<company id>/`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://platform.rescale.com/saml2/<company id>/acs/`

4. Controleer **weergeven geavanceerde instellingen voor URL**, als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_url2.png)

    In de **aanmeldings-URL** textbox, typ de waarde met het volgende patroon volgen:`https://platform.rescale.com/saml2/<company id>/sso/`
     
    > [!NOTE] 
    > Deze zijn niet de werkelijke waarden. Deze waarden bijwerken met de werkelijke-id, antwoord-URL of aanmeldings-URL. Neem contact op met [ScaleX Enterprise Client ondersteuningsteam](http://info.rescale.com/contact_sales) ophalen van deze waarden. 

5. Uw toepassing ScaleX verwacht de SAML-asserties in een specifieke indeling waarvoor u het aangepaste kenmerktoewijzingen aan uw configuratie van SAML-token kenmerken wijzigen. Klik op **weergeven en bewerken van alle andere gebruikerskenmerken** selectievakje openen van de aangepaste kenmerken instellingen.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-scalexenterprise-tutorial/scalex_attributes.png)
    
    a. Klik met de rechtermuisknop op het kenmerk **naam** en klik op verwijderen.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-scalexenterprise-tutorial/delete_attribute_name.png)

    b. Klik op **emailaddress** kenmerk om het kenmerk bewerken-venster te openen. Wijzig de waarde van **user.mail** naar **user.userprincipalname** en klik op Ok.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-scalexenterprise-tutorial/edit_email_attribute.png)   
    
5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_certificate.png) 

6. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_400.png)
    
7. Op de **ScaleX Ondernemingsconfiguratie** sectie, klikt u op **configureren ScaleX Enterprise** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML entiteit-ID** en **SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_configure.png) 

8. Eenmalige aanmelding configureren op **ScaleX Enterprise** side, meld u aan bij de bedrijfswebsite ScaleX Enterprise als beheerder.

9. Klik op het menu in het bovenste rechts en selecteer **Contoso beheer**.

    > [!NOTE] 
    > Contoso is slechts een voorbeeld. Dit moet de werkelijke naam van uw bedrijf. 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-scalexenterprise-tutorial/Test_Admin.png) 

10. Selecteer **integraties** van het bovenste menu en selecteer **Single Sign-On**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-scalexenterprise-tutorial/admin_sso.png) 

11. Vul het formulier als volgt:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-scalexenterprise-tutorial/scalex_admin_save.png) 
    
    a. Selecteer **'Alle gebruikers die kan worden geverifieerd met eenmalige aanmelding maken'.**

    b. **Service Provider saml**: waarde van het plakken ***urn: oasis: namen: tc: SAML:2.0:nameid-indeling: permanente***

    c. **Naam van identiteitsprovider e veld in de ACS-antwoord**: waarde van het plakken`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    d. **Identiteit EntityDescriptor entiteit-ID van Provider:** plakken de **SAML entiteit-ID** waarde opgehaald uit de Azure-portal.

    e. **ID-Provider SingleSignOnService URL:** plakken de **SAML Single Sign-On Service-URL** vanuit de Azure-portal.

    f. **Provider openbare X509 identiteitscertificaat:** Open de X509 certificaat gedownload van de Azure in Kladblok en plak de inhoud in dit vak. Zorg ervoor dat er dat geen regeleinden in het midden van de inhoud van het certificaat.
    
    g. Controleer de volgende selectievakjes: **ingeschakeld, NameID versleutelen en aanmelding AuthnRequests.**

    h. Klik op **SSO-Update-instellingen** de instellingen op te slaan.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_01.png) 

2. Ga naar **gebruikers en groepen** en klik op **alle gebruikers** om de lijst met gebruikers weer te geven.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_02.png) 

3. Aan de bovenkant van het dialoogvenster, klikt u op **toevoegen** openen de **gebruiker** dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-scalex-enterprise-test-user"></a>Maken van een testgebruiker ScaleX Enterprise

Om Azure AD-gebruikers zich aanmelden bij ScaleX Enterprise, moeten ze worden ingericht voor ScaleX onderneming. Inrichting is een automatische taak in het geval van ScaleX Enterprise, en er zijn geen handmatige stappen vereist. Elke gebruiker die kan worden geverifieerd met eenmalige aanmelding referenties worden automatisch aan de kant ScaleX ingericht.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding gebruiken door de gebruikerstoegang verlenen aan ScaleX Enterprise.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen ScaleX Enterprise, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **ScaleX Enterprise**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.

### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Klik op de tegel ScaleX Enterprise in het deelvenster toegang, u wordt u automatisch aangemeld bij uw toepassing ScaleX Enterprise. Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](https://msdn.microsoft.com/library/dn308586).


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_203.png

