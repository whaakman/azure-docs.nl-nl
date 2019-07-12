---
title: Wat is Azure gegevens delen Preview
description: Een overzicht van voorbeeld van Azure gegevens delen
author: joannapea
ms.service: data-share
ms.topic: overview
ms.date: 07/10/2019
ms.author: joanpo
ms.openlocfilehash: 7d4e51ec9564bfb123cf73d9fe89d040f42fe650
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807544"
---
# <a name="what-is-azure-data-share-preview"></a>Wat is Azure Data Share Preview?

Gegevens worden weergegeven in de wereld van vandaag, als belangrijkste strategische activa die veel organisaties moeten eenvoudig en veilig delen met hun klanten en partners. Er zijn veel manieren klanten doen dit vandaag, inclusief via FTP-, e-mail, API's om een paar te noemen. Organisaties kunnen eenvoudig verloren gaan bijhouden die ze hun gegevens mee hebt gedeeld. Delen van gegevens via FTP of via het handhaven van de infrastructuur van hun eigen API is vaak duur voor het inrichten en beheren. Er is een beheer-overhead die is gekoppeld met behulp van deze methoden op grote schaal te delen. 

Veel organisaties moeten zijn verantwoordelijk voor de gegevens die ze hebben gedeeld. Naast aansprakelijkheid willen veel organisaties mogelijk om te bepalen, beheren en controleren van al hun gegevens delen in een eenvoudige manier. Organisaties moeten in vandaag ter wereld, waar gegevens naar verwachting blijven groeien exponentieel, een eenvoudige manier voor het delen van big data. Klanten van de meest recente gegevens om ervoor te zorgen dat ze kunnen tijdig inzicht vraag.

Voorbeeld voor Azure delen van gegevens kunnen organisaties eenvoudig en veilig delen van gegevens met meerdere klanten en partners. In een paar klikken kunt u inrichten van een nieuwe gegevens delen van een account, gegevenssets toevoegen en uw klanten en partners uitnodigen voor het delen van uw gegevens. Gegevensproviders zich altijd in de controle over de gegevens die ze hebben gedeeld. Azure het delen van gegevens is het eenvoudig is om te controleren welke gegevens u is gedeeld, wanneer en door wie. 

Gegevensprovider kan behoud de controle van hoe hun gegevens worden verwerkt door gebruiksvoorwaarden voor het delen van hun gegevens op te geven. De gegevensverbruiker moet deze accepteren voordat ze kunnen de gegevens worden ontvangen. Gegevensproviders kunnen opgeven van de frequentie waarmee de gebruikers van hun gegevens ontvangen van updates. Toegang tot nieuwe updates kan op elk gewenst moment worden ingetrokken door de gegevensprovider. 

Azure het delen van gegevens kunt inzichten verbeteren zodat het eenvoudiger om gegevens van derden om te verrijken analyses en AI-scenario's te combineren. Eenvoudig power ot Azure analytics hulpmiddelen gebruiken voor het voorbereiden, verwerken en analyseren van gegevens die worden gedeeld met behulp van Azure het delen van gegevens. 

## <a name="scenarios-for-azure-data-share"></a>Scenario's voor het delen van gegevens van Azure

Azure het delen van gegevens kan worden gebruikt in een aantal verschillende branches. Detailhandel wilt bijvoorbeeld recente punt van verkoopgegevens delen met hun leveranciers. Met behulp van Azure-gegevens delen, detailhandel instellen van een punt van verkoopgegevens voor al hun leveranciers met het delen van gegevens en delen van verkoop op basis van per uur of dagelijks. 

Azure het delen van gegevens kan ook worden gebruikt om een gegevensmarkt voor een specifieke branche stand te brengen. Bijvoorbeeld, een overheid of een research-instelling die regelmatig anonieme gegevens over de groei van de populatie met derden deelt. 

Een andere use-case voor Azure het delen van gegevens is tot stand brengen van een consortium gegevens. Een aantal verschillende onderzoeksinstituten kan bijvoorbeeld gegevens delen met een enkel vertrouwde instantie. Gegevens worden geanalyseerd, samengevoegd of verwerkt met behulp van Azure analytics-hulpprogramma's en vervolgens gedeeld met belanghebbenden. 

## <a name="how-it-works"></a>Hoe werkt het?

Azure het delen van gegevens maakt gebruik van een momentopname-benadering op basis van delen, waar de gegevens verplaatst van de gegevensprovider Azure-abonnement en terechtkomt in Azure-abonnement van de gegevensverbruiker. Als een gegevensprovider inrichten van een gegevensshare en uitnodigen ontvangers voor het delen van de gegevens. Gebruikers van gegevens wordt een uitnodiging voor uw gegevens delen via e-mail. Zodra een gegevensgebruiker de uitnodiging accepteert, ze een volledige momentopname kunnen activeren van de gedeelde gegevens u ze hebt gedeeld. Deze gegevens worden naar het opslagaccount voor consumenten van gegevens ontvangen. Gegevensgebruikers kunnen normale, incrementele updates voor de gegevens die met hen worden gedeeld zodat ze altijd de meest recente versie van de gegevens hebben ontvangen. 

Gegevensproviders kunnen hun gegevens bieden consumenten incrementele updates voor de gegevens die met hen worden gedeeld via een momentopnameschema. Momentopname van schema's worden aangeboden op elk uur of dagelijks. Wanneer een gegevensgebruiker accepteert en het delen van hun gegevens configureert, kunnen ze zich abonneren op de planning van een momentopname. Dit is nuttig in scenario's waar de gedeelde gegevens worden regelmatig bijgewerkt en de gegevensverbruiker moet de meest recente gegevens. 

![stroom van gegevens delen](media/data-share-flow.png)

Wanneer een gegevensgebruiker een delen van gegevens accepteert, zijn ze kunnen ontvangen van de gegevens in een storage-account van hun keuze. Bijvoorbeeld, als de gegevensprovider gegevens met behulp van Azure Blob-opslag deelt, kan de gegevensverbruiker deze gegevens ontvangen in Azure Data Lake Store. 

## <a name="key-capabilities"></a>Belangrijkste mogelijkheden

Azure het delen van gegevens kunnen gegevensproviders:

* Delen van gegevens uit Azure Storage en Azure Data Lake Store met klanten en partners buiten uw organisatie

* Bijhouden met wie u uw gegevens met hebt gedeeld

* Hoe vaak de gebruikers van uw gegevens worden ontvangen van updates voor uw gegevens

* Uw klanten om op te halen van de meest recente versie van uw gegevens naar behoefte toe te staan ze ontvangen automatisch incrementele wijzigingen in uw gegevens met tussenpozen die door u of

Azure het delen van gegevens kunt gebruikers van de gegevens op: 

* Een beschrijving van het type gegevens dat wordt gedeeld weergeven

* Gebruiksvoorwaarden voor de gegevens bekijken

* Accepteren of weigeren van een uitnodiging Azure het delen van gegevens

* Activeer een volledige of incrementele momentopname van een delen van gegevens die een organisatie met u heeft gedeeld

* Abonneren op een Share van de gegevens voor het ontvangen van de meest recente versie van de gegevens via momentopname van de incrementele kopie

* Acceptatie van gegevens naar een Azure Blob Storage of Azure Data Lake Gen2-account met u gedeeld

Alle bovenstaande belangrijke functies worden ondersteund via de Azure of via REST API's. Bekijk onze documentatie voor meer informatie over het gebruik van Azure het delen van gegevens via REST API's. 

## <a name="security"></a>Beveiliging

Azure het delen van gegevens maakt gebruik van de onderliggende beveiliging die Azure biedt om gegevens in rust en doorvoer te beveiligen. Gegevens in rust worden versleuteld, waar dit wordt ondersteund door de onderliggende opslagmechanisme. Gegevens worden ook versleuteld tijdens de overdracht. Metagegevens over het delen van een gegevens zijn eveneens versleuteld in rust en onderweg zijn. 

Besturingselementen voor toegang kunnen worden ingesteld op het niveau van delen van gegevens van Azure-resource om te controleren of dat deze is toegankelijk door degenen die zijn gemachtigd. 

Azure het delen van gegevens maakt gebruik van beheerde identiteiten voor Azure-Resources (voorheen bekend als MSI-bestanden) voor automatische identiteitsbeheer in Azure Active Directory. Beheerde identiteiten voor Azure-Resources worden gebruikt voor toegang tot de opslagaccounts die worden gebruikt voor het delen van gegevens. Er is geen exchange van referenties tussen een gegevensprovider en een gegevensverbruiker. Raadpleeg voor meer informatie de [beheerde identiteiten voor een Azure-Resources pagina](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/services-support-managed-identities). 

## <a name="supported-regions"></a>Ondersteunde regio’s

Voor een lijst van Azure-regio's die beschikbaar maken voor Azure het delen van gegevens, raadpleegt u de [producten beschikbaar per regio](https://azure.microsoft.com/global-infrastructure/services/) pagina en zoek naar Azure het delen van gegevens. 

De gegevens zelf worden niet opgeslagen door Azure het delen van gegevens. De gegevens worden opgeslagen in de onderliggende opslagaccounts die worden gedeeld. Bijvoorbeeld, als hun gegevens door een producent gegevens worden opgeslagen in een Azure Data Lake Store-account zich in VS-West, is dat waar de gegevens worden opgeslagen. Als deze gegevens met een Azure Storage-account zich bevindt in West-Europa deelt, worden de gegevens overgedragen rechtstreeks naar de Azure Storage-account zich bevindt in West-Europa. 

Het delen van gegevens van Azure-service heeft geen beschikbaar zijn in uw regio gebruikmaken van de service. Bijvoorbeeld, als u gegevens die zijn opgeslagen in een Azure Storage-account zich in een regio waarin Azure-gegevens delen nog niet beschikbaar is hebt, kunt u nog steeds gebruikmaken van de service om uw gegevens te delen. 

## <a name="next-steps"></a>Volgende stappen

Als u wilt meer informatie over het delen van gegevens, blijven de [delen van uw gegevens](share-your-data.md) zelfstudie.
