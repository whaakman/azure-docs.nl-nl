---
title: 'Zelfstudie: Azure Active Directory-integratie met Skillport | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Skillport.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 4df349b2-a73f-4b88-a077-ec0fbfc26527
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 936ad2e49aaf449144296dd941a3fbc42719fe4f
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58879026"
---
# <a name="tutorial-azure-active-directory-integration-with-skillport"></a>Zelfstudie: Azure Active Directory-integratie met Skillport

In deze zelfstudie leert u hoe u Skillport integreren met Azure Active Directory (Azure AD).

Skillport integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Skillport heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Skillport (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Skillport, moet u de volgende items:

- Een Azure AD-abonnement
- Een Skillport eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Skillport uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-skillport-from-the-gallery"></a>Skillport uit de galerie toe te voegen
Voor het configureren van de integratie van Skillport in Azure AD, moet u Skillport uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Skillport uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Klik op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **Skillport**.

    ![Het maken van een Azure AD-testgebruiker](./media/skillport-tutorial/tutorial_skillport_search.png)

1. Selecteer in het deelvenster resultaten **Skillport**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/skillport-tutorial/tutorial_skillport_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Skillport op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Skillport is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Skillport tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door toe te wijzen de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Skillport.

Om te configureren en testen van Azure AD eenmalige aanmelding met Skillport, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker Skillport](#creating-a-skillport-test-user)**  : als u wilt een equivalent van Britta Simon in Skillport die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Skillport.

**Voor het configureren van Azure AD eenmalige aanmelding met Skillport, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Skillport** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/skillport-tutorial/tutorial_skillport_samlbase.png)

1. Op de **Skillport domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/skillport-tutorial/tutorial_skillport_url.png)

    a. Typ een URL in het tekstvak **Aanmeldings-URL**:
      
      EU Datacenter: `https://adfs.skillport.eu`
   
      VS Datacenter: `https://sso.skillport.com`

    b. Typ de URL in het tekstvak **Id**:
      
      EU Datacenter: `http://adfs.skillport.eu/adfs/services/trust`
   
      VS Datacenter: `https://sso.skillport.com`
   
    c. Typ de URL in het tekstvak **Antwoord-URL**:
    
      EU Datacenter: `https://adfs.skillport.eu/adfs/ls/`
    
      VS Datacenter: `https://sso.skillport.com/sp/ACS.saml2`
 
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het XML-bestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/skillport-tutorial/tutorial_skillport_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/skillport-tutorial/tutorial_general_400.png)

1. Het configureren van eenmalige aanmelding op **Skillport** zijde, moet u voor het verzenden van de gedownloade **Metadata XML** naar [Skillport ondersteuningsteam](https://www.skillsoft.com/contact.asp). Ze zal deze zodanig instellen dat voor de SAML SSO-verbinding instellen goed aan beide zijden.

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/skillport-tutorial/create_aaduser_01.png) 

1. Ga naar **gebruikers en groepen** en klikt u op **alle gebruikers** om de lijst met gebruikers weer te geven.
    
    ![Het maken van een Azure AD-testgebruiker](./media/skillport-tutorial/create_aaduser_02.png) 

1. Aan de bovenkant van het dialoogvenster, klikt u op **toevoegen** openen de **gebruiker** dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/skillport-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/skillport-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-skillport-test-user"></a>Het maken van een testgebruiker Skillport

Om te kunnen Skillport testgebruiker maken, moet u contact opnemen met [Skillport ondersteuningsteam](https://www.skillsoft.com/contact.asp) als ze beschikken over meerdere bedrijfsscenario's op basis van de vereiste van eindgebruiker. Ze configureert het na overleg met de gebruikers.  

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Skillport.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan Skillport toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Skillport**.

    ![Eenmalige aanmelding configureren](./media/skillport-tutorial/tutorial_skillport_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op de knop **Add**. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Skillport in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Skillport.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/skillport-tutorial/tutorial_general_01.png
[2]: ./media/skillport-tutorial/tutorial_general_02.png
[3]: ./media/skillport-tutorial/tutorial_general_03.png
[4]: ./media/skillport-tutorial/tutorial_general_04.png

[100]: ./media/skillport-tutorial/tutorial_general_100.png

[200]: ./media/skillport-tutorial/tutorial_general_200.png
[201]: ./media/skillport-tutorial/tutorial_general_201.png
[202]: ./media/skillport-tutorial/tutorial_general_202.png
[203]: ./media/skillport-tutorial/tutorial_general_203.png

