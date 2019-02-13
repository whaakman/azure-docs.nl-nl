---
title: 'Zelfstudie: Azure Active Directory-integratie met Kantega SSO voor FishEye/filterkroes | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Kantega SSO voor FishEye/filterkroes.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 9fe951fd-1530-4d33-a1a4-390385b99ce9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e8205fe300cb88dc9cced5f5a75e692700f9f34b
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56166335"
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-fisheyecrucible"></a>Zelfstudie: Azure Active Directory-integratie met Kantega SSO voor FishEye/filterkroes

In deze zelfstudie leert u hoe u Kantega SSO voor FishEye/filterkroes integreren met Azure Active Directory (Azure AD).

Kantega SSO voor FishEye/filterkroes integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Kantega SSO voor FishEye/filterkroes heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Kantega SSO voor FishEye/filterkroes (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Kantega SSO voor FishEye/filterkroes, moet u de volgende items:

- Een Azure AD-abonnement
- Een SSO Kantega voor eenmalige aanmelding FishEye/filterkroes ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Kantega SSO voor FishEye/filterkroes uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-kantega-sso-for-fisheyecrucible-from-the-gallery"></a>Kantega SSO voor FishEye/filterkroes uit de galerie toe te voegen
Voor het configureren van de integratie van Kantega SSO voor FishEye/filterkroes in Azure AD, moet u Kantega SSO voor FishEye/filterkroes uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Kantega SSO voor FishEye/filterkroes uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **Kantega SSO voor FishEye/filterkroes**.

    ![Het maken van een Azure AD-testgebruiker](./media/kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_search.png)

1. Selecteer in het deelvenster resultaten **Kantega SSO voor FishEye/filterkroes**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Kantega SSO voor FishEye/filterkroes op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Kantega SSO voor FishEye/filterkroes is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Kantega SSO voor FishEye/filterkroes tot stand worden gebracht.

In Kantega SSO voor FishEye/filterkroes, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Kantega SSO voor FishEye/filterkroes, u nodig hebt voor de volgende bouwstenen:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een SSO Kantega voor de testgebruiker FishEye/filterkroes](#creating-a-kantega-sso-for-fisheyecrucible-test-user)**  : als u wilt een equivalent van Britta Simon in Kantega SSO voor FishEye/filterkroes die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en eenmalige aanmelding in uw Kantega SSO voor FishEye/filterkroes toepassing configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Kantega SSO voor FishEye/filterkroes, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Kantega SSO voor FishEye/filterkroes** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_samlbase.png)

1. In **IDP** modus gestart op de **Kantega SSO voor FishEye/filterkroes domein en URL's** sectie de volgende stap uitvoeren:

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_url1.png)

    a. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    b. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

1. In **SP** gestart modus selectievakje **geavanceerde URL-instellingen weergeven** en voer de volgende stap:

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_url2.png)

    Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`
     
    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke id, antwoord-URL en aanmeldings-URL. Deze waarden zijn ontvangen tijdens de configuratie van FishEye/filterkroes-invoegtoepassing die later in de zelfstudie wordt uitgelegd.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/tutorial_general_400.png)
    
1. In een ander browservenster aan te melden bij uw FishEye/filterkroes on-premises server als beheerder.

1. Wijs het tandwiel aan met de muisaanwijzer en klik op **Add-ons**.

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/addon1.png)

1. Systeeminstellingen sectie, klikt u op **nieuwe invoegtoepassingen zoeken**. 

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/add-on2.png)

1. Search **Kantega SSO voor filterkroes** en klikt u op **installeren** knop voor het installeren van de nieuwe SAML-invoegtoepassing.

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/addon2.png)

1. De installatie van de invoegtoepassing wordt gestart. 

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/addon33.png)

1. Nadat de installatie voltooid is. Klik op **Sluiten**.

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/addon34.png)

1.  Klik op **Beheren**.

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/addon35.png)

1. Klik op **Configure** om de nieuwe invoegtoepassing te configureren. 

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/addon3.png)

1. In de **SAML** sectie. Selecteer **Azure Active Directory (Azure AD)** uit de **id-provider toevoegen** vervolgkeuzelijst.

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/addon4.png)

1. Selecteer het abonnementsniveau van als **Basic**.

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/addon5.png)

1. Op de **eigenschappen van de App** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/addon6.png)

    a. Kopieer de **App ID URI** waarde en deze gebruiken als **-id, de antwoord-URL en de aanmeldings-URL** op de **Kantega SSO voor FishEye/filterkroes domein en URL's** sectie in Azure portal.

    b. Klik op **volgende**.

1. Op de **metagegevens importeren** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/addon7.png)

    a. Selecteer **metagegevensbestand op mijn computer**, en het bestand met metagegevens uploaden, die u hebt gedownload vanuit Azure portal.

    b. Klik op **volgende**.

1. Op de **naam en de SSO-locatie** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/addon8.png)

    a. Naam van de id-Provider in **identiteit providernaam** tekstvak (bijvoorbeeld Azure AD).

    b. Klik op **volgende**.

1. Controleer of het certificaat voor ondertekening en klikt u op **volgende**.   

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/addon9.png)

1. Op de **vissenoog gebruikersaccounts** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/addon10.png)

    a. Selecteer **gebruikers in de FishEye interne Directory maken, indien nodig** en voer de juiste naam van de groep voor gebruikers (kan meerdere Nee. van groepen met door komma's gescheiden).

    b. Klik op **volgende**.

1. Klik op **Voltooien**.

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/addon11.png)

1. Op de **bekend domeinen voor Azure AD** sectie, voert u de volgende stappen uit:  

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/addon12.png)

    a. Selecteer **bekend domeinen** in het linkerdeelvenster van de pagina.

    b. Voer de domeinnaam van het in de **bekend domeinen** tekstvak.

    c. Klik op **Opslaan**.  

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/kantegassoforfisheyecrucible-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/kantegassoforfisheyecrucible-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/kantegassoforfisheyecrucible-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/kantegassoforfisheyecrucible-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-kantega-sso-for-fisheyecrucible-test-user"></a>Het maken van een SSO Kantega voor FishEye/filterkroes testgebruiker

Als u wilt dat Azure AD-gebruikers zich aanmelden bij FishEye/filterkroes, moeten ze worden ingericht voor FishEye/filterkroes. In Kantega SSO voor FishEye/filterkroes is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Meld u aan bij uw on-premises filterkroes server aan als beheerder.

1. Beweeg de muisaanwijzer op het tandwiel en klikt u op de **gebruikers**.

    ![Werknemer toevoegen](./media/kantegassoforfisheyecrucible-tutorial/user1.png) 

1. Onder **gebruikers** tabblad sectie, klikt u op **gebruiker toevoegen**.

    ![Werknemer toevoegen](./media/kantegassoforfisheyecrucible-tutorial/user2.png)

1. Op de **Add New User** dialoogvenster pagina, voert u de volgende stappen uit:

    ![Werknemer toevoegen](./media/kantegassoforfisheyecrucible-tutorial/user3.png) 

    a. In het tekstvak **Gebruikersnaam** typt u het e-mailadres van de gebruiker, bijvoorbeeld Brittasimon@contoso.com.
    
    b. In de **weergavenaam** tekstvak weergavenaam van de gebruiker, zoals Britta Simon.
    
    c. Typ in het tekstvak **Email address** het e-mailadres van de gebruiker, bijvoorbeeld Brittasimon@contoso.com.

    d. In het tekstvak **Wachtwoord** typt u het wachtwoord van de gebruiker.  

    e. In de **wachtwoord bevestigen** tekstvak, het wachtwoord van gebruiker opnieuw invoeren.

    f. Klik op **Add**.   

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Kantega SSO voor FishEye/filterkroes.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Kantega SSO voor FishEye/filterkroes, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Kantega SSO voor FishEye/filterkroes**.

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de SSO Kantega FishEye/filterkroes tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw Kantega SSO voor FishEye/filterkroes toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/kantegassoforfisheyecrucible-tutorial/tutorial_general_01.png
[2]: ./media/kantegassoforfisheyecrucible-tutorial/tutorial_general_02.png
[3]: ./media/kantegassoforfisheyecrucible-tutorial/tutorial_general_03.png
[4]: ./media/kantegassoforfisheyecrucible-tutorial/tutorial_general_04.png

[100]: ./media/kantegassoforfisheyecrucible-tutorial/tutorial_general_100.png

[200]: ./media/kantegassoforfisheyecrucible-tutorial/tutorial_general_200.png
[201]: ./media/kantegassoforfisheyecrucible-tutorial/tutorial_general_201.png
[202]: ./media/kantegassoforfisheyecrucible-tutorial/tutorial_general_202.png
[203]: ./media/kantegassoforfisheyecrucible-tutorial/tutorial_general_203.png

