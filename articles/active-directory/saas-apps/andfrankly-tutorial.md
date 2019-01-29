---
title: 'Zelfstudie: Azure Active Directory-integratie met & eerlijk gezegd | Microsoft Docs'
description: Meer informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en & eerlijk gezegd.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 1d702060-1b89-4e9d-9f01-ede4f1171c73
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 8b95459e00296950924c7837890e2313bbcb183d
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55173102"
---
# <a name="tutorial-azure-active-directory-integration-with-frankly"></a>Zelfstudie: Azure Active Directory-integratie met & eerlijk gezegd

In deze zelfstudie leert u hoe u integreert & eerlijk gezegd met Azure Active Directory (Azure AD).

Integratie van & eerlijk gezegd met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot & eerlijk gezegd heeft
- U kunt uw gebruikers automatisch aangemeld op & eerlijk gezegd inschakelen (Single Sign-On) met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Integratie met & eerlijk gezegd, u hebt de volgende items nodig voor het configureren van Azure AD:

- Een Azure AD-abonnement
- A & eerlijk gezegd eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toe te voegen & eerlijk gezegd uit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-frankly-from-the-gallery"></a>Toe te voegen & eerlijk gezegd uit de galerie
Voor het configureren van de integratie van & eerlijk gezegd in Azure AD, moet u toevoegen & eerlijk gezegd uit de galerie aan de lijst met beheerde SaaS-apps.

**Op toevoegen & eerlijk gezegd uit de galerie, de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

4. Typ in het zoekvak **& eerlijk gezegd**.

    ![Het maken van een Azure AD-testgebruiker](./media/andfrankly-tutorial/tutorial_andfrankly_search.png)

5. Selecteer in het deelvenster resultaten **& eerlijk gezegd**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/andfrankly-tutorial/tutorial_andfrankly_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureren en testen van Azure AD eenmalige aanmelding met & eerlijk gezegd op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, Azure AD moet weten wat de equivalente-gebruiker in & eerlijk gezegd is aan een gebruiker in Azure AD. Met andere woorden, een koppeling de relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in & eerlijk gezegd moet tot stand worden gebracht.

In & eerlijk gezegd, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en testen van Azure AD moet eenmalige aanmelding met & eerlijk gezegd, u voltooien van de volgende bouwstenen:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een & eerlijk gezegd testgebruiker](#creating-a-frankly-test-user)**  : als u wilt een equivalent van Britta Simon hebben in & eerlijk gezegd dat is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u Azure AD eenmalige aanmelding in de Azure-portal inschakelen en configureren van eenmalige aanmelding in uw & eerlijk gezegd toepassing.

**Het configureren van Azure AD eenmalige aanmelding met & eerlijk gezegd, voer de volgende stappen uit:**

1. In de Azure-portal op de **& eerlijk gezegd** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/andfrankly-tutorial/tutorial_andfrankly_samlbase.png)

3. Op de **& eerlijk gezegd domein en URL's** sectie, als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Eenmalige aanmelding configureren](./media/andfrankly-tutorial/tutorial_andfrankly_url.png)

    a. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://andfrankly.com/saml/simplesaml/www/module.php/saml/sp/metadata.php/<tenant id>`

    b. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://andfrankly.com/saml/simplesaml/www/module.php/saml/sp/saml2-acs.php/<tenant id>`

4. Controleer **geavanceerde URL-instellingen weergeven**. Als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Eenmalige aanmelding configureren](./media/andfrankly-tutorial/tutorial_andfrankly_url1.png)

    Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://andfrankly.com/saml/okta/?saml_sso=<tenant id>`
    > [!NOTE] 
    > Dit zijn geen echte waarden. Deze waarden bijwerken met de werkelijke-id, aanmelding en antwoord-URL. Neem contact op met [andfrankly ondersteuningsteam](mailto:help@andfrankly.com) om deze waarden te verkrijgen.

5. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/andfrankly-tutorial/tutorial_andfrankly_certificate.png) 

6. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/andfrankly-tutorial/tutorial_general_400.png)

7. Het configureren van eenmalige aanmelding op **& eerlijk gezegd** zijde, moet u voor het verzenden van de gedownloade **Metadata XML** naar [andfrankly ondersteuningsteam](mailto:help@andfrankly.com). 

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/andfrankly-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/andfrankly-tutorial/create_aaduser_02.png) 

3. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/andfrankly-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/andfrankly-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-frankly-test-user"></a>Het maken van een & eerlijk gezegd testgebruiker

In deze sectie maakt u een gebruiker met de naam van Britta Simon in & eerlijk gezegd. Werken met [andfrankly ondersteuningsteam](mailto:help@andfrankly.com) om toe te voegen de gebruikers in de & eerlijk gezegd platform.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen in & eerlijk gezegd.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon in & eerlijk gezegd, kunt u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **& eerlijk gezegd**.

    ![Eenmalige aanmelding configureren](./media/andfrankly-tutorial/tutorial_andfrankly_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van de configuratie van uw Azure AD-eenmalige aanmelding via het toegangsvenster.

Wanneer u klikt op de & eerlijk gezegd tegel in het toegangsvenster, krijgt u automatisch aangemeld bij uw & eerlijk gezegd toepassing

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/andfrankly-tutorial/tutorial_general_01.png
[2]: ./media/andfrankly-tutorial/tutorial_general_02.png
[3]: ./media/andfrankly-tutorial/tutorial_general_03.png
[4]: ./media/andfrankly-tutorial/tutorial_general_04.png

[100]: ./media/andfrankly-tutorial/tutorial_general_100.png

[200]: ./media/andfrankly-tutorial/tutorial_general_200.png
[201]: ./media/andfrankly-tutorial/tutorial_general_201.png
[202]: ./media/andfrankly-tutorial/tutorial_general_202.png
[203]: ./media/andfrankly-tutorial/tutorial_general_203.png

