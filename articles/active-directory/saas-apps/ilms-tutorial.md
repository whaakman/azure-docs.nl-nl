---
title: 'Zelfstudie: Azure Active Directory-integratie met iLMS | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en iLMS.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: d6e11639-6cea-48c9-b008-246cf686e726
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/13/2017
ms.author: jeedes
ms.openlocfilehash: 7a533c4428169a219b131247a044632e614868e9
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54814882"
---
# <a name="tutorial-azure-active-directory-integration-with-ilms"></a>Zelfstudie: Azure Active Directory-integratie met iLMS

In deze zelfstudie leert u hoe u iLMS integreren met Azure Active Directory (Azure AD).

ILMS integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot iLMS heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij iLMS (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met iLMS, moet u de volgende items:

- Een Azure AD-abonnement
- Een iLMS eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- U moet uw productie-omgeving, niet gebruiken als dit nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. ILMS uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-ilms-from-the-gallery"></a>ILMS uit de galerie toe te voegen
Voor het configureren van de integratie van iLMS in Azure AD, moet u iLMS uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen iLMS uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **iLMS**.

    ![Het maken van een Azure AD-testgebruiker](./media/ilms-tutorial/tutorial_ilms_search.png)

1. Selecteer in het deelvenster resultaten **iLMS**, klikt u vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/ilms-tutorial/tutorial_ilms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met iLMS op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in iLMS is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in iLMS tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door toe te wijzen de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in iLMS.

Om te configureren en testen van Azure AD eenmalige aanmelding met iLMS, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker iLMS](#creating-an-ilms-test-user)**  : als u wilt een equivalent van Britta Simon in iLMS die is gekoppeld aan de Azure AD-weergave van haar zijn.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing iLMS.

**Voor het configureren van Azure AD eenmalige aanmelding met iLMS, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **iLMS** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/ilms-tutorial/tutorial_ilms_samlbase.png)

1. Op de **iLMS domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Eenmalige aanmelding configureren](./media/ilms-tutorial/tutorial_ilms_url.png)

    a. In de **id** tekstvak, plak de **id** waarde u van kopiëren **serviceprovider** sectie van SAML-instellingen in de beheerportal iLMS.

    b. In de **antwoord-URL** tekstvak, plak de **eindpunt (URL)** waarde u van kopiëren **serviceprovider** sectie van SAML-instellingen in het volgende patroon van iLMS-beheerportal `https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`

    >[!Note]
    >Deze '123456' is een van de Voorbeeldwaarde van id.

1. Controleer **geavanceerde URL-instellingen weergeven**, als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Eenmalige aanmelding configureren](./media/ilms-tutorial/tutorial_ilms_url1.png)

    In de **aanmeldings-URL** tekstvak, plak de **eindpunt (URL)** waarde u van kopiëren **serviceprovider** sectie van SAML-instellingen in de beheerportal iLMS als `https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`     

1. Om in te schakelen JIT-inrichting, verwacht iLMS toepassing de SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit de sectie **Gebruikerskenmerken** op de integratiepagina van de toepassing-beheren. In de volgende schermopname ziet u een voorbeeld hiervan.
    
    ![Eenmalige aanmelding configureren](./media/ilms-tutorial/4.png)
    
    Maak **afdeling, regio** en **deling** kenmerken en voegt u de naam van deze kenmerken in iLMS. Alle deze kenmerken die hierboven zijn vereist.  

    > [!NOTE] 
    > U moet inschakelen **Un-recognized-gebruikersaccount maken** in iLMS om toe te wijzen deze kenmerken. Volg de instructies [hier](http://support.inspiredelearning.com/customer/portal/articles/2204526) voor een beter beeld van de configuratie van de kenmerken.

1. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de bovenstaande afbeelding en voer de volgende stappen uit:
    
    | Naam kenmerk | Waarde kenmerk |
    | ---------------| --------------- |    
    | deling | user.department |
    | regio | user.state |
    | department | user.jobtitle |

    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/ilms-tutorial/tutorial_ilms_04.png)

    ![Eenmalige aanmelding configureren](./media/ilms-tutorial/tutorial_ilms_05.png)
    
    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.
    
    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    d. Klik op **OK**.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het XML-bestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/ilms-tutorial/tutorial_ilms_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/ilms-tutorial/tutorial_general_400.png)

1. In een ander browservenster aanmelden bij uw **iLMS-beheerportal** als beheerder.

1. Klik op **SSO:SAML** onder **instellingen** tabblad SAML-instellingen openen en voer de volgende stappen uit:
    
    ![Eenmalige aanmelding configureren](./media/ilms-tutorial/1.png) 

    a. Vouw de **serviceprovider** sectie en kopieert u de **id** en **eindpunt (URL)** waarde.

    ![Eenmalige aanmelding configureren](./media/ilms-tutorial/2.png) 

    b. Onder **id-Provider** sectie, klikt u op **metagegevens importeren**.
    
    c. Selecteer de **metagegevens** bestand gedownload vanuit Azure Portal uit **SAML-handtekeningcertificaat** sectie.

    ![Eenmalige aanmelding configureren](./media/ilms-tutorial/tutorial_ilms_ssoconfig1.png) 

    d. Als u inschakelen wilt voor het maken van accounts iLMS voor ongedaan maken inrichting JIT-gebruikers herkennen, volgt u onderstaande stappen te volgen:
        
       - Controleer **Account maken voor niet-herkende gebruiker**.
       
       ![Configure Single Sign-On](./media/ilms-tutorial/tutorial_ilms_ssoconfig2.png)

       -  De kenmerken in Azure AD met de kenmerken in iLMS toewijzen. Geef de naam van de kenmerken of de standaardwaarde is opgegeven in de kolom kenmerk.

    e. Ga naar **bedrijfsregels** tabblad en voer de volgende stappen uit: 
        
       ![Configure Single Sign-On](./media/ilms-tutorial/5.png)

       - Controleer **Un-recognized regio's maken, afdelingen en afdelingen** om regio's, afdelingen en afdelingen die nog niet bestaan op het moment van eenmalige aanmelding te maken.
        
       - Controleer **Update gebruiker profiel tijdens aanmelding** om op te geven of profiel van de gebruiker is bijgewerkt met elke eenmalige aanmelding. 
        
       - Als de **"Update lege waarden voor niet verplichte velden in gebruikersprofiel"** optie is ingeschakeld, optioneel profiel velden die leeg bij het aanmelden wordt zijn ook voor zorgen dat iLMS profiel van de gebruiker bevatten lege waarden voor deze velden.
        
       - Controleer **fout meldingse-mail verzenden** en voer het e-mailadres van de gebruiker waar u de fout e-mailmelding ontvangen.

1. Klik op **Save** om de instellingen op te slaan.

    ![Eenmalige aanmelding configureren](./media/ilms-tutorial/save.png)

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
    
### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/ilms-tutorial/create_aaduser_01.png) 

1. Ga naar **gebruikers en groepen** en klikt u op **alle gebruikers** om de lijst met gebruikers weer te geven.
    
    ![Het maken van een Azure AD-testgebruiker](./media/ilms-tutorial/create_aaduser_02.png) 

1. Aan de bovenkant van het dialoogvenster klikt u op **toevoegen** openen de **gebruiker** dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/ilms-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/ilms-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-an-ilms-test-user"></a>Het maken van een testgebruiker iLMS

Toepassing ondersteunt Just in time gebruikersinrichting en na-verificatiegebruikers automatisch in de toepassing gemaakt worden. JIT werkt als u hebt geklikt op de **Un-recognized-gebruikersaccount maken** selectievakje tijdens de SAML-configuratie-instelling op iLMS-beheerportal.

Als u een gebruiker handmatig hebt gemaakt wilt, volgt u onderstaande stappen te volgen:

1. Meld u aan bij uw bedrijf iLMS site aan als beheerder.

1. Klik op **'Gebruiker registreren'** onder **gebruikers** tabblad openen **gebruiker registreren** pagina. 
   
   ![Werknemer toevoegen](./media/ilms-tutorial/3.png)

1. Op de **'Gebruiker registreren'** pagina, de volgende stappen uitvoeren.

    ![Werknemer toevoegen](./media/ilms-tutorial/create_testuser_add.png)

    a. In de **voornaam** tekstvak, de eerste naam Julia.
   
    b. In de **achternaam** tekstvak typt u de achternaam Simon.

    c. In de **E-mail-ID** tekstvak typt u het e-mailadres van Britta Simon-account.

    d. In de **regio** vervolgkeuzelijst, selecteer de waarde voor de regio.

    e. In de **deling** vervolgkeuzelijst, selecteer de waarde voor afdeling.

    f. In de **afdeling** vervolgkeuzelijst, selecteer de waarde voor afdeling.

    g. Klik op **Opslaan**.

    > [!NOTE] 
    > U kunt registratie-e-mail naar gebruiker verzenden door te selecteren **registratie E-mail** selectievakje.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door haar toegang verlenen tot iLMS.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan iLMS toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **iLMS**.

    ![Eenmalige aanmelding configureren](./media/ilms-tutorial/tutorial_ilms_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel iLMS in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing iLMS.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/ilms-tutorial/tutorial_general_01.png
[2]: ./media/ilms-tutorial/tutorial_general_02.png
[3]: ./media/ilms-tutorial/tutorial_general_03.png
[4]: ./media/ilms-tutorial/tutorial_general_04.png

[100]: ./media/ilms-tutorial/tutorial_general_100.png

[200]: ./media/ilms-tutorial/tutorial_general_200.png
[201]: ./media/ilms-tutorial/tutorial_general_201.png
[202]: ./media/ilms-tutorial/tutorial_general_202.png
[203]: ./media/ilms-tutorial/tutorial_general_203.png

