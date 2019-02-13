---
title: 'Zelfstudie: Azure Active Directory-integratie met Intacct | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Intacct.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 92518e02-a62c-4b1b-a8e9-2803eb2b49ac
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f5d30bae699a69bed893e36e24b6f8370db4f8b1
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56167096"
---
# <a name="tutorial-azure-active-directory-integration-with-intacct"></a>Zelfstudie: Azure Active Directory-integratie met Intacct

In deze zelfstudie leert u hoe u Intacct integreren met Azure Active Directory (Azure AD).

Intacct integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Intacct heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Intacct (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Intacct, moet u de volgende items:

- Een Azure AD-abonnement
- Een Intacct eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Intacct uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-intacct-from-the-gallery"></a>Intacct uit de galerie toe te voegen
Voor het configureren van de integratie van Intacct in Azure AD, moet u Intacct uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Intacct uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **Intacct**.

    ![Het maken van een Azure AD-testgebruiker](./media/intacct-tutorial/tutorial_intacct_search.png)

1. Selecteer in het deelvenster resultaten **Intacct**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/intacct-tutorial/tutorial_intacct_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Intacct op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Intacct is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Intacct tot stand worden gebracht.

In Intacct, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Intacct, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker Intacct](#creating-an-intacct-test-user)**  : als u wilt een equivalent van Britta Simon in Intacct die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Intacct.

**Voor het configureren van Azure AD eenmalige aanmelding met Intacct, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Intacct** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/intacct-tutorial/tutorial_intacct_samlbase.png)

1. Op de **Intacct domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/intacct-tutorial/tutorial_intacct_url.png)

    In het tekstvak **Antwoord-URL** typt u een URL met de volgende notatie:
    | |
    |--|
    | `https://<companyname>.intacct.com/ia/acct/sso_response.phtml`|
    | `https://www.intacct.com/ia/acct/sso_response.phtml` |

    > [!NOTE] 
    > Deze waarde is niet echt. Deze waarde bijwerken met de werkelijke antwoord-URL. Neem contact op met [Intacct ondersteuningsteam](https://us.intacct.com/support) deze waarde op te halen.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/intacct-tutorial/tutorial_intacct_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/intacct-tutorial/tutorial_general_400.png)

1. Op de **Intacct configuratie** sectie, klikt u op **configureren Intacct** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/intacct-tutorial/tutorial_intacct_configure.png) 

1. In een ander browservenster aanmelden bij uw bedrijf Intacct site als beheerder.

1. Klik op de **bedrijf** tabblad en klik vervolgens op **bedrijfsgegevens**.

    ![Bedrijf](./media/intacct-tutorial/ic790037.png "bedrijf")

1. Klik op de **Security** tabblad en klik vervolgens op **bewerken**.

    ![Beveiliging](./media/intacct-tutorial/ic790038.png "Beveiliging")

1. In de **eenmalige aanmelding (SSO)** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding](./media/intacct-tutorial/ic790039.png "eenmalige aanmelding")

    a. Selecteer **eenmalige aanmelding inschakelen**.

    b. Als **type id-provider**, selecteer **SAML 2.0**.

    c. In **URL-verlener** tekstvak, plak de waarde van **SAML entiteit-ID** die u hebt gekopieerd vanuit Azure portal.
   
    d. In **aanmeldings-URL** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML** die u hebt gekopieerd vanuit Azure portal.

    e. Open uw **base-64** gecodeerd certificaat in Kladblok, Kopieer de inhoud ervan in het Klembord en plakt u deze naar de **certificaat** vak.
   
    f. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/intacct-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/intacct-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/intacct-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/intacct-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-an-intacct-test-user"></a>Het maken van een testgebruiker Intacct

Als u Azure AD-gebruikers instelt, zodat ze kunnen zich aanmelden bij Intacct, moeten ze worden ingericht voor Intacct. Voor Intacct is het inrichten van een handmatige taak.

**Voor het inrichten van gebruikersaccounts, moet u de volgende stappen uitvoeren:**

1. Aanmelden bij uw **Intacct** tenant.

1. Klik op de **bedrijf** tabblad en klik vervolgens op **gebruikers**.

    ![Gebruikers](./media/intacct-tutorial/ic790041.png "Gebruikers")
1. Klik op de **toevoegen** tabblad.

    ![Add](./media/intacct-tutorial/ic790042.png "Add")
1. In de **gebruikersgegevens** sectie, voert u de volgende stappen uit:

    ![Gebruikersgegevens](./media/intacct-tutorial/ic790043.png "gebruikersgegevens")

    a. Voer de **gebruikers-ID**, wordt de **achternaam**, **voornaam**, wordt de **e-mailadres**, wordt de **titel**, en de **Phone** van een Azure AD-account dat u wilt inrichten in de **gebruikersgegevens** sectie.

    b. Selecteer de **beheerdersbevoegdheden** van een Azure AD-account dat u inrichten wilt.
   
    c. Klik op **Opslaan**. De houder van Azure AD-account ontvangt een e-mailbericht en volgt een koppeling om te bevestigen van hun account voordat deze geactiveerd wordt.

>[!NOTE]
>U kunt andere Intacct gebruiker-account maken-hulpprogramma's of API's die worden geleverd door Intacct gebruiken voor het inrichten van gebruikersaccounts van de Azure AD.
        
### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Intacct.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan Intacct toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Intacct**.

    ![Eenmalige aanmelding configureren](./media/intacct-tutorial/tutorial_intacct_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw configuratie Azure AD eenmalige aanmelding testen met behulp van het toegangsvenster.

Wanneer u op de tegel Intacct in het toegangsvenster, moet u worden automatisch aangemeld bij uw toepassing Intacct.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/intacct-tutorial/tutorial_general_01.png
[2]: ./media/intacct-tutorial/tutorial_general_02.png
[3]: ./media/intacct-tutorial/tutorial_general_03.png
[4]: ./media/intacct-tutorial/tutorial_general_04.png

[100]: ./media/intacct-tutorial/tutorial_general_100.png

[200]: ./media/intacct-tutorial/tutorial_general_200.png
[201]: ./media/intacct-tutorial/tutorial_general_201.png
[202]: ./media/intacct-tutorial/tutorial_general_202.png
[203]: ./media/intacct-tutorial/tutorial_general_203.png

