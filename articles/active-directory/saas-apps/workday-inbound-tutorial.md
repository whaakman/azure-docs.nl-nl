---
title: 'Zelfstudie: Workday voor het automatisch gebruikers inrichten met Azure Active Directory configureren | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en inrichting ongedaan maken-gebruikersaccounts met Workday.
services: active-directory
author: asmalser-msft
documentationcenter: na
manager: mtillman
ms.assetid: 1a2c375a-1bb1-4a61-8115-5a69972c6ad6
ms.service: active-directory
ms.component: saas-app-tutorial
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/18/2018
ms.author: asmalser
ms.openlocfilehash: 930ca49a63e34214ec197d8dd37f38361b34fe90
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44347032"
---
# <a name="tutorial-configure-workday-for-automatic-user-provisioning-preview"></a>Zelfstudie: Configureren van Workday voor automatisch gebruikers inrichten (preview)

Het doel van deze zelfstudie is om weer te geven u de stappen die u uitvoeren wilt voor het importeren van mensen uit Workday in Active Directory en Azure Active Directory, met optionele Write-back van enkele kenmerken naar Workday.

## <a name="overview"></a>Overzicht

De [Azure Active Directory-gebruiker inrichtingsservice](../manage-apps/user-provisioning.md) kan worden geïntegreerd met de [Workday Human Resources API](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html) om het inrichten van gebruikersaccounts. Azure AD maakt gebruik van deze verbinding om in te schakelen van de volgende gebruiker inrichten van werkstromen:

* **Inrichten van gebruikers voor Active Directory** -geselecteerde sets van gebruikers in Workday synchroniseren naar een of meer Active Directory-forests.

* **Inrichten van gebruikers alleen in de cloud met Azure Active Directory** - In scenario's waarbij on-premises Active Directory niet gebruikt wordt, gebruikers rechtstreeks vanuit Workday kunnen worden ingericht op Azure Active Directory met de Azure AD-gebruiker-service inricht. 

* **Write-back van e-adressen aan Workday** -de Azure AD-gebruiker inrichtingsservice kunt terugschrijven van het e-mailadressen van Azure AD-gebruikers naar Workday. 

### <a name="what-human-resources-scenarios-does-it-cover"></a>Welke human resources-scenario's onder deze wet?

De Workday gebruiker inrichting werkstromen die worden ondersteund door de inrichtingsservice voor Azure AD-gebruiker inschakelen van de volgende human resources en scenario's voor het beheer van identiteit lifecycle-automatisering:

* **Nieuwe werknemers aanstellen** : wanneer een nieuwe werknemer wordt toegevoegd aan de werkdag, een gebruikersaccount wordt automatisch gemaakt in Active Directory, Azure Active Directory en eventueel Office 365 en [andere SaaS-toepassingen ondersteund door Azure AD](../manage-apps/user-provisioning.md), met terugschrijven van het e-mailadres Workday.

* **Updates voor het kenmerk en het profiel van werknemer** : wanneer een werknemerrecord wordt bijgewerkt in Workday (zoals de naam, titel of manager), hun gebruikersaccount, automatisch worden bijgewerkt in Active Directory, Azure Active Directory en eventueel Office 365 en [andere SaaS-toepassingen die worden ondersteund door Azure AD](../manage-apps/user-provisioning.md).

* **Werknemer afsluitingen** : wanneer een werknemer is beëindigd in Workday, hun gebruikersaccount is automatisch uitgeschakeld in Active Directory, Azure Active Directory en eventueel Office 365 en [andere SaaS-toepassingen die door Azure worden ondersteund AD](../manage-apps/user-provisioning.md).

* **Werknemer opnieuw hires** : wanneer een werknemer is rehired in Workday, hun oude account kan worden automatisch geactiveerd of opnieuw ingericht (afhankelijk van uw voorkeur) naar Active Directory, Azure Active Directory, en eventueel Office 365 en [andere SaaS-toepassingen die worden ondersteund door Azure AD](../manage-apps/user-provisioning.md).

### <a name="who-is-this-user-provisioning-solution-best-suited-for"></a>Wie is deze gebruiker inrichting oplossing het beste geschikt is voor?

Deze oplossing voor gebruikersinrichting van Workday bevindt zich momenteel in openbare preview en is ideaal voor:

* Organisaties die behoefte hebben aan een vooraf gemaakte, cloud-gebaseerde oplossing voor het inrichten van Workday-gebruikers

* Organisaties waarvoor directe gebruikersinrichting van Workday naar Active Directory of Azure Active Directory

* Organisaties die vereisen dat gebruikers worden ingericht met behulp van gegevens die zijn verkregen van de werkdag HCM-module (Zie [Get_Workers](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html))

* Organisaties die lid wordt, verplaatst, vereisen en waardoor gebruikers kunnen worden gesynchroniseerd met een of meer Active Directory-Forests, domeinen en OE's alleen op basis van de gegevens gedetecteerd in de module Workday HCM wijzigen (Zie [Get_Workers](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html))

* Organisaties met behulp van Office 365 voor e-mail

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-hybrid-note.md)]

## <a name="planning-your-solution"></a>Plannen van uw oplossing

Controleer de onderstaande vereisten voordat u begint met uw Workday-integratie, en lees de volgende richtlijnen over het afstemmen op uw huidige Active Directory-architectuur en gebruikers vereisten inrichten met de oplossing(en) geleverd door Azure Active Directory.

### <a name="prerequisites"></a>Vereiste onderdelen

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

* Een geldige Azure AD Premium P1-abonnement met de globale beheerderstoegang
* Een tenant van de implementatie van Workday voor test-en integratie
* Beheerdersmachtigingen in Workday een system integration-gebruiker maken en hierin wijzigingen voor het testen van gegevens van werknemers voor testdoeleinden
* Voor het inrichten van gebruikers met Active Directory, een domein-server met Windows-Service 2012 of hoger is vereist op de host de [on-premises synchronisatieagent](https://go.microsoft.com/fwlink/?linkid=847801)
* [Azure AD Connect](../connect/active-directory-aadconnect.md) voor de synchronisatie tussen Active Directory en Azure AD

### <a name="solution-architecture"></a>Oplossingsarchitectuur

Azure AD biedt een grote verscheidenheid aan connectors om u te helpen bij het oplossen van inrichting en identiteit levensduurbeheer van Workday naar Active Directory, Azure AD, SaaS-apps en andere cloudservices inrichten. Onderdelen die u wilt gebruiken en hoe u de oplossing hebt ingesteld, hangt af van de omgeving en vereisten van uw organisatie. Als een eerste stap rijtje hoeveel van de volgende aanwezig en geïmplementeerd in uw organisatie zijn:

* Het aantal Active Directory-Forests worden gebruikt?
* Het aantal Active Directory-domeinen worden gebruikt?
* Het aantal Active Directory organisatie-eenheden (OU's) worden gebruikt?
* Hoeveel Azure Active Directory-tenants worden gebruikt?
* Zijn er gebruikers die moeten worden ingericht voor Active Directory en Azure Active Directory (bijvoorbeeld 'hybride' gebruikers)?
* Zijn er gebruikers die moeten worden ingericht op Azure Active Directory, maar niet in Active Directory (bijvoorbeeld 'cloud-only' gebruikers)?
* Moeten gebruiker e-mailadressen worden teruggeschreven naar Workday?

Zodra u antwoorden op deze vragen hebt, kunt u uw werkdag implementatie wordt ingericht door de onderstaande richtlijnen te volgen plannen.

#### <a name="using-provisioning-connector-apps"></a>Met behulp van de inrichting connector-apps

Azure Active Directory biedt ondersteuning voor vooraf geïntegreerde inrichting connectors voor Workday en een groot aantal andere SaaS-toepassingen.

Één inrichting connector interfaces met de API van een centraal bronsysteem en helpt u Inrichtingsgegevens op een systeem met één doel. De meeste inrichting connectors die ondersteuning biedt voor Azure AD zijn voor een enkele bron en doel-systeem (bijvoorbeeld Azure AD met ServiceNow), en kunnen worden ingesteld door de app toe te voegen in kwestie uit de galerie met Azure AD-app (bijvoorbeeld ServiceNow).

Er is een-op-een relatie tussen inrichting exemplaren van de connector en app-exemplaren in Azure AD:

| Bronsysteem | Doelsysteem |
| ---------- | ---------- |
| Azure AD-tenant | SaaS-toepassing |

Als u werkt met Workday en Active Directory, zijn er echter meerdere bron- en systemen te worden beschouwd:

| Bronsysteem | Doelsysteem | Opmerkingen |
| ---------- | ---------- | ---------- |
| WorkDay | Active Directory-forest | Elk forest wordt beschouwd als een afzonderlijke doelsysteem |
| WorkDay | Azure AD-tenant | Zoals is vereist voor cloudgebruikers |
| Active Directory-forest | Azure AD-tenant | Deze stroom wordt verwerkt door AAD Connect vandaag nog |
| Azure AD-tenant | WorkDay | Voor write-back van e-mailadressen |

Azure AD biedt het kader van deze meerdere werkstromen met meerdere bron- en systemen, meerdere inrichting apps van de connector die u uit de galerie met Azure AD-app kunt toevoegen:

![AAD-App-galerie](./media/workday-inbound-tutorial/WD_Gallery.PNG)

* **Voor het inrichten van Active Directory WorkDay** -deze app vereenvoudigt het uitvoeren van het inrichten van gebruikersaccounts uit Workday naar één Active Directory-forest. Als u meerdere forests hebt, kunt u één exemplaar van deze app uit de galerie met Azure AD-app voor elk Active Directory-forest dat u inrichten wilt op toevoegen.

* **WorkDay naar Azure AD-inrichting** : hoewel AAD Connect het hulpprogramma dat moet worden gebruikt is voor het synchroniseren van Active Directory: gebruikers met Azure Active Directory, deze app kunnen worden gebruikt voor het inrichten van gebruikers in Workday naar een enkel Azure alleen in de cloud vereenvoudigen Active Directory-tenant.

* **Write-back van WorkDay** -Write-back van gebruiker e-mailadressen uit Azure Active Directory naar Workday vereenvoudigt het uitvoeren van deze app.

> [!TIP]
> De reguliere 'Werkdag'-app wordt gebruikt voor het instellen van eenmalige aanmelding tussen Workday en Azure Active Directory. 

Over het instellen en configureren van deze speciale inrichting connector-apps is het onderwerp van de rest van deze zelfstudie. Welke apps u wilt configureren, is afhankelijk van de systemen die u inrichten, en het aantal Active Directory-Forests en Azure AD wilt-tenants in uw omgeving zijn.

![Overzicht](./media/workday-inbound-tutorial/WD_Overview.PNG)

## <a name="configure-a-system-integration-user-in-workday"></a>Een gebruiker van de integratie van systeem in Workday configureren
Een algemene vereiste voor alle Workday inrichting connectors is hiervoor referenties voor Workday-integratie systeemaccount van een verbinding maken met de Workday Human Resources-API. In deze sectie wordt beschreven hoe u een system integrator-account maken in Workday.

> [!NOTE]
> Het is mogelijk om deze procedure overslaan en gebruiken in plaats daarvan een globale beheerdersaccount van Workday als het systeemaccount van de integratie. Dit werkt prima voor demo's, maar wordt niet aanbevolen voor productie-implementaties.

### <a name="create-an-integration-system-user"></a>Maakt u een integratie van system-gebruiker

**Maakt een systeemgebruiker integratie:**

1. Meld u aan bij uw Workday-tenant met een administrator-account. In de **Workday Workbench**, voer gebruiker maken in het zoekvak en klik vervolgens op **maken integratie systeemgebruiker**.

    ![Gebruiker maken](./media/workday-inbound-tutorial/IC750979.png "gebruiker maken")
2. Voltooi de **integratie systeemgebruiker maken** taak door het opgeven van een gebruikersnaam en wachtwoord voor een nieuwe gebruiker van de integratie-systeem.  
 * Laat de **wachtwoord vereisen bij volgende aanmelding In** optie dit selectievakje uitschakelt, omdat deze gebruiker via een programma aanmelden.
 * Laat de **minuten voor de time-out sessie** met de standaardwaarde 0 en waarmee wordt voorkomen dat van de gebruiker sessies voortijdig een time-out opgetreden.

    ![Integratie-systeemgebruiker maken](./media/workday-inbound-tutorial/IC750980.png "Integration System-gebruiker maken")

### <a name="create-a-security-group"></a>Maak een beveiligingsgroep
U moet een onbeperkte integratie system security group maken en aan de gebruiker toewijzen.

**Een beveiligingsgroep maken:**

1. Voer security group maken in het zoekvak en klik vervolgens op **beveiligingsgroep maken**.

    ![Groep CreateSecurity](./media/workday-inbound-tutorial/IC750981.png "CreateSecurity groep")
2. Voltooi de **beveiligingsgroep maken** taak.  
3. Selecteer **integratie Systeembeveiligingsgroep (onbeperkt)** uit de **Type van de Tenants beveiligingsgroep** vervolgkeuzelijst.
4. Maak een beveiligingsgroep waarmee leden expliciet worden toegevoegd.

    ![Groep CreateSecurity](./media/workday-inbound-tutorial/IC750982.png "CreateSecurity groep")

### <a name="assign-the-integration-system-user-to-the-security-group"></a>De integratie van systeemgebruiker toewijzen aan de beveiligingsgroep

**De integratie van systeemgebruiker toewijzen:**

1. Bewerken-beveiligingsgroep invoeren in het zoekvak in en klik vervolgens op **beveiligingsgroep bewerken**.

    ![Beveiligingsgroep bewerken](./media/workday-inbound-tutorial/IC750983.png "beveiligingsgroep bewerken")
1. Zoek en selecteer de nieuwe beveiligingsgroep voor integratie met de naam.

    ![Beveiligingsgroep bewerken](./media/workday-inbound-tutorial/IC750984.png "beveiligingsgroep bewerken")
2. De nieuwe gebruiker van de integratie-systeem toevoegen aan de nieuwe beveiligingsgroep. 

    ![Systeembeveiligingsgroep](./media/workday-inbound-tutorial/IC750985.png "Systeembeveiligingsgroep")  

### <a name="configure-security-group-options"></a>Groep Beveiligingsopties configureren
In deze stap maakt u moet hiervoor toestemming domeinbeveiliging beleid voor de worker-gegevens aan de beveiligingsgroep.

**Groep Beveiligingsopties configureren:**

1. Voer **beveiligingsbeleid voor domein** in het zoekvak en klik vervolgens op de koppeling **domein beveiligingsbeleid voor functiegebied**.  

    ![Beveiligingsbeleid voor domein](./media/workday-inbound-tutorial/IC750986.png "beveiligingsbeleid voor domein")  
2. Zoeken naar systeem en selecteer de **System** functiegebied Systeembeheer.  Klik op **OK**.  

    ![Beveiligingsbeleid voor domein](./media/workday-inbound-tutorial/IC750987.png "beveiligingsbeleid voor domein")  
3. Vouw in de lijst met beveiligingsbeleid voor het systeem functioneel gebied, **beveiligingsbeheer** en selecteert u het beveiligingsbeleid voor domein **extern Account inrichten**.  

    ![Beveiligingsbeleid voor domein](./media/workday-inbound-tutorial/IC750988.png "beveiligingsbeleid voor domein")  
1. Klik op **machtigingen bewerken**, en klik op de **machtigingen bewerken** dialoogvenster pagina, de nieuwe beveiligingsgroep toevoegen aan de lijst met beveiligingsgroepen met **ophalen** en **plaatsen**  integratie machtigingen.

    ![Machtiging bewerken](./media/workday-inbound-tutorial/IC750989.png "machtiging bewerken")  

1. Herhaal stappen 1-4 hierboven voor elk van deze resterende beveiligingsbeleid:

| Bewerking | Beveiligingsbeleid voor domein |
| ---------- | ---------- | 
| Get- en Put | Werknemersgegevens: Openbare Worker rapporten |
| Get- en Put | Werknemersgegevens: Neem contact op met werkgegevens |
| Get | Werknemersgegevens: Alle functies |
| Get | Werknemersgegevens: Huidige bezetting van informatie |
| Get | Werknemersgegevens: Functie in werknemersprofiel |


### <a name="activate-security-policy-changes"></a>Wijzigingen in het beveiligingsbeleid activeren

**Wijzigingen in het beveiligingsbeleid activeren:**

1. Voer activeren in het zoekvak in en klik vervolgens op de koppeling **activeren in behandeling zijnde wijzigingen voor het domeinbeveiligingsbeleid**.

    ![Activeren](./media/workday-inbound-tutorial/IC750992.png "activeren") 
2. De taak activeren in behandeling zijnde wijzigingen voor het domeinbeveiligingsbeleid u eerst een opmerking invoeren voor controledoeleinden en klik vervolgens op **OK**. 

    ![In afwachting van beveiliging activeren](./media/workday-inbound-tutorial/IC750993.png "activeren in afwachting van beveiliging")  
1. De taak op het volgende scherm voltooien door het controleren van het selectievakje **bevestigen**, en klik vervolgens op **OK**.

    ![In afwachting van beveiliging activeren](./media/workday-inbound-tutorial/IC750994.png "activeren in afwachting van beveiliging")  

## <a name="configuring-user-provisioning-from-workday-to-active-directory"></a>Gebruikersinrichting van Workday naar Active Directory configureren
Volg deze instructies voor het inrichten van Workday voor elk Active Directory-forest die u nodig hebt met het inrichten van gebruikersaccount configureren.

### <a name="planning"></a>Planning

Voordat u configureert het inrichten van gebruikers aan een Active Directory-forest, moet u de volgende vragen beantwoorden. De antwoorden op deze vragen kunnen bepalen hoe uw bereikfilters en kenmerktoewijzingen moeten worden ingesteld. 

* **Wat gebruikers in Workday moeten worden ingericht op deze Active Directory-forest?**

   * *Voorbeeld: Gebruikers waar het kenmerk Workday 'Bedrijf' bevat de waarde 'Contoso' en het kenmerk "Worker_Type" "Normale"*

* **Hoe worden gebruikers gerouteerd naar de andere organisatie-eenheden (OU's)?**

   * *Voorbeeld: Gebruikers worden doorgestuurd naar de OE's die met een office-locatie overeenkomen, zoals gedefinieerd in de Workday "Gemeenteraad" en "Country_Region_Reference" kenmerken*

* **Hoe moeten de volgende kenmerken worden ingevuld in de Active Directory?**

   * Algemene naam (cn)
      * *Voorbeeld: Gebruik de waarde van Workday User_ID, zoals ingesteld door human resources*
      
   * Werknemer-ID (employeeId)
      * *Voorbeeld: Gebruik de waarde van Workday Worker_ID*
      
   * SAM-accountnaam (sAMAccountName)
      * *Voorbeeld: Gebruik de waarde van Workday User_ID, gefilterd via een Azure AD-expressie wordt ingericht om ongeldige tekens te verwijderen*
      
   * User Principal Name (userPrincipalName)
      * *Voorbeeld: Gebruik de waarde van Workday User_ID, met een Azure AD-inrichting expressie de domeinnaam van een toe te voegen*

* **Hoe gebruikers overeenkomt tussen Workday en Active Directory?**

  * *Voorbeeld: De gebruikers met een specifieke werkdag 'Worker_ID', waarde wordt voldaan met Active Directory-gebruikers waar "employeeID" dezelfde waarde heeft. Als de waarde Worker_ID niet in Active Directory gevonden is, maakt u een nieuwe gebruiker.*
  
* **Bevat het Active Directory-forest al de gebruikers-id voor de overeenkomende logica is vereist om te werken?**

  * *Voorbeeld: Als dit een nieuwe implementatie van de werkdag, het is raadzaam dat Active Directory ingevuld met de juiste Workday Worker_ID waarden (of de unieke id-waarde van keuze worden) de overeenkomende logica zo eenvoudig mogelijk houden.*
    
    
### <a name="part-1-adding-the-provisioning-connector-app-and-creating-the-connection-to-workday"></a>Deel 1: De inrichting connector-app toevoegen en het maken van de verbinding met Workday

**Workday configureren voor het inrichten van Active Directory:**

1.  Ga naar <https://portal.azure.com>

2.  Selecteer in de linker navigatiebalk **Azure Active Directory**

3.  Selecteer **bedrijfstoepassingen**, klikt u vervolgens **alle toepassingen**.

4.  Selecteer **toevoegen van een toepassing**, en selecteer de **alle** categorie.

5.  Zoeken naar **Workday Provisioning naar Active Directory**, en het toevoegen van de app uit de galerie.

6.  Nadat de app is toegevoegd en het detailscherm app weergegeven, selecteert wordt **inrichten**

7.  Wijzig de **inrichten** **modus** te **automatische**

8.  Voltooi de **beheerdersreferenties** sectie als volgt:

   * **Admin Username** – Geef de gebruikersnaam van het systeemaccount van Workday-integratie met de naam van de tenant-domein toegevoegd. **Ziet er ongeveer als volgt: username@contoso4**

   * **Beheerderswachtwoord –** voert u het wachtwoord van het systeem-account van de Workday-integratie

   * **Tenant-URL:** Voer de URL naar het Workday web services-eindpunt voor uw tenant. Dit moet er als volgt uitzien: https://wd3-impl-services1.workday.com/ccx/service/contoso4/Human_Resources, waarbij contoso4 wordt vervangen door de juiste tenantnaam en wd3 impl wordt vervangen door de juiste omgeving-tekenreeks.

   * **Active Directory-Forest -** de 'naam' van uw Active Directory-forest, zoals geretourneerd door de powershell-commandlet Get-ADForest. Dit is doorgaans een tekenreeks, zoals: *contoso.com*

   * **Active Directory-Container -** voert u de container-verbindingsreeks die alle gebruikers in uw AD-forest bevat. Voorbeeld: *organisatie-eenheid = standaardgebruikers, organisatie-eenheid = Users, DC = contoso, DC = test*

   * **E-mailmelding –** Voer uw e-mailadres in en schakel het selectievakje 'e-mail verzenden als een fout optreedt' in.

   * Klik op de **testverbinding** knop. Als de verbindingstest is geslaagd, klikt u op de **opslaan** bovenaan op de knop. Als dit mislukt, moet u controleren of de Workday-referenties geldig in Workday zijn. 

![Azure Portal](./media/workday-inbound-tutorial/WD_1.PNG)

### <a name="part-2-configure-attribute-mappings"></a>Deel 2: Kenmerktoewijzingen configureren 

In deze sectie configureert u hoe gegevens stromen van Workday naar Active Directory.

1.  Op het tabblad inrichten onder **toewijzingen**, klikt u op **Workday werknemers synchroniseren on**.

2.  In de **bereik van het bronobject** veld, kunt u selecteren welke groepen gebruikers in Workday moet binnen het bereik voor het inrichten naar AD, bevat een reeks van filters op basis van een kenmerk. Het bereik van standaard is 'alle gebruikers in Workday'. Voorbeeld van de filters:

   * Voorbeeld: Bereik aan gebruikers met werknemer-id's tussen 1000000 en 2000000

      * Kenmerk: WorkerID

      * Operator: REGEX-overeenkomst

      * Waarde: (1[0-9][0-9][0-9][0-9][0-9][0-9])

   * Voorbeeld: Alleen werknemers en niet voorwaardelijke werknemers 

      * Kenmerk: werknemer-id

      * De operator: IS niet null zijn

3.  In de **acties voor doelobject** veld wereldwijd kunt u filteren welke acties moeten worden uitgevoerd op Active Directory zijn toegestaan. **Maak** en **Update** komen het meest.

4.  In de **kenmerktoewijzingen** sectie, kunt u definiëren hoe afzonderlijke Workday kenmerken toewijzen aan Active Directory-kenmerken.

5. Klik op een bestaande kenmerktoewijzing bij te werken, of klik op **nieuwe toewijzing toevoegen** aan de onderkant van het scherm om toe te voegen nieuwe toewijzingen. Een afzonderlijke kenmerktoewijzing biedt ondersteuning voor deze eigenschappen:

      * **Toewijzingstype**

         * **Directe** : de waarde van het kenmerk Workday schrijft naar de AD-kenmerk, zonder wijzigingen

         * **Constante** -schrijven van een statisch, constante string-waarde naar het AD-kenmerk

         * **Expressie** – kunt u een aangepaste waarde schrijven naar de AD-kenmerk op basis van een of meer kenmerken van Workday. [Zie voor meer informatie in dit artikel voor expressies](../manage-apps/functions-for-customizing-application-data.md).

      * **Bronkenmerk** -het gebruikerskenmerk van de van Workday. Als het kenmerk dat u zoekt niet aanwezig is, raadpleegt u [aanpassen van de lijst met gebruikerskenmerken Workday](#customizing-the-list-of-workday-user-attributes).

      * **Standaardwaarde** -optioneel. Als het kenmerk van de gegevensbron een lege waarde heeft, wordt de toewijzing van deze waarde in plaats daarvan schrijven.
            De configuratie van de meest voorkomende is te laat dit veld leeg.

      * **Doelkenmerk** – het gebruikerskenmerk in Active Directory.

      * **Overeenkomen met de objecten met behulp van dit kenmerk** – ongeacht of deze toewijzing moet worden gebruikt voor het aanduiden van gebruikers tussen Workday en Active Directory of niet. Dit is meestal ingesteld in het veld werknemer-ID voor Workday, die meestal is toegewezen aan een van de werknemer-ID-kenmerken in Active Directory.

      * **Prioriteit bij** : meerdere overeenkomende kenmerken kan worden ingesteld. Wanneer er meerdere, worden deze geëvalueerd in de volgorde die is gedefinieerd in dit veld. Zodra een overeenkomst wordt gevonden, geen verdere overeenkomende kenmerken worden geëvalueerd.

      * **Deze toewijzing toepassen**
       
         * **Altijd** : deze toewijzing toepassen op beide gebruiker maken en bijwerken van acties

         * **Alleen tijdens het maken van** -deze toewijzing is van toepassing alleen op acties van het maken van de gebruiker

6. Om uw toewijzingen hebt opgeslagen, klikt u op **opslaan** aan de bovenkant van de kenmerktoewijzing sectie.

![Azure Portal](./media/workday-inbound-tutorial/WD_2.PNG)

**Hieronder worden enkele voorbeeld kenmerktoewijzingen tussen Workday en Active Directory, met enkele algemene expressies**

-   De expressie die is toegewezen aan het kenmerk parentDistinguishedName wordt gebruikt voor het inrichten van een gebruiker aan verschillende organisatie-eenheden op basis van een of meer Workday bronkenmerken. Hier in dit voorbeeld wordt gebruikers in verschillende organisatie-eenheden die zijn gebaseerd op welke plaats ze zich in.

-   Het kenmerk userPrincipalName in Active Directory wordt gegenereerd door het samenvoegen van de gebruikers-ID van Workday met een domeinachtervoegsel

-   [Er wordt documentatie over het schrijven van expressies hier](../manage-apps/functions-for-customizing-application-data.md). Dit bevat voorbeelden over het verwijderen van speciale tekens.

  
| WORKDAY KENMERK | ACTIVE DIRECTORY-KENMERK |  OVEREENKOMENDE ID? | MAKEN / BIJWERKEN |
| ---------- | ---------- | ---------- | ---------- |
| **WorkerID**  |  Werknemer-id | **Ja** | Geschreven alleen bij het maken |
| **UserID**    |  algemene naam    |   |   Geschreven alleen bij het maken |
| **Join (' @ ', [gebruikersnaam] 'contoso.com')**   | userPrincipalName     |     | Geschreven alleen bij het maken 
| **Vervang(Mid(Vervang(\[UserID\], , "(\[\\\\/\\\\\\\\\\\\\[\\\\\]\\\\:\\\\;\\\\|\\\\=\\\\,\\\\+\\\\\*\\\\?\\\\&lt;\\\\&gt;\])", , "", , ), 1, 20), , "([\\\\.)\*\$](file:///\\.)*$)", , "", , )**      |    sAMAccountName            |     |         Geschreven alleen bij het maken |
| **Switch (\[Active\],, "0", "True", "1")** |  accountDisabled      |     | Maken en bijwerken |
| **Voornaam**   | givenName       |     |    Maken en bijwerken |
| **LastName**   |   SN   |     |  Maken en bijwerken |
| **PreferredNameData**  |  displayName |     |   Maken en bijwerken |
| **Bedrijf**         | Bedrijf   |     |  Maken en bijwerken |
| **SupervisoryOrganization**  | Afdeling  |     |  Maken en bijwerken |
| **ManagerReference**   | beheerder  |     |  Maken en bijwerken |
| **BusinessTitle**   |  titel     |     |  Maken en bijwerken | 
| **AddressLineData**    |  streetAddress  |     |   Maken en bijwerken |
| **Gemeenteraad**   |   l   |     | Maken en bijwerken |
| **CountryReferenceTwoLetter**      |   CO |     |   Maken en bijwerken |
| **CountryReferenceTwoLetter**    |  c  |     |         Maken en bijwerken |
| **CountryRegionReference** |  St     |     | Maken en bijwerken |
| **WorkSpaceReference** | physicalDeliveryOfficeName    |     |  Maken en bijwerken |
| **PostalCode**  |   Postcode  |     | Maken en bijwerken |
| **PrimaryWorkTelephone**  |  telephoneNumber   |     | Maken en bijwerken |
| **Fax**      | facsimileTelephoneNumber     |     |    Maken en bijwerken |
| **Mobile**  |    mobiele       |     |       Maken en bijwerken |
| **LocalReference** |  preferredLanguage  |     |  Maken en bijwerken |                                               
| **Switch (\[gemeenteraad\], "organisatie-eenheid standaardgebruikers, OU = = Users, organisatie-eenheid = standaard, organisatie-eenheid locaties, DC = = contoso, DC = com", 'Rotterdam', ' organisatie-eenheid standaardgebruikers, OU = = Users, organisatie-eenheid Dallas, OU = locaties, DC = = contoso, DC = com ","Austin"," organisatie-eenheid standaardgebruikers, OU = = Users, organisatie-eenheid Austin, OU = locaties, DC = = contoso, DC = com ","Seattle"," organisatie-eenheid standaardgebruikers, OU = = Users, organisatie-eenheid Seattle, OU = locaties, DC = = contoso, DC = com ","Londen"," organisatie-eenheid standaardgebruikers, OU = = Users, organisatie-eenheid Londen, OU = locaties, DC = = contoso, DC = com ")**  | parentDistinguishedName     |     |  Maken en bijwerken |
  
### <a name="part-3-configure-the-on-premises-synchronization-agent"></a>Deel 3: De on-premises synchronisatieagent configureren

Als u wilt inrichten naar Active Directory on-premises, moet een agent worden geïnstalleerd op een server domein in de wens van Active Directory-forest. Domein-(of ondernemingsadministrator-) referenties vereist zijn om de procedure te voltooien.

**[U kunt hier de on-premises synchronisatieagent downloaden](https://go.microsoft.com/fwlink/?linkid=847801)**

Na de installatie van agent, voer de onderstaande Powershell-opdrachten om de agent voor uw omgeving te configureren.

**Opdracht #1**

> cd 'C:\Program Files\Microsoft Azure AD Connect Agent\Modules\AADSyncAgent inrichten' Agent\\Modules\\AADSyncAgent

> Import-Module "C:\Program Files\Microsoft Azure AD Connect inrichting Agent\Modules\AADSyncAgent\AADSyncAgent.psd1"

**Opdracht #2**

> Add-ADSyncAgentActiveDirectoryConfiguration

* Invoer: Voor de 'Naam van de map', voer de naam van de AD-Forest, zoals opgegeven in het onderdeel \#2
* Invoer: De gebruikersnaam van beheerder en het wachtwoord voor Active Directory-forest

>[!TIP]
> Als u ontvangt het foutbericht 'de relatie tussen het primaire domein en het vertrouwde domein is mislukt', is het omdat de lokale computer zich in een omgeving waar meerdere Active Directory-forests of domeinen zijn geconfigureerd, en ten minste één vertrouwensrelaties geconfigureerd relatie is een mislukte of niet operationeel. Los het probleem, Corrigeer of verwijder de relatie verbroken vertrouwensrelatie.

**Opdracht #3**

> Add-ADSyncAgentAzureActiveDirectoryConfiguration

* Invoer: De gebruikersnaam van de globale beheerder en het wachtwoord voor uw Azure AD-tenant

>[!IMPORTANT]
>Er is momenteel een bekend probleem met de hoofdbeheerdersreferenties werkt niet als ze een aangepast domein gebruiken (voorbeeld: admin@contoso.com). Als tijdelijke oplossing, maken en gebruiken van een globale beheerdersaccount met een.onmicrosoft.com-domein (voorbeeld: admin@contoso.onmicrosoft.com)

>[!IMPORTANT]
>Er is momenteel een bekend probleem met de hoofdbeheerdersreferenties werkt niet als ze beschikken over multi-factor authentication ingeschakeld. Als tijdelijke oplossing, moet u multi-factor authentication voor de globale beheerder uitschakelen.

**Opdracht #4**

> Get-AdSyncAgentProvisioningTasks

* Actie: Controleer de gegevens worden geretourneerd. Met deze opdracht wordt automatisch gedetecteerd Workday inrichting van apps in uw Azure AD-tenant. Voorbeelduitvoer:

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

> Start-AdSyncAgentSynchronization-automatische

**Opdracht #6**

> net stop aadsyncagent

**Opdracht #7**

> net start aadsyncagent

>[!TIP]
>Naast de 'net'-opdrachten in Powershell, de synchronisatie-agent-service kan ook worden gestart en gestopt met behulp van **Services.msc**. Als er fouten optreden bij het uitvoeren van de Powershell-opdrachten, zorg ervoor dat de **Microsoft Azure AD Connect inrichting Agent** wordt uitgevoerd in **Services.msc**.

![Services](./media/workday-inbound-tutorial/Services.png)  

**Aanvullende configuratie voor klanten in de Europese Unie**

Als uw Azure Active Directory-tenant in een van de EU-datacenters bevindt zich, volg daarna de onderstaande extra stappen.

1. Open **Services.msc**, en stop de **Microsoft Azure AD Connect inrichting Agent** service.
2. Ga naar de map van de agent-installatie (voorbeeld: C:\Program Files\Microsoft Azure AD Connect inrichting Agent).
3. Open **SyncAgnt.exe.config** in een teksteditor.
4. Vervang https://manage.hub.syncfabric.windowsazure.com/Management met **https://eu.manage.hub.syncfabric.windowsazure.com/Management**
5. Vervang https://provision.hub.syncfabric.windowsazure.com/Provisioning met **https://eu.provision.hub.syncfabric.windowsazure.com/Provisioning**
6. Sla de **SyncAgnt.exe.config** bestand.
7. Open **Services.msc**, en start de **Microsoft Azure AD Connect inrichting Agent** service.

**Agent oplossen**

De [Windows-gebeurtenislogboek](https://technet.microsoft.com/library/cc722404(v=ws.11).aspx) op de Windows Server-machine die als host fungeert voor de agent bevat gebeurtenissen voor alle bewerkingen die worden uitgevoerd door de agent. Deze gebeurtenissen weergeven:
    
1. Open **Eventvwr.msc**.
2. Selecteer **Windows Logboeken > toepassing**.
3. Alle gebeurtenissen vastgelegd onder de bron weergeven **AADSyncAgent**. 
4. Controleren op fouten en waarschuwingen.

Als er een machtigingsprobleem met de Active Directory of een Azure Active Directory-referenties in de Powershell-opdrachten is, ziet u een foutbericht zoals deze: 
    
![Gebeurtenislogboeken](./media/workday-inbound-tutorial/Windows_Event_Logs.png) 


### <a name="part-4-start-the-service"></a>Deel 4: De service starten
Zodra de onderdelen 1-3 zijn voltooid, kun u de inrichtingsservice in Azure portal.

1.  In de **Provisioning** tabblad, stelt u de **Inrichtingsstatus** naar **op**.

2. Klik op **Opslaan**.

3. Hiermee start u de initiële synchronisatie, wat een variabele aantal uren, afhankelijk van hoeveel gebruikers in Workday zijn kan duren.

4. Controleer op elk gewenst moment de **auditlogboeken** tabblad in de Azure portal om te zien welke acties de provisioning-service heeft uitgevoerd. De auditlogboeken worden alle afzonderlijke synchronisatie gebeurtenissen die worden uitgevoerd door de provisioning-service, zoals welke gebruikers worden gelezen uit Workday en vervolgens later toegevoegd of bijgewerkt naar Active Directory. **[Zie de handleiding over rapportering inrichting voor gedetailleerde instructies over het lezen van de auditlogboeken](../manage-apps/check-status-user-account-provisioning.md)**

1.  Controleer de [Windows-gebeurtenislogboek](https://technet.microsoft.com/library/cc722404(v=ws.11).aspx) op de Windows-Server die als host fungeert voor de agent voor nieuwe fouten of waarschuwingen. Deze gebeurtenissen kunnen worden bekeken door te starten **Eventvwr.msc** op de server en selecteer **Windows Logboeken > toepassing**. Alle berichten met betrekking tot inrichting worden geregistreerd onder de bron **AADSyncAgent**.

6. Een voltooid, wordt er een overzichtsrapport van de audit schrijven in de **Provisioning** tabblad, zoals hieronder wordt weergegeven.

![Azure Portal](./media/workday-inbound-tutorial/WD_3.PNG)


## <a name="configuring-user-provisioning-to-azure-active-directory"></a>Configuratie van gebruikers inrichten met Azure Active Directory
Hoe u inrichten met Azure Active Directory configureren afhankelijk van uw provisioning vereisten, zoals beschreven in de onderstaande tabel.

| Scenario | Oplossing |
| -------- | -------- |
| **Gebruikers moeten worden ingericht voor Active Directory en Azure AD** | Gebruik  **[AAD Connect](../connect/active-directory-aadconnect.md)** |
| **Gebruikers moeten alleen worden ingericht met Active Directory** | Gebruik  **[AAD Connect](../connect/active-directory-aadconnect.md)** |
| **Gebruikers moeten worden ingericht met alleen Azure AD (alleen voor de cloud)** | Gebruik de **Workday voor het inrichten van Azure Active Directory** -app in de app-galerie |

Zie voor instructies over het instellen van Azure AD Connect de [documentatie voor Azure AD Connect](../connect/active-directory-aadconnect.md).

De volgende secties beschrijven het instellen van een verbinding tussen Workday en Azure AD voor het inrichten van gebruikers alleen in de cloud.

> [!IMPORTANT]
> Volg de onderstaande procedure alleen als u alleen in de cloud-gebruikers die moeten worden ingericht met Azure AD hebben en niet on-premises Active Directory.

### <a name="part-1-adding-the-azure-ad-provisioning-connector-app-and-creating-the-connection-to-workday"></a>Deel 1: De Azure AD-app voor inrichting connector toevoegen en het maken van de verbinding met Workday

**Workday configureren voor het inrichten van Azure Active Directory voor gebruikers alleen in de cloud:**

1.  Ga naar <https://portal.azure.com>.

2.  Selecteer in de linker navigatiebalk **Azure Active Directory**

3.  Selecteer **bedrijfstoepassingen**, klikt u vervolgens **alle toepassingen**.

4.  Selecteer **toevoegen van een toepassing**, en selecteer vervolgens de **alle** categorie.

5.  Zoeken naar **Workday voor het inrichten van Azure AD**, en het toevoegen van de app uit de galerie.

6.  Nadat de app is toegevoegd en het detailscherm app weergegeven, selecteert wordt **inrichten**

7.  Wijzig de **inrichten** **modus** te **automatische**

8.  Voltooi de **beheerdersreferenties** sectie als volgt:

   * **Admin Username** – Geef de gebruikersnaam van het systeemaccount van Workday-integratie met de naam van de tenant-domein toegevoegd. Ziet er ongeveer als volgt: username@contoso4

   * **Beheerderswachtwoord –** voert u het wachtwoord van het systeem-account van de Workday-integratie

   * **Tenant-URL:** Voer de URL naar het Workday web services-eindpunt voor uw tenant. Dit moet er als volgt uitzien: https://wd3-impl-services1.workday.com/ccx/service/contoso4/Human_Resources, waarbij contoso4 wordt vervangen door de juiste tenantnaam en wd3 impl wordt vervangen door de juiste omgeving-tekenreeks. Als deze URL niet bekend is, neem contact op met uw Workday-integratie-partner of ondersteuning-vertegenwoordiger om te bepalen van de juiste URL te gebruiken.

   * **E-mailmelding –** Voer uw e-mailadres in en schakel het selectievakje 'e-mail verzenden als een fout optreedt' in.

   * Klik op de **testverbinding** knop.

   * Als de verbindingstest is geslaagd, klikt u op de **opslaan** bovenaan op de knop. Als dit mislukt, moet u controleren dat de URL voor Workday en referenties geldig in Workday zijn.

### <a name="part-2-configure-attribute-mappings"></a>Deel 2: Kenmerktoewijzingen configureren 

In deze sectie configureert u hoe de gebruikersgegevens wordt stromen van Workday naar Azure Active Directory voor cloudgebruikers.

1. Op het tabblad inrichten onder **toewijzingen**, klikt u op **werknemers synchroniseren naar Azure AD**.

2. In de **bereik van het bronobject** veld, kunt u selecteren welke groepen gebruikers in Workday moet binnen het bereik voor levering aan Azure AD bevat een reeks van filters op basis van een kenmerk. Het bereik van standaard is 'alle gebruikers in Workday'. Voorbeeld van de filters:

   * Voorbeeld: Bereik aan gebruikers met werknemer-id's tussen 1000000 en 2000000

      * Kenmerk: WorkerID

      * Operator: REGEX-overeenkomst

      * Waarde: (1[0-9][0-9][0-9][0-9][0-9][0-9])

   * Voorbeeld: Alleen voorwaardelijke werknemers en niet regelmatig werknemers

      * Kenmerk: ContingentID

      * De operator: IS niet null zijn

3. In de **acties voor doelobject** veld wereldwijd kunt u filteren welke acties moeten worden uitgevoerd op Azure AD zijn toegestaan. **Maak** en **Update** komen het meest.

4. In de **kenmerktoewijzingen** sectie, kunt u definiëren hoe afzonderlijke Workday kenmerken toewijzen aan Active Directory-kenmerken.

5. Klik op een bestaande kenmerktoewijzing bij te werken, of klik op **nieuwe toewijzing toevoegen** aan de onderkant van het scherm om toe te voegen nieuwe toewijzingen. Een afzonderlijke kenmerktoewijzing biedt ondersteuning voor deze eigenschappen:

   * **Toewijzingstype**

      * **Directe** : de waarde van het kenmerk Workday schrijft naar de AD-kenmerk, zonder wijzigingen

      * **Constante** -schrijven van een statisch, constante string-waarde naar het AD-kenmerk

      * **Expressie** – kunt u een aangepaste waarde schrijven naar de AD-kenmerk op basis van een of meer kenmerken van Workday. [Zie voor meer informatie in dit artikel voor expressies](../manage-apps/functions-for-customizing-application-data.md).

   * **Bronkenmerk** -het gebruikerskenmerk van de van Workday. Als het kenmerk dat u zoekt niet aanwezig is, raadpleegt u [aanpassen van de lijst met gebruikerskenmerken Workday](#customizing-the-list-of-workday-user-attributes).

   * **Standaardwaarde** -optioneel. Als het kenmerk van de gegevensbron een lege waarde heeft, wordt de toewijzing van deze waarde in plaats daarvan schrijven.
            De configuratie van de meest voorkomende is te laat dit veld leeg.

   * **Doelkenmerk** – het gebruikerskenmerk in Azure AD.

   * **Overeenkomen met de objecten met behulp van dit kenmerk** – ongeacht of deze toewijzing moet worden gebruikt voor het aanduiden van gebruikers tussen Workday en Azure AD of niet. Dit is meestal ingesteld in het veld werknemer-ID voor Workday, die meestal is toegewezen aan de werknemer-ID-kenmerk (nieuw) of een extensiekenmerk in Azure AD.

   * **Prioriteit bij** : meerdere overeenkomende kenmerken kan worden ingesteld. Wanneer er meerdere, worden deze geëvalueerd in de volgorde die is gedefinieerd in dit veld. Zodra een overeenkomst wordt gevonden, geen verdere overeenkomende kenmerken worden geëvalueerd.

   * **Deze toewijzing toepassen**

     * **Altijd** : deze toewijzing toepassen op beide gebruiker maken en bijwerken van acties

     * **Alleen tijdens het maken van** -deze toewijzing is van toepassing alleen op acties van het maken van de gebruiker

6. Om uw toewijzingen hebt opgeslagen, klikt u op **opslaan** aan de bovenkant van de kenmerktoewijzing sectie.

### <a name="part-3-start-the-service"></a>Deel 3: De service starten
Zodra de onderdelen 1-2 zijn voltooid, kunt u de provisioning-service starten.

1. In de **Provisioning** tabblad, stelt u de **Inrichtingsstatus** naar **op**.

2. Klik op **Opslaan**.

3. Hiermee start u de initiële synchronisatie, wat een variabele aantal uren, afhankelijk van hoeveel gebruikers in Workday zijn kan duren.

4. Afzonderlijke synchronisatie gebeurtenissen kunnen worden weergegeven in de **auditlogboeken** tabblad. **[Zie de handleiding over rapportering inrichting voor gedetailleerde instructies over het lezen van de auditlogboeken](../manage-apps/check-status-user-account-provisioning.md)**

5. Een voltooid, wordt er een overzichtsrapport van de audit schrijven in de **Provisioning** tabblad, zoals hieronder wordt weergegeven.

## <a name="configuring-writeback-of-email-addresses-to-workday"></a>Write-back van e-mailadressen naar Workday configureren
Volg deze instructies voor het configureren van Write-back van gebruiker e-mailadressen uit Azure Active Directory naar Workday.

### <a name="part-1-adding-the-provisioning-connector-app-and-creating-the-connection-to-workday"></a>Deel 1: De inrichting connector-app toevoegen en het maken van de verbinding met Workday

**Workday configureren voor het inrichten van Active Directory:**

1. Ga naar <https://portal.azure.com>

2. Selecteer in de linker navigatiebalk **Azure Active Directory**

3. Selecteer **bedrijfstoepassingen**, klikt u vervolgens **alle toepassingen**.

4. Selecteer **toevoegen van een toepassing**en selecteer vervolgens de **alle** categorie.

5. Zoeken naar **Write-back van Workday**, en het toevoegen van de app uit de galerie.

6. Nadat de app is toegevoegd en het detailscherm app weergegeven, selecteert wordt **inrichten**

7. Wijzig de **inrichten** **modus** te **automatische**

8. Voltooi de **beheerdersreferenties** sectie als volgt:

   * **Admin Username** – Geef de gebruikersnaam van het systeemaccount van Workday-integratie met de naam van de tenant-domein toegevoegd. Ziet er ongeveer als volgt: username@contoso4

   * **Beheerderswachtwoord –** voert u het wachtwoord van het systeem-account van de Workday-integratie

   * **Tenant-URL:** Voer de URL naar het Workday web services-eindpunt voor uw tenant. Dit moet er als volgt uitzien: https://wd3-impl-services1.workday.com/ccx/service/contoso4/Human_Resources, waarbij contoso4 wordt vervangen door de juiste tenantnaam en wd3 impl wordt vervangen door de juiste omgeving-tekenreeks (indien nodig).

   * **E-mailmelding –** Voer uw e-mailadres in en schakel het selectievakje 'e-mail verzenden als een fout optreedt' in.

   * Klik op de **testverbinding** knop. Als de verbindingstest is geslaagd, klikt u op de **opslaan** bovenaan op de knop. Als dit mislukt, moet u controleren dat de URL voor Workday en referenties geldig in Workday zijn.

### <a name="part-2-configure-attribute-mappings"></a>Deel 2: Kenmerktoewijzingen configureren 

In deze sectie configureert u hoe gegevens stromen van Workday naar Active Directory.

1. Op het tabblad inrichten onder **toewijzingen**, klikt u op **synchroniseert Azure AD-gebruikers Workday**.

2. In de **bereik van het bronobject** veld, kunt u desgewenst filteren welke groepen gebruikers in Azure Active Directory moeten hebben hun e-mailadressen teruggeschreven naar Workday. Het bereik van standaard is 'alle gebruikers in Azure AD'. 

3. In de **kenmerktoewijzingen** sectie, bij te werken van de overeenkomende ID om aan te geven het kenmerk in Azure Active Directory waar het Workday werknemer-ID of de werknemer-ID wordt opgeslagen. Een populaire overeenkomende methode is om de Workday werknemer-ID of de werknemer-ID aan extensionAttribute1 15 in Azure AD synchroniseren en vervolgens dit kenmerk in Azure AD gebruiken om gebruikers te vergelijken vorige in Workday. 

4. Om uw toewijzingen hebt opgeslagen, klikt u op **opslaan** aan de bovenkant van de kenmerktoewijzing sectie.

### <a name="part-3-start-the-service"></a>Deel 3: De service starten
Zodra de onderdelen 1-2 zijn voltooid, kunt u de provisioning-service starten.

1. In de **Provisioning** tabblad, stelt u de **Inrichtingsstatus** naar **op**.

2. Klik op **Opslaan**.

3. Hiermee start u de initiële synchronisatie, wat een variabele aantal uren, afhankelijk van hoeveel gebruikers in Workday zijn kan duren.

4. Afzonderlijke synchronisatie gebeurtenissen kunnen worden weergegeven in de **auditlogboeken** tabblad. **[Zie de handleiding over rapportering inrichting voor gedetailleerde instructies over het lezen van de auditlogboeken](../manage-apps/check-status-user-account-provisioning.md)**

5. Een voltooid, wordt er een overzichtsrapport van de audit schrijven in de **Provisioning** tabblad, zoals hieronder wordt weergegeven.

## <a name="customizing-the-list-of-workday-user-attributes"></a>De lijst met gebruikerskenmerken Workday aanpassen
De inrichting van apps voor Active Directory en Azure AD beide een lijst Workday gebruikerskenmerken bevatten Workday u kunt kiezen uit. Deze lijsten zijn echter niet volledig. WorkDay biedt ondersteuning voor vele honderden mogelijk gebruikerskenmerken, die standaard of uniek is voor uw Workday-tenant. 

De Azure AD-inrichtingsservice ondersteunt de mogelijkheid om aan te passen van de lijst of Workday-kenmerk om op te nemen van de kenmerken die beschikbaar zijn in de [Get_Workers](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html) bewerking van het Human Resources-API.

Om dit te doen, moet u [Workday Studio](https://community.workday.com/studio-download) om op te halen van de XPath-expressies die staan voor de kenmerken die u wilt gebruiken, en deze vervolgens toevoegen aan uw provisioning-configuratie met behulp van de geavanceerde kenmerkeditor in Azure portal.

**Een XPath-expressie voor een gebruikerskenmerk Workday ophalen:**

1. Download en installeer [Workday Studio](https://community.workday.com/studio-download). U moet een werkdag community-account voor toegang tot het installatieprogramma.

2. Download het bestand Workday Human_Resources WDSL vanaf deze URL: https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Human_Resources.wsdl

3. Workday Studio starten.

4. Selecteer in de opdrachtbalk de **Workday > Test Web Service in Tester** optie.

5. Selecteer **externe**, en selecteer het Human_Resources WSDL-bestand dat u hebt gedownload in stap 2.

    ![WorkDay Studio](./media/workday-inbound-tutorial/WDstudio1.PNG)

6. Stel de **locatie** veld `https://IMPL-CC.workday.com/ccx/service/TENANT/Human_Resources`, maar "IMPL-CC" vervangen door uw werkelijke type instantie en 'TENANT' met de naam van uw bestaande tenant.

7. Stel **bewerking** naar **Get_Workers**

8.  Klik op de kleine **configureren** koppeling hieronder de deelvensters aanvraag/antwoord om in te stellen van uw Workday-referenties. Controleer **verificatie**, en voer de gebruikersnaam en het wachtwoord voor het systeem-account van uw Workday-integratie. Zorg ervoor dat u de naam van de gebruiker als indeling name@tenant, en laat de **WS-Security UsernameToken** optie is geselecteerd.

    ![WorkDay Studio](./media/workday-inbound-tutorial/WDstudio2.PNG)

9. Selecteer **OK**.

10. De **aanvragen** deelvenster plakken in het XML-bestand op de onderstaande en stel **werknemer** naar de werknemer-ID van een echte gebruiker in uw Workday-tenant. Selecteer een gebruiker die het kenmerk heeft ingevuld die u wilt extraheren.

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
            <wd:Include_Roles>true</wd:Include_Roles>
          </wd:Response_Group>
        </wd:Get_Workers_Request>
      </env:Body>
    </env:Envelope>
    ```
 
11. Klik op de **aanvraag verzenden** (groene pijl) de opdracht uit te voeren. Als geslaagd, het antwoord moet worden weergegeven in de **antwoord** deelvenster. Controleer het antwoord om te controleren of er de gegevens van de gebruikers-ID die u hebt ingevoerd, en niet een fout.

12. Als dit lukt, kopieert u het XML-bestand van de **antwoord** deelvenster en sla deze op als een XML-bestand.

13. Selecteer in de opdracht balk van Workday Studio **bestand > bestand openen...**  en open het XML-bestand dat u hebt opgeslagen. Hiermee wordt deze geopend in de Workday Studio XML-editor.

    ![WorkDay Studio](./media/workday-inbound-tutorial/WDstudio3.PNG)

14. Navigeer in de bestandsstructuur van het via **/env: envelop > env: hoofdtekst > wd:Get_Workers_Response > wd:Response_Data > Word: Worker** om gegevens van uw gebruikers te vinden. 

15. Onder **Word: Worker**, zoek het kenmerk dat u wilt toevoegen en selecteer deze.

16. Kopieer de XPath-expressie voor het geselecteerde kenmerk van de **documentpad** veld.

1. Verwijder de **/env:Envelope / env:Body / wd:Get_Workers_Response / wd:Response_Data /** voorvoegsel van de gekopieerde expressie.

18. Als het laatste item in de gekopieerde expressie een knooppunt is (voorbeeld: "/ Word: Birth_Date"), Voeg **/text()** aan het einde van de expressie. Dit is niet nodig als het laatste item een kenmerk is (voorbeeld: "/@wd: type ').

19. Het resultaat moet er ongeveer als `wd:Worker/wd:Worker_Data/wd:Personal_Data/wd:Birth_Date/text()`. Dit is wat u kopieert naar de Azure-portal.


**Uw aangepaste Workday gebruikerskenmerk toevoegen aan uw provisioning-configuratie:**

1. Start de [Azure-portal](https://portal.azure.com), en navigeer naar de sectie inrichting van uw werkdag inrichting van toepassing, zoals eerder in deze zelfstudie wordt beschreven.

2. Stel **Inrichtingsstatus** te **uit**, en selecteer **opslaan**. Dit helpt ervoor te zorgen dat uw wijzigingen worden doorgevoerd alleen wanneer u klaar bent.

3. Onder **toewijzingen**, selecteer **werknemers synchroniseren on** (of **werknemers synchroniseren naar Azure AD**).

4. Ga naar de onderkant van het volgende scherm en selecteer **geavanceerde opties weergeven**.

5. Selecteer **kenmerkenlijst bewerken voor Workday**.

    ![WorkDay Studio](./media/workday-inbound-tutorial/WDstudio_AAD1.PNG)

6. Ga naar de onderkant van de lijst met kenmerken aan waar de invoervelden.

7. Voor **naam**, voer een weergavenaam voor het kenmerk.

8. Voor **Type**,-type selecteren dat op de juiste wijze komt met het kenmerk overeen (**tekenreeks** is gebruikelijk).

9. Voor **API-expressie**, geef de XPath-expressie die u hebt gekopieerd uit Workday Studio. Voorbeeld: `wd:Worker/wd:Worker_Data/wd:Personal_Data/wd:Birth_Date/text()`

10. Selecteer **kenmerk toevoegen**.

    ![WorkDay Studio](./media/workday-inbound-tutorial/WDstudio_AAD2.PNG)

11. Selecteer **opslaan** hierboven, en vervolgens **Ja** naar het dialoogvenster. Sluit het scherm kenmerktoewijzing als deze nog steeds geopend is.

12. Terug op de hoofdpagina **Provisioning** tabblad **werknemers synchroniseren on** (of **werknemers synchroniseren naar Azure AD**) opnieuw.

13. Selecteer **nieuwe toewijzing toevoegen**.

14. Het nieuwe kenmerk wordt nu weergegeven in de **bronkenmerk** lijst.

15. Een toewijzing voor het nieuwe kenmerk toevoegen naar wens.

16. Wanneer u klaar bent, moet u om in te stellen **Inrichtingsstatus** terug naar **op** en op te slaan.

## <a name="known-issues"></a>Bekende problemen

* Bij het uitvoeren van de **toevoegen ADSyncAgentAzureActiveDirectoryConfiguration** Powershell-opdracht, er is momenteel een bekend probleem met de hoofdbeheerdersreferenties werkt niet als ze een aangepast domein gebruiken (voorbeeld: admin@contoso.com) . Als tijdelijke oplossing, maken en gebruiken van een globale beheerdersaccount in Azure AD met een.onmicrosoft.com-domein (voorbeeld: admin@contoso.onmicrosoft.com).

* Schrijven van gegevens naar het kenmerk thumbnailphoto wordt door gebruiker in on-premises Active Directory is momenteel niet ondersteund.

* De 'Workday naar Azure AD'-connector is momenteel niet ondersteund op Azure AD-tenants waar AAD Connect is ingeschakeld.  

* Een vorig probleem met de logboeken voor controle niet wordt weergegeven in Azure AD-tenants die zich in de Europese Unie is opgelost. Aanvullende agentconfiguratie is echter vereist voor Azure AD-tenants in de EU. Zie voor meer informatie, [deel 3: de on-premises synchronisatieagent configureren](#Part 3: Configure the on-premises synchronization agent)

## <a name="managing-personal-data"></a>Persoonlijke gegevens beheren

De oplossing wordt ingericht voor Active Directory Workday vereist een synchronisatieagent moet worden geïnstalleerd op een domein-server en deze agent logboeken gemaakt in het Windows-gebeurtenislogboek die persoonlijk identificeerbare informatie kan bevatten.

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../manage-apps/check-status-user-account-provisioning.md)
* [Meer informatie over het configureren van eenmalige aanmelding tussen Workday en Azure Active Directory](workday-tutorial.md)
* [Meer informatie over andere SaaS-toepassingen integreren met Azure Active Directory](tutorial-list.md)

