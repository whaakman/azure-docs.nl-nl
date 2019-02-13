---
title: 'Zelfstudie: Azure Active Directory-integratie met Shmoop voor scholen | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Shmoop voor scholen.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 1d75560a-55b3-42e9-bda1-92b01c572d8e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/23/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 095aff293b23f4589dce9f71890b5e9bfd8636d4
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56201853"
---
# <a name="tutorial-azure-active-directory-integration-with-shmoop-for-schools"></a>Zelfstudie: Azure Active Directory-integratie met Shmoop voor scholen

In deze zelfstudie leert u hoe u Shmoop voor scholen integreren met Azure Active Directory (Azure AD).

Shmoop voor scholen integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Shmoop voor scholen heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Shmoop voor scholen met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie, de Azure-portal beheren.

Zie voor meer informatie over de integratie met Azure AD SaaS [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Shmoop voor scholen, moet u de volgende items:

- Een Azure AD-abonnement
- Een Shmoop voor scholen eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> U kunt beter geen een productie-omgeving voor het testen van de stappen in deze zelfstudie.

Als u wilt testen van de stappen in deze zelfstudie, raden we het volgende aan:

- Met behulp van de productie-evironment alleen als het nodig is.
- Ophalen van een [gratis proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/) als u nog een proefversie Azure AD-omgeving hebt.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario dat wordt beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Shmoop voor scholen uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="add-shmoop-for-schools-from-the-gallery"></a>Shmoop voor scholen uit de galerie toevoegen
Voor het configureren van de integratie van Shmoop voor scholen in Azure AD, moet u Shmoop voor scholen uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen voor scholen Shmoop uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster de **Azure Active Directory** pictogram. 

    ![De knop Azure Active Directory][1]

2. Ga naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Bedrijfstoepassingen][2]
    
3. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop boven aan in het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

4. Typ in het zoekvak **Shmoop voor scholen**. Selecteer **Shmoop voor scholen** uit de resultaten, selecteer de **toevoegen** om toe te voegen van de toepassing.

    ![Shmoop voor scholen in de lijst met resultaten](./media/shmoopforschools-tutorial/tutorial_shmoopforschools_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Shmoop voor scholen op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD te weten wie de gebruiker equivalent in Shmoop voor scholen is aan een gebruiker in Azure AD. Met andere woorden, moet u een koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Shmoop voor scholen.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Shmoop voor scholen, voert u de volgende bouwstenen:

1. [Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on) zodat uw gebruikers deze functie wilt gebruiken.
2. [Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user) voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. [Maak een testgebruiker Shmoop voor scholen](#create-a-shmoop-for-schools-test-user) hebben een equivalent van Britta Simon in Shmoop voor scholen die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. [Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user) Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. [Eenmalige aanmelding testen](#test-single-sign-on) om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u Azure AD eenmalige aanmelding in de Azure-portal inschakelen en configureren van eenmalige aanmelding in uw toepassing Shmoop voor scholen.

**Voor het configureren van Azure AD eenmalige aanmelding met Shmoop voor scholen, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Shmoop voor scholen** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

2. In de **eenmalige aanmelding** in het dialoogvenster, in de vervolgkeuzelijst onder **modus voor één aanmelding**, selecteer **SAML gebaseerde aanmelding**.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/shmoopforschools-tutorial/tutorial_shmoopforschools_samlbase.png)

3. In de **Shmoop voor scholen domein en URL's** sectie, de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/shmoopforschools-tutorial/tutorial_shmoopforschools_url.png)

    a. In de **aanmeldings-URL** vak, typ een URL met het volgende patroon: `https://schools.shmoop.com/public-api/saml2/start/<uniqueid>`

    b. In de **id** vak, typ een URL met het volgende patroon: `https://schools.shmoop.com/<uniqueid>`

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden met de werkelijke aanmeldings-URL en -id. Neem contact op met de [Shmoop voor scholen Client ondersteuningsteam](mailto:support@shmoop.com) om deze waarden te verkrijgen. 
 
4. De toepassing Shmoop voor scholen wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de **gebruikerskenmerken** sectie op de pagina van de integratie van toepassingen. De volgende schermafbeelding ziet u hoe de asserties configureren:

    ![Eenmalige aanmelding configureren](./media/shmoopforschools-tutorial/tutorial_attribute.png)

    > [!NOTE]
    > Shmoop voor School ondersteunt twee rollen voor gebruikers: **Docent** en **Student**. Instellen van deze rollen in Azure AD zodat gebruikers de juiste rollen kunnen worden toegewezen. Zie voor meer informatie over het configureren van rollen in Azure AD, [toegang met RBAC en de Azure-portal beheren](../../role-based-access-control/role-assignments-portal.md).
    
5. In de **gebruikerskenmerken** sectie de **eenmalige aanmelding** dialoogvenster vak, het kenmerk van SAML-token configureren zoals wordt weergegeven in de vorige afbeelding.  Voer dan de volgende stappen uit:

    | De naam van kenmerk | De waarde van kenmerk |
    | -------------- | --------------- |
    | role           | user.assignedroles |

    a. Om te openen de **kenmerk toevoegen** in het dialoogvenster, selecteer **kenmerk toevoegen**.
    
    ![Eenmalige aanmelding configureren ](./media/shmoopforschools-tutorial/tutorial_attribute_04.png)
    
    ![Eenmalige aanmelding configureren](./media/shmoopforschools-tutorial/tutorial_attribute_05.png)
    
    b. In de **naam** typt u de naam van het kenmerk dat wordt weergegeven voor die rij.
    
    c. Uit de **waarde** , selecteert u de waarde van het kenmerk dat wordt weergegeven voor die rij.

    d. Laat de **Namespace** vak leeg zijn.
    
    e. Selecteer **OK**.

6. Selecteer de knop **Opslaan**.

    ![Eenmalige aanmelding configureren](./media/shmoopforschools-tutorial/tutorial_general_400.png)

7. Op de **SAML-handtekeningcertificaat** sectie, klikt u op de knop kopiëren om te kopiëren **App-Url voor federatieve metagegevens** en plak deze in Kladblok.

    ![De link om het certificaat te downloaden](./media/shmoopforschools-tutorial/tutorial_shmoopforschools_certificate.png)

8. Het configureren van eenmalige aanmelding op de **Shmoop voor scholen** zijde, moet u voor het verzenden van de **App-Url voor federatieve metagegevens** naar de [Shmoop voor scholen ondersteuningsteam](mailto:support@shmoop.com).

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker Britta Simon aangeroepen in de Azure-portal.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Selecteer in de Azure portal, in het linkerdeelvenster de **Azure Active Directory** knop.

    ![De knop Azure Active Directory](./media/shmoopforschools-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**. Selecteer vervolgens **alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](./media/shmoopforschools-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, selecteer **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/shmoopforschools-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster Gebruiker](./media/shmoopforschools-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Selecteer **Maken**.
 
### <a name="create-a-shmoop-for-schools-test-user"></a>Maak een testgebruiker Shmoop voor scholen

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Shmoop voor scholen. Shmoop voor scholen biedt ondersteuning voor just-in-time inrichting, dat standaard is ingeschakeld. Er is geen actie-item voor u in deze sectie. Als een nieuwe gebruiker nog niet bestaat, wordt dit gemaakt tijdens de poging tot toegang tot Shmoop voor scholen.

>[!NOTE]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met de [Shmoop voor scholen ondersteuningsteam](mailto:support@shmoop.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Shmoop voor scholen.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon aan Shmoop voor scholen, moet u de volgende stappen uitvoeren:**

1. Open de toepassingen in de Azure-portal. Ga vervolgens naar **bedrijfstoepassingen** in de directoryweergave.  Selecteer vervolgens **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Shmoop voor scholen**.

    ![De koppeling Shmoop voor scholen in de lijst met toepassingen](./media/shmoopforschools-tutorial/tutorial_shmoopforschools_app.png)  

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen][202]

4. Selecteer de knop **Add**. Klik in de **toevoegen toewijzing** in het dialoogvenster, selecteer **gebruikers en groepen**.

    ![Het deelvenster Toewijzing toevoegen][203]

5. In de **gebruikers en groepen** in het dialoogvenster, selecteer **Britta Simon** in de gebruikerslijst.

6. In de **gebruikers en groepen** in het dialoogvenster, klikt u op de **Selecteer** knop. 

7. In de **toevoegen toewijzing** in het dialoogvenster, selecteer de **toewijzen** knop.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw configuratie Azure AD eenmalige aanmelding testen met behulp van het toegangsvenster.

Wanneer u selecteert de **Shmoop voor scholen** tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Shmoop voor scholen.

Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies voor informatie over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/shmoopforschools-tutorial/tutorial_general_01.png
[2]: ./media/shmoopforschools-tutorial/tutorial_general_02.png
[3]: ./media/shmoopforschools-tutorial/tutorial_general_03.png
[4]: ./media/shmoopforschools-tutorial/tutorial_general_04.png

[100]: ./media/shmoopforschools-tutorial/tutorial_general_100.png

[200]: ./media/shmoopforschools-tutorial/tutorial_general_200.png
[201]: ./media/shmoopforschools-tutorial/tutorial_general_201.png
[202]: ./media/shmoopforschools-tutorial/tutorial_general_202.png
[203]: ./media/shmoopforschools-tutorial/tutorial_general_203.png

