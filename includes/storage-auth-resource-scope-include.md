---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/21/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: ad4b244b58d741ad45463297df5bd358f3ae9918
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58449959"
---
Voordat u een RBAC-rol aan een beveiligings-principal toewijzen, bepalen het bereik van toegang dat de beveiligings-principal moet hebben. Aanbevolen procedures leggen dat is het altijd verstandig om alleen de smalst mogelijke bereik verlenen.

De volgende lijst beschrijft de niveaus waarop u kunt het bereik van toegang tot Azure blob- en wachtrijservices-resources, beginnen met het kleinst mogelijke bereik:

- **Als een afzonderlijke container.** Op dit bereik heeft een beveiligings-principal toegang tot alle blobs in de container, evenals de eigenschappen van de container en de metagegevens.
- **Een afzonderlijke wachtrij.** Op dit bereik heeft een beveiligings-principal toegang tot berichten in de wachtrij, evenals wachtrij-eigenschappen en metagegevens.
- **Het opslagaccount.** Op dit bereik heeft een beveiligings-principal toegang tot alle containers en hun blobs of naar alle wachtrijen en hun berichten.
- **De resourcegroep.** Op dit bereik heeft een beveiligings-principal toegang tot alle containers of wachtrijen in alle van de storage-accounts in de resourcegroep.
- **Het abonnement.** Op dit bereik heeft een beveiligings-principal toegang tot alle containers of wachtrijen in alle van de storage-accounts in alle resourcegroepen in het abonnement.
