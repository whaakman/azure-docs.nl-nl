---
title: 'Zelfstudie: Azure Active Directory-integratie met FirmPlay - werknemer Advocacy voor werving | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en FirmPlay - werknemer Advocacy voor werving.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a6799629-7546-43f8-a966-956db32864b1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2017
ms.author: jeedes
ms.openlocfilehash: 3cddd5b9508159089bf344dbb3882d462799747c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-firmplay---employee-advocacy-for-recruiting"></a>Zelfstudie: Azure Active Directory-integratie met FirmPlay - werknemer Advocacy voor werving

In deze zelfstudie leert u hoe integreren FirmPlay - werknemer Advocacy voor werving met Azure Active Directory (Azure AD).

Integratie van FirmPlay - werknemer Advocacy voor werving met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot FirmPlay - werknemer Advocacy voor werving heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij FirmPlay - werknemer Advocacy voor werving (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure Management portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met FirmPlay - werknemer Advocacy voor werving, moet u de volgende items:

- Een Azure AD-abonnement
- Een FirmPlay - werknemer Advocacy voor het werven van eenmalige aanmelding ingeschakeld abonnement


> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- U moet uw productieomgeving niet gebruiken tenzij dit noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. FirmPlay - werknemer Advocacy voor werving uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-firmplay---employee-advocacy-for-recruiting-from-the-gallery"></a>FirmPlay - werknemer Advocacy voor werving uit de galerie toevoegen
Voor het configureren van de integratie van FirmPlay - werknemer Advocacy voor werving in Azure AD, moet u FirmPlay - werknemer Advocacy voor werving uit de galerie aan de lijst met beheerde SaaS-apps toevoegen.

**Als u wilt toevoegen, FirmPlay - werknemer Advocacy voor werving uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure Management Portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Klik op **toevoegen** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **FirmPlay - werknemer Advocacy voor werving**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_001.png)

5. Selecteer in het deelvenster resultaten **FirmPlay - werknemer Advocacy voor werving**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met FirmPlay - werknemer Advocacy voor werving op basis van een testgebruiker 'Britta Simon' genoemd.

Azure AD moet weten wat de equivalente gebruiker in FirmPlay - werknemer Advocacy voor werving is aan een gebruiker in Azure AD voor eenmalige aanmelding werkt. Met andere woorden, een relatie koppeling tussen een Azure AD-gebruiker en de betreffende gebruiker in FirmPlay - werknemer Advocacy voor het werven van moet tot stand worden gebracht.

Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in FirmPlay - werknemer Advocacy voor werving.

Als u wilt configureren en testen Azure AD eenmalige aanmelding met FirmPlay - werknemer Advocacy voor werving, moet u de volgende elementen voltooid:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een FirmPlay - werknemer Advocacy voor de testgebruiker werving](#creating-a-firmplay---employee-advocacy-for-recruiting-test-user)**  - met een exemplaar van Britta Simon FirmPlay: werknemer Advocacy voor het werven van is gekoppeld aan de Azure AD-representatie van haar.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure-beheerportal en eenmalige aanmelding in uw FirmPlay - werknemer Advocacy voor werving toepassing configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met FirmPlay - werknemer Advocacy voor werving, moet u de volgende stappen uitvoeren:**

1. In de Azure-beheerportal op de **FirmPlay - werknemer Advocacy voor werving** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster als **modus** Selecteer **op basis van SAML aanmelding** eenmalige aanmelding inschakelen op.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_01.png)

3. Op de **FirmPlay - werknemer Advocacy voor het werven van domein en de URL's** sectie in het **aanmelding op URL** textbox, typ een URL met het volgende patroon volgen:`https://<your-subdomain>.firmplay.com/`

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_02.png)

    > [!NOTE] 
    > Houd er rekening mee dat dit geen de feitelijke waarde is. U moet deze waarde met de werkelijke aanmelding op de URL bijwerken. Neem contact op met [FirmPlay - werknemer Advocacy voor werving ondersteuningsteam](mailto:engineering@firmplay.com) deze waarde op te halen. 

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **nieuw certificaat maken**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_03.png)   

5. Op de **nieuw certificaat maken** dialoogvenster, klikt u op het pictogram van de kalender en selecteer een **vervaldatum**. Klik vervolgens op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-firmplay-tutorial/tutorial_general_300.png)

6. Op de **SAML-certificaat voor ondertekening van** sectie **nieuwe certificaat activeren** en klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_04.png)

7. In het pop-upvenster **rollovercertificaat** venster, klikt u op **OK**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-firmplay-tutorial/tutorial_general_400.png)

8. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (base64)** en sla het certificaatbestand op uw computer. 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_05.png) 

9. Op de **FirmPlay - werknemer Advocacy voor configuratie werven** sectie, klikt u op **configureren FirmPlay - werknemer Advocacy voor werving** openen **eenmalige aanmelding configureren** het dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_06.png) 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_07.png)

10. Als u eenmalige aanmelding die zijn geconfigureerd voor uw toepassing, neem contact op met [FirmPlay - werknemer Advocacy voor werving ondersteuningsteam](mailto:engineering@firmplay.com) en geeft u het volgende: 

    • De gedownloade **certificaatbestand**

    • De **URL voor SAML-Service voor eenmalige aanmelding**

    • De **SAML entiteit-ID**

    • De **afmelden URL**
  

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in Azure Management portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure Management portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-firmplay-tutorial/create_aaduser_01.png) 

2. Ga naar **gebruikers en groepen** en klik op **alle gebruikers** om de lijst met gebruikers weer te geven.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-firmplay-tutorial/create_aaduser_02.png) 

3. Klik aan de bovenkant van het dialoogvenster **toevoegen** openen de **gebruiker** dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-firmplay-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-firmplay-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**. 



### <a name="creating-a-firmplay---employee-advocacy-for-recruiting-test-user"></a>Maken van een FirmPlay - werknemer Advocacy voor werving testgebruiker

In deze sectie kunt u een gebruiker Britta Simon aangeroepen in FirmPlay - werknemer Advocacy voor werving maken. Neem contact op met [FirmPlay - werknemer Advocacy voor werving ondersteuningsteam](mailto:engineering@firmplay.com) toevoegen van de gebruikers in de FirmPlay - werknemer Advocacy voor werving platform.


### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie maakt inschakelen u Britta Simon Azure eenmalige aanmelding gebruiken door haar toegang verlenen aan FirmPlay - werknemer Advocacy voor werving.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen FirmPlay - werknemer Advocacy voor werving, moet u de volgende stappen uitvoeren:**

1. In de Azure-beheerportal, opent u de weergave toepassingen en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **FirmPlay - werknemer Advocacy voor werving**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_50.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    


### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Wanneer u klikt op de FirmPlay - werknemer Advocacy werving tegel in het deelvenster toegang u moet ophalen automatisch aangemeld bij uw FirmPlay - werknemer Advocacy voor werving toepassing.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_203.png