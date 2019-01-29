---
title: 'Zelfstudie: Azure Active Directory-integratie met InsideView | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en InsideView.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: c489a7ab-6b1f-4efb-8a66-8bc13bca78c3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: 38749d9d920a3243c5fe63912c08cafa5834350c
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55154148"
---
# <a name="tutorial-azure-active-directory-integration-with-insideview"></a>Zelfstudie: Azure Active Directory-integratie met InsideView

In deze zelfstudie leert u hoe u InsideView integreren met Azure Active Directory (Azure AD).

InsideView integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot InsideView heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij InsideView (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met InsideView, moet u de volgende items:

- Een Azure AD-abonnement
- Een InsideView eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. InsideView uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-insideview-from-the-gallery"></a>InsideView uit de galerie toe te voegen
Voor het configureren van de integratie van InsideView in met Azure AD, moet u InsideView uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen InsideView uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **InsideView**.

    ![Het maken van een Azure AD-testgebruiker](./media/insideview-tutorial/tutorial_insideview_search.png)

1. Selecteer in het deelvenster resultaten **InsideView**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/insideview-tutorial/tutorial_insideview_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met InsideView op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in InsideView is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in InsideView tot stand worden gebracht.

In InsideView, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met InsideView, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker InsideView](#creating-a-insideview-test-user)**  : als u wilt een equivalent van Britta Simon in InsideView die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing InsideView.

**Voor het configureren van Azure AD eenmalige aanmelding met InsideView, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **InsideView** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/insideview-tutorial/tutorial_insideview_samlbase.png)

1. Op de **InsideView domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/insideview-tutorial/tutorial_insideview_url.png)
    
    In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://my.insideview.com/iv/<STS Name>/login.iv`

    > [!NOTE] 
    > Deze waarde is niet echt. Deze waarde bijwerken met de werkelijke antwoord-URL. Neem contact op met [InsideView ondersteuningsteam ](mailto:support@insideview.com) deze waarde op te halen.
 
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Raw)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/insideview-tutorial/tutorial_insideview_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/insideview-tutorial/tutorial_general_400.png)

1. Op de **InsideView configuratie** sectie, klikt u op **configureren InsideView** openen **aanmelding configureren** venster. Kopiëren de **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/insideview-tutorial/tutorial_insideview_configure.png) 

1. In een ander browservenster aanmelden bij uw bedrijf InsideView site als beheerder.

1. Klik in de werkbalk bovenaan op **Admin**, **SingleSignOn instellingen**, en klik vervolgens op **toevoegen SAML**.
   
   ![Eenmalige aanmelding instellingen SAML](./media/insideview-tutorial/ic794135.png "eenmalige aanmelding van de SAML-instellingen")

1. In de **toevoegen van een nieuwe SAML** sectie, voert u de volgende stappen uit:

    ![Toevoegen van een nieuwe SAML](./media/insideview-tutorial/ic794136.png "toevoegen van een nieuwe SAML")
   
    a. In de **STS Name** tekstvak, typ een naam voor uw configuratie.

    b. In **SamlP/WS-Federation ongevraagde eindpunt** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML**, die u hebt gekopieerd vanuit Azure portal.
    
    c. Open uw base-64 gecodeerde certificaat, dat u hebt gedownload vanuit Azure portal, de inhoud van het kopiëren naar het Klembord en plakt u deze naar de **STS Certificate** tekstvak.

    d. In de **Crm-Id Gebruikerstoewijzing** tekstvak, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.
        
    e. In de **Crm-e-mailbericht toewijzen** tekstvak, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    f. In de **Crm voornaam toewijzing** tekstvak, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.
    
    g. In de **Crm lastName toewijzing** tekstvak, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.  

    h. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 
 
### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/insideview-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/insideview-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/insideview-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/insideview-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-insideview-test-user"></a>Het maken van een testgebruiker InsideView

Om Azure AD-gebruikers zich aanmelden bij InsideView, moeten ze worden ingericht voor InsideView. In het geval van InsideView is inrichten een handmatige taak.

Als u gebruikers of contactpersonen gemaakt in InsideView, neem contact op met [InsideView ondersteuningsteam](mailto:support@insideview.com).

>[!NOTE]
>U kunt alle andere InsideView gebruiker-account maken van hulpprogramma's of API's geleverd door InsideView voor het inrichten van gebruikersaccounts van de Azure AD.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan InsideView.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan InsideView toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **InsideView**.

    ![Eenmalige aanmelding configureren](./media/insideview-tutorial/tutorial_insideview_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel InsideView in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing InsideView.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/insideview-tutorial/tutorial_general_01.png
[2]: ./media/insideview-tutorial/tutorial_general_02.png
[3]: ./media/insideview-tutorial/tutorial_general_03.png
[4]: ./media/insideview-tutorial/tutorial_general_04.png

[100]: ./media/insideview-tutorial/tutorial_general_100.png

[200]: ./media/insideview-tutorial/tutorial_general_200.png
[201]: ./media/insideview-tutorial/tutorial_general_201.png
[202]: ./media/insideview-tutorial/tutorial_general_202.png
[203]: ./media/insideview-tutorial/tutorial_general_203.png

