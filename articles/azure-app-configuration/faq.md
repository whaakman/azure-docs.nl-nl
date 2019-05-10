---
title: Azure App-configuratie Veelgestelde vragen over | Microsoft Docs
description: Veelgestelde vragen over Azure App-configuratie
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: 3181609bf34a04de4e31b73429f9bc5fa3fe3408
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65411894"
---
# <a name="azure-app-configuration-faq"></a>Veelgestelde vragen over Azure Appconfiguratie

In dit artikel adressen regelmatig vragen over Azure App-configuratie.

## <a name="how-is-app-configuration-different-from-key-vault"></a>Wat is App-configuratie verschil met Key Vault?

App-configuratie is ontworpen voor een afzonderlijke reeks gebruiksvoorbeelden: het helpt ontwikkelaars Toepassingsinstellingen beheren en controleren van de beschikbaarheid van functies. Het is erop gericht om te vereenvoudigen veel van de taken van het werken met complexe configuratiegegevens. Het ondersteunt hiërarchische naamruimte, labels, uitgebreide query's, ophalen, batch, en gespecialiseerde beheerbewerkingen en gebruikersinterfaces. App-configuratie is een aanvulling op de Key Vault en de twee naast elkaar kunnen worden gebruikt in de meeste implementaties van toepassingen.

## <a name="should-i-store-secrets-in-app-configuration"></a>Moet ik geheimen in de App-configuratie opslaan?

App-configuratie biedt beperkte beveiliging, wordt Key Vault is nog steeds de beste plaats voor het opslaan van toepassingsgeheimen. Biedt versleuteling op apparaatniveau hardware, gedetailleerde toegangsbeleid en beheerbewerkingen zoals certificaatrotatie.

## <a name="does-app-configuration-encrypt-my-data"></a>App-configuratie versleuteld met mijn gegevens?

Ja. App-configuratie versleutelt alle sleutelwaarden die deze bevat en netwerkcommunicatie. Sleutelnamen als indexen worden gebruikt voor het ophalen van configuratiegegevens en worden niet versleuteld.

## <a name="does-app-configuration-support-azure-virtual-network-vnet"></a>App-configuratie biedt ondersteuning voor Azure Virtual Network (VNET)?

Nog niet. VNET-ondersteuning is gepland voor algemene beschikbaarheid.

## <a name="how-should-i-store-configurations-for-multiple-environments-test-staging-production-and-so-on"></a>Hoe kan ik opslaan configuraties voor meerdere omgevingen (testen, fasering, productie, enzovoort)?

Op dit moment kunt u bepalen wie toegang heeft tot App-configuratie op het niveau van een per-store. U moet een afzonderlijk archief gebruiken voor elke omgeving die andere machtigingen vereist. Deze aanpak biedt de beste beveiligingsisolatie.

## <a name="what-are-the-recommended-ways-to-use-app-configuration"></a>Wat zijn de aanbevolen manieren waarop u met App-configuratie?

Zie [aanbevolen procedures](./howto-best-practices.md).

## <a name="how-much-does-app-configuration-cost"></a>Wat kost App-configuratie?

De service is gratis tijdens de preview-versie moet worden gebruikt.

## <a name="how-can-i-report-an-issue-or-give-a-suggestion"></a>Hoe kan ik een probleem melden of geven een suggestie wilt doen?

U kunt ons op bereiken [GitHub](https://github.com/Azure/AppConfiguration/issues).

## <a name="next-steps"></a>Volgende stappen

* [Over Azure App-configuratie](./overview.md)
