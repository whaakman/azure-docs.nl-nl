---
title: 'Zelfstudie: Azure Active Directory-integratie met SensoScientific draadloze temperatuur Monitoring System | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory- en bewakingssysteem voor SensoScientific draadloze temperatuur.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: ee9a924d-ccde-45b0-ab40-877f82f5dfa2
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 33a625e82f41bee1b8e3980192076d24a7471953
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56183408"
---
# <a name="tutorial-azure-active-directory-integration-with-sensoscientific-wireless-temperature-monitoring-system"></a>Zelfstudie: Azure Active Directory-integratie met bewakingssysteem voor SensoScientific draadloze temperatuur

In deze zelfstudie leert u hoe u SensoScientific draadloze temperatuur-bewakingssysteem integreren met Azure Active Directory (Azure AD).

SensoScientific draadloze temperatuur-bewakingssysteem integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot bewakingssysteem voor SensoScientific draadloze temperatuur heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij SensoScientific draadloze temperatuur Monitoring System (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met bewakingssysteem voor SensoScientific draadloze temperatuur, moet u de volgende items:

- Een Azure AD-abonnement
- Een bewakingssysteem voor SensoScientific draadloze temperatuur eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SensoScientific draadloze temperatuur Monitoring System uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-sensoscientific-wireless-temperature-monitoring-system-from-the-gallery"></a>SensoScientific draadloze temperatuur Monitoring System uit de galerie toe te voegen
Voor het configureren van de integratie van bewakingssysteem voor SensoScientific draadloze temperatuur in Azure AD, moet u SensoScientific draadloze temperatuur Monitoring System uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SensoScientific draadloze temperatuur Monitoring System uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **SensoScientific draadloze temperatuur-bewakingssysteem**.

    ![Het maken van een Azure AD-testgebruiker](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_search.png)

1. Selecteer in het deelvenster resultaten **SensoScientific draadloze temperatuur-bewakingssysteem**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met SensoScientific draadloze temperatuur bewakingssysteem op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in SensoScientific draadloze temperatuur Monitoring System is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in bewakingssysteem voor SensoScientific draadloze temperatuur tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door toe te wijzen de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in SensoScientific draadloze temperatuur-bewakingssysteem.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met bewakingssysteem voor SensoScientific draadloze temperatuur, u nodig hebt voor de volgende bouwstenen:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker bewakingssysteem voor SensoScientific draadloze temperatuur](#creating-a-sensoscientific-wireless-temperature-monitoring-system-test-user)**  : als u wilt een equivalent van Britta Simon in SensoScientific draadloze temperatuur bewakingssysteem hebben die is gekoppeld aan de Azure AD-weergave van de gebruiker.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing SensoScientific draadloze temperatuur-bewakingssysteem.

**Voor het configureren van Azure AD eenmalige aanmelding met bewakingssysteem voor SensoScientific draadloze temperatuur, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **SensoScientific draadloze temperatuur-bewakingssysteem** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_samlbase.png)

1. Op de **SensoScientific draadloze temperatuur bewaking systeemdomein en URL's** sectie, niet nodig om u te werk als de app al vooraf geïntegreerd in Azure is:

    ![Eenmalige aanmelding configureren](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_url.png)

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/sensoscientific-tutorial/tutorial_general_400.png)

1. Op de **SensoScientific draadloze temperatuur bewaking systeemconfiguratie** sectie, klikt u op **configureren SensoScientific draadloze temperatuur bewakingssysteem** openen **configureren aanmeldings-** venster. Kopiëren de **URL van de afmelding, SAML-entiteit-ID** en **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_configure.png) 

1. Meld u aan bij uw toepassing bewakingssysteem voor SensoScientific draadloze temperatuur als beheerder.

1. Klik in het navigatiemenu aan de bovenkant op **configuratie** en Ga naar **configureren** onder **Single Sign On** om de één teken op instellingen te openen.

    ![Eenmalige aanmelding configureren](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_admin.png) 

1. In **één teken op instellingen** formulier de volgende stappen uitvoeren:
 
    a. Selecteer **Verlenernaam** als Azure AD.
    
    b. Plak de **SAML entiteit-ID** die u hebt gekopieerd vanuit Azure portal in het tekstvak URL-verlener.
    
    c. Plak de **Single Sign-On Service URL voor SAML** die u hebt gekopieerd vanuit Azure portal naar Single Sign-On Service URL-tekstvak.

    d. Plak de **afmelding URL** die u hebt gekopieerd vanuit Azure portal in het tekstvak URL voor eenmalige afmelding-Service.

    e. Blader door het certificaat dat u hebt gedownload vanuit Azure portal en upload het hier.
    
    f. Klik op **Opslaan**.
  
> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD](https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/sensoscientific-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/sensoscientific-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/sensoscientific-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/sensoscientific-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-sensoscientific-wireless-temperature-monitoring-system-test-user"></a>Het maken van een testgebruiker bewakingssysteem voor SensoScientific draadloze temperatuur

Om in te schakelen in Azure AD-gebruikers zich aanmelden bij bewakingssysteem voor SensoScientific draadloze temperatuur, moeten ze worden ingericht voor SensoScientific draadloze temperatuur-bewakingssysteem. Werken met [SensoScientific draadloze temperatuur Monitoring System-ondersteuningsteam](https://www.sensoscientific.com/contact-us/) om toe te voegen de gebruikers in het bewakingssysteem voor SensoScientific draadloze temperatuur-platform. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken. 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan SensoScientific draadloze temperatuur bewakingssysteem.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon SensoScientific draadloze temperatuur bewaking systeem, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **SensoScientific draadloze temperatuur-bewakingssysteem**.

    ![Eenmalige aanmelding configureren](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster. Klik op de tegel bewakingssysteem voor SensoScientific draadloze temperatuur in het toegangsvenster, u zal worden automatisch aangemeld bij uw toepassing SensoScientific draadloze temperatuur-bewakingssysteem. Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/sensoscientific-tutorial/tutorial_general_01.png
[2]: ./media/sensoscientific-tutorial/tutorial_general_02.png
[3]: ./media/sensoscientific-tutorial/tutorial_general_03.png
[4]: ./media/sensoscientific-tutorial/tutorial_general_04.png

[100]: ./media/sensoscientific-tutorial/tutorial_general_100.png

[200]: ./media/sensoscientific-tutorial/tutorial_general_200.png
[201]: ./media/sensoscientific-tutorial/tutorial_general_201.png
[202]: ./media/sensoscientific-tutorial/tutorial_general_202.png
[203]: ./media/sensoscientific-tutorial/tutorial_general_203.png

