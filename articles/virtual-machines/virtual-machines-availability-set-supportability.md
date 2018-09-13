---
title: Ondersteuning voor het Azure-VM's toe te voegen aan een bestaande beschikbaarheidsset instellen | Microsoft Docs
description: Ondersteuning voor het Azure-VM's toe te voegen aan een bestaande beschikbaarheidsset.
services: virtual-machines-linux
documentationcenter: ''
author: Deland-Han
manager: cshepard
editor: ''
ms.assetid: ''
ms.service: virtual-machines
ms.workload: virtual-machines
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: delhan
ms.openlocfilehash: 13448f4b335d84264d4141cb4fb8c3eadcf0303e
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35914767"
---
# <a name="supportability-of-adding-azure-vms-to-an-existing-availability-set"></a>Ondersteuning voor het Azure-VM's toe te voegen aan een bestaande beschikbaarheidsset

Af en toe kunnen optreden beperkingen wanneer u nieuwe virtuele machines (VM's) toevoegen aan een bestaande beschikbaarheidsset. Het volgende diagram wordt uitgelegd welke VM-reeks die u in dezelfde beschikbaarheidsset combineren kunt.

Dit is de matrix supportability om verschillende typen virtuele machines:

Reeks & Beschikbaarheidsset|Tweede virtuele machine|A|Av2|D|Dv2|Dv3|
|---|---|---|---|---|---|---|
|Eerste VM|||||||
|A||OK|OK|OK|OK|OK|
|Av2||OK|OK|OK|OK|OK|
|D||OK|OK|OK|OK|OK|
|Dv2||OK|OK|OK|OK|OK|
|Dv3||OK|OK|OK|OK|OK|

Alle andere reeks kan niet worden in dezelfde beschikbaarheidsset omdat ze een specifieke hardware vereist.

A8/A9 VM-grootte kan niet worden gecombineerd vanwege requirment op toegewezen RDMA-back-endnetwerk.
