---
title: 'Zelfstudie: Azure Active Directory-integratie met ArcGIS Online | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding van Azure Active Directory en ArcGIS Online.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a9e132a4-29e7-48bf-beb9-4148e617c8a9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: jeedes
ms.openlocfilehash: df72270ca6443b456c079b22425f1660aa522389
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-arcgis-online"></a>Zelfstudie: Azure Active Directory-integratie met ArcGIS Online

In deze zelfstudie leert u hoe ArcGIS Online integreren met Azure Active Directory (Azure AD).

Online ArcGIS integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot ArcGIS Online heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij ArcGIS Online (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

<!--## Overview

To enable single sign-on with ArcGIS Online, it must be configured to use Azure Active Directory as an identity provider. This guide provides information and tips on how to perform this configuration in ArcGIS Online.

>[!Note]: 
>This embedded guide is brand new in the new Azure portal, and we’d love to hear your thoughts. Use the Feedback ? button at the top of the portal to provide feedback. The older guide for using the [Azure classic portal](https://manage.windowsazure.com) to configure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met ArcGIS Online, moet u de volgende items:

- Een Azure AD-abonnement
- Een ArcGIS Online eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toevoegen van Online ArcGIS uit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-arcgis-online-from-the-gallery"></a>Toevoegen van Online ArcGIS uit de galerie
Voor het configureren van de integratie van ArcGIS Online in Azure AD, moet u ArcGIS Online uit de galerie toevoegt aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen in de galerie ArcGIS Online, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Klik op **nieuwe toepassing** knop boven aan het dialoogvenster nieuwe toepassing toevoegen.

    ![Toepassingen][3]

4. Typ in het zoekvak **ArcGIS Online**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_search.png)

5. Selecteer in het deelvenster resultaten **ArcGIS Online**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met ArcGIS Online op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in ArcGIS Online is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker ArcGIS online worden gemaakt.

Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** ArcGIS Online.

Om te configureren en testen van Azure AD eenmalige aanmelding met ArcGIS Online, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een Online ArcGIS testgebruiker](#creating-an-arcgis-online-test-user)**  - bevatten een equivalent van Britta Simon ArcGIS Online die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing ArcGIS Online configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met ArcGIS Online, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **ArcGIS Online** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_samlbase.png)

3. Op de **ArcGIS Online domein en de URL's** sectie, voert u de volgende stap:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_url.png)

    In de **aanmeldings-URL** textbox, typ de waarde met het volgende patroon volgen:`https://<company>.maps.arcgis.com`

    > [!NOTE] 
    > Deze waarde is niet de werkelijke. Deze waarde bijwerken met de werkelijke URL voor eenmalige aanmelding. Neem contact op met [ArcGIS Online Client ondersteuningsteam](http://support.esri.com/) deze waarde op te halen. 

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het XML-bestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-arcgis-tutorial/tutorial_general_400.png)

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


### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-arcgis-tutorial/create_aaduser_01.png) 

2. Ga naar **gebruikers en groepen** en klik op **alle gebruikers** om de lijst met gebruikers weer te geven.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-arcgis-tutorial/create_aaduser_02.png) 

3. Klik aan de bovenkant van het dialoogvenster **toevoegen** openen de **gebruiker** dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-arcgis-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-arcgis-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van Britta Simon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-an-arcgis-online-test-user"></a>Maken van een Online ArcGIS testgebruiker

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

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding toegang verleent tot ArcGIS Online.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen ArcGIS Online, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **ArcGIS Online**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

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

