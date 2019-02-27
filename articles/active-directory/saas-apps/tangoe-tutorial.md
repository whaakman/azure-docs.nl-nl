---
title: 'Zelfstudie: Azure Active Directory-integratie met Tangoe opdracht Premium mobiele | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Tangoe opdracht Premium Mobile.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 2b0b544c-9c2c-49cd-862b-ec2ee9330126
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8c9f410fa890df7aac3c3bf4d89468b92e69ba38
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56883229"
---
# <a name="tutorial-azure-active-directory-integration-with-tangoe-command-premium-mobile"></a>Zelfstudie: Azure Active Directory-integratie met Tangoe opdracht Premium Mobile

In deze zelfstudie leert u hoe u Tangoe opdracht Premium Mobile integreren met Azure Active Directory (Azure AD).

Tangoe opdracht Premium Mobile integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Tangoe opdracht Premium Mobile heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Tangoe opdracht Premium Mobile (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Tangoe opdracht Premium mobiele, moet u de volgende items:

- Een Azure AD-abonnement
- Een Tangoe opdracht Premium Mobile eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Tangoe opdracht Premium Mobile uit de galerie toevoegen
1. Azure AD-eenmalige aanmelding configureren en testen

## <a name="add-tangoe-command-premium-mobile-from-the-gallery"></a>Tangoe opdracht Premium Mobile uit de galerie toevoegen
Voor het configureren van de integratie van Tangoe opdracht Premium Mobile in Azure AD, moet u Tangoe opdracht Premium Mobile uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Tangoe opdracht Premium Mobile uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **Tangoe opdracht Premium Mobile**, selecteer **Tangoe opdracht Premium Mobile** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Tangoe opdracht Premium Mobile uit galerie toevoegen](./media/tangoe-tutorial/tutorial_tangoe_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Tangoe opdracht Premium Mobile op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Tangoe opdracht Premium Mobile is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Tangoe opdracht Premium Mobile tot stand worden gebracht.

In Tangoe opdracht Premium Mobile, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Tangoe opdracht Premium mobiele, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker Tangoe opdracht Premium Mobile](#create-a-tangoe-command-premium-mobile-test-user)**  : als u wilt een equivalent van Britta Simon in Tangoe opdracht Premium mobiele die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Tangoe opdracht Premium Mobile.

**Voor het configureren van Azure AD eenmalige aanmelding met Tangoe opdracht Premium mobiele, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Tangoe opdracht Premium Mobile** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Op SAML gebaseerde aanmelding](./media/tangoe-tutorial/tutorial_tangoe_samlbase.png)

1. Op de **Tangoe opdracht Premium Mobile domein en URL's** sectie, voert u de volgende stappen uit:

    ![Tangoe opdracht Premium mobiele domein en URL 's](./media/tangoe-tutorial/tutorial_tangoe_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://sso.tangoe.com/sp/startSSO.ping?PartnerIdpId=<tenant issuer>&TARGET=<target page url>`

    b. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://sso.tangoe.com/sp/ACS.saml2`

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden met de werkelijke antwoord-URL en de aanmeldings-URL. Neem contact op met [Tangoe opdracht Premium Mobile-Client-ondersteuningsteam](https://www.tangoe.com/contact-us/) om deze waarden te verkrijgen. 

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Sectie voor SAML-handtekeningcertificaat](./media/tangoe-tutorial/tutorial_tangoe_certificate.png) 

1. Klik op de knop **Save**.

    ![De knop Opslaan](./media/tangoe-tutorial/tutorial_general_400.png)
    
1. Op de **Tangoe opdracht Premium mobiele configuratie** sectie, klikt u op **configureren Tangoe opdracht Premium Mobile** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Configuratiesectie Tangoe opdracht Premium Mobile](./media/tangoe-tutorial/tutorial_tangoe_configure.png) 

1. Als u eenmalige aanmelding configureren voor uw toepassing, neem contact op met uw [Tangoe opdracht Premium Mobile-Client-ondersteuningsteam](https://www.tangoe.com/contact-us/) en geeft u het volgende:

   - Het gedownloade metagegevensbestand
   - De **SAML entiteit-ID**
   - De **URL voor SAML-Service voor eenmalige aanmelding**
   - De **de afmeld-URL**

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/tangoe-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Gebruikers en groepen -> alle gebruikers](./media/tangoe-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Gebruiker toevoegen](./media/tangoe-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Dialoogvenster op de gebruikerspagina](./media/tangoe-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="create-a-tangoe-command-premium-mobile-test-user"></a>Maak een testgebruiker Tangoe opdracht Premium Mobile

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Tangoe opdracht Premium Mobile. 

Tangoe opdracht Premium mobiele toepassing moet alle gebruikers in te richten in de toepassing voordat u eenmalige aanmelding. Zo kunt u werken met de [Tangoe opdracht Premium Mobile-Client-ondersteuningsteam](https://www.tangoe.com/contact-us/) voor het inrichten van al deze gebruikers in de toepassing. 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen op Tangoe opdracht Premium Mobile.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon Tangoe opdracht Premium Mobile, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Tangoe opdracht Premium Mobile**.

    ![Tangoe opdracht Premium mobiel in de lijst met Apps](./media/tangoe-tutorial/tutorial_tangoe_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u de configuratie van de Azure AD-eenmalige aanmelding via het toegangsvenster.

Wanneer u op de tegel Tangoe opdracht Premium Mobile in het toegangsvenster, u moet u automatisch aangemeld bij uw Tangoe opdracht Premium mobiele toepassing. Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/tangoe-tutorial/tutorial_general_01.png
[2]: ./media/tangoe-tutorial/tutorial_general_02.png
[3]: ./media/tangoe-tutorial/tutorial_general_03.png
[4]: ./media/tangoe-tutorial/tutorial_general_04.png

[100]: ./media/tangoe-tutorial/tutorial_general_100.png

[200]: ./media/tangoe-tutorial/tutorial_general_200.png
[201]: ./media/tangoe-tutorial/tutorial_general_201.png
[202]: ./media/tangoe-tutorial/tutorial_general_202.png
[203]: ./media/tangoe-tutorial/tutorial_general_203.png

