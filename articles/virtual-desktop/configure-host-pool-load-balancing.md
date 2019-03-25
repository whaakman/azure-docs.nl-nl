---
title: Configureren van de Windows virtuele bureaublad Preview load balancing - Azure
description: Klik hier voor meer informatie over het configureren van de taakverdelingsmethode voor een virtuele Windows-bureaublad-omgeving.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: 0c4702dada17e759d89c33be99b3155f4b15ad9e
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58399859"
---
# <a name="configure-the-windows-virtual-desktop-preview-load-balancing-method"></a>Configureren van de Windows virtuele bureaublad Preview load balancing-methode

Configureren van de taakverdelingsmethode voor een groep host, kunt u de Windows virtuele bureaublad Preview-omgeving aanpassen aan uw behoeften aanpassen.

>[!NOTE]
> Dit geldt niet aan een groep permanente bureaubladhost omdat gebruikers altijd beschikken over een 1:1 toewijzing aan een sessiehost in de pool host.

## <a name="configure-breadth-first-load-balancing"></a>Breedte op de eerste taakverdeling configureren

Breedte op de eerste load balancing is de standaardconfiguratie voor nieuwe niet-permanente host-pools. Breedte op de eerste load balancing, wordt nieuwe gebruikerssessies verspreid over alle beschikbare hosts in de groep host. Bij het configureren van breedte-first-taakverdeling, kunt u een limiet voor maximale sessie per sessiehost instellen in de groep host.

Eerste, [downloaden en importeren van de Windows virtuele bureaublad PowerShell-module](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) te gebruiken in uw PowerShell-sessie als u dat nog niet gedaan hebt.

Voer de volgende PowerShell-cmdlet voor het configureren van een groep host om uit te voeren van de breedte op de eerste load balancing-zonder het aanpassen van de limiet voor maximale sessie:

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -BreadthFirstLoadBalancer
```

Voer de volgende PowerShell-cmdlet voor het configureren van een groep host om uit te voeren taakverdeling breedte op de eerste en het gebruik van een nieuwe sessie van de maximale limiet:

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -BreadthFirstLoadBalancer -MaxSessionLimit ###
```

## <a name="configure-depth-first-load-balancing"></a>Diepte van de eerste taakverdeling configureren

Diepte van de eerste load balancer distribueert nieuwe gebruikerssessies op een sessiehost beschikbaar met het hoogste aantal verbindingen, maar heeft niet de limiet voor maximale sessie drempelwaarde bereikt. Bij het configureren van de diepte van de eerste load balancing u **moet** een maximale sessielimiet per sessiehost instellen in de groep host.

Voer de volgende PowerShell-cmdlet voor het configureren van een groep host om uit te voeren taakverdeling diepte van de eerste:

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -DepthFirstLoadBalancer -MaxSessionLimit ###
```
