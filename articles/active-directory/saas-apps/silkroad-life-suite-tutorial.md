---
title: 'Zelfstudie: Azure Active Directory-integratie met SilkRoad leven Suite | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SilkRoad leven Suite.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3cd92319-7964-41eb-8712-444f5c8b4d15
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/12/2017
ms.author: jeedes
ms.openlocfilehash: 962b3f0e18c2dbedf80c0cfca6bc8275b394307b
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39046012"
---
# <a name="tutorial-azure-active-directory-integration-with-silkroad-life-suite"></a>Zelfstudie: Azure Active Directory-integratie met SilkRoad leven Suite

In deze zelfstudie leert u hoe u SilkRoad leven Suite integreren met Azure Active Directory (Azure AD).

SilkRoad leven Suite integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot SilkRoad leven Suite heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij SilkRoad leven Suite (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SilkRoad leven Suite, moet u de volgende items:

- Een Azure AD-abonnement
- Een Suite van SilkRoad levensduur van eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SilkRoad leven Suite uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-silkroad-life-suite-from-the-gallery"></a>SilkRoad leven Suite uit de galerie toe te voegen
Voor het configureren van de integratie van SilkRoad leven Suite in Azure AD, moet u SilkRoad leven Suite uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SilkRoad leven Suite uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **SilkRoad leven Suite**, selecteer **SilkRoad leven Suite** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![SilkRoad leven Suite in de lijst met resultaten](./media/silkroad-life-suite-tutorial/tutorial_silkroadlifesuite_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met SilkRoad leven Suite op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in SilkRoad leven Suite is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in SilkRoad leven Suite tot stand worden gebracht.

In SilkRoad leven Suite, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met SilkRoad leven Suite, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker SilkRoad leven Suite](#create-a-silkroad-life-suite-test-user)**  : als u wilt een equivalent van Britta Simon in SilkRoad leven Suite die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing SilkRoad leven Suite.

**Voor het configureren van Azure AD eenmalige aanmelding met SilkRoad leven Suite, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **SilkRoad leven Suite** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/silkroad-life-suite-tutorial/tutorial_silkroadlifesuite_samlbase.png)

3. Op de **SilkRoad leven Suite domein en URL's** sectie, voert u de volgende stappen uit:

    ![SilkRoad leven Suite domein en URL's eenmalige aanmelding informatie](./media/silkroad-life-suite-tutorial/tutorial_silkroadlifesuite_url1.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<subdomain>.silkroad-eng.com/Authentication/`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: 
    | |
    |--|
    | `https://<subdomain>.silkroad-eng.com/Authentication/SP` |
    | `https://<subdomain>.silkroad.com/Authentication/SP` |

    c. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: 
    | |
    |--|
    | `https://<subdomain>.silkroad-eng.com/Authentication/` |
    | `https://<subdomain>.silkroad.com/Authentication/` |
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke-id, de antwoord-URL en aanmeldings-URL. Neem contact op met [SilkRoad leven Suite Client ondersteuningsteam](https://www.silkroad.com/locations/) om deze waarden te verkrijgen. 

4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/silkroad-life-suite-tutorial/tutorial_silkroadlifesuite_certificate.png) 

5. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/silkroad-life-suite-tutorial/tutorial_general_400.png)
    
6. Op de **configuratie van de Suite SilkRoad leven** sectie, klikt u op **SilkRoad leven-pakket configureren** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Configuratie van de Suite SilkRoad levensduur](./media/silkroad-life-suite-tutorial/tutorial_silkroadlifesuite_configure.png) 

7. Aanmelding bij uw bedrijf SilkRoad site als administrator. 
 
    >[!NOTE] 
    > Neem contact op met ondersteuning voor SilkRoad of met uw vertegenwoordiger SilkRoad Services voor het verkrijgen van toegang tot de toepassing SilkRoad leven Suite verificatie voor federatie configureren met Microsoft Azure AD.

8. Ga naar **serviceprovider**, en klik vervolgens op **Federation Details**. 
   
    ![Azure AD voor eenmalige aanmelding][10]

9. Klik op **Federatiemetagegevens downloaden**, en sla het bestand met metagegevens op uw computer.
   
    ![Azure AD voor eenmalige aanmelding][11] 

10. In uw **SilkRoad** toepassing, klikt u op **verificatie bronnen**.
   
    ![Azure AD voor eenmalige aanmelding][12] 

11. Klik op **verificatiebron toevoegen**. 
   
    ![Azure AD voor eenmalige aanmelding][13] 

12. In de **verificatiebron toevoegen** sectie, voert u de volgende stappen uit: 
   
    ![Azure AD voor eenmalige aanmelding][14]
  
    a. Onder **optie 2 - bestand met metagegevens**, klikt u op **Bladeren** voor het uploaden van het gedownloade metagegevensbestand vanuit Azure portal.
  
    b. Klik op **maken id-Provider met behulp van gegevens uit een bestand**.

13. In de **verificatie bronnen** sectie, klikt u op **bewerken**. 
    
     ![Azure AD voor eenmalige aanmelding][15] 

14. Op de **verificatiebron bewerken** dialoogvenster, voer de volgende stappen uit: 
    
     ![Azure AD voor eenmalige aanmelding][16] 

    a. Als **ingeschakeld**, selecteer **Ja**.

    b. In de **EntityId** tekstvak, plak de waarde van **SAML entiteit-ID** die u hebt gekopieerd vanuit Azure portal.
   
    c. In de **IdP beschrijving** tekstvak, typ een beschrijving voor uw configuratie (bijvoorbeeld: *Azure AD-eenmalige aanmelding*).

    d. In de **metagegevensbestand** tekstvak, Upload het **metagegevens** -bestand dat u hebt gedownload vanuit Azure portal.
  
    e. In de **IdP naam** tekstvak, typ een naam die specifiek is voor uw configuratie (bijvoorbeeld: *Azure SP*).
  
    f. In de **afmeldings-URL van Service** tekstvak, plak de waarde van **afmelding URL** die u hebt gekopieerd vanuit Azure portal.

    g. In de **aanmeldings-URL van de service** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML** die u hebt gekopieerd vanuit Azure portal.

    h. Klik op **Opslaan**.

15. Alle andere verificatie-bronnen uitschakelen. 
    
     ![Azure AD voor eenmalige aanmelding][17]

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/silkroad-life-suite-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/silkroad-life-suite-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/silkroad-life-suite-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/silkroad-life-suite-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-silkroad-life-suite-test-user"></a>Maak een testgebruiker SilkRoad leven Suite

In deze sectie maakt u een gebruiker met de naam van Britta Simon in SilkRoad leven Suite. Werken met [SilkRoad leven Suite Client ondersteuningsteam](https://www.silkroad.com/locations/) om toe te voegen de gebruikers in het platform SilkRoad leven Suite. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken. 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot SilkRoad leven Suite.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon SilkRoad leven Suite, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **SilkRoad leven Suite**.

    ![De koppeling SilkRoad leven Suite in de lijst met toepassingen](./media/silkroad-life-suite-tutorial/tutorial_silkroadlifesuite_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel SilkRoad leven Suite in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing SilkRoad leven Suite.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/silkroad-life-suite-tutorial/tutorial_general_01.png
[2]: ./media/silkroad-life-suite-tutorial/tutorial_general_02.png
[3]: ./media/silkroad-life-suite-tutorial/tutorial_general_03.png
[4]: ./media/silkroad-life-suite-tutorial/tutorial_general_04.png

[100]: ./media/silkroad-life-suite-tutorial/tutorial_general_100.png

[200]: ./media/silkroad-life-suite-tutorial/tutorial_general_200.png
[201]: ./media/silkroad-life-suite-tutorial/tutorial_general_201.png
[202]: ./media/silkroad-life-suite-tutorial/tutorial_general_202.png
[203]: ./media/silkroad-life-suite-tutorial/tutorial_general_203.png
[10]: ./media/silkroad-life-suite-tutorial/tutorial_silkroad_06.png
[11]: ./media/silkroad-life-suite-tutorial/tutorial_silkroad_07.png
[12]: ./media/silkroad-life-suite-tutorial/tutorial_silkroad_08.png
[13]: ./media/silkroad-life-suite-tutorial/tutorial_silkroad_09.png
[14]: ./media/silkroad-life-suite-tutorial/tutorial_silkroad_10.png
[15]: ./media/silkroad-life-suite-tutorial/tutorial_silkroad_11.png
[16]: ./media/silkroad-life-suite-tutorial/tutorial_silkroad_12.png
[17]: ./media/silkroad-life-suite-tutorial/tutorial_silkroad_13.png
