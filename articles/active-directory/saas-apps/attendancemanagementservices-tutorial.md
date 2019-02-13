---
title: 'Zelfstudie: Azure Active Directory-integratie met aanwezigheid | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en aanwezigheid Management Services.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 1f56e612-728b-4203-a545-a81dc5efda00
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b7b8b157b890bcdda8c06a0dd1b208a82f7f9058
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56209129"
---
# <a name="tutorial-azure-active-directory-integration-with-attendance-management-services"></a>Zelfstudie: Azure Active Directory-integratie met aanwezigheid

In deze zelfstudie leert u hoe u aanwezigheid Management Services integreren met Azure Active Directory (Azure AD).

Aanwezigheid Management Services integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot aanwezigheid Management Services heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij aanwezigheid Management Services (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met aanwezigheid Management Services, moet u de volgende items:

- Een Azure AD-abonnement
- Een aanwezigheid beheerservices eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Aanwezigheid Management-Services uit de galerie toevoegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-attendance-management-services-from-the-gallery"></a>Aanwezigheid Management-Services uit de galerie toevoegen
Voor het configureren van de integratie van aanwezigheid beheerservices in Azure AD, moet u aanwezigheid Management-Services uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen aanwezigheid Management-Services uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **aanwezigheid beheerservices**, selecteer **aanwezigheid Management Services** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Aanwezigheid Management-Services in de lijst met resultaten](./media/attendancemanagementservices-tutorial/tutorial_attendancemanagementservices_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met aanwezigheid Management Services op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in aanwezigheid beheerservices in Azure AD aan een gebruiker is. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in aanwezigheid Management-Services tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met aanwezigheid Management Services, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **Maak een testgebruiker aanwezigheid beheerservices** : als u wilt een equivalent van Britta Simon in aanwezigheid-beheerservices die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw aanwezigheid Management Services-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met aanwezigheid Management Services, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **aanwezigheid beheerservices** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/attendancemanagementservices-tutorial/tutorial_attendancemanagementservices_samlbase.png)

1. Op de **aanwezigheid Management Services-domein en URL's** sectie, voert u de volgende stappen uit:

    ![Aanwezigheid Management Services-domein en URL's, eenmalige aanmelding informatie](./media/attendancemanagementservices-tutorial/tutorial_attendancemanagementservices_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://id.obc.jp/<tenant information >/`

    b. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://id.obc.jp/<tenant information >/`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de daadwerkelijke aanmeldings-URL en id. Neem contact op met [aanwezigheid Management Services Client ondersteuningsteam](https://www.obcnet.jp/) om deze waarden te verkrijgen.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![De link om het certificaat te downloaden](./media/attendancemanagementservices-tutorial/tutorial_attendancemanagementservices_certificate.png) 

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/attendancemanagementservices-tutorial/tutorial_general_400.png)

1. Op de **aanwezigheid Management Services-configuratie** sectie, klikt u op **aanwezigheid Management-Services configureren** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Aanwezigheid Management Services-configuratie](./media/attendancemanagementservices-tutorial/tutorial_attendancemanagementservices_configure.png) 

1. In een ander browservenster aanmelden voor uw bedrijf aanwezigheid Management Services site als administrator.

1. Klik op **SAML-verificatie** onder de **Security beheersectie**.

    ![Aanwezigheid Management Services-configuratie](./media/attendancemanagementservices-tutorial/user1.png)

1. Voer de volgende stappen uit:

    ![Aanwezigheid Management Services-configuratie](./media/attendancemanagementservices-tutorial/user2.png)

    a. Selecteer **gebruik SAML-verificatie**.

    b. In de **id** tekstvak, plak de waarde van **SAML entiteit-ID**, die u hebt gekopieerd vanuit Azure portal. 

    c. In de **eindpunt-URL webverificatie** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML**, die u hebt gekopieerd vanuit Azure portal.

    d. Klik op **selecteert u een bestand** voor het uploaden van het certificaat dat u hebt gedownload van Azure AD.

    e. Selecteer **uitschakelen wachtwoordverificatie**.

    f. Klik op **registratie**

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt! Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/attendancemanagementservices-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/attendancemanagementservices-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/attendancemanagementservices-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/attendancemanagementservices-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-attendance-management-services-test-user"></a>Maak een testgebruiker aanwezigheid Management Services

Als u wilt dat Azure AD-gebruikers kunnen zich aanmelden bij aanwezigheid Management Services, moeten deze zijn ingericht in aanwezigheid Management Services. In het geval van aanwezigheid Management Services, met de inrichting is een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Meld u aan bij uw bedrijf aanwezigheid Management Services site aan als beheerder.

1. Klik op **Gebruikersbeheer** onder de **Security beheersectie**.

    ![Werknemer toevoegen](./media/attendancemanagementservices-tutorial/user5.png)

1. Klik op **nieuwe regels aanmelding**.

    ![Werknemer toevoegen](./media/attendancemanagementservices-tutorial/user3.png)

1. In de **OBCiD informatie** sectie, voert u de volgende stappen uit:

    ![Werknemer toevoegen](./media/attendancemanagementservices-tutorial/user4.png)

    a. In de **OBCiD** tekstvak, typ het e-mailadres van gebruiker, zoals **BrittaSimon@contoso.com**.

    b. In het tekstvak **Wachtwoord** typt u het wachtwoord van de gebruiker.

    c. Klik op **registratie**


### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen voor aanwezigheid Management Services.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon en aanwezigheid Management-Services, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **aanwezigheid beheerservices**.

    ![De aanwezigheid Management Services-koppeling in de lijst met toepassingen](./media/attendancemanagementservices-tutorial/tutorial_attendancemanagementservices_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de aanwezigheid Management Services-tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw aanwezigheid Management Services-toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/attendancemanagementservices-tutorial/tutorial_general_01.png
[2]: ./media/attendancemanagementservices-tutorial/tutorial_general_02.png
[3]: ./media/attendancemanagementservices-tutorial/tutorial_general_03.png
[4]: ./media/attendancemanagementservices-tutorial/tutorial_general_04.png

[100]: ./media/attendancemanagementservices-tutorial/tutorial_general_100.png

[200]: ./media/attendancemanagementservices-tutorial/tutorial_general_200.png
[201]: ./media/attendancemanagementservices-tutorial/tutorial_general_201.png
[202]: ./media/attendancemanagementservices-tutorial/tutorial_general_202.png
[203]: ./media/attendancemanagementservices-tutorial/tutorial_general_203.png

