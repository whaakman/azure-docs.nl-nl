---
title: 'Zelfstudie: Azure Active Directory-integratie met Benefitsolver | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Benefitsolver.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 333394c1-b5a7-489c-8f7b-d1a5b4e782ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2017
ms.author: jeedes
ms.openlocfilehash: d23f1c4d6613d525695ed222e0d84736b9007833
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="tutorial-azure-active-directory-integration-with-benefitsolver"></a>Zelfstudie: Azure Active Directory-integratie met Benefitsolver

In deze zelfstudie leert u hoe Benefitsolver integreren met Azure Active Directory (Azure AD).

Benefitsolver integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Benefitsolver heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Benefitsolver (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Benefitsolver, moet u de volgende items:

- Een Azure AD-abonnement
- Een Benefitsolver eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Benefitsolver uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-benefitsolver-from-the-gallery"></a>Benefitsolver uit de galerie toevoegen
Voor het configureren van de integratie van Benefitsolver in Azure AD, moet u Benefitsolver uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Benefitsolver uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Benefitsolver**, selecteer **Benefitsolver** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Benefitsolver in de lijst met resultaten](./media/active-directory-saas-benefitsolver-tutorial/tutorial_benefitsolver_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met Benefitsolver op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Benefitsolver is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Benefitsolver tot stand worden gebracht.

Wijs in Benefitsolver, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Benefitsolver, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Benefitsolver](#create-a-benefitsolver-test-user)**  - Benefitsolver die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Benefitsolver configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Benefitsolver, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Benefitsolver** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-benefitsolver-tutorial/tutorial_benefitsolver_samlbase.png)

3. Op de **Benefitsolver domein en de URL's** sectie, voert u de volgende stappen uit:

    ![URL's en Benefitsolver domein eenmalige aanmelding informatie](./media/active-directory-saas-benefitsolver-tutorial/tutorial_benefitsolver_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`http://<companyname>.benefitsolver.com`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://<companyname>.benefitsolver.com/saml20`

    c. In de **antwoord-URL** textbox, typ de URL:`https://www.benefitsolver.com/benefits/BenefitSolverView?page_name=single_signon_saml`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke aanmeldings-URL, -id en antwoord-URL. Neem contact op met [Benefitsolver Client ondersteuningsteam](https://www.businessolver.com/contact) ophalen van deze waarden.

4. Uw toepassing Benefitsolver verwacht de SAML-asserties in een specifieke indeling waarvoor u het toevoegen van aangepast kenmerktoewijzingen aan uw **saml-token kenmerken** configuratie.

    ![Benefitsolver kenmerk sectie](./media/active-directory-saas-benefitsolver-tutorial/tutorial_attribute.png)

5. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding en de volgende stappen uitvoeren:
    
    | Naam van kenmerk| De waarde van kenmerk|
    |---------------|----------------|
    | ClientID | U moet deze waarde van uw [Benefitsolver Client ondersteuningsteam](https://www.businessolver.com/contact).|
    | ClientKey | U moet deze waarde van uw [Benefitsolver Client ondersteuningsteam](https://www.businessolver.com/contact).|
    | LogoutURL | U moet deze waarde van uw [Benefitsolver Client ondersteuningsteam](https://www.businessolver.com/contact).|
    | Werknemer-id | U moet deze waarde van uw [Benefitsolver Client ondersteuningsteam](https://www.businessolver.com/contact).|

    a. Klik op toevoegen kenmerk om het dialoogvenster kenmerk toevoegen te openen.

    ![Benefitsolver kenmerk sectie](./media/active-directory-saas-benefitsolver-tutorial/tutorial_attribute_04.png)
    
    ![Benefitsolver kenmerk sectie](./media/active-directory-saas-benefitsolver-tutorial/tutorial_attribute_05.png)

    b. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.
    
    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    d. Klik op **OK**.

6. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-benefitsolver-tutorial/tutorial_benefitsolver_certificate.png) 

7. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-benefitsolver-tutorial/tutorial_general_400.png)

8. Eenmalige aanmelding configureren op **Benefitsolver** zijde, moet u de gedownloade verzenden **Metadata XML** naar [Benefitsolver ondersteuningsteam](https://www.businessolver.com/contact).

    > [!NOTE]
    > Het ondersteuningsteam Benefitsolver heeft te maken van de werkelijke SSO-configuratie. U ontvangt een melding wanneer SSO is ingeschakeld voor uw abonnement.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-benefitsolver-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-benefitsolver-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-benefitsolver-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-benefitsolver-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-benefitsolver-test-user"></a>Een testgebruiker Benefitsolver maken

Om in te schakelen gebruikers van Azure AD aan te melden bij Benefitsolver, moeten ze worden ingericht in Benefitsolver. In het geval van Benefitsolver is werknemersgegevens in uw toepassing ingevuld met behulp van een bestand telling van het systeem HRIS (doorgaans elke nacht).

> [!NOTE]
> U kunt andere Benefitsolver gebruiker account hulpmiddelen voor het maken of API's die is geleverd door Benefitsolver aan inrichten AAD-gebruikersaccounts.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Benefitsolver.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen Benefitsolver, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Benefitsolver**.

    ![De koppeling Benefitsolver in de lijst met toepassingen](./media/active-directory-saas-benefitsolver-tutorial/tutorial_benefitsolver_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Benefitsolver in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Benefitsolver.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-benefitsolver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-benefitsolver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-benefitsolver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-benefitsolver-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-benefitsolver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-benefitsolver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-benefitsolver-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-benefitsolver-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-benefitsolver-tutorial/tutorial_general_203.png

