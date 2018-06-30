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
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37113228"
---
# <a name="azure-security-and-compliance-blueprint---hipaahitrust-health-data-and-ai"></a>Azure-beveiliging en naleving blauwdruk - statusgegevens HIPAA/HITRUST en AI

## <a name="overview"></a>Overzicht

**De Azure-beveiliging en naleving blauwdruk - statusgegevens HIPAA/HITRUST en AI biedt een directe implementatie van een Azure-PaaS-oplossing te laten zien hoe veilig opnemen, opslaan, analyseren en communiceren met health gegevens tijdens kunnen voldoen aan nalevingsvereisten bedrijfstak vereisten. De blauwdruk kunt versnellen cloud acceptatie en het gebruik voor klanten met gegevens die wordt geregeld.**

De Azure-beveiliging en naleving blauwdruk - statusgegevens HIPAA/HITRUST en AI blauwdruk biedt hulpprogramma's en richtlijnen voor het implementeren van een veilige, Health Insurance Portability en Accountability Act (HIPAA) en Health informatie vertrouwen Alliance (HITRUST) gereed platform as a service (PaaS)-omgeving voor het opnemen van, opslaan, analyseren en interactie met persoonlijke en niet-persoonlijke medische gegevens in een cloudomgeving met meerdere lagen, veilige geïmplementeerd als een oplossing voor end-to-end. Het een algemene referentiearchitectuur gepresenteerd en is ontworpen om u te vereenvoudigen, de acceptatie van Microsoft Azure. Deze architectuur opgegeven ziet u een oplossing om te voldoen aan de behoeften van organisaties die een vermindering van de werkbelasting en de kosten van de implementatie van cloud-gebaseerde benadering zoeken.

![](images/components.png)

De oplossing is zodanig ontworpen dat een voorbeeldgegevensset geformatteerd met snel gezondheidszorg interoperabiliteit Resources (FHIR), een wereldwijde norm voor de gezondheidszorg gegevensuitwisseling elektronisch, gebruiken en sla deze op een veilige manier. Klanten kunnen Azure Machine Learning vervolgens gebruiken om te profiteren van krachtige hulpprogramma's voor bedrijfsinformatie en -analyse voorspellingen op de voorbeeldgegevens te bekijken. Als een voorbeeld van het soort experiment die Azure Machine Learning kunnen vergemakkelijken, bevat de blauwdruk een voorbeeldgegevensset, scripts en hulpprogramma's voor het voorspellen van de lengte van een geduld verblijf in een faciliteit ziekenhuis bent. 

Deze blauwdruk is bedoeld om te fungeren als een modulaire basis voor klanten om aan te passen aan de specifieke vereisten voor het ontwikkelen van nieuwe Azure Machine learning-experimenten om op te lossen beide klinische en operationele gebruiksscenario's. Het is ontworpen om te worden beveiligd en compatibel wanneer geïmplementeerd; Er zijn echter klanten die verantwoordelijk is voor rollen correct configureren en implementeren van eventuele wijzigingen. Houd rekening met het volgende:

-   Deze blauwdruk biedt een basis zodat klanten gebruik van Microsoft Azure in een HITRUST en HIPAA-omgeving.

-   Hoewel de blauwdruk is ontworpen om te worden uitgelijnd met HIPAA en HITRUST (via de algemene beveiliging Framework--CSF), deze moet niet als compatibel worden beschouwd totdat gecertificeerd door een externe revisor per HIPAA en HITRUST certificeringsvereisten.

-   Klanten zijn verantwoordelijk voor het uitvoeren van de juiste beveiliging en naleving beoordelingen van een oplossing die zijn gebouwd met behulp van deze fundamentele architectuur.

## <a name="deploying-the-automation"></a>De automation implementeren

- Volg de instructies in de implementatie-instructies voor het implementeren van de oplossing. 

[![](./images/deploy.png)](https://aka.ms/healthblueprintdeploy)

Voor een snel overzicht van de werking van deze oplossing, bekijkt u deze [video](https://aka.ms/healthblueprintvideo) waarin wordt uitgelegd en demonstreren van de implementatie ervan.

- Veelgestelde vragen kunt u vinden in de [Veelgestelde vragen over](https://aka.ms/healthblueprintfaq) richtlijnen.

-   **Architecturaal diagram.** Het diagram toont de referentiearchitectuur gebruikt voor de blauwdruk en in het voorbeeld voorbeeldscenario gebruiken.

-   **Implementatiesjablonen**. In deze implementatie [Azure Resource Manager-sjablonen](/azure/azure-resource-manager/resource-group-overview#template-deployment) worden gebruikt voor de onderdelen van de architectuur automatisch implementeren in Microsoft Azure door te geven configuratieparameters tijdens de installatie.

-   **[Geautomatiseerde implementatiescripts](https://aka.ms/healthblueprintdeploy)**. Deze scripts helpen bij het implementeren van de oplossing. De scripts bestaan uit:


-   Een installatie van de module en [hoofdbeheerder](/azure/active-directory/active-directory-assign-admin-roles-azure-portal) installatiescript wordt gebruikt om te installeren en te controleren of vereiste PowerShell-modules en -rollen van de globale beheerder correct zijn geconfigureerd. 
-   Installatie van een PowerShell-script wordt gebruikt voor het implementeren van de oplossing die beschikbaar is via een ZIP-bestand dat een vooraf samengestelde demo-functies bevat.

## <a name="solution-components"></a>Oplossingsonderdelen


De fundamentele architectuur bestaat uit de volgende onderdelen:

-   **[Risicomodel](https://aka.ms/healththreatmodel)**  een uitgebreide risicomodel is opgegeven in tm7-indeling voor gebruik met de [Microsoft Threat Modeling Tool](https://www.microsoft.com/en-us/download/details.aspx?id=49168), de onderdelen van de oplossing wordt weergegeven, de gegevens tussen en de vertrouwensrelatie stroomt grenzen. Het model kunt zodat klanten inzicht krijgen van de punten van de mogelijke risico's in de infrastructuur van het systeem bij het ontwikkelen van machine learning-onderdelen of andere wijzigingen.

-   **[Klant-implementatie matrix](https://aka.ms/healthcrmblueprint)**  een Microsoft Excel-werkmap worden de vereisten van de relevante HITRUST en wordt uitgelegd hoe Microsoft en de klant die verantwoordelijk is voor elk criterium voldoen.

-   **[Status controleren.](https://aka.ms/healthreviewpaper)** De oplossing is gelezen door Coalfire systems, Inc. De Health-naleving (HIPAA en HITRUST) te beoordelen en richtlijnen voor implementatie biedt auditor\'s beoordeling van de oplossing en overwegingen voor het transformeren van de blauwdruk aan een gereed is voor productie-implementatie.

# <a name="architectural-diagram"></a>Architectuurdiagram


![](images/refarch.png)

## <a name="roles"></a>Rollen


De blauwdruk definieert twee rollen voor gebruikers met beheerdersrechten (operators) en de drie functies voor gebruikers in ziekenhuis beheer- en patiëntenzorg. Een zesde rol is gedefinieerd voor auditor evalueren van naleving van HIPAA en andere voorschriften. Azure op rollen gebaseerde toegangsbeheer (RBAC) beschikt nauwkeurig gerichte toegangsbeheer voor elke gebruiker van de oplossing door ingebouwde en aangepaste rollen. Zie [aan de slag met toegangsbeheer op basis van rollen in Azure portal](https://docs.microsoft.com/azure/role-based-access-control/overview) en [ingebouwde functies voor op rollen gebaseerd toegangsbeheer van Azure](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) voor gedetailleerde informatie over RBAC, rollen en machtigingen.

### <a name="site-administrator"></a>De beheerder van site


Beheerder van de site is verantwoordelijk voor de klant Azure-abonnement. Ze bepalen van de algehele implementatie, maar hebben geen toegang tot patiëntrecords.

-   Standaard roltoewijzingen: [eigenaar](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner)

-   Aangepaste roltoewijzingen: N.V.T.

-   Bereik: abonnement

### <a name="database-analyst"></a>Database-analist

De databaseanalist worden beheerd door de SQL Server-exemplaar en de database.
Ze hebben geen toegang tot patiëntrecords.

-   Ingebouwde roltoewijzingen: [SQL DB Contributor](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-db-contributor), [Inzender van SQL Server](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-server-contributor)

-   Aangepaste roltoewijzingen: N.V.T.

-   Bereik: ResourceGroup

 ### <a name="data-scientist"></a>Gegevens wetenschappelijk


Het wetenschappelijk gegevens werkt de Azure Machine Learning-service. Ze kunnen importeren, exporteren, en gegevens beheren en uitvoeren van rapporten. Het wetenschappelijk gegevens toegang heeft tot patiëntgegevens, maar heeft geen beheerdersrechten heeft.

-   Ingebouwde roltoewijzingen: [Storage Account Inzender](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-account-contributor)

-   Aangepaste roltoewijzingen: N.V.T.

-   Bereik: ResourceGroup

### <a name="chief-medical-information-officer-cmio"></a>Chief medische Information Officer (CMIO)


De CMIO de Verdeel tussen informatics/technologie en professionals in de gezondheidszorg in een organisatie in de gezondheidszorg gevestigd. Hun taken omvatten met analytics om te bepalen of bronnen op de juiste wijze worden toegewezen binnen de organisatie.

-   Ingebouwde roltoewijzingen: geen

### <a name="care-line-manager"></a>Zorg lijnmanager


De lijnmanager voorzichtig is rechtstreeks met zorg patiënten betrokken.
Het is de taak van deze functie om de status van individuele patiënten te bewaken, alsmede ervoor te zorgen dat er personeel beschikbaar is om aan de specifieke zorgeisen van hun patiënten te voldoen. De lijnmanager voorzichtig is verantwoordelijk voor het toevoegen en patiëntrecords bijwerken.

-   Ingebouwde roltoewijzingen: geen

-   Aangepaste roltoewijzingen: heeft bevoegdheden om uit te voeren HealthcareDemo.ps1 hiervoor beide toelating geduld en ontladen.

-   Bereik: ResourceGroup

### <a name="auditor"></a>Auditor


De controleur evalueert de oplossing voor naleving. Ze hebben geen directe toegang tot het netwerk.

-   Ingebouwde roltoewijzingen: [lezer](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#reader)

-   Aangepaste roltoewijzingen: N.V.T.

-   Bereik: abonnement

## <a name="example-use-case"></a>Voorbeeld gebruiksvoorbeeld


Het voorbeeld gebruiksvoorbeeld die deel uitmaakt van deze blauwdruk ziet u hoe de blauwdruk kan worden gebruikt voor het inschakelen van machine learning en analyse van de health-gegevens in de cloud. Contosoclinic is een kleine ziekenhuis zich in de Verenigde Staten. De netwerkbeheerders ziekenhuis wilt gebruiken van Azure Machine Learning om te anticiperen op de lengte van een geduld blijven op het moment van toegelaten om de werkbelasting van de operationele efficiëntie verhogen en de kwaliteit van voorzichtig die kan geven te verbeteren.

### <a name="predicting-length-of-stay"></a>Verblijf voorspellen


Het voorbeeldscenario voor gebruik case maakt gebruik van Azure Machine Learning te voorspellen, een nieuw toegelaten geduld lengte verblijf door de medische details die is genomen op patiënt inname tot samengevoegde historische gegevens uit de vorige patiënten vergelijken.
De blauwdruk bevat een groot aantal geanonimiseerde medische registreert ter illustratie van de mogelijkheden van de oplossing voor training en voorspeld. Klanten zouden hun eigen records gebruiken voor het trainen van de oplossing voor een meer nauwkeurige prognoses maakt met de unieke gegevens van hun omgeving, opslagruimten en patiënten opgetreden bij het weergeven in een productie-implementatie.

### <a name="users-and-roles"></a>Gebruikers en -rollen


**Site-beheerder--Alex**

*E-mailadres: Alex\_SiteAdmin*

Alex van taak is het evalueren van de technologieën die u kunnen Verminder de belasting van het beheer van een on-premises netwerk en de kosten voor het beheer. Alex evalueren Azure heeft gedurende een bepaalde periode, maar heeft pogingen gedaan om de services die hij nodig heeft om te voldoen aan de nalevingsvereisten HiTrust geduld gegevens opslaan in de cloud te configureren. Alex is de Azure Health AI voor het implementeren van een oplossing voor naleving-ready health, die de vereisten te voldoen aan de klantvereisten voor HiTrust heeft opgelost geselecteerd.

**Gegevens wetenschappelijk--Debra**

*E-mailadres: Debra\_DataScientist*

Debra is verantwoordelijk voor het gebruik en het maken van modellen die medische gegevens om op te geven inzicht in de zorgsector analyseren. Debra maakt gebruik van SQL en de programmeertaal R statistische om haar modellen te maken.

**Database-analist--Danny**

*E-mailadres: Danny\_DBAnalyst*

Danny is de belangrijkste contactpersoon voor alles met betrekking tot de Microsoft SQL Server die de patiëntgegevens voor Contosoclinic opslaat. Danny is een ervaren SQL Server-beheerder die onlangs vertrouwd met Azure SQL Database raken is.

**Medische informatie directeur--Caroline**

Caroline werkt samen met de lijnmanager Care Chris en Debra het wetenschappelijk gegevens om te bepalen welke factoren van invloed patiënt lengte verblijf.
Caroline gebruikt de voorspellingen van de lengte van verblijf (LOS)-oplossing om te bepalen of bronnen op de juiste wijze wordt toegewezen in het netwerk ziekenhuis bent. Bijvoorbeeld, met behulp van het dashboard dat is opgegeven in deze oplossing.

**Belangrijk lijnmanager--Chris**

*E-mailadres: Chris\_CareLineManager*

Als de afzonderlijke rechtstreeks verantwoordelijk voor het beheren van patiënt toelating en lozingen op Contosoclinic, Chris maakt gebruik van de voorspellingen gegenereerd door de oplossing LOS om ervoor te zorgen dat voldoende personeel beschikbaar zijn voor voorzichtig bieden aan patiënten terwijl ze zijn blijven de faciliteit.

**Auditor--Han**

*E-mailadres: Han\_Auditor*

Han is een gecertificeerde controleur die ervaring controle voor ISO, SOC en HiTrust heeft. Han is ingehuurd om te controleren van Contosoclinc netwerk. Han kunt bekijken van de klant verantwoordelijkheid Matrix met de oplossing is bedoeld om ervoor te zorgen dat de blauwdruk en LOS oplossing kunnen worden gebruikt op te slaan, proces, en gevoelige, persoonlijke gegevens worden weergegeven.


# <a name="design-configuration"></a>Configuratie ontwerpen


In deze sectie worden de standaardconfiguraties en veiligheidsmaatregelen ingebouwd in de blauwdruk beschreven aan:

- **OPNEMEN** onbewerkte gegevensbronnen waaronder FHIR-gegevensbron
- **STORE** vertrouwelijke gegevens
- **ANALYSEREN** en resultaten te voorspellen
- **INTERACT** met de resultaten en voorspellingen
- **IDENTITEIT** beheer van de oplossing
- **BEVEILIGING** functies ingeschakeld


## <a name="identity"></a>IDENTITEIT 

### <a name="azure-active-directory-and-role-based-access-control-rbac"></a>Azure Active Directory en op rollen gebaseerde toegangsbeheer (RBAC)


**Verificatie:**

-   [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) is de Microsoft\'s multitenant cloudgebaseerde directory en identity management-service. Alle gebruikers voor de oplossing zijn gemaakt in Azure Active Directory, inclusief gebruikers met toegang tot de SQL-Database.



-   Verificatie van de toepassing wordt uitgevoerd met behulp van Azure AD. Zie voor meer informatie [toepassingen integreren met Azure Active Directory](/azure/active-directory/develop/active-directory-integrating-applications).

-   [Azure Active Directory: Identity Protection](/azure/active-directory/active-directory-identityprotection) detecteert mogelijke beveiligingsproblemen die invloed hebben op de identiteiten van uw organisatie, configureert u automatische antwoorden op gedetecteerde verdachte acties met betrekking tot de identiteiten van uw organisatie, en verdachte incidenten onderzoekt en neemt nodige actie deze op te lossen.

-   [Azure op rollen gebaseerde toegangsbeheer (RBAC)](/azure/role-based-access-control/role-assignments-portal) kunt nauwkeurig gerichte toegangsbeheer voor Azure. Abonnement toegang is beperkt tot de abonnementsbeheerder en Azure Key Vault toegang is beperkt tot de sitebeheerder. Sterke wachtwoorden (minimale met ten minste één hoofdletters en kleine letter, cijfer en speciaal teken 12 tekens) zijn vereist.

-   Multi-factor authentication-server wordt ondersteund als de schakeloptie - enableMFA is ingeschakeld tijdens de implementatie.

-   Wachtwoorden verlopen na 60 dagen als de schakeloptie - enableADDomainPasswordPolicy tijdens de implementatie is ingeschakeld.

**Rollen:**

-   De oplossing maakt gebruik van [ingebouwde rollen](/azure/role-based-access-control/built-in-roles) voor het beheren van toegang tot bronnen.

-   Alle gebruikers worden specifieke ingebouwde rollen standaard toegewezen.

### <a name="azure-key-vault"></a>Azure Key Vault

-   Gegevens die zijn opgeslagen in de Sleutelkluis bevat:

    -   Inzicht Toepassingssleutel
    -   Patiënt sleutel voor toegang tot de opslag van gegevens
    -   Patiënt verbindingsreeks
    -   De tabelnaam patiëntgegevens
    -   Azure ML Web Service-eindpunt
    -   API-sleutel voor Azure ML-Service

-   Geavanceerde beleidsregels zijn geconfigureerd op basis van de noodzaak
-   Sleutelkluis-beleid worden gedefinieerd met de minimaal vereiste machtigingen aan sleutels en geheimen
-   Alle sleutels en geheimen in de Sleutelkluis hebben verloopdatum
-   Alle sleutels in de Sleutelkluis zijn beveiligd met HSM \[sleuteltype = HSM beveiligde 2048-bits RSA-sleutel\]
-   Alle gebruikers/identiteiten krijgen minimaal vereiste machtigingen met rollen gebaseerd toegangsbeheer (RBAC)
-   Toepassingen delen een Sleutelkluis niet tenzij ze elkaar vertrouwen en ze nodig hebben toegang tot de dezelfde geheimen tijdens runtime
-   Diagnostische logboeken voor Sleutelkluis worden ingeschakeld met een bewaarperiode van ten minste 365 dagen.
-   Toegestane cryptografische bewerkingen voor sleutels zijn beperkt tot de referenties die nodig is

## <a name="ingest"></a>OPNEMEN 

### <a name="azure-functions"></a>Azure Functions
De oplossing is zodanig ontworpen dat gebruik [Azure Functions](/azure/azure-functions/) voor het verwerken van de voorbeeld-lengte van blijven gegevens in de demo analytics gebruikt. Drie mogelijkheden in de functies die zijn gemaakt.

**1. Bulkimport van klantgegevens gegevens phi**

Als u de demoscript. . \\HealthcareDemo.ps1 met de **BulkPatientAdmission** schakelen zoals beschreven in **implementeren en uitvoeren van de demo** wordt uitgevoerd de volgende pipeline verwerkt:
1. **Azure Blob Storage** -patiëntgegevens CSV-voorbeeldbestand geüpload naar de opslag
2. **Gebeurtenis raster** -gebeurtenis publiceert gegevens naar Azure-functie (bulkimport - blob gebeurtenis)
3. **Azure-functie** - voert de verwerking en slaat de gegevens in de SQL-opslag met de beveiligde functie - gebeurtenis (Typ; blob-url)
4. **SQL-database** : het databasearchief voor geduld gegevens met labels voor classificatie en het trainingsexperiment doen het ML-proces wordt gestart.

![](images/dataflow.png)

Bovendien is de functie azure ontworpen om te lezen en aangewezen gevoelige gegevens in de verzameling voorbeeldgegevens met behulp van de volgende codes beveiligen:
- dataProfile = > 'ePHI'
- eigenaar = > \<Site Admin UPN\>
- omgeving = > 'Test'
- afdeling = > 'Globale ecosysteem' de codering is toegepast op de voorbeeldgegevensset waar geduld 'names' is geïdentificeerd als leesbare tekst.

**2. Toelating van nieuwe patiënten**

Als u de demoscript. . \\HealthcareDemo.ps1 met de **BulkPatientadmission** schakelen zoals beschreven in **implementeren en uitvoeren van de demo** wordt uitgevoerd de volgende pipeline verwerkt: ![](images/securetransact.png) 
 **1. Azure-functie** geactiveerd en de functie aanvragen voor een [bearer-token](/rest/api/) van Azure Active directory.

**2. Sleutelkluis** aangevraagd voor een geheim dat is gekoppeld aan het aangevraagde token.

**3. Azure rollen valideren van de aanvraag en autoriseren toegangsaanvraag voor de Sleutelkluis.

**4. Sleutelkluis** retourneert het geheim in dit geval de SQL DB-verbindingsreeks.

**5. Azure-functie** maakt gebruik van de verbindingsreeks veilig verbinding kunnen maken met SQL-Database en verdere verwerking voor het opslaan van gegevens ePHI blijft.

Voor de opslag van de gegevens, is een algemene API-schema geïmplementeerd snel gezondheidszorg interoperabiliteit Resources (FHIR, fire verergerd) te volgen. De functie is de volgende FHIR exchange-elementen worden opgegeven:

-   [Patiënt schema](https://www.hl7.org/fhir/patient.html) bevat informatie over het 'wie' informatie over een geduld.

-   [Observatie schema](https://www.hl7.org/fhir/observation.html) bevat informatie over het centrale element in de gezondheidszorg, gebruikt voor het ondersteunen van diagnose, voortgang, basislijnen en patronen te bepalen en zelfs vastleggen demografische kenmerken. 

-   [Schema optreden](https://www.hl7.org/fhir/encounter.html) behandelt de soorten tegenkomt zoals ambulant, noodgevallen, status, opneming en virtuele tegenkomt thuis.

-   [Voorwaarde schema](https://www.hl7.org/fhir/condition.html) bevat gedetailleerde informatie over een voorwaarde, probleem, diagnose, of andere gebeurtenis, situatie, probleem of klinische concept dat is toegenomen tot een niveau van belang.  



### <a name="event-grid"></a>Event Grid


De oplossing biedt ondersteuning voor Azure Event raster één service voor het beheren van routering van alle gebeurtenissen uit elke bron naar een bestemming, mits:

-   [Beveiliging en verificatie](/azure/event-grid/security-authentication)

-   [Toegangsbeheer op basis van rollen](/azure/event-grid/security-authentication#management-access-control) voor verschillende bewerkingen zoals aanbieding abonnementen, een nieuwe te maken en genereren van sleutels

-   Controleren

## <a name="store"></a>STORE 

### <a name="sql-database-and-server"></a>SQL Database en Server 


-   [Transparent Data Encryption (TDE)](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) biedt realtime versleuteling en ontsleuteling van gegevens die zijn opgeslagen in de Azure SQL Database, met een sleutel die is opgeslagen in Azure Sleutelkluis.

-   [Controle op beveiligingslekken SQL](https://docs.microsoft.com/azure/sql-database/sql-vulnerability-assessment) is een eenvoudig te configureren die u kunt detecteren, bijhouden en herstellen van mogelijke beveiligingsproblemen van de database.

-   [Detectie van SQL Database dreigingen](/azure/sql-database/sql-database-threat-detection) ingeschakeld.

-   [SQL Database Auditing](/azure/sql-database/sql-database-auditing) ingeschakeld.

-   [SQL-Database metrische gegevens en diagnostische logboekregistratie](/azure/sql-database/sql-database-metrics-diag-logging) ingeschakeld.

-   [Server - en databaseniveau firewall-regels](/azure/sql-database/sql-database-firewall-configure) hebben is strengere.

-   [Altijd versleutelde kolommen](/azure/sql-database/sql-database-always-encrypted-azure-key-vault) worden gebruikt voor het beveiligen van patiënt, middelste, voor- en achternamen.
    Daarnaast gebruikt de versleuteling van de kolom database ook Azure Active Directory (AD) om te verifiëren van de toepassing met Azure SQL Database.

-   Toegang tot de SQL-Database en SQL Server is geconfigureerd volgens het principe van minimale bevoegdheden.

-   Alleen vereiste IP-adressen mogen toegang via de SQL-firewall.

### <a name="storage-accounts"></a>Opslagaccounts


-   [Gegevens in beweging worden overgedragen met behulp van TLS/SSL alleen](/azure/storage/common/storage-require-secure-transfer?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json).

-   Anonieme toegang is niet toegestaan voor containers.

-   Regels voor waarschuwingen zijn geconfigureerd voor anonieme activiteiten bijhouden.

-   HTTPS is vereist voor toegang tot resources voor storage-account.

-   Verificatiegegevens van de aanvraag wordt geregistreerd en bewaakt.

-   Gegevens in Blob storage in rust versleuteld.

## <a name="analyze"></a>ANALYSEREN

### <a name="machine-learning"></a>Machine Learning


-   [Logboekregistratie is ingeschakeld](/azure/machine-learning/studio/web-services-logging) voor Machine Learning-webservices.
- met behulp van [Machine Learning](/azure/machine-learning/desktop-workbench/experimentation-service-configuration) workbench vereist de ontwikkeling van experimenten, die de mogelijkheid om te voorspellen op een set oplossing biedt. [Integratie van de workbench](/azure/machine-learning/desktop-workbench/using-git-ml-project) beheer van experimenten kan stroomlijnen.

## <a name="security"></a>BEVEILIGING

### <a name="azure-security-center"></a>Azure Security Center
- [Azure Security Center](https://azure.microsoft.com/services/security-center/) biedt een gecentraliseerde weergave van de beveiligingsstatus van alle Azure-resources. In een oogopslag kunt u controleren dat de juiste beveiligingscontroles zijn geïnstalleerd en correct geconfigureerd en u alle bronnen die aandacht vereisen snel kan identificeren. 

- [Azure Advisor](/azure/advisor/advisor-overview) is een persoonlijke cloud-consultant waarmee u Volg de aanbevolen procedures voor het optimaliseren van uw Azure-implementaties. Het analyseert uw resourceconfiguratie en gebruikstelemetrie, en raadt vervolgens oplossingen aan die u kunnen helpen de kosteneffectiviteit, prestaties, hoge beschikbaarheid en beveiliging van uw Azure-resources te verbeteren.

### <a name="application-insights"></a>Application Insights
- [Application Insights](/azure/application-insights/app-insights-overview) is een uitbreidbaar Management APM (Application Performance)-service voor webontwikkelaars op meerdere platforms. Hiermee kunt u uw livewebtoepassing controleren. Er worden afwijkingen gedetecteerd. De service bevat krachtige analysehulpmiddelen om u te helpen bij het vaststellen van problemen en te begrijpen wat gebruikers daadwerkelijk doen met uw app. Het is bedoeld om u te helpen de prestaties en bruikbaarheid continu te verbeteren.

### <a name="azure-alerts"></a>Azure-waarschuwingen
- [Meldingen bieden een methode voor het Azure-services bewaken en kunnen u voorwaarden configureren via de gegevens. Waarschuwingen bieden ook meldingen wanneer een meldingsvoorwaarde overeenkomt met de bewakingsgegevens.

### <a name="operations-management-suite-oms"></a>Operations Management Suite (OMS)
[Operations Management Suite (ook wel bekend als OMS)](/azure/operations-management-suite/operations-management-suite-overview) is een verzameling met beheerservices.

-   Werkruimte is ingeschakeld voor Security Center

-   Werkruimte is voor bewaking van werkbelasting ingeschakeld

-   Bewaking van de werkbelasting is ingeschakeld voor:

    -   Identiteit en toegang

    -   Beveiliging en controle

    -   Azure SQL DB Analytics

    -   [Azure WebApp Analytics](/azure/log-analytics/log-analytics-azure-web-apps-analytics) oplossing

    -   Key Vault-analyse

    -   Tracering wijzigen

-   [Application Insights-Connector (Preview)](/azure/log-analytics/log-analytics-app-insights-connector) is ingeschakeld

-   [Activiteit logboekanalyse](/azure/log-analytics/log-analytics-activity) is ingeschakeld
