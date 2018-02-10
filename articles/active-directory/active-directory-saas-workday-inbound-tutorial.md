---
title: 'Zelfstudie: Workday voor automatisch gebruikers inrichten met Azure Active Directory configureren | Microsoft Docs'
description: Informatie over het gebruik van Workday als bron van identiteitsgegevens voor Active Directory en Azure Active Directory.
services: active-directory
author: asmalser-msft
documentationcenter: na
manager: mtillman
ms.assetid: 1a2c375a-1bb1-4a61-8115-5a69972c6ad6
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/26/2018
ms.author: asmalser
ms.openlocfilehash: 2db9e60fe2807b1aa8ed7cab7eed6f7db8059a89
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="tutorial-configure-workday-for-automatic-user-provisioning"></a>Zelfstudie: Workday voor automatisch gebruikers inrichten configureren

Het doel van deze zelfstudie is zodat u de stappen die u uitvoeren moet om mensen uit Workday importeren in Active Directory en Azure Active Directory, met optionele Write-back van enkele kenmerken voor Workday. 



## <a name="overview"></a>Overzicht

De [Azure Active Directory-gebruikers inrichten van service](active-directory-saas-app-provisioning.md) kan worden geïntegreerd met de [Workday Human Resources API](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html) om het inrichten van accounts voor gebruikers. Azure AD maakt gebruik van deze verbinding, zodat de volgende gebruikers inrichten van werkstromen:

* **Gebruikers met Active Directory-inrichting** -geselecteerde sets van gebruikers uit Workday synchroniseren in een of meer Active Directory-forests. 

* **Gebruikers alleen in de cloud met Azure Active Directory-inrichting** -hybride gebruikers die aanwezig zijn in Active Directory en Azure Active Directory kunnen worden ingericht in de laatste via [AAD Connect](connect/active-directory-aadconnect.md). Gebruikers die alleen in de cloud kunnen echter rechtstreeks vanuit Workday worden ingericht op Azure Active Directory met het Azure AD-gebruiker-service inricht.

* **Write-back van e-adressen aan Workday** -de inrichting van de service Azure AD-gebruiker kan schrijven geselecteerd kenmerken van de Azure AD-gebruiker terug naar de werkdag, zoals het e-mailadres.

### <a name="scenarios-covered"></a>Scenario's worden behandeld

De Workday gebruiker inrichting werkstromen worden ondersteund door de service van Azure AD-gebruiker-inrichting inschakelen automatisering van de volgende human resources en de identiteit van de levenscyclus van scenario's voor beheer:

* **Nieuwe medewerkers inhuren** : wanneer een nieuwe werknemer wordt toegevoegd aan de werkdag, een gebruikersaccount wordt automatisch gemaakt in Active Directory, Azure Active Directory en eventueel Office 365 en [andere SaaS-toepassingen die worden ondersteund door Azure AD](active-directory-saas-app-provisioning.md), met terugschrijven van het e-mailadres werkdag.

* **Werknemer-kenmerk en het profiel updates** : wanneer een werknemersrecord wordt bijgewerkt in Workday (zoals de naam, titel of manager), wordt hun gebruikersaccount wordt automatisch bijgewerkt in Active Directory, Azure Active Directory en eventueel Office 365 en [andere SaaS-toepassingen die worden ondersteund door Azure AD](active-directory-saas-app-provisioning.md).

* **Werknemer afsluitingen worden** : wanneer een werknemer wordt beëindigd in werkdag, hun gebruikersaccount wordt automatisch uitgeschakeld in Active Directory, Azure Active Directory en eventueel Office 365 en [andere SaaS-toepassingen die worden ondersteund door Azure AD](active-directory-saas-app-provisioning.md).

* **Werknemer opnieuw huurt** : wanneer een werknemer is rehired in werkdag, hun oude account kan worden automatisch geactiveerd of opnieuw worden ingericht (afhankelijk van uw voorkeur) naar Active Directory, Azure Active Directory en eventueel Office 365 en [andere SaaS-toepassingen die worden ondersteund door Azure AD](active-directory-saas-app-provisioning.md).


## <a name="planning-your-solution"></a>Plannen van uw oplossing

Voordat u uw Workday-integratie, Controleer de onderstaande vereisten en lees de volgende instructies voor het overeenkomen met uw huidige Active Directory-architectuur en vereisten van gebruikers met de oplossing(en) verstrekt door Azure Active Directory.

### <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

* Een geldig abonnement voor Azure AD Premium-P1 met globale beheerderstoegang
* Een tenant van de implementatie Workday voor test-en-integratie
* Beheerdersmachtigingen in Workday maken van een gebruiker van de integratie van systeem en wijzigingen aanbrengen om werknemersgegevens voor testdoeleinden om te testen
* Voor gebruikers inrichten met Active Directory, een domein-server met Windows-Service 2012 of hoger is vereist op host de [lokale synchronisatie-agent](https://go.microsoft.com/fwlink/?linkid=847801)
* [Azure AD Connect](connect/active-directory-aadconnect.md) voor de synchronisatie tussen Active Directory en Azure AD


### <a name="solution-architecture"></a>Oplossingsarchitectuur

Azure AD levert een uitgebreide set voor het leveren van connectors om u te helpen bij het oplossen van de inrichting en de levenscyclus van identiteitsbeheer uit Workday voor Active Directory, Azure AD, SaaS-apps en meer. Onderdelen die u gebruikt en hoe u de oplossing hebt ingesteld, hangt af van de omgeving en vereisten van uw organisatie. Als een eerste stap rijtje hoeveel van de volgende aanwezig en geïmplementeerd in uw organisatie zijn:

* Hoeveel Active Directory-Forests worden gebruikt?
* Hoeveel Active Directory-domeinen worden gebruikt?
* Hoeveel Active Directory organisatie-eenheden (OE's) worden gebruikt?
* Hoeveel tenants van Azure Active Directory worden gebruikt?
* Zijn er gebruikers die moeten worden ingericht op Active Directory en Azure Active Directory (bijvoorbeeld 'hybride' gebruikers)?
* Zijn er gebruikers die moeten worden ingericht op Azure Active Directory, maar niet in Active Directory (bijvoorbeeld 'alleen in de cloud' gebruikers)?
* Moeten gebruiker e-mailadressen worden teruggeschreven naar Workday?

Zodra u antwoorden op deze vragen hebt, kunt u uw werkdag implementatie inrichten door de onderstaande richtlijnen te volgen plannen.

#### <a name="using-provisioning-connector-apps"></a>Met behulp van de inrichting connector-apps

Azure Active Directory biedt ondersteuning voor vooraf geïntegreerde inrichting connectors voor Workday en een groot aantal andere SaaS-toepassingen. 

Één inrichting connector interfaces met de API van een systeem met één bron en Inrichtingsgegevens kunt u een systeem met één doel. De meeste inrichting connectors die ondersteuning biedt voor Azure AD zijn voor een enkele bron en doel-systeem (bijvoorbeeld Azure AD ServiceNow) en kunnen worden ingesteld door de app toe te voegen in kwestie uit de galerie van Azure AD-app (bijvoorbeeld ServiceNow). 

Er is een-op-een relatie tussen inrichting connector-exemplaren en app-exemplaren in Azure AD:

| Bronsysteem | Doelsysteem |
| ---------- | ---------- | 
| Azure AD-tenant | SaaS-toepassing |


Als u werkt met Workday en Active Directory, zijn er echter meerdere bron en doel-systemen worden overwogen:

| Bronsysteem | Doelsysteem | Opmerkingen |
| ---------- | ---------- | ---------- |
| Werkdag | Active Directory-forest | Elk forest wordt behandeld als een afzonderlijke doelsysteem |
| Werkdag | Azure AD-tenant | Zo vereist zijn voor gebruikers die alleen in de cloud |
| Active Directory-forest | Azure AD-tenant | Deze stroom wordt verwerkt door de AAD Connect vandaag |
| Azure AD-tenant | Werkdag | Voor write-back van e-mailadressen |

Azure AD levert vergemakkelijkt deze meerdere werkstromen op meerdere bron en doel-systemen, meerdere apps voor inrichting connector die u kunt uit de galerie van Azure AD-app toevoegen:

![AAD-App-galerie](./media/active-directory-saas-workday-inbound-tutorial/WD_Gallery.PNG)

* **Werkdag voor het inrichten van Active Directory** -deze app vereenvoudigt gebruiker account inrichten vanuit Workday voor één Active Directory-forest. Als u meerdere forests hebt, kunt u één exemplaar van deze app kunt toevoegen van de app-galerie van Azure AD voor elk Active Directory-forest dat u wilt inrichten.

* **Werkdag voor Azure AD-inrichting** - terwijl AAD Connect het hulpprogramma dat moet worden gebruikt om Active Directory gebruikers met Azure Active Directory, deze app kunnen worden gebruikt wordt ter bevordering van de inrichting van gebruikers uit Workday in één Azure Active Directory-tenant alleen in de cloud te synchroniseren.

* **Write-back van werkdag** -deze app Write-back van gebruiker e-mailadressen van Azure Active Directory naar Workday vergemakkelijkt.

> [!TIP]
> De reguliere 'Workday'-app wordt gebruikt voor het instellen van eenmalige aanmelding tussen Workday en Azure Active Directory. 

Het instellen en configureren van deze speciale inrichting connector apps is het onderwerp van de resterende secties van deze zelfstudie. Welke apps u wilt configureren, is afhankelijk van de systemen die u inrichten, en hoeveel Active Directory-Forests en Azure AD wilt-tenants in uw omgeving zijn.

![Overzicht](./media/active-directory-saas-workday-inbound-tutorial/WD_Overview.PNG)

## <a name="configure-a-system-integration-user-in-workday"></a>Een systeemgebruiker integratie in Workday configureren
Een algemene vereiste voor alle Workday inrichting connectors is dat ze referenties voor een Workday-integratie systeemaccount verbinding maken met de API van Workday Human Resources nodig hebben. Deze sectie beschrijft het maken van een account system integrator in Workday.

> [!NOTE]
> Het is mogelijk om deze procedure overslaan en gebruiken in plaats daarvan een globale beheerdersaccount van Workday als het systeemaccount van de integratie. Dit werkt prima voor demo's, maar wordt niet aanbevolen voor productie-implementaties.

### <a name="create-an-integration-system-user"></a>Maken van een gebruiker van het systeem netwerkintegratie

**Maken van een systeemgebruiker integratie:**

1. Aanmelden bij uw Workday-tenant met een administrator-account. In de **Workday Workbench**, voer gebruiker maken in het zoekvak en klik vervolgens op **integratie systeemgebruiker maken**. 
   
    ![Gebruiker maken](./media/active-directory-saas-workday-inbound-tutorial/IC750979.png "gebruiker maken")
2. Voltooi de **integratie systeemgebruiker maken** taak door het opgeven van een gebruikersnaam en wachtwoord voor een nieuwe gebruiker van de integratie-systeem.  
 * Laat de **vereisen nieuwe wachtwoord bij volgende aanmelding** optie is uitgeschakeld, omdat deze gebruiker programmatisch aanmelden. 
 * Laat de **minuten voor de time-out sessie** met de standaardwaarde van 0, dit voorkomt dat de gebruiker sessies voortijdig een time-out opgetreden. 
   
    ![Maken van de systeemgebruiker integratie](./media/active-directory-saas-workday-inbound-tutorial/IC750980.png "integratie systeemgebruiker maken")

### <a name="create-a-security-group"></a>Maak een beveiligingsgroep
U moet een onbeperkte integratie systeembeveiligingsgroep maken en aan de gebruiker toewijzen.

**Een beveiligingsgroep maken:**

1. Voer beveiligingsgroep maken in het zoekvak en klik vervolgens op **beveiligingsgroep maken**. 
   
    ![Groep CreateSecurity](./media/active-directory-saas-workday-inbound-tutorial/IC750981.png "CreateSecurity groep")
2. Voltooi de **beveiligingsgroep maken** taak.  
3. Selecteer integratie Systeembeveiligingsgroep: onbeperkte uit de **Type van verpachte beveiligingsgroep** vervolgkeuzelijst.
4. Maak een beveiligingsgroep waarmee leden expliciet worden toegevoegd. 
   
    ![Groep CreateSecurity](./media/active-directory-saas-workday-inbound-tutorial/IC750982.png "CreateSecurity groep")

### <a name="assign-the-integration-system-user-to-the-security-group"></a>De integratie systeemgebruiker toewijzen aan de beveiligingsgroep

**Toewijzen van de systeemgebruiker integratie:**

1. Voer beveiligingsgroep bewerken in het zoekvak en klik vervolgens op **beveiligingsgroep bewerken**. 
   
    ![Beveiligingsgroep bewerken](./media/active-directory-saas-workday-inbound-tutorial/IC750983.png "beveiligingsgroep bewerken")
2. Zoek en selecteer de nieuwe beveiligingsgroep voor de integratie met de naam. 
   
    ![Beveiligingsgroep bewerken](./media/active-directory-saas-workday-inbound-tutorial/IC750984.png "beveiligingsgroep bewerken")
3. De nieuwe gebruiker van de integratie-systeem toevoegen aan de nieuwe beveiligingsgroep. 
   
    ![Systeembeveiligingsgroep](./media/active-directory-saas-workday-inbound-tutorial/IC750985.png "Systeembeveiligingsgroep")  

### <a name="configure-security-group-options"></a>Configureer de beveiligingsopties voor groep
In deze stap hebt u domeinbeveiliging beleid machtigingen voor de worker-gegevens worden beveiligd door het volgende beveiligingsbeleid voor domein verlenen:


| Bewerking | Beveiligingsbeleid voor domein |
| ---------- | ---------- | 
| Ophalen en plaatsen |  Extern Account inrichten |
| Ophalen en plaatsen | Werknemersgegevens: Openbare Worker rapporten |
| Ophalen en plaatsen | Werknemersgegevens: Alle posities |
| Ophalen en plaatsen | Werknemersgegevens: Huidige bezetting van informatie |
| Ophalen en plaatsen | Werknemersgegevens: Functie in werknemersprofiel |
| Weergeven en wijzigen | Werknemersgegevens: Werke-mailadres |

**Groep Beveiligingsopties configureren:**

1. Beveiligingsbeleid voor domein invoeren in het zoekvak en klik vervolgens op de koppeling **domein beveiligingsbeleid voor functiegebied**.  
   
    ![Beveiligingsbeleid voor domein](./media/active-directory-saas-workday-inbound-tutorial/IC750986.png "beveiligingsbeleid voor domein")  
2. Zoeken naar het systeem en selecteer de **System** functiegebied.  Klik op **OK**.  
   
    ![Beveiligingsbeleid voor domein](./media/active-directory-saas-workday-inbound-tutorial/IC750987.png "beveiligingsbeleid voor domein")  
3. Vouw in de lijst van beveiligingsbeleid voor het systeem functiegebied **beveiligingsbeheer** en selecteert u het beveiligingsbeleid voor domein **extern Account inrichten**.  
   
    ![Beveiligingsbeleid voor domein](./media/active-directory-saas-workday-inbound-tutorial/IC750988.png "beveiligingsbeleid voor domein")  
4. Klik op **machtigingen bewerken**, en klik op de **machtigingen bewerken** dialoogvenster pagina, de nieuwe beveiligingsgroep toevoegen aan de lijst met beveiligingsgroepen met **ophalen** en **plaatsen**  integratie machtigingen. 
   
    ![Machtigingen bewerken](./media/active-directory-saas-workday-inbound-tutorial/IC750989.png "machtigingen bewerken")  
5. Herhaal stap 1 hierboven om terug te keren naar het scherm voor het selecteren van functiegebieden en deze keer zoeken voor bezetting, selecteer de **personeel functiegebied** en klik op **OK**.
   
    ![Beveiligingsbeleid voor domein](./media/active-directory-saas-workday-inbound-tutorial/IC750990.png "beveiligingsbeleid voor domein")  
6. Vouw in de lijst van beveiligingsbeleid voor het personeel functiegebied **werknemersgegevens: personeel** en Herhaal stap 4 hierboven voor elk van deze resterende beveiligingsbeleid:

   * Werknemersgegevens: Openbare Worker rapporten
   * Werknemersgegevens: Alle posities
   * Werknemersgegevens: Huidige bezetting van informatie
   * Werknemersgegevens: Functie in werknemersprofiel
   
7. Herhaal stap 1, hierboven om terug te keren naar het scherm voor het selecteren van functionele gebieden en deze keer zoeken naar **contactgegevens**, selecteert u het functionele gebied personeel en klikt u op **OK**.

8.  Vouw in de lijst van beveiligingsbeleid voor het personeel functiegebied **werknemersgegevens: contactgegevens werk**, en Herhaal stap 4 hierboven voor beveiligingsbeleid van het onderstaande:

    * Werknemersgegevens: Werke-mailadres

    ![Beveiligingsbeleid voor domein](./media/active-directory-saas-workday-inbound-tutorial/IC750991.png "beveiligingsbeleid voor domein")  
    
### <a name="activate-security-policy-changes"></a>Wijzigingen in het beveiligingsbeleid activeren

**Wijzigingen in het beveiligingsbeleid activeren:**

1. Voer activeren in het zoekvak en klik vervolgens op de koppeling **activeren in behandeling zijnde wijzigingen in beveiligingsbeleid**. 
   
    ![Activate](./media/active-directory-saas-workday-inbound-tutorial/IC750992.png "Activate") 
2. Moet u de wijzigingen in behandeling het beveiligingsbeleid activeren taak eerst een opmerking invoeren voor controledoeleinden en klik vervolgens op **OK**. 
   
    ![In afwachting van beveiliging activeren](./media/active-directory-saas-workday-inbound-tutorial/IC750993.png "activeren in afwachting van beveiliging")   
3. De taak op het volgende scherm voltooien door het selectievakje **bevestigen**, en klik vervolgens op **OK**. 
   
    ![In afwachting van beveiliging activeren](./media/active-directory-saas-workday-inbound-tutorial/IC750994.png "activeren in afwachting van beveiliging")  

## <a name="configuring-user-provisioning-from-workday-to-active-directory"></a>Gebruikers inrichten van Workday naar Active Directory configureren
Volg deze instructies voor het inrichten van Workday voor elk Active Directory-forest die u nodig hebt met het inrichten van gebruikersaccount configureren.

### <a name="part-1-adding-the-provisioning-connector-app-and-creating-the-connection-to-workday"></a>Deel 1: De inrichting connector app toe te voegen en het maken van de verbinding met Workday

**Werkdag configureren voor het inrichten van Active Directory:**

1.  Ga naar <https://portal.azure.com>

2.  Selecteer in de linkernavigatiebalk **Azure Active Directory**

3.  Selecteer **bedrijfstoepassingen**, klikt u vervolgens **alle toepassingen**.

4.  Selecteer **een toepassing toevoegen**, en selecteer de **alle** categorie.

5.  Zoeken naar **Workday inrichten van Active Directory**, en die app uit de galerie toevoegt.

6.  Nadat de app is toegevoegd en het scherm app-informatie weergegeven, selecteert wordt **inrichten**

7.  Wijzig de **inrichting** **modus** naar **automatische**

8.  Voltooi de **beheerdersreferenties** sectie als volgt:

   * **Admin Username** – Geef de gebruikersnaam van het systeemaccount van Workday-integratie met de naam van de tenant-domein toegevoegd. **Moet als volgt uitzien:username@contoso4**

   * **Beheerderswachtwoord –** Geef het wachtwoord van het systeem-account van Workday-integratie

   * **Tenant-URL:** voert u de URL naar het Workday web services-eindpunt voor uw tenant. Dit moet eruitzien als: https://wd3-impl-services1.workday.com/ccx/service/contoso4, waarbij contoso4 is vervangen door de juiste tenantnaam en wd3 impl is vervangen door de juiste omgevingstekenreeks.

   * **Active Directory-Forest -** 'Name' van uw Active Directory-forest, worden geretourneerd door de powershell-cmdlet Get-ADForest. Dit is doorgaans een tekenreeks, zoals: *contoso.com*

   * **Active Directory-Container -** Voer de tekenreeks in de container waarin alle gebruikers in uw AD-forest. Voorbeeld: *organisatie-eenheid = standaardgebruikers, OU = Users, DC = contoso, DC = test*

   * **E-mailmelding –** Voer uw e-mailadres in en schakel het selectievakje 'e-mailbericht verzenden als een fout optreedt'.

   * Klik op de **testverbinding** knop. Als de verbindingstest is geslaagd, klikt u op de **opslaan** bovenaan op de knop. Als dit mislukt, Controleer of de Workday-referenties geldig in Workday zijn. 

![Azure Portal](./media/active-directory-saas-workday-inbound-tutorial/WD_1.PNG)

### <a name="part-2-configure-attribute-mappings"></a>Deel 2: Kenmerktoewijzingen configureren 

In deze sectie configureert u hoe gebruikersgegevens uit Workday loopt naar Active Directory.

1.  Op het tabblad inrichten onder **toewijzingen**, klikt u op **Workday werknemers synchroniseren OnPremises**.

2.  In de **bron objectbereik** veld, kunt u selecteren welke groepen gebruikers in Workday moet binnen het bereik van het inrichten van AD, door een reeks filters op basis van kenmerken te definiëren. Het standaardbereik is 'alle gebruikers in Workday'. Voorbeeld van de filters:

   * Voorbeeld: Scope aan gebruikers met werknemer-id's tussen 1000000 en 2000000

      * Kenmerk: WorkerID

      * Operator: REGEX-overeenkomst

      * Waarde: (1[0-9][0-9][0-9][0-9][0-9][0-9])

   * Voorbeeld: Alleen werknemers en niet contingent werknemers 

      * Kenmerk: werknemer-id

      * Operator: Niet NULL IS

3.  In de **doel Objectacties** veld globaal kunt u filteren welke acties moeten worden uitgevoerd op Active Directory zijn toegestaan. **Maak** en **Update** meest voorkomende zijn.

4.  In de **kenmerktoewijzingen** sectie, kunt u definiëren hoe afzonderlijke Workday kenmerken worden toegewezen aan Active Directory-kenmerken.

5. Klik op een bestaande kenmerktoewijzing bij te werken of klik op **toevoegen van nieuwe toewijzing** aan de onderkant van het scherm om toe te voegen nieuwe toewijzingen. De toewijzing van een individueel kenmerk ondersteunt deze eigenschappen:

      * **Toewijzingstype**

         * **Directe** : de waarde van het kenmerk Workday schrijft naar het kenmerk AD zonder wijzigingen

         * **Constante** -schrijven van een statische, constante string-waarde naar het AD-kenmerk

         * **Expressie** – kunt u een aangepaste waarde schrijven naar het kenmerk AD op basis van een of meer kenmerken van Workday. [Zie voor meer informatie in dit artikel op expressies](active-directory-saas-writing-expressions-for-attribute-mappings.md).

      * **Bronkenmerk** -gebruikerskenmerk uit Workday. Als het kenmerk dat u zoekt niet aanwezig is, Zie [aanpassen van de lijst met gebruikerskenmerken Workday](#customizing-the-list-of-workday-user-attributes).

      * **Standaardwaarde** : optioneel. Als het bronkenmerk een lege waarde heeft, wordt de toewijzing in plaats daarvan deze waarde schrijven.
            De configuratie van de meest voorkomende is leeg laten.

      * **Doelkenmerk** – het gebruikerskenmerk in Active Directory.

      * **Overeen met objecten met behulp van dit kenmerk** : of deze toewijzing moet worden gebruikt als unieke identificatie van gebruikers tussen Workday en Active Directory of niet. Deze wordt meestal ingesteld in het veld ID van de werknemer voor Workday die doorgaans wordt toegewezen aan een van de werknemer-ID-kenmerken in Active Directory.

      * **Overeenkomende voorrang** – meerdere overeenkomende kenmerken kan worden ingesteld. Wanneer er meerdere, moeten ze worden geëvalueerd in de volgorde gedefinieerd door dit veld. Als een overeenkomst is gevonden, er is geen verdere overeenkomende kenmerken worden geëvalueerd.

      * **Deze toewijzing is van toepassing**
       
         * **Altijd** : deze toewijzing is van toepassing op beide maken van een gebruikersaccount en acties bijwerken

         * **Alleen tijdens het maken van** -deze toewijzing is van toepassing alleen op gebruikersacties maken

6. Als u wilt uw toewijzingen opslaan, klikt u op **opslaan** boven aan de sectie kenmerk toewijzen.

![Azure Portal](./media/active-directory-saas-workday-inbound-tutorial/WD_2.PNG)

**Hieronder volgen enkele voorbeelden kenmerktoewijzingen tussen Workday en Active Directory met een aantal veelgebruikte expressies**

-   De expressie die is toegewezen aan de parentDistinguishedName AD-kenmerk kan worden gebruikt voor het inrichten van een gebruiker aan een specifieke organisatie-eenheid op basis van een of meer Workday bronkenmerken. In dit voorbeeld plaatst gebruikers in verschillende organisatie-eenheden, afhankelijk van hun gegevens plaats in Workday.

-   De expressie die is toegewezen aan het kenmerk userPrincipalName AD maken een UPN van firstName.LastName@contoso.com. Ook vervangt deze ongeldige tekens.

-   [Er is documentatie over het schrijven van expressies hier](active-directory-saas-writing-expressions-for-attribute-mappings.md)

  
| WERKDAG KENMERK | ACTIVE DIRECTORY-KENMERK |  OVEREENKOMENDE ID? | MAKEN / BIJWERKEN |
| ---------- | ---------- | ---------- | ---------- |
|  **WorkerID**  |  Werknemer-id | **Ja** | Geschreven op alleen maken | 
|  **Gemeente**   |   l   |     | Maken en bijwerken |
|  **Bedrijf**         | Bedrijf   |     |  Maken en bijwerken |
|  **CountryReferenceTwoLetter**      |   CO |     |   Maken en bijwerken |
| **CountryReferenceTwoLetter**    |  c  |     |         Maken en bijwerken |
| **SupervisoryOrganization**  | Afdeling  |     |  Maken en bijwerken |
|  **PreferredNameData**  |  displayName |     |   Maken en bijwerken |
| **Werknemer-id**    |  algemene naam    |   |   Geschreven op alleen maken |
| **Fax**      | facsimileTelephoneNumber     |     |    Maken en bijwerken |
| **FirstName**   | givenName       |     |    Maken en bijwerken |
| **Switch (\[Active\],, '0', 'True', '1')** |  accountDisabled      |     | Maken en bijwerken |
| **Mobile**  |    mobiele       |     |       Maken en bijwerken |
| **EmailAddress**    | E-mail    |     |     Maken en bijwerken |
| **ManagerReference**   | manager  |     |  Maken en bijwerken |
| **WorkSpaceReference** | physicalDeliveryOfficeName    |     |  Maken en bijwerken |
| **PostalCode**  |   postalCode  |     | Maken en bijwerken |
| **LocalReference** |  preferredLanguage  |     |  Maken en bijwerken |
| **Vervang (Mid (Vervang (\[werknemer-id\],, ' (\[ \\ \\ / \\ \\ \\ \\ \\ \\\[\\\\\]\\\\:\\\\;\\ \\|\\\\=\\\\,\\\\+\\\\\*\\ \\? \\ \\ &lt; \\ \\ &gt; \]) ', ' ',), 1, 20), ' ([\\\\.) \* \$] (file:///\\.) *$)", , "", , )**      |    sAMAccountName            |     |         Geschreven op alleen maken |
| **LastName**   |   SN   |     |  Maken en bijwerken |
| **CountryRegionReference** |  St     |     | Maken en bijwerken |
| **AddressLineData**    |  StreetAddress  |     |   Maken en bijwerken |
| **PrimaryWorkTelephone**  |  telephoneNumber   |     | Maken en bijwerken |
| **BusinessTitle**   |  titel     |     |  Maken en bijwerken |
| **Join("@",Replace(Replace(Replace(Replace(Replace(Replace(Replace( Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace( Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Join(".", [FirstName], [LastName]), , "([Øø])", , "oe", , ), , "[Ææ]", , "ae", , ), , "([äãàâãåáąÄÃÀÂÃÅÁĄA])", , "a", , ), , "([B])", , "b", , ), , "([CçčćÇČĆ])", , "c", , ), , "([ďĎD])", , "d", , ), , "([ëèéêęěËÈÉÊĘĚE])", , "e", , ), , "([F])", , "f", , ), , "([G])", , "g", , ), , "([H])", , "h", , ), , "([ïîìíÏÎÌÍI])", , "i", , ), , "([J])", , "j", , ), , "([K])", , "k", , ), , "([ľłŁĽL])", , "l", , ), , "([M])" ,, ''M',), '([ñńňÑŃŇN])', "n",), '([öòőõôóÖÒŐÕÔÓO])', "o",), '([P])', 'p',), '([Q])', 'q',), '([řŘR])', 'r',), '([ßšśŠŚS])', "s",), '([TŤť])', "t",), '([üùûúůűÜÙÛÚŮŰU])', "u",), '([V])', 'v',), '([B])', 'w',), '([ýÿýŸÝY])', 'y',), '([źžżŹŽŻZ])', 'z',), ' ',,, ' ',), 'contoso.com')**   | userPrincipalName     |     | Geschreven op alleen maken                                                   
| **Switch (\[gemeente\], ' organisatie-eenheid standaardgebruikers, OU = gebruikers, OU = = standaard, organisatie-eenheid locaties, DC = = contoso, DC = com ', 'Dallas' ' organisatie-eenheid standaardgebruikers, OU = gebruikers, OU = Dallas, OU = locaties, DC = = contoso, DC = com ', 'Austin' ' organisatie-eenheid standaardgebruikers, OU = gebruikers, OU = Austin, OU = locaties, DC = = contoso, DC = com ","Seattle"' organisatie-eenheid standaardgebruikers, OU = gebruikers, OU = Seattle, OU = locaties, DC = = contoso, DC = com ', 'Londen', ' organisatie-eenheid standaardgebruikers = OU = Users, organisatie-eenheid Londen, OU = locaties, DC = = contoso, DC = com ")**  | parentDistinguishedName     |     |  Maken en bijwerken |
  
### <a name="part-3-configure-the-on-premises-synchronization-agent"></a>Deel 3: De lokale synchronisatie-agent configureren

Als u wilt inrichten met Active Directory on-premises, moet een agent worden geïnstalleerd op een domein-server in de wens Active Directory-forest. Domein-(of ondernemingsadministrator-) referenties vereist zijn om de procedure te voltooien.

**[U kunt de lokale agent voor Wachtwoordsynchronisatie hier downloaden](https://go.microsoft.com/fwlink/?linkid=847801)**

Na de installatie van agent, voer de onderstaande Powershell-opdrachten voor het configureren van de agent voor uw omgeving.

**Opdracht #1**

> cd C:\\programmabestanden\\Microsoft Azure Active Directory-synchronisatieagent\\Modules\\AADSyncAgent

> AADSyncAgent.psd1 import-module

**Opdracht #2**

> Add-ADSyncAgentActiveDirectoryConfiguration

* Invoer: Voor 'Directory Name', voer de naam van de AD-Forest, zoals ingevoerd gedeeltelijk \#2
* Invoer: De gebruikersnaam van de beheerder en het wachtwoord voor Active Directory-forest

**Opdracht #3**

> Add-ADSyncAgentAzureActiveDirectoryConfiguration

* Invoer: De gebruikersnaam van de globale beheerder en het wachtwoord voor uw Azure AD-tenant

>[!IMPORTANT]
>Er is momenteel een bekend probleem met de hoofdbeheerdersreferenties werkt niet als ze met een aangepast domein (voorbeeld: admin@contoso.com). Als tijdelijke oplossing maken en gebruiken van een globale beheerdersaccount met een onmicrosoft.com-domein (voorbeeld: admin@contoso.onmicrosoft.com)


**Opdracht #4**

> Get-AdSyncAgentProvisioningTasks

* Actie: Controleer de gegevens worden geretourneerd. Met deze opdracht detecteert automatisch Workday inrichting van apps in uw Azure AD-tenant. Voorbeelduitvoer:

> Naam: Mijn AD-Forest
>
> Ingeschakeld: True
>
> DirectoryName: mydomain.contoso.com
>
> Bevoegde: False
>
> Identifier    : WDAYdnAppDelta.c2ef8d247a61499ba8af0a29208fb853.4725aa7b-1103-41e6-8929-75a5471a5203

**Opdracht #5**

> Start AdSyncAgentSynchronization-automatische

**Opdracht #6**

> net stop aadsyncagent

**Opdracht #7**

> net start aadsyncagent

>[!TIP]
>Naast de 'net'-opdrachten in Powershell de synchronisatie-agent-service kan ook worden gestart en gestopt met het gebruik **Services.msc**. Als er fouten optreden bij het uitvoeren van de Powershell-opdrachten, zorg ervoor dat de **Microsoft Azure AD Connect inrichting Agent** wordt uitgevoerd in de **Services.msc**.

![Services](./media/active-directory-saas-workday-inbound-tutorial/Services.png)  

**Aanvullende configuratie voor klanten in de Europese Unie**

Als uw Azure Active Directory-tenant bevindt zich in een van de EU-datacenters, volgt u de volgende aanvullende stappen uit.

1. Open **Services.msc** , en stop de **Microsoft Azure AD Connect inrichting Agent** service.
2. Ga naar de installatiemap van de agent (voorbeeld: C:\Program Files\Microsoft Azure AD Connect inrichting Agent).
3. Open **SyncAgnt.exe.config** in een teksteditor.
4. Vervang https://manage.hub.syncfabric.windowsazure.com/Management met **https://eu.manage.hub.syncfabric.windowsazure.com/Management**
5. Vervang https://provision.hub.syncfabric.windowsazure.com/Provisioning met **https://eu.provision.hub.syncfabric.windowsazure.com/Provisioning**
6. Sla de **SyncAgnt.exe.config** bestand.
7. Open **Services.msc**, en start de **Microsoft Azure AD Connect inrichting Agent** service.

**Het oplossen van agent**

De [Windows-gebeurtenislogboek](https://technet.microsoft.com/en-us/library/cc722404(v=ws.11).aspx) op de Windows Server-computer die als host fungeert voor de agent bevat gebeurtenissen die zijn voor alle bewerkingen die door de agent wordt uitgevoerd. Deze gebeurtenissen weergeven:
    
1. Open **Eventvwr.msc**.
2. Selecteer **Windows-Logboeken > toepassing**.
3. Alle gebeurtenissen geregistreerd onder de bron weergeven **AADSyncAgent**. 
4. Controleren op fouten en waarschuwingen.

Als er een probleem met de rechten met de Active Directory of een Azure Active Directory-referenties in de Powershell-opdrachten, ziet u een foutbericht zoals deze: 
    
![Gebeurtenislogboeken](./media/active-directory-saas-workday-inbound-tutorial/Windows_Event_Logs.png) 


### <a name="part-4-start-the-service"></a>Deel 4: Start de service
Wanneer onderdelen 1-3 hebt voltooid, kunt u de inrichting service starten in de Azure portal.

1.  In de **inrichten** tabblad, stelt u de **inrichting Status** naar **op**.

2. Klik op **Opslaan**.

3. Hiermee start u de eerste synchronisatie, waarmee een variabele aantal uren, afhankelijk van hoeveel gebruikers in Workday zijn kan duren.

4. Controleer op elk gewenst moment de **controlelogboeken** tabblad in de Azure portal om te zien welke acties de inrichting-service heeft uitgevoerd. De controlelogboeken geeft een lijst van alle afzonderlijke sync gebeurtenissen die worden uitgevoerd door de inrichting service, zoals welke gebruikers worden gelezen uit Workday en klik vervolgens toegevoegd of bijgewerkt naar Active Directory. **[Raadpleeg de inrichting reporting guide voor gedetailleerde instructies voor het lezen van de controlelogboeken](active-directory-saas-provisioning-reporting.md)**

5.  Controleer de [Windows-gebeurtenislogboek](https://technet.microsoft.com/en-us/library/cc722404(v=ws.11).aspx) op de Windows Server die als host fungeert voor de agent voor nieuwe fouten of waarschuwingen. Deze gebeurtenissen kunnen worden bekeken door starten **Eventvwr.msc** op de server en het selecteren van **Windows-Logboeken > toepassing**. Alle berichten met betrekking tot inrichting worden geregistreerd onder de bron **AADSyncAgent**. 
    

6. Een voltooid, wordt er een overzichtsrapport van de audit schrijven in de **inrichten** tabblad, zoals hieronder wordt weergegeven.

![Azure Portal](./media/active-directory-saas-workday-inbound-tutorial/WD_3.PNG)


## <a name="configuring-user-provisioning-to-azure-active-directory"></a>Gebruikers inrichten met Azure Active Directory configureren
Hoe u het inrichten van Azure Active Directory configureren afhankelijk van uw provisioning vereisten, zoals beschreven in de onderstaande tabel.

| Scenario | Oplossing |
| -------- | -------- |
| **Gebruikers moeten worden ingericht op Active Directory en Azure AD** | Gebruik  **[AAD Connect](connect/active-directory-aadconnect.md)** |
| **Gebruikers moeten worden ingericht met Active Directory alleen** | Gebruik  **[AAD Connect](connect/active-directory-aadconnect.md)** |
| **Gebruikers moeten worden ingericht met alleen Azure AD (alleen cloud)** | Gebruik de **werkdag voor het inrichten van Azure Active Directory** -app in de app-galerie |

Zie voor instructies over het instellen van Azure AD Connect de [documentatie van Azure AD Connect](connect/active-directory-aadconnect.md).

De volgende secties beschrijven instellen van een verbinding tussen Workday en Azure AD om in te richten gebruikers alleen in de cloud.

> [!IMPORTANT]
> Volg de onderstaande procedure alleen als u alleen in de cloud-gebruikers die moeten worden ingericht met Azure AD en niet on-premises Active Directory.

### <a name="part-1-adding-the-azure-ad-provisioning-connector-app-and-creating-the-connection-to-workday"></a>Deel 1: De Azure AD-app voor inrichting-connector toe te voegen en het maken van de verbinding met Workday

**Werkdag configureren voor het inrichten van Azure Active Directory voor gebruikers alleen in de cloud:**

1.  Ga naar <https://portal.azure.com>.

2.  Selecteer in de linkernavigatiebalk **Azure Active Directory**

3.  Selecteer **bedrijfstoepassingen**, klikt u vervolgens **alle toepassingen**.

4.  Selecteer **een toepassing toevoegen**, en selecteer vervolgens de **alle** categorie.

5.  Zoeken naar **Workday voor het inrichten van Azure AD**, en die app uit de galerie toevoegt.

6.  Nadat de app is toegevoegd en het scherm app-informatie weergegeven, selecteert wordt **inrichten**

7.  Wijzig de **inrichting** **modus** naar **automatische**

8.  Voltooi de **beheerdersreferenties** sectie als volgt:

   * **Admin Username** – Geef de gebruikersnaam van het systeemaccount van Workday-integratie met de naam van de tenant-domein toegevoegd. Moet als volgt uitzien:username@contoso4

   * **Beheerderswachtwoord –** Geef het wachtwoord van het systeem-account van Workday-integratie

   * **Tenant-URL:** voert u de URL naar het Workday web services-eindpunt voor uw tenant. Dit moet eruitzien als: https://wd3-impl-services1.workday.com/ccx/service/contoso4, waarbij contoso4 is vervangen door de juiste tenantnaam en wd3 impl is vervangen door de juiste omgevingstekenreeks. Als deze URL is niet bekend, neem contact op met uw Workday-integratie partner of ondersteuning vertegenwoordiger om te bepalen van de juiste URL moet worden gebruikt.

   * **E-mailmelding –** Voer uw e-mailadres in en schakel het selectievakje 'e-mailbericht verzenden als een fout optreedt'.

   * Klik op de **testverbinding** knop.

   * Als de verbindingstest is geslaagd, klikt u op de **opslaan** bovenaan op de knop. Als dit mislukt, Controleer of de URL voor Workday en de referenties geldig zijn in Workday zijn.


### <a name="part-2-configure-attribute-mappings"></a>Deel 2: Kenmerktoewijzingen configureren 

In deze sectie configureert u hoe de gebruikersgegevens voor gebruikers alleen in de cloud uit Workday wordt loopt met Azure Active Directory.

1.  Op het tabblad inrichten onder **toewijzingen**, klikt u op **werknemers synchroniseren naar Azure AD**.

2.   In de **bron objectbereik** veld, kunt u selecteren welke groepen gebruikers in Workday moet binnen het bereik van het inrichten van Azure AD, door het definiëren van een set van filters op basis van kenmerken. Het standaardbereik is 'alle gebruikers in Workday'. Voorbeeld van de filters:

   * Voorbeeld: Scope aan gebruikers met werknemer-id's tussen 1000000 en 2000000

      * Kenmerk: WorkerID

      * Operator: REGEX-overeenkomst

      * Waarde: (1[0-9][0-9][0-9][0-9][0-9][0-9])

   * Voorbeeld: Alleen contingent werknemers en geen vaste medewerkers

      * Kenmerk: ContingentID

      * Operator: Niet NULL IS

3.  In de **doel Objectacties** veld globaal kunt u filteren welke acties moeten worden uitgevoerd op Azure AD zijn toegestaan. **Maak** en **Update** meest voorkomende zijn.

4.  In de **kenmerktoewijzingen** sectie, kunt u definiëren hoe afzonderlijke Workday kenmerken worden toegewezen aan Active Directory-kenmerken.

5. Klik op een bestaande kenmerktoewijzing bij te werken of klik op **toevoegen van nieuwe toewijzing** aan de onderkant van het scherm om toe te voegen nieuwe toewijzingen. De toewijzing van een individueel kenmerk ondersteunt deze eigenschappen:

   * **Toewijzingstype**

      * **Directe** : de waarde van het kenmerk Workday schrijft naar het kenmerk AD zonder wijzigingen

      * **Constante** -schrijven van een statische, constante string-waarde naar het AD-kenmerk

      * **Expressie** – kunt u een aangepaste waarde schrijven naar het kenmerk AD op basis van een of meer kenmerken van Workday. [Zie voor meer informatie in dit artikel op expressies](active-directory-saas-writing-expressions-for-attribute-mappings.md).

   * **Bronkenmerk** -gebruikerskenmerk uit Workday. Als het kenmerk dat u zoekt niet aanwezig is, Zie [aanpassen van de lijst met gebruikerskenmerken Workday](#customizing-the-list-of-workday-user-attributes).

   * **Standaardwaarde** : optioneel. Als het bronkenmerk een lege waarde heeft, wordt de toewijzing in plaats daarvan deze waarde schrijven.
            De configuratie van de meest voorkomende is leeg laten.

   * **Doelkenmerk** – het gebruikerskenmerk in Azure AD.

   * **Overeen met objecten met behulp van dit kenmerk** : of deze toewijzing moet worden gebruikt als unieke identificatie van gebruikers tussen Workday en Azure AD of niet. Deze wordt meestal ingesteld in het veld ID van de werknemer voor Workday die normaal gesproken is toegewezen aan de werknemer-ID-kenmerk (nieuw) of een kenmerk toestelnummer in Azure AD.

   * **Overeenkomende voorrang** – meerdere overeenkomende kenmerken kan worden ingesteld. Wanneer er meerdere, moeten ze worden geëvalueerd in de volgorde gedefinieerd door dit veld. Als een overeenkomst is gevonden, er is geen verdere overeenkomende kenmerken worden geëvalueerd.

   * **Deze toewijzing is van toepassing**

     * **Altijd** : deze toewijzing is van toepassing op beide maken van een gebruikersaccount en acties bijwerken

     * **Alleen tijdens het maken van** -deze toewijzing is van toepassing alleen op gebruikersacties maken

6. Als u wilt uw toewijzingen opslaan, klikt u op **opslaan** boven aan de sectie kenmerk toewijzen.

### <a name="part-3-start-the-service"></a>Deel 3: Start de service
Zodra de onderdelen 1-2 zijn voltooid, kunt u de inrichting service starten.

1.  In de **inrichten** tabblad, stelt u de **inrichting Status** naar **op**.

2. Klik op **Opslaan**.

3. Hiermee start u de eerste synchronisatie, waarmee een variabele aantal uren, afhankelijk van hoeveel gebruikers in Workday zijn kan duren.

4. Afzonderlijke sync gebeurtenissen kunnen worden weergegeven de **controlelogboeken** tabblad. **[Raadpleeg de inrichting reporting guide voor gedetailleerde instructies voor het lezen van de controlelogboeken](active-directory-saas-provisioning-reporting.md)**

5. Een voltooid, wordt er een overzichtsrapport van de audit schrijven in de **inrichten** tabblad, zoals hieronder wordt weergegeven.


## <a name="configuring-writeback-of-email-addresses-to-workday"></a>Write-back van e-mailadressen aan Workday configureren
Volg deze instructies voor het configureren van Write-back van gebruiker e-mailadressen van Azure Active Directory naar werkdag.

### <a name="part-1-adding-the-provisioning-connector-app-and-creating-the-connection-to-workday"></a>Deel 1: De inrichting connector app toe te voegen en het maken van de verbinding met Workday

**Werkdag configureren voor het inrichten van Active Directory:**

1.  Ga naar <https://portal.azure.com>

2.  Selecteer in de linkernavigatiebalk **Azure Active Directory**

3.  Selecteer **bedrijfstoepassingen**, klikt u vervolgens **alle toepassingen**.

4.  Selecteer **een toepassing toevoegen**, selecteer vervolgens de **alle** categorie.

5.  Zoeken naar **Write-back van Workday**, en die app uit de galerie toevoegt.

6.  Nadat de app is toegevoegd en het scherm app-informatie weergegeven, selecteert wordt **inrichten**

7.  Wijzig de **inrichting** **modus** naar **automatische**

8.  Voltooi de **beheerdersreferenties** sectie als volgt:

   * **Admin Username** – Geef de gebruikersnaam van het systeemaccount van Workday-integratie met de naam van de tenant-domein toegevoegd. Moet als volgt uitzien:username@contoso4

   * **Beheerderswachtwoord –** Geef het wachtwoord van het systeem-account van Workday-integratie

   * **Tenant-URL:** voert u de URL naar het Workday web services-eindpunt voor uw tenant. Dit moet eruitzien als: https://wd3-impl-services1.workday.com/ccx/service/contoso4, waarbij contoso4 is vervangen door de juiste tenantnaam en wd3 impl is vervangen door de juiste omgevingstekenreeks (indien nodig).

   * **E-mailmelding –** Voer uw e-mailadres in en schakel het selectievakje 'e-mailbericht verzenden als een fout optreedt'.

   * Klik op de **testverbinding** knop. Als de verbindingstest is geslaagd, klikt u op de **opslaan** bovenaan op de knop. Als dit mislukt, Controleer of de URL voor Workday en de referenties geldig zijn in Workday zijn.


### <a name="part-2-configure-attribute-mappings"></a>Deel 2: Kenmerktoewijzingen configureren 


In deze sectie configureert u hoe gebruikersgegevens uit Workday loopt naar Active Directory.

1.  Op het tabblad inrichten onder **toewijzingen**, klikt u op **synchroniseren Azure AD-gebruikers Workday**.

2.  In de **bron objectbereik** veld, kunt u eventueel filteren welke groepen gebruikers in Azure Active Directory moeten hebben hun e-mailadressen teruggeschreven naar werkdag. Het standaardbereik is 'alle gebruikers in Azure AD'. 

3.  In de **kenmerktoewijzingen** sectie, kunt u definiëren hoe afzonderlijke Workday kenmerken worden toegewezen aan Active Directory-kenmerken. Er is een toewijzing voor het e-mailadres standaard. De overeenkomende ID moet echter worden bijgewerkt zodat deze overeenkomen met gebruikers in Azure AD met hun overeenkomstige vermeldingen in Workday. Een populaire overeenkomende methode is om te synchroniseren van de werkdag werknemer-ID of ID van de werknemer op extensionAttribute1 15 in Azure AD en vervolgens dit kenmerk in Azure AD gebruiken om gebruikers te vergelijken terug in Workday.

4.  Als u wilt uw toewijzingen opslaan, klikt u op **opslaan** boven aan de sectie kenmerk toewijzen.

### <a name="part-3-start-the-service"></a>Deel 3: Start de service
Zodra de onderdelen 1-2 zijn voltooid, kunt u de inrichting service starten.

1.  In de **inrichten** tabblad, stelt u de **inrichting Status** naar **op**.

2. Klik op **Opslaan**.

3. Hiermee start u de eerste synchronisatie, waarmee een variabele aantal uren, afhankelijk van hoeveel gebruikers in Workday zijn kan duren.

4. Afzonderlijke sync gebeurtenissen kunnen worden weergegeven de **controlelogboeken** tabblad. **[Raadpleeg de inrichting reporting guide voor gedetailleerde instructies voor het lezen van de controlelogboeken](active-directory-saas-provisioning-reporting.md)**

5. Een voltooid, wordt er een overzichtsrapport van de audit schrijven in de **inrichten** tabblad, zoals hieronder wordt weergegeven.


## <a name="customizing-the-list-of-workday-user-attributes"></a>De lijst met gebruikerskenmerken Workday aanpassen
De inrichting van apps voor Active Directory en Azure AD zowel een standaardlijst met gebruikerskenmerken Workday opnemen werkdag u kunt kiezen uit. Deze lijsten zijn echter niet uitgebreid. Werkdag ondersteunt honderden mogelijke gebruikerskenmerken, die kunnen standaard of uniek is voor uw Workday-tenant zijn. 

De Azure AD-service inricht ondersteunt de mogelijkheid voor het aanpassen van de lijst of Workday-kenmerk op alle kenmerken die worden weergegeven in de [Get_Workers](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html) bewerking van de API Human Resources.

Om dit te doen, moet u [Workday Studio](https://community.workday.com/studio-download) uitpakken van de XPath-expressies die staan voor de kenmerken die u wilt gebruiken en deze vervolgens toevoegen aan uw provisioning-configuratie met behulp van de geavanceerde kenmerkeditor in de Azure portal.

**Voor het ophalen van een XPath-expressie voor een gebruikerskenmerk Workday:**

1. Download en installeer [Workday Studio](https://community.workday.com/studio-download). U moet een werkdag community-account voor toegang tot het installatieprogramma.

2. Download het bestand Workday Human_Resources WDSL vanaf deze URL: https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Human_Resources.wsdl

3. Werkdag Studio starten.

4. Selecteer in de opdrachtbalk de **Workday > Test-webservice in Tester** optie.

5. Selecteer **externe**, en selecteert u het gedownloade Human_Resources WSDL-bestand in stap 2.

    ![Workday Studio](./media/active-directory-saas-workday-inbound-tutorial/WDstudio1.PNG)

6. Stel de **locatie** veld `https://IMPL-CC.workday.com/ccx/service/TENANT/Human_Resources`, maar "IMPL CC" vervangen door uw werkelijke type instantie en 'TENANT' met de tenantnaam van uw echte.

7. Stel **bewerking** naar **Get_Workers**

8.  Klik op de kleine **configureren** koppeling hieronder de deelvensters aanvragen/reacties uw Workday-referenties in te stellen. Controleer **verificatie**, en voer vervolgens de gebruikersnaam en het wachtwoord voor het systeem-account van uw Workday-integratie. Zorg ervoor dat de naam van de gebruiker als indeling name@tenant, en laat de **WS-Security UsernameToken** optie is geselecteerd.

    ![Workday Studio](./media/active-directory-saas-workday-inbound-tutorial/WDstudio2.PNG)

9. Selecteer **OK**.

10. De **aanvragen** deelvenster plakken in de XML-code hieronder en stel **werknemer** naar de werknemer-ID van een echte gebruiker in uw Workday-tenant. Selecteer een gebruiker die het kenmerk heeft ingevuld die u wilt uitpakken.

    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <env:Envelope xmlns:env="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
      <env:Body>
        <wd:Get_Workers_Request xmlns:wd="urn:com.workday/bsvc" wd:version="v21.1">
          <wd:Request_References wd:Skip_Non_Existing_Instances="true">
            <wd:Worker_Reference>
              <wd:ID wd:type="Employee_ID">21008</wd:ID>
            </wd:Worker_Reference>
          </wd:Request_References>
          <wd:Response_Group>
            <wd:Include_Reference>true</wd:Include_Reference>
            <wd:Include_Personal_Information>true</wd:Include_Personal_Information>
            <wd:Include_Employment_Information>true</wd:Include_Employment_Information>
            <wd:Include_Management_Chain_Data>true</wd:Include_Management_Chain_Data>
            <wd:Include_Organizations>true</wd:Include_Organizations>
            <wd:Include_Reference>true</wd:Include_Reference>
            <wd:Include_Transaction_Log_Data>true</wd:Include_Transaction_Log_Data>
            <wd:Include_Photo>true</wd:Include_Photo>
            <wd:Include_User_Account>true</wd:Include_User_Account>
          </wd:Response_Group>
        </wd:Get_Workers_Request>
      </env:Body>
    </env:Envelope>
    ```
 
11. Klik op de **aanvraag verzenden** (groene pijl) de opdracht uit te voeren. Als geslaagd, het antwoord moet worden weergegeven in de **antwoord** deelvenster. Controleer het antwoord om te controleren of dat de gegevens van de ingevoerde gebruikersnaam heeft en niet een fout opgetreden.

12. Als dit lukt, kopieert u de XML van de **antwoord** deelvenster en als XML-bestand op te slaan.

13. Selecteer in de opdracht balk van Workday Studio **bestand > bestand openen...**  en open het XML-bestand dat u zojuist hebt opgeslagen. Dit geopend in de Workday Studio XML-editor.

    ![Workday Studio](./media/active-directory-saas-workday-inbound-tutorial/WDstudio3.PNG)

14. Navigeer in de bestandsstructuur van het via **/env:Envelope > env > wd:Get_Workers_Response > wd:Response_Data > wd:Worker** om gegevens van uw gebruikers te zoeken. 

15. Onder **wd:Worker**vinden van het kenmerk dat u wilt toevoegen en selecteer deze.

16. Kopieer de XPath-expressie voor het geselecteerde kenmerk buiten de **documentpad** veld.

17. Verwijder de **/env:Envelope / env / wd:Get_Workers_Response / wd:Response_Data /** voorvoegsel van de gekopieerde expressie. 

18. Als het laatste item in de gekopieerde expressie is van een knooppunt (voorbeeld: "/ wd:Birth_Date '), Voeg **/text()** aan het einde van de expressie. Dit is niet nodig als een kenmerk is het laatste item (voorbeeld: '/@wd:type').

19. Het resultaat moet ongeveer `wd:Worker/wd:Worker_Data/wd:Personal_Data/wd:Birth_Date/text()`. Dit is wat u kopieert naar de Azure-portal.


**Uw Workday-kenmerk voor aangepaste gebruiker toevoegen aan uw provisioning-configuratie:**

1. Start de [Azure-portal](https://portal.azure.com), en navigeer naar de sectie inrichten van uw werkdag inrichting van toepassing, zoals eerder in deze zelfstudie wordt beschreven.

2. Ingesteld **inrichting Status** naar **uit**, en selecteer **opslaan**. Dit helpt ervoor te zorgen dat uw wijzigingen van kracht alleen wanneer u klaar bent.

3. Onder **toewijzingen**, selecteer **werknemers synchroniseren naar OnPremises** (of **werknemers synchroniseren naar Azure AD**).

4. Ga naar de onderkant van het volgende scherm en selecteer **geavanceerde opties weergeven**.

5. Selecteer **kenmerk controlelijst voor Workday**.

    ![Workday Studio](./media/active-directory-saas-workday-inbound-tutorial/WDstudio_AAD1.PNG)

6. Ga naar de onderkant van de lijst met kenmerken aan waarvoor de invoer velden zijn.

7. Voor **naam**, voer een weergavenaam voor het kenmerk.

8. Voor **Type**, selecteren dat op de juiste wijze komt met het kenmerk overeen (**tekenreeks** is gebruikelijk).

9. Voor **API expressie**, geef de XPath-expressie die u hebt gekopieerd uit Workday Studio. Voorbeeld:`wd:Worker/wd:Worker_Data/wd:Personal_Data/wd:Birth_Date/text()`

10. Selecteer **kenmerk toevoegen**.

    ![Workday Studio](./media/active-directory-saas-workday-inbound-tutorial/WDstudio_AAD2.PNG)

11. Selecteer **opslaan** hierboven, en vervolgens **Ja** naar het dialoogvenster. Sluit het scherm kenmerk toewijzen als deze nog steeds geopend.

12. Terug op de hoofdblade **inrichten** tabblad **werknemers synchroniseren naar OnPremises** (of **werknemers synchroniseren naar Azure AD**) opnieuw.

13. Selecteer **toevoegen van nieuwe toewijzing**.

14. Het nieuwe kenmerk wordt nu weergegeven in de **bronkenmerk** lijst.

15. Een toewijzing voor het nieuwe kenmerk naar wens toevoegen.

16. Als u klaar bent denken bij het **inrichting Status** terug naar **op** en op te slaan.


## <a name="known-issues"></a>Bekende problemen

* Bij het uitvoeren van de **toevoegen ADSyncAgentAzureActiveDirectoryConfiguration** Powershell-opdracht, er is momenteel een bekend probleem met de hoofdbeheerdersreferenties werkt niet als ze met een aangepast domein (voorbeeld: admin@contoso.com) . Als tijdelijke oplossing maken en gebruiken van een globale beheerdersaccount in Azure AD met een onmicrosoft.com-domein (voorbeeld: admin@contoso.onmicrosoft.com).

* Een vorige probleem met de controlelogboeken verschijnt niet in Azure AD-tenants die zich in de Europese Unie is opgelost. Aanvullende agentconfiguratie is echter vereist voor Azure AD-tenants in de EU. Zie voor meer informatie [deel 3: de lokale synchronisatie-agent configureren](#Part 3: Configure the on-premises synchronization agent)


## <a name="additional-resources"></a>Aanvullende resources
* [Zelfstudie: Eenmalige aanmelding tussen Workday en Azure Active Directory configureren](active-directory-saas-workday-tutorial.md)
* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Volgende stappen

* [Informatie over het bekijken van Logboeken en rapporten over het inrichten van de activiteit ophalen](https://docs.microsoft.com/azure/active-directory/active-directory-saas-provisioning-reporting)
