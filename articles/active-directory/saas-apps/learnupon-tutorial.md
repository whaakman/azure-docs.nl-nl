---
title: 'Zelfstudie: Azure Active Directory-integratie met LearnUpon | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en LearnUpon.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: b11c6315-c79d-4f34-9610-bd17070ab7c7
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: a548f11b1f423fb738108d9314f2fc887ef02c52
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55176842"
---
# <a name="tutorial-azure-active-directory-integration-with-learnupon"></a>Zelfstudie: Azure Active Directory-integratie met LearnUpon

In deze zelfstudie leert u hoe u LearnUpon integreren met Azure Active Directory (Azure AD).

LearnUpon integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot LearnUpon heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij LearnUpon (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met LearnUpon, moet u de volgende items:

- Een Azure AD-abonnement
- Een LearnUpon eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. LearnUpon uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-learnupon-from-the-gallery"></a>LearnUpon uit de galerie toe te voegen
Voor het configureren van de integratie van LearnUpon in Azure AD, moet u LearnUpon uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen LearnUpon uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **LearnUpon**.

    ![Het maken van een Azure AD-testgebruiker](./media/learnupon-tutorial/tutorial_learnupon_search.png)

1. Selecteer in het deelvenster resultaten **LearnUpon**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/learnupon-tutorial/tutorial_learnupon_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met LearnUpon op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in LearnUpon is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in LearnUpon tot stand worden gebracht.

In LearnUpon, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met LearnUpon, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker LearnUpon](#creating-a-learnupon-test-user)**  : als u wilt een equivalent van Britta Simon in LearnUpon die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing LearnUpon.

**Voor het configureren van Azure AD eenmalige aanmelding met LearnUpon, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **LearnUpon** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/learnupon-tutorial/tutorial_learnupon_samlbase.png)

1. Op de **LearnUpon domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/learnupon-tutorial/tutorial_learnupon_url.png)

    In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://<companyname>.learnupon.com/saml/consumer`

    > [!NOTE] 
    > Houd er rekening mee dat dit niet de werkelijke waarde is. u moet deze waarde bijwerken met de werkelijke antwoord-URL. Deze waarde Contact op te halen [LearnUpon ondersteuningsteam](https://www.learnupon.com/features/support/).



1. Op de **SAML-handtekeningcertificaat** sectie, Ga naar de **vingerafdruk** -dit wordt toegevoegd aan uw LearnUpon SAML-instellingen.

    ![Eenmalige aanmelding configureren](./media/learnupon-tutorial/tutorial_learnupon_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/learnupon-tutorial/tutorial_general_400.png)

1. Op de **LearnUpon configuratie** sectie, klikt u op **configureren LearnUpon** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/learnupon-tutorial/tutorial_learnupon_configure.png) 

1. Open een ander exemplaar van de browser en meld u aan bij LearnUpon met een administrator-account. 

1. Klik op de **instellingen** tabblad.
   
    ![Eenmalige aanmelding configureren](./media/learnupon-tutorial/tutorial_learnupon_06.png)

1. Klik op **Single Sign On - SAML**, en klik vervolgens op **algemene instellingen** om SAML-instellingen te configureren.
   
    ![Eenmalige aanmelding configureren](./media/learnupon-tutorial/tutorial_learnupon_07.png) 

1. In de **algemene instellingen** sectie, voert u de volgende stappen uit:
   
    ![Eenmalige aanmelding configureren](./media/learnupon-tutorial/tutorial_learnupon_08.png)  
  
    a. Selecteer **ingeschakeld**.

    b. Selecteer **versie** als **2.0**.

    c. Selecteer **overslaan voorwaarden** als **Nee**.

    d. In de **parameternaam SAML-Token plaatsen** tekstvak, type de naam van de aanvraagparameter-bericht naar de URL voor SAML-consument hierboven aangegeven dat de SAML-verklaring worden geverifieerd en geverifieerd - bijvoorbeeld bevat **SAMLResponse** .

    e. In de **indeling van de id** tekstvak, type de waarde die waar in de SAML-verklaring (e-mailadres) van de gebruikers-id aangeeft bevindt zich - bijvoorbeeld **urn: oasis: namen: tc: SAML:1.1:nameid-indeling: emailAddress**.
  
    f. In de **Providerlocatie identificeren** tekstvak typt u de waarde die waar de gebruikers om te worden verzonden aangeeft als ze op het pictogram van uw geüploade vanuit het aanmeldingsscherm van de Azure portal.
  
    g. In de **afmeldings-URL** tekstvak, plak de **afmelding URL** die u hebt gekopieerd vanuit Azure portal.
    
    h. Klik op **vinger afdrukken bestellen beheren**, en vervolgens de vingerafdruk van het gedownloade certificaat te uploaden.

1. Klik op **gebruikersinstellingen**, en voer de volgende stappen uit:
   
     ![Eenmalige aanmelding configureren](./media/learnupon-tutorial/tutorial_learnupon_11.png)  
 
    a. In de **voornaam id indeling** tekstvak, type de waarde die kan worden achterhaald terwijl in het SAML-verklaring de voornaam van de gebruikers zich - bijvoorbeeld bevindt: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
  
    b. In de **indeling van de laatste id** tekstvak, type de waarde die kan worden achterhaald terwijl in het SAML-verklaring de achternaam van de gebruikers zich - bijvoorbeeld bevindt: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/learnupon-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/learnupon-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/learnupon-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/learnupon-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-learnupon-test-user"></a>Het maken van een testgebruiker LearnUpon

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in LearnUpon. LearnUpon biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld.

Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker wordt gemaakt tijdens een poging tot toegang tot LearnUpon als deze nog niet bestaat. [Configureren van Azure AD voor eenmalige aanmelding](#configuring-azure-ad-single-single-sign-on).

>[!NOTE]
>Als u een gebruiker handmatig hebt gemaakt wilt, moet u contact opnemen met [LearnUpon ondersteuningsteam](https://www.learnupon.com/features/support/). 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan LearnUpon.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan LearnUpon toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **LearnUpon**.

    ![Eenmalige aanmelding configureren](./media/learnupon-tutorial/tutorial_learnupon_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel LearnUpon in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing LearnUpon.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/learnupon-tutorial/tutorial_general_01.png
[2]: ./media/learnupon-tutorial/tutorial_general_02.png
[3]: ./media/learnupon-tutorial/tutorial_general_03.png
[4]: ./media/learnupon-tutorial/tutorial_general_04.png

[100]: ./media/learnupon-tutorial/tutorial_general_100.png

[200]: ./media/learnupon-tutorial/tutorial_general_200.png
[201]: ./media/learnupon-tutorial/tutorial_general_201.png
[202]: ./media/learnupon-tutorial/tutorial_general_202.png
[203]: ./media/learnupon-tutorial/tutorial_general_203.png

