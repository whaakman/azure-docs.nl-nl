---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/28/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 518c57bc3327511b70deef143826f2a1b9df8639
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58632882"
---
Voordat u een RBAC-rol aan een beveiligings-principal toewijzen, bepalen het bereik van toegang dat de beveiligings-principal moet hebben. Aanbevolen procedures leggen dat is het altijd verstandig om alleen de smalst mogelijke bereik verlenen.

De volgende lijst beschrijft de niveaus waarop u kunt het bereik van toegang tot Azure blob- en wachtrijservices-resources, beginnen met het kleinst mogelijke bereik:

- **Als een afzonderlijke container.** Op dit bereik een roltoewijzing is van toepassing op alle van de blobs in de container, evenals de eigenschappen van de container en de metagegevens.
- **Een afzonderlijke wachtrij.** Op dit bereik een roltoewijzing is van toepassing op berichten in de wachtrij, evenals wachtrij-eigenschappen en metagegevens.
- **Het opslagaccount.** Een roltoewijzing binnen dit bereik is van toepassing op alle containers en hun blobs of naar alle wachtrijen en hun berichten.
- **De resourcegroep.** Op dit bereik een roltoewijzing is van toepassing op alle containers of wachtrijen in alle van de storage-accounts in de resourcegroep.
- **Het abonnement.** Op dit bereik een roltoewijzing is van toepassing op alle containers of wachtrijen in alle van de storage-accounts in alle resourcegroepen in het abonnement.

> [!IMPORTANT]
> Als uw abonnement een Azure DataBricks-naamruimte bevat, wordt rollen toegewezen op het abonnementsbereik geblokkeerd verlenen van toegang tot blobopslag-en wachtrij.
