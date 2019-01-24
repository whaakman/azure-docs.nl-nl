---
title: 'Zelfstudie: Azure Active Directory-integratie met FirmPlay - werknemer Advocacy voor werving | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en FirmPlay - ondersteuning voor werving werknemer.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: a6799629-7546-43f8-a966-956db32864b1
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2017
ms.author: jeedes
ms.openlocfilehash: 929494d5d802dbc545c750386a286029c4bf962d
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54809799"
---
# <a name="tutorial-azure-active-directory-integration-with-firmplay---employee-advocacy-for-recruiting"></a>Zelfstudie: Azure Active Directory-integratie met FirmPlay - ondersteuning voor werving werknemer

In deze zelfstudie leert u hoe u integreert FirmPlay - werknemer Advocacy voor werving met Azure Active Directory (Azure AD).

Integratie van FirmPlay - werknemer Advocacy voor werving met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot FirmPlay - ondersteuning voor werving werknemer heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij FirmPlay - werknemer Advocacy voor werving (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - de Azure Management portal beheren

Zie [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met FirmPlay - werknemer Advocacy voor werving, moet u de volgende items:

- Een Azure AD-abonnement
- Een FirmPlay - ondersteuning voor eenmalige aanmelding ingeschakeld abonnement werving werknemer


> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- U moet uw productie-omgeving, niet gebruiken als dit nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.


## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. FirmPlay - werknemer Advocacy voor werving uit de galerie toevoegen
1. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-firmplay---employee-advocacy-for-recruiting-from-the-gallery"></a>FirmPlay - werknemer Advocacy voor werving uit de galerie toevoegen
Voor het configureren van de integratie van FirmPlay - werknemer Advocacy voor werving in Azure AD, moet u FirmPlay - werknemer Advocacy voor werving uit de galerie aan de lijst met beheerde SaaS-apps toevoegen.

**Als u wilt toevoegen, FirmPlay - werknemer Advocacy voor werving uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure Management Portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Klik op **toevoegen** knop boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **FirmPlay - werknemer Advocacy voor werving**.

    ![Het maken van een Azure AD-testgebruiker](./media/firmplay-tutorial/tutorial_firmplay_001.png)

1. Selecteer in het deelvenster resultaten **FirmPlay - werknemer Advocacy voor werving**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/firmplay-tutorial/tutorial_firmplay_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met FirmPlay - werknemer Advocacy voor werving op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding voor het werk, Azure AD moet weten wat de equivalente-gebruiker in FirmPlay - ondersteuning voor werving werknemer is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in FirmPlay - ondersteuning voor werving werknemer tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door toe te wijzen de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in FirmPlay - ondersteuning voor werving werknemer.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met FirmPlay - werknemer Advocacy voor werving, u nodig hebt voor de volgende bouwstenen:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een FirmPlay - ondersteuning voor de testgebruiker werving werknemer](#creating-a-firmplay---employee-advocacy-for-recruiting-test-user)**  : als u wilt een equivalent van Britta Simon in FirmPlay hebben: Werknemer ondersteuning voor het werven van dat is gekoppeld aan de Azure AD-weergave van haar.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure Management portal en eenmalige aanmelding in uw FirmPlay - werknemer Advocacy voor werving toepassing configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met FirmPlay - werknemer Advocacy voor werving, moet u de volgende stappen uitvoeren:**

1. In de Azure-beheerportal, op de **FirmPlay - werknemer Advocacy voor werving** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster als **modus** Selecteer **SAML gebaseerde aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/firmplay-tutorial/tutorial_firmplay_01.png)

1. Op de **FirmPlay - ondersteuning voor het werven van domein en URL's werknemer** sectie in de **aanmelding URL** tekstvak, een URL met behulp van het volgende patroon: `https://<your-subdomain>.firmplay.com/`

    ![Eenmalige aanmelding configureren](./media/firmplay-tutorial/tutorial_firmplay_02.png)

    > [!NOTE] 
    > Houd er rekening mee dat dit niet de werkelijke waarde is. U moet deze waarde bijwerken met de werkelijke op URL. Neem contact op met [FirmPlay - ondersteuning voor het ondersteuningsteam werving werknemer](mailto:engineering@firmplay.com) deze waarde op te halen. 

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **nieuw certificaat maken**.

    ![Eenmalige aanmelding configureren](./media/firmplay-tutorial/tutorial_firmplay_03.png)     

1. Op de **nieuw certificaat maken** dialoogvenster, klikt u op het kalenderpictogram en selecteer een **vervaldatum**. Klik vervolgens op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/firmplay-tutorial/tutorial_general_300.png)

1. Op de **SAML-handtekeningcertificaat** sectie, selecteer **nieuw certificaat activeren** en klikt u op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/firmplay-tutorial/tutorial_firmplay_04.png)

1. In het pop-upvenster **rollovercertificaat** venster, klikt u op **OK**.

    ![Eenmalige aanmelding configureren](./media/firmplay-tutorial/tutorial_general_400.png)

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (base64)** en slaat u het certificaatbestand op uw computer. 

    ![Eenmalige aanmelding configureren](./media/firmplay-tutorial/tutorial_firmplay_05.png) 

1. Op de **FirmPlay - werknemer ondersteuning voor de configuratie van werving** sectie, klikt u op **configureren FirmPlay - werknemer Advocacy voor werving** openen **aanmelding configureren** het dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/firmplay-tutorial/tutorial_firmplay_06.png) 

    ![Eenmalige aanmelding configureren](./media/firmplay-tutorial/tutorial_firmplay_07.png)

1. Als u eenmalige aanmelding configureren voor uw toepassing, neem contact op met [FirmPlay - ondersteuning voor het ondersteuningsteam werving werknemer](mailto:engineering@firmplay.com) en geeft u het volgende: 

    • De gedownloade **certificaatbestand**

    • De **URL voor SAML-Service voor eenmalige aanmelding**

    • De **SAML entiteit-ID**

    • De **de afmeld-URL**
  

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in de Azure Management portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure Management portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/firmplay-tutorial/create_aaduser_01.png) 

1. Ga naar **gebruikers en groepen** en klikt u op **alle gebruikers** om de lijst met gebruikers weer te geven.
    
    ![Het maken van een Azure AD-testgebruiker](./media/firmplay-tutorial/create_aaduser_02.png) 

1. Aan de bovenkant van het dialoogvenster klikt u op **toevoegen** openen de **gebruiker** dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/firmplay-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/firmplay-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**. 



### <a name="creating-a-firmplay---employee-advocacy-for-recruiting-test-user"></a>Het maken van een FirmPlay - ondersteuning voor de testgebruiker werving werknemer

In deze sectie maakt u een gebruiker met de naam van Britta Simon in FirmPlay - ondersteuning voor werving werknemer. Neem contact op met [FirmPlay - ondersteuning voor het ondersteuningsteam werving werknemer](mailto:engineering@firmplay.com) om toe te voegen de gebruikers in de FirmPlay - werknemer Advocacy voor werving platform.


### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door haar toegang verlenen tot FirmPlay - werknemer Advocacy voor werving.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen FirmPlay - werknemer Advocacy voor werving, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-beheerportal, en vervolgens gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **FirmPlay - werknemer Advocacy voor werving**.

    ![Eenmalige aanmelding configureren](./media/firmplay-tutorial/tutorial_firmplay_50.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    


### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u klikt op de FirmPlay - werknemer Advocacy werving tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw FirmPlay - werknemer Advocacy voor werving toepassing.


## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/firmplay-tutorial/tutorial_general_01.png
[2]: ./media/firmplay-tutorial/tutorial_general_02.png
[3]: ./media/firmplay-tutorial/tutorial_general_03.png
[4]: ./media/firmplay-tutorial/tutorial_general_04.png

[100]: ./media/firmplay-tutorial/tutorial_general_100.png

[200]: ./media/firmplay-tutorial/tutorial_general_200.png
[201]: ./media/firmplay-tutorial/tutorial_general_201.png
[202]: ./media/firmplay-tutorial/tutorial_general_202.png
[203]: ./media/firmplay-tutorial/tutorial_general_203.png