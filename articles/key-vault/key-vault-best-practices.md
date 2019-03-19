---
title: Aanbevolen procedures voor het gebruik van Key Vault - Azure Key Vault | Microsoft Docs
description: Dit document wordt uitgelegd in enkele van de aanbevolen procedures voor het gebruik van Key Vault
services: key-vault
documentationcenter: ''
author: yvprashanth
manager: barbkess
tags: azure-key-vault
ms.assetid: e5b4e083-4a39-4410-8e3a-2832ad6db405
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/07/2019
ms.author: prashanthyv
ms.openlocfilehash: 85bd1858bc3f2d505e1e4d0a88e8c77a46ae4447
ms.sourcegitcommit: b8f9200112cae265155b8877f7e1621c4bcc53fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/14/2019
ms.locfileid: "57882029"
---
# <a name="best-practices-to-use-key-vault"></a>Aanbevolen procedures voor het gebruik van Key Vault

## <a name="control-access-to-your-vault"></a>Toegangsbeheer voor uw kluis

Azure Key Vault is een cloudservice die beveiligt uw versleutelingssleutels en geheimen, zoals certificaten, verbindingsreeksen en wachtwoorden. Omdat deze gegevens vertrouwelijk zijn en bedrijfskritiek, u moet voor het beveiligen van toegang tot uw key vaults door toe te staan alleen gemachtigde toepassingen en gebruikers. Dit [artikel](key-vault-secure-your-key-vault.md) biedt een overzicht van het model voor Key Vault. Het wordt verificatie en autorisatie worden uitgelegd en wordt beschreven hoe u veilig toegang tot uw key vaults.

Suggesties tijdens het beheren van toegang tot de kluis zijn als volgt:
1. Vergrendelen van toegang tot uw abonnement, resourcegroep en de sleutel kluizen (RBAC)
2. Toegangsbeleid voor elke kluis maken
3. Minimale bevoegdheden toegang principal gebruiken om toegang te verlenen
4. Firewall inschakelen en [VNET-Service-eindpunten](key-vault-overview-vnet-service-endpoints.md)

## <a name="use-separate-key-vault"></a>Gebruik afzonderlijke Key Vault

Onze aanbeveling is het gebruik van een sleutelkluis per toepassing per omgeving (ontwikkeling, Pre-productie en productie). Dit kunt u geen geheimen delen tussen omgevingen en verlaagt u ook de bedreiging in het geval van een inbreuk.

## <a name="backup-your-vault"></a>Back-up van uw kluis

Zorg ervoor dat u ondernemen regelmatig terug ups van uw [kluis](https://blogs.technet.microsoft.com/kv/2018/07/20/announcing-backup-and-restore-of-keys-secrets-and-certificates/) op update/delete/maken van objecten in een kluis.

## <a name="turn-on-logging"></a>Logboekregistratie inschakelen

[Logboekregistratie inschakelen](key-vault-logging.md) voor uw kluis. Ook waarschuwingen instellen.

## <a name="turn-on-recovery-options"></a>Opties voor Systeemherstel inschakelen

1. Schakel [voorlopig verwijderen](key-vault-ovw-soft-delete.md).
2. Opschonen-beveiliging inschakelen als u wilt beschermen tegen het verwijderen van de werking van het geheim / kluis, zelfs nadat de functie voor voorlopig verwijderen is ingeschakeld.
