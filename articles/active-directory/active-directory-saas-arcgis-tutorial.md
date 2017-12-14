---
title: 'Zelfstudie: Azure Active Directory-integratie met ArcGIS Online | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding van Azure Active Directory en ArcGIS Online.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a9e132a4-29e7-48bf-beb9-4148e617c8a9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/13/2017
ms.author: jeedes
ms.openlocfilehash: b09dd977cbf5c4273667167217e86bb79ac2a9d8
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="tutorial-azure-active-directory-integration-with-arcgis-online"></a>Zelfstudie: Azure Active Directory-integratie met ArcGIS Online

In deze zelfstudie leert u hoe ArcGIS Online integreren met Azure Active Directory (Azure AD).

Online ArcGIS integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot ArcGIS Online heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij ArcGIS Online (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met ArcGIS Online, moet u de volgende items:

- Een Azure AD-abonnement
- Een ArcGIS Online eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toevoegen van Online ArcGIS uit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-arcgis-online-from-the-gallery"></a>Toevoegen van Online ArcGIS uit de galerie
Voor het configureren van de integratie van ArcGIS Online in Azure AD, moet u ArcGIS Online uit de galerie toevoegt aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen in de galerie ArcGIS Online, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **ArcGIS Online**, selecteer **ArcGIS Online** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![ArcGIS Online in de lijst met resultaten](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met ArcGIS Online op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in ArcGIS Online is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker ArcGIS online worden gemaakt.

In Online ArcGIS, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met ArcGIS Online, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een ArcGIS Online testgebruiker](#create-a-arcgis-online-test-user)**  - bevatten een equivalent van Britta Simon ArcGIS Online die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing ArcGIS Online configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met ArcGIS Online, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **ArcGIS Online** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_samlbase.png)

3. Op de **ArcGIS Online domein en de URL's** sectie, voert u de volgende stappen uit:

    ![URL's en ArcGIS Online domein eenmalige aanmelding informatie](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<companyname>.maps.arcgis.com`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:`<companyname>.maps.arcgis.com`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met het werkelijke aanmeldings-URL en de id. Neem contact op met [ArcGIS Online Client ondersteuningsteam](http://support.esri.com/en/) ophalen van deze waarden. 
 


4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_certificate.png) 

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-arcgis-tutorial/tutorial_general_400.png)

6. In een ander browservenster, meld u bij uw bedrijf ArcGIS site als beheerder.

7. Klik op **bewerken van instellingen**.

    ![Instellingen bewerken](./media/active-directory-saas-arcgis-tutorial/ic784742.png "instellingen bewerken")

8. Klik op **beveiliging**.

    ![Beveiliging](./media/active-directory-saas-arcgis-tutorial/ic784743.png "beveiliging")

9. Onder **Enterprise aanmeldingen**, klikt u op **IDENTITEITSPROVIDER ingesteld**.

    ![Enterprise-aanmeldingen](./media/active-directory-saas-arcgis-tutorial/ic784744.png "Enterprise-aanmeldingen")

10. Op de **identiteitsprovider ingesteld** configuratie pagina, voert u de volgende stappen uit:
   
    ![Instellen van de identiteitsprovider](./media/active-directory-saas-arcgis-tutorial/ic784745.png "identiteitsprovider instellen")
   
    a. In de **naam** textbox, typ de naam van uw organisatie.

    b. Voor **metagegevens voor de Enterprise-id-Provider worden opgegeven met behulp van**, selecteer **een bestand**.

    c. Als u wilt uw van het gedownloade metagegevensbestand uploadt, klikt u op **bestand kiezen**.

    d. Klik op **SET IDENTITEITSPROVIDER**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-arcgis-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-arcgis-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-arcgis-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-arcgis-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-arcgis-online-test-user"></a>Een ArcGIS Online testgebruiker maken

Om in te schakelen gebruikers van Azure AD aan te melden bij ArcGIS Online, moeten ze worden ingericht in ArcGIS Online.  
In het geval van Online ArcGIS is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **ArcGIS** tenant.

2. Klik op **leden UITNODIGEN**.
   
    ![Leden uitnodigen](./media/active-directory-saas-arcgis-tutorial/ic784747.png "leden uit te nodigen")

3. Selecteer **automatisch leden toevoegen zonder een e-mailbericht te verzenden**, en klik vervolgens op **volgende**.
   
    ![Automatisch toevoegen van leden](./media/active-directory-saas-arcgis-tutorial/ic784748.png "automatisch toevoegen van leden")

4. Op de **leden** dialoogvenster pagina, voert u de volgende stappen uit:
   
     ![Toevoegen en controleren](./media/active-directory-saas-arcgis-tutorial/ic784749.png "toevoegen en controleren")
    
     a. Voer de **e**, **voornaam**, en **achternaam** van een geldige AAD-account dat u inrichten wilt.
  
     b. Klik op **toevoegen en bekijk**.
5. Controleer de gegevens die u hebt ingevoerd, en klik vervolgens op **leden toevoegen**.
   
    ![Lid toevoegen](./media/active-directory-saas-arcgis-tutorial/ic784750.png "lid toevoegen")
        
    > [!NOTE]
    > De accounthouder Azure Active Directory wordt een e-mailbericht ontvangen en Ga als volgt een koppeling om hun account te bevestigen voordat deze geactiveerd wordt.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding toegang verleent tot ArcGIS Online.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen ArcGIS Online, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **ArcGIS Online**.

    ![De ArcGIS Online koppeling in de lijst met toepassingen](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel ArcGIS Online in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw ArcGIS Online-toepassing.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_203.png

