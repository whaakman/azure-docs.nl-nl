---
title: 'Zelfstudie: Azure Active Directory-integratie met Screencast-O-automatische | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Screencast-O-automatische.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 525ad47d-5488-40e2-aeaf-ae6183745682
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 51daf3b9553babb5fc627c02d79914c68c318145
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56190820"
---
# <a name="tutorial-azure-active-directory-integration-with-screencast-o-matic"></a>Zelfstudie: Azure Active Directory-integratie met Screencast-O-automatische

In deze zelfstudie leert u hoe u Screencast O automatische integreren met Azure Active Directory (Azure AD).

Screencast-O-automatische integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Screencast O automatische heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Screencast-O-automatische (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Screencast-O-automatische, moet u de volgende items:

- Een Azure AD-abonnement
- Een Screencast-O-automatische eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Screencast-O-automatische toevoegen vanuit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-screencast-o-matic-from-the-gallery"></a>Screencast-O-automatische toevoegen vanuit de galerie
Voor het configureren van de integratie van Screencast-O-automatische in Azure AD, moet u Screencast O automatische toevoegen uit de galerie aan de lijst met beheerde SaaS-apps.

**Als u wilt Screencast O automatische uit de galerie toevoegen, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

4. Typ in het zoekvak **Screencast-O-automatische**, selecteer **Screencast-O-automatische** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Screencast-O-automatische in de lijst met resultaten](./media/screencast-tutorial/tutorial_screencast_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Screencast-O-automatische op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Screencast-O-automatische is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Screencast-O-automatische tot stand worden gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Screencast-O-automatische, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
3. **[Maak een testgebruiker Screencast-O-automatische](#create-a-screencast-o-matic-test-user)**  : als u wilt een equivalent van Britta Simon in Screencast-O-automatische die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw Screencast O automatische toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met Screencast-O-automatische, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Screencast-O-automatische** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/screencast-tutorial/tutorial_screencast_samlbase.png)

3. Op de **Screencast O automatische domein en URL's** sectie, voert u de volgende stappen uit:

    ![Screencast-O-automatische domein en URL's, eenmalige aanmelding informatie](./media/screencast-tutorial/tutorial_screencast_url.png)

    Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://screencast-o-matic.com/<InstanceName>`

    > [!NOTE] 
    > De waarde voor de aanmeldings-URL is niet echt. Werk de waarde bij met de werkelijke aanmeldings-URL. Neem contact op met [Screencast O automatische Client-ondersteuningsteam](mailto:support@screencast-o-matic.com) om de waarde. 
 
4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/screencast-tutorial/tutorial_screencast_certificate.png) 

5. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/screencast-tutorial/tutorial_general_400.png)

6. In een ander browservenster, meld u aan bij Screencast-O-automatische als beheerder.

7. Klik op **abonnement**.

    ![Het abonnement](./media/screencast-tutorial/tutorial_screencast_sub.png)

8. Onder **toegangspagina** sectie, klikt u op **Setup**.

    ![De toegang](./media/screencast-tutorial/tutorial_screencast_setup.png)

9. Op de **toegang installatiepagina**, voer de volgende stappen uit:

    * Onder **toegangs-URL** sectie, typt u uw instancename in het tekstvak opgegeven.

    ![De toegang](./media/screencast-tutorial/tutorial_screencast_access.png)

    * Selecteer **domeingebruiker vereisen** onder **gebruikersbeperking SAML (optioneel)** sectie.

    * Onder **IDP metagegevens XML-bestand uploaden**, klikt u op **bestand kiezen** voor het uploaden van de metagegevens die u hebt gedownload vanuit Azure portal.

    * Klik op **OK**. 

    ![De toegang](./media/screencast-tutorial/tutorial_screencast_save.png)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/screencast-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/screencast-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/screencast-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/screencast-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-screencast-o-matic-test-user"></a>Maak een testgebruiker Screencast O automatische

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Screencast-O-automatische. Screencast-O-automatische biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot Screencast O automatische als deze nog niet bestaat.

>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [Screencast O automatische Client-ondersteuningsteam](mailto:support@screencast-o-matic.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Screencast-O-automatische.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan Screencast-O-automatische toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Screencast-O-automatische**.

    ![De Screencast-O-automatische koppeling in de lijst met toepassingen](./media/screencast-tutorial/tutorial_screencast_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de Screencast-O-automatische tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw Screencast O automatische toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/screencast-tutorial/tutorial_general_01.png
[2]: ./media/screencast-tutorial/tutorial_general_02.png
[3]: ./media/screencast-tutorial/tutorial_general_03.png
[4]: ./media/screencast-tutorial/tutorial_general_04.png

[100]: ./media/screencast-tutorial/tutorial_general_100.png

[200]: ./media/screencast-tutorial/tutorial_general_200.png
[201]: ./media/screencast-tutorial/tutorial_general_201.png
[202]: ./media/screencast-tutorial/tutorial_general_202.png
[203]: ./media/screencast-tutorial/tutorial_general_203.png

