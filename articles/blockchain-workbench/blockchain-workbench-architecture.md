---
title: Azure Blockchain Workbench-architectuur
description: Overzicht van Azure Blockchain Workbench architectuur en de bijbehorende onderdelen.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 4/20/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: a6c415123a8e1254e214af69711fc4143a929a58
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
---
# <a name="azure-blockchain-workbench-architecture"></a>Azure Blockchain Workbench-architectuur

Azure Blockchain Workbench vereenvoudigt blockchain toepassingsontwikkeling door een oplossing met meerdere Azure-onderdelen. Blockchain Workbench kan worden geïmplementeerd met behulp van een oplossingssjabloon in Azure Marketplace. De sjabloon kan gebruikers de modules en -onderdelen met Blockchain Workbench, zoals blockchain stack, type clienttoepassing, implementeren en ondersteuning bieden voor IoT-integratie. Zodra geïmplementeerd, biedt Blockchain Workbench toegang tot een web-app, iOS-app en Android-app.

![Blockchain Workbench-architectuur](media/blockchain-workbench-architecture/architecture.png)

## <a name="identity-and-authentication"></a>Identiteit en verificatie

Blockchain Workbench, een consortium, kan communiceren de enterprise-identiteiten met Azure Active Directory (Azure AD). Workbench genereert nieuwe gebruikersaccounts voor voor keten identiteiten met de enterprise-identiteiten opgeslagen in Azure AD. De Identiteitstoewijzing vereenvoudigt geverifieerde Meld u aan bij de client-API's en toepassingen en maakt gebruik van het verificatiebeleid van organisaties. Workbench biedt ook de mogelijkheid om te koppelen bedrijfsentiteiten tot specifieke functies binnen een bepaalde smart opdracht. Bovendien Workbench biedt ook een mechanisme voor het identificeren van de acties die functies kunt uitvoeren en welk tijdstip.

Nadat de Blockchain Workbench is geïmplementeerd, worden gebruikers communiceren met Blockchain Workbench hetzij via de clienttoepassingen, de client op basis van REST-API of de Messaging-API. In alle gevallen moeten interacties worden geverifieerd, hetzij via Azure Active Directory (Azure AD) of apparaatspecifieke referenties.

Gebruikers federeren hun identiteit aan een consortium Azure AD via een e-uitnodiging voor deelnemers op hun e-mailadres. Wanneer u zich aanmeldt, kan deze gebruikers worden geverifieerd met de naam, het wachtwoord en het beleid. Bijvoorbeeld: verificatie met twee factoren van hun organisatie.

Azure AD wordt gebruikt voor het beheren van alle gebruikers die toegang tot Blockchain Workbench hebben. Elk apparaat verbinding maakt met een smartcard-contract is ook gekoppeld aan Azure AD.

Azure AD wordt ook gebruikt voor de gebruikers toewijzen aan een speciale beheerdersgroep. Gebruikers die zijn gekoppeld aan de beheerdersgroep krijgen beheerder rechten/acties in de Blockchain Workbench, zoals het implementeren van contracten en machtigingen geven aan een gebruiker toegang tot een contract. Gebruikers buiten deze groep hebben geen toegang tot beheerdersacties.

## <a name="client-applications"></a>Client-toepassingen

Workbench biedt een automatisch gegenereerde clienttoepassingen voor web- en mobile (iOS, Android), die kan worden gebruikt om te valideren, testen en blockchain toepassingen bekijkt. De toepassingsinterface wordt dynamisch gegenereerd op basis van metagegevens van de smartcard contract en een gebruiksvoorbeeld aankan. De clienttoepassingen bieden een gebruikersgerichte front-end voor de volledige blockchain toepassingen die worden gegenereerd door Blockchain Workbench. Clienttoepassingen gebruikers verifiëren via Azure Active Directory (Azure AD) en vervolgens een aangepast aan de zakelijke context van de smartcard contract gebruikerservaring aanwezig. De gebruikerservaring wordt het maken van nieuwe exemplaren van de smartcard contract door bevoegde personen en geeft vervolgens de mogelijkheid om uit te voeren op de juiste plaatsen van bepaalde transactietypen in het bedrijfsproces dat Hiermee geeft u de smart contract.

Gemachtigde gebruikers hebben toegang tot de Administrator-Console in de webtoepassing. De console is beschikbaar voor gebruikers in de groep Administrators in Azure AD en biedt toegang tot de volgende functionaliteit:

* Smart contracten van Microsoft voor populaire scenario's implementeren. Bijvoorbeeld, een asset overdracht scenario.
* Uploaden en implementeren van hun eigen smart contracten.
* Een gebruikerstoegang tot het smart contract in de context van een specifieke rol toewijzen.

## <a name="gateway-service-api"></a>API voor gateway-service

Blockchain Workbench bevat een API van de service op basis van de REST-gateway. Bij het schrijven naar een blockchain, de API wordt gegenereerd en levert berichten voor een gebeurtenis broker. Wanneer gegevens worden aangevraagd door de API, worden query's verzonden naar de SQL-database uit de keten. De SQL-database bevat een replica van de gegevens in de keten en metagegevens die context en configuratie-informatie voor ondersteunde smart contracten biedt. Query's kunt u de vereiste gegevens van de replica uit keten in een indeling die door de metagegevens voor het contract in kennis gesteld geretourneerd.

Ontwikkelaars hebben toegang tot de API van de gateway-service om te bouwen of integreren blockchain oplossingen zonder afhankelijk te zijn van de client-apps Blockchain Workbench.

> [!NOTE]
> Voor geverifieerde toegang tot de API, worden twee clienttoepassingen geregistreerd in Azure Active Directory. Azure Active Directory vereist afzonderlijke toepassing registraties van elk toepassingstype (systeemeigen en web). 

## <a name="message-broker-for-incoming-messages"></a>Bericht-broker voor inkomende berichten

Ontwikkelaars die willen berichten rechtstreeks naar de Blockchain Workbench verzenden kunnen berichten rechtstreeks naar Service Bus verzenden. Berichten API kan bijvoorbeeld worden gebruikt voor de integratie van systeem of IoT-apparaten.

## <a name="message-broker-for-downstream-consumers"></a>Bericht-broker voor downstream consumenten

Tijdens de levenscyclus van de toepassing plaatsvinden gebeurtenissen. Gebeurtenissen kunnen worden geactiveerd door de API-Gateway of op het grootboek. Gebeurtenismeldingen downstream code op basis van de gebeurtenis tot stand kunnen brengen.

Blockchain Workbench implementeert automatisch twee soorten gebeurtenisconsumers. Een wordt geactiveerd door gebeurtenissen, blockchain voor het vullen van de SQL-archief uit de keten. Het andere type is voor het vastleggen van metagegevens voor gebeurtenissen die worden gegenereerd door de API die betrekking hebben op het uploaden en de opslag van documenten.

## <a name="message-consumers"></a>Bericht consumenten

 Bericht consumenten nemen berichten uit de Service Bus. Het onderliggende model eventing voor consumenten bericht kunt u uitbreidingen van extra services en systemen. U kan bijvoorbeeld ondersteuning voor het vullen CosmosDB of berichten met Azure Streaming Analytics evalueren toevoegen. De volgende secties beschrijven de bericht-consumenten opgenomen in de Blockchain Workbench.

### <a name="distributed-ledger-consumer"></a>Gedistribueerde grootboek consumer

Gedistribueerde grootboek-technologie (DLT) berichten bevatten de metagegevens voor transacties naar de blockchain moeten worden geschreven. De consument de berichten worden opgehaald en duwt de gegevens voor een transactie builder, ondertekenaar en -router.

### <a name="database-consumer"></a>Database-consumer

De consument database neemt berichten uit de Service Bus en duwt de gegevens naar een gekoppelde database, zoals SQL-database.

### <a name="storage-consumer"></a>Opslag consumer

De consument opslag duurt berichten uit de Service Bus en gegevens verstuurd naar een gekoppelde opslag. Bijvoorbeeld, opslaan hash documenten in Azure Storage.

## <a name="transaction-builder-and-signer"></a>Transactie builder en ondertekenaar

Als een bericht op de broker binnenkomend bericht worden geschreven naar de blockchain moet, wordt door de consument DLT worden verwerkt. De consument DLT is een service, die haalt het bericht met metagegevens voor een gewenste transactie worden uitgevoerd en stuurt de gegevens naar de *transactie builder en ondertekenaar*. De *transactie builder en ondertekenaar* ophaalprotocol blockchain transactie op basis van de gegevens en de gewenste blockchain bestemming. Zodra verzameld, wordt de transactie is ondertekend. Persoonlijke sleutels worden opgeslagen in Azure Sleutelkluis.

 Blockchain Workbench haalt de juiste persoonlijke sleutel van de Sleutelkluis en de transactie buiten Sleutelkluis ondertekent. Eenmaal is ondertekend, wordt de transactie verzonden naar transactie routers en posten.

## <a name="transaction-routers-and-ledgers"></a>Transactie-routers en posten

Transactie-routers en posten ondertekende transacties nemen en deze doorsturen naar de juiste blockchain. Op dit moment ondersteunt Blockchain Workbench Ethereum als de doel-blockchain.

## <a name="dlt-watcher"></a>DLT-watcher

Een gedistribueerde grootboek-technologie (DLT)-servicewatcher bewaakt gebeurtenissen plaatsvinden op blok ketens gekoppeld aan Blockchain Workbench.
Gebeurtenissen geven informatie die relevant zijn voor personen en systemen. Het maken van nieuwe exemplaren van het contract, de uitvoering van transacties en wijzigingen van status. De gebeurtenissen worden vastgelegd en worden verzonden naar de broker uitgaand bericht, zodat ze kunnen worden gebruikt door downstream consumenten.

Bijvoorbeeld, de consument SQL gebeurtenissen bewaakt, verbruikt ze en vult de SQL-database met de opgenomen waarden. De kopie kunt opnieuw een replica van gegevens in een archief uit de keten voor de keten.

## <a name="azure-sql-database"></a>Azure SQL-database

De Azure SQL database gekoppeld aan Blockchain Workbench slaat contract definities, metagegevens van de configuratie en een SQL-toegankelijk replica van de gegevens die zijn opgeslagen in de blockchain. Deze gegevens kan eenvoudig worden opgevraagd, die gevisualiseerd of geanalyseerd door rechtstreeks toegang hebben tot de database. Ontwikkelaars en andere gebruikers kunnen de database gebruiken voor rapportage, analytics of andere gegevensgerichte integraties. Bijvoorbeeld: gebruikers kunnen transactiegegevens visualiseren met Power BI.

Deze uitschakelen keten opslag biedt de mogelijkheid voor ondernemingen query uitvoeren op gegevens in SQL in plaats van een blockchain-grootboek. Bovendien, door te standaardiseren op een standaard schema dat is agnostisch van blockchain technologie stacks, kan de opslag uit de keten het hergebruik van rapporten en andere onderdelen via projecten, scenario's en organisaties.

## <a name="azure-storage"></a>Azure Storage

Azure-opslag wordt gebruikt voor het opslaan van contracten en metagegevens die zijn gekoppeld aan contracten.

Van inkoop en vrachtbrieven aan installatiekopieën gebruikt in het nieuws en röntgenfoto's, om de video die afkomstig zijn van één geheel worden beschouwd inclusief politie hoofdtekst camera's en grote motion afbeeldingen, spelen documenten een rol in veel blockchain gerichte scenario's. Documenten zijn niet geschikt om rechtstreeks op de blockchain te plaatsen.

Blockchain Workbench ondersteunt de mogelijkheid documenten of andere media-inhoud met blockchain-bedrijfslogica toe te voegen. Een hash van het document of media-inhoud is opgeslagen in de blockchain en de werkelijke document of media-inhoud is opgeslagen in Azure Storage. De gekoppelde transactie-informatie is geleverd aan de broker binnenkomend bericht, verpakt, ondertekend en doorgestuurd naar de blockchain. Dit proces activeert gebeurtenissen die worden gedeeld via de broker uitgaand bericht. De SQL-database maakt gebruik van deze informatie en verzendt het naar de database later query's. Downstream-systemen kunnen ook deze gebeurtenissen om te fungeren als juiste verbruiken.

## <a name="monitoring"></a>Bewaking

Workbench biedt toepassingslogboeken met Application Insights en Azure-Monitor. Application Insights wordt gebruikt voor het opslaan van alle geregistreerde gegevens vanuit Blockchain Workbench en bevat fouten, waarschuwingen en geslaagde bewerkingen. Application Insights kunnen worden gebruikt door ontwikkelaars fouten opsporen in problemen met Blockchain Workbench. 

Monitor voor Azure bevat informatie over de status van het netwerk blockchain. 

## <a name="next-steps"></a>Volgende stappen

[Azure Blockchain Workbench implementeren](blockchain-workbench-deploy.md)