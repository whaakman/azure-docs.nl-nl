---
title: 'Zelfstudie: Azure Active Directory-integratie met Zscaler persoonlijke toegang (ZPA) | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Zscaler persoonlijke toegang (ZPA).
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 83711115-1c4f-4dd7-907b-3da24b37c89e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: jeedes
ms.openlocfilehash: 8d150a91a00652d5af74253dfdf60eb728db97b5
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55192825"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-private-access-zpa"></a>Zelfstudie: Azure Active Directory-integratie met Zscaler persoonlijke toegang (ZPA)

In deze zelfstudie leert u hoe u Zscaler persoonlijke toegang (ZPA) integreren met Azure Active Directory (Azure AD).

Zscaler persoonlijke toegang (ZPA) integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang hebben naar Zscaler persoonlijke toegang (ZPA)
- U kunt uw gebruikers automatisch ophalen aangemeld op Zscaler persoonlijke toegang (ZPA) (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - de Azure Management portal beheren

Zie [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Zscaler persoonlijke toegang (ZPA), moet u de volgende items:

- Een Azure AD-abonnement
- Een Zscaler persoonlijke toegang (ZPA) eenmalige aanmelding ingeschakeld abonnement


> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- U moet uw productie-omgeving, niet gebruiken als dit nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.


## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Zscaler persoonlijke toegang (ZPA) uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-zscaler-private-access-zpa-from-the-gallery"></a>Zscaler persoonlijke toegang (ZPA) uit de galerie toe te voegen
Voor het configureren van de integratie van de Zscaler persoonlijke toegang (ZPA) in Azure AD, moet u Zscaler persoonlijke toegang (ZPA) uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Zscaler persoonlijke toegang (ZPA) uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure Management Portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Klik op **toevoegen** knop boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **Zscaler persoonlijke toegang (ZPA)**.

    ![Het maken van een Azure AD-testgebruiker](./media/zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_001.png)

1. Selecteer in het deelvenster resultaten **Zscaler persoonlijke toegang (ZPA)**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Zscaler persoonlijke toegang (ZPA) op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Zscaler persoonlijke toegang (ZPA) is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Zscaler persoonlijke toegang (ZPA) tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door toe te wijzen de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Zscaler persoonlijke toegang (ZPA).

Om te configureren en testen van Azure AD eenmalige aanmelding met Zscaler persoonlijke toegang (ZPA), moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker Zscaler persoonlijke toegang (ZPA)](#creating-a-zscaler-private-access-(zpa)-test-user)**  : als u wilt een equivalent van Britta Simon in Zscaler persoonlijke toegang (ZPA) die is gekoppeld aan de Azure AD-weergave van haar zijn.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u Azure AD eenmalige aanmelding in de Azure Management portal inschakelen en configureren van eenmalige aanmelding in uw toepassing Zscaler persoonlijke toegang (ZPA).

**Voor het configureren van Azure AD eenmalige aanmelding met Zscaler persoonlijke toegang (ZPA), moet u de volgende stappen uitvoeren:**

1. In de Azure-beheerportal, op de **Zscaler persoonlijke toegang (ZPA)** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster als **modus** Selecteer **SAML gebaseerde aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/zscalerprivateaccess-tutorial/tutorial_general_300.png)
    
1. Op de **Zscaler persoonlijke toegang (ZPA)-domein en URL's** sectie, voert u de volgende stappen uit:
    
    ![Eenmalige aanmelding configureren](./media/zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_01.png)

    a. In de **aanmelding URL** tekstvak, een URL met behulp van het volgende patroon: `https://samlsp.private.zscaler.com/auth/login?domain=<your-domain-name>`

    b. In de **id** tekstvak, type: `https://samlsp.private.zscaler.com/auth/metadata`

    > [!NOTE] 
    > Houd er rekening mee dat dit niet de werkelijke waarden zijn. U hebt deze waarden bijwerken met de werkelijke aanmelding URL en -id. Hier stellen we voor u voor het gebruik van de unieke waarde van de URL in de id. Neem contact op met [Zscaler persoonlijke toegang (ZPA)-ondersteuningsteam](https://help.zscaler.com/zpa-submit-ticket) om deze waarden te verkrijgen.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **nieuw certificaat maken**.

    ![Eenmalige aanmelding configureren](./media/zscalerprivateaccess-tutorial/tutorial_general_400.png)     

1. Op de **nieuw certificaat maken** dialoogvenster, klikt u op het kalenderpictogram en selecteer een **vervaldatum**. Klik vervolgens op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/zscalerprivateaccess-tutorial/tutorial_general_500.png)

1. Op de **SAML-handtekeningcertificaat** sectie, selecteer **nieuw certificaat activeren** en klikt u op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_02.png)

1. In het pop-upvenster **rollovercertificaat** venster, klikt u op **OK**.

    ![Eenmalige aanmelding configureren](./media/zscalerprivateaccess-tutorial/tutorial_general_600.png)

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_03.png) 

1. Meld u in een ander browservenster in uw bedrijf Zscaler persoonlijke toegang (ZPA) site als beheerder.

1. Navigeer naar **beheerder** en klik vervolgens op **Idp-configuratie**.

    ![Eenmalige aanmelding aan app-zijde configureren](./media/zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_04.png)

1. In de **Idp-configuratie** sectie, klikt u op **nieuwe IDP-configuratie toevoegen**.

    ![Eenmalige aanmelding aan app-zijde configureren](./media/zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_05.png)

1. In de **nieuwe IDP-configuratie** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding aan app-zijde configureren](./media/zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_06.png)

    a. Klik op **bestand selecteren** en uw gedownloade metagegevensbestand uploaden.

    b. Klik op de knop **Save**.
    


### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in de Azure Management portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure Management portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/zscalerprivateaccess-tutorial/create_aaduser_01.png) 

1. Ga naar **gebruikers en groepen** en klikt u op **alle gebruikers** om de lijst met gebruikers weer te geven.
    
    ![Het maken van een Azure AD-testgebruiker](./media/zscalerprivateaccess-tutorial/create_aaduser_02.png) 

1. Aan de bovenkant van het dialoogvenster klikt u op **toevoegen** openen de **gebruiker** dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/zscalerprivateaccess-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/zscalerprivateaccess-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**. 



### <a name="creating-a-zscaler-private-access-zpa-test-user"></a>Het maken van een testgebruiker Zscaler persoonlijke toegang (ZPA)

In deze sectie maakt u een gebruiker met de naam Britta Simon in Zscaler persoonlijke toegang (ZPA). Neem contact op met [Zscaler persoonlijke toegang (ZPA)-ondersteuningsteam](https://help.zscaler.com/zpa-submit-ticket) om toe te voegen de gebruikers in het platform Zscaler persoonlijke toegang (ZPA).


### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding in haar door toegang te verlenen aan Zscaler persoonlijke toegang (ZPA).

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon naar Zscaler persoonlijke toegang (ZPA), moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-beheerportal, en vervolgens gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Zscaler persoonlijke toegang (ZPA)**.

    ![Eenmalige aanmelding configureren](./media/zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_50.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    


### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Zscaler persoonlijke toegang (ZPA) in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Zscaler persoonlijke toegang (ZPA).


## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/zscalerprivateaccess-tutorial/tutorial_general_01.png
[2]: ./media/zscalerprivateaccess-tutorial/tutorial_general_02.png
[3]: ./media/zscalerprivateaccess-tutorial/tutorial_general_03.png
[4]: ./media/zscalerprivateaccess-tutorial/tutorial_general_04.png

[100]: ./media/zscalerprivateaccess-tutorial/tutorial_general_100.png

[200]: ./media/zscalerprivateaccess-tutorial/tutorial_general_200.png
[201]: ./media/zscalerprivateaccess-tutorial/tutorial_general_201.png
[202]: ./media/zscalerprivateaccess-tutorial/tutorial_general_202.png
[203]: ./media/zscalerprivateaccess-tutorial/tutorial_general_203.png
