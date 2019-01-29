---
title: 'Zelfstudie: Azure Active Directory-integratie met FilesAnywhere | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en FilesAnywhere.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: jeedes
ms.openlocfilehash: a6cc65feb49052a00fed94fb318570182dd9a49b
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55154623"
---
# <a name="tutorial-azure-active-directory-integration-with-filesanywhere"></a>Zelfstudie: Azure Active Directory-integratie met FilesAnywhere

In deze zelfstudie leert u hoe u FilesAnywhere integreren met Azure Active Directory (Azure AD).

FilesAnywhere integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot FilesAnywhere heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij FilesAnywhere (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - de Azure Management portal beheren

Zie [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met FilesAnywhere, moet u de volgende items:

- Een Azure AD-abonnement
- Een FilesAnywhere eenmalige aanmelding ingeschakeld abonnement


> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- U moet uw productie-omgeving, niet gebruiken als dit nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.


## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. FilesAnywhere uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-filesanywhere-from-the-gallery"></a>FilesAnywhere uit de galerie toe te voegen
Voor het configureren van de integratie van FilesAnywhere in Azure AD, moet u FilesAnywhere uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen FilesAnywhere uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure Management Portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Klik op **toevoegen** knop boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **FilesAnywhere**.

    ![Het maken van een Azure AD-testgebruiker](./media/filesanywhere-tutorial/tutorial_FilesAnywhere_search.png)

1. Selecteer in het deelvenster resultaten **FilesAnywhere**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/filesanywhere-tutorial/tutorial_FilesAnywhere_addfromgallery.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met FilesAnywhere op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in FilesAnywhere is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in FilesAnywhere tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door toe te wijzen de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in FilesAnywhere.

Om te configureren en testen van Azure AD eenmalige aanmelding met FilesAnywhere, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker FilesAnywhere](#creating-a-filesanywhere-test-user)**  : als u wilt een equivalent van Britta Simon in FilesAnywhere die is gekoppeld aan de Azure AD-weergave van haar hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u Azure AD eenmalige aanmelding in de Azure Management portal inschakelen en configureren van eenmalige aanmelding in uw toepassing FilesAnywhere.

**Voor het configureren van Azure AD eenmalige aanmelding met FilesAnywhere, moet u de volgende stappen uitvoeren:**

1. In de Azure-beheerportal, op de **FilesAnywhere** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster als **modus** Selecteer **SAML gebaseerde aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/filesanywhere-tutorial/tutorial_FilesAnywhere_samlbase.png)

1. Op de **FilesAnywhere domein en URL's** sectie, als u wilt configureren van de toepassing in **IDP gestart door modus**:

    ![Eenmalige aanmelding configureren](./media/filesanywhere-tutorial/tutorial_filesanywhere_url.png)
    
    a. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://<company name>.filesanywhere.com/saml20.aspx?c=215`
> [!NOTE]
> Houd er rekening mee dat de waarde **215** is een **clientid** en is slechts een voorbeeld. U moet deze vervangen door de werkelijke clientid-waarde.

1. Op de **FilesAnywhere domein en URL's** sectie, als u wilt configureren van de toepassing in **SP geïnitieerde modus**, voer de volgende stappen uit:
    
    ![Eenmalige aanmelding configureren](./media/filesanywhere-tutorial/tutorial_filesanywhere_url1.png)

    a. Klik op de **geavanceerde URL-instellingen weergeven** optie

    b. In de **aanmelding URL** tekstvak, een URL met behulp van het volgende patroon: `https://<sub domain>.filesanywhere.com/`

    > [!NOTE] 
    > Houd er rekening mee dat dit niet de werkelijke waarden zijn. U hebt deze waarden bijwerken met de werkelijke aanmelding URL en antwoord-URL. Neem contact op met [FilesAnywhere ondersteuningsteam](mailto:support@FilesAnywhere.com) om deze waarden te verkrijgen. 

1. FilesAnywhere softwaretoepassing wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de "**gebruikerskenmerken**" sectie op de pagina van de toepassing-integratie. In de volgende schermopname ziet u een voorbeeld hiervan.
    
    ![Eenmalige aanmelding configureren](./media/filesanywhere-tutorial/tutorial_filesanywhere_attribute.png)
    
    Wanneer de gebruikers zich aanmeldt met FilesAnywhere krijgen de waarde van **clientid** kenmerk uit [FilesAnywhere team](mailto:support@FilesAnywhere.com). U moet de "Client-Id-kenmerk met de unieke waarde die is opgegeven door FilesAnywhere toevoegen. Alle deze kenmerken die hierboven zijn vereist.
    > [!NOTE] 
    > Houd er rekening mee dat de waarde **. 2331** van **clientid** is slechts een voorbeeld. U moet de werkelijke waarde opgeven.


1. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de bovenstaande afbeelding en voer de volgende stappen uit:
    
    | Naam kenmerk | Waarde kenmerk |
    | ---------------| --------------- |    
    | clientid | *"uniquevalue"* |

    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/filesanywhere-tutorial/tutorial_FilesAnywhere_04.png)

    ![Eenmalige aanmelding configureren](./media/filesanywhere-tutorial/tutorial_FilesAnywhere_05.png)
    
    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.
    
    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    d. Klik op **OK**.

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/filesanywhere-tutorial/tutorial_general_400.png)

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/filesanywhere-tutorial/tutorial_FilesAnywhere_certificate.png) 

1. Op de **FilesAnywhere configuratie** sectie, klikt u op **configureren FilesAnywhere** openen **aanmelding configureren** venster.

    ![Eenmalige aanmelding configureren](./media/filesanywhere-tutorial/tutorial_FilesAnywhere_configure.png) 

    ![Eenmalige aanmelding configureren](./media/filesanywhere-tutorial/tutorial_FilesAnywhere_configuresignon.png)

1.  Als u SSO-configuratie voltooid voor uw toepassing aan FilesAnywhere einde, neem contact op met [FilesAnywhere ondersteuningsteam](mailto:support@FilesAnywhere.com) en geeft u het gedownloade SAML-token ondertekenen certificaat en eenmalige aanmelding (SSO)-URL.

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in de Azure Management portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure Management portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/filesanywhere-tutorial/create_aaduser_01.png) 

1. Ga naar **gebruikers en groepen** en klikt u op **alle gebruikers** om de lijst met gebruikers weer te geven.
    
    ![Het maken van een Azure AD-testgebruiker](./media/filesanywhere-tutorial/create_aaduser_02.png) 

1. Aan de bovenkant van het dialoogvenster klikt u op **toevoegen** openen de **gebruiker** dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/filesanywhere-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/filesanywhere-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**. 



### <a name="creating-a-filesanywhere-test-user"></a>Het maken van een testgebruiker FilesAnywhere

De toepassing biedt ondersteuning voor just in time-gebruikersinrichting. Dit betekent dat gebruikers na verificatie automatisch worden aangemaakt in de toepassing. 


### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door haar toegang verlenen tot FilesAnywhere.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan FilesAnywhere toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-beheerportal, en vervolgens gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **FilesAnywhere**.

    ![Eenmalige aanmelding configureren](./media/filesanywhere-tutorial/tutorial_FilesAnywhere_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    


### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel FilesAnywhere in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing FilesAnywhere.


## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/FilesAnywhere-tutorial/tutorial_general_01.png
[2]: ./media/FilesAnywhere-tutorial/tutorial_general_02.png
[3]: ./media/FilesAnywhere-tutorial/tutorial_general_03.png
[4]: ./media/FilesAnywhere-tutorial/tutorial_general_04.png

[100]: ./media/FilesAnywhere-tutorial/tutorial_general_100.png

[200]: ./media/FilesAnywhere-tutorial/tutorial_general_200.png
[201]: ./media/FilesAnywhere-tutorial/tutorial_general_201.png
[202]: ./media/FilesAnywhere-tutorial/tutorial_general_202.png
[203]: ./media/FilesAnywhere-tutorial/tutorial_general_203.png
