---
title: Veelgestelde vragen en bekende problemen met beheerde Service identiteit (MSI) voor Azure Active Directory
description: Bekende problemen met de Service-identiteit beheerd voor Azure Active Directory.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.assetid: 2097381a-a7ec-4e3b-b4ff-5d2fb17403b6
ms.service: active-directory
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 12/12/2017
ms.author: daveba
ms.openlocfilehash: 78148c6538efa06018628297a89681ec6ec3d32d
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="faqs-and-known-issues-with-managed-service-identity-msi-for-azure-active-directory"></a>Veelgestelde vragen en bekende problemen met beheerde Service identiteit (MSI) voor Azure Active Directory

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

## <a name="frequently-asked-questions-faqs"></a>Veelgestelde vragen

### <a name="does-msi-work-with-azure-cloud-services"></a>Werkt MSI met Azure Cloud Services?

Nee, er zijn geen plannen voor de ondersteuning van MSI in Azure Cloud Services.

### <a name="does-msi-work-with-the-active-directory-authentication-library-adal-or-the-microsoft-authentication-library-msal"></a>Werkt MSI met Active Directory Authentication Library (ADAL) of de Microsoft Authentication Library (MSAL)?

Nee, MSI niet nog geïntegreerd met ADAL of MSAL. Zie voor meer informatie over het verkrijgen van een MSI-token met behulp van de REST van de MSI-eindpunt [het gebruik van een Azure VM beheerde Service identiteit (MSI) voor de aanschaf van token](how-to-use-vm-token.md).

### <a name="what-is-the-security-boundary-of-a-managed-service-identity"></a>Wat is de beveiligingsgrens van een beheerde Service-identiteit?

De beveiligingsgrens van de identiteit is de bron waarnaar deze is gekoppeld. De beveiligingsgrens voor een MSI virtuele Machine is bijvoorbeeld de virtuele Machine. Een code die wordt uitgevoerd op deze virtuele machine, kan het MSI-eindpunt bellen en vragen van tokens. Het is de vergelijkbare ervaring met andere bronnen die ondersteuning bieden voor MSI.

### <a name="should-i-use-the-msi-vm-imds-endpoint-or-the-msi-vm-extension-endpoint"></a>Moet ik het eindpunt MSI VM IMDS of het eindpunt MSI VM-extensie gebruiken?

Als u MSI met virtuele machines, raden we met behulp van het eindpunt MSI IMDS. De Azure-Service-exemplaar metagegevens is een REST-eindpunt toegankelijk voor alle IaaS VM's die worden gemaakt via de Azure Resource Manager. Enkele van de voordelen van het gebruik van MSI via IMDS zijn:

1. Alle besturingssystemen van Azure IaaS ondersteund kunt MSI via IMDS gebruiken. 
2. Niet langer nodig voor het installeren van een uitbreiding op de virtuele machine om in te schakelen van MSI. 
3. De certificaten die door MSI zijn niet meer aanwezig zijn in de virtuele machine. 
4. Het eindpunt IMDS is een bekende niet-routeerbare IP-adres alleen beschikbaar vanuit de virtuele machine. 

De MSI-VM-extensie is nog steeds beschikbaar zijn voor vandaag; echter, u verder gaat er standaard voor het gebruik van het eindpunt IMDS. De MSI-VM-extensie wordt binnenkort gestart op een plan afschaffing. 

Zie voor meer informatie over Azure exemplaar Metada Service [IMDS documentatie](https://docs.microsoft.com/azure/virtual-machines/windows/instance-metadata-service)

### <a name="what-are-the-supported-linux-distributions"></a>Wat zijn de ondersteunde Linux-distributies?

Alle Linux-distributies die wordt ondersteund door Azure IaaS kunnen worden gebruikt met MSI via het IMDS-eindpunt. 

Opmerking: De MSI-VM-extensie biedt alleen ondersteuning voor de volgende Linux-distributies:
- Virtuele CoreOS stabiel
- CentOS 7.1
- 7.2 RedHat
- Ubuntu 15.04
- Ubuntu 16.04

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

![Fout bij het MSI automation script exporteren](../media/msi-known-issues/automation-script-export-error.png)

De beheerde Service identiteit van VM-extensie biedt momenteel geen ondersteuning voor de mogelijkheid om het schema aan een resource group-sjabloon. Hierdoor worden de gegenereerde sjabloon geen configuratieparameters in te schakelen beheerde Service-identiteit voor de resource weergegeven. Deze secties kunnen handmatig worden toegevoegd aan de hand van de voorbeelden in [een VM beheerde Service-identiteit configureren met behulp van een sjabloon](qs-configure-template-windows-vm.md).

Zodra de schema-export-functionaliteit beschikbaar voor de MSI-VM-extensie, wordt het weergegeven in [exporteren resourcegroepen met VM-extensies](../../virtual-machines/windows/extensions-export-templates.md#supported-virtual-machine-extensions).

### <a name="configuration-blade-does-not-appear-in-the-azure-portal"></a>Configuratie-blade niet wordt weergegeven in de Azure-portal

Als de blade VM-configuratie op de virtuele machine niet wordt weergegeven, klikt u vervolgens is MSI niet ingeschakeld in de portal in uw regio nog.  Controleer het later opnieuw.  U kunt ook MSI inschakelen voor uw virtuele machine met [PowerShell](qs-configure-powershell-windows-vm.md) of de [Azure CLI](qs-configure-cli-windows-vm.md).

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

## <a name="known-issues-with-user-assigned-msi-preview"></a>Bekende problemen met de gebruiker toegewezen MSI *(Preview)*

- De enige manier om alle gebruiker toegewezen MSI-bestanden te verwijderen is doordat het systeem MSI toegewezen. 
- Het inrichten van de VM-extensie voor een virtuele machine kan mislukken als gevolg van DNS-lookup-fouten. Opnieuw opstarten van de virtuele machine en probeer het opnieuw. 
- Toevoegen van een MSI 'niet-bestaande' zorgt ervoor dat de virtuele machine mislukt. *Opmerking: De oplossing voor toewijzen-identity mislukken als MSI niet bestaat, wordt teruggedraaid-out*
- Zelfstudie voor Azure Storage is alleen beschikbaar in de centrale EUAP ons op dit moment. 
- U maakt een MSI met speciale tekens (dat wil zeggen liggend streepje) in de naam van de toegewezen gebruiker, wordt niet ondersteund.
- Wanneer een tweede gebruiker toe te voegen identiteit toegewezen, de clientID mogelijk niet beschikbaar voor het aanvragen van tokens voor. Als een risicobeperking start opnieuw op de MSI-VM-extensie met de volgende twee bash-opdrachten:
 - `sudo bash -c "/var/lib/waagent/Microsoft.ManagedIdentity.ManagedIdentityExtensionForLinux-1.0.0.8/msi-extension-handler disable"`
 - `sudo bash -c "/var/lib/waagent/Microsoft.ManagedIdentity.ManagedIdentityExtensionForLinux-1.0.0.8/msi-extension-handler enable"`
- De VMAgent in Windows biedt momenteel geen ondersteuning voor gebruiker toegewezen MSI. 
- Als een virtuele machine een MSI toegewezen gebruiker heeft, maar er is geen systeem MSI toegewezen de portal gebruikersinterface weergegeven MSI ingeschakeld. Gebruik een Azure Resource Manager-sjabloon, een Azure CLI of een SDK zodat het systeem MSI toegewezen.
