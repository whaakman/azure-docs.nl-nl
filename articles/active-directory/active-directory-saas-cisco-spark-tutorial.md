---
title: 'Zelfstudie: Azure Active Directory-integratie met Cisco Spark | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory- en Cisco Spark.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c47894b1-f5df-4755-845d-f12f4c602dc4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: a0a221622afe1c801a331e2319f3a7ace3111dad
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-spark"></a>Zelfstudie: Azure Active Directory-integratie met Cisco Spark

In deze zelfstudie leert u hoe Cisco Spark integreren met Azure Active Directory (Azure AD).

Cisco Spark integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Spark Cisco heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Cisco Spark (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Cisco Spark, moet u de volgende items:

- Een Azure AD-abonnement
- Een Cisco-Spark eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Cisco Spark uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-cisco-spark-from-the-gallery"></a>Cisco Spark uit de galerie toevoegen
Voor het configureren van de integratie van Cisco Spark in Azure AD, moet u Cisco Spark uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Cisco Spark uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Cisco Spark**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_search.png)

5. Selecteer in het deelvenster resultaten **Cisco Spark**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met Cisco Spark op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Cisco Spark in Azure AD voor een gebruiker is. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Cisco Spark tot stand worden gebracht.

Wijs in Cisco Spark, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Cisco Spark, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een Cisco-Spark testgebruiker](#creating-a-cisco-spark-test-user)**  - Cisco Spark die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing Cisco Spark.

**Voor het configureren van Azure AD eenmalige aanmelding met Cisco Spark, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Cisco Spark** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_samlbase.png)

3. Op de **Cisco Spark domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL als:`https://web.ciscospark.com/#/signin`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://idbroker.webex.com/<companyname>`

    > [!NOTE] 
    > Deze waarde is geen echte. Deze waarde door de werkelijke ID bijwerken. Neem contact op met [Cisco Spark Client ondersteuningsteam](https://support.ciscospark.com/) deze waarde op te halen. 
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_certificate.png) 

5. Cisco Spark toepassing verwacht de SAML-asserties naar specifieke kenmerken bevatten. Configureer de volgende kenmerken voor deze toepassing. U kunt de waarden van deze kenmerken van beheren de **gebruikerskenmerken** sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_07.png) 

6. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding hierboven en voer de volgende stappen uit:
    
    | Naam van kenmerk  | De waarde van kenmerk |
    | --------------- | -------------------- |    
    |   UID    | User.userPrincipalName |   

    a. Klik op **toevoegen kenmerk** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_attribute_04.png)

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_attribute_05.png)
    
    b. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.
    
    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    d. Klik op **OK**.

7. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_400.png)

8. Aanmelden bij [Cisco Cloud samenwerking Management](https://admin.ciscospark.com/) met uw volledige beheerdersreferenties.

9. Selecteer **instellingen** en klikt u onder de **verificatie** sectie, klikt u op **wijzigen**.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_10.png)
    
10. Selecteer **een 3rd derden identiteitsprovider integreren. (Geavanceerd)**  en Ga naar het volgende scherm.

11. Op de **Idp-metagegevens importeren** pagina beide slepen en neerzetten van het metagegevensbestand van de Azure AD naar de pagina of de optie van de browser bestand gebruiken om te zoeken en upload het metagegevensbestand van de Azure AD. Selecteer **certificaat dat is ondertekend door een certificeringsinstantie in de metagegevens (veiliger) vereisen** en klik op **volgende**. 
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_11.png)

12. Selecteer **SSO testverbinding**, en wanneer er wordt een nieuw browsertabblad geopend, verifiëren met Azure AD met aanmelden.

13. Ga terug naar de **Cisco Cloud samenwerking Management** browsertabblad. Als de test geslaagd is, schakelt u **deze test is geslaagd. Schakel de optie eenmalige aanmelding** en klik op **volgende**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-cisco-spark-test-user"></a>Maken van een Cisco-Spark testgebruiker

In deze sectie kunt u een gebruiker Britta Simon aangeroepen in Cisco Spark maken. In deze sectie kunt u een gebruiker Britta Simon aangeroepen in Cisco Spark maken.

1. Ga naar de [Cisco Cloud samenwerking Management](https://admin.ciscospark.com/) met uw volledige beheerdersreferenties.

2. Klik op **gebruikers** en vervolgens **gebruikers beheren**.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_12.png) 

3. In de **gebruiker beheren** Selecteer **handmatig toevoegen of wijzigen van gebruikers** en klik op **volgende**.

4. Selecteer **namen en e-mailadres**. Vul vervolgens het tekstvak als volgt in:
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_13.png) 
    
    a. In de **voornaam** textbox type **Britta**. 
    
    b. In de **achternaam** textbox type **Simon**.
    
    c. In de **e-mailadres** textbox type  **britta.simon@contoso.com** .

5. Klik op het plusteken om toe te voegen Britta Simon. Klik op **Volgende**.

6. In de **Services toevoegen voor gebruikers** venster, klikt u op **opslaan** en vervolgens **voltooien**.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Cisco Spark.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon met Cisco Spark, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Cisco Spark**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is het testen van uw Azure AD SSO-configuratie met behulp van het toegangsvenster.

Als u op de tegel Cisco Spark in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Cisco Spark.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_04.png
[10]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_060.png
[100]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_203.png

