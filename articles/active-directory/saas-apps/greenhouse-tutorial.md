---
title: 'Zelfstudie: Azure Active Directory-integratie met handel | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en handel.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 78ec1766-4f79-4f16-9a66-d5584c4b6151
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: da81fed0e3ce38c4a6fba27d550a74cca8edfa6f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55152022"
---
# <a name="tutorial-azure-active-directory-integration-with-greenhouse"></a>Zelfstudie: Azure Active Directory-integratie met handel

In deze zelfstudie leert u hoe u handel integreren met Azure Active Directory (Azure AD).

Handel integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot handel heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij handel (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met handel, moet u de volgende items:

- Een Azure AD-abonnement
- Een handel eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Handel uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-greenhouse-from-the-gallery"></a>Handel uit de galerie toe te voegen
Voor het configureren van de integratie van handel in Azure AD, moet u handel uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen handel uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **handel**, selecteer **handel** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Handel in de lijst met resultaten](./media/greenhouse-tutorial/tutorial_greenhouse_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met handel op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in de handel is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in de handel tot stand worden gebracht.

In de handel, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met handel, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker handel](#create-a-greenhouse-test-user)**  : als u wilt een equivalent van Britta Simon hebben in de handel die is gekoppeld aan de Azure AD-weergave van de gebruiker.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing handel.

**Voor het configureren van Azure AD eenmalige aanmelding met handel, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **handel** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/greenhouse-tutorial/tutorial_greenhouse_samlbase.png)

1. Op de **handel domein en URL's** sectie, voert u de volgende stappen uit:

    ![Handel domein en URL's, eenmalige aanmelding informatie](./media/greenhouse-tutorial/tutorial_greenhouse_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<companyname>.greenhouse.io`

    b. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<companyname>.greenhouse.io`

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de daadwerkelijke aanmeldings-URL en id. Neem contact op met [handel Client ondersteuningsteam](https://www.greenhouse.io/contact) om deze waarden te verkrijgen. 
 


1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/greenhouse-tutorial/tutorial_greenhouse_certificate.png) 

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/greenhouse-tutorial/tutorial_general_400.png)

1. Het configureren van eenmalige aanmelding op **handel** zijde, moet u voor het verzenden van de gedownloade **Metadata XML** naar [handel ondersteuningsteam](https://www.greenhouse.io/contact).

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/greenhouse-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/greenhouse-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/greenhouse-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/greenhouse-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-greenhouse-test-user"></a>Maak een testgebruiker handel

Om in te schakelen in Azure AD-gebruikers zich aanmelden bij handel, moeten ze worden ingericht in de handel. In het geval van handel is inrichten een handmatige taak.

>[!NOTE]
>U kunt alle andere handel gebruiker-account maken van hulpprogramma's of API's geleverd door handel aan inrichten AAD-gebruikersaccounts. 

**Voer de volgende stappen uit om een gebruikersaccount in te richten:**

1. Meld u aan bij uw **handel** bedrijf site als beheerder.

1. Klik in het menu aan de bovenkant op **configureren**, en klik vervolgens op **gebruikers**.
   
   ![Gebruikers](./media/greenhouse-tutorial/ic790791.png "Gebruikers")

1. Klik op **nieuwe gebruikers**.
   
   ![New User](./media/greenhouse-tutorial/ic790792.png "New User")

1. In de **Add New User** sectie, voert u de volgende stappen uit:
   
   ![Nieuwe gebruiker toevoegen](./media/greenhouse-tutorial/ic790793.png "nieuwe gebruiker toevoegen")

   a. In de **Voer e-mailberichten** tekstvak typt u het e-mailadres van een geldige Azure Active Directory-account dat u inrichten wilt.

   b. Klik op **Opslaan**.    
   
      >[!NOTE]
      >De houders van Azure Active Directory-account ontvangt een e-mailbericht ook een koppeling voor het account te bevestigen voordat deze geactiveerd wordt.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan handel.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon aan handel, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **handel**.

    ![De koppeling handel in de lijst met toepassingen](./media/greenhouse-tutorial/tutorial_greenhouse_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel handel in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing handel.
Zie [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/greenhouse-tutorial/tutorial_general_01.png
[2]: ./media/greenhouse-tutorial/tutorial_general_02.png
[3]: ./media/greenhouse-tutorial/tutorial_general_03.png
[4]: ./media/greenhouse-tutorial/tutorial_general_04.png

[100]: ./media/greenhouse-tutorial/tutorial_general_100.png

[200]: ./media/greenhouse-tutorial/tutorial_general_200.png
[201]: ./media/greenhouse-tutorial/tutorial_general_201.png
[202]: ./media/greenhouse-tutorial/tutorial_general_202.png
[203]: ./media/greenhouse-tutorial/tutorial_general_203.png

