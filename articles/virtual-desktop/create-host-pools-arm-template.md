---
title: Een host-pool maken met een Azure Resource Manager-sjabloon (preview) - Azure
description: Het maken van een host van toepassingen in virtuele Windows-bureaublad met een Azure Resource Manager-sjabloon.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: 7f4849c19d56bb385e7ad3ce0aa95e16d5c53c23
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58318462"
---
# <a name="create-a-host-pool-with-an-azure-resource-manager-template-preview"></a>Een host-pool maken met een Azure Resource Manager-sjabloon (Preview)

Host-pools zijn een verzameling van een of meer identieke virtuele machines in virtuele Windows-bureaublad-tenant (preview)-omgevingen. Elke groep host kan een app-groep die gebruikers werken kunnen met net zoals op een fysieke bureaublad bevatten.

Volg de instructies in deze sectie van een groep host voor een virtuele Windows-bureaublad-tenant maken met een Azure Resource Manager-sjabloon die is geleverd door Microsoft. In dit artikel wordt uitgelegd hoe u een host-pool maakt in een virtuele Windows-bureaublad, een resourcegroep maken met virtuele machines in een Azure-abonnement, deze virtuele machines toevoegen aan de AD-domein en het registreren van de virtuele machines met virtuele Windows-bureaublad.

## <a name="what-you-need-to-run-the-azure-resource-manager-template"></a>Wat u nodig hebt om uit te voeren van de Azure Resource Manager-sjabloon

Zorg ervoor dat u de volgende dingen weten voordat u de Azure Resource Manager-sjabloon:

- Waarbij de bron van de afbeelding die u wilt gebruiken is. Is het uit Azure-galerie of aangepast is?
- Uw domeinreferenties join.
- De referenties voor uw virtuele Windows-bureaublad.

Wanneer u een virtuele Windows-bureaublad host-pool met de Azure Resource Manager-sjabloon maken, kunt u een virtuele machine van de Azure-galerie, een beheerde installatiekopie of een niet-beheerde installatiekopie maken. Zie voor meer informatie over het maken van VM-installatiekopieën, [voorbereiden van een Windows VHD of VHDX te uploaden naar Azure](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image) en [maken van een beheerde installatiekopie van een gegeneraliseerde VM in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource).

## <a name="run-the-azure-resource-manager-template-for-provisioning-a-new-host-pool"></a>De Azure Resource Manager-sjabloon voor het inrichten van een nieuwe groep van de host worden uitgevoerd

Als u wilt starten, gaat u naar [deze GitHub-URL](https://github.com/Azure/RDS-Templates/tree/master/wvd-templates/Create%20and%20provision%20WVD%20host%20pool).

### <a name="deploy-the-template-to-azure"></a>De sjabloon implementeren in Azure

Als u in een Enterprise-abonnement implementeert, schuif naar beneden en selecteer **implementeren in Azure**, klikt u vervolgens overslaan vooruit, vult u de parameters op basis van de bron van uw installatiekopie.

Als u in een Cloud Solution Provider-abonnement implementeert, volgt u deze stappen om te implementeren in Azure:

1. Schuif naar beneden en met de rechtermuisknop op **implementeren in Azure**en selecteer vervolgens **kopie koppelingslocatie**.
2. Open een teksteditor zoals Kladblok en plak er de koppeling.
3. Direct na het 'https://portal.azure.com/' en voert u vóór de hashtag (#) een apenstaartje (@) gevolgd door de naam van de tenant-domein. Hier volgt een voorbeeld van de indeling moet u: https://portal.azure.com/@Contoso.onmicrosoft.com#create/.
4. Meld u aan de Azure-portal aan als een gebruiker met Administrator-/ Inzenderrechten machtigingen voor de Cloud Solution Provider-abonnement.
5. Plak de koppeling die u hebt gekopieerd naar de teksteditor in de adresbalk.

Zie voor meer informatie over welke parameters u voor uw scenario invoeren moet het virtuele Windows-bureaublad [Leesmij-bestand](https://github.com/Azure/RDS-Templates/blob/master/wvd-templates/Create%20and%20provision%20WVD%20host%20pool/README.md). Het Leesmij-bestand wordt altijd bijgewerkt met de meest recente wijzigingen.

## <a name="assign-users-to-the-desktop-application-group"></a>Gebruikers toewijzen aan de groep bureaubladtoepassing

Nadat de GitHub Azure Resource Manager-sjabloon is voltooid, gebruikerstoegang toewijzen voordat u begint met het testen van de volledige sessie bureaubladen op uw virtuele machines.

Eerste, [downloaden en importeren van de Windows virtuele bureaublad PowerShell-module](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) te gebruiken in uw PowerShell-sessie als u dat nog niet gedaan hebt.

Voor gebruikers toewijzen aan de groep bureaubladtoepassing, open een PowerShell-venster en voer deze cmdlet te melden bij de virtuele Windows-bureaublad-omgeving:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

Vervolgens stelt u de context aan de tenant-groep die is opgegeven in de Azure Resource Manager-sjabloon met deze cmdlet:

```powershell
Set-RdsContext -TenantGroupName <Tenant Group name>
```

Hierna moet u gebruikers toevoegen aan de groep bureaubladtoepassing met deze cmdlet:

```powershell
Add-RdsAppGroupUser <tenantname> <hostpoolname> "Desktop Application Group" -UserPrincipalName <userupn>
```

UPN van de gebruiker moet overeenkomen met de identiteit van de gebruiker in Azure Active Directory (bijvoorbeeld user1@contoso.com). Als u wilt dat meerdere gebruikers toevoegt, moet u deze cmdlet voor elke gebruiker uitvoeren.

Nadat u deze stappen hebt voltooid, kunnen gebruikers die zijn toegevoegd aan de groep bureaubladtoepassing aanmelden bij virtuele Windows-bureaublad met ondersteunde extern bureaublad-clients en ziet u een resource voor een sessie-desktop.
