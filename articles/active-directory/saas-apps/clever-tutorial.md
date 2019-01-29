---
title: 'Zelfstudie: Azure Active Directory-integratie met Clever | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Clever.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 069ff13a-310e-4366-a147-d6ec5cca12a5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2018
ms.author: jeedes
ms.openlocfilehash: e65f0cb3ef30fb5b001acdb72481c1c3b55ca058
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55197310"
---
# <a name="tutorial-azure-active-directory-integration-with-clever"></a>Zelfstudie: Azure Active Directory-integratie met Clever

In deze zelfstudie leert u hoe u Clever integreren met Azure Active Directory (Azure AD).

Clever integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Clever heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Clever (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Clever, moet u de volgende items:

- Een Azure AD-abonnement
- Een slimme eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Clever uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-clever-from-the-gallery"></a>Clever uit de galerie toe te voegen
Voor het configureren van de integratie van Clever in Azure AD, moet u Clever uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Clever uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **Clever**, selecteer **Clever** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Slimme in de lijst met resultaten](./media/clever-tutorial/tutorial_clever_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Clever op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Clever is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Clever tot stand worden gebracht.

In Clever, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Clever, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een slimme testgebruiker](#create-a-clever-test-user)**  : als u wilt een equivalent van Britta Simon in Clever die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw slimme toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met Clever, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Clever** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![In het dialoogvenster voor eenmalige aanmelding](./media/clever-tutorial/tutorial_clever_samlbase.png)

1. Op de **slimme domein en URL's** sectie, voert u de volgende stappen uit:

    ![Slimme domein en URL's, eenmalige aanmelding informatie](./media/clever-tutorial/tutorial_clever_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://clever.com/in/<companyname>`

    b. In de **id** tekstvak typt u de URL: `https://clever.com/oauth/saml/metadata.xml`

    > [!NOTE]
    > Aanmeldings-URL-waarde is niet echt. Deze waarde bijwerken met de werkelijke aanmeldings-URL. Neem contact op met [slimme Client ondersteuningsteam](https://clever.com/about/contact/) deze waarde op te halen.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op de knop kopiëren om te kopiëren **App-Url voor federatieve metagegevens** en plak deze in Kladblok.
    
    ![Eenmalige aanmelding configureren](./media/clever-tutorial/tutorial_metadataurl.png)

1. De slimme toepassing wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, die vereist dat u om toe te voegen van aangepast kenmerktoewijzingen aan uw **SAML-Token kenmerken** configuratie.

    In de volgende schermopname ziet u een voorbeeld hiervan.

    ![Eenmalige aanmelding configureren](./media/clever-tutorial/tutorial_clever_07.png)

1. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de bovenstaande afbeelding en voer de volgende stappen uit:
    
    | Naam kenmerk  | Waarde kenmerk |
    | --------------- | -------------------- |
    | clever.teacher.credentials.district_username|user.userprincipalname|
    | clever.student.credentials.district_username| user.userprincipalname |
    | Voornaam  | user.givenname |
    | Lastname  | user.surname |

    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/clever-tutorial/tutorial_attribute_04.png)
    
    ![Eenmalige aanmelding configureren](./media/clever-tutorial/tutorial_attribute_05.png)
    
    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Laat de **Namespace** tekstvak leeg.
    
    d. Klik op **OK**.
    
1. Klik op de knop **Save**.

    ![De knop voor enkelvoudige aanmelding configureren](./media/clever-tutorial/tutorial_general_400.png)

1. In een ander browservenster aanmelden bij uw site slimme bedrijf als beheerder.

1. Klik in de werkbalk op **directe aanmelding**.

    ![Directe aanmelding](./media/clever-tutorial/ic798984.png "directe aanmelding")

    > [!NOTE]
    > Voordat u eenmalige aanmelding testen kunt, u moet contact opnemen met [slimme Client ondersteuningsteam](https://clever.com/about/contact/) Office 365 SSO inschakelen in de back-end.

1. Op de **directe aanmelding** pagina, voert u de volgende stappen uit:
    
      ![Directe aanmelding](./media/clever-tutorial/ic798985.png "directe aanmelding")
    
      a. Type de **aanmeldings-URL**.
    
      >[!NOTE]
      >De **aanmeldings-URL** is een aangepaste waarde. Neem contact op met [slimme Client ondersteuningsteam](https://clever.com/about/contact/) deze waarde op te halen.
    
      b. Als **Identiteitssysteem**, selecteer **ADFS**.

      c. In de **metagegevens-URL** tekstvak plakken **App-Url voor federatieve metagegevens** waarde die u hebt gekopieerd vanuit Azure portal.
    
      d. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/clever-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/clever-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/clever-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/clever-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.

### <a name="create-a-clever-test-user"></a>Maak een slimme testgebruiker

Als u wilt dat Azure AD-gebruikers zich aanmelden bij Clever, moeten ze worden ingericht voor Clever.

In het geval van Clever, werken met [slimme Client ondersteuningsteam](https://clever.com/about/contact/) om toe te voegen de gebruikers in de slimme platform. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

>[!NOTE]
>U kunt een andere gebruiker slimme account hulpmiddelen voor het maken of API's wordt geleverd door Clever voor het inrichten van gebruikersaccounts van de Azure AD.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Clever.

![De de gebruikersrol toewijzen][200]

**Als u wilt Britta Simon aan Clever toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

1. Selecteer in de lijst met toepassingen, **Clever**.

    ![De Clever koppelen in de lijst met toepassingen](./media/clever-tutorial/tutorial_clever_app.png)

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u klikt op de slimme tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw slimme toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/clever-tutorial/tutorial_general_01.png
[2]: ./media/clever-tutorial/tutorial_general_02.png
[3]: ./media/clever-tutorial/tutorial_general_03.png
[4]: ./media/clever-tutorial/tutorial_general_04.png

[100]: ./media/clever-tutorial/tutorial_general_100.png

[200]: ./media/clever-tutorial/tutorial_general_200.png
[201]: ./media/clever-tutorial/tutorial_general_201.png
[202]: ./media/clever-tutorial/tutorial_general_202.png
[203]: ./media/clever-tutorial/tutorial_general_203.png

