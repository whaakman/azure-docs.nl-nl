---
title: 'Zelfstudie: Azure Active Directory-integratie met InTime | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en InTime.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: d4e2c6e1-ae5d-4d2c-8ffc-1b24534d376a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: jeedes
ms.openlocfilehash: 05ac96b1b048d6d04b1988414312c2f8e5f41971
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55158142"
---
# <a name="tutorial-azure-active-directory-integration-with-intime"></a>Zelfstudie: Azure Active Directory-integratie met InTime

In deze zelfstudie leert u hoe u InTime integreren met Azure Active Directory (Azure AD).

InTime integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot InTime heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij InTime (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met InTime, moet u de volgende items:

- Een Azure AD-abonnement
- Een InTime eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. InTime uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-intime-from-the-gallery"></a>InTime uit de galerie toe te voegen
Voor het configureren van de integratie van InTime in Azure AD, moet u InTime uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen InTime uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **InTime**, selecteer **InTime** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![InTime in de lijst met resultaten](./media/intime-tutorial/tutorial_intime_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met InTime op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in InTime is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in InTime tot stand worden gebracht.

In InTime, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met InTime, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker InTime](#create-a-intime-test-user)**  : als u wilt een equivalent van Britta Simon in InTime die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw InTime toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met InTime, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **InTime** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/intime-tutorial/tutorial_intime_samlbase.png)

1. Op de **InTime domein en URL's** sectie, voert u de volgende stappen uit:

    ![InTime domein en URL's, eenmalige aanmelding informatie](./media/intime-tutorial/tutorial_intime_url.png)

    a. In de **aanmeldings-URL** tekstvak typt u de URL: `https://intime6.intimesoft.com/mytime/login/login.xhtml`

    b. In de **id** tekstvak typt u de URL: `https://auth.intimesoft.com/auth/realms/master`

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De link om het certificaat te downloaden](./media/intime-tutorial/tutorial_intime_certificate.png) 

1. Uw toepassing InTime wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. In de volgende schermopname ziet u een voorbeeld hiervan. De standaardwaarde van **gebruikers-id** is **user.userprincipalname** maar InTime wordt verwacht dat deze optie om te worden toegewezen met de e-mailadres van de gebruiker. Hiervoor kunt u **user.mail** kenmerk in de lijst of gebruik de waarde van het juiste kenmerk op basis van de organisatieconfiguratie van uw 

    ![Kenmerk configureren](./media/intime-tutorial/tutorial_intime_attribute.png)

1. Klik op de knop **Save**.

    ![De knop voor enkelvoudige aanmelding configureren](./media/intime-tutorial/tutorial_general_400.png)

1. Op de **InTime configuratie** sectie, klikt u op **InTime configureren** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![InTime configuratie](./media/intime-tutorial/tutorial_intime_configure.png) 

1. Het configureren van eenmalige aanmelding op **InTime** zijde, moet u voor het verzenden van de gedownloade **Metadata XML**, **afmelding-URL en Single Sign-On Service URL voor SAML-** naar [ InTime ondersteuningsteam](mailto:hdollard@intimesoft.com). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/intime-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/intime-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/intime-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/intime-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-intime-test-user"></a>Maak een testgebruiker InTime

In deze sectie maakt u een gebruiker met de naam van Britta Simon in InTime. Werken met [InTime ondersteuningsteam](mailto:hdollard@intimesoft.com) om toe te voegen de gebruikers in het InTime-platform. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan InTime.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan InTime toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **InTime**.

    ![De InTime koppelen in de lijst met toepassingen](./media/intime-tutorial/tutorial_intime_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u klikt op de tegel InTime in het toegangsvenster, krijgt u de aanmeldingspagina van uw InTime toepassing. Klik op de **aanmelding** knop, en vervolgens een reeks van id-providers wordt weergegeven op een lijst met knoppen. Klik op **IDP naam** verstrekt door [InTime ondersteuningsteam](mailto:hdollard@intimesoft.com) om aan te melden bij uw InTime toepassing. Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/intime-tutorial/tutorial_general_01.png
[2]: ./media/intime-tutorial/tutorial_general_02.png
[3]: ./media/intime-tutorial/tutorial_general_03.png
[4]: ./media/intime-tutorial/tutorial_general_04.png

[100]: ./media/intime-tutorial/tutorial_general_100.png

[200]: ./media/intime-tutorial/tutorial_general_200.png
[201]: ./media/intime-tutorial/tutorial_general_201.png
[202]: ./media/intime-tutorial/tutorial_general_202.png
[203]: ./media/intime-tutorial/tutorial_general_203.png

