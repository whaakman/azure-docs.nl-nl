---
title: Een virtuele Windows-bureaublad preview host van toepassingen voor het valideren van service-updates - Azure maken
description: Het maken van een pool van de host validatie voor het controleren van service-updates voordat u updates uitgebracht voor productie.
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 05/08/2019
ms.author: v-chjenk
ms.openlocfilehash: c9b2a593a6943fe2e9577acc61b1d5a7bcd98607
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67070663"
---
# <a name="tutorial-create-a-host-pool-to-validate-service-updates"></a>Zelfstudie: Een hostpool voor het valideren van service-updates maken

Host-pools zijn een verzameling van een of meer identieke virtuele machines in Windows Virtual Desktop Preview tenant omgevingen. Voordat u implementeert pools van host naar uw productieomgeving, raden wij dat u een pool van de host validatie maakt. Updates worden eerst toegepast op validatie host opslaggroepen, zodat u kunt bewaken van service-updates voordat uitrollen naar uw productieomgeving. Zonder een pool van de host validatie ontdekt u mogelijk geen wijzigingen die fouten, wat tot uitvaltijd voor gebruikers in uw productieomgeving leiden kunnen.

Om te zorgen dat uw apps werken met de meest recente updates, moet de validatie van host-toepassingen als die vergelijkbaar is met de host van toepassingen in uw productieomgeving mogelijk. Gebruikers moeten verbinding maken met net zo vaak aan de groep van de host validatie als aan de groep van de host productie. Als u hebt automatisch testen op uw host-pool, moet u het geautomatiseerd testen op de host validatie groep opnemen.

U kunt fouten opsporen van problemen in de groep van de host validatie in beide gevallen [de functie voor diagnostische gegevens](diagnostics-role-service.md) of de [virtuele Windows-bureaublad artikelen over probleemoplossing](https://docs.microsoft.com/Azure/virtual-desktop/troubleshoot-set-up-overview).

>[!NOTE]
> Het is raadzaam dat u de groep van de host validatie intact laten voor het testen van alle toekomstige updates.

Voordat u begint, [downloaden en importeren van de Windows virtuele bureaublad PowerShell-module](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview), als u dat nog niet gedaan hebt.

## <a name="create-your-host-pool"></a>Uw host-pool maken

U kunt een host-pool maken door de instructies in een van deze artikelen:
- [Zelfstudie: Een host-pool maken met Azure Marketplace](create-host-pools-azure-marketplace.md)
- [Een host-pool maken met een Azure Resource Manager-sjabloon](create-host-pools-arm-template.md)
- [Een host-pool maken met PowerShell](create-host-pools-powershell.md)

## <a name="define-your-host-pool-as-a-validation-host-pool"></a>Uw host-pool definiëren als een groep van de host validatie

Voer de volgende PowerShell-cmdlets voor het definiëren van de nieuwe pool voor de host als een groep van de host validatie. Vervang de waarden in de aanhalingstekens door de waarden die relevant zijn voor uw sessie:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
Set-RdsHostPool -TenantName $myTenantName -Name "contosoHostPool" -ValidationEnv $true
```

Voer de volgende PowerShell-cmdlet om te bevestigen dat de validatie-eigenschap is ingesteld. Vervang de waarden tussen aanhalingstekens door de waarden die relevant zijn voor uw sessie.

```powershell
Get-RdsHostPool -TenantName $myTenantName -Name "contosoHostPool" -ValidationEnv $true
```

De resultaten van de cmdlet moeten naar deze uitvoer als volgt uitzien:

```
    TenantName          : contoso 
    TenantGroupName     : Default Tenant Group
    HostPoolName        : contosoHostPool
    FriendlyName        :
    Description         :
    Persistent          : False 
    CustomRdpProperty   : use multimon:i:0;
    MaxSessionLimit     : 10
    LoadBalancerType    : BreadthFirst
    ValidationEnv       : True
    Ring                :
```

## <a name="update-schedule"></a>Planning bijwerken

In de Preview-versie optreden service-updates ongeveer een maandelijkse uitgebracht. Als er ernstige problemen, essentiële updates ontvangt een vaker uitgebracht.

## <a name="next-steps"></a>Volgende stappen

Nu dat u een pool van de host validatie hebt gemaakt, leert u over het implementeren en verbinding maken met een hulpprogramma voor het beheren van virtuele bureaublad van Microsoft-resources.

> [!div class="nextstepaction"]
> [Zelfstudie: een hulpprogramma management implementeren](./manage-resources-using-ui.md)
