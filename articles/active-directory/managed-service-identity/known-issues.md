---
title: Veelgestelde vragen en bekende problemen met Managed Service Identity (MSI) voor Azure Active Directory
description: Bekende problemen met de beheerde Service-identiteit voor Azure Active Directory.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.assetid: 2097381a-a7ec-4e3b-b4ff-5d2fb17403b6
ms.service: active-directory
ms.component: msi
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 12/12/2017
ms.author: daveba
ms.openlocfilehash: 05096050dfc29aebd2859b298eef884dcd9a1111
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2018
ms.locfileid: "37908075"
---
# <a name="faqs-and-known-issues-with-managed-service-identity-msi-for-azure-active-directory"></a>Veelgestelde vragen en bekende problemen met Managed Service Identity (MSI) voor Azure Active Directory

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

## <a name="frequently-asked-questions-faqs"></a>Veelgestelde vragen

### <a name="does-msi-work-with-azure-cloud-services"></a>Werkt MSI met Azure Cloud Services?

Nee, er zijn geen plannen voor de ondersteuning van MSI in Azure Cloud Services.

### <a name="does-msi-work-with-the-active-directory-authentication-library-adal-or-the-microsoft-authentication-library-msal"></a>Werkt MSI-bestand met de Active Directory Authentication Library (ADAL) of de Microsoft Authentication Library (MSAL)?

Nee, MSI-bestand niet nog is geïntegreerd met ADAL of MSAL. Zie voor meer informatie over het verkrijgen van een MSI-token met het MSI-REST-eindpunt, [over het gebruik van een Azure VM Managed Service Identity (MSI) voor het ophalen van tokens](how-to-use-vm-token.md).

### <a name="what-is-the-security-boundary-of-a-managed-service-identity"></a>Wat is de beveiligingsgrens van een beheerde Service-identiteit?

De beveiligingsgrens van de identiteit is de resource waaraan deze is gekoppeld. Bijvoorbeeld, is de beveiligingsgrens voor een virtuele Machine MSI-bestand, de virtuele Machine. Een code die wordt uitgevoerd op deze VM, kan het MSI-eindpunt aanroepen en aanvragen van tokens. Het is de vergelijkbare ervaring met andere resources die ondersteuning bieden voor MSI-bestand.

### <a name="should-i-use-the-msi-vm-imds-endpoint-or-the-msi-vm-extension-endpoint"></a>Moet ik het eindpunt van de MSI VM IMDS of het MSI-VM-extensie-eindpunt gebruiken?

Bij het gebruik van MSI-bestand met VM's, wordt u aangeraden met behulp van de MSI IMDS-eindpunt. De Azure Instance Metadata Service is een toegankelijk is voor alle IaaS-VM's die zijn gemaakt via de Azure Resource Manager REST-eindpunt. Enkele van de voordelen van het gebruik van MSI-bestand via IMDS zijn:

1. Alle besturingssystemen van Azure IaaS ondersteund kunt MSI-bestand gebruiken via IMDS. 
2. Niet meer nodig hebt voor het installeren van een uitbreiding op de virtuele machine om in te schakelen van MSI. 
3. De certificaten die worden gebruikt door MSI-bestand zijn niet meer aanwezig zijn in de virtuele machine. 
4. Het eindpunt IMDS is een bekende niet-routeerbare IP-adres, alleen beschikbaar vanuit de virtuele machine. 

De MSI-VM-extensie is nog steeds beschikbaar vandaag nog; worden gebruikt echter vooruit we standaard het IMDS-eindpunt. De MSI-VM-extensie wordt binnenkort gestart een afschaffing op. 

Zie voor meer informatie over Azure-instantie Metada Service [IMDS-documentatie](https://docs.microsoft.com/azure/virtual-machines/windows/instance-metadata-service)

### <a name="what-are-the-supported-linux-distributions"></a>Wat zijn de ondersteunde Linux-distributies?

Alle Linux-distributies ondersteund door Azure IaaS kunnen worden gebruikt met MSI-bestand via het eindpunt IMDS. 

Opmerking: De MSI-VM-extensie biedt alleen ondersteuning voor de volgende Linux-distributies:
- Stable van CoreOS
- CentOS 7.1
- Red Hat 7.2
- Ubuntu 15.04
- Ubuntu 16.04

Andere Linux-distributies worden momenteel niet ondersteund en extensie mislukken op niet-ondersteunde distributies.

De extensie werkt op CentOS 6,9. Echter, vanwege de afwezigheid van systeemondersteuning in 6,9, de uitbreiding wordt niet automatisch opnieuw starten als is vastgelopen of gestopt. Er wordt opnieuw opgestart wanneer de virtuele machine opnieuw wordt opgestart. Als u wilt de extensie handmatig opnieuw opstart, Zie [hoe doet u het MSI-extensie?](#how-do-you-restart-the-msi-extension)

### <a name="how-do-you-restart-the-msi-extension"></a>Hoe doet u het MSI-extensie
Op Windows en bepaalde versies van Linux, als de extensie wordt gestopt, kan de volgende cmdlet worden gebruikt om handmatig opnieuw starten:

```powershell
Set-AzureRmVMExtension -Name <extension name>  -Type <extension Type>  -Location <location> -Publisher Microsoft.ManagedIdentity -VMName <vm name> -ResourceGroupName <resource group name> -ForceRerun <Any string different from any last value used>
```

Waar: 
- Naam van de uitbreiding en het type voor Windows is: ManagedIdentityExtensionForWindows
- Naam van de uitbreiding en het type voor Linux is: ManagedIdentityExtensionForLinux

## <a name="known-issues"></a>Bekende problemen

### <a name="automation-script-fails-when-attempting-schema-export-for-msi-extension"></a>"Automatiseringsscript" is mislukt bij het schema exporteren voor MSI-extensie

Als beheerde Service-identiteit is ingeschakeld op een virtuele machine, worden de volgende fout wordt weergegeven bij het gebruik van de functie 'Automatiseringsscript' voor de virtuele machine of de resourcegroep:

![MSI automation scriptfout exporteren](../media/msi-known-issues/automation-script-export-error.png)

De beheerde Service-identiteit VM-extensie biedt momenteel geen ondersteuning voor de mogelijkheid waarvan het schema exporteren naar een resourcegroepsjabloon. Als gevolg hiervan weergegeven de gegenereerde sjabloon niet. configuratieparameters beheerde Service-identiteit inschakelen op de resource. Deze secties kunnen handmatig worden toegevoegd aan de hand van de voorbeelden in [configureren van een virtuele machine beheerde Service-identiteit met een sjabloon](qs-configure-template-windows-vm.md).

Wanneer de schema-export-functionaliteit beschikbaar is voor de MSI-VM-extensie, wordt deze weergegeven in [exporteren van resourcegroepen met VM-extensies](../../virtual-machines/extensions/export-templates.md#supported-virtual-machine-extensions).

### <a name="configuration-blade-does-not-appear-in-the-azure-portal"></a>Configuratieblade wordt niet weergegeven in de Azure-portal

Als de VM-configuratie-blade op de virtuele machine niet wordt weergegeven, klikt u vervolgens is MSI-bestand niet ingeschakeld in de portal in uw regio nog.  Probeer het later opnieuw.  U kunt ook MSI inschakelen voor uw virtuele machine met [PowerShell](qs-configure-powershell-windows-vm.md) of de [Azure CLI](qs-configure-cli-windows-vm.md).

### <a name="cannot-assign-access-to-virtual-machines-in-the-access-control-iam-blade"></a>Kan geen toegang toewijzen aan virtuele machines in de blade van de Access Control (IAM)

Als **virtuele Machine** wordt niet weergegeven in de Azure-portal als een keuze voor **toegang toewijzen aan** in **Access Control (IAM)** > **toevoegen machtigingen**, en vervolgens de beheerde Service-identiteit is nog niet ingeschakeld in de portal in uw regio. Probeer het later opnieuw.  U kunt nog steeds de toewijzing van de rol van de door de beheerde Service-identiteit selecteren door te zoeken voor het MSI Service-Principal.  Voer de naam van de virtuele machine in de **Selecteer** veld en de Service-Principal wordt weergegeven in de zoekresultaten.

### <a name="vm-fails-to-start-after-being-moved-from-resource-group-or-subscription"></a>Virtuele machine niet kan worden gestart na worden verplaatst van resourcegroep of abonnement

Als u een virtuele machine in de status running doorbrengt verplaatst, blijft tijdens het verplaatsen uitgevoerd. Echter na de verplaatsing mislukt als de virtuele machine wordt gestopt en opnieuw opgestart, dit om te starten. Dit probleem treedt op omdat de virtuele machine niet voor de verwijzing naar de identiteit van de MSI-bestand bijwerken is en om te verwijzen naar deze in de oude resourcegroep blijft.

**Tijdelijke oplossing** 
 
Trigger voor een update voor de virtuele machine, zodat de juiste waarden voor het MSI-bestand kan worden opgehaald. U kunt de wijziging van een VM-eigenschap voor het bijwerken van de verwijzing naar de identiteit van de MSI-bestand kunt doen. U kunt bijvoorbeeld een nieuwe tagwaarde instellen op de virtuele machine met de volgende opdracht:

```azurecli-interactive
 az  vm update -n <VM Name> -g <Resource Group> --set tags.fixVM=1
```
 
Met deze opdracht wordt een nieuwe tag 'fixVM' met een waarde van 1 ingesteld op de virtuele machine. 
 
Deze eigenschap instelt, de virtuele machine wordt bijgewerkt met de juiste MSI-resource-URI en vervolgens moet u de virtuele machine gestart. 
 
Nadat de virtuele machine is gestart, kan de tag worden verwijderd met behulp van de volgende opdracht:

```azurecli-interactive
az vm update -n <VM Name> -g <Resource Group> --remove tags.fixVM
```

## <a name="known-issues-with-user-assigned-identities"></a>Bekende problemen met de gebruiker toegewezen identiteiten

- Gebruiker toegewezen identiteit toewijzingen zijn alleen beschikbaar voor virtuele machine en VMSS. Belangrijk: Gebruiker toegewezen identiteit toewijzingen in de komende maanden wordt gewijzigd.
- Dubbele gebruiker toegewezen identiteiten op dezelfde VM/VMSS, zorgt ervoor dat de VM/VMSS mislukken. Dit omvat identiteiten die worden toegevoegd met een ander hoofdlettergebruik. bijvoorbeeld MyUserAssignedIdentity en myuserassignedidentity. 
- Inrichting van de VM-extensie aan een virtuele machine kan mislukken vanwege DNS-lookup-fouten. Start de VM opnieuw op en probeer het opnieuw. 
- Toevoegen van een "niet-bestaande" door gebruiker toegewezen identiteit zorgt ervoor dat de virtuele machine mislukt. 
- Het maken van een gebruiker toegewezen identiteit met speciale tekens (dat wil zeggen onderstrepingstekens) in de naam, wordt niet ondersteund.
- Door gebruiker toegewezen Identiteitsnamen zijn beperkt tot 24 tekens in voor de end-to-end-scenario. Gebruiker toegewezen identiteiten met namen die langer dan 24 tekens worden niet worden toegewezen.
- Als de extensie van de virtuele machine beheerde identiteit wordt gebruikt, is de ondersteunde limiet van 32 gebruiker beheerde identiteiten toegewezen. De ondersteunde limiet is zonder de extensie van de virtuele machine beheerde identiteit 512.  
- Bij het toevoegen van een tweede gebruiker toegewezen identiteit, de clientID mogelijk niet beschikbaar is voor aanvragen tokens voor de VM-extensie. Als een beperking, opnieuw starten de MSI-VM-extensie met de volgende twee bash-opdrachten:
 - `sudo bash -c "/var/lib/waagent/Microsoft.ManagedIdentity.ManagedIdentityExtensionForLinux-1.0.0.8/msi-extension-handler disable"`
 - `sudo bash -c "/var/lib/waagent/Microsoft.ManagedIdentity.ManagedIdentityExtensionForLinux-1.0.0.8/msi-extension-handler enable"`
- Wanneer een virtuele machine heeft een gebruiker toegewezen identiteit, maar er zijn geen systeem toegewezen identiteit, de portal UI geeft MSI als uitgeschakeld. Om in te schakelen op het systeem toegewezen identiteit, een Azure Resource Manager-sjabloon, een Azure-CLI of een SDK te gebruiken.
