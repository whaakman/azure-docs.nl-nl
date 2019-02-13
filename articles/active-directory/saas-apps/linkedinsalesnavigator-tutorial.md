---
title: 'Zelfstudie: Azure Active Directory-integratie met LinkedIn Sales Navigator | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en LinkedInSalesNavigator.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 7a9fa8f3-d611-4ffe-8d50-04e9586b24da
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 63e3e98c2c3dc8f99e733174c86965304fe483ce
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56181997"
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-sales-navigator"></a>Zelfstudie: Azure Active Directory-integratie met LinkedIn Sales Navigator

In deze zelfstudie leert u hoe u LinkedIn Sales Navigator integreren met Azure Active Directory (Azure AD).

LinkedIn Sales Navigator integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot LinkedIn Sales Navigator heeft
- U kunt uw gebruikers automatisch ophalen aangemeld voor LinkedIn Sales Navigator (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u meer informatie over de integratie van de SaaS-app met Azure AD kent wilt, blader [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met LinkedIn Sales Navigator, moet u de volgende items:

- Een Azure AD-abonnement
- Een LinkedIn Sales Navigator eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Vermijd het gebruik van uw productieomgeving, als dat nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. LinkedIn Sales Navigator uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-linkedin-sales-navigator-from-the-gallery"></a>LinkedIn Sales Navigator uit de galerie toe te voegen
Voor het configureren van de integratie van LinkedIn Sales Navigator in Azure AD, moet u LinkedIn Sales Navigator uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen LinkedIn Sales Navigator uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Klik op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **LinkedIn Sales Navigator**.

    ![Het maken van een Azure AD-testgebruiker](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_search.png)

1. Selecteer in het deelvenster resultaten **LinkedIn Sales Navigator**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met LinkedIn Sales Navigator op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in LinkedIn Sales Navigator is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in LinkedIn Sales Navigator tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door toe te wijzen de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in LinkedIn Sales Navigator.

Om te configureren en testen van Azure AD eenmalige aanmelding met LinkedIn Sales Navigator, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker LinkedIn Sales Navigator](#creating-a-linkedin-sales-navigator-test-user)**  : als u wilt een equivalent van Britta Simon in LinkedIn Sales Navigator die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw LinkedIn Sales Navigator-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met LinkedIn Sales Navigator, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **LinkedIn Sales Navigator** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster in **modus** Selecteer **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_samlbase.png)

1. In een ander browservenster aanmelden voor uw **LinkedIn Sales Navigator** website als beheerder.

1. Klik in **Accountcentrum** onder **Instellingen** op **Algemene instellingen**. Schakel ook **Sales Navigator** in de vervolgkeuzelijst.

    ![Eenmalige aanmelding configureren](./media/linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_01.png)

1. Klik op **of Klik hier om te laden en kopiëren van afzonderlijke velden van het formulier** en kopieert u **entiteit-Id** en **Assertion Consumer Access (ACS) Url**.

    ![Eenmalige aanmelding configureren](./media/linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_031.png)

1. In Azure portal onder **LinkedIn Sales Navigator domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart.

    ![Eenmalige aanmelding configureren](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_url1.png)

    a. In de **id** tekstvak, voer de **entiteit-ID** opgehaald uit de LinkedIn-Portal 

    b. In de **antwoord-URL** tekstvak, voer de **Assertion Consumer Access (ACS) Url** opgehaald uit de LinkedIn-Portal

1. Controleer **geavanceerde URL-instellingen weergeven**, als u wilt configureren van de toepassing in **SP** modus gestart.

    ![Eenmalige aanmelding configureren](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_url2.png)

    In de **aanmeldings-URL** tekstvak typt u de waarde met behulp van het volgende patroon: `https://www.linkedin.com/checkpoint/enterprise/login/<account id>?application=salesNavigator`

1. Uw **LinkedIn Sales Navigator** toepassing verwacht het SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. De volgende Schermafbeelding toont een voorbeeld. De standaardwaarde van **gebruikers-id** is **user.userprincipalname** maar LinkedIn Sales Navigator wordt verwacht dat wordt toegewezen met de e-mailadres van de gebruiker. U kunt **user.mail** kenmerk in de lijst of gebruik de waarde van het juiste kenmerk op basis van de organisatieconfiguratie van uw. 

    ![Eenmalige aanmelding configureren](./media/linkedinsalesnavigator-tutorial/updateusermail.png)
    
1. In **gebruikerskenmerken** sectie, klikt u op **weergeven en bewerken van alle andere gebruikerskenmerken** en de kenmerken instellen. De gebruiker nodig heeft om toe te voegen vier claims met de naam **e**, **afdeling**, **firstname**, en **lastname** en de waarde moet worden toegewezen met **user.mail**, **user.department**, **user.givenname**, en **user.surname** respectievelijk

    | Naam kenmerk | Waarde kenmerk |
    | --- | --- |    
    | e-mail| user.mail |
    | department| user.department |
    | firstname| user.givenname |
    | lastname| user.surname |
    
    ![Het maken van een Azure AD-testgebruiker](./media/linkedinsalesnavigator-tutorial/userattribute.png)
    
    a. Klik op **kenmerk toevoegen** om het dialoogvenster kenmerk te openen.
    
    ![Het maken van een Azure AD-testgebruiker](./media/linkedinsalesnavigator-tutorial/tutorial_attribute_04.png)
    
    ![Het maken van een Azure AD-testgebruiker](./media/linkedinsalesnavigator-tutorial/tutorial_attribute_05.png)
   
    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.
    
    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    d. Klik op **OK**.

1. Voer de volgende stappen uit op de **naam** kenmerk -

    a. Klik op het kenmerk te openen de **kenmerk bewerken** venster.

    ![Eenmalige aanmelding configureren](./media/linkedinsalesnavigator-tutorial/url_update.png)

    b. Verwijderen van de URL-waarde uit de **naamruimte**.
    
    c. Klik op **Ok** om op te slaan van de instelling.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het XML-bestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/linkedinsalesnavigator-tutorial/tutorial_general_400.png)

1. Ga naar het gedeelte **met beheerdersinstellingen voor LinkedIn**. Klik op **uploaden XML-bestand** voor het uploaden van de Metadata-XML-bestand dat u hebt gedownload vanuit Azure portal.

    ![Eenmalige aanmelding configureren](./media/linkedinsalesnavigator-tutorial/tutorial_linkedin_metadata_03.png)

1. Klik op **Aan** om SSO in te schakelen. De SSO-status verandert van **Niet verbonden** naar **Verbonden**

    ![Eenmalige aanmelding configureren](./media/linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_05.png)


> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/linkedinsalesnavigator-tutorial/create_aaduser_01.png) 

1. Ga naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/linkedinsalesnavigator-tutorial/create_aaduser_02.png) 

1. Aan de bovenkant van het dialoogvenster klikt u op **toevoegen** openen de **gebruiker** dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/linkedinsalesnavigator-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/linkedinsalesnavigator-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-linkedin-sales-navigator-test-user"></a>Het maken van een testgebruiker LinkedIn Sales Navigator

Gekoppelde Sales Navigator toepassing biedt ondersteuning voor Just in Time (JIT) inrichten van gebruikers en na-verificatiegebruikers automatisch in de toepassing gemaakt worden. Activeren **automatisch toewijzen van licenties** een licentie toewijzen aan de gebruiker.
   
   ![Het maken van een Azure AD-testgebruiker](./media/linkedinsalesnavigator-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen voor LinkedIn Sales Navigator.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon voor LinkedIn Sales Navigator, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **LinkedIn Sales Navigator**.

    ![Eenmalige aanmelding configureren](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel LinkedIn Sales Navigator in het toegangsvenster, moet u worden omgeleid naar de organisatie-pagina waar u hebt voor uw persoonlijke gegevens van LinkedIn-account. Uw persoonlijke account met uw LinkedIn-bedrijven-account worden gekoppeld. Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_01.png
[2]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_02.png
[3]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_03.png
[4]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_04.png

[100]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_100.png

[200]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_200.png
[201]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_201.png
[202]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_202.png
[203]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_203.png

