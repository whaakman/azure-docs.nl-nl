---
title: Functies van Azure Sleutelkluis klant gegevens | Microsoft Docs
description: Meer informatie over de gegevens van de klant in de Sleutelkluis
services: key-vault
documentationcenter: ''
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 05/22/2018
ms.author: barclayn
ms.openlocfilehash: 1ddc74b1960095509a77d4b3072017847df42d90
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34637358"
---
# <a name="azure-key-vault-customer-data-features"></a>Functies van Azure Sleutelkluis klant gegevens

Azure Sleutelkluis ontvangt gegevens van de klant tijdens het maken of bijwerken van de kluizen, sleutels, geheimen, certificaten en beheerde storage-accounts. Deze gegevens van de klant is direct zichtbaar in de Azure-portal en de REST-API. Klantgegevens worden bewerkt of verwijderd door bijwerken of verwijderen van het object dat de gegevens bevat.

Toegang tot het systeemlogboek in Logboeken worden gegenereerd wanneer een gebruiker of toepassing toegang heeft tot de Sleutelkluis. Gedetailleerde toegangslogboeken zijn beschikbaar voor klanten die gebruikmaken van Azure Insights.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="identifying-customer-data"></a>Identificeren van klantgegevens

De volgende informatie bevat klantgegevens binnen Azure Sleutelkluis.

- Beleidsregels voor toegang voor Azure Sleutelkluis-object-id's bevatten die gebruikers, groepen of toepassingen
- Objecten van het certificaat mogelijk zijn er e-mailadressen of andere gebruiker of organisatie-id 's
- Certificaat contactpersonen kunnen gebruiker e-mailadressen, namen of telefoonnummers bevatten
- Certificaatverleners kunnen e-mailadressen, namen, telefoonnummers, referenties en organisatiegegevens bevatten
- Willekeurige tags kunnen worden toegepast op objecten in Azure Sleutelkluis. Deze objecten bevatten kluizen, sleutels, geheimen, certificaten en storage-accounts. De labels die gebruikt kunnen persoonlijke gegevens bevatten
- Azure Sleutelkluis-logboeken bevatten object-id's [UPN's](../active-directory/connect/active-directory-aadconnect-userprincipalname.md), en IP-adressen voor elke aanroep van de REST-API
- Diagnostische logboeken van Azure Sleutelkluis kunnen-object-id's en IP-adressen voor de REST API-aanroepen bevatten

## <a name="deleting-customer-data"></a>Verwijderen van klantgegevens

REST-API's, ervaring van de Portal en SDK's gebruikt voor het maken van kluizen, sleutels, geheimen, certificaten en beheerde storage-accounts kunnen ook bijwerken en verwijderen van deze objecten.

Voorlopig verwijderen kunt u verwijderde gegevens herstellen voor 90 dagen na verwijdering. Wanneer u voorlopig verwijderen, kan de gegevens permanent worden verwijderd voordat de 90 dagen door het uitvoeren van een bewerking opschonen bewaartermijn is verstreken. Als de kluis of het abonnement is geconfigureerd voor het blok opschonen bewerkingen, het is niet mogelijk gegevens definitief verwijderen totdat de geplande bewaarperiode is verstreken.

## <a name="exporting-customer-data"></a>Exporteren van klantgegevens

De dezelfde REST-API's, portal ervaring en SDK's die worden gebruikt voor het maken van kluizen, sleutels, geheimen, certificaten en beheerde storage-accounts ook zijn kunnen u weergeven en exporteren van deze objecten.

Azure Sleutelkluis-Logboeken is een optionele functie die kan worden ingeschakeld op genereren logboeken voor elke REST-API-aanroep. Deze logboeken wordt overgedragen naar een opslagaccount in uw abonnement waar u het bewaarbeleid die voldoet aan de vereisten van uw organisatie kunt toepassen.

Diagnostische logboeken van Azure Sleutelkluis die persoonlijke gegevens kunnen worden opgehaald door het maken van een aanvraag exporteren in de portal van de Privacy van gebruikers. Deze aanvraag moet worden gemaakt door de tenantbeheerder.

## <a name="next-steps"></a>Volgende stappen

- [Logboekregistratie van Azure Key Vault](key-vault-logging.md)

- [Overzicht van Azure Sleutelkluis voorlopig verwijderen](key-vault-soft-delete-cli.md)

- [Kluizen](https://docs.microsoft.com/rest/api/keyvault/vaults)

- [Bewerkingen met Azure Sleutelkluis sleutels](https://docs.microsoft.com/rest/api/keyvault/key-operations)

- [Azure Sleutelkluis geheime bewerkingen](https://docs.microsoft.com/rest/api/keyvault/secret-operations)

- [Azure Sleutelkluis-certificaten en het beleid](https://docs.microsoft.com/rest/api/keyvault/certificates-and-policies)

- [Uitgevers van certificaten](https://docs.microsoft.com/rest/api/keyvault/certificate-issuers)

- [Azure Sleutelkluis account opslagbewerkingen](https://docs.microsoft.com/rest/api/keyvault/storage-account-key-operations)
