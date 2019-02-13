---
title: 'Zelfstudie: Azure Active Directory-integratie met de Jitbit Helpdesk | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Jitbit Helpdesk.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 15ce27d4-0621-4103-8a34-e72c98d72ec3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7cb752a6b598c9fe7f146cd6ce96182405fc0dc6
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56167674"
---
# <a name="tutorial-azure-active-directory-integration-with-jitbit-helpdesk"></a>Zelfstudie: Azure Active Directory-integratie met Jitbit Helpdesk

In deze zelfstudie leert u hoe u Jitbit Helpdesk integreren met Azure Active Directory (Azure AD).

Jitbit Helpdesk integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Jitbit Helpdesk heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij de Jitbit Helpdesk (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met de Jitbit Helpdesk, moet u de volgende items:

- Een Azure AD-abonnement
- Een Jitbit Helpdesk eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Jitbit Helpdesk uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-jitbit-helpdesk-from-the-gallery"></a>Jitbit Helpdesk uit de galerie toe te voegen
Voor het configureren van de integratie van Jitbit Helpdesk in Azure AD, moet u Jitbit Helpdesk uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Jitbit Helpdesk uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **Jitbit Helpdesk**.

    ![Het maken van een Azure AD-testgebruiker](./media/jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_search.png)

1. Selecteer in het deelvenster resultaten **Jitbit Helpdesk**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie configureert u en test Azure AD eenmalige aanmelding met de Jitbit Helpdesk op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Jitbit Helpdesk is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Jitbit Helpdesk tot stand worden gebracht.

In de Jitbit Helpdesk, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met de Jitbit Helpdesk, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker Jitbit Helpdesk](#creating-a-jitbit-helpdesk-test-user)**  : als u wilt een equivalent van Britta Simon in Jitbit Helpdesk die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Jitbit Helpdesk.

**Voor het configureren van Azure AD eenmalige aanmelding met de Jitbit Helpdesk, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Jitbit Helpdesk** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_samlbase.png)

1. Op de **Jitbit Helpdesk domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_url.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met het volgende patroon: 
    | |     
    | ----------------------------------------|
    | `https://<hostname>/helpdesk/User/Login`|
    | `https://<tenant-name>.Jitbit.com`|
    | |
    
    > [!NOTE] 
    > Deze waarde is niet echt. Werk deze waarde bij met de werkelijke aanmeldings-URL. Neem contact op met [Jitbit Helpdesk Client ondersteuningsteam](https://www.jitbit.com/support/) deze waarde op te halen. 
    
    b.  In de **id** tekstvak, een URL als volgt: `https://www.jitbit.com/web-helpdesk/`

    
 


1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/jitbit-helpdesk-tutorial/tutorial_general_400.png)

1. Op de **Jitbit Helpdesk configuratie** sectie, klikt u op **configureren Jitbit Helpdesk** openen **aanmelding configureren** venster. Kopiëren de **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_configure.png) 

1. Meld u in een ander browservenster in uw bedrijf Jitbit Helpdesk site als beheerder.

1. Klik in de werkbalk bovenaan op **beheer**.
   
    ![Beheer](./media/jitbit-helpdesk-tutorial/ic777681.png "Beheer")

1. Klik op **algemene instellingen**.
   
    ![Gebruikers, bedrijven en machtigingen](./media/jitbit-helpdesk-tutorial/ic777680.png "gebruikers, bedrijven en machtigingen")

1. In de **verificatie-instellingen** configuratie sectie, voert u de volgende stappen uit:
   
    ![Verificatie-instellingen](./media/jitbit-helpdesk-tutorial/ic777683.png "verificatie-instellingen")
    
    a. Selecteer **inschakelen-SAML één 2.0 Meld u aan bij**, aan te melden bij het gebruik van eenmalige aanmelding (SSO), met **OneLogin**.

    b. In de **eindpunt-URL** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML** die u hebt gekopieerd vanuit Azure portal.

    c. Open uw **base-64** gecodeerd certificaat in Kladblok, Kopieer de inhoud ervan in het Klembord en plakt u deze naar de **X.509-certificaat** tekstvak

    d. Klik op **wijzigingen opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/jitbit-helpdesk-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/jitbit-helpdesk-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/jitbit-helpdesk-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/jitbit-helpdesk-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak typenaam als **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-jitbit-helpdesk-test-user"></a>Het maken van een testgebruiker Jitbit Helpdesk

Om in te schakelen in Azure AD-gebruikers zich aanmelden bij de Jitbit Helpdesk, moeten ze worden ingericht voor Jitbit Helpdesk.  In het geval van Jitbit Helpdesk is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Meld u aan bij uw **Jitbit Helpdesk** tenant.

1. Klik in het menu aan de bovenkant op **beheer**.
   
    ![Beheer](./media/jitbit-helpdesk-tutorial/ic777681.png "Beheer")

1. Klik op **gebruikers, bedrijven en machtigingen**.
   
    ![Gebruikers, bedrijven en machtigingen](./media/jitbit-helpdesk-tutorial/ic777682.png "gebruikers, bedrijven en machtigingen")

1. Klik op **Gebruiker toevoegen**.
   
    ![Add user](./media/jitbit-helpdesk-tutorial/ic777685.png "Add user")
   
1. In de sectie maken, typt u de gegevens van de Azure AD-account dat u wilt inrichten gaat als volgt:

    ![Maak](./media/jitbit-helpdesk-tutorial/ic777686.png "maken")
   
   a. In de **gebruikersnaam** tekstvak, type **BrittaSimon**, zoals in de Azure-portal de naam van de gebruiker.

   b. In de **e** tekstvak type e-mailadres van de gebruiker, zoals **BrittaSimon@contoso.com**.

   c. In de **voornaam** tekstvak, type de voornaam van de gebruiker, zoals **Julia**.

   d. In de **achternaam** tekstvak, type achternaam van de gebruiker, zoals **Simon**.
   
   e. Klik op **Create**.

>[!NOTE]
>U kunt een andere Jitbit Helpdesk gebruiker-account maken-hulpprogramma's of API's die worden geleverd door de Jitbit Helpdesk gebruiken voor het inrichten van gebruikersaccounts van de Azure AD.
> 
        

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot de Jitbit Helpdesk.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon Jitbit Helpdesk, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Jitbit Helpdesk**.

    ![Eenmalige aanmelding configureren](./media/jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Jitbit Helpdesk in het toegangsvenster, krijgt u de aanmeldingspagina van Jitbit Helpdesk-toepassing.
Zie [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/jitbit-helpdesk-tutorial/tutorial_general_01.png
[2]: ./media/jitbit-helpdesk-tutorial/tutorial_general_02.png
[3]: ./media/jitbit-helpdesk-tutorial/tutorial_general_03.png
[4]: ./media/jitbit-helpdesk-tutorial/tutorial_general_04.png

[100]: ./media/jitbit-helpdesk-tutorial/tutorial_general_100.png

[200]: ./media/jitbit-helpdesk-tutorial/tutorial_general_200.png
[201]: ./media/jitbit-helpdesk-tutorial/tutorial_general_201.png
[202]: ./media/jitbit-helpdesk-tutorial/tutorial_general_202.png
[203]: ./media/jitbit-helpdesk-tutorial/tutorial_general_203.png

