---
title: Azure Stack implementeren | Microsoft Docs
description: Implementeer Azure Stack opnieuw.
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 795af5ea-892d-40b1-a080-42e4472e4bba
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: erikje
ms.openlocfilehash: 891cde9b16bbbb51729129b6ad7a0f3794307baa
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="redeploy-azure-stack"></a>Azure Stack implementeren
Als u wilt implementeren in Azure-Stack, moet u starten via helemaal zoals hieronder wordt beschreven.

## <a name="steps-to-redeploy-azure-stack"></a>Stappen voor het implementeren van Azure-Stack
1. Open een PowerShell-console met verhoogde bevoegdheid op de host van de kit ontwikkeling > Ga naar het script asdk installer.ps1 > uitvoeren > klikt u op **opnieuw opstarten**.
2. Selecteer het basisbesturingssysteem (geen **Azure Stack**) en klik op **volgende**.
3. Nadat de development kit host opnieuw is opgestart, moet u het CloudBuilder.vhdx-bestand dat is gebruikt als onderdeel van de vorige implementatie is verwijderd.
4. [Implementeer de development kit](azure-stack-run-powershell-script.md).

## <a name="next-steps"></a>Volgende stappen
[Verbinding maken met Azure Stack](azure-stack-connect-azure-stack.md)

