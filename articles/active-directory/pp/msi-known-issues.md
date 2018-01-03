---
title: Veelgestelde vragen en bekende problemen met beheerde Service identiteit (MSI) voor Azure Active Directory
description: Bekende problemen met de Service-identiteit beheerd voor Azure Active Directory.
services: active-directory
documentationcenter: 
author: BryanLa
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: identity
ms.date: 12/15/2017
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 7a71010567a76569da969db3d53f71535f96f2d0
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2017
---
# <a name="faq-and-known-issues-with-managed-service-identity-msi-for-azure-active-directory"></a>Veelgestelde vragen en bekende problemen met beheerde Service identiteit (MSI) voor Azure Active Directory

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="does-msi-work-with-azure-cloud-services"></a>Werkt MSI met Azure Cloud Services?

Nee, er zijn geen plannen voor de ondersteuning van MSI in Azure Cloud Services.

### <a name="does-msi-work-with-the-active-directory-authentication-library-adal-or-the-microsoft-authentication-library-msal"></a>Werkt MSI met Active Directory Authentication Library (ADAL) of de Microsoft Authentication Library (MSAL)?

Nee, MSI niet nog geïntegreerd met ADAL of MSAL. Zie voor meer informatie over het verkrijgen van een MSI-token met behulp van de REST van de MSI-eindpunt [het gebruik van een Azure VM beheerde Service identiteit (MSI) voor de aanschaf van token](msi-how-to-use-vm-msi-token.md).

### <a name="what-are-the-supported-linux-distributions"></a>Wat zijn de ondersteunde Linux-distributies?

De volgende Linux-distributies ondersteunen MSI: 

- Virtuele CoreOS stabiel
- CentOS 7.1
- 7.2 RedHat
- Ubuntu 15.04

Andere Linux-distributies worden momenteel niet ondersteund en extensie mislukken op niet-ondersteunde distributies.

De extensie werkt op CentOS 6,9. Echter, vanwege een gebrek aan systeemondersteuning in 6,9, de uitbreiding wordt niet automatisch opnieuw starten als vastgelopen of gestopt. Opnieuw wordt gestart wanneer de virtuele machine opnieuw wordt opgestart. Zie de extensie opnieuw [hoe doet u het MSI-extensie?](#how-do-you-restart-the-msi-extension)

### <a name="how-do-you-restart-the-msi-extension"></a>Hoe u de MSI-uitbreiding opnieuw opstarten?
Voor Windows en bepaalde versies van Linux als de extensie stopt, kan de volgende cmdlet worden gebruikt voor handmatig opnieuw starten:

```powershell
Set-AzureRmVMExtension -Name <extension name>  -Type <extension Type>  -Location <location> -Publisher Microsoft.ManagedIdentity -VMName <vm name> -ResourceGroupName <resource group name> -ForceRerun <Any string different from any last value used>
```

Waar: 
- Naam en type voor Windows is: ManagedIdentityExtensionForWindows
- Extensienaam en het type voor Linux: ManagedIdentityExtensionForLinux

## <a name="known-issues"></a>Bekende problemen

### <a name="automation-script-fails-when-attempting-schema-export-for-msi-extension"></a>'Automatiseringsscript' mislukt bij een poging voor MSI-extensie voor schema-export

Wanneer de Service-identiteit beheerd op een virtuele machine is ingeschakeld, wordt de volgende fout weergegeven bij een poging tot het gebruik van de functie 'Automatiseringsscript' voor de virtuele machine of de resourcegroep:

![Fout bij het MSI automation script exporteren](~/articles/active-directory/media/msi-known-issues/automation-script-export-error.png)

De beheerde Service identiteit van VM-extensie biedt momenteel geen ondersteuning voor de mogelijkheid om het schema aan een resource group-sjabloon. Hierdoor worden de gegenereerde sjabloon geen configuratieparameters in te schakelen beheerde Service-identiteit voor de resource weergegeven. Deze secties kunnen handmatig worden toegevoegd aan de hand van de voorbeelden in [een VM beheerde Service-identiteit configureren met behulp van een sjabloon](msi-qs-configure-template-windows-vm.md).

Zodra de schema-export-functionaliteit beschikbaar voor de MSI-VM-extensie, wordt het weergegeven in [exporteren resourcegroepen met VM-extensies](~/articles/virtual-machines/windows/extensions-export-templates.md#supported-virtual-machine-extensions).

### <a name="configuration-blade-does-not-appear-in-the-azure-portal"></a>Configuratie-blade niet wordt weergegeven in de Azure-portal

Als de blade VM-configuratie op de virtuele machine niet wordt weergegeven, klikt u vervolgens is MSI niet ingeschakeld in de portal in uw regio nog.  Controleer het later opnieuw.  U kunt ook MSI inschakelen voor uw virtuele machine met [PowerShell](msi-qs-configure-powershell-windows-vm.md) of de [Azure CLI](msi-qs-configure-cli-windows-vm.md).

### <a name="cannot-assign-access-to-virtual-machines-in-the-access-control-iam-blade"></a>Kan geen toegang toewijzen aan virtuele machines in de blade Access Control (IAM)

Als **virtuele Machine** niet wordt weergegeven in de Azure-portal als keuze voor **toewijzen van toegang tot** in **Access Control (IAM)** > **toevoegen machtigingen**, en vervolgens de beheerde Service-identiteit is niet ingeschakeld in de portal in uw regio nog. Controleer het later opnieuw.  U kunt nog steeds de identiteit van de Service worden beheerd voor de roltoewijzing selecteren door te zoeken voor het MSI Service-Principal.  Voer de naam van de virtuele machine in de **Selecteer** veld en de Service-Principal wordt weergegeven in de zoekresultaten.

### <a name="vm-fails-to-start-after-being-moved-from-resource-group-or-subscription"></a>Virtuele machine niet start na worden verplaatst van resourcegroep of abonnement

Als u een virtuele machine in de actieve status verplaatst, blijft deze tijdens het verplaatsen uitgevoerd. Echter, na het verplaatsen als de virtuele machine is gestopt en opnieuw is opgestart, niet worden gestart. Dit probleem treedt op omdat de virtuele machine niet de verwijzing naar de identiteit van de MSI wordt bijgewerkt en wijs in het oude resourcegroep blijft.

**Tijdelijke oplossing** 
 
Een update op de virtuele machine activeren zodat de juiste waarden voor het MSI-bestand kan worden opgehaald. U kunt de wijziging van een VM-eigenschap voor het bijwerken van de verwijzing naar de identiteit van de MSI doen. U kunt bijvoorbeeld een nieuwe tagwaarde instellen op de virtuele machine met de volgende opdracht:

```azurecli-interactive
 az  vm update -n <VM Name> -g <Resource Group> --set tags.fixVM=1
```
 
Met deze opdracht wordt een nieuwe tag 'fixVM' met een waarde van 1 ingesteld op de virtuele machine. 
 
Deze eigenschap instelt, de virtuele machine wordt bijgewerkt met de juiste MSI-bron-URI en vervolgens moet u kunnen de virtuele machine te starten. 
 
Nadat de virtuele machine is gestart, kan de code kan worden verwijderd via de volgende opdracht:

```azurecli-interactive
az vm update -n <VM Name> -g <Resource Group> --remove tags.fixVM
```

## <a name="known-issues-with-user-assigned-msi-private-preview-feature"></a>Bekende problemen met de gebruiker toegewezen MSI *(Private Preview-functie)*

- De enige manier om alle gebruiker toegewezen MSI-bestanden te verwijderen is doordat het systeem MSI toegewezen. 
- Het inrichten van de VM-extensie voor een virtuele machine kan mislukken als gevolg van DNS-lookup-fouten. Opnieuw opstarten van de virtuele machine en probeer het opnieuw. 
- Azure CLI: `Az resource show` en `Az resource list` op een virtuele machine mislukt met MSI toegewezen door een gebruiker. Als tijdelijke oplossing gebruiken`az vm/vmss show`
- Zelfstudie voor Azure Storage is alleen beschikbaar in de centrale EUAP ons op dit moment. 
- Als een gebruiker toegewezen MSI is toegang tot een resource verleend, ziet u de IAM-blade voor die bron 'Kan niet voor toegang tot gegevens'. Gebruik als tijdelijke oplossing de CLI roltoewijzingen voor die bron weergeven/bewerken.
- U maakt een MSI toegewezen met een onderstrepingsteken in de naam van de gebruiker, wordt niet ondersteund.
- Wanneer een tweede gebruiker toe te voegen identiteit toegewezen, de clientID mogelijk niet beschikbaar voor het aanvragen van tokens voor. Als een risicobeperking start opnieuw op de MSI-VM-extensie met de volgende twee bash-opdrachten:
 - `sudo bash -c "/var/lib/waagent/Microsoft.ManagedIdentity.ManagedIdentityExtensionForLinux-1.0.0.8/msi-extension-handler disable"`
 - `sudo bash -c "/var/lib/waagent/Microsoft.ManagedIdentity.ManagedIdentityExtensionForLinux-1.0.0.8/msi-extension-handler enable"`
- De VMAgent in Windows biedt momenteel geen ondersteuning voor gebruiker toegewezen MSI. 
- Een rol toewijzen aan een MSI-bestand voor toegang tot een bron op dit moment vereist geen speciale machtigingen. 
- Als een virtuele machine een MSI toegewezen gebruiker heeft, maar er is geen systeem MSI toegewezen de portal gebruikersinterface weergegeven MSI ingeschakeld. Gebruik een Azure Resource Manager-sjabloon, een Azure CLI of een SDK zodat het systeem MSI toegewezen.
