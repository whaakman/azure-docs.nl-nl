---
title: 'Zelfstudie: Azure Active Directory-integratie met TINFOIL SECURITY | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en TINFOIL SECURITY.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: da02da92-e3b0-4c09-ad6c-180882b0f9f8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: ae136c9aa0e21b2a5dabf06b050727871bddf03b
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55191972"
---
# <a name="tutorial-azure-active-directory-integration-with-tinfoil-security"></a>Zelfstudie: Azure Active Directory-integratie met TINFOIL SECURITY

In deze zelfstudie leert u hoe u TINFOIL SECURITY integreren met Azure Active Directory (Azure AD).

TINFOIL SECURITY integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot TINFOIL SECURITY heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij TINFOIL SECURITY (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met TINFOIL SECURITY, moet u de volgende items:

- Een Azure AD-abonnement
- Een TINFOIL SECURITY eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. TINFOIL SECURITY uit de galerie toevoegen
1. Azure AD-eenmalige aanmelding configureren en testen

## <a name="add-tinfoil-security-from-the-gallery"></a>TINFOIL SECURITY uit de galerie toevoegen
Voor het configureren van de integratie van TINFOIL SECURITY in Azure AD, moet u TINFOIL SECURITY uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen TINFOIL SECURITY uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **TINFOIL SECURITY**, selecteer **TINFOIL SECURITY** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![TINFOIL SECURITY vanuit galerie](./media/tinfoil-security-tutorial/tutorial_tinfoil-security_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met TINFOIL SECURITY op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in TINFOIL SECURITY is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in TINFOIL SECURITY tot stand worden gebracht.

In TINFOIL SECURITY, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met TINFOIL SECURITY, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker TINFOIL SECURITY](#create-a-tinfoil-security-test-user)**  : als u wilt hebben van een equivalent van Britta Simon in TINFOIL SECURITY die is gekoppeld aan de Azure AD-weergave van de gebruiker.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u Azure AD eenmalige aanmelding in de Azure-portal inschakelen en configureren van eenmalige aanmelding in uw toepassing TINFOIL SECURITY.

**Voor het configureren van Azure AD eenmalige aanmelding met TINFOIL SECURITY, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **TINFOIL SECURITY** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![SAML-aanmelding](./media/tinfoil-security-tutorial/tutorial_tinfoil-security_samlbase.png)

1. Op de **TINFOIL SECURITY-domein en URL's** sectie, de gebruiker beschikt niet over de stappen uitvoeren omdat de app is al vooraf geïntegreerd met Azure.

    ![Eenmalige aanmelding configureren](./media/tinfoil-security-tutorial/tutorial_tinfoil-security_url.png)


1. Op de **SAML-handtekeningcertificaat** sectie, Kopieer de **VINGERAFDRUK** waarde.

    ![Sectie voor SAML-handtekeningcertificaat](./media/tinfoil-security-tutorial/tutorial_tinfoil-security_certificate.png) 

1. Als u wilt toevoegen de vereiste kenmerktoewijzingen, moet u de volgende stappen uitvoeren:
    
    ![Kenmerken](./media/tinfoil-security-tutorial/tutorial_tinfoil-security_attribute1.png "Kenmerken")
    
    | Naam kenmerk    |   Waarde kenmerk |
    | ------------------- | -------------------- |
    | AccountId | UXXXXXXXXXXXXX |
    
    a. Klik op **toevoegen gebruikerskenmerk**.
    
    ![Kenmerk toevoegen](./media/tinfoil-security-tutorial/tutorial_tinfoil-security_attribute.png "kenmerken")
    
    ![Kenmerk toevoegen](./media/tinfoil-security-tutorial/tutorial_tinfoil-security_addatt.png "kenmerken")
    
    b. In de **kenmerknaam** tekstvak, type **accountid**.
    
    c. In de **kenmerkwaarde** tekstvak, plak de account-ID-waarde die u later op de zelfstudie krijgt.
    
    d. Klik op **OK**.    

1. Klik op de knop **Save**.

    ![De knop Opslaan](./media/tinfoil-security-tutorial/tutorial_general_400.png)

1. Op de **TINFOIL SECURITY Configuration** sectie, klikt u op **configureren TINFOIL SECURITY** openen **aanmelding configureren** venster. Kopiëren de **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![TINFOIL SECURITY-configuratie](./media/tinfoil-security-tutorial/tutorial_tinfoil-security_configure.png) 

1. Meld u in een ander browservenster in uw bedrijf TINFOIL SECURITY site als beheerder.

1. Klik in de werkbalk bovenaan op **Mijn Account**.
   
    ![Dashboard](./media/tinfoil-security-tutorial/ic798971.png "Dashboard")

1. Klik op **Beveiliging**.
   
    ![Beveiliging](./media/tinfoil-security-tutorial/ic798972.png "Beveiliging")

1. Op de **Single Sign-On** configuratie pagina, voert u de volgende stappen uit:
   
    ![Single Sign-On](./media/tinfoil-security-tutorial/ic798973.png "Single Sign-On")
   
    a. Selecteer **SAML inschakelen**.
   
    b. Klik op **handmatige configuratie**.
   
    c. In **URL voor SAML-Post** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML** die u hebt gekopieerd vanuit Azure portal
   
    d. In **SAML-certificaat vingerafdruk** tekstvak, plak de waarde van **vingerafdruk** die u hebt gekopieerd uit **SAML-handtekeningcertificaat** sectie.
  
    e. Kopie **uw Account-ID** waarde en plak de waarde in **kenmerkwaarde** tekstvak onder **kenmerk toevoegen** sectie in Azure portal.
   
    f. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/tinfoil-security-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Gebruikers en groepen -> alle gebruikers ](./media/tinfoil-security-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Gebruiker](./media/tinfoil-security-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/tinfoil-security-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="create-a-tinfoil-security-test-user"></a>Maak een testgebruiker TINFOIL SECURITY

Als u wilt dat Azure AD-gebruikers kunnen zich aanmelden bij TINFOIL SECURITY, moeten ze worden ingericht voor TINFOIL SECURITY. In het geval van TINFOIL SECURITY is inrichten een handmatige taak.

**Als u een gebruiker die is ingericht, kunt u de volgende stappen uitvoeren:**

1. Als de gebruiker een onderdeel van een Enterprise-account is, moet u [Neem contact op met het ondersteuningsteam van TINFOIL SECURITY](https://www.tinfoilsecurity.com/contact) om op te halen van het gebruikersaccount dat is gemaakt.

1. Als de gebruiker een standaardgebruiker TINFOIL SECURITY SaaS, kunnen de gebruiker kan een samenwerker toevoegen met een van de sites van de gebruiker. Dit activeert een proces voor het verzenden van een uitnodiging voor het opgegeven e-mailbericht om een nieuwe TINFOIL SECURITY-gebruikersaccount te maken.

> [!NOTE]
> U kunt een andere hulpprogramma's van TINFOIL SECURITY gebruiker-account maken of geleverd door TINFOIL SECURITY-API's gebruiken voor het inrichten van gebruikersaccounts van de Azure AD.
> 
> 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan TINFOIL SECURITY.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon en TINFOIL SECURITY, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **TINFOIL SECURITY**.

    ![TINFOIL SECURITY selecteren](./media/tinfoil-security-tutorial/tutorial_tinfoil-security_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel TINFOIL SECURITY in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing TINFOIL SECURITY. Zie [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/tinfoil-security-tutorial/tutorial_general_01.png
[2]: ./media/tinfoil-security-tutorial/tutorial_general_02.png
[3]: ./media/tinfoil-security-tutorial/tutorial_general_03.png
[4]: ./media/tinfoil-security-tutorial/tutorial_general_04.png

[100]: ./media/tinfoil-security-tutorial/tutorial_general_100.png

[200]: ./media/tinfoil-security-tutorial/tutorial_general_200.png
[201]: ./media/tinfoil-security-tutorial/tutorial_general_201.png
[202]: ./media/tinfoil-security-tutorial/tutorial_general_202.png
[203]: ./media/tinfoil-security-tutorial/tutorial_general_203.png

