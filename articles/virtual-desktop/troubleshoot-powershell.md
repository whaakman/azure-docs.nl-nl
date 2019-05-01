---
title: PowerShell in Windows Virtual PC - Azure
description: Klik hier voor meer informatie over het oplossen van problemen met PowerShell bij het instellen van een omgeving met virtuele Windows-bureaublad tenants.
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: troubleshoot
ms.date: 04/08/2019
ms.author: v-chjenk
ms.openlocfilehash: ad32f7ff883812830dbcf2ed900c4034bd90abfc
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64927505"
---
# <a name="windows-virtual-desktop-powershell"></a>Windows virtuele bureaublad PowerShell

Gebruik dit artikel om op te lossen fouten en problemen bij het gebruik van PowerShell met virtuele Windows-bureaublad. Zie voor meer informatie over extern bureaublad-Services PowerShell [Windows virtuele bureaublad Powershell](https://docs.microsoft.com/powershell/module/windowsvirtualdesktop/).

## <a name="provide-feedback"></a>Feedback geven

We zijn niet op dit moment kwesties duurt zolang virtuele Windows-bureaublad in preview. Ga naar de [Windows virtuele bureaublad Tech Community](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) bespreken van de virtuele Windows-bureaublad-service met het productteam en actieve communityleden.

## <a name="powershell-commands-used-during-windows-virtual-desktop-setup"></a>PowerShell-opdrachten gebruikt tijdens de installatie van de virtuele Windows-bureaublad

Deze sectie vindt u PowerShell-opdrachten die meestal worden gebruikt tijdens het instellen van virtuele Windows-bureaublad en biedt methoden voor het oplossen van problemen die optreden tijdens het gebruik ervan.

### <a name="error-add-rdsappgroupuser-command----the-specified-userprincipalname-is-already-assigned-to-a-remoteapp-app-group-in-the-specified-host-pool"></a>Fout: Opdracht voor toevoegen RdsAppGroupUser--de opgegeven UserPrincipalName is al toegewezen aan een RemoteApp-app-groep in de opgegeven Host-groep

```Powershell
Add-RdsAppGroupUser -TenantName <TenantName> -HostPoolName <HostPoolName> -AppGroupName 'Desktop Application Group' -UserPrincipalName <UserName>
```

**Oorzaak:** De gebruikersnaam die wordt gebruikt is al toegewezen aan een app-groep van een ander type. Gebruikers kunnen niet worden toegewezen aan zowel een extern bureaublad en externe app-groep onder de dezelfde session host-groep.

**Fix:** Als de gebruiker moet zowel externe apps en extern bureaublad, maken van groepen van de andere host of gebruikerstoegang geven tot de extern bureaublad, die het gebruik van een toepassing op de sessiehost-VM van de.

### <a name="error-add-rdsappgroupuser-command----the-specified-userprincipalname-doesnt-exist-in-the-azure-active-directory-associated-with-the-remote-desktop-tenant"></a>Fout: Opdracht voor toevoegen RdsAppGroupUser--de opgegeven UserPrincipalName bestaat niet in de Azure Active Directory die zijn gekoppeld aan de extern bureaublad-tenant

```PowerShell
Add-RdsAppGroupUser -TenantName <TenantName> -HostPoolName <HostPoolName> -AppGroupName “Desktop Application Group” -UserPrincipalName <UserPrincipalName>
```

**Oorzaak:** De gebruiker die is opgegeven door de UserPrincipalName - kan niet worden gevonden in de Azure Active Directory gekoppeld aan de tenant virtuele Windows-bureaublad.

**Fix:** Controleer of de items in de volgende lijst.

- De gebruiker is gesynchroniseerd met Azure Active Directory.
- De gebruiker is niet gebonden aan business-to-consumer (B2C) of -business-to-business (B2B) commerce.
- De virtuele Windows-bureaublad-tenant is gekoppeld aan de juiste Azure Active Directory.

### <a name="error-get-rdsdiagnosticactivities----user-isnt-authorized-to-query-the-management-service"></a>Fout: Get-RdsDiagnosticActivities--Gebruiker is niet geautoriseerd om op te vragen van de management-service

```PowerShell
Get-RdsDiagnosticActivities -ActivityId <ActivityId>
```

**Oorzaak:** - Tenantnaam parameter

**Fix:** Get-RdsDiagnosticActivities met - Tenantnaam uitgeven <TenantName>.

### <a name="error-get-rdsdiagnosticactivities----the-user-isnt-authorized-to-query-the-management-service"></a>Fout: Get-RdsDiagnosticActivities--de gebruiker is niet geautoriseerd om op te vragen van de management-service

```PowerShell
Get-RdsDiagnosticActivities -Deployment -username <username>
```

**Oorzaak:** Met de parameter - implementatie-switch.

**FIX:** -implementatie-switch kan alleen door beheerders van de implementatie worden gebruikt. Deze beheerders zijn meestal leden van het team extern bureaublad Services/Windows virtuele bureaublad. Vervang de - implementatie-switch met - Tenantnaam <TenantName>.

### <a name="error-new-rdsroleassignment----the-user-isnt-authorized-to-query-the-management-service"></a>Fout: Nieuwe-RdsRoleAssignment--de gebruiker is niet geautoriseerd om op te vragen van de management-service

**1 oorzaak:** Het gebruikte account geen extern bureaublad-Services eigenaarsmachtigingen voor de tenant.

**Fix 1:** Er moet een gebruiker met eigenaarsmachtigingen voor extern bureaublad-Services voor het uitvoeren van de roltoewijzing.

**2 oorzaak:** Het gebruikte account eigenaarsmachtigingen voor extern bureaublad-Services heeft maar geen deel uitmaken van een van de tenant Azure Active Directory of beschikt niet over machtigingen om op te vragen van de Azure Active Directory waar de gebruiker zich bevindt.

**Fix 2:** Er moet een gebruiker met machtigingen voor Active Directory voor het uitvoeren van de roltoewijzing.

>[!Note]
>Nieuwe RdsRoleAssignment kan geen machtigingen geven aan een gebruiker die niet in de Azure Active Directory (AD bestaat).

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht over het oplossen van virtuele Windows-bureaublad en de sporen escalatie [overzicht, feedback en ondersteuning voor probleemoplossing](troubleshoot-set-up-overview.md).
- Zie voor het oplossen van problemen tijdens het maken van een tenant en host-pool in een omgeving met virtuele Windows-bureaublad, [Tenant en de host een pool maken](troubleshoot-set-up-issues.md).
- Zie voor het oplossen van problemen tijdens het configureren van een virtuele machine (VM) in virtuele Windows-bureaublad, [Session host Virtuele-machineconfiguratie](troubleshoot-vm-configuration.md).
- Zie voor het oplossen van problemen met virtuele Windows-bureaublad-clientverbindingen, [verbindingen met extern bureaublad-client](troubleshoot-client-connection.md).
- Zie voor meer informatie over de Preview-service, [Windows Desktop Preview-omgeving](https://docs.microsoft.com/azure/virtual-desktop/environment-setup).
- Als u wilt u een zelfstudie voor problemen oplossen, Zie [zelfstudie: Problemen met sjabloonimplementaties van Resource Manager-oplossen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-tutorial-troubleshoot).
- Zie voor meer informatie over het controleren van acties, [bewerkingen controleren met Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit).
- Zie voor meer informatie over acties voor het bepalen van de fouten tijdens de implementatie, [implementatiebewerkingen bekijken](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-operations).