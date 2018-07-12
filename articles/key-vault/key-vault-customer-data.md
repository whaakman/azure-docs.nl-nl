---
title: Azure Key Vault klant functies | Microsoft Docs
description: Meer informatie over de gegevens van de klant in Key Vault
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
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38235530"
---
# <a name="azure-key-vault-customer-data-features"></a>Azure Key Vault klant functies

Azure Key Vault ontvangt gegevens van de klant tijdens het maken of bijwerken van kluizen, sleutels, geheimen, certificaten en beheerde opslagaccounts. Deze gegevens van de klant is direct zichtbaar in de Azure-portal en via de REST-API. Gegevens van de klant kan worden bewerkt of verwijderd door bijwerken of verwijderen van het object dat de gegevens bevat.

Toegang tot het systeemlogboek in Logboeken worden gegenereerd wanneer een gebruiker of toepassing toegang heeft tot Key Vault. Gedetailleerde logboeken zijn beschikbaar voor klanten met Azure Insights.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="identifying-customer-data"></a>Identificeren van klantgegevens

De volgende informatie geeft klantgegevens in Azure Key Vault:

- Toegangsbeleid voor Azure Key Vault-object-id's bevatten voor gebruikers, groepen of toepassingen
- Certificaat onderwerpen omvat mogelijk e-mailadressen of andere gebruiker of organisatie-id 's
- Certificaatcontactpersonen mag gebruiker e-mailadressen, namen en telefoonnummers
- Certificaatverleners mag e-mailadressen, namen, telefoonnummers, accountreferenties en organisatie-details
- Willekeurige labels kunnen worden toegepast op objecten in Azure Key Vault. Deze objecten bevatten kluizen, sleutels, geheimen, certificaten en storage-accounts. De labels die worden gebruikt mogelijk persoonlijke gegevens bevatten.
- Azure Key Vault toegang tot logboeken bevatten de object-id's [UPN's](../active-directory/connect/active-directory-aadconnect-userprincipalname.md), en IP-adressen voor elke aanroep van de REST-API
- Diagnostische logboeken in Azure Key Vault kunnen-object-id's en IP-adressen voor REST API-aanroepen bevatten

## <a name="deleting-customer-data"></a>Verwijderen van klantgegevens

REST-API's, Portal-ervaring en SDK's gebruikt voor het maken van kluizen, sleutels, geheimen, certificaten en accounts van beheerde opslag, kunnen ook bijwerken en verwijderen van deze objecten.

Voorlopig verwijderen kunt u verwijderde gegevens herstellen voor 90 dagen na verwijdering. Wanneer u de functie voor voorlopig verwijderen, kan de gegevens definitief worden verwijderd voordat de 90 dagen door het uitvoeren van een bewerking opschonen bewaartermijn is verstreken. Als de kluis of het abonnement is geconfigureerd voor het blok opschonen operations, het is niet mogelijk gegevens definitief verwijderen totdat de geplande bewaarperiode is verstreken.

## <a name="exporting-customer-data"></a>Exporteren van gegevens van de klant

De dezelfde REST-API's, portal-ervaring, en SDK's die worden gebruikt voor het maken van kluizen, sleutels, geheimen, certificaten en beheerde opslag-accounts ook zijn kunnen u weergeven en exporteren van deze objecten.

Azure Key Vault-logboekregistratie een optionele functie die kan worden ingeschakeld is op genereren logboeken voor elke REST-API-aanroep. Deze logboeken worden overgedragen naar een opslagaccount in uw abonnement waar u het bewaarbeleid dat voldoet aan de vereisten van uw organisatie kunt toepassen.

Diagnostische logboeken in Azure Key Vault met persoonsgegevens kunnen worden opgehaald door een aanvraag voor exporteren in de portal van de Privacy van gebruikers. Deze aanvraag moet worden gemaakt door de tenantbeheerder.

## <a name="next-steps"></a>Volgende stappen

- [Logboekregistratie van Azure Key Vault](key-vault-logging.md)

- [Overzicht van Azure Key Vault-functie voor voorlopig verwijderen](key-vault-soft-delete-cli.md)

- [Kluizen](https://docs.microsoft.com/rest/api/keyvault/vaults)

- [Azure Key Vault-sleutelbewerkingen](https://docs.microsoft.com/rest/api/keyvault/key-operations)

- [Azure Key Vault geheime bewerkingen](https://docs.microsoft.com/rest/api/keyvault/secret-operations)

- [Azure Key Vault-certificaten en het beleid](https://docs.microsoft.com/rest/api/keyvault/certificates-and-policies)

- [Uitgevers van certificaten](https://docs.microsoft.com/rest/api/keyvault/certificate-issuers)

- [Azure Key Vault-storage-accountbewerkingen](https://docs.microsoft.com/rest/api/keyvault/storage-account-key-operations)
