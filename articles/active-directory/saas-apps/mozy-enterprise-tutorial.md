---
title: 'Zelfstudie: Azure Active Directory-integratie met Mozy Enterprise | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Mozy Enterprise.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 489b5e62-85c2-45c9-8766-326632d48114
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: jeedes
ms.openlocfilehash: 5bbaa90554e09d27a3c521d4a13eda44021721c8
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55176893"
---
# <a name="tutorial-azure-active-directory-integration-with-mozy-enterprise"></a>Zelfstudie: Azure Active Directory-integratie met Mozy Enterprise

In deze zelfstudie leert u hoe u Mozy Enterprise integreren met Azure Active Directory (Azure AD).

Mozy Enterprise integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Mozy Enterprise heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Mozy Enterprise (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Mozy Enterprise, moet u de volgende items:

- Een Azure AD-abonnement
- Een Mozy ondernemingsbrede eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Mozy Enterprise uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-mozy-enterprise-from-the-gallery"></a>Mozy Enterprise uit de galerie toe te voegen
Voor het configureren van de integratie van Mozy Enterprise in Azure AD, moet u Mozy Enterprise uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Mozy Enterprise uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **Mozy Enterprise**.

    ![Het maken van een Azure AD-testgebruiker](./media/mozy-enterprise-tutorial/tutorial_mozyenterprise_search.png)

1. Selecteer in het deelvenster resultaten **Mozy Enterprise**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/mozy-enterprise-tutorial/tutorial_mozyenterprise_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie configureert u en test Azure AD eenmalige aanmelding met Mozy Enterprise op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in de onderneming Mozy is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in de onderneming Mozy tot stand worden gebracht.

In de onderneming Mozy, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Mozy Enterprise, u nodig hebt voor de volgende bouwstenen:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker Mozy Enterprise](#creating-a-mozy-enterprise-test-user)**  : als u wilt een equivalent van Britta Simon in Mozy-onderneming die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Mozy Enterprise.

**Voor het configureren van Azure AD eenmalige aanmelding met Mozy Enterprise, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Mozy Enterprise** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/mozy-enterprise-tutorial/tutorial_mozyenterprise_samlbase.png)

1. Op de **Mozy Enterprise domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/mozy-enterprise-tutorial/tutorial_mozyenterprise_url.png)

    Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<tenantname>.Mozyenterprise.com`

    > [!NOTE] 
    > Deze waarde is niet echt. Deze waarde bijwerken met de werkelijke aanmeldings-URL. Neem contact op met [Mozy Enterprise Client-ondersteuningsteam](http://support.mozy.com/) deze waarde op te halen.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/mozy-enterprise-tutorial/tutorial_mozyenterprise_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/mozy-enterprise-tutorial/tutorial_general_400.png)

1. Op de **Mozy Ondernemingsconfiguratie** sectie, klikt u op **configureren Mozy Enterprise** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/mozy-enterprise-tutorial/tutorial_mozyenterprise_configure.png) 

1. Meld u in een ander browservenster in uw bedrijf Mozy Enterprise site als beheerder.

1. In de **configuratie** sectie, klikt u op **verificatiebeleid**.
   
   ![Verificatiebeleid](./media/mozy-enterprise-tutorial/ic777314.png "verificatiebeleid")

1. Op de **verificatiebeleid** sectie, voert u de volgende stappen uit:
   
   ![Verificatiebeleid](./media/mozy-enterprise-tutorial/ic777315.png "verificatiebeleid")
   
   a. Selecteer **adreslijstservice** als **Provider**.
   
   b. Selecteer **Gebruik LDAP-Push**.
   
   c. Klik op het tabblad **SAML-verificatie**.
   
   d. Plakken **Single Sign-On Service URL voor SAML**, die u hebt gekopieerd vanuit de Azure portal in de **-URL webverificatie** tekstvak.
   
   e. Plakken **SAML entiteit-ID**, die u hebt gekopieerd vanuit de Azure portal in de **SAML-eindpunt** tekstvak.
   
   f. Open uw gedownloade base-64 gecodeerde certificaat in Kladblok, Kopieer de inhoud ervan in het Klembord en plakt u het gehele certificaat in **SAML-certificaat** tekstvak.
   
   g. Selecteer **eenmalige aanmelding inschakelen voor beheerders zich kunnen aanmelden met hun netwerkreferenties**.
   
   h. Klik op **Wijzigingen opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/mozy-enterprise-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/mozy-enterprise-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/mozy-enterprise-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/mozy-enterprise-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-mozy-enterprise-test-user"></a>Het maken van een testgebruiker Mozy Enterprise

Om in te schakelen in Azure AD-gebruikers zich aanmelden bij Mozy Enterprise, moeten ze worden ingericht voor Mozy Enterprise. In het geval van Mozy Enterprise is inrichten een handmatige taak.

>[!NOTE]
>U kunt alle andere Mozy Enterprise gebruiker-account maken van hulpprogramma's of API's geleverd door Mozy Enterprise aan inrichten AAD-gebruikersaccounts.

**Voer de volgende stappen uit om een gebruikersaccount in te richten:**

1. Meld u aan bij uw **Mozy Enterprise** tenant.

1. Klik op **gebruikers**, en klik vervolgens op **Add New User**.
   
   ![Gebruikers](./media/mozy-enterprise-tutorial/ic777317.png "Gebruikers")
   
   >[!NOTE]
   >De **Add New User** optie wordt alleen weergegeven als **Mozy** is geselecteerd als de provider onder **verificatiebeleid**. Als SAML-verificatie is geconfigureerd, worden klikt u vervolgens de gebruikers automatisch toegevoegd aan hun eerste keer aanmelden via eenmalige aanmelding op.
    
1. In het gebruikersdialoogvenster nieuwe de volgende stappen uitvoeren:
   
   ![Gebruikers toevoegen](./media/mozy-enterprise-tutorial/ic777318.png "gebruikers toevoegen")
   
   a. Uit de **kiest u een groep** , selecteert u een groep.
   
   b. Uit de **welk type gebruiker** , selecteert u een type.
   
   c. In de **gebruikersnaam** tekstvak, typ de naam van de Azure AD-gebruiker.
   
   d. In de **e** tekstvak typt u het e-mailadres van de Azure AD-gebruiker.
   
   e. Selecteer **verzenden van e-mailadres van gebruiker instructie**.
   
   f. Klik op **toevoegen van gebruiker (s)**.

     >[!NOTE]
     > Nadat de gebruiker is gemaakt, wordt een e-mailbericht worden verzonden naar de Azure AD-gebruiker met een koppeling om te bevestigen dat het account voordat deze geactiveerd wordt.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Mozy onderneming.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon naar Mozy Enterprise, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Mozy Enterprise**.

    ![Eenmalige aanmelding configureren](./media/mozy-enterprise-tutorial/tutorial_mozyenterprise_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Mozy Enterprise in het toegangsvenster, krijgt u de aanmeldingspagina van Mozy bedrijfstoepassing.
Zie [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/mozy-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/mozy-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/mozy-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/mozy-enterprise-tutorial/tutorial_general_04.png

[100]: ./media/mozy-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/mozy-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/mozy-enterprise-tutorial/tutorial_general_201.png
[202]: ./media/mozy-enterprise-tutorial/tutorial_general_202.png
[203]: ./media/mozy-enterprise-tutorial/tutorial_general_203.png

