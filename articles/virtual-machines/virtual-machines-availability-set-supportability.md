---
title: Ondersteuning van Azure Virtual machines toevoegen aan de bestaande beschikbaarheidsset instellen | Microsoft Docs
description: Ondersteuning van Azure Virtual machines toevoegen aan een bestaande beschikbaarheidsset.
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
ms.date: 11/03/2017
ms.author: delhan
ms.openlocfilehash: 8bf2a55563772e26239445732b2b08df677436ef
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2017
ms.locfileid: "23987717"
---
# <a name="supportability-of-adding-azure-vms-to-an-existing-availability-set"></a>Ondersteuning van Azure Virtual machines toevoegen aan een bestaande beschikbaarheidsset

Van tijd tot tijd kan verschijnen beperkingen wanneer u nieuwe virtuele machines (VM's) toevoegen aan een bestaande beschikbaarheidsset. Het volgende diagram details over welke VM-reeks u in dezelfde beschikbaarheidsset mengen kunt.

Dit is de matrix ondersteuningsmogelijkheden mix van verschillende typen van virtuele machines:

Reeks & Beschikbaarheidsset|Tweede VM|A|Av2|D|Dv2|Dv3|
|---|---|---|---|---|---|---|
|Eerste VM|||||||
|A||OK|OK|OK|OK|OK|
|Av2||OK|OK|OK|OK|OK|
|D||OK|OK|OK|OK|OK|
|Dv2||OK|OK|OK|OK|OK|
|Dv3||OK|OK|OK|OK|OK|

Alle andere reeks kan niet worden in dezelfde beschikbaarheidsset omdat ze een specifieke hardware vereist.

A8/A9-VM-grootte kan niet worden gecombineerd vanwege requirment op speciaal RDMA back-end-netwerk.
