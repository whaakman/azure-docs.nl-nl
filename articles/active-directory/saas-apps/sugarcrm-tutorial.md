---
title: 'Zelfstudie: Azure Active Directory-integratie met CRM suiker | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en suiker CRM.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 3331b9fc-ebc0-4a3a-9f7b-bf20ee35d180
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9a00c9cb0ce1bd8a6f36070e81df1185ef23a307
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56201762"
---
# <a name="tutorial-azure-active-directory-integration-with-sugar-crm"></a>Zelfstudie: Azure Active Directory-integratie met CRM suiker

In deze zelfstudie leert u hoe u suiker CRM integreren met Azure Active Directory (Azure AD).

Suiker CRM integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot suiker CRM heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij suiker CRM (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met CRM suiker, moet u de volgende items:

- Een Azure AD-abonnement
- Een suiker CRM eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Suiker CRM uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-sugar-crm-from-the-gallery"></a>Suiker CRM uit de galerie toe te voegen
Voor het configureren van de integratie van suiker CRM in Azure AD, moet u suiker CRM uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen suiker CRM vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **suiker CRM**.

    ![Het maken van een Azure AD-testgebruiker](./media/sugarcrm-tutorial/tutorial_sugarcrm_search.png)

1. Selecteer in het deelvenster resultaten **suiker CRM**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/sugarcrm-tutorial/tutorial_sugarcrm_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met suiker CRM op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in suiker CRM is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in suiker CRM tot stand worden gebracht.

In de CRM-suiker, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met suiker CRM, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker suiker CRM](#creating-a-sugar-crm-test-user)**  : als u wilt een equivalent van Britta Simon in suiker CRM die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw suiker CRM-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met suiker CRM, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **suiker CRM** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/sugarcrm-tutorial/tutorial_sugarcrm_samlbase.png)

1. Op de **suiker CRM-domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/sugarcrm-tutorial/tutorial_sugarcrm_url.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met het volgende patroon:
    | |
    |--|
    | `https://<companyname>.sugarondemand.com` |
    | `https://<companyname>.trial.sugarcrm` |

    > [!NOTE] 
    > De waarde is niet echt. Werk de waarde bij met de werkelijke aanmeldings-URL. Neem contact op met [suiker CRM-Client-ondersteuningsteam](https://support.sugarcrm.com/) om de waarde. 
 
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/sugarcrm-tutorial/tutorial_sugarcrm_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/sugarcrm-tutorial/tutorial_general_400.png)

1. Op de **suiker CRM-configuratie** sectie, klikt u op **configureren suiker CRM** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/sugarcrm-tutorial/tutorial_sugarcrm_configure.png) 

1. In een ander browservenster aanmelden bij uw bedrijf suiker CRM site als beheerder.

1. Ga naar **Admin**.
   
    ![Admin](./media/sugarcrm-tutorial/ic795888.png "Admin")

1. In de **beheer** sectie, klikt u op **wachtwoordbeheer**.
   
    ![Beheer](./media/sugarcrm-tutorial/ic795889.png "Beheer")

1. Selecteer **SAML-verificatie inschakelen**.
   
    ![Beheer](./media/sugarcrm-tutorial/ic795890.png "Beheer")

1. In de **SAML-verificatie** sectie, voert u de volgende stappen uit:
   
    ![SAML-verificatie](./media/sugarcrm-tutorial/ic795891.png "SAML-verificatie")  
 
    a. In de **aanmeldings-URL** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML**, die u hebt gekopieerd vanuit Azure portal.
  
    b. In de **SLO URL** tekstvak, plak de waarde van **afmelding URL**, die u hebt gekopieerd vanuit Azure portal.
  
    c. Open uw base-64 gecodeerde certificaat in Kladblok, Kopieer de inhoud ervan in het Klembord en plakt u het gehele certificaat in **X.509-certificaat** tekstvak.
  
    d. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/sugarcrm-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/sugarcrm-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/sugarcrm-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/sugarcrm-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-sugar-crm-test-user"></a>Het maken van een testgebruiker suiker CRM

Als u wilt dat Azure AD-gebruikers zich aanmelden bij suiker CRM, moeten ze worden ingericht op suiker CRM.

In het geval van CRM-suiker is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Meld u aan bij uw **suiker CRM** bedrijf site als administrator.

1. Ga naar **Admin**.
   
    ![Admin](./media/sugarcrm-tutorial/ic795888.png "Admin")

1. In de **beheer** sectie, klikt u op **Gebruikersbeheer**.
   
    ![Beheer](./media/sugarcrm-tutorial/ic795893.png "Beheer")

1. Ga naar **gebruikers \> nieuwe gebruiker maken**.
   
    ![Nieuwe gebruiker maken](./media/sugarcrm-tutorial/ic795894.png "nieuwe gebruiker maken")

1. Op de **gebruikersprofiel** tabblad, voert u de volgende stappen uit:
   
    ![New User](./media/sugarcrm-tutorial/ic795895.png "New User")

    a. Type de **gebruikersnaam**, **achternaam**, en **e-mailadres** van een geldige Azure Active Directory-gebruiker in de bijbehorende tekstvakken.
  
1. Als **Status**, selecteer **Active**.

1. Voer de volgende stappen uit op het tabblad wachtwoord:
   
    ![New User](./media/sugarcrm-tutorial/ic795896.png "New User")

    a. Typ het wachtwoord in het bijbehorende tekstvak.

    b. Klik op **Opslaan**.

>[!NOTE]
>Kunt u alle andere suiker CRM gebruiker-account maken van hulpprogramma's of API's die door CRM suiker inrichten AAD-gebruikersaccounts. 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan suiker CRM.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon suiker CRM, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **suiker CRM**.

    ![Eenmalige aanmelding configureren](./media/sugarcrm-tutorial/tutorial_sugarcrm_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie via het toegangsvenster.

Wanneer u op de tegel suiker CRM in het toegangsvenster, u moet u automatisch aangemeld bij uw suiker CRM-toepassing.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/sugarcrm-tutorial/tutorial_general_01.png
[2]: ./media/sugarcrm-tutorial/tutorial_general_02.png
[3]: ./media/sugarcrm-tutorial/tutorial_general_03.png
[4]: ./media/sugarcrm-tutorial/tutorial_general_04.png

[100]: ./media/sugarcrm-tutorial/tutorial_general_100.png

[200]: ./media/sugarcrm-tutorial/tutorial_general_200.png
[201]: ./media/sugarcrm-tutorial/tutorial_general_201.png
[202]: ./media/sugarcrm-tutorial/tutorial_general_202.png
[203]: ./media/sugarcrm-tutorial/tutorial_general_203.png

