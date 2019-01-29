---
title: 'Zelfstudie: Azure Active Directory-integratie met EthicsPoint Incident Management (EPIM) | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en EthicsPoint Incident Management (EPIM).
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 8cb31a4c-9309-469b-93ac-daf0d3c7a3e6
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 352af25d90e674d1dbe960932e3aeb97055f875b
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55184492"
---
# <a name="tutorial-azure-active-directory-integration-with-ethicspoint-incident-management-epim"></a>Zelfstudie: Azure Active Directory-integratie met EthicsPoint Incident Management (EPIM)

In deze zelfstudie leert u hoe u EthicsPoint Incident Management (EPIM) integreren met Azure Active Directory (Azure AD).

EthicsPoint Incident Management (EPIM) integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang hebben naar EthicsPoint Incident Management (EPIM)
- U kunt uw gebruikers automatisch ophalen aangemeld op EthicsPoint Incident Management (EPIM) (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met EthicsPoint Incident Management (EPIM), moet u de volgende items:

- Een Azure AD-abonnement
- Een EthicsPoint Incident Management (EPIM) eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. EthicsPoint Incident Management (EPIM) uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-ethicspoint-incident-management-epim-from-the-gallery"></a>EthicsPoint Incident Management (EPIM) uit de galerie toe te voegen
Voor het configureren van de integratie van EthicsPoint Incident Management (EPIM) in Azure AD, moet u EthicsPoint Incident Management (EPIM) uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen EthicsPoint Incident Management (EPIM) uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **EthicsPoint Incident Management (EPIM)**.

    ![Het maken van een Azure AD-testgebruiker](./media/ethicspoint-incident-management-tutorial/tutorial_ethicspoint_search.png)

1. Selecteer in het deelvenster resultaten **EthicsPoint Incident Management (EPIM)**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/ethicspoint-incident-management-tutorial/tutorial_ethicspoint_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met EthicsPoint Incident Management (EPIM) op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in EthicsPoint Incident Management (EPIM) is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in EthicsPoint Incident Management (EPIM) tot stand worden gebracht.

In EthicsPoint Incident Management (EPIM), wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met EthicsPoint Incident Management (EPIM), moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker EthicsPoint Incident Management (EPIM)](#creating-a-ethicspoint-incident-management-epim-test-user)**  - hebben een equivalent van Britta Simon in EthicsPoint Incident Management (EPIM) die is gekoppeld aan de Azure AD-weergave van de gebruiker.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing EthicsPoint Incident Management (EPIM).

**Voor het configureren van Azure AD eenmalige aanmelding met EthicsPoint Incident Management (EPIM), moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **EthicsPoint Incident Management (EPIM)** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/ethicspoint-incident-management-tutorial/tutorial_ethicspoint_samlbase.png)

1. Op de **EthicsPoint Incident Management (EPIM)-domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/ethicspoint-incident-management-tutorial/tutorial_ethicspoint_url.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met het volgende patroon:
    | |
    |--|
    | `https://<companyname>.navexglobal.com`|
    | `https://<companyname>.ethicspointvp.com`|

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<companyname>.navexglobal.com/adfs/services/trust`

    c. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://<servername>.navexglobal.com/adfs/ls/`

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden met de werkelijke antwoord-URL, id en aanmeldings-URL. Neem contact op met [EthicsPoint Incident Management (EPIM) Client-ondersteuningsteam](https://www.navexglobal.com/company/contact-us) om deze waarden te verkrijgen. 

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/ethicspoint-incident-management-tutorial/tutorial_ethicspoint_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/ethicspoint-incident-management-tutorial/tutorial_general_400.png)
    
1. Het configureren van eenmalige aanmelding op **EthicsPoint Incident Management (EPIM)** zijde, moet u voor het verzenden van de gedownloade **Metadata XML** naar [ondersteuningsteam EthicsPoint Incident Management (EPIM) ](https://www.navexglobal.com/company/contact-us).

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/ethicspoint-incident-management-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/ethicspoint-incident-management-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/ethicspoint-incident-management-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/ethicspoint-incident-management-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-ethicspoint-incident-management-epim-test-user"></a>Het maken van een testgebruiker EthicsPoint Incident Management (EPIM)

In deze sectie maakt u een gebruiker met de naam Britta Simon in EthicsPoint Incident Management (EPIM). Neem contact op met [EthicsPoint Incident Management (EPIM)-ondersteuningsteam](https://www.navexglobal.com/company/contact-us) om toe te voegen de gebruikers van het EthicsPoint Incident Management (EPIM)-platform.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan EthicsPoint Incident Management (EPIM).

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon naar EthicsPoint Incident Management (EPIM), moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **EthicsPoint Incident Management (EPIM)**.

    ![Eenmalige aanmelding configureren](./media/ethicspoint-incident-management-tutorial/tutorial_ethicspoint_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.
Wanneer u op de tegel EthicsPoint Incident Management (EPIM) in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing EthicsPoint Incident Management (EPIM).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/ethicspoint-incident-management-tutorial/tutorial_general_01.png
[2]: ./media/ethicspoint-incident-management-tutorial/tutorial_general_02.png
[3]: ./media/ethicspoint-incident-management-tutorial/tutorial_general_03.png
[4]: ./media/ethicspoint-incident-management-tutorial/tutorial_general_04.png

[100]: ./media/ethicspoint-incident-management-tutorial/tutorial_general_100.png

[200]: ./media/ethicspoint-incident-management-tutorial/tutorial_general_200.png
[201]: ./media/ethicspoint-incident-management-tutorial/tutorial_general_201.png
[202]: ./media/ethicspoint-incident-management-tutorial/tutorial_general_202.png
[203]: ./media/ethicspoint-incident-management-tutorial/tutorial_general_203.png

