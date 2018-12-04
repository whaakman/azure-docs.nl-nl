---
title: 'Zelfstudie: Azure Active Directory-integratie met ArcGIS Online | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en ArcGIS Online.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a9e132a4-29e7-48bf-beb9-4148e617c8a9
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2018
ms.author: jeedes
ms.openlocfilehash: 3284202ffaa6767a8dd4a6a5050dbdc928075237
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52846111"
---
# <a name="tutorial-azure-active-directory-integration-with-arcgis-online"></a>Zelfstudie: Azure Active Directory-integratie met ArcGIS Online

In deze zelfstudie leert u hoe u ArcGIS Online integreren met Azure Active Directory (Azure AD).

ArcGIS Online integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot ArcGIS Online heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij ArcGIS Online (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met ArcGIS Online, moet u de volgende items:

- Een Azure AD-abonnement
- Een ArcGIS Online eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving.
Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. ArcGIS Online toevoegen vanuit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-arcgis-online-from-the-gallery"></a>ArcGIS Online toevoegen vanuit de galerie

Voor het configureren van de integratie van ArcGIS Online in Azure AD, moet u ArcGIS Online toevoegen vanuit de galerie aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen vanuit de galerie ArcGIS Online, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![image](./media/arcgis-tutorial/selectazuread.png)

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![image](./media/arcgis-tutorial/a_select_app.png)
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![image](./media/arcgis-tutorial/a_new_app.png)

4. Typ in het zoekvak **ArcGIS Online**, selecteer **ArcGIS Online** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![image](./media/arcgis-tutorial/a_add_app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met ArcGIS Online op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in ArcGIS Online is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in ArcGIS Online tot stand worden gebracht.

ArcGIS online, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met ArcGIS Online, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker ArcGIS Online](#create-an-arcgis-online-test-user)**  : als u wilt een equivalent van Britta Simon in ArcGIS Online die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing ArcGIS Online.

**Voor het configureren van Azure AD eenmalige aanmelding met ArcGIS Online, moet u de volgende stappen uitvoeren:**

1. In de [Azure-portal](https://portal.azure.com/)op de **ArcGIS Online** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![image](./media/arcgis-tutorial/b1_b2_select_sso.png)

2. Klik op **modus voor één wijziging aanmelding** boven op het scherm selecteren de **SAML** modus.

      ![image](./media/arcgis-tutorial/b1_b2_saml_ssso.png)

3. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster, klikt u op **Selecteer** voor **SAML** modus voor eenmalige aanmelding inschakelen.

    ![image](./media/arcgis-tutorial/b1_b2_saml_sso.png)

4. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** te openen **SAML-basisconfiguratie** dialoogvenster.

    ![image](./media/arcgis-tutorial/b1-domains_and_urlsedit.png)

5. Op de **SAML-basisconfiguratie** sectie, voert u de volgende stappen uit:

    a. In de **aanmeldings-URL** tekstvak typt u een URL met behulp van het volgende patroon: `https://<companyname>.maps.arcgis.com`.

    b. In de **id** tekstvak typt u een URL met behulp van het volgende patroon: `<companyname>.maps.arcgis.com`.

    ![image](./media/arcgis-tutorial/b1-domains_and_urls.png)

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL en -id. Neem contact op met [ArcGIS Online Client ondersteuningsteam](https://support.esri.com/en/) om deze waarden te verkrijgen.

6. Op de **SAML-handtekeningcertificaat** sectie, klikt u op op **downloaden** downloaden **federatieve metagegevens-XML** en sla het xml-bestand op uw computer.

    ![image](./media/arcgis-tutorial/federationxml.png)

7. Voor het automatiseren van de configuratie in **ArcGIS Online**, moet u installeren **mijn Apps beveiligde aanmelding browserextensie** door te klikken op **de extensie installeren**.

    ![image](./media/arcgis-tutorial/install_extension.png)

8. Na het toevoegen van uitbreiding naar de browser, klikt u op **setup ArcGIS Online** wordt u doorgeleid naar de ArcGIS Online toepassing. Van daaruit, geef de referenties van de beheerder zich aanmelden bij ArcGIS Online. De browserextensie wordt automatisch configureren van de toepassing voor u en stappen 9-13 automatiseren.

9. Als u handmatig ArcGIS Online instellen wilt, opent u een nieuw browservenster en meld u aan bij uw site van het bedrijf ArcGIS als beheerder en voer de volgende stappen uit:

10. Klik op **instellingen bewerken**.

    ![Instellingen bewerken](./media/arcgis-tutorial/ic784742.png "instellingen bewerken")

11. Klik op **Security**.

    ![Beveiliging](./media/arcgis-tutorial/ic784743.png "beveiliging")

12. Onder **Enterprise aanmeldingen**, klikt u op **id-PROVIDER instellen**.

    ![Enterprise-aanmeldingen](./media/arcgis-tutorial/ic784744.png "Enterprise-aanmeldingen")

13. Op de **id-Provider instellen** configuratie pagina, voert u de volgende stappen uit:

    ![ID-Provider instellen](./media/arcgis-tutorial/ic784745.png "id-Provider instellen")

    a. In de **naam** tekstvak typt u de naam van uw organisatie.

    b. Voor **metagegevens voor de Enterprise-id-Provider worden opgegeven met behulp van**, selecteer **een bestand**.

    c. Als u wilt uw gedownloade metagegevensbestand uploaden, klikt u op **bestand kiezen**.

    d. Klik op **SET id-PROVIDER**.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD 

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

1. Selecteer in de Azure portal, in het linkerdeelvenster **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.

    ![image](./media/arcgis-tutorial/d_users_and_groups.png)

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![image](./media/arcgis-tutorial/d_adduser.png)

3. In de eigenschappen van de gebruiker de volgende stappen uitvoeren.

    ![image](./media/arcgis-tutorial/d_userproperties.png)

    a. In de **naam** veld **BrittaSimon**.
  
    b. In de **gebruikersnaam** veldtype **brittasimon@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.

### <a name="create-an-arcgis-online-test-user"></a>Maak een testgebruiker ArcGIS Online

Als u wilt inschakelen in Azure AD-gebruikers zich aanmelden bij ArcGIS Online, moeten ze worden ingericht voor ArcGIS Online.  
In het geval van ArcGIS Online is inrichten een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **ArcGIS** tenant.

2. Klik op **leden UITNODIGEN**.
   
    ![Leden uitnodigen](./media/arcgis-tutorial/ic784747.png "leden uitnodigen")

3. Selecteer **leden automatisch toevoegen zonder een e-mailbericht verzenden**, en klik vervolgens op **volgende**.
   
    ![Automatisch toevoegen van leden](./media/arcgis-tutorial/ic784748.png "leden automatisch toevoegen")

4. Op de **leden** dialoogvenster pagina, voert u de volgende stappen uit:
   
     ![Toevoegen en Controleer](./media/arcgis-tutorial/ic784749.png "toevoegen en controleren")
    
     a. Voer de **e**, **voornaam**, en **achternaam** van een geldige AAD-account dat u inrichten wilt.
  
     b. Klik op **toevoegen en bekijk**.
5. Controleer de gegevens die u hebt ingevoerd, en klik vervolgens op **leden toevoegen**.
   
    ![Lid toevoegen](./media/arcgis-tutorial/ic784750.png "lid toevoegen")
        
    > [!NOTE]
    > De houder van Azure Active Directory-account ontvangt een e-mailbericht en gaat u als volgt een koppeling om te bevestigen van hun account voordat deze geactiveerd wordt.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot ArcGIS Online.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![image](./media/arcgis-tutorial/d_all_applications.png)

2. Selecteer in de lijst met toepassingen, **ArcGIS Online**.

    ![image](./media/arcgis-tutorial/d_all_application.png)

3. Selecteer in het menu aan de linkerkant, **gebruikers en groepen**.

    ![image](./media/arcgis-tutorial/d_leftpaneusers.png)

4. Selecteer de **toevoegen** knop en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![image](./media/arcgis-tutorial/d_assign_user.png)

4. In de **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

5. In de **toevoegen toewijzing** dialoogvenster Selecteer de **toewijzen** knop.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de ArcGIS Online tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw ArcGIS Online-toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



