
Oplossen van problemen met een cloudservice van Microsoft Azure, moet de logboekbestanden van de service op virtuele machines verzamelen als de problemen optreden. U kunt de AzureLogCollector-extensie op aanvraag voor het uitvoeren van eenmalige verzamelen van Logboeken van een of meer Cloud-Service-VM's (van webrollen en werkrollen) en de verzamelde bestanden overbrengen naar Azure storage-account – zonder extern aanmelden bij een van de virtuele machines.

> [!NOTE]
> Beschrijvingen voor de meeste van de geregistreerde gegevens kunnen worden gevonden op http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.asp.
> 
> 

Er zijn twee modi van verzameling afhankelijk van de typen bestanden die worden verzameld.

* **Azure Guest Agent-logboeken alleen (GA)**. In deze modus verzameling bevat alle logboeken met betrekking tot Azure-gastagents en andere Azure-onderdelen.
* **Alle logboeken (volledig)**. In deze modus verzameling verzamelt alle bestanden in de modus voor algemene beschikbaarheid plus:
  
  * systeem- en gebeurtenislogboeken
  * HTTP-fout-Logboeken
  * IIS-logboeken
  * Setup-logboeken
  * andere systeemlogboeken

In beide modi verzameling kunnen aanvullende gegevens verzamelingsmappen worden opgegeven met behulp van een verzameling van de volgende structuur:

* **Naam**: de naam van de verzameling, die wordt gebruikt als de naam van de submap in het zip-bestand met de verzamelde bestanden.
* **Locatie**: het pad naar de map op de virtuele machine waar de bestanden moeten worden verzameld zich bevinden.
* **SearchPattern**: het patroon van de namen van bestanden moeten worden verzameld. De standaardwaarde is "\*"
* **Recursieve**: als de bestanden moeten worden verzameld bevindt recursief onder de opgegeven locatie.

## <a name="prerequisites"></a>Vereisten
* Een opslagaccount voor de uitbreiding voor het opslaan van de gegenereerde zip-bestanden hebben.
* Gebruik Azure PowerShell-Cmdlets v0.8.0 of hoger. Zie voor meer informatie, [Azure Downloads](https://azure.microsoft.com/downloads/).

## <a name="add-the-extension"></a>De extensie toevoegen
U kunt [Microsoft Azure PowerShell](https://msdn.microsoft.com/library/dn495240.aspx) cmdlets of [Service Management REST API's](https://msdn.microsoft.com/library/ee460799.aspx) om toe te voegen van de extensie AzureLogCollector.

Voor Cloud Services, de bestaande Azure Powershell-cmdlet **Set AzureServiceExtension**, kan worden gebruikt voor het inschakelen van de extensie op instanties van de Service in de Cloud. Telkens wanneer deze uitbreiding tot en met deze cmdlet is ingeschakeld, wordt op de geselecteerde rol-instanties van de geselecteerde rollen logboekverzameling geactiveerd.

Voor virtuele Machines, de bestaande Azure Powershell-cmdlet **Set AzureVMExtension**, kan worden gebruikt voor het inschakelen van de uitbreiding van virtuele Machines. Telkens wanneer deze uitbreiding is ingeschakeld via de cmdlets, wordt op elk exemplaar logboekverzameling geactiveerd.

Intern, deze extensie gebruikmaakt van JSON-indeling PublicConfiguration en PrivateConfiguration. Hier volgt de indeling van een voorbeeld-JSON voor openbare en persoonlijke configuratie.

### <a name="publicconfiguration"></a>PublicConfiguration

```json
{
    "Instances":  "*",
    "Mode":  "Full",
    "SasUri":  "SasUri to your storage account with sp=wl",
    "AdditionalData":
    [
      {
              "Name":  "StorageData",
              "Location":  "%roleroot%storage",
              "SearchPattern":  "*.*",
              "Recursive":  "true"
      },
      {
            "Name":  "CustomDataFolder2",
            "Location":  "c:\customFolder",
            "SearchPattern":  "*.log",
            "Recursive":  "false"
      },
    ]
}
```

### <a name="privateconfiguration"></a>PrivateConfiguration

```json
{

}
```

> [!NOTE]
> Deze extensie hoeven niet **privateConfiguration**. U kunt alleen een lege structuur voor opgeven de **– PrivateConfiguration** argument.
> 
> 

U kunt een van de twee volgende stappen uit de AzureLogCollector toevoegen aan een of meer exemplaren van een Cloudservice of virtuele Machine van de geselecteerde rollen die de verzamelingen op elke virtuele machine worden uitgevoerd en de verzamelde bestanden verzenden naar Azure-account opgegeven activeert volgen.

## <a name="adding-as-a-service-extension"></a>Als een Service-extensie toe te voegen
1. Volg de instructies voor het verbinden van Azure PowerShell aan uw abonnement.
2. Geef de naam, sleuf, rollen en functie van exemplaren van service die u wilt toevoegen en de extensie AzureLogCollector inschakelen.

  ```powershell
  #Specify your cloud service name
  $ServiceName = 'extensiontest2'

  #Specify the slot. 'Production' or 'Staging'
  $slot = 'Production'

  #Specified the roles on which the extension will be installed and enabled
  $roles = @("WorkerRole1","WebRole1")

  #Specify the instances on which extension will be installed and enabled.  Use wildcard * for all instances
  $instances = @("*")

  #Specify the collection mode, "Full" or "GA"
  $mode = "GA"
  ```

3. Geef de map op aanvullende gegevens waarvoor bestanden worden verzameld (deze stap is optioneel).

  ```powershell
  #add one location
  $a1 = New-Object PSObject

  $a1 | Add-Member -MemberType NoteProperty -Name "Name" -Value "StorageData"
  $a1 | Add-Member -MemberType NoteProperty -Name "SearchPattern" -Value "*"
  $a1 | Add-Member -MemberType NoteProperty -Name "Location" -Value "%roleroot%storage"  #%roleroot% is normally E: or F: drive
  $a1 | Add-Member -MemberType NoteProperty -Name "Recursive" -Value "true"

  $AdditionalDataList+= $a1
  #more locations can be added....
  ```

   > [!NOTE]
   > U kunt token `%roleroot%` om op te geven van het basisstation van de rol, omdat deze geen gebruik maakt van een vaste schijf.
   > 
   > 
4. Geef de Azure storage-accountnaam en de sleutel waarnaar de verzamelde bestanden worden geüpload.

  ```powershell
  $StorageAccountName = 'YourStorageAccountName'
  $StorageAccountKey  = 'YourStorageAccountKey'
  ```

5. Roep de SetAzureServiceLogCollector.ps1 (opgenomen aan het einde van het artikel) als volgt om in te schakelen van de extensie AzureLogCollector voor een Cloudservice. Als de uitvoering is voltooid, kunt u het geüploade bestand onder vinden `https://YourStorageAccountName.blob.core.windows.net/vmlogs`

  ```powershell
  .\SetAzureServiceLogCollector.ps1 -ServiceName YourCloudServiceName  -Roles $roles  -Instances $instances –Mode $mode -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey -AdditionDataLocationList $AdditionalDataList
  ```

Hier volgt de definitie van de parameters doorgegeven aan het script. (Dit wordt hieronder ook gekopieerd.)

```powershell
[CmdletBinding(SupportsShouldProcess = $true)]

param (
  [Parameter(Mandatory=$true)]
  [string]   $ServiceName,

  [Parameter(Mandatory=$false)]
  [string[]] $Roles ,

  [Parameter(Mandatory=$false)]
  [string[]] $Instances,

  [Parameter(Mandatory=$false)]
  [string]   $Slot = 'Production',

  [Parameter(Mandatory=$false)]
  [string]   $Mode = 'Full',

  [Parameter(Mandatory=$false)]
  [string]   $StorageAccountName,

  [Parameter(Mandatory=$false)]
  [string]   $StorageAccountKey,

  [Parameter(Mandatory=$false)]
  [PSObject[]] $AdditionDataLocationList = $null
)
```

* **ServiceName**: de naam van uw cloud-service.
* **Rollen**: een lijst met functies, zoals 'WebRole1' of 'WorkerRole1'.
* **Exemplaren**: een lijst met de namen van de rolinstanties gescheiden door komma's, gebruikt u de jokerteken-tekenreeks ("*") voor alle rolinstanties.
* **Sleuf**: de naam van de site. 'Productie' of "" Staging".
* **Modus**: Verzamelmodus. 'Volledige' of 'GA'.
* **StorageAccountName**: de naam van de Azure storage-account voor het opslaan van verzamelde gegevens.
* **StorageAccountKey**: de naam van de Azure storage-accountsleutel.
* **AdditionalDataLocationList**: een lijst van de volgende structuur:

  ```powershell
  {
    String Name,
    String Location,
    String SearchPattern,
    Bool   Recursive
  }
  ```

## <a name="adding-as-a-vm-extension"></a>Als een VM-extensie toe te voegen
Volg de instructies voor het verbinden van Azure PowerShell aan uw abonnement.

1. Geef de naam van de virtuele machine en de modus voor het verzamelen.

  ```powershell
  #Specify your cloud service name
  $ServiceName = 'YourCloudServiceName'

  #Specify the VM name
  $VMName = "'YourVMName'"

  #Specify the collection mode, "Full" or "GA"
  $mode = "GA"

  Specify the additional data folder for which files will be collected (this step is optional).

  #add one location
  $a1 = New-Object PSObject

  $a1 | Add-Member -MemberType NoteProperty -Name "Name" -Value "StorageData"
  $a1 | Add-Member -MemberType NoteProperty -Name "SearchPattern" -Value "*"
  $a1 | Add-Member -MemberType NoteProperty -Name "Location" -Value "%roleroot%storage"  #%roleroot% is normally E: or F: drive
  $a1 | Add-Member -MemberType NoteProperty -Name "Recursive" -Value "true"

  $AdditionalDataList+= $a1
        #more locations can be added....
  ```
  
2. Geef de Azure storage-accountnaam en de sleutel waarnaar de verzamelde bestanden worden geüpload.

  ```powershell
  $StorageAccountName = 'YourStorageAccountName'
  $StorageAccountKey  = 'YourStorageAccountKey'
  ```

3. Roep de SetAzureVMLogCollector.ps1 (opgenomen aan het einde van het artikel) als volgt om in te schakelen van de extensie AzureLogCollector voor een Cloudservice. Als de uitvoering is voltooid, kunt u het geüploade bestand onder vinden `https://YourStorageAccountName.blob.core.windows.net/vmlogs`

Hier volgt de definitie van de parameters doorgegeven aan het script. (Dit wordt hieronder ook gekopieerd.)

```powershell
[CmdletBinding(SupportsShouldProcess = $true)]

param (
  [Parameter(Mandatory=$true)]
  [string]   $ServiceName,

  [Parameter(Mandatory=$false)]
  [string] $VMName ,

  [Parameter(Mandatory=$false)]
  [string]   $Mode = 'Full',

  [Parameter(Mandatory=$false)]
  [string]   $StorageAccountName,

  [Parameter(Mandatory=$false)]
  [string]   $StorageAccountKey,

  [Parameter(Mandatory=$false)]
  [PSObject[]] $AdditionDataLocationList = $null
)
```

* **ServiceName**: de naam van uw cloud-service.
* **VMName**: de naam van de virtuele machine.
* **Modus**: Verzamelmodus. 'Volledige' of 'GA'.
* **StorageAccountName**: de naam van de Azure storage-account voor het opslaan van verzamelde gegevens.
* **StorageAccountKey**: de naam van de Azure storage-accountsleutel.
* **AdditionalDataLocationList**: een lijst van de volgende structuur:

  ```
  {
    String Name,
    String Location,
    String SearchPattern,
    Bool   Recursive
  }
  ```

## <a name="extention-powershell-script-files"></a>Extensie PowerShell-Script-bestanden
### <a name="setazureservicelogcollectorps1"></a>SetAzureServiceLogCollector.ps1

```powershell
[CmdletBinding(SupportsShouldProcess = $true)]

param (
  [Parameter(Mandatory=$true)]
  [string]   $ServiceName,

  [Parameter(Mandatory=$false)]
  [string[]] $Roles ,

  [Parameter(Mandatory=$false)]
  [string[]] $Instances = '*',

  [Parameter(Mandatory=$false)]
  [string]   $Slot = 'Production',

  [Parameter(Mandatory=$false)]
  [string]   $Mode = 'Full',

  [Parameter(Mandatory=$false)]
  [string]   $StorageAccountName,

  [Parameter(Mandatory=$false)]
  [string]   $StorageAccountKey,

  [Parameter(Mandatory=$false)]
  [PSObject[]] $AdditionDataLocationList = $null
)

$publicConfig = New-Object PSObject

if ($Instances -ne $null -and $Instances.Count -gt 0)  #Instances should be seperated by ,
{
  $instanceText = $Instances[0]
  for ($i = 1;$i -lt $Instances.Count;$i++)
  {
    $instanceText = $instanceText+ "," + $Instances[$i]
  }
  $publicConfig | Add-Member -MemberType NoteProperty -Name "Instances" -Value $instanceText
}
else  #For all instances if not specified.  The value should be a space or *
{
  $publicConfig | Add-Member -MemberType NoteProperty -Name "Instances" -Value " "
}

if ($Mode -ne $null )
{
  $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value $Mode
}
else
{
  $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value "Full"
}

#
#we need to get the Sasuri from StorageAccount and containers
#
$context = New-AzureStorageContext -Protocol https -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

$ContainerName = "azurelogcollectordata"
$existingContainer = Get-AzureStorageContainer -Context $context |  Where-Object { $_.Name -like $ContainerName}
if ($existingContainer -eq $null)
{
  "Container ($ContainerName) doesn't exist. Creating it now.."
  New-AzureStorageContainer -Context $context -Name $ContainerName -Permission off
}

$ExpiryTime =  [DateTime]::Now.AddMinutes(120).ToString("o")
$SasUri = New-AzureStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rwl -Context $context
$publicConfig | Add-Member -MemberType NoteProperty -Name "SasUri" -Value $SasUri

#
#Add AdditionalData to collect data from additional folders
#
if ($AdditionDataLocationList -ne $null )
{
  $publicConfig | Add-Member -MemberType NoteProperty -Name "AdditionalData" -Value $AdditionDataLocationList
}

#
# Convert it to JSON format
#
$publicConfigJSON = $publicConfig | ConvertTo-Json
"publicConfig is:  $publicConfigJSON"

#we just provide a empty privateConfig object
$privateconfig = "{
}"

if ($Roles -ne $null)
{
  Set-AzureServiceExtension -Service $ServiceName -Slot $Slot -Role $Roles -ExtensionName 'AzureLogCollector' -ProviderNamespace Microsoft.WindowsAzure.Compute -PublicConfiguration $publicConfigJSON -PrivateConfiguration $privateconfig -Version 1.0 -Verbose
}
else
{
  Set-AzureServiceExtension -Service $ServiceName -Slot $Slot  -ExtensionName 'AzureLogCollector' -ProviderNamespace Microsoft.WindowsAzure.Compute -PublicConfiguration $publicConfigJSON -PrivateConfiguration $privateconfig -Version 1.0 -Verbose
}

#
#This is an optional step: generate a sasUri to the container so it can be shared with other people if nened
#
$SasExpireTime = [DateTime]::Now.AddMinutes(120).ToString("o")
$SasUri = New-AzureStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rl -Context $context
$SasUri = $SasUri + "&restype=container&comp=list"
Write-Output "The container for uploaded file can be accessed using this link:`r`n$sasuri"
```

### <a name="setazurevmlogcollectorps1"></a>SetAzureVMLogCollector.ps1

```powershell
[CmdletBinding(SupportsShouldProcess = $true)]

param (
              [Parameter(Mandatory=$true)]
              [string]   $ServiceName,

              [Parameter(Mandatory=$false)]
              [string] $VMName ,

              [Parameter(Mandatory=$false)]
              [string]   $Mode = 'Full',

              [Parameter(Mandatory=$false)]
              [string]   $StorageAccountName,

              [Parameter(Mandatory=$false)]
              [string]   $StorageAccountKey,

              [Parameter(Mandatory=$false)]
              [PSObject[]] $AdditionDataLocationList = $null
        )

$publicConfig = New-Object PSObject
$publicConfig | Add-Member -MemberType NoteProperty -Name "Instances" -Value "*"

if ($Mode -ne $null )
{
    $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value $Mode
}
else
{
    $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value "Full"
}

#
#we need to get the Sasuri from StorageAccount and containers
#
$context = New-AzureStorageContext -Protocol https -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

$ContainerName = "azurelogcollectordata"
$existingContainer = Get-AzureStorageContainer -Context $context |  Where-Object { $_.Name -like $ContainerName}
if ($existingContainer -eq $null)
{
    "Container ($ContainerName) doesn't exist. Creating it now.."
    New-AzureStorageContainer -Context $context -Name $ContainerName -Permission off
}

$ExpiryTime =  [DateTime]::Now.AddMinutes(90).ToString("o")
$SasUri = New-AzureStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rwl -Context $context
$publicConfig | Add-Member -MemberType NoteProperty -Name "SasUri" -Value $SasUri

#
#Add AdditionalData to collect data from additional folders
#
if ($AdditionDataLocationList -ne $null )
{
  $publicConfig | Add-Member -MemberType NoteProperty -Name "AdditionalData" -Value $AdditionDataLocationList
}

#
# Convert it to JSON format
#
$publicConfigJSON = $publicConfig | ConvertTo-Json

Write-Output "PublicConfiguration is: \r\n$publicConfigJSON"

#
#we just provide a empty privateConfig object
#
$privateconfig = "{
}"

if ($VMName -ne $null )
{
      $VM = Get-AzureVM -ServiceName $ServiceName -Name $VMName
      $VM.VM.OSVirtualHardDisk.OS

      if ($VM.VM.OSVirtualHardDisk.OS -like '*Windows*')
      {
            Set-AzureVMExtension -VM $VM -ExtensionName "AzureLogCollector" -Publisher Microsoft.WindowsAzure.Compute -PublicConfiguration $publicConfigJSON -PrivateConfiguration $privateconfig -Version 1.* | Update-AzureVM -Verbose

            #
            #We will check the VM status to find if operation by extension has been completed or not. The completion of the operation,either succeed or fail, can be indicated by
            #the presence of SubstatusList field.
            #
            $Completed = $false
            while ($Completed -ne $true)
            {
                    $VM = Get-AzureVM -ServiceName $ServiceName -Name $VMName
                    $status = $VM.ResourceExtensionStatusList | Where-Object {$_.HandlerName -eq "Microsoft.WindowsAzure.Compute.AzureLogCollector"}

                    if ( ($status.Code -ne 0) -and ($status.Status -like '*error*'))
                    {
                        Write-Output "Error status is returned: $($Status.ExtensionSettingStatus.FormattedMessage.Message)."
                          $Completed = $true
                    }
                    elseif (($status.ExtensionSettingStatus.SubstatusList -eq $null -or $status.ExtensionSettingStatus.SubstatusList.Count -lt 1))
                    {
                          $Completed = $false
                          Write-Output "Waiting for operation to complete..."
                    }
                    else
                    {
                          $Completed = $true
                          Write-Output "Operation completed."

                    $UploadedFileUri = $Status.ExtensionSettingStatus.SubStatusList[0].FormattedMessage.Message
                          $blob = New-Object Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob($UploadedFileUri)

                  #
                        # This is an optional step:  For easier access to the file, we can generate a read-only SasUri directly to the file
                          #
                          $ExpiryTimeRead =  [DateTime]::Now.AddMinutes(120).ToString("o")
                          $ReadSasUri = New-AzureStorageBlobSASToken -ExpiryTime $ExpiryTimeRead  -FullUri  -Blob  $blob.name -Container $blob.Container.Name -Permission r -Context $context

                        Write-Output "The uploaded file can be accessed using this link: $ReadSasUri"

                          #
                          #This is an optional step:  Remove the extension after we are done
                          #
                          Get-AzureVM -ServiceName $ServiceName -Name $VMName | Set-AzureVMExtension -Publisher Microsoft.WindowsAzure.Compute -ExtensionName "AzureLogCollector" -Version 1.* -Uninstall | Update-AzureVM -Verbose

                    }
                    Start-Sleep -s 5
            }
      }
      else
      {
          Write-Output "VM OS Type is not Windows, the extension cannot be enabled"
      }

}
else
{
  Write-Output "VM name is not specified, the extension cannot be enabled"
}
```

## <a name="next-steps"></a>Volgende stappen
Nu kunt u controleren of uw logboeken van de ene eenvoudige locatie kopiëren.

