---
title: 'Zelfstudie: Azure Active Directory-integratie met LogicMonitor | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en LogicMonitor.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 496156c3-0e22-4492-b36f-2c29c055e087
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2018
ms.author: jeedes
ms.openlocfilehash: 5aa11c15e038c0af8447debba0cd385c944c0bce
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54812417"
---
# <a name="tutorial-azure-active-directory-integration-with-logicmonitor"></a>Zelfstudie: Azure Active Directory-integratie met LogicMonitor

In deze zelfstudie leert u hoe u LogicMonitor integreren met Azure Active Directory (Azure AD).

LogicMonitor integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot LogicMonitor heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij LogicMonitor (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met LogicMonitor, moet u de volgende items:

- Een Azure AD-abonnement
- Een LogicMonitor eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. LogicMonitor uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-logicmonitor-from-the-gallery"></a>LogicMonitor uit de galerie toe te voegen
Voor het configureren van de integratie van LogicMonitor in Azure AD, moet u LogicMonitor uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen LogicMonitor uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **LogicMonitor**.

    ![Het maken van een Azure AD-testgebruiker](./media/logicmonitor-tutorial/tutorial_logicmonitor_search.png)

1. Selecteer in het deelvenster resultaten **LogicMonitor**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/logicmonitor-tutorial/tutorial_logicmonitor_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met LogicMonitor op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in LogicMonitor is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in LogicMonitor tot stand worden gebracht.

In LogicMonitor, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met LogicMonitor, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker LogicMonitor](#creating-a-logicmonitor-test-user)**  : als u wilt een equivalent van Britta Simon in LogicMonitor die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing LogicMonitor.

**Voor het configureren van Azure AD eenmalige aanmelding met LogicMonitor, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **LogicMonitor** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/logicmonitor-tutorial/tutorial_logicmonitor_samlbase.png)

1. Op de **LogicMonitor domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/logicmonitor-tutorial/tutorial_logicmonitor_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<companyname>.logicmonitor.com`

    b. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<companyname>.logicmonitor.com`

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de daadwerkelijke aanmeldings-URL en id. Neem contact op met [LogicMonitor Client ondersteuningsteam](https://www.logicmonitor.com/contact/) om deze waarden te verkrijgen. 
 


1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/logicmonitor-tutorial/tutorial_logicmonitor_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/logicmonitor-tutorial/tutorial_general_400.png)

1. Meld u aan bij uw **LogicMonitor** bedrijf site als beheerder.

1. Klik in het menu aan de bovenkant op **instellingen**.
   
    ![Settings](./media/logicmonitor-tutorial/ic790052.png "Settings")

1. Klik in de navigatie-bat aan de linkerkant, **eenmalige aanmelding**
   
    ![Single Sign-On](./media/logicmonitor-tutorial/ic790053.png "Single Sign-On")

1. In de **instellingen voor eenmalige aanmelding (SSO)** sectie, voert u de volgende stappen uit:
   
    ![Single Sign-On instellingen](./media/logicmonitor-tutorial/ic790054.png "Single Sign-On-instellingen")
   
    a. Selecteer **eenmalige aanmelding inschakelen**.

    b. Als **roltoewijzing standaard**, selecteer **readonly**.
   
    c. Open het gedownloade metagegevensbestand in Kladblok en plak de inhoud van het bestand in de **Identity Provider metagegevens** tekstvak.
   
    d. Klik op **Wijzigingen opslaan**.

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/logicmonitor-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/logicmonitor-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/logicmonitor-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/logicmonitor-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-logicmonitor-test-user"></a>Het maken van een testgebruiker LogicMonitor

Voor Azure AD-gebruikers kunnen zich aanmelden, moeten ze worden ingericht voor de LogicMonitor-toepassing met behulp van hun Azure Active Directory-gebruikersnamen.

**Voer de volgende stappen uit om de gebruikersinrichting te configureren:**

1. Meld u aan bij uw bedrijf LogicMonitor site aan als beheerder.

1. Klik in het menu aan de bovenkant op **instellingen**, en klik vervolgens op **rollen en gebruikers**.
   
    ![Rollen en gebruikers](./media/logicmonitor-tutorial/ic790056.png "rollen en gebruikers")

1. Klik op **Add**.

1. In de **een account toevoegen** sectie, voert u de volgende stappen uit:
   
    ![Een account toevoegen](./media/logicmonitor-tutorial/ic790057.png "een account toevoegen")
   
    a. Type de **gebruikersnaam**, **e**, **wachtwoord**, en **Typ opnieuw wachtwoord** waarden van de Azure Active Directory-gebruiker die u inrichten wilt in de bijbehorende tekstvakken.
   
    b. Selecteer **rollen**, **machtigingen weergeven**, en de **Status**.
   
    c. Klik op **Submit**

>[!NOTE]
>U kunt elke andere LogicMonitor gebruiker account hulpmiddelen voor het maken of API's geleverd door LogicMonitor voor het inrichten van Azure Active Directory-gebruikersaccounts. 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan LogicMonitor.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan LogicMonitor toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **LogicMonitor**.

    ![Eenmalige aanmelding configureren](./media/logicmonitor-tutorial/tutorial_logicmonitor_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.
 
Wanneer u op de tegel LogicMonitor in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing LogicMonitor.
Zie [Introduction to the Access Panel](../active-directory-saas-access-panel-introduction.md) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster. 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/logicmonitor-tutorial/tutorial_general_01.png
[2]: ./media/logicmonitor-tutorial/tutorial_general_02.png
[3]: ./media/logicmonitor-tutorial/tutorial_general_03.png
[4]: ./media/logicmonitor-tutorial/tutorial_general_04.png

[100]: ./media/logicmonitor-tutorial/tutorial_general_100.png

[200]: ./media/logicmonitor-tutorial/tutorial_general_200.png
[201]: ./media/logicmonitor-tutorial/tutorial_general_201.png
[202]: ./media/logicmonitor-tutorial/tutorial_general_202.png
[203]: ./media/logicmonitor-tutorial/tutorial_general_203.png

