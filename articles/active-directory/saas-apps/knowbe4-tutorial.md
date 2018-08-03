---
title: 'Zelfstudie: Azure Active Directory-integratie met KnowBe4 Security Awareness Training | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en KnowBe4 Security Awareness Training.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: b80d2212-cc5f-4adb-836c-570640810c39
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/31/2017
ms.author: jeedes
ms.openlocfilehash: c27af4e7bc88f24b76310336859740d8795f7cbe
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39449270"
---
# <a name="tutorial-azure-active-directory-integration-with-knowbe4-security-awareness-training"></a>Zelfstudie: Azure Active Directory-integratie met KnowBe4 Security Awareness Training

In deze zelfstudie leert u hoe u KnowBe4-Awareness beveiligingstraining integreren met Azure Active Directory (Azure AD).

KnowBe4-Awareness beveiligingstraining integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot KnowBe4 Security Awareness Training heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij KnowBe4-Awareness beveiligingstraining (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met KnowBe4 Security Awareness Training, moet u de volgende items:

- Een Azure AD-abonnement
- Een KnowBe4-Awareness beveiligingstraining eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. KnowBe4-Awareness beveiligingstraining uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-knowbe4-security-awareness-training-from-the-gallery"></a>KnowBe4-Awareness beveiligingstraining uit de galerie toe te voegen
Voor het configureren van de integratie van KnowBe4 Security Awareness Training in Azure AD, moet u KnowBe4-Awareness beveiligingstraining uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen KnowBe4-Awareness beveiligingstraining uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

1. Typ in het zoekvak **KnowBe4-Awareness beveiligingstraining**, selecteer **KnowBe4-Awareness beveiligingstraining** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen de de toepassing.

    ![KnowBe4 Security Awareness Training in de lijst met resultaten](./media/knowbe4-tutorial/tutorial_knowbe4_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met KnowBe4 Security Awareness Training op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in KnowBe4 Security Awareness Training is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in KnowBe4-Awareness beveiligingstraining tot stand worden gebracht.

In KnowBe4 Security Awareness Training, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met KnowBe4 Security Awareness Training, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Maak een testgebruiker KnowBe4-Awareness beveiligingstraining](#create-a-knowbe4-security-awareness-training-test-user)**  : als u wilt een equivalent van Britta Simon in KnowBe4-Awareness beveiligingstraining die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing KnowBe4 Security Awareness Training.

**Voor het configureren van Azure AD eenmalige aanmelding met KnowBe4 Security Awareness Training, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **KnowBe4-Awareness beveiligingstraining** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/knowbe4-tutorial/tutorial_knowbe4_samlbase.png)

1. Op de **KnowBe4 Security Awareness Training domein en URL's** sectie, voert u de volgende stappen uit:

    ![KnowBe4 Security Awareness Training domein en URL's eenmalige aanmelding informatie](./media/knowbe4-tutorial/tutorial_knowbe4_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<companyname>.KnowBe4.com/auth/saml/<instancename>`

    > [!NOTE] 
    > Aanmeldings-URL-waarde is niet echt. Deze waarde bijwerken met de werkelijke aanmeldings-URL. Neem contact op met [Awareness KnowBe4-Training Beveiligingsclient ondersteuningsteam](mailto:support@KnowBe4.com) deze waarde op te halen. 

    b. In de **id** tekstvak typt u de tekenreekswaarde: `KnowBe4`

    > [!NOTE]
    >Dit is hoofdlettergevoelig

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Raw)** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/knowbe4-tutorial/tutorial_knowbe4_certificate.png) 

1. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/knowbe4-tutorial/tutorial_general_400.png)

1. Op de **Awareness KnowBe4-Training Beveiligingsconfiguratie** sectie, klikt u op **configureren KnowBe4-Awareness beveiligingstraining** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Bewustzijn KnowBe4-Training Beveiligingsconfiguratie](./media/knowbe4-tutorial/tutorial_knowbe4_configure.png) 

1. Het configureren van eenmalige aanmelding op **KnowBe4-Awareness beveiligingstraining** zijde, moet u voor het verzenden van de gedownloade **certificaat (Raw)**, **afmelding-URL, SAML-entiteit-ID en SAML Single Sign-On Service-URL** naar [Awareness KnowBe4-Training Beveiligingsclient ondersteuningsteam](mailto:support@KnowBe4.com).

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/knowbe4-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/knowbe4-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/knowbe4-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/knowbe4-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-knowbe4-security-awareness-training-test-user"></a>Maak een testgebruiker KnowBe4 Security Awareness Training

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in KnowBe4 Security Awareness Training. KnowBe4 Security Awareness Training biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld.

Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot KnowBe4-Awareness beveiligingstraining als deze nog niet bestaat. 

>[!NOTE]
>Als u een gebruiker handmatig hebt gemaakt wilt, moet u contact op met de [KnowBe4-Awareness beveiligingstraining ondersteuningsteam](mailto:support@KnowBe4.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot KnowBe4 Security Awareness Training.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon tot KnowBe4 Security Awareness Training, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **KnowBe4-Awareness beveiligingstraining**.

    ![De koppeling KnowBe4 Security Awareness Training in de lijst met toepassingen](./media/knowbe4-tutorial/tutorial_knowbe4_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie via het toegangsvenster.
  
Wanneer u op de tegel KnowBe4 Security Awareness Training in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing KnowBe4 Security Awareness Training. 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/knowbe4-tutorial/tutorial_general_01.png
[2]: ./media/knowbe4-tutorial/tutorial_general_02.png
[3]: ./media/knowbe4-tutorial/tutorial_general_03.png
[4]: ./media/knowbe4-tutorial/tutorial_general_04.png

[100]: ./media/knowbe4-tutorial/tutorial_general_100.png

[200]: ./media/knowbe4-tutorial/tutorial_general_200.png
[201]: ./media/knowbe4-tutorial/tutorial_general_201.png
[202]: ./media/knowbe4-tutorial/tutorial_general_202.png
[203]: ./media/knowbe4-tutorial/tutorial_general_203.png

