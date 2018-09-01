---
title: Veelgestelde vragen en bekende problemen met beheerde identiteiten voor Azure-resources
description: Bekende problemen met beheerde identiteiten voor Azure-resources.
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
ms.openlocfilehash: 72f705e130dc60559651c9478052416294bc31a4
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43344651"
---
# <a name="faqs-and-known-issues-with-managed-identities-for-azure-resources"></a>Veelgestelde vragen en bekende problemen met beheerde identiteiten voor Azure-resources

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

## <a name="frequently-asked-questions-faqs"></a>Veelgestelde vragen

### <a name="does-managed-identities-for-azure-resources-work-with-azure-cloud-services"></a>Werkt beheerde identiteiten voor Azure-resources met Azure Cloud Services?

Nee, er zijn geen plannen voor de ondersteuning van beheerde identiteiten voor Azure-resources in Azure Cloud Services.

### <a name="does-managed-identities-for-azure-resources-work-with-the-active-directory-authentication-library-adal-or-the-microsoft-authentication-library-msal"></a>Werkt beheerde identiteiten voor Azure-resources met de Active Directory Authentication Library (ADAL) of de Microsoft Authentication Library (MSAL)?

Nee, beheerde identiteiten voor Azure-resources nog niet is geïntegreerd met ADAL of MSAL. Zie voor meer informatie over het ophalen van een token voor beheerde identiteiten voor Azure-resources met behulp van de REST-eindpunt, [over het gebruik van beheerde identiteiten voor Azure-resources op een Azure-VM aan een toegangstoken verkrijgen ](how-to-use-vm-token.md).

### <a name="what-is-the-security-boundary-of-managed-identities-for-azure-resources"></a>Wat is de beveiligingsgrens van beheerde identiteiten voor Azure-resources?

De beveiligingsgrens van de identiteit is de resource waaraan deze is gekoppeld. Bijvoorbeeld, de beveiligingsgrens voor een virtuele Machine met beheerde identiteiten voor Azure-resources ingeschakeld, wordt de virtuele Machine. Een code die wordt uitgevoerd op deze VM is kan aanroepen van de beheerde identiteiten voor Azure-resources eindpunt en de aanvraag-tokens. Het is de vergelijkbare ervaring met andere resources die ondersteuning bieden voor beheerde identiteiten voor Azure-resources.

### <a name="should-i-use-the-managed-identities-for-azure-resources-vm-imds-endpoint-or-the-vm-extension-endpoint"></a>Moet ik de beheerde identiteiten voor Azure-resources VM IMDS eindpunt of het eindpunt van de VM-extensie gebruiken?

Bij gebruik van beheerde identiteiten voor Azure-resources met virtuele machines, wordt u aangeraden met behulp van de beheerde identiteiten voor Azure-resources IMDS eindpunt. De Azure Instance Metadata Service is een toegankelijk is voor alle IaaS-VM's die zijn gemaakt via de Azure Resource Manager REST-eindpunt. Enkele van de voordelen van het gebruik van beheerde identiteiten voor Azure-resources via IMDS zijn:

1. Alle besturingssystemen van Azure IaaS ondersteund kunt beheerde identiteiten gebruiken voor Azure-resources via IMDS. 
2. Niet meer hoeft te installeren van een uitbreiding op de virtuele machine om in te schakelen van beheerde identiteiten voor Azure-resources. 
3. De certificaten die op beheerde identiteiten voor Azure-resources zijn niet meer aanwezig zijn in de virtuele machine. 
4. Het eindpunt IMDS is een bekende niet-routeerbare IP-adres, alleen beschikbaar vanuit de virtuele machine. 

De beheerde identiteiten voor VM-extensie nog steeds beschikbaar is voor gebruik vandaag nog; Azure-resources echter vooruit we standaard het IMDS-eindpunt. De beheerde identiteit voor Azure-resources VM-extensie wordt gestart op een afschaffing plan snel. 

Zie voor meer informatie over Azure Instance Metadata Service [IMDS-documentatie](https://docs.microsoft.com/azure/virtual-machines/windows/instance-metadata-service)

### <a name="what-are-the-supported-linux-distributions"></a>Wat zijn de ondersteunde Linux-distributies?

Alle Linux-distributies ondersteund door Azure IaaS kunnen worden gebruikt met beheerde identiteiten voor Azure-resources via de IMDS-eindpunt. 

Opmerking: De beheerde identiteit voor Azure-resources ondersteunt alleen de volgende Linux-distributies VM-extensie:
- Stable van CoreOS
- CentOS 7.1
- Red Hat 7.2
- Ubuntu 15.04
- Ubuntu 16.04

Andere Linux-distributies worden momenteel niet ondersteund en extensie mislukken op niet-ondersteunde distributies.

De extensie werkt op CentOS 6,9. Echter, vanwege de afwezigheid van systeemondersteuning in 6,9, de uitbreiding wordt niet automatisch opnieuw starten als is vastgelopen of gestopt. Er wordt opnieuw opgestart wanneer de virtuele machine opnieuw wordt opgestart. Als u wilt de extensie handmatig opnieuw opstart, Zie [hoe doet u het beheerde identiteiten voor uitbreiding van de Azure-resources?](#how-do-you-restart-the-managed-identities-for-Azure-resources-extension)

### <a name="how-do-you-restart-the-managed-identities-for-azure-resources-extension"></a>Hoe start u de beheerde identiteit voor Azure-resources-uitbreiding opnieuw?
Op Windows en bepaalde versies van Linux, als de extensie wordt gestopt, kan de volgende cmdlet worden gebruikt om handmatig opnieuw starten:

```powershell
Set-AzureRmVMExtension -Name <extension name>  -Type <extension Type>  -Location <location> -Publisher Microsoft.ManagedIdentity -VMName <vm name> -ResourceGroupName <resource group name> -ForceRerun <Any string different from any last value used>
```

Waar: 
- Naam van de uitbreiding en het type voor Windows is: ManagedIdentityExtensionForWindows
- Naam van de uitbreiding en het type voor Linux is: ManagedIdentityExtensionForLinux

## <a name="known-issues"></a>Bekende problemen

### <a name="automation-script-fails-when-attempting-schema-export-for-managed-identities-for-azure-resources-extension"></a>"Automatiseringsscript" is mislukt bij het schema exporteren voor beheerde identiteiten voor uitbreiding van de Azure-resources

Als beheerde identiteiten voor Azure-resources op een virtuele machine is ingeschakeld, wordt de volgende fout weergegeven bij het gebruik van de functie 'Automatiseringsscript' voor de virtuele machine of de resourcegroep:

![Beheerde identiteiten voor een Azure-resources automatiseringsscript exportfout](../managed-service-identity/media/msi-known-issues/automation-script-export-error.png)

De beheerde identiteiten voor VM-extensie voor Azure-resources biedt momenteel geen ondersteuning voor de mogelijkheid waarvan het schema exporteren naar een resourcegroepsjabloon. Als gevolg hiervan weergegeven de gegenereerde sjabloon niet. configuratieparameters voor het inschakelen van beheerde identiteiten voor Azure-resources op de resource. Deze secties kunnen handmatig worden toegevoegd aan de hand van de voorbeelden in [configureren beheerde identiteiten voor een Azure-resources op een Azure-VM met behulp van een sjablonen](qs-configure-template-windows-vm.md).

Wanneer de schema-export-functionaliteit beschikbaar is voor de beheerde identiteiten voor VM-extensie voor Azure-resources, wordt deze weergegeven in [exporteren van resourcegroepen met VM-extensies](../../virtual-machines/extensions/export-templates.md#supported-virtual-machine-extensions).

### <a name="configuration-blade-does-not-appear-in-the-azure-portal"></a>Configuratieblade wordt niet weergegeven in de Azure-portal

Als de VM-configuratie-blade op de virtuele machine niet wordt weergegeven, klikt u vervolgens beheerde identiteiten voor Azure-resources niet is ingeschakeld in de portal in uw regio nog.  Probeer het later opnieuw.  U kunt ook beheerde identiteiten voor Azure-resources inschakelen voor uw virtuele machine met [PowerShell](qs-configure-powershell-windows-vm.md) of de [Azure CLI](qs-configure-cli-windows-vm.md).

### <a name="cannot-assign-access-to-virtual-machines-in-the-access-control-iam-blade"></a>Kan geen toegang toewijzen aan virtuele machines in de blade van de Access Control (IAM)

Als **virtuele Machine** wordt niet weergegeven in de Azure-portal als een keuze voor **toegang toewijzen aan** in **Access Control (IAM)** > **toevoegen machtigingen**, en vervolgens de beheerde identiteiten voor Azure-resources is nog niet ingeschakeld in de portal in uw regio. Probeer het later opnieuw.  U kunt nog steeds de identiteit voor de virtuele machine voor de roltoewijzing selecteren door te zoeken naar de beheerde identiteit voor Azure-resources Service-Principal.  Voer de naam van de virtuele machine in de **Selecteer** veld en de Service-Principal wordt weergegeven in de zoekresultaten.

### <a name="vm-fails-to-start-after-being-moved-from-resource-group-or-subscription"></a>Virtuele machine niet kan worden gestart na worden verplaatst van resourcegroep of abonnement

Als u een virtuele machine in de status running doorbrengt verplaatst, blijft tijdens het verplaatsen uitgevoerd. Echter na de verplaatsing mislukt als de virtuele machine wordt gestopt en opnieuw opgestart, dit om te starten. Dit probleem treedt op omdat de virtuele machine niet voor de verwijzing naar de beheerde identiteit voor de id van de Azure-resources bijwerken is en wijst u deze in de oude resourcegroep wordt voortgezet.

**Tijdelijke oplossing** 
 
Trigger voor een update voor de virtuele machine, zodat de juiste waarden voor de beheerde identiteit voor Azure-resources kunnen worden opgehaald. U kunt de wijziging van een VM-eigenschap voor het bijwerken van de verwijzing naar de beheerde identiteit voor de id van de Azure-resources kunt doen. U kunt bijvoorbeeld een nieuwe tagwaarde instellen op de virtuele machine met de volgende opdracht:

```azurecli-interactive
 az  vm update -n <VM Name> -g <Resource Group> --set tags.fixVM=1
```
 
Met deze opdracht wordt een nieuwe tag 'fixVM' met een waarde van 1 ingesteld op de virtuele machine. 
 
Deze eigenschap instelt, de virtuele machine wordt bijgewerkt met de juiste beheerde identiteit voor Azure-resources resource-URI en vervolgens moet u de virtuele machine gestart. 
 
Nadat de virtuele machine is gestart, kan de tag worden verwijderd met behulp van de volgende opdracht:

```azurecli-interactive
az vm update -n <VM Name> -g <Resource Group> --remove tags.fixVM
```

## <a name="known-issues-with-user-assigned-identities"></a>Bekende problemen met de gebruiker toegewezen identiteiten

- de gebruiker toegewezen identiteit toewijzingen zijn alleen beschikbaar voor virtuele machine en VMSS. Belangrijk: de gebruiker toegewezen identiteit toewijzingen in de komende maanden wordt gewijzigd.
- Dubbele gebruiker toegewezen identiteiten op de dezelfde VM/VMSS, zorgt ervoor dat de VM/VMSS mislukken. Dit omvat identiteiten die worden toegevoegd met een ander hoofdlettergebruik. bijvoorbeeld MyUserAssignedIdentity en myuserassignedidentity. 
- Inrichting van de VM-extensie aan een virtuele machine kan mislukken vanwege DNS-lookup-fouten. Start de VM opnieuw op en probeer het opnieuw. 
- Toevoegen van een 'niet-bestaande' door de gebruiker toegewezen identiteit zorgt ervoor dat de virtuele machine mislukt. 
- Het maken van een gebruiker toegewezen identiteit met speciale tekens (dat wil zeggen onderstrepingstekens) in de naam, wordt niet ondersteund.
- Identiteitsnamen van de gebruiker toegewezen zijn beperkt tot 24 tekens in voor de end-to-end-scenario. de gebruiker toegewezen identiteiten met namen die langer dan 24 tekens worden niet worden toegewezen.
- Als de extensie van de virtuele machine beheerde identiteit wordt gebruikt, is de ondersteunde limiet van 32 gebruiker toegewezen beheerde identiteiten. De ondersteunde limiet is zonder de extensie van de virtuele machine beheerde identiteit 512.  
- Bij het toevoegen van een tweede gebruiker toegewezen identiteit, de clientID mogelijk niet beschikbaar is voor aanvragen tokens voor de VM-extensie. Als een risicobeperking start opnieuw op de beheerde identiteiten voor VM-extensie voor Azure-resources met de volgende twee bash-opdrachten:
 - `sudo bash -c "/var/lib/waagent/Microsoft.ManagedIdentity.ManagedIdentityExtensionForLinux-1.0.0.8/msi-extension-handler disable"`
 - `sudo bash -c "/var/lib/waagent/Microsoft.ManagedIdentity.ManagedIdentityExtensionForLinux-1.0.0.8/msi-extension-handler enable"`
- Wanneer een virtuele machine een gebruiker toegewezen identiteit, maar geen systeem toegewezen identiteit heeft, worden in de portal gebruikersinterface ziet identiteiten voor een Azure-resources beheerd uitgeschakeld. Om in te schakelen op het systeem toegewezen identiteit, een Azure Resource Manager-sjabloon, een Azure-CLI of een SDK te gebruiken.
