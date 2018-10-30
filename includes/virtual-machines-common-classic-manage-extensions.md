---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: d14cfb82ae74f85425dbd3e8a365e8b99969641d
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50227195"
---
## <a name="using-vm-extensions"></a>Met behulp van VM-extensies
Azure VM-extensies gedrag of functies die een andere programma's werken op Azure Virtual machines te implementeren (bijvoorbeeld de **WebDeployForVSDevTest** extensie kunt Visual Studio Web Deploy-oplossingen op uw virtuele Azure-machine) of geef de mogelijkheid om te communiceren met de virtuele machine voor de ondersteuning van sommige andere gedrag (bijvoorbeeld, kunt u de extensies van de VM-toegang vanuit PowerShell, de Azure CLI en REST-clients opnieuw instellen of wijzigen van externe toegang-waarden op uw Azure-VM).

> [!IMPORTANT]
> Zie voor een volledige lijst van extensies van de functies die erdoor worden ondersteund, [Azure VM-extensies en functies](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Omdat elke VM-extensie ondersteuning biedt voor een specifieke functie, precies wat u wel en niet met de extensie is afhankelijk van de extensie. Voordat u uw virtuele machine wijzigt, zorg ervoor dat u de documentatie voor de VM-extensie die u wilt gebruiken hebt gelezen. Aantal VM-extensies verwijderen wordt niet ondersteund; anderen hebben eigenschappen die kunnen worden ingesteld die aanzienlijk VM-gedrag wijzigen.
> 
> 

De meest voorkomende taken zijn:

1. Beschikbare extensies zoeken
2. Geladen extensies bijwerken
3. Uitbreidingen toevoegen
4. Extensies verwijderen

## <a name="find-available-extensions"></a>Beschikbare extensies zoeken
U vindt de extensie en uitgebreide informatie over het gebruik:

* PowerShell
* Azure platformoverschrijdende Command Line Interface (Azure CLI)
* Service Management REST API

### <a name="azure-powershell"></a>Azure PowerShell
Aantal extensies zijn PowerShell-cmdlets die specifiek zijn voor, die mogelijk eenvoudiger hun configuratie vanuit PowerShell; maar de volgende cmdlets werken voor alle VM-extensies.

U kunt de volgende cmdlets gebruiken om informatie over de beschikbare uitbreidingen te verkrijgen:

* Voor exemplaren van de functies van de web- of werkrollen, kunt u de [Get-AzureServiceAvailableExtension](https://msdn.microsoft.com/library/azure/dn722498.aspx) cmdlet.
* Voor exemplaren van virtuele Machines, kunt u de [Get-AzureVMAvailableExtension](https://msdn.microsoft.com/library/azure/dn722480.aspx) cmdlet.
  
   Bijvoorbeeld, het volgende codevoorbeeld laat zien hoe u de gegevens voor de **IaaSDiagnostics** extensie met behulp van PowerShell.
  
      PS C:\> Get-AzureVMAvailableExtension -ExtensionName IaaSDiagnostics
  
      Publisher                   : Microsoft.Azure.Diagnostics
      ExtensionName               : IaaSDiagnostics
      Version                     : 1.2
      Label                       : Microsoft Monitoring Agent Diagnostics
      Description                 : Microsoft Monitoring Agent Extension
      PublicConfigurationSchema   :
      PrivateConfigurationSchema  :
      IsInternalExtension         : False
      SampleConfig                :
      ReplicationCompleted        : True
      Eula                        :
      PrivacyUri                  :
      HomepageUri                 :
      IsJsonExtension             : True
      DisallowMajorVersionUpgrade : False
      SupportedOS                 :
      PublishedDate               :
      CompanyName                 :

### <a name="azure-command-line-interface-azure-cli"></a>Azure Command Line Interface (Azure CLI)
Aantal extensies hebben Azure CLI-opdrachten die specifiek voor deze zijn (de Docker VM-extensie is een voorbeeld), die hun configuratie mogelijk eenvoudiger; maar de volgende opdrachten werkt voor alle VM-extensies.

Kunt u de **lijst met azure-vm-extensies** opdracht voor het verkrijgen van informatie over de beschikbare uitbreidingen en gebruik de **–-json** optie om alle beschikbare informatie over een of meer uitbreidingen weer te geven. Als u een extensie niet gebruikt, retourneert de opdracht een JSON-beschrijving van alle beschikbare uitbreidingen.

Bijvoorbeeld het volgende codevoorbeeld laat zien hoe u de gegevens voor de **IaaSDiagnostics** extensie met de Azure CLI **lijst met azure-vm-extensies** opdracht en maakt gebruik van de **–-json**  optie voor volledige informatie retourneren.

    $ azure vm extension list -n IaaSDiagnostics --json
    [
      {
        "publisher": "Microsoft.Azure.Diagnostics",
        "name": "IaaSDiagnostics",
        "version": "1.2",
        "label": "Microsoft Monitoring Agent Diagnostics",
        "description": "Microsoft Monitoring Agent Extension",
        "replicationCompleted": true,
        "isJsonExtension": true
      }
    ]



### <a name="service-management-rest-apis"></a>REST API's voor Service Management
U kunt de volgende REST-API's gebruiken om informatie over de beschikbare uitbreidingen te verkrijgen:

* Voor exemplaren van de functies van de web- of werkrollen, kunt u de [lijst met beschikbare uitbreidingen](https://msdn.microsoft.com/library/dn169559.aspx) bewerking. Als u wilt de versies van de beschikbare uitbreidingen weergeven, kunt u [lijst met extensies met versie](https://msdn.microsoft.com/library/dn495437.aspx).
* Voor exemplaren van virtuele Machines, kunt u de [lijst Resource-uitbreidingen](https://msdn.microsoft.com/library/dn495441.aspx) bewerking. Als u wilt de versies van de beschikbare uitbreidingen weergeven, kunt u [lijst Resource-uitbreiding versies](https://msdn.microsoft.com/library/dn495440.aspx).

## <a name="add-update-or-disable-extensions"></a>Toevoegen, bijwerken of uitbreiding hebt uitgeschakeld
Extensies kunnen worden toegevoegd wanneer een exemplaar wordt gemaakt of ze kunnen worden toegevoegd aan een actief exemplaar. Extensies kunnen worden bijgewerkt, uitgeschakeld of verwijderd. U kunt deze acties uitvoeren met behulp van Azure PowerShell-cmdlets of met behulp van de Service Management REST API-bewerkingen. Parameters zijn vereist om te installeren en instellen van bepaalde extensies. Openbare en persoonlijke parameters worden ondersteund voor extensies.

### <a name="azure-powershell"></a>Azure PowerShell
Met behulp van Azure PowerShell-cmdlets is de eenvoudigste manier om toevoegen en bijwerken van extensies. Wanneer u de extensie-cmdlets gebruikt, geldt de meeste van de configuratie van de extensie voor u. Soms moet u mogelijk een extensie via een programma toevoegen. Wanneer u nodig hebt om dit te doen, moet u de configuratie van de extensie opgeven.

U kunt de volgende cmdlets gebruiken om te weten of een uitbreiding van een configuratie van openbare en persoonlijke parameters vereist:

* Voor exemplaren van de functies van de web- of werkrollen, kunt u de **Get-AzureServiceAvailableExtension** cmdlet.
* Voor exemplaren van virtuele Machines, kunt u de **Get-AzureVMAvailableExtension** cmdlet.

### <a name="service-management-rest-apis"></a>REST API's voor Service Management
Wanneer u een lijst met beschikbare uitbreidingen ophalen met behulp van de REST API's, ontvangt u informatie over hoe de extensie is om te worden geconfigureerd. De informatie die wordt geretourneerd, mogelijk parameterinformatie vertegenwoordigd door een openbare schema en een persoonlijke schema weer. Openbare parameterwaarden worden in query's over de exemplaren geretourneerd. Persoonlijke parameterwaarden worden niet geretourneerd.

U kunt de volgende REST-API's gebruiken om te weten of een uitbreiding van een configuratie van openbare en persoonlijke parameters vereist:

* Voor exemplaren van de functies van web- of werkrollen, de **PublicConfigurationSchema** en **PrivateConfigurationSchema** elementen bevatten de informatie in het antwoord van de [lijst die beschikbaar zijn Extensies](https://msdn.microsoft.com/library/dn169559.aspx) bewerking.
* Voor exemplaren van virtuele Machines, de **PublicConfigurationSchema** en **PrivateConfigurationSchema** elementen bevatten de informatie in het antwoord van de [lijst Resource Extensies](https://msdn.microsoft.com/library/dn495441.aspx) bewerking.

> [!NOTE]
> Extensies kunnen ook gebruiken voor configuraties die zijn gedefinieerd met JSON. Wanneer deze typen uitbreidingen worden gebruikt, alleen de **SampleConfig** element wordt gebruikt.
> 
> 

