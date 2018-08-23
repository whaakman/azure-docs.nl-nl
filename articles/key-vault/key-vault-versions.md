---
title: Versies van Key Vault
description: De verschillende versies van Azure Key Vault
services: key-vault
documentationcenter: ''
author: bryanla
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: e8622dcc-59a3-4f4b-9f63-cd2232515a65
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: bryanla
ms.openlocfilehash: 50d35f0dbfb180628577286d6e658daca3c41dc8
ms.sourcegitcommit: 0fcd6e1d03e1df505cf6cb9e6069dc674e1de0be
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/14/2018
ms.locfileid: "42054852"
---
# <a name="key-vault-versions"></a>Versies van Key Vault

## <a name="2016-10-01---managed-storage-account-keys"></a>2016-10-01 - beheerde Opslagaccountsleutels

In de zomer plaats 2017 - Opslagaccountsleutels functie toegevoegd eenvoudiger integratie met Azure Storage. Zie het overzichtsonderwerp voor meer informatie, [beheerde Opslagaccountsleutels overzicht](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-storage-keys).

## <a name="2016-10-01---soft-delete"></a>2016-10-01 - functie voor voorlopig verwijderen

In de zomer plaats 2017 - functie voor voorlopig verwijderen toegevoegd voor verbeterde beveiliging van uw sleutelkluizen en key vault objecten. Zie het overzichtsonderwerp voor meer informatie, [voorlopig verwijderen overzicht](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete).

## <a name="2015-06-01---certificate-management"></a>2015-06-01 - certificate management

Het beheren van certificaten wordt toegevoegd als een functie aan de GA-versie 2015-06-01 op 26 September 2016.

## <a name="2015-06-01---general-availability"></a>2015-06-01 - algemene beschikbaarheid

Algemene beschikbaarheid, versie 2015-06-01, aangekondigd op 24 juni 2015.

De volgende wijzigingen zijn aangebracht in deze release:

- Een sleutel verwijderen: "gebruiken" veld verwijderd.
- Krijg informatie over een sleutel - "gebruiken" veld verwijderd.
- Een sleutel in een kluis importeren: "gebruiken" veld verwijderd.
- Een sleutel herstellen: "gebruiken" veld verwijderd.
- Gewijzigde 'RSA_OAEP' naar 'RSA-OAEP' voor RSA-algoritmen. Zie [over sleutels, geheimen en certificaten](about-keys-secrets-and-certificates.md).

## <a name="2015-02-01-preview"></a>2015-02-01-preview 

Tweede preview-versie 2015-02-01-preview, aangekondigd op 20 April 2015. Zie voor meer informatie, [REST API-Update](http://blogs.technet.com/b/kv/archive/2015/04/20/empty-3.aspx) blogbericht.

De volgende taken zijn bijgewerkt:

- De sleutels in een kluis - ondersteuning voor paginering toegevoegd om de bewerking te weergeven.
- De versies weergeven van een sleutel - toegevoegd om de versies van een sleutel weergeven.
- Geheimen in een kluis - ondersteuning voor paginering toegevoegd.
- Versies van een geheim weergeven: toevoegen om de versies van een geheim weergeven.
- Alle bewerkingen - gemaakte of bijgewerkte tijdstempels aan kenmerken toegevoegd.
- Maken van een geheim - Content-Type toegevoegd aan geheimen.
- Maak een sleutel - tags als optionele informatie toegevoegd.
- Maken van een geheim - tags als optionele informatie toegevoegd.
- Bijwerken van een sleutel - tags als optionele informatie toegevoegd.
- Bijwerken van een geheim - tags als optionele informatie toegevoegd.
- Maximale grootte voor geheimen gewijzigd van 10 K in 25 kB. Zie, [over sleutels, geheimen en certificaten](about-keys-secrets-and-certificates.md).

## <a name="2014-12-08-preview"></a>2014-12-08-preview

Eerste preview-versie 2014-12-08-preview, aangekondigd op 8 januari 2015.

## <a name="see-also"></a>Zie ook
- [Informatie over sleutels, geheimen en certificaten](about-keys-secrets-and-certificates.md)
