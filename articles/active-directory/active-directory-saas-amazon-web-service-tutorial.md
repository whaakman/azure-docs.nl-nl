---
title: 'Zelfstudie: Azure Active Directory-integratie met Amazon Web Services (AWS) | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Amazon Web Services (AWS).
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 0fb9c8f428368271b548e3f174726fa01ea910c5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws"></a>Zelfstudie: Azure Active Directory-integratie met Amazon Web Services (AWS)

In deze zelfstudie leert u hoe Amazon Web Services (AWS) integreren met Azure Active Directory (Azure AD).

Amazon Web Services (AWS) integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD met toegang op de Amazon Web Services (AWS)
- U kunt uw gebruikers automatisch ophalen aangemelde naar Amazon Web Services (AWS) (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

<!--## Overview

To enable single sign-on with Amazon Web Services (AWS), it must be configured to use Azure Active Directory as an identity provider. This guide provides information and tips on how to perform this configuration in Amazon Web Services (AWS).

>[!Note]: 
>This embedded guide is brand new in the new Azure portal, and we’d love to hear your thoughts. Use the Feedback ? button at the top of the portal to provide feedback. The older guide for using the [Azure classic portal](https://manage.windowsazure.com) to configure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Amazon Web Services (AWS), moet u de volgende items:

- Een Azure AD-abonnement
- Amazon Web Services (AWS) eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- U moet uw productieomgeving niet gebruiken tenzij dit noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Amazon Web Services (AWS) uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a>Amazon Web Services (AWS) uit de galerie toevoegen
Voor het configureren van de integratie van Amazon Web Services (AWS) in Azure AD, moet u Amazon Web Services (AWS) uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Amazon Web Services (AWS) uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure Portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Klik op **toevoegen** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Amazon Web Services (AWS)**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_search.png)

5. Selecteer in het deelvenster resultaten **Amazon Web Services (AWS)**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met Amazon Web Services (AWS) op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Amazon Web Services (AWS) is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Amazon Web Services (AWS) tot stand worden gebracht.

Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Amazon Web Services (AWS).

Om te configureren en testen van Azure AD eenmalige aanmelding met Amazon Web Services (AWS), moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een gebruiker met Amazon Web Services test](#creating-an-amazon-web-services-test-user)**  - hebben een equivalent van Britta Simon in Amazon Web Services (AWS) die is gekoppeld aan de Azure AD-representatie van haar.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In deze sectie maakt u Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing Amazon Web Services (AWS).

**Voor het configureren van Azure AD eenmalige aanmelding met Amazon Web Services (AWS), moet u de volgende stappen uitvoeren:**

1. In de Azure-Portal op de **Amazon Web Services (AWS)** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster als **modus** Selecteer **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_samlbase.png)

3. Op de **Amazon Web Services (AWS) domein en de URL's** sectie, de gebruiker heeft geen alle stappen uitvoeren als de app al vooraf is geïntegreerd met Azure.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_url.png)

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het XML-bestand op uw computer.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_certificate.png)

5. De SAML-asserties verwacht de Amazon Web Services (AWS) softwaretoepassing in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt beheren de waarden van deze kenmerken van de '**gebruikerskenmerken**' sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_attribute.png)

6. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding hierboven en voer de volgende stappen uit:
    
    | Naam van kenmerk  | De waarde van kenmerk | naamruimte |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | User.userPrincipalName | https://AWS.Amazon.com/SAML/Attributes |
    | Rol            | User.assignedroles |  https://AWS.Amazon.com/SAML/Attributes |
    
    >[!TIP]
    >U moet de gebruiker wordt ingericht in Azure AD ophalen van alle functies van de AWS-Console configureren. Raadpleeg de onderstaande stappen inrichting.

    a. Klik op **toevoegen kenmerk** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_04.png)

    b. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_05.png)

    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij. De waarde Namespace toevoegen zoals hierboven vermeld.
    
    d. Klik op **OK**.

7. Klik op **opslaan** knop om de instellingen in Azure.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_400.png)

8. In een ander browservenster aanmelding bij uw bedrijf Amazon Web Services (AWS) site als administrator.

9. Klik op **Console-startpagina**.
   
    ![Eenmalige aanmelding configureren][11]

10. Klik op **IAM** van **beveiliging, identiteit en compatibiliteit** service.
   
    ![Eenmalige aanmelding configureren][12]

11. Klik op **identiteitsproviders**, en klik vervolgens op **Provider maken**.
   
    ![Eenmalige aanmelding configureren][13]

12. Op de **Provider configureren** dialoogvenster pagina, voert u de volgende stappen uit:
   
    ![Eenmalige aanmelding configureren][14]
 
    a. Als **providertype**, selecteer **SAML**.

    b. In de **providernaam** textbox, typ een providernaam (bijvoorbeeld: *WAAD*).

    c. Als u wilt uw van het gedownloade metagegevensbestand uploadt, klikt u op **bestand kiezen**.

    d. Klik op **volgende stap**.

13. Op de **providerinformatie controleren** dialoogvenster pagina, klikt u op **maken**. 
    
    ![Eenmalige aanmelding configureren][15]

14. Klik op **rollen**, en klik vervolgens op **nieuwe rol maken**. 
    
    ![Eenmalige aanmelding configureren][16]

15. Op de **rolnaam ingesteld** dialoogvenster de volgende stappen uitvoeren: 
    
    ![Eenmalige aanmelding configureren][17] 

    a. In de **rolnaam** textbox, typ een rolnaam (bijvoorbeeld: *testgebruiker*). 

    b. Klik op **volgende stap**.

16. Op de **Roltype Selecteer** dialoogvenster de volgende stappen uitvoeren: 
    
    ![Eenmalige aanmelding configureren][18] 

    a. Selecteer **rol voor toegang van de Provider identiteit**. 

    b. In de **Grant Web eenmalige aanmelding (WebSSO) toegang tot de SAML-providers** sectie, klikt u op **Selecteer**.

17. Op de **vertrouwensrelatie tot stand brengen** dialoogvenster de volgende stappen uitvoeren:  
    
    ![Eenmalige aanmelding configureren][19] 

    a. Selecteer de SAML-provider die u eerder hebt gemaakt als SAML-provider (bijvoorbeeld: *WAAD*)
  
    b. Klik op **volgende stap**.

18. Op de **controleren rol vertrouwen** dialoogvenster, klikt u op **volgende stap**.
    
    ![Eenmalige aanmelding configureren][32]

19. Op de **beleid koppelen** dialoogvenster, klikt u op **volgende stap**.
    
    ![Eenmalige aanmelding configureren][33]

20. Op de **revisie** dialoogvenster de volgende stappen uitvoeren:
    
    ![Eenmalige aanmelding configureren][34]
 
    a. Klik op **rol maken**.

    b. Zo veel functies naar behoefte maken en toe te wijzen aan de id-Provider.

21. De gebruiker om het ophalen van alle functies van de AWS nu configureren

    a. In het AWS-Console-aanmelding met het root-account

    b. Klik op uw naam in de rechterbovenhoek en klik vervolgens op de **mijn beveiligingsreferenties** optie. Hiermee opent u een scherm als een waarschuwing weergegeven. Klik op de knop **beveiligingsreferenties** knop om door te geven van het scherm.
        
       ![Eenmalige aanmelding configureren][36]

       ![Eenmalige aanmelding configureren][37]

    c. Klik in de sectie toegangstoetsen op de **toegangssleutel maken** knop. Hiermee wordt de toegangssleutel-ID en een token waarde gegenereerd.
    
       ![Eenmalige aanmelding configureren][38]

    d. Kopieer deze beide waarden en ook downloaden, zodat u deze niet kwijtraakt.

    e. Klik in de Azure-Portal op de pagina Amazon Web Services (AWS) toepassingen integratie **inrichten**.
        
       ![Eenmalige aanmelding configureren][35]

    f. De modus inrichten instellen op **automatische**
        
       ![Eenmalige aanmelding configureren][39]

    g. Nu in de **clientsecret** en **geheim Token** plakt u de bijbehorende waarden die u hebt gekopieerd vanaf AWS-Console.
    
    h. U kunt klikken op de **verbinding testen** knop voor het testen van de connectiviteit. Als dat is geslaagd kunt u de inrichting connector starten.
       
       ![Eenmalige aanmelding configureren][40]

    ik. Nu de Status van inrichting in inschakelen **op**. Hiermee start u de rollen ophalen uit de toepassing.

       ![Eenmalige aanmelding configureren][41]

    > [!NOTE]
    > Azure AD inrichten de service wordt uitgevoerd elke na enige tijd om te synchroniseren van de rollen van AWS. U ziet de identiteitsprovider gekoppeld AWS rollen in Azure AD en u ze kunt gebruiken bij het toewijzen van de toepassing aan gebruikers of groepen.

<!--### Next steps

To ensure users can sign-in to Amazon Web Services (AWS) after it has been configured to use Azure Active Directory, review the following tasks and topics:

- User accounts must be pre-provisioned into Amazon Web Services (AWS) prior to sign-in. To set this up, see Provisioning.
 
- Users must be assigned access to Amazon Web Services (AWS) in Azure AD to sign-in. To assign users, see Users.
 
- To configure access polices for Amazon Web Services (AWS) users, see Access Policies.
 
- For additional information on deploying single sign-on to users, see [this article](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_01.png) 

2. Ga naar **gebruikers en groepen** en klik op **alle gebruikers** om de lijst met gebruikers weer te geven.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_02.png) 

3. Klik aan de bovenkant van het dialoogvenster **toevoegen** openen de **gebruiker** dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-an-amazon-web-services-test-user"></a>Maken van een testgebruiker Amazon Web Services

Om in te schakelen Azure AD-gebruikers zich aanmelden op de Amazon Web Services (AWS), als ze in Amazon Web Services (AWS) worden ingericht. In het geval van Amazon Web Services (AWS) is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **Amazon Web Services (AWS)** bedrijf site als administrator.

2. Klik op de **Console-startpagina** pictogram. 
   
    ![Eenmalige aanmelding configureren][11]

3. Klik op Identity and Access Management. 
   
    ![Eenmalige aanmelding configureren][28]

4. Klik in het Dashboard op **gebruikers**, en klik vervolgens op **Create New Users**. 
   
    ![Eenmalige aanmelding configureren][29]

5. Voer de volgende stappen uit in het dialoogvenster gebruiker maken: 
   
    ![Eenmalige aanmelding configureren][30]   
    
    a. In de **Voer gebruikersnamen** tekstvakken, typ de naam van Brita Simon gebruiker (userprincipalname) in Azure AD.

    b. Klik op **maken.**
        
### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding gebruiken door haar toegang verlenen aan Amazon Web Services (AWS).

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon naar Amazon Web Services (AWS), moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Amazon Web Services (AWS)**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Op **rol selecteren** tabblad, selecteer de juiste rol voor de gebruiker. Alle deze rollen worden weergegeven met de rolnaam en de naam van de id-provider. Op deze manier kunt u eenvoudig de rollen van AWS identificeren.

8. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Amazon Web Services (AWS) in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Amazon Web Services (AWS). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_203.png
[11]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795031.png
[12]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795032.png
[13]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795033.png
[14]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795034.png
[15]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795035.png
[16]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795022.png
[17]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795023.png
[18]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795024.png
[19]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795025.png
[20]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950351.png
[21]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_80.png
[22]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950352.png
[23]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_81.png
[24]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950353.png
[25]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_15.png

[28]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950321.png
[29]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795037.png
[30]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795038.png
[32]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950251.png
[33]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950252.png
[34]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950253.png
[35]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning.png
[36]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_on.png
