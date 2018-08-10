---
title: Gegevens van de klant functies in Azure Time Series Insights aanvragen
description: Overzicht van functies voor aanvraag klant.
author: ashannon7
ms.author: anshan
manager: cshankar
ms.date: 05/17/2018
ms.topic: conceptual
ms.service: time-series-insights
services: time-series-insights
ms.openlocfilehash: 4696cdaf96a73c54334f553a0affe459e3476946
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39629732"
---
# <a name="summary-of-customer-data-request-features"></a>Overzicht van de klant functies voor aanvraag

Azure Time Series Insights is een beheerde cloudservice met opslag, analyse en visualisatie waarmee u eenvoudig opnemen, opslaan, verkennen en analyseren van miljarden gebeurtenissen tegelijkertijd.

[!INCLUDE [gdpr-intro-sentence](../../includes/gdpr-intro-sentence.md)]

Als u wilt weergeven, exporteren en verwijderen van persoonlijke gegevens die zijn mogelijk onderhevig aan een aanvraag voor onderwerp, kunt een tenantbeheerder van Azure Time Series Insights Azure portal of de REST API's gebruiken. Met behulp van Azure portal data subject aanvragen, biedt een minder gecompliceerde methode voor het uitvoeren van deze bewerkingen die de meeste gebruikers liever.

## <a name="identifying-customer-data"></a>Identificeren van klantgegevens

Azure Time Series Insights beschouwt persoonlijke gegevens worden gegevens die zijn gekoppeld aan beheerders en gebruikers van Time Series Insights. Time Series Insights slaat de Azure Active Directory-object-ID in van gebruikers met toegang tot de omgeving. De Azure-portal wordt weergegeven voor gebruiker e-mailadressen, maar deze e-mailadressen zijn niet opgeslagen in Time Series Insights, ze dynamisch worden bekeken met behulp van de Azure Active Directory-object-ID in Azure Active Directory.

## <a name="deleting-customer-data"></a>Verwijderen van klantgegevens

Een tenantbeheerder kan gegevens van de klant via Azure portal verwijderen.

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

Voordat u gegevens van de klant via de portal verwijdert, moet u het toegangsbeleid van de gebruiker verwijderen uit de Time Series Insights-omgeving in Azure portal. Zie voor meer informatie, [gegevenstoegang verlenen tot een Time Series Insights-omgeving met behulp van Azure portal](time-series-insights-data-access.md).

U kunt ook de delete-bewerkingen op beleidsregels voor toegang met behulp van de REST-API uitvoeren. Zie voor meer informatie, [toegangsbeleid - verwijderen](https://docs.microsoft.com/rest/api/time-series-insights-management/accesspolicies/delete).

Time Series Insights is geïntegreerd met de beleid-blade in Azure portal. Time Series Insights zowel de blade beleid kunnen u bekijken, exporteren en verwijderen van gegevens van de gebruiker die zijn opgeslagen in de service. Een verwijderen actie op die in de blade beleid van de Azure portal leidt tot de verwijdering van gebruikersgegevens binnen de Time Series Insights. Bijvoorbeeld, als een gebruiker een opgeslagen query een persoonlijke heeft, is deze query permanent verwijderd uit de Verkenner van Time Series Insights. Als de gebruiker een opgeslagen gedeelde query heeft, wordt de query blijft bestaan, maar de gebruikersinformatie is definitief verwijderd. De opmerking hieronder bevat instructies over het uitvoeren van deze taken.

## <a name="exporting-customer-data"></a>Exporteren van gegevens van de klant

Op dezelfde manier voor het verwijderen van gegevens, kan een tenantbeheerder weergeven en exporteren van gegevens die zijn opgeslagen in Time Series Insights op de blade beleid in Azure portal.

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

Als u een tenantbeheerder bent, kunt u beleid voor gegevenstoegang binnen de Time Series Insights-omgeving bekijken in Azure portal. Zie voor meer informatie, [gegevenstoegang verlenen tot een Time Series Insights-omgeving met behulp van Azure portal](time-series-insights-data-access.md).

Het is ook mogelijk om uit te voeren van uitvoer-bewerkingen op beleidsregels voor toegang met behulp van de bewerking 'lijst met omgeving' in de opgegeven REST-API. Zie voor meer informatie, [toegangsbeleid - lijst door omgeving](https://docs.microsoft.com/rest/api/time-series-insights-management/accesspolicies/listbyenvironment).

## <a name="to-delete-data-stored-within-time-series-insights"></a>Gegevens die zijn opgeslagen in Time Series Insights verwijderen

Persoonlijke gegevens mogelijk maakt eraan Time Series Insights-opslag, een ander scenario van gebruiker en gegevens van de beheerder. Als u rekening houden met de gegevens die zijn opgeslagen in Time Series Insights als persoonlijke gegevens, kunt u die kunt exporteren en verwijderen van die gegevens met behulp van de volgende stappen uit:

**Weergeven en exporteren van gegevens**

Als u wilt weergeven en exporteren van gegevens die zijn opgeslagen in Time Series Insights, moet u zoeken naar gegevens. U kunt de Time Series Insights explorer of de Time Series Insights-query's gebruiken voor het weergeven en exporteren van gegevens. Als u wilt weergeven en exporteren van gegevens met behulp van de Verkenner van Time Series Insights, eerst zoeken naar de om gebruikersgegevens te vinden in kwestie. Na het zoeken, met de rechtermuisknop op de grafiek en selecteer **gebeurtenissen onderzoeken**. Het raster gebeurtenissen wordt weergegeven en biedt opties voor het exporteren van de gegevens als CSV en JSON.

Zie voor meer informatie, [Azure Time Series Insights explorer](time-series-insights-explorer.md).

**Gegevens verwijderen**

Time Series Insights biedt momenteel geen ondersteuning voor nauwkeurige verwijdering van gegevens. Time Series Insights biedt echter de mogelijkheid om te verwijderen die zijn opgeslagen in Time Series Insights door te configureren voor het bewaren van gegevens van de klant. U kunt de bewaarperiode van de gehele Time Series Insights-omgeving naar een willekeurig aantal dagen voor de ondersteuning van uw vereisten verwijdering aanpassen.

Zie voor meer informatie, [configuratie van de bewaartermijn in Time Series Insights](time-series-insights-how-to-configure-retention.md).