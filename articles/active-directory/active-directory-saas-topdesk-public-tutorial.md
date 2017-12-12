---
title: 'Zelfstudie: Azure Active Directory-integratie met TOPdesk - openbare | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en TOPdesk - openbaar.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 0873299f-ce70-457b-addc-e57c5801275f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: 8f88b232df01aa73a7e06b891ef5ff635d03086a
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---public"></a>Zelfstudie: Azure Active Directory-integratie met TOPdesk - openbare

In deze zelfstudie leert u hoe integreren TOPdesk - openbare met Azure Active Directory (Azure AD).

Integratie van TOPdesk - openbare met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot TOPdesk - openbaar heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij TOPdesk - openbare (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Configureren van Azure AD-integratie met TOPdesk - openbaar is, moet u de volgende items:

- Een Azure AD-abonnement
- Een TOPdesk - openbare eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. TOPdesk - openbare uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-topdesk---public-from-the-gallery"></a>TOPdesk - openbare uit de galerie toevoegen
Om de integratie van TOPdesk - openbare met Azure AD te configureren die u wilt toevoegen TOPdesk - openbare uit de galerie aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen, TOPdesk - openbare uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **TOPdesk - openbare**, selecteer **TOPdesk - openbare** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![TOPdesk - publiek in de lijst met resultaten](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met TOPdesk - openbare op basis van een testgebruiker 'Britta Simon' genoemd.

Azure AD moet weten wat de equivalente gebruiker in TOPdesk - openbaar is aan een gebruiker in Azure AD voor eenmalige aanmelding werkt. Met andere woorden, een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in TOPdesk - openbare moet tot stand worden gebracht.

In TOPdesk - openbaar, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Configureren en testen Azure AD eenmalige aanmelding met TOPdesk - openbaar is, moet u de volgende bouwstenen uitvoeren:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een TOPdesk - openbare testgebruiker](#create-a-topdesk---public-test-user)**  - TOPdesk - openbare die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw TOPdesk - openbare toepassing configureren.

**Eenmalige aanmelding Azure AD configureren met TOPdesk - openbaar, voer de volgende stappen uit:**

1. In de Azure-portal op de **TOPdesk - openbare** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_samlbase.png)

3. Op de **TOPdesk - URL's en openbare domein** sectie, voert u de volgende stappen uit:

    ![TOPdesk - URL's en openbare domein eenmalige aanmelding informatie](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<companyname>.topdesk.net`
    
    b. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://<companyname>.topdesk.net/tas/public/login/verify`

    c. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://<companyname>.topdesk.net/tas/public/login/saml`
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke id, antwoord-URL en aanmeldings-URL. Antwoord-URL is explaned verderop in de zelfstudie. Neem contact op met [TOPdesk - ondersteuningsteam openbare Client](https://help.topdesk.com/saas/enterprise/user/) ophalen van deze waarden.  

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_certificate.png) 

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_400.png)
    
6. Op de **TOPdesk - configuratie van openbare** sectie, klikt u op **configureren TOPdesk - openbare** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![TOPdesk - openbare configuratie](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_configure.png) 

7. Meld u aan bij uw **TOPdesk - openbare** bedrijf site als beheerder.

8. In de **TOPdesk** menu, klikt u op **instellingen**.
   
    ![Instellingen](./media/active-directory-saas-topdesk-public-tutorial/ic790598.png "instellingen")

9. Klik op **aanmeldingsinstellingen**.
   
    ![Instellingen voor aanmelding](./media/active-directory-saas-topdesk-public-tutorial/ic790599.png "aanmeldingsinstellingen")

10. Vouw de **aanmeldingsinstellingen** menu en klik vervolgens op **algemene**.
   
    ![Algemene](./media/active-directory-saas-topdesk-public-tutorial/ic790600.png "algemeen")

11. In de **openbare** sectie van de **SAML aanmelding** configuratie sectie, voert u de volgende stappen uit:
   
    ![Instellingen voor technische](./media/active-directory-saas-topdesk-public-tutorial/ic790601.png "technische instellingen")
   
    a. Klik op **downloaden** downloaden van het metagegevensbestand van de openbare en lokaal opslaan op uw computer.
   
    b. Open het metagegevensbestand van de gedownloade en ga vervolgens naar de **AssertionConsumerService** knooppunt.

    ![AssertionConsumerService](./media/active-directory-saas-topdesk-public-tutorial/ic790619.png "AssertionConsumerService")
   
    c. Kopieer de **AssertionConsumerService** waarde, plakt u deze waarde in de **antwoord-URL** textbox in **TOPdesk - URL's en openbare domein** sectie.      
   
12. Voer de volgende stappen uit voor het maken van een certificaatbestand:
    
    ![Certificaat](./media/active-directory-saas-topdesk-public-tutorial/ic790606.png "certificaat")
    
    a. Open het metagegevensbestand gedownloade vanuit Azure-portal.
    
    b. Vouw de **RoleDescriptor** knooppunt met een **xsi: type** van **ingevoerd: ApplicationServiceType**.
    
    c. Kopieer de waarde van de **X509Certificate** knooppunt.
    
    d. Opslaan van de gekopieerde **X509Certificate** waarde lokaal op uw computer in een bestand.

13. In de **openbare** sectie, klikt u op **toevoegen**.
    
    ![SAML-aanmelding](./media/active-directory-saas-topdesk-public-tutorial/ic790625.png "SAML-aanmelding")

14. Op de **SAML-configuratie-assistent** dialoogvenster pagina, voert u de volgende stappen uit:
    
    ![SAML-configuratie-assistent](./media/active-directory-saas-topdesk-public-tutorial/ic790608.png "assistent voor SAML-configuratie")
    
    a. Voor het uploaden van uw gedownloade metagegevensbestand vanuit Azure-portal onder **Federatiemetagegevens**, klikt u op **Bladeren**.

    b. Voor het uploaden van het certificaatbestand onder **certificaat (RSA)**, klikt u op **Bladeren**.

    c. Het logobestand dat u het ondersteuningsteam TOPdesk onder gekregen te uploaden **Logo pictogram**, klikt u op **Bladeren**.

    d. In de **naam gebruikerskenmerk** textbox type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    e. In de **weergavenaam** textbox, typ een naam voor uw configuratie.

    f. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-topdesk---public-test-user"></a>Een TOPdesk - openbare testgebruiker maken

Om in te schakelen van Azure AD-gebruikers aan te melden bij TOPdesk - openbaar, ze in TOPdesk - openbare moeten worden ingericht.  
In het geval van TOPdesk - openbare inrichting is een handmatige taak.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren voor gebruikers inrichten, moet u de volgende stappen uitvoeren:
1. Meld u aan bij uw **TOPdesk - openbare** bedrijf site als administrator.

2. Klik in het menu bovenaan op **TOPdesk \> nieuw \> ondersteuningsbestanden \> persoon**.
   
    ![Persoon](./media/active-directory-saas-topdesk-public-tutorial/ic790628.png "persoon")

3. In het dialoogvenster nieuwe persoon, moet u de volgende stappen uitvoeren:
   
    ![Nieuwe persoon](./media/active-directory-saas-topdesk-public-tutorial/ic790629.png "nieuwe persoon")
   
    a. Klik op het tabblad Algemeen.

    b. In de **achternaam** textbox, typ de achternaam van de gebruiker zoals Simon
 
    c. Selecteer een **Site** voor het account.
 
    d. Klik op **Opslaan**.

> [!NOTE]
> U kunt andere TOPdesk - hulpmiddelen voor het maken van account openbaar gebruiker of API's die is geleverd door TOPdesk - openbare voor het inrichten van Azure AD-gebruikersaccounts.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan TOPdesk - openbare.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon toewijzen aan TOPdesk - openbaar, voer de volgende stappen uit:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **TOPdesk - openbare**.

    ![De TOPdesk - openbare koppeling in de lijst met toepassingen](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Wanneer u klikt op de TOPdesk - openbare-tegel in het toegangsvenster, u moet ophalen automatisch aangemeld bij uw TOPdesk - openbare toepassing.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_203.png

