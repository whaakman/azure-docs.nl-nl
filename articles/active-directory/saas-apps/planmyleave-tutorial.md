---
title: 'Zelfstudie: Azure Active Directory-integratie met PlanMyLeave | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en PlanMyLeave.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: b0d31cbe-7ae2-488b-9cf3-4927391fa744
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: d578367ef3b1f55715841bf51a3baeef28f198e9
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56207038"
---
# <a name="tutorial-azure-active-directory-integration-with-planmyleave"></a>Zelfstudie: Azure Active Directory-integratie met PlanMyLeave

In deze zelfstudie leert u hoe u PlanMyLeave integreren met Azure Active Directory (Azure AD).

PlanMyLeave integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot PlanMyLeave heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij PlanMyLeave (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met PlanMyLeave, moet u de volgende items:

- Een Azure AD-abonnement
- Een PlanMyLeave eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. PlanMyLeave uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-planmyleave-from-the-gallery"></a>PlanMyLeave uit de galerie toe te voegen
Voor het configureren van de integratie van PlanMyLeave in Azure AD, moet u PlanMyLeave uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen PlanMyLeave uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **PlanMyLeave**, selecteer **PlanMyLeave** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![PlanMyLeave in de lijst met resultaten](./media/planmyleave-tutorial/tutorial_planmyleave_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met PlanMyLeave op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in PlanMyLeave is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in PlanMyLeave tot stand worden gebracht.

In PlanMyLeave, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met PlanMyLeave, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker PlanMyLeave](#create-a-planmyleave-test-user)**  : als u wilt een equivalent van Britta Simon in PlanMyLeave die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing PlanMyLeave.

**Voor het configureren van Azure AD eenmalige aanmelding met PlanMyLeave, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **PlanMyLeave** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/planmyleave-tutorial/tutorial_planmyleave_samlbase.png)

1. Op de **PlanMyLeave domein en URL's** sectie, voert u de volgende stappen uit:

    ![PlanMyLeave domein en URL's, eenmalige aanmelding informatie](./media/planmyleave-tutorial/tutorial_planmyleave_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<company-name>.planmyleave.com/Login.aspx`

    b. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<company-name>.planmyleave.com`

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de daadwerkelijke aanmeldings-URL en id. Neem contact op met [PlanMyLeave Client ondersteuningsteam](mailto:support@planmyleave.com) om deze waarden te verkrijgen. 
 
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/planmyleave-tutorial/tutorial_planmyleave_certificate.png) 

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/planmyleave-tutorial/tutorial_general_400.png)

1. Op de **PlanMyLeave configuratie** sectie, klikt u op **configureren PlanMyLeave** openen **aanmelding configureren** venster. Kopiëren de **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![PlanMyLeave Configuration](./media/planmyleave-tutorial/tutorial_planmyleave_configure.png) 
1. In een ander browservenster, meld u aan bij uw tenant PlanMyLeave als beheerder.

1. Ga naar **Setup van System**. Klik op de **beveiligingsbeheer** sectie Klik **bedrijf SAML-instellingen** .

    ![Eenmalige aanmelding aan app-zijde configureren](./media/planmyleave-tutorial/tutorial_planmyleave_002.png) 

1. Op de **SAML-instellingen** sectie, klikt u op het pictogram van de editor.

    ![Eenmalige aanmelding aan app-zijde configureren](./media/planmyleave-tutorial/tutorial_planmyleave_003.png)

1. Op de **Update SAML-instellingen** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding aan app-zijde configureren](./media/planmyleave-tutorial/tutorial_planmyleave_004.png)

    a.  In de **aanmeldings-URL** tekstvak plakken **Single Sign-On Service URL voor SAML** die u hebt gekopieerd vanuit Azure portal.

    b.  Open uw gedownloade metagegevens kopie **X509Certificate** waarde en plak deze naar de **certificaat** tekstvak.

    c. Stel '**Is inschakelen**'naar'**Ja**'.

    d. Klik op **Opslaan**. 

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/planmyleave-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/planmyleave-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/planmyleave-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/planmyleave-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-planmyleave-test-user"></a>Maak een testgebruiker PlanMyLeave

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in PlanMyLeave. PlanMyLeave biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld.

Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker wordt gemaakt tijdens een poging tot toegang tot PlanMyLeave als deze nog niet bestaat.

> [!NOTE]
> Als u een gebruiker handmatig hebt gemaakt wilt, moet u contact opnemen met [PlanMyLeave ondersteuningsteam](mailto:support@planmyleave.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan PlanMyLeave.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan PlanMyLeave toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **PlanMyLeave**.

    ![De koppeling PlanMyLeave in de lijst met toepassingen](./media/planmyleave-tutorial/tutorial_planmyleave_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel PlanMyLeave in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing PlanMyLeave.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/planmyleave-tutorial/tutorial_general_01.png
[2]: ./media/planmyleave-tutorial/tutorial_general_02.png
[3]: ./media/planmyleave-tutorial/tutorial_general_03.png
[4]: ./media/planmyleave-tutorial/tutorial_general_04.png

[100]: ./media/planmyleave-tutorial/tutorial_general_100.png

[200]: ./media/planmyleave-tutorial/tutorial_general_200.png
[201]: ./media/planmyleave-tutorial/tutorial_general_201.png
[202]: ./media/planmyleave-tutorial/tutorial_general_202.png
[203]: ./media/planmyleave-tutorial/tutorial_general_203.png

