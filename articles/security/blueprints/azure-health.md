---
title: Azure Health Analytics blauwdruk
description: Richtlijnen voor het implementeren van een blauwdruk HIPAA/HITRUST Health Analytics
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 26566e0a-0a54-49f4-a91d-48e20b7cef71
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/27/2018
ms.author: jomolesk
ms.openlocfilehash: f58466bb4cc90823d8e75e0371b400ee674e8b5d
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37113228"
---
# <a name="azure-security-and-compliance-blueprint---hipaahitrust-health-data-and-ai"></a>Azure-beveiliging en naleving blauwdruk - HIPAA/HITRUST Health Data en AI

## <a name="overview"></a>Overzicht

**De Azure-beveiliging en naleving blauwdruk - HIPAA/HITRUST Health Data en AI biedt een directe implementatie van een Azure PaaS-oplossing om u te laten zien hoe u veilig voor het opnemen, opslaan, analyseren en communiceren met statusgegevens terwijl deze kunnen voldoen aan de bedrijfstak van naleving vereisten. De blauwdruk helpt Versnel de acceptatie van cloudcomputing en -gebruik voor klanten met gegevens die wordt geregeld.**

De Azure-beveiliging en naleving blauwdruk - HIPAA/HITRUST Health Data en AI blauwdruk biedt hulpprogramma's en hulp bij het implementeren van een veilige, Health Insurance Portability en Accountability Act (HIPAA) en Health informatie vertrouwen Alliance (HITRUST) gereed platform-as-a-service (PaaS)-omgeving voor het opnemen, opslaan, analyseren en interactie met persoonlijke en niet-persoonlijke medische dossiers in een beveiligde, meerdere lagen cloudomgeving, geïmplementeerd als een end-to-end-oplossing. Het brengt een algemene referentiearchitectuur en is ontworpen voor het vereenvoudigen van de aanvaarding van Microsoft Azure. Deze architectuur opgegeven ziet u een oplossing om te voldoen aan de behoeften van organisaties om een cloud-gebaseerde benadering voor het verminderen van de belasting en de kosten van de implementatie te zoeken.

![](images/components.png)

De oplossing is ontworpen voor een verzameling voorbeeldgegevens geformatteerd met snelle gezondheidszorg interoperabiliteit Resources (FHIR), een wereldwijde standaard voor het uitwisselen van gezondheidszorg informatie elektronisch, gebruiken en op te slaan op een veilige manier. Klanten kunnen Azure Machine Learning vervolgens gebruiken om te profiteren van krachtige hulpprogramma's voor bedrijfsinformatie en analyses bekijken voorspellingen op de voorbeeldgegevens. Als een voorbeeld van het soort experiment die Azure Machine Learning kunt vereenvoudigen, omvat de blauwdruk een voorbeeldgegevensset, scripts en hulpprogramma's voor het voorspellen van een patiënt-verblijf in een ziekenhuisafdeling. 

Deze blauwdruk is bedoeld om te fungeren als een modulaire basis voor klanten om aan te passen aan hun specifieke vereisten, het ontwikkelen van nieuwe Azure Machine learning gebruikt om op te lossen beide klinische en operationele use-casescenario's. Het is ontworpen om te worden beveiligd en compatibel wanneer geïmplementeerd; klanten zijn echter verantwoordelijk voor rollen correct configureren en implementeren van eventuele wijzigingen. Houd rekening met het volgende:

-   Deze blauwdruk biedt een basislijn om Microsoft Azure gebruiken in een HITRUST klanten te helpen en HIPAA-omgeving.

-   Hoewel de blauwdruk is ontworpen om te worden uitgelijnd met HIPAA en HITRUST (via het Common Security Framework - CSF), moet deze niet ook te voldoen aan totdat gecertificeerd door een externe controleur per certificeringsvereisten voor HIPAA en HITRUST.

-   Klanten zijn verantwoordelijk voor het uitvoeren van de juiste beveiligings- en beoordelingen van een oplossing die is gebouwd met behulp van deze fundamentele architectuur.

## <a name="deploying-the-automation"></a>De automation implementeren

- Volg de instructies in de implementatie-instructies voor het implementeren van de oplossing. 

[![](./images/deploy.png)](https://aka.ms/healthblueprintdeploy)

Voor een snel overzicht van de werking van deze oplossing, bekijk deze [video](https://aka.ms/healthblueprintvideo) worden uitgelegd en gedemonstreerd van de implementatie.

- Veelgestelde vragen vindt u de [Veelgestelde vragen over](https://aka.ms/healthblueprintfaq) richtlijnen.

-   **Architecturaal diagram.** Het diagram toont de referentiearchitectuur gebruikt voor de blauwdruk en het voorbeeld van de use-casescenario.

-   **Implementatiesjablonen**. In deze implementatie [Azure Resource Manager-sjablonen](/azure/azure-resource-manager/resource-group-overview#template-deployment) worden gebruikt voor het implementeren van de onderdelen van de architectuur van automatisch in Microsoft Azure door parameters voor de configuratie op te geven tijdens de installatie.

-   **[Geautomatiseerde implementatiescripts](https://aka.ms/healthblueprintdeploy)**. Deze scripts helpen bij het implementeren van de oplossing. De scripts bestaan uit:


-   Een installatie van de module en [hoofdbeheerder](/azure/active-directory/active-directory-assign-admin-roles-azure-portal) setup-script wordt gebruikt om te installeren en te controleren of vereiste PowerShell-modules en rollen voor globale beheerder correct zijn geconfigureerd. 
-   Installatie van een PowerShell-script wordt gebruikt voor het implementeren van de oplossing, die beschikbaar is via een ZIP-bestand met een vooraf gemaakte demo-functies.

## <a name="solution-components"></a>Oplossingsonderdelen


De fundamentele architectuur bestaat uit de volgende onderdelen:

-   **[Risicomodel](https://aka.ms/healththreatmodel)**  een uitgebreide risicomodel is opgegeven in tm7-indeling voor gebruik met de [Microsoft Threat Modeling Tool](https://www.microsoft.com/en-us/download/details.aspx?id=49168), de onderdelen van de oplossing wordt weergegeven, de gegevens stromen tussen de, en het vertrouwen grenzen. Het model kunt klanten inzicht krijgen in de punten van de mogelijke risico's in de infrastructuur van system bij het ontwikkelen van machine learning-onderdelen of andere wijzigingen aanbrengen.

-   **[Klant-implementatie matrix](https://aka.ms/healthcrmblueprint)**  een Microsoft Excel-werkmap bevat de relevante HITRUST-vereisten en wordt uitgelegd hoe Microsoft en de klant verantwoordelijk zijn voor het voldoen aan elkaar.

-   **[Status controleren.](https://aka.ms/healthreviewpaper)** De oplossing is gecontroleerd door Coalfire systems, Inc. De status van naleving (HIPAA en HITRUST) te beoordelen en richtlijnen voor implementatie biedt één auditor\'s beoordeling van de oplossing en overwegingen voor het transformeren van de blauwdruk aan een gereed is voor productie-implementatie.

# <a name="architectural-diagram"></a>Architectuurdiagram


![](images/refarch.png)

## <a name="roles"></a>Rollen


De blauwdruk definieert twee rollen voor gebruikers met beheerdersrechten (operators) en drie rollen voor gebruikers in het ziekenhuismanagement en patiëntenzorg introduceren. Een zesde rol is gedefinieerd voor een revisor voor de evaluatie van naleving van HIPAA en andere voorschriften. Azure Role-based Access Control (RBAC) beschikt u nauwkeurig gerichte toegangsbeheer voor elke gebruiker van de oplossing door middel van ingebouwde en aangepaste rollen. Zie [aan de slag met toegangsbeheer op basis van rollen in Azure portal](https://docs.microsoft.com/azure/role-based-access-control/overview) en [ingebouwde rollen voor op rollen gebaseerd toegangsbeheer in Azure](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) voor gedetailleerde informatie over RBAC-rollen en machtigingen.

### <a name="site-administrator"></a>Sitebeheerder


De beheerder is verantwoordelijk voor het Azure-abonnement van de klant. Ze beheren van de algehele implementatie, maar hebben geen toegang tot de records.

-   Standaard roltoewijzingen: [eigenaar](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner)

-   Aangepaste roltoewijzingen: N.V.T.

-   Bereik: abonnement

### <a name="database-analyst"></a>Database-analist

De databaseanalist worden beheerd door de SQL Server-exemplaar en de database.
Ze hebben geen toegang tot de records.

-   Ingebouwde roltoewijzingen: [' SQL DB Contributor '](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-db-contributor), [Inzender voor SQL Server](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-server-contributor)

-   Aangepaste roltoewijzingen: N.V.T.

-   Bereik: ResourceGroup

 ### <a name="data-scientist"></a>Gegevenswetenschapper


De gegevenswetenschapper werkt de Azure Machine Learning-service. Ze kunnen importeren, exporteren, en beheren van gegevens en rapporten uitvoeren. De gegevenswetenschapper heeft toegang tot de gegevens, maar heeft geen beheerdersrechten.

-   Ingebouwde roltoewijzingen: [Inzender voor Opslagaccounts](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-account-contributor)

-   Aangepaste roltoewijzingen: N.V.T.

-   Bereik: ResourceGroup

### <a name="chief-medical-information-officer-cmio"></a>Chief Medical Information Officer (CMIO)


De CMIO kloof de tussen informatica-/ technologiemedewerkers en zorgprofessionals in een zorginstelling overbrugt. Hun taken behoren doorgaans analytische gegevens gebruiken om te bepalen als resources op de juiste wijze worden toegewezen binnen de organisatie.

-   Ingebouwde roltoewijzingen: geen

### <a name="care-line-manager"></a>Care Line Manager


De care line manager rechtstreeks is betrokken bij de zorgverlening aan patiënten.
Het is de taak van deze functie om de status van individuele patiënten te bewaken, alsmede ervoor te zorgen dat er personeel beschikbaar is om aan de specifieke zorgeisen van hun patiënten te voldoen. De care line manager is verantwoordelijk voor het toevoegen en bijwerken van de records.

-   Ingebouwde roltoewijzingen: geen

-   Aangepaste roltoewijzingen: heeft bevoegdheden om uit te voeren HealthcareDemo.ps1 hiervoor beide toegangsbeheer patiënt en ontladen.

-   Bereik: ResourceGroup

### <a name="auditor"></a>Revisor


De revisor evalueert de oplossing voor naleving. Ze hebben geen directe toegang tot het netwerk.

-   Ingebouwde roltoewijzingen: [lezer](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#reader)

-   Aangepaste roltoewijzingen: N.V.T.

-   Bereik: abonnement

## <a name="example-use-case"></a>Voorbeeld van de Use case


De voorbeeld-use-case deel uitmaakt van deze blauwdruk ziet u hoe de blauwdruk kan worden gebruikt om in te schakelen van machine learning en analyse van health-gegevens in de cloud. Contosoclinic is een kleine ziekenhuis zich in de Verenigde Staten. De beheerders in ziekenhuizen netwerk wilt gebruiken van Azure Machine Learning om te voorspellen beter de verblijfsduur van een patiënt op het moment van toegelaten, om de werkbelasting van de operationele efficiëntie en verbeter de kwaliteit van zorg die kan bieden.

### <a name="predicting-length-of-stay"></a>Verblijfsduur


Het voorbeeld van de use-casescenario maakt gebruik van Azure Machine Learning om te voorspellen van de verblijfsduur van een nieuwe toegelaten patiënt door het vergelijken van de details van de medische genomen om de inname tot samengevoegde historische gegevens van vorige patiënten.
De blauwdruk omvat een groot aantal anonieme medische dossiers ter illustratie van de training en voorspellende mogelijkheden van de oplossing. In een productie-implementatie zou klanten hun eigen records gebruiken voor het trainen van de oplossing voor nauwkeurigere voorspellingen zetten op basis van de unieke details van hun omgeving, opslagruimten en patiënten.

### <a name="users-and-roles"></a>Gebruikers en rollen


**Sitebeheerder--Alex**

*E-mailadres: Alex\_SiteAdmin*

Alex van taak is het evalueren van de technologieën die kunnen reductie van de werkbelasting van het beheer van een on-premises netwerk en verlagen voor beheer. Alex evalueren Azure heeft gedurende een bepaalde periode, maar heeft pogingen gedaan om de services die hij nodig heeft om te voldoen aan de nalevingsvereisten HiTrust patiënt gegevens opslaan in de cloud te configureren. Alex heeft de status van Azure AI voor het implementeren van een oplossing de status gereed is voor naleving, die de vereisten om te voldoen aan de eisen van klanten voor HiTrust heeft verwerkt geselecteerd.

**Gegevenswetenschapper Debra**

*E-mailadres: Debra\_DataScientist*

Debra is verantwoordelijk voor het maken en gebruiken van modellen die medische dossiers analyseren voor inzicht in patiëntenzorg introduceren. Debra maakt gebruik van SQL en de statistische R-programmeertaal om haar modellen te maken.

**Database-analist--Danny**

*E-mailadres: Danny\_DBAnalyst*

Danny is de belangrijkste contactpersoon voor alles wat met betrekking tot de Microsoft SQL Server de patiënt gegevens voor Contosoclinic slaat. Danny is een ervaren SQL Server-beheerder die onlangs met Azure SQL Database raken is.

**Chief Medical Information Officer--Caroline**

Caroline werkt samen met de Care Line Manager Chris en Debra de Gegevenswetenschapper om te bepalen welke factoren van invloed zijn op de verblijfsduur.
De voorspellingen op basis van de lengte van blijven (LOS)-oplossing Caroline gebruikt om te bepalen als resources op de juiste wijze worden toegewezen in het ziekenhuisnetwerk. Bijvoorbeeld, met behulp van het dashboard dat is opgegeven in deze oplossing.

**Care Line Manager--Chris**

*E-mailadres: Chris\_CareLineManager*

Als de persoon rechtstreeks verantwoordelijk voor het beheren van de patiënt is opgenomen en ontslaan op Contosoclinic, Chris maakt gebruik van de prognoses gegenereerd door de oplossing LOS om ervoor te zorgen dat voldoende personeel beschikbaar zijn voor care bieden aan patiënten terwijl ze worden blijven de opslagruimte.

**Auditor--Han**

*E-mailadres: Han\_Auditor*

Han is een gecertificeerde accountant die ervaring controle-instellingen voor ISO, SOC en HiTrust. Han is aangesteld om te controleren van Contosoclinc netwerk. Han kunt bekijken van de klant verantwoordelijkheid Matrix voorzien van de oplossing om ervoor te zorgen dat de blauwdruk en LOS oplossing kunnen worden gebruikt voor het opslaan, verwerken, en gevoelige, persoonlijke gegevens weer te geven.


# <a name="design-configuration"></a>Configuratie ontwerpen


In deze sectie worden de standaardconfiguraties en beveiligingsmaatregelen die zijn ingebouwd in de blauwdruk die worden beschreven op:

- **OPNAME** onbewerkte gegevensbronnen waaronder FHIR-gegevensbron
- **STORE** gevoelige informatie
- **ANALYSEREN** en resultaten te voorspellen
- **INTERACTIE** met de resultaten en voorspellingen
- **IDENTITEIT** beheer van de oplossing
- **BEVEILIGING** -functies ingeschakeld


## <a name="identity"></a>IDENTITEIT 

### <a name="azure-active-directory-and-role-based-access-control-rbac"></a>Azure Active Directory en op rollen gebaseerd toegangsbeheer (RBAC)


**Verificatie:**

-   [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) is de Microsoft\'s multitenant cloudgebaseerde directory en identity management-service. Alle gebruikers voor de oplossing zijn gemaakt in Azure Active Directory, met inbegrip van gebruikers met toegang tot de SQL-Database.



-   Verificatie van de toepassing wordt uitgevoerd met behulp van Azure AD. Zie voor meer informatie, [toepassingen integreren met Azure Active Directory](/azure/active-directory/develop/active-directory-integrating-applications).

-   [Azure Active Directory Identity Protection](/azure/active-directory/active-directory-identityprotection) detecteert mogelijke beveiligingsproblemen die betrekking hebben op de identiteiten van uw organisatie, automatische antwoorden op gedetecteerde verdachte activiteit met betrekking tot de identiteiten van uw organisatie, configureert en verdachte incidenten onderzoekt het probleem en wordt de juiste actie op te lossen.

-   [Azure Role-based Access Control (RBAC)](/azure/role-based-access-control/role-assignments-portal) maakt nauwkeurig beheer gerichte toegang voor Azure. Abonnementstoegang is beperkt tot de beheerder van het abonnement en Azure Key Vault toegang is beperkt tot de sitebeheerder. Sterke wachtwoorden (minimale met ten minste één hoofdletters en kleine letter, cijfer en speciaal teken 12 tekens) zijn vereist.

-   Meervoudige verificatie wordt ondersteund wanneer de switch - enableMFA is ingeschakeld tijdens de implementatie.

-   Wachtwoorden verlopen na 60 dagen wanneer de switch - enableADDomainPasswordPolicy is ingeschakeld tijdens de implementatie.

**Rollen:**

-   De oplossing maakt gebruik van [ingebouwde rollen](/azure/role-based-access-control/built-in-roles) voor het beheren van toegang tot bronnen.

-   Alle gebruikers zijn specifieke ingebouwde rollen toegewezen standaard.

### <a name="azure-key-vault"></a>Azure Key Vault

-   Gegevens die zijn opgeslagen in Key Vault bevat:

    -   Sleutel van Application insight
    -   De sleutel voor toegang tot de opslag van gegevens
    -   De verbindingsreeks
    -   Patiëntgegevens tabelnaam
    -   Azure ML Web Service-eindpunt
    -   Azure ML API-sleutel voor Service

-   Geavanceerde beleidsregels zijn geconfigureerd op basis van behoefte
-   Toegangsbeleid voor Key Vault zijn met de minimaal vereiste machtigingen voor sleutels en geheimen gedefinieerd
-   Alle sleutels en geheimen in Key Vault hebben verloopdatum
-   Alle sleutels in Key Vault zijn beveiligd met HSM \[sleuteltype = HSM beveiligd 2048-bits RSA-sleutel\]
-   Alle gebruikers/identiteiten zijn minimaal vereiste machtigingen met behulp van rollen gebaseerd toegangsbeheer (RBAC) verleend
-   Toepassingen delen een Key Vault niet, tenzij ze elkaar vertrouwen en ze nodig hebben toegang tot de dezelfde geheimen tijdens runtime
-   Diagnostische logboeken voor Key Vault worden ingeschakeld met een bewaarperiode van ten minste 365 dagen.
-   Toegestane cryptografiebewerkingen voor sleutels zijn beperkt tot die vereist is

## <a name="ingest"></a>OPNEMEN 

### <a name="azure-functions"></a>Azure Functions
De oplossing is ontworpen om te gebruiken [Azure Functions](/azure/azure-functions/) voor het verwerken van de voorbeeld-lengte van blijven gegevens in de demo analytics gebruikt. Drie mogelijkheden in de functies die zijn gemaakt.

**1. Bulksgewijs importeren van gegevens phi-gegevens van de klant**

Als u de demoscript. . \\HealthcareDemo.ps1 met de **BulkPatientAdmission** schakelen zoals wordt beschreven in **implementeren en uitvoeren van de demo** het uitvoeren van de volgende verwerking pijplijn:
1. **Azure Blob-opslag** -patiëntgegevens CSV-voorbeeldbestand geüpload naar de opslag
2. **Event Grid** -gebeurtenis wordt gepubliceerd gegevens naar Azure-functie (bulkimport - blob gebeurtenis)
3. **Azure-functie** - de verwerking wordt uitgevoerd en de gegevens worden opgeslagen in SQL-opslag met behulp van de beveiligde functie - gebeurtenis (Typ; blob-url)
4. **SQL DB** : het databasearchief voor de patiënt-gegevens met labels voor classificatie en het ML-proces wordt gestart om te doen het trainingsexperiment.

![](images/dataflow.png)

De azure-functie is bovendien ontworpen om te lezen en aangewezen gevoelige gegevens beschermen in de voorbeeldgegevensset met behulp van de volgende codes:
- dataProfile = > "ePHI"
- de eigenaar van = > \<Site beheerder UPN\>
- omgeving = > 'Test'
- afdeling = > 'Algemene ecosysteem' de codering is toegepast op de voorbeeldgegevensset waar patiënt 'naam' is geïdentificeerd als niet-versleutelde tekst.

**2. Toegangsbeheer van nieuwe patiënten**

Als u de demoscript. . \\HealthcareDemo.ps1 met de **BulkPatientadmission** schakelen zoals wordt beschreven in **implementeren en uitvoeren van de demo** het uitvoeren van de volgende verwerkings-pipeline: ![](images/securetransact.png) 
 **1. Azure-functie** geactiveerd en de functie-aanvragen voor een [bearer-token](/rest/api/) uit Azure Active directory.

**2. Key Vault** aangevraagd voor een geheim dat is gekoppeld aan het aangevraagde token.

**3. Azure-rollen valideren van de aanvraag en autoriseren van de aanvraag voor toegang tot de Key Vault.

**4. Key Vault** retourneert het geheim in dit geval de SQL DB-verbindingsreeks.

**5. Azure-functie** maakt gebruik van de verbindingsreeks veilig verbinding maken met SQL Database en verdere verwerking voor het opslaan van gegevens ePHI blijft.

Voor het bereiken van de opslag van de gegevens, is een algemene API-schema geïmplementeerd snel gezondheidszorg interoperabiliteit Resources (FHIR, uitgesproken fire) te volgen. De functie is de volgende FHIR exchange elementen opgegeven:

-   [Patiënten schema](https://www.hl7.org/fhir/patient.html) 'waar' wordt informatie geboden over van een patiënt.

-   [Observatie van schema](https://www.hl7.org/fhir/observation.html) bevat informatie over het centrale element in de gezondheidszorg, gebruikt te ondersteunen, diagnose, voortgang bepaalt basislijnen en patronen en zelfs vastleggen demografische kenmerken. 

-   [Schema optreden](https://www.hl7.org/fhir/encounter.html) behandelt de typen tegenkomt, zoals ambulant, noodgevallen, home status, opneming en virtuele tegenkomt.

-   [Voorwaarde schema](https://www.hl7.org/fhir/condition.html) bevat gedetailleerde informatie over een voorwaarde, probleem, diagnose, of andere gebeurtenis, situatie, probleem of klinische concept, dat naar een niveau van belang is toegenomen.  



### <a name="event-grid"></a>Event Grid


De oplossing biedt ondersteuning voor Azure Event Grid, een geïntegreerde service voor het beheren van routering van alle gebeurtenissen van elke bron naar elke bestemming opgeven:

-   [Verificatie en beveiliging](/azure/event-grid/security-authentication)

-   [Op rollen gebaseerd toegangsbeheer](/azure/event-grid/security-authentication#management-access-control) voor verschillende bewerkingen zoals gebeurtenisabonnementen aanbieding, een nieuwe te maken en genereren van sleutels

-   Controleren

## <a name="store"></a>STORE 

### <a name="sql-database-and-server"></a>SQL Database en Server 


-   [Transparante gegevensversleuteling (TDE)](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) biedt realtime versleuteling en ontsleuteling van gegevens die zijn opgeslagen in de Azure SQL Database, met behulp van een sleutel is opgeslagen in Azure Key Vault.

-   [Evaluatie van beveiligingsproblemen SQL](https://docs.microsoft.com/azure/sql-database/sql-vulnerability-assessment) is een eenvoudig hulpprogramma dat kan detecteren, volgen en herstellen van potentiële databaseproblemen configureren.

-   [SQL Database Threat Detection](/azure/sql-database/sql-database-threat-detection) ingeschakeld.

-   [SQL Database Auditing](/azure/sql-database/sql-database-auditing) ingeschakeld.

-   [SQL-Database metrische gegevens en registratie in diagnoselogboek](/azure/sql-database/sql-database-metrics-diag-logging) ingeschakeld.

-   [Firewallregels voor server - en databaseniveau](/azure/sql-database/sql-database-firewall-configure) hebben is strengere.

-   [Altijd versleuteld kolommen](/azure/sql-database/sql-database-always-encrypted-azure-key-vault) worden gebruikt voor het beveiligen van de namen van patiënten voornaam, achternaam en laatste.
    Daarnaast gebruikt de versleuteling van de kolom database ook Azure Active Directory (AD) om te verifiëren van de toepassing naar Azure SQL Database.

-   Toegang tot SQL Database en SQL Server is geconfigureerd volgens het principe van minimale bevoegdheden.

-   Alleen vereiste IP-adressen zijn toegestaan via de SQL-firewall.

### <a name="storage-accounts"></a>Opslagaccounts


-   [Gegevens in beweging worden overgedragen met behulp van TLS/SSL alleen](/azure/storage/common/storage-require-secure-transfer?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json).

-   Anonieme toegang is niet toegestaan voor containers.

-   Regels voor waarschuwingen zijn geconfigureerd voor het bijhouden van anonieme activiteit.

-   HTTPS is vereist voor toegang tot de resources van opslagaccount.

-   Verificatiegegevens voor de aanvraag wordt vastgelegd en gecontroleerd.

-   Gegevens in Blob-opslag worden in rust versleuteld.

## <a name="analyze"></a>ANALYSEREN

### <a name="machine-learning"></a>Machine Learning


-   [Logboekregistratie is ingeschakeld](/azure/machine-learning/studio/web-services-logging) voor Machine Learning-webservices.
- met behulp van [Machine Learning](/azure/machine-learning/desktop-workbench/experimentation-service-configuration) workbench is vereist voor de ontwikkeling van experimenten, die de mogelijkheid om te voorspellen op een set oplossing biedt. [Integratie van de workbench](/azure/machine-learning/desktop-workbench/using-git-ml-project) beheer van experimenten kunt stroomlijnen.

## <a name="security"></a>BEVEILIGING

### <a name="azure-security-center"></a>Azure Security Center
- [Azure Security Center](https://azure.microsoft.com/services/security-center/) biedt een gecentraliseerde weergave van de beveiligingsstatus van al uw Azure-resources. In een oogopslag kunt u controleren dat de juiste beveiligingscontroles zijn geïnstalleerd en correct geconfigureerd en u kunt snel zien of er resources die uw aandacht vereisen. 

- [Azure Advisor](/azure/advisor/advisor-overview) is een persoonlijke cloudconsultant die u helpt volgen aanbevolen procedures voor het optimaliseren van uw Azure-implementaties. Het analyseert uw resourceconfiguratie en gebruikstelemetrie, en raadt vervolgens oplossingen aan die u kunnen helpen de kosteneffectiviteit, prestaties, hoge beschikbaarheid en beveiliging van uw Azure-resources te verbeteren.

### <a name="application-insights"></a>Application Insights
- [Application Insights](/azure/application-insights/app-insights-overview) is een uitbreidbare service voor Application Performance Management (APM) voor webontwikkelaars op meerdere platforms. Hiermee kunt u uw livewebtoepassing controleren. Afwijkende prestaties wordt gedetecteerd. De service bevat krachtige analysehulpmiddelen om u te helpen bij het vaststellen van problemen en te begrijpen wat gebruikers daadwerkelijk doen met uw app. Het is bedoeld om u te helpen de prestaties en bruikbaarheid continu te verbeteren.

### <a name="azure-alerts"></a>Azure-waarschuwingen
- [Waarschuwingen bieden een methode van de bewaking van Azure-services en kunnen u voorwaarden configureren voor gegevens. Waarschuwingen bieden ook meldingen wanneer de voorwaarde voor een waarschuwing overeenkomt met de gegevens.

### <a name="operations-management-suite-oms"></a>Operations Management Suite (OMS)
[Operations Management Suite (ook wel bekend als OMS)](/azure/operations-management-suite/operations-management-suite-overview) is een verzameling beheerservices.

-   Werkruimte is ingeschakeld voor Security Center

-   Werkruimte is voor bewaking van de werkbelasting ingeschakeld

-   Bewaking van de werkbelasting is ingeschakeld voor:

    -   Identiteit en toegang

    -   Beveiliging en controle

    -   Azure SQL DB-analyse

    -   [Azure WebApp Analytics](/azure/log-analytics/log-analytics-azure-web-apps-analytics) oplossing

    -   Key Vault-analyse

    -   Tracering wijzigen

-   [Application Insights-Connector (Preview)](/azure/log-analytics/log-analytics-app-insights-connector) is ingeschakeld

-   [Activiteit logboekanalyse](/azure/log-analytics/log-analytics-activity) is ingeschakeld
