---
title: 'Zelfstudie: Azure Active Directory-integratie met Rally Software | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Rally Software.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: ba25fade-e152-42dd-8377-a30bbc48c3ed
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: jeedes
ms.openlocfilehash: 9e3b5ad4487ff1309923a1b0ffac9589084e715b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-rally-software"></a>Zelfstudie: Azure Active Directory-integratie met Rally Software

In deze zelfstudie leert u hoe Software Rally integreren met Azure Active Directory (Azure AD).

Software Rally integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Rally Software heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Rally Software (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Rally Software, moet u de volgende items:

- Een Azure AD-abonnement
- Een Software Rally eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Rally Software uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-rally-software-from-the-gallery"></a>Rally Software uit de galerie toevoegen
Voor het configureren van de integratie van Software Rally in Azure AD, moet u Rally Software uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Rally Software uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Rally Software**, selecteer **Rally Software** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Rally Software in de lijst met resultaten](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met Rally Software op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Software Rally is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Software Rally tot stand worden gebracht.

Wijs in Rally Software, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Rally Software, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Rally Software](#create-a-rally-software-test-user)**  - Rally Software die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Rally configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Rally Software, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Rally Software** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_samlbase.png)

3. Op de **Rally Software domein en URL's** sectie, voert u de volgende stappen uit:

    ![Software-domein en de URL's eenmalige aanmelding informatie Rally](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<tenant-name>.rally.com`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://<tenant-name>.rally.com`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met het werkelijke aanmeldings-URL en de id. Neem contact op met [Rally Software Client ondersteuningsteam](https://help.rallydev.com/) ophalen van deze waarden. 
 


4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_certificate.png) 

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-rally-software-tutorial/tutorial_general_400.png)

6. Op de **softwareconfiguratie Rally** sectie, klikt u op **Rally Software configureren** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL en SAML entiteit-ID** van de **Naslaggids punt.**

    ![De softwareconfiguratie Rally](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_configure.png) 

7. Meld u aan bij uw **Rally Software** tenant.

8. Klik in de werkbalk bovenaan op **Setup**, en selecteer vervolgens **abonnement**.
   
    ![Abonnement](./media/active-directory-saas-rally-software-tutorial/ic769531.png "abonnement")

9. Klik op de **actie** knop. Selecteer **abonnement bewerken** op de rechtsboven in de werkbalk.

10. Op de **abonnement** dialoogvenster pagina de volgende stappen uit en klik vervolgens op **opslaan en sluiten**:
   
    ![Verificatie](./media/active-directory-saas-rally-software-tutorial/ic769542.png "verificatie")
   
    a. Selecteer **Rally of eenmalige aanmelding verificatie** uit de vervolgkeuzelijst verificatie.

    b. In de **identiteit provider URL** textbox, plak de waarde van **SAML entiteit-ID**, die u hebt gekopieerd vanuit Azure-portal. 

    c. In de **eenmalige aanmelding, afmelding** textbox, plak de waarde van **Sign-Out URL**, die u hebt gekopieerd vanuit Azure-portal.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-rally-software-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-rally-software-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-rally-software-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-rally-software-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-rally-software-test-user"></a>Een testgebruiker Rally Software maken

Azure AD-gebruikers moeten kunnen aanmelden, als ze worden ingericht voor de Software Rally-toepassing met behulp van de namen van de Azure Active Directory-gebruiker.

**Als u wilt configureren voor gebruikers inrichten, moet u de volgende stappen uitvoeren:**

1. Aanmelden bij uw tenant Rally Software.

2. Ga naar **Setup \> gebruikers**, en klik vervolgens op **+ nieuw toevoegen**.
   
    ![Gebruikers](./media/active-directory-saas-rally-software-tutorial/ic781039.png "gebruikers")

3. Typ de naam in het tekstvak voor de nieuwe gebruiker en klik vervolgens op **toevoegen met Details**.

4. In de **gebruiker maken** sectie, voert u de volgende stappen uit:
   
    ![Gebruiker maken](./media/active-directory-saas-rally-software-tutorial/ic781040.png "gebruiker maken")

    a. In de **gebruikersnaam** textbox, typ de naam van gebruiker, zoals **Brittsimon**.
   
    b. In **e-mailadres** textbox, voer het e-mailadres van de gebruiker zoals  **brittasimon@contoso.com** .

    c. In **voornaam** tekst en voer de voornaam van de gebruiker zoals **Britta**.

    d. In **achternaam** tekst en voer de achternaam van de gebruiker zoals **Simon**.

    e. Klik op **opslaan en sluiten**.

   >[!NOTE]
   >U kunt geen andere hulpprogramma's voor Software Rally gebruiker-account maken of API's die worden geleverd door Software Rally gebruiken voor het inrichten van Azure AD-gebruikersaccounts.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot Software Rally.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen Rally Software, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Rally Software**.

    ![De koppeling Rally Software in de lijst met toepassingen](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Als u op de tegel Rally Software in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Rally Software.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_203.png

