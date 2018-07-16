---
title: 'Zelfstudie: Azure Active Directory-integratie met Panorama9 | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Panorama9.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 5e28d7fa-03be-49f3-96c8-b567f1257d44
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 7285ef4bea1dccb651360520bfe8bc4289d54f8c
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39046036"
---
# <a name="tutorial-azure-active-directory-integration-with-panorama9"></a>Zelfstudie: Azure Active Directory-integratie met Panorama9

In deze zelfstudie leert u hoe u Panorama9 integreren met Azure Active Directory (Azure AD).

Panorama9 integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Panorama9 heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Panorama9 inschakelen (Single Sign-On) met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Panorama9, moet u de volgende items:

- Een Azure AD-abonnement
- Een Panorama9 eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Panorama9 uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-panorama9-from-the-gallery"></a>Panorama9 uit de galerie toe te voegen
Voor het configureren van de integratie van Panorama9 in Azure AD, moet u Panorama9 uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Panorama9 uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Panorama9**.

    ![Het maken van een Azure AD-testgebruiker](./media/panorama9-tutorial/tutorial_panorama9_search.png)

5. Selecteer in het deelvenster resultaten **Panorama9**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/panorama9-tutorial/tutorial_panorama9_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Panorama9 op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Panorama9 is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Panorama9 tot stand worden gebracht.

In Panorama9, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Panorama9, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker Panorama9](#creating-a-panorama9-test-user)**  : als u wilt een equivalent van Britta Simon in Panorama9 die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Panorama9.

**Voor het configureren van Azure AD eenmalige aanmelding met Panorama9, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Panorama9** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/panorama9-tutorial/tutorial_panorama9_samlbase.png)

3. Op de **Panorama9 domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/panorama9-tutorial/tutorial_panorama9_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL als: `https://dashboard.panorama9.com/saml/access/3262`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `http://www.panorama9.com/saml20/<tenant-name>`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL en -id. Neem contact op met [Panorama9 Client ondersteuningsteam](https://support.panorama9.com) om deze waarden te verkrijgen. 
 
4. Op de **SAML-handtekeningcertificaat** sectie, Kopieer de **VINGERAFDRUK** waarde van het certificaat.

    ![Eenmalige aanmelding configureren](./media/panorama9-tutorial/tutorial_panorama9_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/panorama9-tutorial/tutorial_general_400.png)

6. Op de **Panorama9 configuratie** sectie, klikt u op **configureren Panorama9** openen **aanmelding configureren** venster. Kopiëren de **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/panorama9-tutorial/tutorial_panorama9_configure.png) 

5. Meld u in een ander browservenster in uw bedrijf Panorama9 site als beheerder.

6. Klik in de werkbalk bovenaan op **beheren**, en klik vervolgens op **extensies**.
   
   ![Extensies](./media/panorama9-tutorial/ic790023.png "extensies")
7. Op de **extensies** dialoogvenster, klikt u op **Single Sign-On**.
   
   ![Single Sign-On](./media/panorama9-tutorial/ic790024.png "Single Sign-On")
8. In de **instellingen** sectie, voert u de volgende stappen uit:
   
   ![Instellingen voor](./media/panorama9-tutorial/ic790025.png "instellingen")
   
    a. In **URL van de id-provider** tekstvak, plak de waarde van **Single Sign-On Service URL**, die u hebt gekopieerd vanuit Azure portal.
   
    b. In **certificaat vingerafdruk** tekstvak, plak de **vingerafdruk** waarde van het certificaat dat u hebt gekopieerd vanuit Azure portal.    
         
9. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/panorama9-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/panorama9-tutorial/create_aaduser_02.png) 

3. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/panorama9-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/panorama9-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-panorama9-test-user"></a>Het maken van een testgebruiker Panorama9

Als u wilt inschakelen in Azure AD-gebruikers zich aanmelden bij Panorama9, moeten ze worden ingericht voor Panorama9.  

In het geval van Panorama9 is inrichten een handmatige taak.

**Als u wilt inrichten van gebruikers configureren, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **Panorama9** bedrijf site als beheerder.

2. Klik in het menu aan de bovenkant op **beheren**, en klik vervolgens op **gebruikers**.
   
  ![Gebruikers](./media/panorama9-tutorial/ic790027.png "gebruikers")

3. Klik in de sectie gebruikers **+** nieuwe gebruiker toe te voegen.

 ![Gebruikers](./media/panorama9-tutorial/ic790028.png "gebruikers")

4. Ga naar de sectie van de gebruiker gegevens, typt u het e-mailadres van een geldige Azure Active Directory-gebruiker die u inrichten wilt in de **e** tekstvak.

5. Komen aan de sectie gebruikers, klikt u op **opslaan**.
   
> [!NOTE]
    > De houder van Azure Active Directory-account ontvangt een e-mailbericht en volgt een koppeling om te bevestigen van hun account voordat deze geactiveerd wordt.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Panorama9.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan Panorama9 toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Panorama9**.

    ![Eenmalige aanmelding configureren](./media/panorama9-tutorial/tutorial_panorama9_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Panorama9 in het toegangsvenster, u moet u automatisch aangemeld bij Panorama9 toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/panorama9-tutorial/tutorial_general_01.png
[2]: ./media/panorama9-tutorial/tutorial_general_02.png
[3]: ./media/panorama9-tutorial/tutorial_general_03.png
[4]: ./media/panorama9-tutorial/tutorial_general_04.png

[100]: ./media/panorama9-tutorial/tutorial_general_100.png

[200]: ./media/panorama9-tutorial/tutorial_general_200.png
[201]: ./media/panorama9-tutorial/tutorial_general_201.png
[202]: ./media/panorama9-tutorial/tutorial_general_202.png
[203]: ./media/panorama9-tutorial/tutorial_general_203.png

