---
title: Klantgegevens functies in Azure Time Series Insights aanvragen
description: Samenvatting van de klant aanvraag gegevensfuncties.
author: dominicbetts
ms.author: dobett
manager: timlt
ms.date: 05/17/2018
ms.topic: conceptual
ms.service: time-series-insights
services: time-series-insights
ms.openlocfilehash: fbbb203d4ad0568a061043317e0444d2551d1aef
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34652131"
---
# <a name="summary-of-customer-data-request-features"></a>Overzicht van de klant aanvraag gegevensfuncties

Azure Time Series Insights is een beheerde cloudservice met opslag, analytics en visualisatie onderdelen die eenvoudig kunt opnemen, opslaan, verkennen en analyseren van gebeurtenissen miljarden tegelijkertijd.

[!INCLUDE [gdpr-intro-sentence](../../includes/gdpr-intro-sentence.md)]

Als u wilt weergeven, exporteren en verwijderen van persoonlijke gegevens die mogelijk onderworpen aan een aanvraag voor onderwerp, kunt een tenantbeheerder Azure Time Series Insights de Azure portal of de REST API's gebruiken. Met de Azure-portal serviceaanvragen gegevens onderwerp biedt een minder complexe methode voor het uitvoeren van deze bewerkingen die de meeste gebruikers kiezen.

## <a name="identifying-customer-data"></a>Identificeren van klantgegevens

Inzicht van Azure Time Series beschouwt persoonlijke gegevens worden gegevens die zijn gekoppeld aan beheerders en gebruikers van Time Series inzicht. Time Series Insights slaat de Azure Active Directory-object-ID van gebruikers met toegang tot de omgeving. De Azure-portal wordt weergegeven voor gebruiker e-mailadressen, maar deze e-mailadressen worden niet opgeslagen in het Time Series Insights, ze dynamisch worden opgezocht aan de hand van de Azure Active Directory-object-ID in Azure Active Directory.

## <a name="deleting-customer-data"></a>Verwijderen van klantgegevens

Een tenantbeheerder kan gegevens van de klant met de Azure portal kunt verwijderen.

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

Voordat u gegevens via de portal van de klant verwijderen, moet u het toegangsbeleid van de gebruiker verwijderen uit de omgeving Time Series inzicht in de Azure-portal. Zie voor meer informatie [gegevenstoegang verlenen tot een tijd reeks Insights-omgeving met Azure portal](time-series-insights-data-access.md).

U kunt ook de delete-bewerkingen op met de REST API toegangsbeleid uitvoeren. Zie voor meer informatie [verwijderen van toegangsbeleid -](https://docs.microsoft.com/rest/api/time-series-insights-management/accesspolicies/delete).

Time Series Insights is geïntegreerd met de blade beleid in de Azure portal. Time Series Insights zowel de blade beleid kunnen u bekijken, exporteren en verwijderen van de gebruikersgegevens zijn opgeslagen in de service. Een verwijderen actie op die binnen de blade beleid van de Azure portal leidt tot het verwijderen van de gebruikersgegevens binnen de Time Series inzicht. Bijvoorbeeld als een gebruiker een opgeslagen query voor persoonlijke heeft, is die query permanent verwijderd uit de Time Series Insights explorer. Als de gebruiker een opgeslagen gedeelde query heeft, wordt de query blijft bestaan, maar de gebruikersinformatie wordt definitief verwijderd. De volgende opmerking bevat instructies voor het uitvoeren van deze taken.

## <a name="exporting-customer-data"></a>Exporteren van klantgegevens

Op dezelfde manier voor het verwijderen van gegevens kan een tenantbeheerder weergeven en exporteren van gegevens die zijn opgeslagen in Time Series-inzichten op de blade beleid in de Azure portal.

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

Als u een tenantbeheerder bent, kunt u gegevenstoegangsbeleid binnen de Time Series Insights-omgeving kunt weergeven in de Azure portal. Zie voor meer informatie [gegevenstoegang verlenen tot een tijd reeks Insights-omgeving met Azure portal](time-series-insights-data-access.md).

Het is ook mogelijk om te exporteren bewerkingen uitvoeren op beleidsregels toegang met behulp van de bewerking 'lijst door omgeving' in de opgegeven REST-API. Zie voor meer informatie [toegangsbeleid - lijst door omgeving](https://docs.microsoft.com/rest/api/time-series-insights-management/accesspolicies/listbyenvironment).

## <a name="to-delete-data-stored-within-time-series-insights"></a>Gegevens die zijn opgeslagen binnen de Time Series Insights verwijderen

Persoonlijke gegevens kunnen eraan maken in de Time Series Insights opslag, een ander scenario van gebruiker en gegevens van de beheerder. Als u overweegt de gegevens die zijn opgeslagen in het Time Series Insights als persoonlijke gegevens, kunt u deze kunt exporteren en verwijderen van die gegevens met behulp van de volgende stappen uit:

**Weergeven en exporteren van gegevens**

Als u wilt weergeven en exporteren van gegevens die zijn opgeslagen binnen de Time Series inzichten, moet u zoeken naar die gegevens. U kunt de Time Series Insights explorer of de Time Series Insights query API's weergeven en exporteren van gegevens. Als u wilt weergeven en exporteren van gegevens met behulp van de Time Series Insights explorer, eerst zoeken vinden de gebruikersgegevens in kwestie. Na het zoeken, met de rechtermuisknop op de grafiek en selecteer **verkennen gebeurtenissen**. Het raster gebeurtenissen wordt weergegeven en opties voor het exporteren van de gegevens als CSV en JSON geeft.

Zie voor meer informatie [Azure Time Series Insights explorer](time-series-insights-explorer.md).

**Gegevens verwijderen**

Time Series Insights biedt momenteel geen ondersteuning voor gedetailleerde verwijderen van gegevens. Time Series Insights biedt echter de mogelijkheid om te verwijderen van klantgegevens opgeslagen binnen de Time Series Insights door bewaarbeleid te configureren. U kunt de bewaarperiode van de gehele Time Series Insights-omgeving naar een willekeurig aantal dagen voor de ondersteuning van uw vereisten verwijdering aanpassen.

Zie voor meer informatie [configuratie van de bewaartermijn in tijd reeks inzichten](time-series-insights-how-to-configure-retention.md).