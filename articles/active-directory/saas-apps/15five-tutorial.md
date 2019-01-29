---
title: 'Zelfstudie: Azure Active Directory-integratie met 15Five | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en 15Five.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 2fb301c2-7d7a-4046-8ee1-7dc9e7684806
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: ef156bcf1f2d92c17d470dece63d0cdf2442cba7
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55190833"
---
# <a name="tutorial-azure-active-directory-integration-with-15five"></a>Zelfstudie: Azure Active Directory-integratie met 15Five

In deze zelfstudie leert u hoe u 15Five integreren met Azure Active Directory (Azure AD).

15Five integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot 15Five heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij 15Five inschakelen (Single Sign-On) met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met 15Five, moet u de volgende items:

- Een Azure AD-abonnement
- Een 15Five eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. 15Five uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-15five-from-the-gallery"></a>15Five uit de galerie toe te voegen
Voor het configureren van de integratie van 15Five in Azure AD, moet u 15Five uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen 15Five uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

4. Typ in het zoekvak **15Five**.

    ![Het maken van een Azure AD-testgebruiker](./media/15five-tutorial/tutorial_15five_search.png)

5. Selecteer in het deelvenster resultaten **15Five**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/15five-tutorial/tutorial_15five_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met 15Five op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in 15Five is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in 15Five tot stand worden gebracht.

In 15Five, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met 15Five, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker 15Five](#creating-a-15five-test-user)**  : als u wilt een equivalent van Britta Simon in 15Five die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing 15Five.

**Voor het configureren van Azure AD eenmalige aanmelding met 15Five, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **15Five** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/15five-tutorial/tutorial_15five_samlbase.png)

3. Op de **15Five domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/15five-tutorial/tutorial_15five_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<companyname>.15five.com`

    b. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<companyname>.15five.com/saml2/metadata/`

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de daadwerkelijke aanmeldings-URL en id. Neem contact op met [15Five Client ondersteuningsteam](https://www.15five.com/contact/) om deze waarden te verkrijgen. 
 
4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/15five-tutorial/tutorial_15five_certificate.png) 

5. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/15five-tutorial/tutorial_general_400.png)

6. Het configureren van eenmalige aanmelding op **15Five** zijde, moet u voor het verzenden van de gedownloade **Metadata XML** naar [15Five ondersteuningsteam](https://www.15five.com/contact/).

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/15five-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/15five-tutorial/create_aaduser_02.png) 

3. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/15five-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/15five-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-15five-test-user"></a>Het maken van een testgebruiker 15Five

Als u wilt dat Azure AD-gebruikers zich aanmelden bij 15Five, moeten ze worden ingericht voor 15Five. Wanneer 15Five, inrichting is een handmatige taak.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voer de volgende stappen uit om de inrichting van gebruikers te configureren:
1. Meld u aan bij uw **15Five** bedrijf site als administrator.

2. Ga naar **bedrijf beheren**.
   
    ![Bedrijf beheren](./media/15five-tutorial/IC784675.png "bedrijf beheren")

3. Ga naar **mensen \> personen toevoegen**.
   
    ![People](./media/15five-tutorial/IC784676.png "People")

4. In de sectie nieuwe persoon toevoegen, moet u de volgende stappen uitvoeren:
   
    ![Toevoegen van nieuwe persoon](./media/15five-tutorial/IC784677.png "nieuwe persoon toevoegen")
   
    a. Type de **voornaam**, **achternaam**, **titel**, **e-mailadres** van een geldige Azure Active Directory-account dat u inrichten wilt in de gerelateerde tekstvakken.

    b. Klik op **Gereed**.
   
    > [!NOTE]
    > De houder van Azure AD-account ontvangt een e-mailbericht ook een koppeling voor het account te bevestigen voordat deze geactiveerd wordt.
   
### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot 15Five.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan 15Five toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **15Five**.

    ![Eenmalige aanmelding configureren](./media/15five-tutorial/tutorial_15five_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel 15Five in het toegangsvenster, krijgt u de aanmeldingspagina van 15Five toepassing.
Zie [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster. 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/15five-tutorial/tutorial_general_01.png
[2]: ./media/15five-tutorial/tutorial_general_02.png
[3]: ./media/15five-tutorial/tutorial_general_03.png
[4]: ./media/15five-tutorial/tutorial_general_04.png

[100]: ./media/15five-tutorial/tutorial_general_100.png

[200]: ./media/15five-tutorial/tutorial_general_200.png
[201]: ./media/15five-tutorial/tutorial_general_201.png
[202]: ./media/15five-tutorial/tutorial_general_202.png
[203]: ./media/15five-tutorial/tutorial_general_203.png

