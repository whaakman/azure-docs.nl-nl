---
title: Azure Stack implementeren | Microsoft Docs
description: Implementeer Azure Stack opnieuw.
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 795af5ea-892d-40b1-a080-42e4472e4bba
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2018
ms.author: jeffgilb
ms.openlocfilehash: 0dec5ea70376ff1c8cf488689f1a66190256f6ff
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="redeploy-azure-stack"></a>Azure Stack implementeren
Als u een foutbericht ontvangt bij het implementeren van Azure-Stack, u kunt met de volgende PowerShell-opdracht setup opnieuw uitvoeren: `.\InstallAzureStackpoc.ps1 -rerun`. Met deze opdracht wordt de Azure-Stack setup op het moment dat dit eerder is mislukt zonder om op te starten vanaf het begin opnieuw. Als u dezelfde instellingen fout opnieuw ontvangt, is het mogelijk dat het nodig zijn voor het uitvoeren van een volledige opnieuw distribueren naar het adres van het probleem. 

Als u wilt implementeren in Azure-Stack, moet u starten via helemaal zoals hieronder wordt beschreven.

## <a name="steps-to-redeploy-azure-stack"></a>Stappen voor het implementeren van Azure-Stack
1. Open een PowerShell-console met verhoogde bevoegdheid op de host van de kit ontwikkeling > Ga naar het script asdk installer.ps1 > uitvoeren > klikt u op **opnieuw opstarten**.
2. Selecteer het basisbesturingssysteem (geen **Azure Stack**) en klik op **volgende**.
3. Nadat de development kit host opnieuw is opgestart, moet u het CloudBuilder.vhdx-bestand dat is gebruikt als onderdeel van de vorige implementatie is verwijderd.
4. [Implementeer de development kit](azure-stack-run-powershell-script.md).

## <a name="next-steps"></a>Volgende stappen
[Verbinding maken met Azure Stack](azure-stack-connect-azure-stack.md)

