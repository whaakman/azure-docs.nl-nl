---
title: 'Zelfstudie: Azure Active Directory-integratie met werkplek door Facebook | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en werkplek met Facebook.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 30f2ee64-95d3-44ef-b832-8a0a27e2967c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: jeedes
ms.openlocfilehash: 4f34f6509d762cba6aba98a6eba010fd94656e67
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workplace-by-facebook"></a>Zelfstudie: Azure Active Directory-integratie met werkplek door Facebook

In deze zelfstudie leert u hoe werkplek door Facebook integreren met Azure Active Directory (Azure AD).

Werkplek door Facebook integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de werkplek met Facebook heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld werkplek door Facebook (eenmalige aanmelding) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één locatie beheren: de Azure-portal.

Zie voor meer informatie over de software als een dienst (SaaS)-app-integratie met Azure AD [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met werkplek door Facebook, moet u de volgende items:

- Een Azure AD-abonnement
- Een werkplek door Facebook eenmalige aanmelding (SSO) abonnement ingeschakeld

Test de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie kunt u Azure AD SSO testen in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Werkplek door Facebook uit de galerie toevoegen.
2. Configureren en testen eenmalige aanmelding Azure AD.

## <a name="add-workplace-by-facebook-from-the-gallery"></a>Werkplek door Facebook uit de galerie toevoegen
Voor het configureren van de integratie van werkplek door Facebook in Azure AD werkplek door Facebook uit de galerie te toevoegt aan de lijst met beheerde SaaS-apps.

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster **Azure Active Directory**. 

    ![De Azure Active Directory-knop][1]

2. Blader naar **bedrijfstoepassingen** > **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Selecteer om de nieuwe toepassing toe **nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **werkplek door Facebook**, en selecteer **werkplek door Facebook** van resultaten. Selecteer vervolgens **toevoegen**, de toepassing toevoegen.

    ![Werkplek door Facebook in de lijst met resultaten](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_search.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD
In deze sectie kunt u configureren en testen van Azure AD-SSO met werkplek door Facebook, op basis van een testgebruiker genaamd "Britta Simon."

Azure AD moet weten wat de gebruiker equivalent in werkplek door Facebook is aan een gebruiker in Azure AD voor eenmalige aanmelding werkt. Met andere woorden, moet een gekoppelde relatie tussen een Azure AD-gebruiker en de verwante werkplek door Facebook-gebruiker worden vastgesteld.

Deze relatie tot stand brengen door toe te wijzen de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in werkplek met Facebook.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In deze sectie kunt u Azure AD-eenmalige aanmelding inschakelen in de Azure-portal en u eenmalige aanmelding configureren op uw werkplek met Facebook-toepassing.

1. In de Azure-portal op de **werkplek door Facebook** toepassing Integratiepagina **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. In de **eenmalige aanmelding** dialoogvenster, **modus** als **op basis van SAML aanmelding** eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_samlbase.png)

3. In de **werkplek Facebook-domein en URL's** sectie, het volgende doen:

    a. In de **aanmeldings-URL** tekstvak, typt u een URL die gebruikmaakt van het volgende patroon volgen:`https://<company subdomain>.facebook.com`

    b. In de **id** tekstvak, typt u een URL die gebruikmaakt van het volgende patroon volgen:`https://www.facebook.com/company/<scim company id>`

    > [!NOTE]
    > Deze waarden zijn alleen een voorbeeld. Deze waarden bijwerken met de werkelijke aanmeldings-URL en de id. Neem contact op met de [werkplek door Facebook Client-ondersteuningsteam](https://workplace.fb.com/faq/) ophalen van deze waarden. 

4. In de **SAML-certificaat voor ondertekening van** sectie **certificaat (Base64)**, en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_certificate.png) 

5. Selecteer **Opslaan**.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_400.png)

6. In de **werkplek door Facebook configuratie** sectie **werkplek configureren door Facebook** openen de **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids** sectie.

    ![Werkplek door Facebook-configuratie](./media/active-directory-saas-facebook-at-work-tutorial/config.png) 

7. In een ander browservenster aanmelden bij uw werkplek door Facebook bedrijf site als beheerder.
  
   > [!NOTE] 
   > Als onderdeel van het SAML-verificatieproces kunt werkplek queryreeksen van maximaal 2,5 kilobytes in grootte gebruiken om de parameters doorgeven aan Azure AD.

8. In de **bedrijf Dashboard**, gaat u naar de **verificatie** tabblad.

9. Onder **SAML-verificatie**, selecteer **eenmalige aanmelding alleen** uit de vervolgkeuzelijst.

10. Voer de waarden die zijn gekopieerd uit de **werkplek door Facebook configuratie** sectie van de Azure portal naar de bijbehorende velden:

    *   In de **SAML URL** tekst vak, plak de waarde van **Single Sign-On Service-URL**, die u hebt gekopieerd uit de Azure portal.
    *   In de **URL-verlener SAML** tekst vak, plak de waarde van **SAML entiteit-ID**, die u hebt gekopieerd uit de Azure portal.
    *   In **SAML afmelding omleiden (optioneel)**, plak de waarde van **Sign-Out URL**, die u hebt gekopieerd uit de Azure portal.
    *   Open uw **base-64 gecodeerde certificaat** in Kladblok gedownload van de Azure-portal. Kopieer de inhoud ervan naar het Klembord en plakt u deze naar de **SAML certificaat** in het tekstvak.

11. Mogelijk moet u de doelgroep URL invoeren, ontvanger URL en de URL van de ACS (Assertion Consumer-Service), worden vermeld in de **SAML-configuratie** sectie.

12. Ga naar de onderkant van de sectie en selecteer **Test SSO**. Een pop-upvenster wordt weergegeven, met de aanmeldingspagina van Azure AD. Om te verifiëren, Geef uw referenties als normaal. Zorg ervoor dat het e-mailadres dat is geretourneerd door Azure AD is hetzelfde als het werkplekaccount dat u bent aangemeld.

13. Als de test is voltooid, Ga naar de onderkant van de pagina en selecteer **opslaan**.

14. Iedereen met behulp van werkplek wordt nu weergegeven met Azure AD-aanmeldingspagina voor verificatie.

U kunt een SAML afmeldings-URL die kan worden gebruikt om te verwijzen op de pagina voor het afmelden van Azure AD configureren. Wanneer deze instelling is ingeschakeld en geconfigureerd, wordt de gebruiker niet langer doorgestuurd naar de afmelden pagina Werkplek. In plaats daarvan wordt de gebruiker omgeleid naar de URL die is toegevoegd in de SAML-afmelden omleidings-instelling.


> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt. Na het toevoegen van deze app uit de **Active Directory** > **bedrijfstoepassingen** sectie gewoon de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de functie embedded-documentatie in de [documentatie van Azure AD ingesloten]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="configure-reauthentication-frequency"></a>De frequentie van herauthenticatie configureren

U kunt configureren werkplek vraagt om een SAML-controle elke dag drie dagen één week, twee weken, één maand of nooit.

> [!NOTE] 
>De minimumwaarde voor de SAML-controle voor mobiele toepassingen is ingesteld op één week.

U kunt ook een SAML opnieuw instellen voor alle gebruikers afdwingen. Gebruik hiervoor de **vereisen SAML-verificatie voor alle gebruikers nu** knop.


### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

1. In de **Azure-portal**, selecteer in het linkerdeelvenster **Azure Active Directory**.

    ![De Azure Active Directory-knop](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en selecteer **alle gebruikers**.
    
    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, **toevoegen**.
 
    ![De knop toevoegen](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_03.png) 

4. In de **gebruiker** dialoogvenster de volgende handelingen uit:
 
    ![Het dialoogvenster gebruiker](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_04.png) 

    a. In de **naam** in het tekstvak **BrittaSimon**.

    b. In de **gebruikersnaam** in het tekstvak de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven**, en schrijf deze op.

    d. Selecteer **Maken**.
 
### <a name="create-a-workplace-by-facebook-test-user"></a>Maken van een werkplek door Facebook testgebruiker

In deze sectie wordt een gebruiker met de naam Britta Simon in werkplek gemaakt door Facebook. Werkplek door Facebook ondersteunt just-in-time-inrichting, die standaard is ingeschakeld.

Er is geen actie voor u in deze sectie. Als een gebruiker niet op de werkplek door Facebook bestaat, wordt een nieuw gemaakt wanneer u probeert te openen, werkplek met Facebook.

>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met de [werkplek door Facebook Client-ondersteuningsteam](https://workplace.fb.com/faq/).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon toegang te verlenen aan een werkplek door Facebook gebruikt Azure eenmalige aanmelding.

![Gebruiker toewijzen][200] 

1. Open de toepassingen in de Azure portal. Ga naar de directoryweergave, gaat u naar **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **werkplek door Facebook**.

    ![De werkplek door Facebook-koppeling in de lijst met toepassingen](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_app.png) 

3. Selecteer in het menu aan de linkerkant **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202] 

4. Selecteer **Toevoegen**. Klik in de **toevoegen toewijzing** deelvenster **gebruikers en groepen**.

    ![Het deelvenster toewijzing toevoegen][203]

5. In de **gebruikers en groepen** dialoogvenster, **Britta Simon** in de gebruikerslijst.

6. In de **gebruikers en groepen** dialoogvenster, **Selecteer**.

7. In de **toevoegen toewijzing** dialoogvenster, **toewijzen**.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het toegangsvenster.
Zie [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md) (Inleiding tot het toegangsvenster) voor meer informatie.


## <a name="next-steps"></a>Volgende stappen

* Zie de [lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md).
* Lees [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md).
* Meer informatie over het [configureren gebruikersaanvragen](active-directory-saas-facebook-at-work-provisioning-tutorial.md).



<!--Image references-->

[1]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_203.png

