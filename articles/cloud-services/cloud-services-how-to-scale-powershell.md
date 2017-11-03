---
title: Een Azure-cloud-service schalen in Windows PowerShell | Microsoft Docs
description: (klassiek) Informatie over het gebruiken van PowerShell een Webrol of worker-rol in of uit te schalen in Azure.
services: cloud-services
documentationcenter: 
author: mmccrory
manager: timlt
editor: 
ms.assetid: ee37dd8c-6714-4c61-adb8-03d6bbf76c9a
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2016
ms.author: mmccrory
ms.openlocfilehash: a7ae8ff202d403dff19b8c9a6a09492235db27ac
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-scale-a-cloud-service-in-powershell"></a>Schalen van een cloudservice in PowerShell

Windows PowerShell kunt u een Webrol of worker-rol in- of schalen door het toevoegen of verwijderen van exemplaren.  

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Voordat u geen bewerkingen uit op uw abonnement via PowerShell uitvoeren kunt, moet u zich aanmeldt:

```powershell
Add-AzureAccount
```

Als u meerdere abonnementen die zijn gekoppeld aan uw account hebt, moet u wellicht wijzigen van het huidige abonnement, afhankelijk van waar uw cloudservice zich bevindt. Als u wilt controleren in het huidige abonnement, uitvoeren:

```powershell
Get-AzureSubscription -Current
```

Als u wijzigen van het huidige abonnement wilt, uitvoeren:

```powershell
Set-AzureSubscription -SubscriptionId <subscription_id>
```

## <a name="check-the-current-instance-count-for-your-role"></a>Controleer het huidige aantal exemplaren voor uw rol

Als u wilt controleren in de huidige status van uw rol, worden uitgevoerd:

```powershell
Get-AzureRole -ServiceName '<your_service_name>' -RoleName '<your_role_name>'
```

Ontvangt u informatie over de functie, inclusief de huidige OS-versie en exemplaar aantal. In dit geval heeft de rol één exemplaar.

![Informatie over de rol](./media/cloud-services-how-to-scale-powershell/get-azure-role.png)

## <a name="scale-out-the-role-by-adding-more-instances"></a>De rol uitbreiden door meer exemplaren toe te voegen

Als u wilt uitbreiden uw rol, geeft u het gewenste aantal exemplaren als de **aantal** -parameter voor de **Set AzureRole** cmdlet:

```powershell
Set-AzureRole -ServiceName '<your_service_name>' -RoleName '<your_role_name>' -Slot <target_slot> -Count <desired_instances>
```

De cmdlet-blokken tijdelijk worden tijdens de nieuwe exemplaren worden ingericht en gestart. Tijdens deze periode, als u een nieuwe PowerShell-venster en aanroep openen **Get-AzureRole** zoals eerder besproken, ziet u de nieuwe doel-exemplaren. En als u de Rolstatus van de in de portal inspecteren, ziet u het nieuwe exemplaar wordt opgestart:

![VM-instantie in de portal wordt gestart](./media/cloud-services-how-to-scale-powershell/role-instance-starting.png)

Nadat de nieuwe exemplaren hebt gestart, wordt de cmdlet is geretourneerd:

![Rol exemplaar toename geslaagd](./media/cloud-services-how-to-scale-powershell/set-azure-role-success.png)

## <a name="scale-in-the-role-by-removing-instances"></a>In de rol schalen door het verwijderen van instanties

U kunt een rol schalen door het verwijderen van instanties op dezelfde manier. Stel de **aantal** parameter op **Set AzureRole** aan het aantal exemplaren dat u hebben wilt nadat de schaal in de bewerking voltooid is.

## <a name="next-steps"></a>Volgende stappen

Het is niet mogelijk automatisch geschaald voor cloudservices vanuit PowerShell configureren. Zie hiervoor [automatisch schalen een cloudservice](cloud-services-how-to-scale-portal.md).
