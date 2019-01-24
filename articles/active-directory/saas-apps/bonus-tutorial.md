---
title: 'Zelfstudie: Azure Active Directory-integratie met Bonusly | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Bonusly.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 29fea32a-fa20-47b2-9e24-26feb47b0ae6
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 181577c6a170d28679e1ca486dc2a8c5a596f20d
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54812128"
---
# <a name="tutorial-azure-active-directory-integration-with-bonusly"></a>Zelfstudie: Azure Active Directory-integratie met Bonusly

In deze zelfstudie leert u hoe u Bonusly integreren met Azure Active Directory (Azure AD).

Bonusly integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Bonusly heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Bonusly (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Bonusly, moet u de volgende items:

- Een Azure AD-abonnement
- Een Bonusly eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Bonusly uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-bonusly-from-the-gallery"></a>Bonusly uit de galerie toe te voegen
Voor het configureren van de integratie van Bonusly in Azure AD, moet u Bonusly uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Bonusly uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **Bonusly**, selecteer **Bonusly** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Bonusly in de lijst met resultaten](./media/bonus-tutorial/tutorial_bonusly_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Bonusly op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Bonusly is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Bonusly tot stand worden gebracht.

In Bonusly, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Bonusly, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker Bonusly](#create-a-bonusly-test-user)**  : als u wilt een equivalent van Britta Simon in Bonusly die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw Bonusly toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met Bonusly, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Bonusly** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/bonus-tutorial/tutorial_bonusly_samlbase.png)

1. Op de **Bonusly domein en URL's** sectie, voert u de volgende stappen uit:

    ![Bonusly domein en URL's, eenmalige aanmelding informatie](./media/bonus-tutorial/tutorial_bonusly_url.png)

    In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://Bonus.ly/saml/<tenant-name>`

    > [!NOTE] 
    > De waarde is niet echt. Werk de waarde bij met de werkelijke antwoord-URL. Neem contact op met [Bonusly ondersteuningsteam](https://bonus.ly/contact) om de waarde.
 
1. Op de **SAML-handtekeningcertificaat** sectie, Kopieer de **VINGERAFDRUK** waarde van het certificaat.

    ![De link om het certificaat te downloaden](./media/bonus-tutorial/tutorial_bonusly_certificate.png) 

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/bonus-tutorial/tutorial_general_400.png)

1. Op de **Bonusly configuratie** sectie, klikt u op **Bonusly configureren** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Bonusly configuratie](./media/bonus-tutorial/tutorial_bonusly_configure.png) 

1. In een ander browservenster en meld u aan bij uw **Bonusly** tenant.

1. Klik in de werkbalk bovenaan op **instellingen**, en selecteer vervolgens **integraties en apps**.
   
    ![Bonusly sociale sectie](./media/bonus-tutorial/ic773686.png "Bonusly")
1. Onder **Single Sign-On**, selecteer **SAML**.

1. Op de **SAML** dialoogvenster pagina, voert u de volgende stappen uit:
   
    ![Bonusly Saml-dialoogvenster pagina](./media/bonus-tutorial/ic773687.png "Bonusly")
   
    a. In de **doel-URL voor eenmalige aanmelding IdP** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML**, die u hebt gekopieerd vanuit Azure portal.
   
    b. In de **IdP verlener** tekstvak, plak de waarde van **SAML entiteit-ID**, die u hebt gekopieerd vanuit Azure portal. 

    c. In de **IdP aanmeldings-URL** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML**, die u hebt gekopieerd vanuit Azure portal.

    d. Plak de **vingerafdruk** waarde opgehaald uit Azure portal in de **Cert vingerafdruk** tekstvak.
   
1. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](./media/bonus-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![De koppelingen Gebruikers en groepen en Alle gebruikers](./media/bonus-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![De knop toevoegen](./media/bonus-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het dialoogvenster Gebruiker](./media/bonus-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="create-a-bonusly-test-user"></a>Maak een testgebruiker Bonusly

Als u wilt dat Azure AD-gebruikers zich aanmelden bij Bonusly, moeten ze worden ingericht voor Bonusly. In het geval van Bonusly is inrichten een handmatige taak.

>[!NOTE]
>U kunt een andere gebruiker Bonusly account hulpmiddelen voor het maken of API's die is geleverd door Bonusly aan inrichten AAD-gebruikersaccounts.
>  

**Voer de volgende stappen uit om de gebruikersinrichting te configureren:**

1. Meld u aan bij uw Bonusly tenant aan in een browservenster.

1. Klik op **Instellingen**.
 
    ![Settings](./media/bonus-tutorial/ic781041.png "Settings")

1. Klik op de **gebruikers en bonussen** tabblad.
   
    ![Gebruikers en bonussen](./media/bonus-tutorial/ic781042.png "gebruikers en bonussen")

1. Klik op **gebruikers beheren**.
   
    ![Gebruikers beheren](./media/bonus-tutorial/ic781043.png "gebruikers beheren")

1. Klik op **Add User**.
   
    ![Gebruiker toevoegen](./media/bonus-tutorial/ic781044.png "Gebruiker toevoegen")

1. Op de **gebruiker toevoegen** dialoogvenster, voer de volgende stappen uit:
   
    ![Gebruiker toevoegen](./media/bonus-tutorial/ic781045.png "Gebruiker toevoegen")  

    a. In de **voornaam** tekstvak, geef de voornaam van de gebruiker, zoals **Julia**.

    b. In de **achternaam** tekstvak, geef de achternaam van de gebruiker, zoals **Simon**.
 
    c. Voer in het tekstvak **Email** het e-mailadres van de gebruiker in, bijvoorbeeld **brittasimon@contoso.com**.

    d. Klik op **Opslaan**.
   
     >[!NOTE]
     >De houder van Azure AD-account ontvangt een e-mailbericht een koppeling om te bevestigen dat het account bevat voordat deze geactiveerd wordt.
     >  

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Bonusly.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan Bonusly toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Bonusly**.

    ![De Bonusly koppelen in de lijst met toepassingen](./media/bonus-tutorial/tutorial_bonusly_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie via het toegangsvenster.

Wanneer u klikt op de tegel Bonusly in het toegangsvenster, u moet u automatisch aangemeld bij uw Bonusly toepassing.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/bonus-tutorial/tutorial_general_01.png
[2]: ./media/bonus-tutorial/tutorial_general_02.png
[3]: ./media/bonus-tutorial/tutorial_general_03.png
[4]: ./media/bonus-tutorial/tutorial_general_04.png

[100]: ./media/bonus-tutorial/tutorial_general_100.png

[200]: ./media/bonus-tutorial/tutorial_general_200.png
[201]: ./media/bonus-tutorial/tutorial_general_201.png
[202]: ./media/bonus-tutorial/tutorial_general_202.png
[203]: ./media/bonus-tutorial/tutorial_general_203.png

