---
title: 'Zelfstudie: Azure Active Directory-integratie met kwaliteit NetBenefits | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en betrouwbaarheid NetBenefits.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 77dc8a98-c0e7-4129-ab88-28e7643e432a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2018
ms.author: jeedes
ms.openlocfilehash: 00b90dae4acfae3f8250b94be1f8752aa72bcfe1
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55174887"
---
# <a name="tutorial-azure-active-directory-integration-with-fidelity-netbenefits"></a>Zelfstudie: Azure Active Directory-integratie met kwaliteit NetBenefits

In deze zelfstudie leert u hoe u betrouwbare en NetBenefits integreren met Azure Active Directory (Azure AD).

Kwaliteit NetBenefits integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot kwaliteit NetBenefits heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij kwaliteit NetBenefits (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met kwaliteit NetBenefits, moet u de volgende items:

- Een Azure AD-abonnement
- Een betrouwbare en NetBenefits eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving.
Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Kwaliteit NetBenefits uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-fidelity-netbenefits-from-the-gallery"></a>Kwaliteit NetBenefits uit de galerie toe te voegen

Voor het configureren van de integratie van betrouwbaarheid NetBenefits in Azure AD, moet u nauwkeurige NetBenefits uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen kwaliteit NetBenefits uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

4. Typ in het zoekvak **kwaliteit NetBenefits**, selecteer **kwaliteit NetBenefits** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Kwaliteit NetBenefits in de lijst met resultaten](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureren en testen Azure AD eenmalige aanmelding met kwaliteit NetBenefits op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in kwaliteit NetBenefits is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in kwaliteit NetBenefits tot stand worden gebracht.

In de betrouwbaarheid NetBenefits, **gebruiker** toewijzing moet worden uitgevoerd met **Azure AD-gebruiker** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met kwaliteit NetBenefits, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
3. **[Maak een testgebruiker kwaliteit NetBenefits](#create-a-fidelity-netbenefits-test-user)**  : als u wilt een equivalent van Britta Simon in kwaliteit NetBenefits die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing NetBenefits betrouwbaarheid.

**Voor het configureren van Azure AD eenmalige aanmelding met kwaliteit NetBenefits, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **kwaliteit NetBenefits** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![In het dialoogvenster voor eenmalige aanmelding](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_samlbase.png)

3. Op de **kwaliteit NetBenefits domein en URL's** sectie, voert u de volgende stappen uit:

    ![Kwaliteit NetBenefits domein en URL's eenmalige aanmelding informatie](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_url.png)

    a. In de **id** tekstvak, een URL typen:

    Voor de omgeving testen:  `urn:sp:fidelity:geninbndnbparts20:uat:xq1`

    Voor productie-omgeving:  `urn:sp:fidelity:geninbndnbparts20`

    b. In de **antwoord-URL** tekstvak, een URL opgeven die worden geleverd door betrouwbaarheid op moment van implementatie of neem contact op met uw toegewezen kwaliteit Client-Service Manager.

4. Kwaliteit NetBenefits toepassing verwacht het SAML-asserties ondertekend in een specifieke indeling. We zijn toegewezen de **gebruikers-id** met de **user.userprincipalname**. U kunt dit met toewijzen **employeeid** of enige andere vordering van toepassing op uw organisatie als is **gebruikers-id**. De volgende schermafbeelding ziet u slechts een voorbeeld voor deze.

    ![Kwaliteit NetBenefits kenmerk](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_attribute.png)

    >[!Note]
    >Kwaliteit NetBenefits ondersteuning voor statische en dynamische Federatie. Statische betekent dat SAML op basis van just-in-tijd gebruiker inrichting en dynamische betekent dat die deze biedt ondersteuning voor just-in-time gebruikersinrichting zullen niet worden gebruikt. Voor het gebruik van JIT hebben op basis van inrichting klanten enkele meer claims toevoegen in Azure AD, zoals Geboortedatum van gebruiker enzovoort. Deze gegevens worden geleverd door de uw toegewezen **kwaliteit Client-Service Manager** en ze moeten deze dynamische Federatie voor uw exemplaar inschakelen.

5. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_certificate.png)

6. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/fidelitynetbenefits-tutorial/tutorial_general_400.png)

7. Op de **kwaliteit NetBenefits configuratie** sectie, klikt u op **configureren kwaliteit NetBenefits** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Kwaliteit NetBenefits configuratie](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_configure.png)

8. Het configureren van eenmalige aanmelding op **kwaliteit NetBenefits** zijde, moet u voor het verzenden van de gedownloade **Metadata XML**, **Single Sign-On Service URL voor SAML** en  **SAML-entiteit-ID** naar **uw toegewezen kwaliteit Client-Service Manager**. Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/fidelitynetbenefits-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/fidelitynetbenefits-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/fidelitynetbenefits-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/fidelitynetbenefits-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
  
### <a name="create-a-fidelity-netbenefits-test-user"></a>Maak een testgebruiker kwaliteit NetBenefits

In deze sectie maakt u een gebruiker met de naam van Britta Simon in kwaliteit NetBenefits. Als u statische federation maakt, neem contact op met uw toegewezen **kwaliteit Client-Service Manager** om gebruikers te maken van betrouwbare en NetBenefits-platform. Deze gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken.

Gebruikers worden gemaakt met behulp van Just In Time gebruikers inrichten voor dynamische Federatie. Voor het gebruik van JIT hebben op basis van inrichting klanten enkele meer claims toevoegen in Azure AD, zoals Geboortedatum van gebruiker enzovoort. Deze gegevens worden geleverd door de uw toegewezen **kwaliteit Client-Service Manager** en ze moeten deze dynamische Federatie voor uw exemplaar inschakelen.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan kwaliteit NetBenefits.

![De de gebruikersrol toewijzen][200]

**Britta Simon om aan te wijzen kwaliteit NetBenefits, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **kwaliteit NetBenefits**.

    ![De betrouwbaarheid NetBenefits-koppeling in de lijst met toepassingen](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel kwaliteit NetBenefits in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing kwaliteit NetBenefits.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/fidelitynetbenefits-tutorial/tutorial_general_01.png
[2]: ./media/fidelitynetbenefits-tutorial/tutorial_general_02.png
[3]: ./media/fidelitynetbenefits-tutorial/tutorial_general_03.png
[4]: ./media/fidelitynetbenefits-tutorial/tutorial_general_04.png

[100]: ./media/fidelitynetbenefits-tutorial/tutorial_general_100.png

[200]: ./media/fidelitynetbenefits-tutorial/tutorial_general_200.png
[201]: ./media/fidelitynetbenefits-tutorial/tutorial_general_201.png
[202]: ./media/fidelitynetbenefits-tutorial/tutorial_general_202.png
[203]: ./media/fidelitynetbenefits-tutorial/tutorial_general_203.png
