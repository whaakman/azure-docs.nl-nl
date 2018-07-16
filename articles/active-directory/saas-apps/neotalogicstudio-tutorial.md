---
title: 'Zelfstudie: Azure Active Directory-integratie met Neota logische Studio | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Neota logische Studio.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 842605e6-a91d-42cc-a0bb-e23e67173ae2
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: jeedes
ms.openlocfilehash: 5861d709e238e5da07cd6a44fefdcc456422ad9d
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39046240"
---
# <a name="tutorial-azure-active-directory-integration-with-neota-logic-studio"></a>Zelfstudie: Azure Active Directory-integratie met Neota logische Studio

In deze zelfstudie leert u hoe u Neota logische Studio integreren met Azure Active Directory (Azure AD).

Neota logische Studio integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Neota logische Studio heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Neota logische Studio (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, raadpleegt u. [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Neota logische Studio, hebt u de volgende items nodig:

- Een Azure AD-abonnement
- Een Neota logische Studio eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Neota logische Studio uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-neota-logic-studio-from-the-gallery"></a>Neota logische Studio uit de galerie toe te voegen

Voor het configureren van de integratie van Neota logische Studio in Azure AD, moet u Neota logische Studio uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Neota logische Studio uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Neota logische Studio**.

    ![Het maken van een Azure AD-testgebruiker](./media/neotalogicstudio-tutorial/tutorial_neotalogicstudio_search.png)

5. Selecteer in het deelvenster resultaten **Neota logische Studio**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/neotalogicstudio-tutorial/tutorial_neotalogicstudio_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Neota logische Studio op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Neota logische Studio is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Neota logische Studio tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door toe te wijzen de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Neota logische Studio.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Neota logische Studio, u nodig hebt voor de volgende bouwstenen:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker Neota logische Studio](#creating-a-neota-logic-studio-test-user)**  : als u wilt een equivalent van Britta Simon in Neota logische Studio die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Neota logische Studio.

**Voor het configureren van Azure AD eenmalige aanmelding met Neota logische Studio, kunt u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Neota logische Studio** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/neotalogicstudio-tutorial/tutorial_neotalogicstudio_samlbase.png)

3. Op de **Neota logische Studio domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/neotalogicstudio-tutorial/tutorial_neotalogicstudio_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<sub domain>.neotalogic.com/a/<sub application>`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<sub domain>.neotalogic.com/wb`

    > [!NOTE] 
    > Deze waarden zijn niet de werkelijke. Deze waarden bijwerken met de werkelijke aanmeldings-URL & id. Hier stellen we voor u voor het gebruik van de unieke waarde van de tekenreeks in de id. Neem contact op met [Neota logische Studia ondersteuningsteam](https://www.neotalogic.com/contact-us/) om deze waarden te verkrijgen. 
 
4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/neotalogicstudio-tutorial/tutorial_neotalogicstudio_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/neotalogicstudio-tutorial/tutorial_general_400.png)

6. Als u eenmalige aanmelding configureren voor uw toepassing, neem contact op met [Neota logische Studio ondersteuning](https://www.neotalogic.com/contact-us/) -team en bied hen nauwkeurige gedownload met **Metadata XML** bestand.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/neotalogicstudio-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/neotalogicstudio-tutorial/create_aaduser_02.png) 

3. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/neotalogicstudio-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/neotalogicstudio-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-neota-logic-studio-test-user"></a>Het maken van een testgebruiker Neota logische Studio

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Neota logische Studio. werken met [Neota logische Studia ondersteuningsteam](https://www.neotalogic.com/contact-us/) om toe te voegen de gebruikers in het platform Neota logische Studio. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken. 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Neota logische Studio.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon naar Neota logische Studio, kunt u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Neota logische Studio**.

    ![Eenmalige aanmelding configureren](./media/neotalogicstudio-tutorial/tutorial_neotalogicstudio_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Klik op de tegel Neota logische Studio in het toegangsvenster, wordt u omgeleid naar organisatie aanmeldings-pagina. Na geslaagde aanmelding, u zal worden aangemeld bij uw toepassing Neota logische Studio. Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/neotalogicstudio-tutorial/tutorial_general_01.png
[2]: ./media/neotalogicstudio-tutorial/tutorial_general_02.png
[3]: ./media/neotalogicstudio-tutorial/tutorial_general_03.png
[4]: ./media/neotalogicstudio-tutorial/tutorial_general_04.png

[100]: ./media/neotalogicstudio-tutorial/tutorial_general_100.png

[200]: ./media/neotalogicstudio-tutorial/tutorial_general_200.png
[201]: ./media/neotalogicstudio-tutorial/tutorial_general_201.png
[202]: ./media/neotalogicstudio-tutorial/tutorial_general_202.png
[203]: ./media/neotalogicstudio-tutorial/tutorial_general_203.png

