---
title: 'Zelfstudie: Azure Active Directory-integratie met Druva | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Druva.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ab92b600-1fea-4905-b1c7-ef8e4d8c495c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: b23e73c47b9a00893e036b67826e4b7ead819a1d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-druva"></a>Zelfstudie: Azure Active Directory-integratie met Druva

In deze zelfstudie leert u hoe Druva integreren met Azure Active Directory (Azure AD).

Druva integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Druva heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Druva (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Druva, moet u de volgende items:

- Een Azure AD-abonnement
- Een Druva eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Druva uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-druva-from-the-gallery"></a>Druva uit de galerie toevoegen
Voor het configureren van de integratie van Druva in Azure AD, moet u Druva uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Druva uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Druva**, selecteer **Druva** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Druva in de lijst met resultaten](./media/active-directory-saas-druva-tutorial/tutorial_druva_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met Druva op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Druva is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Druva tot stand worden gebracht.

Wijs in Druva, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Druva, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Druva](#create-a-druva-test-user)**  - Druva die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Druva configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Druva, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Druva** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-druva-tutorial/tutorial_druva_samlbase.png)

3. Op de **Druva domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-druva-tutorial/tutorial_druva_url.png)

    In de **aanmeldings-URL** textbox, typ de URL:`https://cloud.druva.com/home`

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-druva-tutorial/tutorial_druva_certificate.png) 

5. Uw toepassing Druva verwacht de SAML-asserties in een specifieke indeling waarvoor u het toevoegen van aangepast kenmerktoewijzingen aan uw **SAML-Token kenmerken** configuratie. 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-druva-tutorial/tutorial_druva_attribute.png)

6. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de voorgaande afbeelding en de volgende stappen uitvoeren:

    | Naam van kenmerk      | De waarde van kenmerk      |
    | ------------------- | -------------------- |
    | synchroon\_auth\_token |Voer de token gegenereerde waarde |
    
    a. Klik op **toevoegen kenmerk** openen de **kenmerk toevoegen** dialoogvenster.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-druva-tutorial/tutorial_attribute_04.png)
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-druva-tutorial/tutorial_attribute_05.png)
    
    b. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.

    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij. De token gegenereerde waarde wordt verderop in de zelfstudie beschreven.
    
    d. Klik op **OK**.    

7. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-druva-tutorial/tutorial_general_400.png)

8. Op de **Druva configuratie** sectie, klikt u op **configureren Druva** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out URL's en SAML Single Sign-On Service** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-druva-tutorial/tutorial_druva_configure.png) 

9. In een ander browservenster, meld u aan bij uw bedrijf Druva site als beheerder.

10. Ga naar **beheren \> instellingen**.

    ![Instellingen](./media/active-directory-saas-druva-tutorial/ic795091.png "instellingen")

11. Voer de volgende stappen uit in het dialoogvenster Instellingen voor eenmalige aanmelding:

    ![Eenmalige aanmelding instellingen](./media/active-directory-saas-druva-tutorial/ic795092.png "eenmalige aanmelding-instellingen")
    
    a. Plakken **SAML Single Sign-On Service-URL** waarde, die u hebt gekopieerd vanuit de Azure-portal in de **ID-Provider aanmeldings-URL** textbox.
    
    b. Plakken **Sign-Out URL** waarde, die u hebt gekopieerd vanuit de Azure-portal in de **ID-Provider afmelding URL** textbox.
    
     c. Open uw base-64 gecodeerde certificaat in Kladblok, Kopieer de inhoud ervan naar het Klembord en plakt u deze naar de **ID-Provider certificaat** tekstvak
     
     d. Openen van de **instellingen** pagina, klikt u op **opslaan**.

12. Op de **instellingen** pagina, klikt u op **SSO-Token genereren**.

    ![Instellingen](./media/active-directory-saas-druva-tutorial/ic795093.png "instellingen")

13. Op de **Single Sign-on-verificatietoken** dialoogvenster de volgende stappen uitvoeren:

    ![SSO-Token](./media/active-directory-saas-druva-tutorial/ic795094.png "SSO-Token")
    
    a. Klik op **kopie**, plakken gekopieerd waarde in de **waarde** textbox in de **kenmerk toevoegen** sectie.
    
    b. Klik op **Sluiten**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-druva-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-druva-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-druva-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-druva-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-druva-test-user"></a>Een testgebruiker Druva maken

Om Azure AD-gebruikers zich aanmelden bij Druva inschakelt, moeten ze worden ingericht in Druva. In het geval van Druva is inrichting een handmatige taak.

**Als u wilt configureren voor gebruikers inrichten, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **Druva** bedrijf site als administrator.

2. Ga naar **beheren \> gebruikers**.
   
   ![Gebruikers beheren](./media/active-directory-saas-druva-tutorial/ic795097.png "gebruikers beheren")

3. Klik op **maken van nieuwe**.
   
   ![Gebruikers beheren](./media/active-directory-saas-druva-tutorial/ic795098.png "gebruikers beheren")

4. Voer de volgende stappen uit in het dialoogvenster Nieuwe gebruiker maken:
   
   ![Maken van nieuwegebruiker](./media/active-directory-saas-druva-tutorial/ic795099.png "nieuwegebruiker maken")
   
   a. In de **e-mailadres** textbox, voer het e-mailadres van de gebruiker zoals  **brittasimon@contoso.com** .
   
   b. In de **naam** textbox, voer de naam van gebruiker zoals **BrittaSimon**.
   
   c. Klik op **gebruiker maken**.

>[!NOTE]
>U kunt andere Druva gebruiker account hulpmiddelen voor het maken of API's die is geleverd door Druva voor het inrichten van Azure AD-gebruikersaccounts.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Druva.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen Druva, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Druva**.

    ![De koppeling Druva in de lijst met toepassingen](./media/active-directory-saas-druva-tutorial/tutorial_druva_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Druva in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Druva.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-druva-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-druva-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-druva-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-druva-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-druva-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-druva-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-druva-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-druva-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-druva-tutorial/tutorial_general_203.png

