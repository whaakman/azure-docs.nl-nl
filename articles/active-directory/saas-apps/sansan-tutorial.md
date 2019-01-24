---
title: 'Zelfstudie: Azure Active Directory-integratie met Sansan | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Sansan.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: f653a0f2-c44a-4670-b936-68c136b578ea
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: 6bd84e42bf46885a9dced363724538cbd4d2066a
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54815001"
---
# <a name="tutorial-azure-active-directory-integration-with-sansan"></a>Zelfstudie: Azure Active Directory-integratie met Sansan

In deze zelfstudie leert u hoe u Sansan integreren met Azure Active Directory (Azure AD).

Sansan integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Sansan heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Sansan (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Sansan, moet u de volgende items:

- Een Azure AD-abonnement
- Een Sansan eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Sansan uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-sansan-from-the-gallery"></a>Sansan uit de galerie toe te voegen
Voor het configureren van de integratie van Sansan in Azure AD, moet u Sansan uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Sansan uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

4. Typ in het zoekvak **Sansan**.

    ![Het maken van een Azure AD-testgebruiker](./media/sansan-tutorial/tutorial_sansan_search.png)

5. Selecteer in het deelvenster resultaten **Sansan**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/sansan-tutorial/tutorial_sansan_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Sansan op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Sansan is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Sansan tot stand worden gebracht.

In Sansan, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Sansan, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker Sansan](#creating-a-sansan-test-user)**  : als u wilt een equivalent van Britta Simon in Sansan die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Sansan.

**Voor het configureren van Azure AD eenmalige aanmelding met Sansan, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Sansan** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/sansan-tutorial/tutorial_sansan_samlbase.png)

3. Op de **Sansan domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/sansan-tutorial/tutorial_sansan_url.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende patronen: 
    
    | Omgeving | URL |
    |:--- |:--- |
    | PC-web |`https://ap.sansan.com/v/saml2/<company name>/acs` |
    | Systeemeigen mobiele app |`https://internal.api.sansan.com/saml2/<company name>/acs` |
    | Instellingen voor mobiele browser |`https://ap.sansan.com/s/saml2/<company name>/acs` |  

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden met de werkelijke aanmeldings-URL. Neem contact op met [Sansan Client ondersteuningsteam](https://www.sansan.com/form/contact) om deze waarden te verkrijgen. 
     
4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/sansan-tutorial/tutorial_sansan_certificate.png) 

5. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/sansan-tutorial/tutorial_general_400.png)

6. Sansan toepassing verwacht meerdere **id's** en **antwoord-URL's** ter ondersteuning van meerdere omgevingen (PC web, systeemeigen mobiele Apps en instellingen voor mobiele browser), die kunnen worden geconfigureerd met behulp van PowerShell script. Gedetailleerde stappen worden hieronder beschreven.

7. Het configureren van meerdere **id's** en **antwoord-URL's** voor Sansan-toepassing met behulp van PowerShell-script, voert u de volgende stappen uit:

    ![Single Sign-On obj configureren](./media/sansan-tutorial/tutorial_sansan_objid.png)  

    a. Ga naar de **eigenschappen** pagina van **Sansan** toepassing en kopieert u de **Object-ID** met behulp van **kopie** knop en plak deze in Kladblok.

    b. De **Object-ID**, die u hebt gekopieerd vanuit Azure portal wordt gebruikt als **ServicePrincipalObjectId** in PowerShell-scripts die later in de zelfstudie wordt gebruikt. 

    c. Open nu een Windows PowerShell-opdrachtprompt met verhoogde bevoegdheid.
    
    >[!NOTE] 
    > U moet de AzureAD-module installeren (Gebruik de opdracht `Install-Module -Name AzureAD`). Als u hierom wordt gevraagd om een NuGet-module of de nieuwe Azure Active Directory V2 PowerShell-module te installeren, typt u Y en druk op ENTER.

    d. Voer `Connect-AzureAD` en meld u aan met een globale beheerdersaccount voor de gebruiker.

    e. Het volgende script gebruiken voor het bijwerken van meerdere URL's naar een toepassing:

    ```poweshell
     Param(
    [Parameter(Mandatory=$true)][guid]$ServicePrincipalObjectId,
    [Parameter(Mandatory=$false)][string[]]$ReplyUrls,
    [Parameter(Mandatory=$false)][string[]]$IdentifierUrls
    )

    $servicePrincipal = Get-AzureADServicePrincipal -ObjectId $ServicePrincipalObjectId

    if($ReplyUrls.Length)
    {
    echo "Updating Reply urls"
    Set-AzureADServicePrincipal -ObjectId $ServicePrincipalObjectId -ReplyUrls $ReplyUrls
    echo "updated"
    }
    if($IdentifierUrls.Length)
    {
    echo "Updating Identifier urls"
    $applications = Get-AzureADApplication -SearchString $servicePrincipal.AppDisplayName 
    echo "Found Applications =" $applications.Length
    $i = 0;
    do
    {  
    $application = $applications[$i];
    if($application.AppId -eq $servicePrincipal.AppId){
    Set-AzureADApplication -ObjectId $application.ObjectId -IdentifierUris $IdentifierUrls
    $servicePrincipal = Get-AzureADServicePrincipal -ObjectId $ServicePrincipalObjectId
    echo "Updated"
    return;
    }
    $i++;
    }while($i -lt $applications.Length);
    echo "Not able to find the matched application with this service principal"
    }
    ```

8. Na voltooiing van de PowerShell-script, het resultaat van het script is als volgt zoals hieronder wordt weergegeven en de URL-waarden wordt bijgewerkt, maar ze wordt niet ophalen zichtbaar zijn in Azure portal. 

    ![Script voor eenmalige aanmelding configureren](./media/sansan-tutorial/tutorial_sansan_powershell.png)


9. Op de **Sansan configuratie** sectie, klikt u op **configureren Sansan** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/sansan-tutorial/tutorial_sansan_configure.png) 

10. Het configureren van eenmalige aanmelding op **Sansan** zijde, moet u voor het verzenden van de gedownloade **certificaat**, **afmelding URL**, **SAML entiteit-ID**, en **Single Sign-On Service URL voor SAML** naar [Sansan ondersteuningsteam](https://www.sansan.com/form/contact). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

>[!NOTE]
>PC-browserinstelling werkt ook voor mobiele Apps en mobiele browser, samen met PC-website. 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/sansan-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/sansan-tutorial/create_aaduser_02.png) 

3. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/sansan-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/sansan-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-sansan-test-user"></a>Het maken van een testgebruiker Sansan

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Sansan. Sansan toepassing moet de gebruiker moet worden ingericht in de toepassing voordat u eenmalige aanmelding. 

>[!NOTE]
>Als u wilt maken van een gebruiker handmatig of batch-gebruikers, moet u contact opnemen met de [Sansan ondersteuningsteam](https://www.sansan.com/form/contact). 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Sansan.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan Sansan toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Sansan**.

    ![Eenmalige aanmelding configureren](./media/sansan-tutorial/tutorial_sansan_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Sansan in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Sansan.
Zie [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/sansan-tutorial/tutorial_general_01.png
[2]: ./media/sansan-tutorial/tutorial_general_02.png
[3]: ./media/sansan-tutorial/tutorial_general_03.png
[4]: ./media/sansan-tutorial/tutorial_general_04.png

[100]: ./media/sansan-tutorial/tutorial_general_100.png

[200]: ./media/sansan-tutorial/tutorial_general_200.png
[201]: ./media/sansan-tutorial/tutorial_general_201.png
[202]: ./media/sansan-tutorial/tutorial_general_202.png
[203]: ./media/sansan-tutorial/tutorial_general_203.png

