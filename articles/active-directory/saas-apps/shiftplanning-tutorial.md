---
title: 'Zelfstudie: Azure Active Directory-integratie met mensheid | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en de mensheid.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 6aa771e9-31c6-48d1-8dde-024bebc06943
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: b0ddb21ea26098d8c45d335ca6765cf3654ce13e
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55166591"
---
# <a name="tutorial-azure-active-directory-integration-with-humanity"></a>Zelfstudie: Azure Active Directory-integratie met mensheid

In deze zelfstudie leert u hoe u mensheid integreren met Azure Active Directory (Azure AD).

Mensheid integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de mensheid heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij de mensheid (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met de mensheid, moet u de volgende items:

- Een Azure AD-abonnement
- Een mensheid eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Mensheid uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-humanity-from-the-gallery"></a>Mensheid uit de galerie toe te voegen
Voor het configureren van de integratie van de mensheid in Azure AD, moet u de mensheid uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen de mensheid uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **mensheid**.

    ![Het maken van een Azure AD-testgebruiker](./media/shiftplanning-tutorial/tutorial_humanity_search.png)

1. Selecteer in het deelvenster resultaten **mensheid**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/shiftplanning-tutorial/tutorial_humanity_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met de mensheid op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in de mensheid is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in de mensheid tot stand worden gebracht.

In de mensheid, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met de mensheid, u nodig hebt voor de volgende bouwstenen:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker mensheid](#creating-a-humanity-test-user)**  : als u wilt een equivalent van Britta Simon in mensheid die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing mensheid.

**Voor het configureren van Azure AD eenmalige aanmelding met de mensheid, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **mensheid** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/shiftplanning-tutorial/tutorial_humanity_samlbase.png)

1. Op de **mensheid domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/shiftplanning-tutorial/tutorial_humanity_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://company.humanity.com/includes/saml/`

    b. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://company.humanity.com/app/`

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de daadwerkelijke aanmeldings-URL en id. Neem contact op met [mensheid Client ondersteuningsteam](https://www.humanity.com/support/) om deze waarden te verkrijgen. 
 
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/shiftplanning-tutorial/tutorial_humanity_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/shiftplanning-tutorial/tutorial_general_400.png)

1. Op de **mensheid configuratie** sectie, klikt u op **configureren mensheid** openen **aanmelding configureren** venster. Kopiëren de **SAML Single Sign-On Service-URL en afmelding URL** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/shiftplanning-tutorial/tutorial_humanity_configure.png) 

1. In een ander browservenster aanmelden bij uw **mensheid** bedrijf site als beheerder.

1. Klik in het menu bovenaan op **Admin**.
   
    ![Admin](./media/shiftplanning-tutorial/iC786619.png "Admin")

1. Onder **integratie**, klikt u op **Single Sign-On**.
   
    ![Single Sign-On](./media/shiftplanning-tutorial/iC786620.png "Single Sign-On")

1. In de **Single Sign-On** sectie, voert u de volgende stappen uit:
   
    ![Single Sign-On](./media/shiftplanning-tutorial/iC786905.png "Single Sign-On")
   
    a. Selecteer **SAML ingeschakeld**.

    b. Selecteer **wachtwoord aanmelden toestaan**.

    c. Plak de **Single Sign-On Service URL voor SAML** waarde in de **URL voor SAML-verlener** tekstvak.

    d. Plak de **afmelding URL** waarde in de **externe URL voor afmelden** tekstvak.
   
    e. Het base-64 gecodeerde certificaat openen in Kladblok, Kopieer de inhoud ervan in het Klembord en plakt u deze naar de **X.509-certificaat** tekstvak.

1. Klik op **instellingen opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/shiftplanning-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/shiftplanning-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/shiftplanning-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/shiftplanning-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-humanity-test-user"></a>Het maken van een testgebruiker mensheid

Als u wilt dat Azure AD-gebruikers zich aanmelden bij de mensheid, moeten ze worden ingericht voor mensheid. In het geval van de mensheid is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Meld u aan bij uw **mensheid** bedrijf site als beheerder.

1. Klik op **Admin**.
   
    ![Admin](./media/shiftplanning-tutorial/iC786619.png "Admin")

1. Klik op **personeel**.
   
    ![Personeel](./media/shiftplanning-tutorial/ic786623.png "personeel")

1. Onder **acties**, klikt u op **werknemers toevoegen**.
   
    ![Werknemers toevoegen](./media/shiftplanning-tutorial/iC786624.png "werknemers toevoegen")

1. In de **werknemers toevoegen** sectie, voert u de volgende stappen uit:
   
    ![Opslaan van werknemers](./media/shiftplanning-tutorial/iC786625.png "werknemers opslaan")
   
    a. Type de **voornaam**, **achternaam**, en **e** van een geldige AAD-account dat u inrichten in de bijbehorende tekstvakken wilt.

    b. Klik op **opslaan werknemers**.

>[!NOTE]
>U kunt elke andere mensheid gebruiker account hulpmiddelen voor het maken of API's die door de mensheid inrichten AAD-gebruikersaccounts.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan de mensheid.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon aan mensheid, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **mensheid**.

    ![Eenmalige aanmelding configureren](./media/shiftplanning-tutorial/tutorial_humanity_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel mensheid in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing mensheid.
Zie [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/shiftplanning-tutorial/tutorial_general_01.png
[2]: ./media/shiftplanning-tutorial/tutorial_general_02.png
[3]: ./media/shiftplanning-tutorial/tutorial_general_03.png
[4]: ./media/shiftplanning-tutorial/tutorial_general_04.png

[100]: ./media/shiftplanning-tutorial/tutorial_general_100.png

[200]: ./media/shiftplanning-tutorial/tutorial_general_200.png
[201]: ./media/shiftplanning-tutorial/tutorial_general_201.png
[202]: ./media/shiftplanning-tutorial/tutorial_general_202.png
[203]: ./media/shiftplanning-tutorial/tutorial_general_203.png

