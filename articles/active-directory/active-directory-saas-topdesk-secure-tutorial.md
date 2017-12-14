---
title: 'Zelfstudie: Azure Active Directory-integratie met TOPdesk - beveiligde | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en TOPdesk - veilig.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8e06ee33-18f9-4c05-9168-e6b162079d88
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: jeedes
ms.openlocfilehash: ca3362bc3f966adaf9940f6eb4bec5235c6ea7d8
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---secure"></a>Zelfstudie: Azure Active Directory-integratie met TOPdesk - beveiligen

In deze zelfstudie leert u hoe TOPdesk - integreren met Azure Active Directory (Azure AD) beveiligen.

TOPdesk - beveiligde integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot TOPdesk - veilig heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij TOPdesk - beveiligde (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met TOPdesk - wilt beveiligen, moet u de volgende items:

- Een Azure AD-abonnement
- Een TOPdesk - beveiligde eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toe te voegen TOPdesk - beveiligen uit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-topdesk---secure-from-the-gallery"></a>Toe te voegen TOPdesk - beveiligen uit de galerie
Voor het configureren van de integratie van TOPdesk - beveiligen met Azure AD, moet u TOPdesk add - beveiligen uit de galerie aan de lijst met beheerde SaaS apps.

**Als u wilt toevoegen, TOPdesk - beveiligen uit de galerie, de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **TOPdesk - beveiligde**, selecteer **TOPdesk - beveiligde** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![TOPdesk - beveiligd in de lijst met resultaten](./media/active-directory-saas-topdesk-secure-tutorial/tutorial_topdesk-secure_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met TOPdesk - beveiligde op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in TOPdesk - beveiligde is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in TOPdesk - beveiligde tot stand worden gebracht.

In TOPdesk - beveiligen, wordt de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met TOPdesk - wilt beveiligen, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een TOPdesk - beveiligde testgebruiker](#create-a-topdesk---secure-test-user)**  - hebben een equivalent van Britta Simon in TOPdesk - beveiligde die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw TOPdesk - beveiligde toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met TOPdesk - beveiligen, voert u de volgende stappen uit:**

1. In de Azure-portal op de **TOPdesk - beveiligde** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-topdesk-secure-tutorial/tutorial_topdesk-secure_samlbase.png)

3. Op de **TOPdesk - domein beveiligen en URL's** sectie, voert u de volgende stappen uit:

    ![TOPdesk - URL's en beveiligd domein met eenmalige aanmelding informatie](./media/active-directory-saas-topdesk-secure-tutorial/tutorial_topdesk-secure_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<companyname>.topdesk.net`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://<companyname>.topdesk.net/tas/secure/login/verify`

    c. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://<companyname>.topdesk.net/tas/public/login/saml`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met het werkelijke aanmeldings-URL en de id. Antwoord-URL wordt verderop in de zelfstudie beschreven. Neem contact op met [TOPdesk - ondersteuningsteam van de beveiligde Client](http://www.topdesk.com/us/support) ophalen van deze waarden. 

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-topdesk-secure-tutorial/tutorial_topdesk-secure_certificate.png) 

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-topdesk-secure-tutorial/tutorial_general_400.png)

6. Op de **TOPdesk - configuratie Secure** sectie, klikt u op **TOPdesk configureren - beveiligde** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![TOPdesk - veilige configuratie](./media/active-directory-saas-topdesk-secure-tutorial/tutorial_topdesk-secure_configure.png)
    
7. Meld u aan bij uw **TOPdesk - beveiligde** bedrijf site als beheerder.

8. In de **TOPdesk** menu, klikt u op **instellingen**.

    ![Instellingen](./media/active-directory-saas-topdesk-secure-tutorial/ic790598.png "instellingen")

9. Klik op **aanmeldingsinstellingen**.

    ![Instellingen voor aanmelding](./media/active-directory-saas-topdesk-secure-tutorial/ic790599.png "aanmeldingsinstellingen")

10. Vouw de **aanmeldingsinstellingen** menu en klik vervolgens op **algemene**.

    ![Algemene](./media/active-directory-saas-topdesk-secure-tutorial/ic790600.png "algemeen")

11. In de **Secure** sectie van de **SAML aanmelding** configuratie sectie, voert u de volgende stappen uit:

    ![Instellingen voor technische](./media/active-directory-saas-topdesk-secure-tutorial/ic790855.png "technische instellingen")
   
    a. Klik op **downloaden** downloaden van het metagegevensbestand van de openbare en lokaal opslaan op uw computer.
   
    b. Open het metagegevensbestand en Ga naar de **AssertionConsumerService** knooppunt.
    
    ![Verklaring Consumer Service](./media/active-directory-saas-topdesk-secure-tutorial/ic790856.png "Assertion Consumer-Service")
   
    c. Kopieer de **AssertionConsumerService** waarde, plakt u deze waarde in het tekstvak de URL van antwoord in **TOPdesk - domein beveiligen en URL's** sectie.

12. Voer de volgende stappen uit voor het maken van een certificaatbestand:
    
    ![Certificaat](./media/active-directory-saas-topdesk-secure-tutorial/ic790606.png "certificaat")
    
    a. Open het metagegevensbestand gedownloade vanuit Azure-portal.

    b. Vouw de **RoleDescriptor** knooppunt met een **xsi: type** van **ingevoerd: ApplicationServiceType**.

    c. Kopieer de waarde van de **X509Certificate** knooppunt.

    d. Opslaan van de gekopieerde **X509Certificate** waarde lokaal op uw computer in een bestand.

13. In de **openbare** sectie, klikt u op **toevoegen**.
    
    ![Voeg](./media/active-directory-saas-topdesk-secure-tutorial/ic790607.png "toevoegen")

14. Op de **SAML-configuratie-assistent** dialoogvenster pagina, voert u de volgende stappen uit:
    
    ![SAML-configuratie-assistent](./media/active-directory-saas-topdesk-secure-tutorial/ic790608.png "assistent voor SAML-configuratie")
    
    a. Voor het uploaden van uw gedownloade metagegevensbestand vanuit Azure-portal onder **Federatiemetagegevens**, klikt u op **Bladeren**.

    b. Voor het uploaden van het certificaatbestand onder **certificaat (RSA)**, klikt u op **Bladeren**.

    c. Het logobestand dat u het ondersteuningsteam TOPdesk onder gekregen te uploaden **Logo pictogram**, klikt u op **Bladeren**.

    d. In de **naam gebruikerskenmerk** textbox type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    e. In de **weergavenaam** textbox, typ een naam voor uw configuratie.

    f. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-topdesk-secure-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-topdesk-secure-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-topdesk-secure-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-topdesk-secure-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-topdesk---secure-test-user"></a>Een TOPdesk - beveiligde testgebruiker maken

Om in te schakelen van Azure AD-gebruikers aan te melden bij TOPdesk - wilt beveiligen, ze moeten worden ingericht in TOPdesk - veilig.  
In het geval van TOPdesk - veilige, inrichting is een handmatige taak.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren voor gebruikers inrichten, moet u de volgende stappen uitvoeren:
1. Meld u aan bij uw **TOPdesk - beveiligde** bedrijf site als administrator.
2. Klik in het menu bovenaan op **TOPdesk \> nieuw \> ondersteuningsbestanden \> Operator**.
   
    ![Operator](./media/active-directory-saas-topdesk-secure-tutorial/ic790610.png "Operator")

3. Op de **Operator New** dialoogvenster de volgende stappen uitvoeren:
   
    ![Nieuwe Operator](./media/active-directory-saas-topdesk-secure-tutorial/ic790611.png "nieuwe Operator")
   
    a. Klik op de **algemene** tabblad.
   
    b. In de **achternaam** textbox type achternaam van de gebruiker, zoals **Simon**.
   
    c. Selecteer een **Site** voor het account in de **locatie** sectie.
   
    d. In de **aanmeldingsnaam** textbox van de **TOPdesk aanmelding** sectie, typt u een aanmeldingsnaam voor de gebruiker.
   
    e. Klik op **Opslaan**.

> [!NOTE]
> U kunt andere TOPdesk - hulpmiddelen voor het maken van account beveiligde gebruiker of geleverd door TOPdesk - veilig voor het inrichten van accounts voor gebruikers met AAD-API's gebruiken.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan TOPdesk - beveiligde.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen TOPdesk - beveiligen, voert u de volgende stappen uit:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **TOPdesk - beveiligde**.

    ![De TOPdesk - beveiligde verbinding in de lijst met toepassingen](./media/active-directory-saas-topdesk-secure-tutorial/tutorial_topdesk-secure_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Wanneer u klikt op de TOPdesk - beveiligde tegel in het deelvenster toegang u moet ophalen automatisch aangemeld bij uw TOPdesk - beveiligde toepassing.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-topdesk-secure-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-topdesk-secure-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-topdesk-secure-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-topdesk-secure-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-topdesk-secure-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-topdesk-secure-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-topdesk-secure-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-topdesk-secure-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-topdesk-secure-tutorial/tutorial_general_203.png

