---
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 11/09/2018
ms.author: sngun
ms.openlocfilehash: 99dddd86c9348c9791d3012b382298bb020e63c9
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/12/2018
ms.locfileid: "51572046"
---
**1. Hoe wordt klanten worden op de hoogte van de retiring SDK?**

Microsoft biedt 12 maanden voorafgaande melding die aan het einde van ondersteuning van de retiring SDK om te faciliteren van een soepele overgang naar een ondersteunde SDK. Verder, klanten, worden geïnformeerd via verschillende communicatie kanalen: Azure-beheerportal, Developer Center, blogbericht, en rechtstreekse communicatie met toegewezen servicebeheerders.

**2. Kunnen klanten schrijven voor toepassingen die gebruikmaken van een 'die u wilt herstellen' buiten gebruik gestelde Azure Cosmos DB SDK tijdens de periode van 12 maanden?** 

Ja, klanten hebben volledige toegang voor het ontwerpen, implementeren en wijzig toepassingen die met behulp van de 'die u wilt herstellen' buiten gebruik gestelde Azure Cosmos DB SDK tijdens de respijtperiode van 12 maanden. Tijdens de respijtperiode van 12 maanden wordt klanten aangeraden om te migreren naar een nieuwere ondersteunde versie van Azure Cosmos DB SDK waar nodig.

**3. Kunnen klanten maken en wijzigen van toepassingen die gebruikmaken van een buiten gebruik gestelde Azure Cosmos DB SDK na de periode van 12 maanden melding?**

Na de periode van 12 maanden melding, wordt de SDK beëindigd. Geen toegang tot Azure Cosmos DB door een toepassing met behulp van een buiten gebruik gestelde SDK worden niet toegestaan door het Azure Cosmos DB-platform. Microsoft wordt bovendien niet om klantenondersteuning te bieden over het buiten gebruik gestelde SDK.

**4. Wat gebeurt er aan de klant het uitvoeren van toepassingen die van niet-ondersteunde versie van Azure Cosmos DB SDK gebruikmaken?**

Pogingen die verbinding maken met de Azure Cosmos DB-service met een verouderde versie van de SDK worden geweigerd. 

**5. Nieuwe functies en functionaliteit worden toegepast op alle SDK's voor niet buiten gebruik worden gesteld?**

Nieuwe functies en functionaliteit wordt alleen op nieuwe versies worden toegevoegd. Als u een oude, niet buiten gebruik gesteld, versie van de SDK uw aanvragen voor Azure Cosmos DB wordt nog steeds als de vorige functie, maar geen toegang tot alle nieuwe mogelijkheden.  

**6. Wat moet ik doen als ik mijn toepassing voordat u een datum kan niet bijwerken?**

Het is raadzaam dat u een naar de nieuwste SDK zo vroeg mogelijk upgrade. Nadat een SDK is gemarkeerd voor buiten gebruik stellen hebt u 12 maanden om bij te werken van uw toepassing. Als u om welke reden dan ook, u niet kunt voltooien van de update van uw toepassing binnen deze periode en vervolgens Neem contact op met de [Cosmos DB-Team](mailto:askcosmosdb@microsoft.com) en hun hulp voordat de afsluitdatum.

