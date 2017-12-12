---
title: 'Zelfstudie: Azure Active Directory-integratie met Lecorpio | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Lecorpio.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/02/2017
ms.author: jeedes
ms.openlocfilehash: 15eded8b676dfea018f041aed75b4f706bc1a55b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lecorpio"></a>Zelfstudie: Azure Active Directory-integratie met Lecorpio

In deze zelfstudie leert u hoe Lecorpio integreren met Azure Active Directory (Azure AD).

Lecorpio integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Lecorpio heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Lecorpio (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Lecorpio, moet u de volgende items:

- Een Azure AD-abonnement
- Een Lecorpio eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Lecorpio uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-lecorpio-from-the-gallery"></a>Lecorpio uit de galerie toevoegen
Voor het configureren van de integratie van Lecorpio in Azure AD, moet u Lecorpio uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Lecorpio uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Klik op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Lecorpio**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_search.png)

5. Selecteer in het deelvenster resultaten **Lecorpio**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met Lecorpio op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Lecorpio is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Lecorpio tot stand worden gebracht.

Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Lecorpio.

Om te configureren en testen van Azure AD eenmalige aanmelding met Lecorpio, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Lecorpio](#creating-a-lecorpio-test-user)**  - Lecorpio die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Lecorpio configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Lecorpio, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Lecorpio** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_samlbase.png)

3. Op de **Lecorpio domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_url.png)

    a. In de **aanmeldings-URL** textbox, typ de waarde met het volgende patroon volgen:`https://<instance name>.lecorpio.com/<customer name>`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://<instance name>.lecorpio.com/<customer name>`

    > [!NOTE] 
    > Deze waarden zijn niet de werkelijke. Deze waarden bijwerken met de werkelijke aanmeldings-URL en de id. Hier raden we u voor het gebruik van de unieke waarde van een tekenreeks in de id. Neem contact op met [Lecorpio Client ondersteuningsteam](mailto:info@lecorpio.com) ophalen van deze waarden. 
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lecorpio-tutorial/tutorial_general_400.png)

6. Eenmalige aanmelding configureren op **Lecorpio** zijde, moet u de gedownloade verzenden **Metadata XML** naar [Lecorpio ondersteuningsteam](mailto:info@lecorpio.com).

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-lecorpio-tutorial/create_aaduser_01.png) 

2. Ga naar **gebruikers en groepen** en klik op **alle gebruikers** om de lijst met gebruikers weer te geven.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-lecorpio-tutorial/create_aaduser_02.png) 

3. Aan de bovenkant van het dialoogvenster, klikt u op **toevoegen** openen de **gebruiker** dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-lecorpio-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-lecorpio-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-lecorpio-test-user"></a>Een testgebruiker Lecorpio maken

In deze sectie kunt u een gebruiker Britta Simon aangeroepen in Lecorpio maken. 

Neem contact op met [Lecorpio Client ondersteuningsteam](mailto:info@lecorpio.com) de gebruikers in de toepassing Lecorpio toevoegen.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Lecorpio.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Lecorpio, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Lecorpio**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Lecorpio in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Lecorpio.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_203.png

