


## <a name="using-vm-extensions"></a>Met behulp van de VM-extensies
Azure VM-extensies implementeren gedrag of functies waarmee een andere programma's werken op Azure Virtual machines (bijvoorbeeld de **WebDeployForVSDevTest** uitbreiding kunt Visual Studio Web Deploy-oplossingen voor uw Azure VM) of geef de mogelijkheid om te communiceren met de virtuele machine te ondersteunen sommige andere gedrag (bijvoorbeeld, kunt u de toegang van de VM-uitbreidingen van PowerShell, de Azure CLI en REST-clients opnieuw instellen of wijzigen van waarden voor externe toegang in uw Azure VM).

> [!IMPORTANT]
> Zie voor een volledige lijst met extensies die door de functies ze ondersteunen [Azure VM-extensies en functies](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Omdat elke VM-extensie een specifieke functie ondersteunt, precies wat u wel en niet met de extensie is afhankelijk van de extensie. Voordat u uw virtuele machine wijzigt, zorg er daarom voor dat u de documentatie voor de VM-extensie die u wilt gebruiken hebt gelezen. Verwijderen van bepaalde extensies VM wordt niet ondersteund; anderen hebben eigenschappen die kunnen worden ingesteld die het gedrag van de virtuele machine drastisch wijzigen.
> 
> 

De meest algemene taken zijn:

1. Zoeken naar beschikbare uitbreidingen
2. Bijwerken van geladen uitbreidingen
3. Uitbreidingen toevoegen
4. Extensies verwijderen

## <a name="find-available-extensions"></a>Zoeken naar beschikbare uitbreidingen
U kunt-extensie en met behulp van uitgebreide informatie vinden:

* PowerShell
* Azure platformoverschrijdende opdrachtregel-Interface (Azure CLI)
* Service Management REST API

### <a name="azure-powershell"></a>Azure PowerShell
Bepaalde extensies hebben PowerShell-cmdlets die specifiek zijn voor deze, waardoor de configuratie vanuit PowerShell gemakkelijker; maar de volgende cmdlets werkt voor alle VM-extensies.

U kunt de volgende cmdlets gebruiken om informatie over de beschikbare uitbreidingen te verkrijgen:

* Voor exemplaren van de functie web- of werkrollen kunt u de [Get-AzureServiceAvailableExtension](https://msdn.microsoft.com/library/azure/dn722498.aspx) cmdlet.
* Voor exemplaren van virtuele Machines, kunt u de [Get-AzureVMAvailableExtension](https://msdn.microsoft.com/library/azure/dn722480.aspx) cmdlet.
  
   Het volgende codevoorbeeld ziet u hoe u de gegevens voor de **IaaSDiagnostics** uitbreiding met behulp van PowerShell.
  
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

### <a name="azure-command-line-interface-azure-cli"></a>Azure vanaf de opdrachtregel-Interface (Azure CLI)
Bepaalde extensies zijn Azure CLI-opdrachten die specifiek voor deze zijn (de Docker-VM-extensie is een voorbeeld), die de configuratie kan eenvoudiger; maar de volgende opdrachten beschikbaar voor alle VM-extensies.

Kunt u de **azure vm-extensielijst** opdracht voor het verkrijgen van informatie over de beschikbare uitbreidingen en gebruik de **–-json** optie om alle beschikbare informatie over een of meer uitbreidingen weer te geven. Als u de naam van een uitbreiding niet gebruikt, retourneert de opdracht een JSON-beschrijving van alle beschikbare uitbreidingen.

Het volgende codevoorbeeld ziet u hoe u de gegevens voor de **IaaSDiagnostics** uitbreiding met de Azure CLI **azure vm-extensielijst** opdracht en het gebruik de **–-json**  optie voor volledige informatie te retourneren.

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

* Voor exemplaren van de functie web- of werkrollen kunt u de [lijst met beschikbare uitbreidingen](https://msdn.microsoft.com/library/dn169559.aspx) bewerking. Als u de versies van de beschikbare uitbreidingen weergeven, kunt u [lijst extensie versies](https://msdn.microsoft.com/library/dn495437.aspx).
* Voor exemplaren van virtuele Machines, kunt u de [lijst Resource-uitbreidingen](https://msdn.microsoft.com/library/dn495441.aspx) bewerking. Als u de versies van de beschikbare uitbreidingen weergeven, kunt u [lijst Resource-uitbreiding versies](https://msdn.microsoft.com/library/dn495440.aspx).

## <a name="add-update-or-disable-extensions"></a>Toevoegen, bijwerken of extensies uitschakelen
Uitbreidingen kunnen worden toegevoegd wanneer een exemplaar is gemaakt of ze kunnen worden toegevoegd aan een actief exemplaar. Uitbreidingen kunnen worden bijgewerkt, uitgeschakeld of verwijderd. U kunt deze acties uitvoeren met behulp van Azure PowerShell-cmdlets of met behulp van de Service Management REST-API-bewerkingen. Parameters zijn vereist voor het installeren en instellen van bepaalde extensies. Openbare en persoonlijke parameters worden ondersteund voor uitbreidingen.

### <a name="azure-powershell"></a>Azure PowerShell
Met Azure PowerShell-cmdlets, is de eenvoudigste manier toevoegen en bijwerken van de uitbreidingen. Wanneer u de extensie-cmdlets gebruikt, geldt de configuratie van de uitbreiding voor de meeste voor u. Soms moet u wellicht een uitbreiding via programmacode toevoegen. Wanneer u om dit te doen, moet u de configuratie van de extensie opgeven.

U kunt de volgende cmdlets gebruiken om te weten of een uitbreiding voor een configuratie van openbare en persoonlijke parameters vereist:

* Voor exemplaren van de functie web- of werkrollen kunt u de **Get-AzureServiceAvailableExtension** cmdlet.
* Voor exemplaren van virtuele Machines, kunt u de **Get-AzureVMAvailableExtension** cmdlet.

### <a name="service-management-rest-apis"></a>REST API's voor Service Management
Wanneer u een lijst met beschikbare uitbreidingen ophalen met de REST-API's, krijgt u informatie over hoe de extensie is geconfigureerd. De informatie die wordt geretourneerd mogelijk parameterinformatie dat wordt vertegenwoordigd door een openbare schema en een persoonlijke schema weergeven. Openbare parameterwaarden worden in query's over de exemplaren geretourneerd. Persoonlijke parameterwaarden worden niet geretourneerd.

U kunt de volgende REST-API's gebruiken om te weten of een uitbreiding voor een configuratie van openbare en persoonlijke parameters vereist:

* Voor exemplaren van webrollen of werkrollen, de **PublicConfigurationSchema** en **PrivateConfigurationSchema** elementen bevatten de informatie in het antwoord van de [lijst die beschikbaar zijn Extensies](https://msdn.microsoft.com/library/dn169559.aspx) bewerking.
* Voor exemplaren van virtuele Machines, de **PublicConfigurationSchema** en **PrivateConfigurationSchema** elementen bevatten de informatie in het antwoord van de [lijst Resource Extensies](https://msdn.microsoft.com/library/dn495441.aspx) bewerking.

> [!NOTE]
> Extensies kunnen ook gebruiken voor configuraties die zijn gedefinieerd met JSON. Wanneer deze typen uitbreidingen worden gebruikt, alleen de **SampleConfig** element wordt gebruikt.
> 
> 

