---
title: Sleutelkluis-versies
description: De verschillende versies van Azure Sleutelkluis
services: key-vault
documentationcenter: ''
author: lleonard-msft
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: e8622dcc-59a3-4f4b-9f63-cd2232515a65
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: alleonar
ms.openlocfilehash: beb73be66f36ccf95fe27d4d8128106cd12722a8
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
ms.locfileid: "34012103"
---
# <a name="key-vault-versions"></a>Sleutelkluis-versies

## <a name="2016-10-01---managed-storage-account-keys"></a>2016-10-01 - beheerde Opslagaccountsleutels

2017 zomer - toegangscodes voor opslag functie toegevoegd vereenvoudiging van de integratie met Azure Storage. Zie het overzichtsonderwerp voor meer informatie [Opslagaccountsleutels beheerd overzicht](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-storage-keys).

## <a name="2016-10-01---soft-delete"></a>2016-10-01-voorlopig verwijderen

2017 zomer - functie voor voorlopig verwijderen toegevoegd voor verbeterde beveiliging van uw sleutelkluizen en sleutelkluis objecten. Zie het overzichtsonderwerp voor meer informatie [voorlopig verwijderen overzicht](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete).

## <a name="2015-06-01---certificate-management"></a>2015-06-01 - Certificaatbeheer

Certificaatbeheer is toegevoegd als een functie aan de GA-versie 2015-06-01 op 26 September 2016.

## <a name="2015-06-01---general-availability"></a>2015-06-01 - algemene beschikbaarheid

Algemene beschikbaarheid versie 2015-06-01 aangekondigd op 24 juni 2015.

De volgende wijzigingen zijn aangebracht in deze release:

- Een sleutel verwijderen: "gebruiken" veld verwijderd.
- Informatie over een sleutel - "gebruiken" veld verwijderd ophalen.
- Een sleutel in een kluis importeren: "gebruiken" veld verwijderd.
- Een sleutel herstellen: "gebruiken" veld verwijderd.
- Gewijzigde 'RSA_OAEP' naar 'RSA-OAEP' voor RSA-algoritmen. Zie [over sleutels, geheimen en certificaten](about-keys-secrets-and-certificates.md).

## <a name="2015-02-01-preview"></a>2015-02-01-preview 

Tweede preview-versie 2015-02-01-preview aangekondigd op 20 April 2015. Zie voor meer informatie [REST API Update](http://blogs.technet.com/b/kv/archive/2015/04/20/empty-3.aspx) blogbericht.

De volgende taken zijn bijgewerkt:

- Lijst met sleutels in een kluis - paginering toegevoegde ondersteuning voor bewerking.
- Lijst van de versies van een sleutel - toegevoegd bewerking als de versies van een sleutel wilt weergeven.
- Lijst met geheimen in een kluis - ondersteuning voor paginering toegevoegd.
- Lijst van de versies van een geheim - toevoegbewerking als u de versies van een geheim wilt weergeven.
- Alle bewerkingen - tijdstempels gemaakt/bijgewerkt aan kenmerken toegevoegd.
- Content-Type maken van een geheim - aan geheimen worden toegevoegd.
- Maak een sleutel - labels toegevoegd als optionele informatie.
- Maken van een geheim - labels toegevoegd als optionele informatie.
- Bijwerken van een sleutel - labels toegevoegd als optionele informatie.
- Bijwerken van een geheim - labels toegevoegd als optionele informatie.
- Maximale grootte voor geheimen gewijzigd van 10 K tot 25 kB. Zie, [over sleutels, geheimen en certificaten](about-keys-secrets-and-certificates.md).

## <a name="2014-12-08-preview"></a>2014-12-08-preview

Eerste preview-versie 2014-12-08-preview, aangekondigd op 8 januari 2015.

## <a name="see-also"></a>Zie ook
- [Informatie over sleutels, geheimen en certificaten](about-keys-secrets-and-certificates.md)
