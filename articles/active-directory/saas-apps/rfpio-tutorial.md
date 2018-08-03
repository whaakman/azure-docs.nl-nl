---
title: 'Zelfstudie: Azure Active Directory-integratie met RFPIO | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en RFPIO.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 87187076-7b50-4247-814f-f217b052703f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 04ba94e3263af03279b74b4832b8291ad6414274
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39426557"
---
# <a name="tutorial-azure-active-directory-integration-with-rfpio"></a>Zelfstudie: Azure Active Directory-integratie met RFPIO

In deze zelfstudie leert u hoe u RFPIO integreren met Azure Active Directory (Azure AD).

RFPIO integreren met Azure AD biedt u de volgende voordelen:

- U kunt bepalen wie in Azure AD die toegang heeft tot RFPIO.
- U kunt uw gebruikers automatisch ophalen aangemeld bij RFPIO (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie, de Azure-portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met RFPIO, moet u de volgende items:

- Een Azure AD-abonnement.
- Een RFPIO single sign-on-ingeschakeld abonnement.

> [!NOTE]
> Wordt niet aanbevolen dat u een productie-omgeving gebruiken voor het testen van de stappen in deze zelfstudie.

Als u wilt testen van de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een [proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario dat wordt beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toe te voegen RFPIO uit de galerie.
1. Configureren en testen van Azure AD eenmalige aanmelding.

## <a name="add-rfpio-from-the-gallery"></a>RFPIO uit de galerie toevoegen
Voor het configureren van de integratie van RFPIO in Azure AD, moet u RFPIO uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

### <a name="to-add-rfpio-from-the-gallery"></a>RFPIO uit de galerie toevoegen

1. In de  **[Azure-portal](https://portal.azure.com)**, selecteer in het navigatiedeelvenster links in de **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Selecteer **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![Toepassingen][2]
    
1. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop boven aan in het dialoogvenster.

    ![Toepassingen][3]

1. Typ in het zoekvak **RFPIO**.

    ![Het maken van een Azure AD-testgebruiker](./media/rfpio-tutorial/tutorial_rfpio_search.png)

1. Selecteer in het deelvenster resultaten **RFPIO**, en selecteer vervolgens de **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/rfpio-tutorial/tutorial_rfpio_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met RFPIO op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de relatie tussen equivalent gebruiker in RFPIO en een gebruiker in Azure AD is. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in RFPIO tot stand worden gebracht.

In RFPIO, wijs de waarde van **gebruikersnaam** in Azure AD als de waarde van **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met RFPIO, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configuring-azure-ad-single-sign-on)**--zodat uw gebruikers deze functie wilt gebruiken.
1. **[Maak een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**--voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Maak een testgebruiker RFPIO](#creating-a-rfpio-test-user)**  --een equivalent van Britta Simon in RFPIO die is gekoppeld aan de Azure AD-weergave van de gebruiker kan hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**--Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  --om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing RFPIO.

**Voor het configureren van Azure AD eenmalige aanmelding met RFPIO, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **RFPIO** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/rfpio-tutorial/tutorial_rfpio_samlbase.png)

1. Op de **RFPIO domein en URL's** sectie, als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Eenmalige aanmelding configureren](./media/rfpio-tutorial/tutorial_rfpio_url.png)

    a. In de **id** tekstvak typt u de URL: `https://www.rfpio.com`

    ![Eenmalige aanmelding configureren](./media/rfpio-tutorial/tutorial_rfpio_url1.png)

    b. Controleer **geavanceerde URL-instellingen weergeven**.

    c. In de **Relaystatus** tekstvak een string-waarde. Neem contact op met [RFPIO ondersteuningsteam](https://www.rfpio.com/contact/) deze waarde op te halen. 

1. Controleer **geavanceerde URL-instellingen weergeven**. Als u wilt configureren van de toepassing in **SP** modus gestart: 

    ![Eenmalige aanmelding configureren](./media/rfpio-tutorial/tutorial_rfpio_url2.png)

    In de **aanmeldings-URL** tekstvak typt u de URL: `https://www.app.rfpio.com`

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/rfpio-tutorial/tutorial_rfpio_certificate.png) 

1. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/rfpio-tutorial/tutorial_general_400.png)

1. In een ander browservenster, meld u aan bij de **RFPIO** website als beheerder.

1. Klik op de onderste linkerhoek vervolgkeuzelijst.

    ![Eenmalige aanmelding configureren](./media/rfpio-tutorial/app1.png)

1. Klik op de **organisatie-instellingen**. 

    ![Eenmalige aanmelding configureren](./media/rfpio-tutorial/app2.png)

1. Klik op de **functies en integratie**.

    ![Eenmalige aanmelding configureren](./media/rfpio-tutorial/app4.png)

1. In de **SAML SSO-configuratie** klikt u op **bewerken**.

    ![Eenmalige aanmelding configureren](./media/rfpio-tutorial/app3.png)

1. In deze sectie voert u de volgende acties:

    ![Eenmalige aanmelding configureren](./media/rfpio-tutorial/app5.png)
    
    a. Kopieer de inhoud van de **gedownload metagegevens-XML** en plak deze in de **identiteit configuratie** veld.

    > [!NOTE]
    >Kopieer de inhoud van gedownload **Metadata XML** gebruik **Kladblok ++** of een beginhoofdletter **XML-Editor**. 

    b. Klik op **valideren**.

    c. Nadat u hebt geklikt **valideren**, spiegelen **SAML(Enabled)** op aan.

    d. Klik op **indienen**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/rfpio-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/rfpio-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/rfpio-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/rfpio-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="create-a-rfpio-test-user"></a>Maak een testgebruiker RFPIO

Als u wilt dat Azure AD-gebruikers zich aanmelden bij RFPIO, moeten ze worden ingericht voor RFPIO.  
In het geval van RFPIO is inrichten een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf RFPIO site aan als beheerder.

1. Klik op de onderste linkerhoek vervolgkeuzelijst.

    ![Eenmalige aanmelding configureren](./media/rfpio-tutorial/app1.png)

1. Klik op de **organisatie-instellingen**. 

    ![Eenmalige aanmelding configureren](./media/rfpio-tutorial/app2.png)

1. Klik op **TEAMLEDEN**.

    ![Eenmalige aanmelding configureren](./media/rfpio-tutorial/app6.png)

1. Klik op **leden toevoegen**.

    ![Eenmalige aanmelding configureren](./media/rfpio-tutorial/app7.png)

1. In de **nieuwe leden toevoegen** sectie. Voer de volgende acties:

    ![Eenmalige aanmelding configureren](./media/rfpio-tutorial/app8.png)

    a. ENTER **e-mailadres** in de **Voer één e-mailadres per regel** veld.

    b. Selecteer de Resourceverplaatsing **rol** volgens uw vereisten.

    c. Klik op **leden toevoegen**.
        
    > [!NOTE]
    > De houder van Azure Active Directory-account ontvangt een e-mailbericht en volgt een koppeling om te bevestigen van hun account voordat deze geactiveerd wordt.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan RFPIO.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan RFPIO toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **RFPIO**.

    ![Eenmalige aanmelding configureren](./media/rfpio-tutorial/tutorial_rfpio_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw configuratie Azure AD eenmalige aanmelding testen met behulp van het toegangsvenster.

Wanneer u op de tegel RFPIO in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing RFPIO.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/rfpio-tutorial/tutorial_general_01.png
[2]: ./media/rfpio-tutorial/tutorial_general_02.png
[3]: ./media/rfpio-tutorial/tutorial_general_03.png
[4]: ./media/rfpio-tutorial/tutorial_general_04.png

[100]: ./media/rfpio-tutorial/tutorial_general_100.png

[200]: ./media/rfpio-tutorial/tutorial_general_200.png
[201]: ./media/rfpio-tutorial/tutorial_general_201.png
[202]: ./media/rfpio-tutorial/tutorial_general_202.png
[203]: ./media/rfpio-tutorial/tutorial_general_203.png

