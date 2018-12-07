---
title: Azure Blockchain Workbench-architectuur
description: Overzicht van Azure Blockchain Workbench-architectuur en de bijbehorende onderdelen.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 10/1/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 91dd0f262d1a7e661a6f9e0c4974087503dde3e1
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48242492"
---
# <a name="azure-blockchain-workbench-architecture"></a>Azure Blockchain Workbench-architectuur

Azure Blockchain Workbench vereenvoudigt de ontwikkeling van blockchain-toepassingen door het aanbieden van een oplossing die gebruikmaakt van verschillende Azure-onderdelen. Blockchain Workbench kan worden geïmplementeerd met behulp van een oplossingssjabloon in de Azure Marketplace. De sjabloon stelt u in staat te kiezen voor modules en -onderdelen om te implementeren met inbegrip van een blockchain-stack, type van de clienttoepassing, en ondersteuning voor IoT-integratie. Eenmaal geïmplementeerd biedt Blockchain Workbench toegang tot een web-app, iOS-app, en Android-app.

![Blockchain Workbench-architectuur](./media/architecture/architecture.png)

## <a name="identity-and-authentication"></a>Identiteit en verificatie

Via Blockchain Workbench kan een consortium haar ondernemings-id's met behulp van Azure Active Directory (Azure AD) federeren. Workbench genereert nieuwe gebruikersaccounts voor identiteiten in de chain met de ondernemings-id's die zijn opgeslagen in Azure AD. De mapping van identiteiten zorgt voor geverifieerde aanmelding bij client-API's en -toepassingen en maakt gebruik van het verificatiebeleid van organisaties. Workbench biedt ook de mogelijkheid om bedrijfsidentiteiten te koppelen aan specifieke rollen binnen een smart contract. Workbench biedt bovendien een mechanisme om de acties te identificeren die deze rollen kunnen uitvoeren en op welk moment.

Nadat Blockchain Workbench is geïmplementeerd, werken gebruikers met Blockchain Workbench via de clienttoepassingen, een REST-gebaseerde client API of een Messaging API. In alle gevallen moeten interacties worden geverifieerd, via Azure Active Directory (Azure AD) of apparaatspecifieke referenties.

Gebruikers koppelen hun identiteit met Azure AD in een consortium door een e-mailuitnodiging naar deelnemers te sturen op hun e-mailadres. Bij het inloggen worden deze gebruikers geverifieerd met behulp van de naam, het wachtwoord en het toepasselijke beleid. Twee-factor authenticatie van hun organisatie bijvoorbeeld.

Azure AD wordt gebruikt om alle gebruikers te beheren die toegang hebben tot Blockchain Workbench. Elk apparaat dat verbinding maakt met een smart contract, is ook gekoppeld aan Azure AD.

Azure AD wordt ook gebruikt om gebruikers toe te wijzen aan een speciale beheerdersgroep. Gebruikers die aan de beheerdersgroep zijn gekoppeld krijgen toegang tot rechten en acties binnen Blockchain Workbench, waaronder het implementeren van contracten en het verlenen van rechten aan een gebruiker voor toegang tot een contract. Gebruikers buiten deze groep hebben geen toegang tot beheerdersacties.

## <a name="client-applications"></a>Client-toepassingen

Workbench biedt automatisch gegenereerde clienttoepassingen voor web en mobiel (iOS, Android), die kunnen worden gebruikt voor het valideren, testen en bekijken van blockchain-applicaties. De toepassingsinterface wordt dynamisch gegenereerd op basis van metadata van een smart contract en is geschikt voor elk gebruik. De clienttoepassingen leveren een gebruikersgericht frontend aan alle blockchain-applicaties die door Blockchain Workbench worden gegenereerd. Clienttoepassingen verifiëren gebruikers via Azure Active Directory (Azure AD) en presenteren vervolgens een gebruikerservaring die is afgestemd op de zakelijke context van het smart contract. De gebruikerservaring maakt het mogelijk nieuwe instanties van een smart contract aan te maken door geautoriseerde personen en biedt vervolgens de mogelijkheid om bepaalde soorten transacties uit te voeren op geschikte punten in het bedrijfsproces dat het smart contract vertegenwoordigt.

In de webtoepassing hebben bevoegde gebruikers toegang tot de beheerdersconsole. Deze console is beschikbaar voor gebruikers in de beheerdersgroep in Azure AD en biedt toegang tot de volgende functionaliteit:

* Implementeer door Microsoft verstrekte smarts contracts voor populaire scenario's. Bijvoorbeeld een scenario voor activaoverdracht.
* Upload en implementeer eigen smart contracts.
* Wijs gebruikerstoegang toe aan het smart contract in de context van een specifieke rol.

## <a name="gateway-service-api"></a>Gateway service API

Blockchain Workbench bevat een op REST gebaseerde Gateway service API. Bij het schrijven naar een blockchain genereert en levert de API berichten aan een event broker. Wanneer gegevens worden opgevraagd door de API, worden query's verzonden naar de off-chain SQL-database. De SQL-database bevat een replica van on-chain gegevens en metagegevens die context- en configuratie-informatie bieden voor ondersteunde smart contracts. Query's retourneren de vereiste gegevens uit de off-chainreplica in een indeling die is gebaseerd op de metagegevens voor het contract.

Ontwikkelaars hebben toegang tot de Gateway service API om blockchain-oplossingen te bouwen of te integreren zonder verplicht gebruik van Blockchain Workbench-client-apps.

> [!NOTE]
> Om geverifieerde toegang tot de API in te schakelen, worden twee clienttoepassingen geregistreerd in Azure Active Directory. Azure Active Directory vereist verschillende toepassingsregistraties voor elk type toepassing (native en web). 

## <a name="message-broker-for-incoming-messages"></a>Berichtenbroker voor inkomende berichten

Ontwikkelaars die berichten rechtstreeks naar Blockchain Workbench willen verzenden, kunnen berichten rechtstreeks naar Service Bus verzenden. De berichten-API kan bijvoorbeeld worden gebruikt voor systeem-naar-systeemintegratie of IoT-apparaten.

## <a name="message-broker-for-downstream-consumers"></a>Berichtenbroker voor downstream consumenten

Tijdens de levenscyclus van de toepassing vinden gebeurtenissen plaats. Gebeurtenissen kunnen worden geactiveerd door de Gateway API of in het grootboek. Eventmeldingen kunnen de downstream-code initiëren op basis van de gebeurtenis.

Blockchain Workbench maakt automatisch gebruik van twee soorten gebruikers van gebeurtenissen. Eén consument wordt getriggerd door blockchain-gebeurtenissen om de off-chain SQL-opslag te vullen. De andere consument moet metagegevens vastleggen voor gebeurtenissen die door de API worden gegenereerd met betrekking tot het uploaden en opslaan van documenten.

## <a name="message-consumers"></a>Afnemers van berichten

 Afnemers van berichten halen berichten op uit Service Bus. Het onderliggende eventing-model voor berichtgebruikers maakt uitbreidingen van extra diensten en systemen mogelijk. U kunt bijvoorbeeld ondersteuning toevoegen om CosmosDB te vullen of berichten te evalueren met Azure Streaming Analytics. In de volgende secties worden de berichtgebruikers die deel uitmaken van Blockchain Workbench beschreven.

### <a name="distributed-ledger-consumer"></a>De consument van een gedistribueerd grootboek

Berichten in Distributed ledger technology (DLT) bevatten de metagegevens voor transacties die naar de blockchain moeten worden geschreven. De consument haalt de berichten op en pusht de gegevens naar een transactiebuilder, ondertekenaar en router.

### <a name="database-consumer"></a>Databaseconsument

De databaseconsument neemt berichten van de servicebus en pusht de gegevens naar een gekoppelde database, zoals een SQL-database.

### <a name="storage-consumer"></a>Opslagconsument

De consument opslag gebruikt berichten van Service Bus en gegevens naar een gekoppelde storage wordt gepusht. Bijvoorbeeld, opslaan hash documenten in Azure Storage.

## <a name="transaction-builder-and-signer"></a>Transactie builder en ondertekend door

Als een bericht op het binnenkomende berichtenbroker worden geschreven naar de blockchain moet, wordt deze door de gebruiker DLT worden verwerkt. De gebruiker DLT is een service, die haalt het bericht met metagegevens voor een gewenste transactie om uit te voeren en stuurt de gegevens naar de *transactie builder en ondertekend door*. De *transactie builder en ondertekend door* ophaalprotocol een blockchain-transactie op basis van de gegevens en de gewenste blockchain-bestemming. Nadat u samengesteld, wordt de transactie is ondertekend. Persoonlijke sleutels worden opgeslagen in Azure Key Vault.

 Blockchain Workbench haalt de juiste persoonlijke sleutel uit Key Vault en de transactie buiten Key Vault zich. Eenmaal is ondertekend, wordt de transactie wordt verzonden naar transactie routers en grootboeken.

## <a name="transaction-routers-and-ledgers"></a>Transactie-routers en grootboeken

Transactie-routers en grootboeken ondertekende transacties nemen en deze doorsturen naar de juiste blockchain. Op dit moment ondersteunt Blockchain Workbench Ethereum als de doel-blockchain.

## <a name="dlt-watcher"></a>DLT watcher

Een watcher van de technologie (DLT) gedistribueerd grootboek bewaakt gebeurtenissen plaatsvinden op blok-ketens die is gekoppeld aan Blockchain Workbench.
Gebeurtenissen zijn de informatie die relevant is voor personen en systemen. Bijvoorbeeld, het maken van nieuwe instanties van het contract, uitvoeren van transacties en wijzigingen van status. De gebeurtenissen worden vastgelegd en verzonden naar de broker uitgaand multicastverkeer voor message zodat ze kunnen worden gebruikt door de downstream consumenten.

Bijvoorbeeld, de SQL-consument gebeurtenissen bewaakt, verbruikt ze en vult de SQL-database met de opgenomen waarden. De kopie kunt opnieuw kunt maken van een replica van de gegevens in de chain in een archief buiten de chain.

## <a name="azure-sql-database"></a>Azure SQL Database

De Azure SQL-database die is gekoppeld aan Blockchain Workbench contract definities, metagegevens van de configuratie en een SQL-toegankelijk replica van gegevens die zijn opgeslagen in de blockchain opgeslagen. Deze gegevens kan eenvoudig worden opgevraagd, gevisualiseerd of geanalyseerd door rechtstreeks toegang tot de database. Ontwikkelaars en andere gebruikers kunnen u de database gebruiken voor rapportage, analyse, of andere gegevensgerichte integraties. Gebruikers kunnen bijvoorbeeld transactiegegevens met behulp van Power BI visualiseren.

Deze buiten de chain-opslag biedt de mogelijkheid voor organisaties om gegevens te doorzoeken in SQL in plaats van een grootboek blockchain enterprise. Ook op een standard-schema dat staat los van blockchain-technologie-stacks standaardiseert, kan de opslag buiten de chain het hergebruik van rapporten en andere artefacten in projecten, scenario's en organisaties.

## <a name="azure-storage"></a>Azure Storage

Azure Storage wordt gebruikt voor het opslaan van contracten en metagegevens die zijn gekoppeld aan contracten.

Van inkooporders en vrachtbrieven, naar afbeeldingen die worden gebruikt in het nieuws en de medische beeldmateriaal naar de video die afkomstig zijn van een geheel worden beschouwd, waaronder politie hoofdtekst van de camera's en grote beweging afbeeldingen, spelen documenten een rol in veel blockchain gerichte scenario's. Documenten zijn niet geschikt om rechtstreeks op de blockchain te plaatsen.

Blockchain Workbench ondersteunt de mogelijkheid documenten of andere media-inhoud met blockchain-bedrijfslogica toe te voegen. Een hash van het document of media-inhoud is opgeslagen in de blockchain en de werkelijke document of media-inhoud is opgeslagen in Azure Storage. De bijbehorende transactie-informatie is geleverd aan het binnenkomende berichtenbroker, verpakt, ondertekend en doorgestuurd naar de blockchain. Dit proces wordt geactiveerd voor gebeurtenissen die worden gedeeld via de uitgaande berichtenbroker. De SQL-database gebruikt deze informatie en verzendt dit naar de database voor het later opnieuw uitvoeren van query's. Downstream-systemen kunnen ook gebruiken voor deze gebeurtenissen om te fungeren, indien van toepassing.

## <a name="monitoring"></a>Bewaking

Workbench biedt logboekregistratie van toepassingen met behulp van Application Insights en Azure Monitor. Application Insights wordt gebruikt voor het opslaan van alle geregistreerde gegevens van Blockchain Workbench en fouten, waarschuwingen en voltooide bewerkingen bevat. Application Insights kunnen worden gebruikt door ontwikkelaars om problemen met Blockchain Workbench kunt verhelpen. 

Azure Monitor biedt informatie over de status van de blockchain-netwerk. 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Azure Blockchain Workbench implementeren](../../blockchain-workbench/blockchain-workbench-deploy.md)