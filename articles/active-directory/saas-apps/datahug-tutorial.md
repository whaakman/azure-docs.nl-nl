---
title: 'Zelfstudie: Azure Active Directory-integratie met Datahug | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Datahug.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 5c0dc1ea-7ff4-4554-b60b-0f2fa9f5abaa
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2017
ms.author: jeedes
ms.openlocfilehash: b3c67d794bd5947dc377cbdb7578e23ff3e05390
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39441507"
---
# <a name="tutorial-azure-active-directory-integration-with-datahug"></a>Zelfstudie: Azure Active Directory-integratie met Datahug

In deze zelfstudie leert u hoe u Datahug integreren met Azure Active Directory (Azure AD).

Datahug integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Datahug heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Datahug (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, raadpleegt u. [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Datahug, moet u de volgende items:

- Een Azure AD-abonnement
- Een Datahug eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Datahug uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-datahug-from-the-gallery"></a>Datahug uit de galerie toe te voegen
Voor het configureren van de integratie van Datahug in Azure AD, moet u Datahug uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Datahug uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

1. Typ in het zoekvak **Datahug**.

    ![Het maken van een Azure AD-testgebruiker](./media/datahug-tutorial/tutorial_datahug_search.png)

1. Selecteer in het deelvenster resultaten **Datahug**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/datahug-tutorial/tutorial_datahug_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Datahug op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Datahug is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Datahug tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door toe te wijzen de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Datahug.

Om te configureren en testen van Azure AD eenmalige aanmelding met Datahug, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker Datahug](#creating-a-datahug-test-user)**  : als u wilt een equivalent van Britta Simon in Datahug die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Datahug.

**Voor het configureren van Azure AD eenmalige aanmelding met Datahug, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Datahug** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/datahug-tutorial/tutorial_datahug_samlbase.png)

1. Op de **Datahug domein en URL's** sectie, als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Eenmalige aanmelding configureren](./media/datahug-tutorial/tutorial_datahug_ur1.png)

    a. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://apps.datahug.com/identity/<uniqueID>`

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://apps.datahug.com/identity/<uniqueID>/acs`

1. Controleer **geavanceerde URL-instellingen weergeven**. Als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Eenmalige aanmelding configureren](./media/datahug-tutorial/tutorial_datahug_url2.png)

    In de **aanmeldings-URL** tekstvak, een URL als: `https://apps.datahug.com/`
     
    > [!NOTE] 
    > Deze waarden zijn niet de werkelijke. Werk deze waarden met de werkelijke id en de antwoord-URL. Hier stellen we voor u voor het gebruik van de unieke waarde van de tekenreeks in de id en de antwoord-URL. Neem contact op met [Datahug Client ondersteuningsteam](http://datahug.com/about/contact-us/) om deze waarden te verkrijgen. 

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/datahug-tutorial/tutorial_datahug_certificate.png) 

1.  Controleer **'Geavanceerde instellingen voor het ondertekenen van certificaat weergeven'** en voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/datahug-tutorial/tutorial_datahug_cert.png)

    a. In **ondertekening optie**, selecteer **aanmelding SAML-verklaring**.
    
    b. In **algoritme voor ondertekening**, selecteer **SHA1**.
 
1. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/datahug-tutorial/tutorial_general_400.png)
    
1. Op de **Datahug configuratie** sectie, klikt u op **configureren Datahug** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID** en **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/datahug-tutorial/tutorial_datahug_configure.png) 

1. Het configureren van eenmalige aanmelding op **Datahug** zijde, moet u voor het verzenden van de gedownloade **Metadata XML**, **SAML entiteit-ID** en **SAML Single Sign-On Service-URL**  naar [Datahug ondersteuning](http://datahug.com/about/contact-us/). Ze instellen hebben de SAML SSO-verbinding juist ingesteld voor beide zijden van deze toepassing.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/datahug-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/datahug-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/datahug-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/datahug-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-datahug-test-user"></a>Het maken van een testgebruiker Datahug

Als u wilt dat Azure AD-gebruikers zich aanmelden bij Datahug, moeten ze worden ingericht voor Datahug.  
Datahug, wordt ingericht als een handmatige taak is.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf Datahug site aan als beheerder.

1. Beweeg de muisaanwijzer over de **tandwiel** in de rechterbovenhoek en klik op **instellingen**
   
   ![Werknemer toevoegen](./media/datahug-tutorial/1.png)

1. Kies **mensen** en klikt u op de **gebruikers toevoegen** tabblad

    ![Werknemer toevoegen](./media/datahug-tutorial/2.png)

1. Typ de e-mailadres van de persoon die u wilt een account voor het maken en klik op **toevoegen**.

    ![Werknemer toevoegen](./media/datahug-tutorial/3.png)

    > [!NOTE] 
    > U kunt registratie-e-mail naar gebruiker verzenden door te selecteren **welkomstbericht verzenden** selectievakje.  
    > Als u maakt een Salesforce-account niet de verwelkomingse-mail verzenden.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Datahug.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan Datahug toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Datahug**.

    ![Eenmalige aanmelding configureren](./media/datahug-tutorial/tutorial_datahug_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.
Wanneer u op de tegel Datahug in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Datahug. Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/datahug-tutorial/tutorial_general_01.png
[2]: ./media/datahug-tutorial/tutorial_general_02.png
[3]: ./media/datahug-tutorial/tutorial_general_03.png
[4]: ./media/datahug-tutorial/tutorial_general_04.png

[100]: ./media/datahug-tutorial/tutorial_general_100.png

[200]: ./media/datahug-tutorial/tutorial_general_200.png
[201]: ./media/datahug-tutorial/tutorial_general_201.png
[202]: ./media/datahug-tutorial/tutorial_general_202.png
[203]: ./media/datahug-tutorial/tutorial_general_203.png

