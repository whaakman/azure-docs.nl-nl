---
title: 'Zelfstudie: Azure Active Directory-integratie met halogeen Software | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en halogeen Software.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 2ca2298d-9a0c-4f14-925c-fa23f2659d28
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 8220f94be58cd5602ffcb814d8b24db4c19e1f41
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54827972"
---
# <a name="tutorial-azure-active-directory-integration-with-halogen-software"></a>Zelfstudie: Azure Active Directory-integratie met halogeen Software

In deze zelfstudie leert u hoe u halogeen om Software te integreren met Azure Active Directory (Azure AD).

Halogeen Software integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot halogeen Software heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij halogeen Software (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met halogeen Software, moet u de volgende items:

- Een Azure AD-abonnement
- Een halogeen Software eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Halogeen Software uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-halogen-software-from-the-gallery"></a>Halogeen Software uit de galerie toe te voegen

Voor het configureren van de integratie van halogeen Software in Azure AD, moet u halogeen Software uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen halogeen Software uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **halogeen Software**.

    ![Het maken van een Azure AD-testgebruiker](./media/halogen-software-tutorial/tutorial_halogensoftware_search.png)

1. Selecteer in het deelvenster resultaten **halogeen Software**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/halogen-software-tutorial/tutorial_halogensoftware_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met halogeen-Software op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in halogeen Software is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Software halogeen tot stand worden gebracht.

Wijs in halogeen Software, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en Azure AD eenmalige aanmelding met halogeen Software testen, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker halogeen Software](#creating-a-halogen-software-test-user)**  : als u wilt een equivalent van Britta Simon in halogeen-Software die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing halogeen Software.

**Voor het configureren van Azure AD eenmalige aanmelding met halogeen Software, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **halogeen Software** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/halogen-software-tutorial/tutorial_halogensoftware_samlbase.png)

1. Op de **halogeen Software domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/halogen-software-tutorial/tutorial_halogensoftware_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://global.hgncloud.com/<companyname>`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://global.halogensoftware.com/<companyname>`, `https://global.hgncloud.com/<companyname>`

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de daadwerkelijke aanmeldings-URL en id. Neem contact op met [halogeen softwareclient ondersteuningsteam](https://support.halogensoftware.com/) om deze waarden te verkrijgen. 
 


1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/halogen-software-tutorial/tutorial_halogensoftware_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/halogen-software-tutorial/tutorial_general_400.png)

1. In een ander browservenster aanmelden voor uw **halogeen Software** toepassing als beheerder.

1. Klik op de **opties** tabblad. 
   
    ![Wat is Azure AD Connect?][12]

1. Klik in het linkernavigatiedeelvenster op **SAML-configuratie**. 
   
    ![Wat is Azure AD Connect?][13]

1. Op de **SAML-configuratie** pagina, voert u de volgende stappen uit: 

    ![Wat is Azure AD Connect?][14]

     a. Als **unieke id**, selecteer **NameID**.

     b. Als **unieke id gerelateerd aan**, selecteer **gebruikersnaam**.
  
     c. Als u wilt uw gedownloade metagegevensbestand uploaden, klikt u op **Bladeren** om het bestand te selecteren en vervolgens **-bestand uploaden**.
 
     d. Als u wilt testen van de configuratie, klikt u op **Test uitvoeren**. 
    
    >[!NOTE]
    >U moet wachten op het bericht '*de SAML-test is voltooid. Sluit dit venster*'. Sluit het geopende browservenster. De **SAML inschakelen** selectievakje is alleen beschikbaar als de test is voltooid. 
     
     e. Selecteer **SAML inschakelen**.
    
     f. Klik op **Wijzigingen opslaan**. 

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/halogen-software-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/halogen-software-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/halogen-software-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/halogen-software-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak typenaam als **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-halogen-software-test-user"></a>Het maken van een testgebruiker halogeen Software

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in halogeen Software.

**Voor het maken van een gebruiker met de naam van Britta Simon in halogeen Software, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **halogeen Software** toepassing als beheerder.

1. Klik op de **gebruiker Center** tabblad en klik vervolgens op **Create User**.
   
    ![Wat is Azure AD Connect?][300]  

1. Op de **nieuwe gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
   
    ![Wat is Azure AD Connect?][301]

    a. In de **voornaam** tekstvak, type de voornaam van de gebruiker, zoals **Julia**.
    
    b. In de **achternaam** tekstvak, type achternaam van de gebruiker, zoals **Simon**. 

    c. In de **gebruikersnaam** tekstvak, type **Britta Simon**, zoals in de Azure-portal de naam van de gebruiker.

    d. In de **wachtwoord** tekstvak, typ een wachtwoord voor Julia.
    
    e. Klik op **Opslaan**.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot halogeen Software.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon halogeen software, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **halogeen Software**.

    ![Eenmalige aanmelding configureren](./media/halogen-software-tutorial/tutorial_halogensoftware_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van de configuratie van uw Azure AD-eenmalige aanmelding via het toegangsvenster.

Wanneer u op de tegel halogeen Software in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing halogeen Software.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/halogen-software-tutorial/tutorial_general_01.png
[2]: ./media/halogen-software-tutorial/tutorial_general_02.png
[3]: ./media/halogen-software-tutorial/tutorial_general_03.png
[4]: ./media/halogen-software-tutorial/tutorial_general_04.png

[12]: ./media/halogen-software-tutorial/tutorial_halogen_12.png

[13]: ./media/halogen-software-tutorial/tutorial_halogen_13.png

[14]: ./media/halogen-software-tutorial/tutorial_halogen_14.png

[100]: ./media/halogen-software-tutorial/tutorial_general_100.png

[200]: ./media/halogen-software-tutorial/tutorial_general_200.png
[201]: ./media/halogen-software-tutorial/tutorial_general_201.png
[202]: ./media/halogen-software-tutorial/tutorial_general_202.png
[203]: ./media/halogen-software-tutorial/tutorial_general_203.png

[300]: ./media/halogen-software-tutorial/tutorial_halogen_300.png

[301]: ./media/halogen-software-tutorial/tutorial_halogen_301.png
