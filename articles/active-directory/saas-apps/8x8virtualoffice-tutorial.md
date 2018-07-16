---
title: 'Zelfstudie: Azure Active Directory-integratie met 8 x 8 virtuele Office | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en 8 x 8 virtuele Office.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: b34a6edf-e745-4aec-b0b2-7337473d64c5
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 3a33f9ba0ca744709e21e9e55acc22b657c2adc2
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39048416"
---
# <a name="tutorial-azure-active-directory-integration-with-8x8-virtual-office"></a>Zelfstudie: Azure Active Directory-integratie met 8 x 8 virtuele Office

In deze zelfstudie leert u hoe u 8 x 8 virtuele Office integreren met Azure Active Directory (Azure AD).

Integratie van 8 x 8 biedt virtuele Office met Azure AD u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot 8 x 8 virtuele Office heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij 8 x 8 virtuele Office (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met 8 x 8 virtuele Office, moet u de volgende items:

- Een Azure AD-abonnement
- Een 8 x 8 virtuele Office eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. 8 x 8 virtuele Office uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-8x8-virtual-office-from-the-gallery"></a>8 x 8 virtuele Office uit de galerie toe te voegen
Voor het configureren van de integratie van 8 x 8 virtuele Office in Azure AD, moet u 8 x 8 virtuele Office uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen 8 x 8 virtuele Office uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **8 x 8 virtuele Office**.

    ![Het maken van een Azure AD-testgebruiker](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_search.png)

5. Selecteer in het deelvenster resultaten **8 x 8 virtuele Office**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met 8 x 8 die virtuele Office op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD weten wat de equivalente-gebruiker in 8 x 8 virtuele Office aan een gebruiker in Azure AD is. Met andere woorden, een koppeling de relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in 8 x 8 virtuele Office moet tot stand worden gebracht.

In 8 x 8 virtuele Office, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met 8 x 8 virtuele Office, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker van 8 x 8 virtuele Office](#creating-a-8x8-virtual-office-test-user)**  : als u wilt een equivalent van Britta Simon in 8 x 8 virtuele Office die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw 8 x 8 virtuele Office-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met 8 x 8 virtuele Office, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **8 x 8 virtuele Office** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_samlbase.png)

3. Op de **8 x 8 virtuele Office domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_url.png)

    a. In de **id** tekstvak, een URL met behulp van het volgende patroon:

    | `https://sso.8x8.com/<companyname>` |
    | `https://www.8x8.com/<companyname>` |
    | `https://sso.8x8pilot.com/<companyname>` |

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon:

    | `https://<subdomain>.8x8.com/saml2` |
    | `https://<subdomain>.8x8pilot.com/saml2`|

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke id en de antwoord-URL. Neem contact op met [8 x 8 virtuele Office-ondersteuningsteam](https://www.8x8.com/about-us/contact-us) om deze waarden te verkrijgen.
 


4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Raw)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/8x8virtualoffice-tutorial/tutorial_general_400.png)

6. Op de **8 x 8 virtuele Office configuratie** sectie, klikt u op **configureren 8 x 8 virtuele Office** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_configure.png) 

7. Aanmelding bij uw tenant 8 x 8 virtuele Office als beheerder.

8. Selecteer **virtuele Office Account Mgr** toepassing in het venster.
   
    ![Aan de App configureren](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_001.png)

9. Selecteer **Business** account die u wilt beheren en op **aanmelden** knop.
   
    ![Aan de App configureren](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_002.png)

10. Klik op **Accounts** tabblad in de menulijst met.
   
    ![Aan de App configureren](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_003.png)

11. Klik op **Single Sign On** in de lijst van Accounts.
   
    ![Aan de App configureren](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_004.png)

12. Selecteer **Single Sign On** onder verificatiemethode en klik op **SAML**.
    
    ![Aan de App configureren](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_005.png)

13. Kopiëren **SAML SSO-URL**, **eenmalig gebruik van de URL van één** en **URL-verlener** uit Azure AD naar **aanmelden URL**, **afmeldings-URL**  en **URL-verlener** in 8 x 8 virtuele Office. 
    
    ![Aan de App configureren](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_006.png)
    
14. Klik op **Browser** om het certificaat dat u hebt gedownload van Azure AD te uploaden en klik op de **opslaan** knop.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/8x8virtualoffice-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/8x8virtualoffice-tutorial/create_aaduser_02.png) 

3. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/8x8virtualoffice-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/8x8virtualoffice-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-8x8-virtual-office-test-user"></a>Het maken van een gebruiker 8 x 8 virtuele Office testen

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in 8 x 8 virtuele Office. 8 x 8 virtuele Office biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld.

Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot 8 x 8 virtuele Office als deze nog niet bestaat. 

>[!NOTE]
>Als u een gebruiker handmatig hebt gemaakt wilt, moet u contact op met de [8 x 8 virtuele Office-ondersteuningsteam](https://www.8x8.com/about-us/contact-us). 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot 8 x 8 virtuele Office.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon 8 x 8 virtuele Office, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **8 x 8 virtuele Office**.

    ![Eenmalige aanmelding configureren](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel 8 x 8 virtuele Office in het toegangsvenster, u moet u automatisch aangemeld bij uw 8 x 8 virtuele Office-toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md)

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/8x8virtualoffice-tutorial/tutorial_general_01.png
[2]: ./media/8x8virtualoffice-tutorial/tutorial_general_02.png
[3]: ./media/8x8virtualoffice-tutorial/tutorial_general_03.png
[4]: ./media/8x8virtualoffice-tutorial/tutorial_general_04.png

[100]: ./media/8x8virtualoffice-tutorial/tutorial_general_100.png

[200]: ./media/8x8virtualoffice-tutorial/tutorial_general_200.png
[201]: ./media/8x8virtualoffice-tutorial/tutorial_general_201.png
[202]: ./media/8x8virtualoffice-tutorial/tutorial_general_202.png
[203]: ./media/8x8virtualoffice-tutorial/tutorial_general_203.png

