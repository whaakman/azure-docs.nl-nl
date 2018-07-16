---
title: 'Zelfstudie: Azure Active Directory-integratie met Aha! | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Aha!.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: ad955d3d-896a-41bb-800d-68e8cb5ff48d
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: 30f0f316727cfcf20daa58c35d0ba11c25311898
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39044115"
---
# <a name="tutorial-azure-active-directory-integration-with-aha"></a>Zelfstudie: Azure Active Directory-integratie met Aha!

In deze zelfstudie leert u hoe u integreren Aha! met Azure Active Directory (Azure AD).

Integratie van Aha! met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Aha heeft!
- U kunt uw gebruikers automatisch ophalen aangemeld bij Aha inschakelen. (Single Sign-On) met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met Aha!, moet u de volgende items:

- Een Azure AD-abonnement
- Een Aha! eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toe te voegen Aha! in de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-aha-from-the-gallery"></a>Toe te voegen Aha! in de galerie
Het configureren van de integratie van Aha! in Azure AD moet u Aha toevoegen. in de galerie aan de lijst met beheerde SaaS-apps.

**Om toe te voegen Aha! in de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Aha!**.

    ![Het maken van een Azure AD-testgebruiker](./media/aha-tutorial/tutorial_aha_search.png)

5. Selecteer in het deelvenster resultaten **Aha!**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/aha-tutorial/tutorial_aha_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Aha! op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD weten wat de equivalente-gebruiker in Aha! is aan een gebruiker in Azure AD. Met andere woorden, een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Aha! moet de tot stand worden gebracht.

In Aha!, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Aha!, moet u de volgende bouwstenen uitvoeren:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een Aha! testgebruiker](#creating-an-aha-test-user)**  : als u wilt een equivalent van Britta Simon in Aha hebben! dat is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u Azure AD eenmalige aanmelding in de Azure-portal inschakelen en configureren van eenmalige aanmelding in uw Aha. de toepassing.

**Het configureren van Azure AD eenmalige aanmelding met Aha!, voer de volgende stappen uit:**

1. In de Azure-portal op de **Aha!** pagina van de integratie van toepassing, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/aha-tutorial/tutorial_aha_samlbase.png)

3. Op de **Aha! Domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/aha-tutorial/tutorial_aha_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<companyname>.aha.io/session/new`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<companyname>.aha.io`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL en -id. Neem contact op met [Aha! Client-ondersteuningsteam](https://www.aha.io/company/contact) om deze waarden te verkrijgen. 
 
4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/aha-tutorial/tutorial_aha_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/aha-tutorial/tutorial_general_400.png)

6. In een ander browservenster aanmelden bij uw Aha! bedrijf site als beheerder.

7. Klik in het menu aan de bovenkant op **instellingen**.

    ![Instellingen voor](./media/aha-tutorial/IC798950.png "instellingen")

8. Klik op **Account**.
   
    ![Profiel](./media/aha-tutorial/IC798951.png "profiel")

9. Klik op **beveiligings- en eenmalige aanmelding**.
   
    ![Beveiliging en eenmalige aanmelding](./media/aha-tutorial/IC798952.png "beveiligings- en eenmalige aanmelding")

10. In **Single Sign-On** sectie als **id-Provider**, selecteer **SAML2.0**.
   
    ![Beveiliging en eenmalige aanmelding](./media/aha-tutorial/IC798953.png "beveiligings- en eenmalige aanmelding")

11. Op de **Single Sign-On** configuratie pagina, voert u de volgende stappen uit:
    
    ![Single Sign-On](./media/aha-tutorial/IC798954.png "Single Sign-On")
    
       a. In de **naam** tekstvak, typ een naam voor uw configuratie.

       b. Voor **configureren met behulp van**, selecteer **metagegevensbestand**.
   
       c. Als u wilt uw gedownloade metagegevensbestand uploaden, klikt u op **Bladeren**.
   
       d. Klik op **Update**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/aha-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/aha-tutorial/create_aaduser_02.png) 

3. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/aha-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/aha-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-an-aha-test-user"></a>Het maken van een Aha! testgebruiker

Om in te schakelen van Azure AD-gebruikers zich aanmelden bij Aha!, ze moeten worden ingericht voor Aha!.  

In het geval van Aha!, inrichting is een geautomatiseerde taak. Er is geen actie-item voor u.

Gebruikers worden automatisch gemaakt als dat nodig tijdens de eerste eenmalige aanmelding.

>[!NOTE]
>U kunt elke andere Aha! hulpmiddelen voor het maken van account of API's die worden geleverd door Aha! voor het inrichten van gebruikersaccounts van AAD.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Aha!.

![Gebruiker toewijzen][200] 

**Britta Simon toewijzen aan Aha!, voer de volgende stappen uit:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Aha!**.

    ![Eenmalige aanmelding configureren](./media/aha-tutorial/tutorial_aha_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het toegangsvenster. Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/aha-tutorial/tutorial_general_01.png
[2]: ./media/aha-tutorial/tutorial_general_02.png
[3]: ./media/aha-tutorial/tutorial_general_03.png
[4]: ./media/aha-tutorial/tutorial_general_04.png

[100]: ./media/aha-tutorial/tutorial_general_100.png

[200]: ./media/aha-tutorial/tutorial_general_200.png
[201]: ./media/aha-tutorial/tutorial_general_201.png
[202]: ./media/aha-tutorial/tutorial_general_202.png
[203]: ./media/aha-tutorial/tutorial_general_203.png

