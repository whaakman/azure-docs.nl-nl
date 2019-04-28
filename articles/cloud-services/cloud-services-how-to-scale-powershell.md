---
title: Een Azure-cloud-service in Windows PowerShell schalen | Microsoft Docs
description: (klassiek) Leer hoe u PowerShell gebruiken om te schalen van een Webrol of werkrol in- of uitschalen in Azure.
services: cloud-services
documentationcenter: ''
author: mmccrory
manager: timlt
editor: ''
ms.assetid: ee37dd8c-6714-4c61-adb8-03d6bbf76c9a
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2016
ms.author: mmccrory
ms.openlocfilehash: 240b34abd2f9b937c8a7ea449ad7f148870a9d33
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "61433226"
---
# <a name="how-to-scale-a-cloud-service-in-powershell"></a>Een cloudservice schalen in PowerShell

U kunt Windows PowerShell gebruiken voor het schalen van een Webrol of werkrol in of uit door het toevoegen of verwijderen van exemplaren.  

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Voordat u bewerkingen op uw abonnement via PowerShell uitvoeren kunt, moet u zich aanmeldt:

```powershell
Add-AzureAccount
```

Als u meerdere abonnementen die zijn gekoppeld aan uw account hebt, moet u mogelijk om te wijzigen van het huidige abonnement, afhankelijk van waar uw cloudservice zich bevindt. Om te controleren of het huidige abonnement, voert u de volgende uit:

```powershell
Get-AzureSubscription -Current
```

Als u wijzigen van het huidige abonnement wilt, moet uitvoeren:

```powershell
Set-AzureSubscription -SubscriptionId <subscription_id>
```

## <a name="check-the-current-instance-count-for-your-role"></a>Controleer het huidige aantal instanties voor uw rol

Om te controleren of de huidige status van uw rol, voert u de volgende uit:

```powershell
Get-AzureRole -ServiceName '<your_service_name>' -RoleName '<your_role_name>'
```

U moet terug informatie ophalen over de functie, inclusief de huidige OS-versie en exemplaar aantal. In dit geval heeft de rol van één exemplaar.

![Informatie over de rol](./media/cloud-services-how-to-scale-powershell/get-azure-role.png)

## <a name="scale-out-the-role-by-adding-more-instances"></a>De rol uitschalen door meer instanties toe te voegen

Als u wilt schalen uw rol, geeft u het gewenste aantal exemplaren als de **aantal** parameter voor de **Set AzureRole** cmdlet:

```powershell
Set-AzureRole -ServiceName '<your_service_name>' -RoleName '<your_role_name>' -Slot <target_slot> -Count <desired_instances>
```

De cmdlet-blokken verbroken terwijl de nieuwe instanties zijn ingericht en is gestart. Gedurende deze tijd, als u een nieuwe PowerShell-venster en aanroep opent **Get-AzureRole** zoals eerder besproken, ziet u de nieuwe doel-exemplaren. En als u de Rolstatus van de in de portal inspecteren, ziet u het nieuwe exemplaar starten:

![VM-exemplaar starten in de portal](./media/cloud-services-how-to-scale-powershell/role-instance-starting.png)

Nadat de nieuwe exemplaren hebt gestart, retourneert de cmdlet is:

![Rol exemplaar toename geslaagd](./media/cloud-services-how-to-scale-powershell/set-azure-role-success.png)

## <a name="scale-in-the-role-by-removing-instances"></a>In de rol door het verwijderen van instanties schalen

U kunt schalen in een rol door het verwijderen van exemplaren op dezelfde manier. Stel de **aantal** parameter op **Set AzureRole** aan het aantal instanties dat u wilt, nadat de schaal in de bewerking voltooid is.

## <a name="next-steps"></a>Volgende stappen

Het is niet mogelijk om te configureren van automatisch schalen voor cloud-services vanuit PowerShell. Om dit te doen, Zie [schalen een cloudservice](cloud-services-how-to-scale-portal.md).
