---
title: 'Zelfstudie: Azure Active Directory-integratie met Druva | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en druva nodig.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: ab92b600-1fea-4905-b1c7-ef8e4d8c495c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2017
ms.author: jeedes
ms.openlocfilehash: 0429a7d0845b8590570dfb8ffc513ca3bb0ce998
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55195942"
---
# <a name="tutorial-azure-active-directory-integration-with-druva"></a>Zelfstudie: Azure Active Directory-integratie met druva nodig

In deze zelfstudie leert u hoe u Druva integreren met Azure Active Directory (Azure AD).

Druva integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot druva nodig heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Druva (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met druva nodig, moet u de volgende items:

- Een Azure AD-abonnement
- Een Druva eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Druva uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-druva-from-the-gallery"></a>Druva uit de galerie toe te voegen
Voor het configureren van de integratie van druva nodig in Azure AD, moet u Druva uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Druva uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **Druva**, selecteer **Druva** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Druva nodig in de lijst met resultaten](./media/druva-tutorial/tutorial_druva_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met druva nodig op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD weten wat de gebruiker equivalent in druva nodig is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Druva tot stand worden gebracht.

Wijs in druva nodig, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met druva nodig, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker Druva](#create-a-druva-test-user)**  : als u wilt een equivalent van Britta Simon in druva nodig die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing druva nodig.

**Voor het configureren van Azure AD eenmalige aanmelding met druva nodig, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Druva** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/druva-tutorial/tutorial_druva_samlbase.png)

1. Op de **Druva domein en URL's** sectie, als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Eenmalige aanmelding configureren](./media/druva-tutorial/tutorial_druva_url.png)

    In de **id** tekstvak typt u de tekenreekswaarde: `druva-cloud`
    
1. Controleer **geavanceerde URL-instellingen weergeven**. Als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Eenmalige aanmelding configureren](./media/druva-tutorial/tutorial_druva_url1.png)
    
    In de **aanmeldings-URL** tekstvak typt u de URL: `https://cloud.druva.com/home`

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![De link om het certificaat te downloaden](./media/druva-tutorial/tutorial_druva_certificate.png) 

1. Uw toepassing Druva wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, die vereist dat u om toe te voegen van aangepast kenmerktoewijzingen aan uw **SAML-Token kenmerken** configuratie. 

    ![Eenmalige aanmelding configureren](./media/druva-tutorial/tutorial_druva_attribute.png)

1. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de voorgaande afbeelding en voer de volgende stappen uit:

    | Naam kenmerk      | Waarde kenmerk      |
    | ------------------- | -------------------- |
    | insync\_auth\_token |Voer het token gegenereerde waarde |
    
    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.
    
    ![Eenmalige aanmelding configureren](./media/druva-tutorial/tutorial_attribute_04.png)
    
    ![Eenmalige aanmelding configureren](./media/druva-tutorial/tutorial_attribute_05.png)
    
    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij. De token gegenereerde waarde wordt verderop in de zelfstudie.
    
    d. Klik op **OK**.    

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/druva-tutorial/tutorial_general_400.png)

1. Op de **Druva configuratie** sectie, klikt u op **configureren Druva** openen **aanmelding configureren** venster. Kopiëren de **afmelding URL's en SAML Single Sign-On Service** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/druva-tutorial/tutorial_druva_configure.png) 

1. In een ander browservenster aanmelden bij uw bedrijf Druva site als beheerder.

1. Ga naar **beheren \> instellingen**.

    ![Settings](./media/druva-tutorial/ic795091.png "Settings")

1. In het dialoogvenster Instellingen voor eenmalige aanmelding in de volgende stappen uitvoeren:

    ![Single Sign-On instellingen](./media/druva-tutorial/ic795092.png "Single Sign-On-instellingen")
    
    a. In **aanmeldings-URL van ID-Provider** tekstvak, plak de waarde van **Single Sign-On Service URL**, die u hebt gekopieerd vanuit Azure portal.
        
    b. In **afmeldings-URL van ID-Provider** tekstvak, plak de waarde van **afmelding URL**, die u hebt gekopieerd vanuit Azure portal
        
    c. Het base-64 gecodeerde certificaat openen in Kladblok, Kopieer de inhoud ervan in het Klembord en plakt u deze naar de **-ID-Provider certificaat** tekstvak
     
    d. Om te openen de **instellingen** pagina, klikt u op **opslaan**.

1. Op de **instellingen** pagina, klikt u op **SSO-Token genereren**.

    ![Settings](./media/druva-tutorial/ic795093.png "Settings")

1. Op de **Single Sign-on-verificatietoken** dialoogvenster, voer de volgende stappen uit:

    ![Het SSO-Token](./media/druva-tutorial/ic795094.png "SSO-Token")
    
    a. Klik op **kopie**, plakken gekopieerd waarde in de **waarde** -tekstvak in de **kenmerk toevoegen** sectie in Azure portal.
    
    b. Klik op **Sluiten**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/druva-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/druva-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/druva-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/druva-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-druva-test-user"></a>Maak een testgebruiker druva nodig

Als u wilt dat Azure AD-gebruikers zich aanmelden bij druva nodig, moeten ze worden ingericht voor druva nodig. In het geval van druva nodig, met de inrichting is een handmatige taak.

**Voer de volgende stappen uit om de gebruikersinrichting te configureren:**

1. Meld u aan bij uw **Druva** bedrijf site als administrator.

1. Ga naar **beheren \> gebruikers**.
   
   ![Gebruikers beheren](./media/druva-tutorial/ic795097.png "gebruikers beheren")

1. Klik op **Maak een nieuwe**.
   
   ![Gebruikers beheren](./media/druva-tutorial/ic795098.png "gebruikers beheren")

1. Voer de volgende stappen uit in het dialoogvenster Nieuwe gebruiker maken:
   
   ![Maken van nieuwegebruiker](./media/druva-tutorial/ic795099.png "nieuwegebruiker maken")
   
   a. In de **e-mailadres** tekstvak, voer het e-mailadres van gebruiker, zoals **brittasimon@contoso.com**.
   
   b. In de **naam** tekstvak, voer de naam van gebruiker, zoals **BrittaSimon**.
   
   c. Klik op **gebruiker maken**.

>[!NOTE]
>U kunt alle andere Druva gebruiker-account maken van hulpprogramma's of API's geleverd door druva nodig voor het inrichten van gebruikersaccounts van de Azure AD.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Druva.

![De de gebruikersrol toewijzen][200] 

**Britta Simon om aan te wijzen druva nodig, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Druva**.

    ![De koppeling druva nodig in de lijst met toepassingen](./media/druva-tutorial/tutorial_druva_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel druva nodig in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing druva nodig.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/druva-tutorial/tutorial_general_01.png
[2]: ./media/druva-tutorial/tutorial_general_02.png
[3]: ./media/druva-tutorial/tutorial_general_03.png
[4]: ./media/druva-tutorial/tutorial_general_04.png

[100]: ./media/druva-tutorial/tutorial_general_100.png

[200]: ./media/druva-tutorial/tutorial_general_200.png
[201]: ./media/druva-tutorial/tutorial_general_201.png
[202]: ./media/druva-tutorial/tutorial_general_202.png
[203]: ./media/druva-tutorial/tutorial_general_203.png

