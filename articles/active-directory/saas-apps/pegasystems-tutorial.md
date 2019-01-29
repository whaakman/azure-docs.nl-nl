---
title: 'Zelfstudie: Azure Active Directory-integratie met Pega systemen | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Pega systemen.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 31acf80f-1f4b-41f1-956f-a9fbae77ee69
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/16/2017
ms.author: jeedes
ms.openlocfilehash: abeffacedd8b3cae9802e5a8c264748df313abf0
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55196732"
---
# <a name="tutorial-azure-active-directory-integration-with-pega-systems"></a>Zelfstudie: Azure Active Directory-integratie met Pega systemen

In deze zelfstudie leert u hoe u Pega systemen integreren met Azure Active Directory (Azure AD).

Pega systemen integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Pega systemen heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Pega-systemen (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Pega systemen, moet u de volgende items:

- Een Azure AD-abonnement
- Een Pega systemen eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Pega systemen uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-pega-systems-from-the-gallery"></a>Pega systemen uit de galerie toe te voegen
Voor het configureren van de integratie van Pega systemen in Azure AD, moet u Pega systemen uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Pega systemen uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **Pega systemen**, selecteer **Pega systemen** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Pega systemen in de lijst met resultaten](./media/pegasystems-tutorial/tutorial_pegasystems_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Pega systemen op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Pega systemen is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Pega systemen tot stand worden gebracht.

In systemen Pega, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Pega systemen, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker Pega systemen](#create-a-pega-systems-test-user)**  : als u wilt een equivalent van Britta Simon in Pega systemen die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Pega systemen.

**Voor het configureren van Azure AD eenmalige aanmelding met Pega systemen, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Pega systemen** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/pegasystems-tutorial/tutorial_pegasystems_samlbase.png)

1. Op de **Pega systemen domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Pega systemen domein en URL's, eenmalige aanmelding informatie](./media/pegasystems-tutorial/tutorial_pegasystems_url.png)

    a. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<CUSTOMERNAME>.pegacloud.io:443/prweb/sp/<INSTANCEID>`

    b. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://<CUSTOMERNAME>.pegacloud.io:443/prweb/PRRestService/WebSSO/SAML/AssertionConsumerService`

1. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Pega systemen domein en URL's, eenmalige aanmelding informatie](./media/pegasystems-tutorial/tutorial_pegasystems_url1.png)

    In de **Relaystatus** tekstvak, een URL met behulp van het volgende patroon: `https://<CUSTOMERNAME>.pegacloud.io/prweb/sso`
     
    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden met de werkelijke-id, de antwoord-URL en Relay status URL. Hier vindt u de waarden van de id en antwoord-URL van Pega-toepassing die verderop in deze zelfstudie wordt uitgelegd. Voor de Relay-status, neem contact op met [Pega systemen Client ondersteuningsteam](https://www.pega.com/contact-us) om de waarde. 

1. De toepassing Pega systemen wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. Deze claims specifieke-klant bent en is afhankelijk van uw behoeften. Volgende optioneel claims voorbeeld worden alleen die u kunt configureren voor uw toepassing. U kunt de waarden van deze kenmerken vanuit beheren de "**gebruikerskenmerken**" sectie op de pagina van de toepassing-integratie. 

    ![Eenmalige aanmelding configureren](./media/pegasystems-tutorial/tutorial_attribute.png)

1. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de voorgaande afbeelding en voer de volgende stappen uit:
    
    | Naam kenmerk | Waarde kenmerk |
    | ------------------- | -------------------- |    
    | uid | *********** |
    | algemene naam  | *********** |
    | mail | *********** |
    | accessgroup | *********** |
    | organisatie | *********** |
    | orgdivision | *********** |
    | orgunit | *********** |
    | Werkgroep | *********** |
    | Telefoon | *********** |

    > [!NOTE]
    > Dit zijn de specifieke waarden klant. Geef de juiste waarden.

    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/pegasystems-tutorial/tutorial_attribute_04.png)

    ![Eenmalige aanmelding configureren](./media/pegasystems-tutorial/tutorial_attribute_05.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    d. Klik op **OK**.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/pegasystems-tutorial/tutorial_pegasystems_certificate.png) 
1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/pegasystems-tutorial/tutorial_general_400.png)
    
1. Het configureren van eenmalige aanmelding op **Pega systemen** aan clientzijde, open de **Pega Portal** met beheerdersaccount in een ander browservenster.

1. Selecteer **maken** -> **SysAdmin** -> **verificatieservice**.

    ![De knop voor enkelvoudige aanmelding configureren](./media/pegasystems-tutorial/tutorial_pegasystems_admin.png)
    
1. De volgende acties uitvoeren op **Aauthentication Service maken** scherm:

    ![De knop voor enkelvoudige aanmelding configureren](./media/pegasystems-tutorial/tutorial_pegasystems_admin1.png)

    a. Selecteer **SAML 2.0** van het Type

    b. In de **naam** tekstvak, voer een naam bijvoorbeeld Azure AD-eenmalige aanmelding

    c. In de **korte beschrijving** tekstvak, een beschrijving invoeren  

    d. Klik op **maken en openen** 
    
1. In **id-Provider (IdP) informatie** sectie, klikt u op **metagegevens importeren IdP** en blader naar het metagegevensbestand die u hebt gedownload vanuit Azure portal. Klik op **indienen** laden van de metagegevens.

    ![De knop voor enkelvoudige aanmelding configureren](./media/pegasystems-tutorial/tutorial_pegasystems_admin2.png)
    
1. Dit vult de IdP-gegevens, zoals hieronder weergegeven.

    ![De knop voor enkelvoudige aanmelding configureren](./media/pegasystems-tutorial/tutorial_pegasystems_admin3.png)
    
1. De volgende acties uitvoeren op **instellingen voor Service Provider (SP)** sectie:

    ![De knop voor enkelvoudige aanmelding configureren](./media/pegasystems-tutorial/tutorial_pegasystems_admin4.png)

    a. Kopieer de **entiteits-id** waarde en plak terug in Azure-Portal **id** tekstvak.

    b.  Kopieer de **Assertion Consumer Service (ACS) locatie** waarde en plak terug in Azure-Portal **antwoord-URL** tekstvak.

    c. Selecteer **aanvraag ondertekening uitschakelen**.

1. Klik op **Opslaan**.
    
> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/pegasystems-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/pegasystems-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/pegasystems-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/pegasystems-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-pega-systems-test-user"></a>Maak een testgebruiker Pega systemen

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Pega systemen. Neem contact op met [Pega systemen Client ondersteuningsteam](https://www.pega.com/contact-us) in Pega Sysyems gebruikers kunnen maken.


### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot Pega systemen.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon Pega systemen, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Pega systemen**.

    ![De koppeling Pega systemen in de lijst met toepassingen](./media/pegasystems-tutorial/tutorial_pegasystems_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Pega systemen in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Pega systemen.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/pegasystems-tutorial/tutorial_general_01.png
[2]: ./media/pegasystems-tutorial/tutorial_general_02.png
[3]: ./media/pegasystems-tutorial/tutorial_general_03.png
[4]: ./media/pegasystems-tutorial/tutorial_general_04.png

[100]: ./media/pegasystems-tutorial/tutorial_general_100.png

[200]: ./media/pegasystems-tutorial/tutorial_general_200.png
[201]: ./media/pegasystems-tutorial/tutorial_general_201.png
[202]: ./media/pegasystems-tutorial/tutorial_general_202.png
[203]: ./media/pegasystems-tutorial/tutorial_general_203.png

