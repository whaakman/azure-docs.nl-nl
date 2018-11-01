---
title: Poort-whitelist voor Avere vFXT voor Azure
description: Lijst met poorten die worden gebruikt door de vFXT Avere voor Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: 394415ffc7593d058d8d0bf1c0040b88ec25073b
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50633972"
---
# <a name="required-ports"></a>Vereiste poorten

De poorten die worden vermeld in deze sectie worden gebruikt voor vFXT binnenkomende en uitgaande communicatie.

Nooit beschikbaar maakt het cluster vFXT of het exemplaar van de netwerkcontroller cluster rechtstreeks naar het openbare internet.

## <a name="api"></a>API

| Binnenkomend: | | |
| --- | ---- | --- |
| TCP | 22  | SSH  |
| TCP | 443 | HTTPS|



| Uitgaand: |     |       |
|----------|-----|-------|
| TCP      | 443 | HTTPS |

 
## <a name="nfs"></a>NFS

| Inkomend en uitgaand  | | |
| --- | --- | ---|
| TCP/UDP | 111  | RPCBIND  |
| TCP/UDP | 2049 | NFS      |
| TCP/UDP | 4045 | NLOCKMGR |
| TCP/UDP | 4046 | MOUNTD   |
| TCP/UDP | 4047 | STATUS   |

