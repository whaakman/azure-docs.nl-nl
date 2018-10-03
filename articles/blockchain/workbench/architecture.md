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

Azure Blockchain Workbench vereenvoudigt de ontwikkeling van blockchain-toepassingen door op te geven van een oplossing met behulp van verschillende Azure-onderdelen. Blockchain Workbench kan worden geïmplementeerd met behulp van een oplossingssjabloon in de Azure Marketplace. De sjabloon kunt u kiezen modules en -onderdelen implementeren met inbegrip van blockchain-stack, type van de clienttoepassing, en ondersteuning bieden voor IoT-integratie. Zodra geïmplementeerd, biedt Blockchain Workbench toegang tot een web-app, iOS-app, en Android-app.

![Blockchain Workbench-architectuur](./media/architecture/architecture.png)

## <a name="identity-and-authentication"></a>Identiteit en verificatie

Blockchain Workbench gebruiken, kan een consortium hun ondernemings-id's met behulp van Azure Active Directory (Azure AD) federeren. Workbench genereert nieuwe gebruikersaccounts voor identiteiten in de chain met de ondernemings-id's die zijn opgeslagen in Azure AD. De Identiteitstoewijzing kunt u geverifieerde Meld u aan bij de client-API's en toepassingen en maakt gebruik van het verificatiebeleid van organisaties. Workbench biedt ook de mogelijkheid om te koppelen aan specifieke rollen binnen een bepaalde opdracht slimme ondernemings-id's. Bovendien Workbench biedt ook een mechanisme voor het identificeren van de acties die deze rollen kunt nemen en op welk tijdstip.

Nadat Blockchain Workbench is geïmplementeerd, wordt gebruikers met Blockchain Workbench via de clienttoepassingen, de client op basis van REST-API of de API voor Apparaatmessaging werken. In alle gevallen moeten interacties worden geverifieerd, via Azure Active Directory (Azure AD) of een apparaat-specifieke referenties.

Gebruikers wordt de identiteit aan een consortium Azure AD federeren door te sturen een e-mailuitnodiging voor deelnemers aan het e-mailadres. Wanneer u zich aanmeldt, kan deze gebruikers worden geverifieerd met de naam, wachtwoord en het beleid. Bijvoorbeeld: verificatie met twee factoren van hun organisatie.

Azure AD wordt gebruikt voor het beheren van alle gebruikers die toegang tot Blockchain Workbench hebben. Elk apparaat verbinding maakt met een slimme contract is ook gekoppeld aan Azure AD.

Azure AD wordt ook gebruikt om gebruikers toewijzen aan een groep speciale beheerdersmachtigingen. Gebruikers die zijn gekoppeld aan de groep Administrators krijgen toegang tot de rechten en acties binnen Blockchain Workbench, met inbegrip van de implementatie van contracten en machtigingen verlenen aan een gebruiker voor toegang tot een contract. Gebruikers buiten deze groep geen toegang tot beheerdersacties.

## <a name="client-applications"></a>Client-toepassingen

Workbench biedt toepassingen voor web en mobiel (iOS, Android), die kan worden gebruikt om te valideren, testen en weergeven van blockchain-toepassingen automatisch gegenereerde client. De toepassingsinterface wordt dynamisch gegenereerd op basis van slimme contract metagegevens en kan worden aangepast aan elk use-casescenario. De clienttoepassingen leveren een gebruikersgerichte front-end voor de volledige blockchaintoepassingen die worden gegenereerd door Blockchain Workbench. Clienttoepassingen gebruikers via Azure Active Directory (Azure AD) worden gebruikt en een ervaring die zijn afgestemd op de zakelijke context van de slimme contract. De gebruikerservaring wordt het maken van nieuwe slimme contract instanties door gemachtigde personen en vervolgens wordt de mogelijkheid om uit te voeren op de juiste plaatsen van bepaalde transactietypen in het bedrijfsproces dat Hiermee geeft u de slimme contract.

In de webtoepassing gemachtigde gebruikers toegang krijgen tot de Administrator-Console. De console is beschikbaar voor gebruikers in de groep Administrators in Azure AD en biedt toegang tot de volgende functionaliteit:

* Slimme contracten van Microsoft voor populaire scenario's implementeren. Bijvoorbeeld: een asset overdracht scenario.
* Uploaden en implementeren van hun eigen slimme contracten.
* Een gebruikerstoegang tot de slimme overeenkomst in de context van een specifieke rol toewijzen.

## <a name="gateway-service-api"></a>Service-API voor gateway

Blockchain Workbench bevat een gateway op basis van REST-API. Bij het schrijven naar een blockchain, wordt de API genereert en levert berichten aan een gebeurtenis-broker. Wanneer gegevens worden aangevraagd door de API, worden query's verzonden naar de buiten de chain SQL-database. De SQL-database bevat een replica van gegevens in de chain en metagegevens die context en de configuratie-informatie over ondersteunde slimme contracten. De vereiste gegevens-query's retourneren van de replica buiten de chain in een indeling die door de metagegevens voor de opdracht geïnformeerd.

Ontwikkelaars hebben toegang tot de service-API voor gateway als u wilt maken of integreren van blockchain-oplossingen zonder afhankelijkheid van Blockchain Workbench client-apps.

> [!NOTE]
> Twee clienttoepassingen zijn voor geverifieerde toegang tot de API, geregistreerd in Azure Active Directory. Azure Active Directory vereist verschillende toepassingsregistraties elk toepassingstype (systeemeigen en web). 

## <a name="message-broker-for-incoming-messages"></a>Berichtenbroker voor inkomende berichten

Ontwikkelaars die willen verzenden van berichten rechtstreeks naar Blockchain Workbench kunnen versturen berichten rechtstreeks aan Service Bus. Berichten-API kan bijvoorbeeld worden gebruikt voor de integratie van systeem of IoT-apparaten.

## <a name="message-broker-for-downstream-consumers"></a>Berichtenbroker voor downstream consumenten

Tijdens de levenscyclus van de toepassing zich gebeurtenissen voordoen. Gebeurtenissen kunnen worden geactiveerd door de API-Gateway of op het grootboek. Meldingen van gebeurtenissen kunnen downstream code op basis van de gebeurtenis te starten.

Blockchain Workbench implementeert automatisch twee soorten consumenten van gebeurtenissen. Een consument wordt geactiveerd door gebeurtenissen voor het vullen van de SQL-archief van buiten de chain blockchain. Een andere consument zich om vast te leggen van de metagegevens voor gebeurtenissen die worden gegenereerd door de API met betrekking tot het uploaden en opslaan van documenten.

## <a name="message-consumers"></a>Gebruikers van berichten

 Gebruikers van berichten worden berichten van Service Bus. Het onderliggende model eventing voor gebruikers van berichten kan voor uitbreidingen van aanvullende services en systemen. Bijvoorbeeld, kan u ondersteuning voor het vullen CosmosDB of berichten met behulp van Azure stream Analytics evalueren toevoegen. De volgende secties beschrijven de gebruikers van berichten in Blockchain Workbench opgenomen.

### <a name="distributed-ledger-consumer"></a>De consument gedistribueerd grootboek

Gedistribueerd grootboek-technologie (DLT) berichten bevatten de metagegevens voor transacties die moeten worden geschreven naar de blockchain. De consument de berichten opgehaald en de gegevens naar een transactie builder, ondertekend door en router gepusht.

### <a name="database-consumer"></a>Database-consument

De consument database gebruikt berichten van Service Bus en de gegevens naar een gekoppelde database, zoals SQL-database worden gepusht.

### <a name="storage-consumer"></a>Opslag-consument

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