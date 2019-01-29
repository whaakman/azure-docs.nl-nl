---
title: 'Zelfstudie: Azure Active Directory-integratie met SAP Business ByDesign | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SAP Business ByDesign.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 82938920-33ba-47cb-b141-511b46d19e66
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: 62a448c5a0994dc549d06808c6ece520eabefc3e
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55193315"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-bydesign"></a>Zelfstudie: Azure Active Directory-integratie met SAP Business ByDesign

In deze zelfstudie leert u hoe u SAP Business ByDesign integreren met Azure Active Directory (Azure AD).

SAP Business ByDesign integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot SAP Business ByDesign heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij SAP Business ByDesign (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SAP Business ByDesign, moet u de volgende items:

- Een Azure AD-abonnement
- Een SAP Business ByDesign eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SAP Business ByDesign uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-sap-business-bydesign-from-the-gallery"></a>SAP Business ByDesign uit de galerie toe te voegen
Voor het configureren van de integratie van SAP Business ByDesign in Azure AD, moet u SAP Business ByDesign uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SAP Business ByDesign uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **SAP Business ByDesign**, selecteer **SAP Business ByDesign** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![SAP Business ByDesign in de lijst met resultaten](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met SAP Business ByDesign op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in SAP Business ByDesign is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in SAP Business ByDesign tot stand worden gebracht.

In SAP Business ByDesign, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met SAP Business ByDesign, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker SAP Business ByDesign](#create-an-sap-business-bydesign-test-user)**  : als u wilt een equivalent van Britta Simon hebben in SAP Business ByDesign die is gekoppeld aan de Azure AD-weergave van de gebruiker.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u Azure AD eenmalige aanmelding in de Azure-portal inschakelen en configureren van eenmalige aanmelding in uw toepassing SAP Business ByDesign.

**Voor het configureren van Azure AD eenmalige aanmelding met SAP Business ByDesign, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **SAP Business ByDesign** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_samlbase.png)

1. Op de **SAP Business ByDesign domein en URL's** sectie, voert u de volgende stappen uit:

    ![SAP Business ByDesign domein en URL's, eenmalige aanmelding informatie](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<servername>.sapbydesign.com`

    b. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<servername>.sapbydesign.com`

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de daadwerkelijke aanmeldings-URL en id. Neem contact op met [SAP Business ByDesign Client ondersteuningsteam](https://www.sap.com/products/cloud-analytics.support.html) om deze waarden te verkrijgen.

1. Op de **gebruikerskenmerken** sectie, voert u de volgende stappen uit:

    ![SAP Business ByDesign kenmerk sectie](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_attribute.png)
    
    a. In **gebruikers-id** in de lijst met de **ExtractMailPrefix()** functie.
    
    b. Uit de **e-Mail** , selecteert u het gebruikerskenmerk die u wilt gebruiken voor uw implementatie. Als u bijvoorbeeld de werknemer-id wilt gebruiken als unieke gebruikers-id en u de waarde van het kenmerk in de ExtensionAttribute2 hebt opgeslagen, selecteert u vervolgens user.extensionattribute2.     

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_certificate.png) 

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/sapbusinessbydesign-tutorial/tutorial_general_400.png)

1. Op de **SAP Business ByDesign configuratie** sectie, klikt u op **configureren SAP Business ByDesign** openen **aanmelding configureren** venster. Kopiëren de **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![Configuratie van SAP Business ByDesign](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_configure.png) 

1. Als u eenmalige aanmelding configureren voor uw toepassing, moet u de volgende stappen uitvoeren:
   
    a. Meld u aan bij de portal van uw SAP Business ByDesign met administrator-rechten.
   
    b. Navigeer naar **toepassings- en algemene Gebruikersbeheertaak** en klikt u op de **id-Provider** tabblad.
   
    c. Klik op **nieuwe id-Provider** en selecteert u de metagegevens-XML-bestand dat u hebt gedownload vanuit Azure portal. Door het importeren van de metagegevens worden automatisch het vereiste handtekeningcertificaat en versleutelingscertificaat geüpload.
   
    ![Eenmalige aanmelding configureren](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_54.png)
   
    d. Om op te nemen de **URL van de Bevestigingsconsumerservice** selecteren in de SAML-aanvraag **opnemen URL van de Bevestigingsconsumerservice**.
   
    e. Klik op **Eenmalige aanmelding activeren**.
   
    f. Sla uw wijzigingen op.
   
    g. Klik op het tabblad **Mijn systeem**.
   
    ![Eenmalige aanmelding configureren](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_52.png)
   
    h. Plakken **Single Sign-On Service URL voor SAML**, die u hebt gekopieerd vanuit Azure portal naar de **Azure AD aanmeldings-URL** tekstvak.
   
    ![Eenmalige aanmelding configureren](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_53.png)
   
    i. Opgeven of de werknemer handmatig kiezen kunt tussen het aanmelden met gebruikersnaam en wachtwoord of eenmalige aanmelding door te selecteren **handmatige Identity Provider selectie**.
   
    j. In de **SSO URL** sectie, geef de URL die moet worden gebruikt door de werknemer om aan te melden bij het systeem. 
    In de verzonden naar de werknemer vervolgkeuzelijst URL, kunt u kiezen uit de volgende opties:
   
    **Niet-SSO-URL**
   
    Alleen de normale systeem-URL wordt naar de werknemer verstuurd. De werknemer kan niet aanmelden met eenmalige aanmelding, maar moet in plaats daarvan gebruikmaken van een wachtwoord of certificaat.
   
    **SSO-URL** 
   
    Alleen de SSO-URL wordt naar de werknemer verzonden. De werknemer kan zich aanmelden met behulp van eenmalige aanmelding. Een verificatieaanvraag wordt omgeleid via de IdP.
   
    **Automatische selectie**
   
    Als eenmalige aanmelding niet actief is, wordt alleen de normale systeem-URL naar de werknemer verstuurd. Als eenmalige aanmelding actief is, controleert het systeem of de werknemer een wachtwoord heeft. Als er een wachtwoord beschikbaar is, worden zowel de SSO-URL als de niet-SSO-URL naar de werknemer verzonden. Als de werknemer geen wachtwoord heeft, wordt echter alleen de URL voor eenmalige aanmelding naar de werknemer verzonden.
   
    k. Sla uw wijzigingen op.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/sapbusinessbydesign-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/sapbusinessbydesign-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/sapbusinessbydesign-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/sapbusinessbydesign-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-sap-business-bydesign-test-user"></a>Maak een testgebruiker SAP Business ByDesign

In deze sectie maakt u een gebruiker met de naam van Britta Simon in SAP Business ByDesign. Neem contact op met [SAP Business ByDesign Client ondersteuningsteam](https://www.sap.com/products/cloud-analytics.support.html) om toe te voegen de gebruikers in de SAP Business ByDesign-platform. 

> [!NOTE]
> Zorg ervoor dat NameID-waarde met het gebruikersnaamveld in de SAP Business ByDesign-platform overeenkomen moet.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding toegang verlenen tot SAP Business ByDesign.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan SAP Business ByDesign toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **SAP Business ByDesign**.

    ![De SAP Business ByDesign-koppeling in de lijst met toepassingen](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel SAP Business ByDesign in het toegangsvenster, u moet u automatisch aangemeld bij uw SAP Business ByDesign-toepassing.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/sapbusinessbydesign-tutorial/tutorial_general_01.png
[2]: ./media/sapbusinessbydesign-tutorial/tutorial_general_02.png
[3]: ./media/sapbusinessbydesign-tutorial/tutorial_general_03.png
[4]: ./media/sapbusinessbydesign-tutorial/tutorial_general_04.png

[100]: ./media/sapbusinessbydesign-tutorial/tutorial_general_100.png

[200]: ./media/sapbusinessbydesign-tutorial/tutorial_general_200.png
[201]: ./media/sapbusinessbydesign-tutorial/tutorial_general_201.png
[202]: ./media/sapbusinessbydesign-tutorial/tutorial_general_202.png
[203]: ./media/sapbusinessbydesign-tutorial/tutorial_general_203.png

