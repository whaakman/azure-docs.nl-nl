---
title: bestand opnemen
description: bestand opnemen
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: include
ms.date: 06/28/2018
ms.author: raynew
ms.openlocfilehash: f7d6c3f68618fec839ccff06b73ba44d106999d2
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2018
ms.locfileid: "37342819"
---
| Naam | Commerciële URL | URL van de overheid | Beschrijving |
|---|---|---|---|
| Azure AD | ``login.microsoftonline.com`` | ``login.microsoftonline.us`` | Wordt gebruikt voor toegang en identiteitsbeheer met AAD |
| Backup | ``*.backup.windowsazure.com`` | ``*.backup.windowsazure.us`` | Gebruikt voor overdracht van replicatiegegevens en coördinatie |
| Replicatie | ``*.hypervrecoverymanager.windowsazure.com`` | ``*.hypervrecoverymanager.windowsazure.us``  | Gebruikt voor bewerkingen en coördinatie |
| Storage | ``*.blob.core.windows.net`` | ``*.blob.core.usgovcloudapi.net``  | Gebruikt voor toegang tot het opslagaccount waarin de gerepliceerde gegevens |
| Telemetrie (optioneel) | ``dc.services.visualstudio.com`` | ``dc.services.visualstudio.com`` | Gebruikt voor telemetrie |

``time.nist.gov`` en ``time.windows.com`` worden gebruikt om te controleren van de tijdsynchronisatie tussen de systeemtijd en de algemene tijd in alle implementaties.


