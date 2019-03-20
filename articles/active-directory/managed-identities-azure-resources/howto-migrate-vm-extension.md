---
title: Stoppen met het gebruik van de beheerde identiteit VM-extensie en start met behulp van de Azure Instance Metadata Service-eindpunt
description: Voor stap door stapsgewijze instructies voor het gebruik van de VM-extensie stoppen en starten met behulp van Azure Instance Metadata Service (IMDS) voor verificatie.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/25/2018
ms.author: priyamo
ms.openlocfilehash: c29eb22f2606fc44cfd7d948f2c363cb51c811dd
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58227615"
---
# <a name="how-to-stop-using-the-virtual-machine-managed-identities-extension-and-start-using-the-azure-instance-metadata-service"></a>Stoppen met het gebruik van de virtuele machine beheerde identiteiten-extensie en start met behulp van Azure Instance Metadata Service

## <a name="virtual-machine-extension-for-managed-identities"></a>Virtuele machine-extensie voor beheerde identiteiten

De extensie van de virtuele machine voor beheerde identiteit wordt gebruikt om aan te vragen van tokens voor een beheerde identiteit binnen de virtuele machine. De werkstroom bestaat uit de volgende stappen uit:

1. Eerst de werkbelasting binnen de resource het lokale eindpunt roept `http://localhost/oauth2/token` om aan te vragen van een toegangstoken.
2. De virtuele machine-extensie gebruikt vervolgens de referenties voor de beheerde identiteit, om aan te vragen van een toegangstoken van Azure AD... 
3. Het toegangstoken wordt geretourneerd naar de aanroeper en kan worden gebruikt om de services die ondersteuning bieden voor Azure AD-verificatie, zoals Azure Key Vault of Azure Storage te verifiëren.

Door verschillende beperkingen die worden beschreven in de volgende sectie, is de beheerde identiteit VM-extensie afgeschaft en vervangen door met behulp van het gelijkwaardige eindpunt in de Azure Instance Metadata Service (IMDS)

### <a name="provision-the-extension"></a>De extensie inrichten 

Wanneer u een virtuele machine of virtuele-machineschaalset om een beheerde identiteit configureert, kunt u optioneel wilt, kunt u besluiten (optioneel) voor het inrichten van de beheerde identiteiten voor het gebruik van Azure-resources VM extension de `-Type` parameter op de [ Set-AzVMExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmextension) cmdlet. U kunt een doorgeven `ManagedIdentityExtensionForWindows` of `ManagedIdentityExtensionForLinux`, afhankelijk van het type van de virtuele machine en de naam met behulp van de `-Name` parameter. De `-Settings` parameter geeft u de poort die door het eindpunt van de OAuth-token gebruikt voor het ophalen van tokens:

```powershell
   $settings = @{ "port" = 50342 }
   Set-AzVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
```

U kunt ook de sjabloon van Azure Resource Manager-implementatie gebruiken voor het inrichten van de VM-extensie door toe te voegen van de volgende JSON naar de `resources` sectie aan de sjabloon (Gebruik `ManagedIdentityExtensionForLinux` voor de naam en type-elementen voor de Linux-versie).

    ```json
    {
        "type": "Microsoft.Compute/virtualMachines/extensions",
        "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForWindows')]",
        "apiVersion": "2018-06-01",
        "location": "[resourceGroup().location]",
        "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
        ],
        "properties": {
            "publisher": "Microsoft.ManagedIdentity",
            "type": "ManagedIdentityExtensionForWindows",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "port": 50342
            }
        }
    }
    ```
    
    
Als u met virtual machine scale sets werkt, kunt u de beheerde identiteiten ook inrichten voor Azure-resources virtuele-met behulp van extensie machineschaalset de [toevoegen AzVmssExtension](/powershell/module/az.compute/add-azvmssextension) cmdlet. U kunt een doorgeven `ManagedIdentityExtensionForWindows` of `ManagedIdentityExtensionForLinux`, afhankelijk van het type virtuele-machineschaalset instellen en de naam met behulp van de `-Name` parameter. De `-Settings` parameter geeft u de poort die door het eindpunt van de OAuth-token gebruikt voor het ophalen van tokens:

   ```powershell
   $setting = @{ "port" = 50342 }
   $vmss = Get-AzVmss
   Add-AzVmssExtension -VirtualMachineScaleSet $vmss -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Setting $settings 
   ```
Voor het inrichten van de virtuele-machineschaalset extensie met de sjabloon van Azure Resource Manager-implementatie instellen toevoegen de volgende JSON naar de `extensionpProfile` sectie aan de sjabloon (Gebruik `ManagedIdentityExtensionForLinux` voor de naam en type-elementen voor de Linux-versie).

    ```json
    "extensionProfile": {
        "extensions": [
            {
                "name": "ManagedIdentityWindowsExtension",
                "properties": {
                    "publisher": "Microsoft.ManagedIdentity",
                    "type": "ManagedIdentityExtensionForWindows",
                    "typeHandlerVersion": "1.0",
                    "autoUpgradeMinorVersion": true,
                    "settings": {
                        "port": 50342
                    },
                    "protectedSettings": {}
                }
            }
    ```

Inrichting van de extensie van de virtuele machine kan mislukken vanwege DNS-lookup-fouten. Als dit het geval is, start de virtuele machine en probeer het opnieuw. 

### <a name="remove-the-extension"></a>Verwijder de extensie 
U kunt de extensie verwijderen `-n ManagedIdentityExtensionForWindows` of `-n ManagedIdentityExtensionForLinux` overschakelen (afhankelijk van het type virtuele machine) met [az vm extension delete](https://docs.microsoft.com/cli/azure/vm/), of [az vmss extension delete](https://docs.microsoft.com/cli/azure/vmss) voor virtuele-machineschaalsets Hiermee stelt u met behulp van Azure CLI, of `Remove-AzVMExtension` voor Powershell:

```azurecli-interactive
az vm identity --resource-group myResourceGroup --vm-name myVm -n ManagedIdentityExtensionForWindows
```

```azurecli-interactive
az vmss extension delete -n ManagedIdentityExtensionForWindows -g myResourceGroup -vmss-name myVMSS
```

```powershell
Remove-AzVMExtension -ResourceGroupName myResourceGroup -Name "ManagedIdentityExtensionForWindows" -VMName myVM
```

### <a name="acquire-a-token-using-the-virtual-machine-extension"></a>Een token met behulp van de extensie van de virtuele machine ophalen

Hier volgt een voorbeeld van een aanvraag met behulp van de beheerde identiteiten voor Azure-resources VM-extensie-eindpunt:

```
GET http://localhost:50342/oauth2/token?resource=https%3A%2F%2Fmanagement.azure.com%2F HTTP/1.1
Metadata: true
```

| Element | Description |
| ------- | ----------- |
| `GET` | De HTTP-term, die aangeeft dat u wilt ophalen van gegevens uit het eindpunt. In dit geval een OAuth-toegangstoken. | 
| `http://localhost:50342/oauth2/token` | De beheerde identiteiten voor het eindpunt van de Azure-resources, waarbij 50342 is de standaardpoort en kan worden geconfigureerd. |
| `resource` | Een queryreeks-parameter, die wijzen op de URI van de App-ID van de doelresource. Het verschijnt ook in de `aud` claim (doelgroep) van de uitgegeven tokens. In dit voorbeeld vraagt een token voor toegang tot Azure Resource Manager heeft een URI van de App-ID van https://management.azure.com/. |
| `Metadata` | Een HTTP-aanvraag veld header wordt vereist door beheerde identiteiten voor Azure-resources als een bescherming tegen aanvallen van Server Side aanvraag kunnen worden vervalst (SSRF). Deze waarde moet worden ingesteld op "true", volledig in kleine letters.|
| `object_id` | (Optioneel) Een queryreeks-parameter, die de object_id van de beheerde identiteit die u wilt dat het token voor aangeeft. Vereist, als uw virtuele machine meerdere beheerde identiteiten gebruiker toegewezen heeft.|
| `client_id` | (Optioneel) Een queryreeks-parameter, die de client_id van de beheerde identiteit die u wilt dat het token voor aangeeft. Vereist, als uw virtuele machine meerdere beheerde identiteiten gebruiker toegewezen heeft.|


Het voorbeeldantwoord:

```
HTTP/1.1 200 OK
Content-Type: application/json
{
  "access_token": "eyJ0eXAi...",
  "refresh_token": "",
  "expires_in": "3599",
  "expires_on": "1506484173",
  "not_before": "1506480273",
  "resource": "https://management.azure.com/",
  "token_type": "Bearer"
}
```

| Element | Description |
| ------- | ----------- |
| `access_token` | Het aangevraagde toegangstoken. Bij het aanroepen van een beveiligde REST-API, het token is ingesloten in de `Authorization` headerveld aanvraag als een token 'bearer', zodat de API voor verificatie van de oproepende functie. | 
| `refresh_token` | Niet gebruikt door beheerde identiteiten voor Azure-resources. |
| `expires_in` | Het aantal seconden dat het toegangstoken nog steeds geldig zijn voordat het verloopt na uitgifte. Tijd van uitgifte kunt u vinden in van het token `iat` claim. |
| `expires_on` | De timespan wanneer het toegangstoken is verlopen. De datum wordt weergegeven als het aantal seconden van ' 1970-01-01T0:0:0Z UTC ' (komt overeen met van het token `exp` claim). |
| `not_before` | De timespan wanneer het toegangstoken wordt van kracht en kan worden geaccepteerd. De datum wordt weergegeven als het aantal seconden van ' 1970-01-01T0:0:0Z UTC ' (komt overeen met van het token `nbf` claim). |
| `resource` | De resource het toegangstoken is aangevraagd voor, die overeenkomt met de `resource` query-tekenreeksparameter van de aanvraag. |
| `token_type` | Het type token, dat een toegangstoken 'Bearer', wat betekent dat de resource kan toegang geven tot de houder van dit token is. |


### <a name="troubleshoot-the-virtual-machine-extension"></a>Problemen met de virtuele machine-extensie oplossen 

#### <a name="restart-the-virtual-machine-extension-after-a-failure"></a>Opnieuw opstarten van de extensie van de virtuele machine na een storing

Op Windows en bepaalde versies van Linux, als de extensie wordt gestopt, kan de volgende cmdlet worden gebruikt om handmatig opnieuw starten:

```powershell
Set-AzVMExtension -Name <extension name>  -Type <extension Type>  -Location <location> -Publisher Microsoft.ManagedIdentity -VMName <vm name> -ResourceGroupName <resource group name> -ForceRerun <Any string different from any last value used>
```

Waar: 
- Naam van de uitbreiding en het type voor Windows is: `ManagedIdentityExtensionForWindows`
- Naam van de uitbreiding en het type voor Linux is: `ManagedIdentityExtensionForLinux`

#### <a name="automation-script-fails-when-attempting-schema-export-for-managed-identities-for-azure-resources-extension"></a>"Automatiseringsscript" is mislukt bij het schema exporteren voor beheerde identiteiten voor uitbreiding van de Azure-resources

Als beheerde identiteiten voor Azure-resources op een virtuele machine is ingeschakeld, wordt de volgende fout weergegeven bij het gebruik van de functie 'Automatiseringsscript' voor de virtuele machine of de resourcegroep:

![Beheerde identiteiten voor een Azure-resources automatiseringsscript exportfout](./media/howto-migrate-vm-extension/automation-script-export-error.png)

De beheerde identiteiten voor virtuele machine-extensie voor Azure-resources biedt momenteel geen ondersteuning voor de mogelijkheid waarvan het schema exporteren naar een resourcegroepsjabloon. Als gevolg hiervan weergegeven de gegenereerde sjabloon niet. configuratieparameters voor het inschakelen van beheerde identiteiten voor Azure-resources op de resource. Deze secties kunnen handmatig worden toegevoegd aan de hand van de voorbeelden in [configureren beheerde identiteiten voor een Azure-resources op een Azure-machine met behulp van een sjablonen](qs-configure-template-windows-vm.md).

Wanneer de schema-export-functionaliteit beschikbaar is voor de beheerde identiteiten voor virtuele machine-extensie voor Azure-resources (gepland voor de afschaffing in januari 2019), wordt deze weergegeven in [exporteren van resourcegroepen met VM-extensies ](../../virtual-machines/extensions/export-templates.md#supported-virtual-machine-extensions).

## <a name="limitations-of-the-virtual-machine-extension"></a>Beperkingen van de extensie van de virtuele machine 

Er zijn enkele belangrijke beperkingen voor het gebruik van de extensie van de virtuele machine. 

 * De meest ernstige beperking is het feit dat de referenties gebruikt voor het aanvragen van tokens worden opgeslagen op de virtuele machine. Een aanvaller is kiezen oplossingen de virtuele machine kunt gegevens te exfiltreren de referenties. 
 * Bovendien de extensie van de virtuele machine wordt nog steeds niet ondersteund door verschillende Linux-distributies, met de ontwikkeling van een enorme kosten als u wilt wijzigen, ontwikkelen en testen van de extensie op elk van deze distributies. Op dit moment worden alleen de volgende Linux-distributies ondersteund: 
    * Stable van CoreOS
    * CentOS 7.1 
    * Red Hat 7.2 
    * Ubuntu 15.04 
    * Ubuntu 16.04
 * Er is een prestatie-impact op de implementatie van virtuele machines met beheerde identiteiten, omdat de extensie van de virtuele machine ook moeten worden ingericht. 
 * Ten slotte kan de extensie van de virtuele machine alleen ondersteuning voor 32 gebruiker toegewezen beheerde identiteiten per virtuele machine. 

## <a name="azure-instance-metadata-service"></a>Azure Instance Metadata Service

De [Azure Instance Metadata Service (IMDS)](https://docs.microsoft.com/azure/virtual-machines/instance-metadata-service) is een REST-eindpunt waarmee u informatie over het uitvoeren van de virtuele machine-instanties die kunnen worden gebruikt om te beheren en configureren uw virtuele machines. Het eindpunt is beschikbaar op een bekende niet-routeerbare IP-adres (`169.254.169.254`) die kunnen worden gebruikt alleen de virtuele machine.

Er zijn diverse voordelen voor het gebruik van Azure IMDS naar aanvragen van tokens. 

1. De service bevindt zich buiten de virtuele machine, de referenties die worden gebruikt door beheerde identiteiten zijn daarom niet meer aanwezig zijn op de virtuele machine. In plaats daarvan worden ze gehost en beveiligd op de hostcomputer van de virtuele machine van Azure.   
2. Alle Windows en Linux-besturingssystemen ondersteund op Azure IaaS kunt beheerde identiteiten gebruiken.
3. Implementatie is sneller en eenvoudiger, omdat de VM-extensie niet meer moet worden ingericht.
4. Met de IMDS kan eindpunt maximaal 1000 toegewezen gebruiker beheerde identiteiten worden toegewezen aan één virtuele machine.
5. Er is geen belangrijke wijziging aan de aanvragen met behulp van IMDS in plaats van die met behulp van de extensie van de virtuele machine, dus het is redelijk eenvoudig naar poort via bestaande implementaties die momenteel gebruikmaken van de extensie van de virtuele machine.

Daarom is de service Azure IMDS de facto manier om aanvragen van tokens, nadat de extensie van de virtuele machine is afgeschaft. 


## <a name="next-steps"></a>Volgende stappen

* [Over het gebruik van beheerde identiteiten voor Azure-resources op een Azure-machine naar een toegangstoken verkrijgen](how-to-use-vm-token.md)
* [Azure Instance Metadata Service](https://docs.microsoft.com/azure/virtual-machines/windows/instance-metadata-service)
