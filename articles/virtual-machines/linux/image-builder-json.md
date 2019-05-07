---
title: Maken van een sjabloon Azure Image Builder (preview)
description: Informatie over het maken van een sjabloon die u wilt gebruiken met Azure Image Builder.
author: cynthn
ms.author: cynthn
ms.date: 05/02/2019
ms.topic: article
ms.service: virtual-machines-linux
manager: jeconnoc
ms.openlocfilehash: b4646879eb7eeecf41852baab7ab64e4053b05e1
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65159598"
---
# <a name="preview-create-an-azure-image-builder-template"></a>Preview: Een Azure Image Builder-sjabloon maken 

Azure Image Builder maakt gebruik van een .json-bestand om door te geven informatie in de Image Builder-service. In dit artikel gaat we over de secties van het json-bestand, zodat u kunt ze zelf kunt ontwikkelen. Zie voor voorbeelden van volledige .json-bestanden, de [Azure Image Builder GitHub](https://github.com/danielsollondon/azvmimagebuilder/tree/master/quickquickstarts).

Dit is de voor basic-sjabloonindeling:

```json
 { 
    "type": "Microsoft.VirtualMachineImages/imageTemplates", 
    "apiVersion": "2019-05-01-preview", 
    "location": "<region>", 
    "tags": {
        "<name": "<value>",
        "<name>": "<value>"
             }
    "identity":{},           
    "dependsOn": [], 
    "properties": { 
        "<build timeout in minutes>": {}, 
        "build": {}, 
        "customize": {}, 
        "distribute": {} 
      } 
 } 
```



## <a name="type-and-api-version"></a>Type en de API-versie

De `type` is van het resourcetype dat moet worden `"Microsoft.VirtualMachineImages/imageTemplates"`. De `apiVersion` verandert na verloop van tijd als de API wordt gewijzigd, maar moet `"2019-05-01-preview"` voor Preview-versie.

```json
    "type": "Microsoft.VirtualMachineImages/imageTemplates",
    "apiVersion": "2019-05-01-preview",
```

## <a name="location"></a>Locatie

De locatie is de regio waar de aangepaste installatiekopie wordt gemaakt. Voor de Preview Image Builder-versie, worden de volgende regio's ondersteund:

- US - oost
- US - oost 2
- US - west-centraal
- US - west
- US - west 2


```json
    "location": "<region>",
```
    
## <a name="depends-on-optional"></a>Afhankelijk van de (optioneel)

Deze optionele sectie kan worden gebruikt om ervoor te zorgen dat afhankelijkheden zijn voltooid voordat u doorgaat. 

```json
    "dependsOn": [],
```

Zie voor meer informatie, [resourceafhankelijkheden definiëren](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-define-dependencies#dependson).

## <a name="identity"></a>Identiteit
Standaard Image Builder ondersteunt met behulp van scripts of kopiëren van bestanden vanaf meerdere locaties, zoals GitHub en Azure storage. Als u wilt gebruiken, moeten ze openbaar toegankelijk zijn.

U kunt ook een Azure User-Assigned beheerde identiteit, gedefinieerd door u, om toe te staan Image Builder toegang tot Azure Storage, zolang de identiteit een minimum van 'Gegevenslezer voor Opslagblob' in de Azure storage-account heeft gekregen. Dit betekent dat u niet wilt maken van de storage-blobs die extern toegankelijk is, of setup SAS-Tokens.


```json
    "identity": {
    "type": "UserAssigned",
          "userAssignedIdentities": {
            "<imgBuilderId>": {}
        }
        },
```

Zie voor een compleet voorbeeld [ een Azure User-Assigned beheerde identiteit gebruiken om bestanden te openen in Azure Storage](https://github.com/danielsollondon/azvmimagebuilder/tree/master/quickquickstarts/7_Creating_Custom_Image_using_MSI_to_Access_Storage).

Image Builder-ondersteuning voor de identiteit van een User-Assigned: • één identiteit ondersteunt alleen • biedt geen ondersteuning voor aangepaste domeinnamen

Zie voor meer informatie, [wat is beheerde identiteiten voor Azure-resources?](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).
Zie voor meer informatie over het implementeren van deze functie [configureren beheerde identiteiten voor een Azure-resources op een Azure-VM met behulp van Azure CLI](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm#user-assigned-managed-identity).

## <a name="properties-source"></a>Eigenschappen: bron

De `source` sectie bevat informatie over de installatiekopie van de bron die wordt gebruikt door de functie voor installatiekopieën maken.

De API is een 'SourceType' waarin de bron voor de installatiekopie-build vereist, er zijn momenteel drie typen:
- ISO - Gebruik deze instelling als de bron een ISO RHEL is.
- PlatformImage - aangegeven dat de broninstallatiekopie is een Marketplace-installatiekopie.
- ManagedImage - Gebruik deze instelling als u vanaf een beheerde standaardinstallatiekopie.
- SharedImageVersion - dit wordt gebruikt wanneer u de versie van een installatiekopie in de galerie met installatiekopieën van een gedeeld als bron gebruikt.

### <a name="iso-source"></a>ISO-bron

Azure Image Builder ondersteunt alleen het gebruik van gepubliceerde Red Hat Enterprise Linux 7.x binaire DVD ISO's, voor de Preview-versie. Image Builder ondersteunt:
- RHEL 7.3 
- RHEL 7.4 
- RHEL 7.5 
 
```json
"source": {
       "type": "ISO",
       "sourceURI": "<sourceURI from the download center>",
       "sha256Checksum": "<checksum associated with ISO>"
}
```

Aan de `sourceURI` en `sha256Checksum` waarden, Ga naar `https://access.redhat.com/downloads` selecteert u het product **Red Hat Enterprise Linux**, en een ondersteunde versie. 

In de lijst met **installatieprogramma's en -installatiekopieën voor Red Hat Enterprise Linux Server**, moet u de koppeling voor Red Hat Enterprise Linux 7.x binaire DVD en de controlesom kopiëren.

> [!NOTE]
> De toegangstokens te geven van de koppelingen met regelmatige tussenpozen worden vernieuwd, zodat telkens als u wilt een sjabloon indienen, moet u controleren als een koppeling maakt de RH-adres is gewijzigd.
 
### <a name="platformimage-source"></a>PlatformImage bron 
Azure Image Builder ondersteunt de volgende Azure Marketplace-installatiekopieën:
* Ubuntu 18.04
* Ubuntu 16.04
* RHEL 7,6
* CentOS 7,6
* Windows 2016
* Windows 2019

```json
        "source": {
            "type": "PlatformImage",
                "publisher": "Canonical",
                "offer": "UbuntuServer",
                "sku": "18.04-LTS",
                "version": "18.04.201903060"
        },
```


Hier de eigenschappen zijn dezelfde die worden gebruikt voor het maken van VM's met AZ CLI, voer de onderstaande om op te halen van de eigenschappen: 
 
```azurecli-interactive
az vm image list -l westus -f UbuntuServer -p Canonical --output table –-all 
```

> [!NOTE]
> Versie kan niet 'nieuwste', moet u de bovenstaande opdracht gebruiken om een uniek versienummer. 

### <a name="managedimage-source"></a>ManagedImage bron

Hiermee stelt u de broninstallatiekopie als een bestaande beheerde installatiekopie van een gegeneraliseerde VHD of een virtuele machine. Beheerde installatiekopie van de bron moet worden van een ondersteund besturingssysteem, en worden in dezelfde regio als uw Azure Image Builder-sjabloon. 

```json
        "source": { 
            "type": "ManagedImage", 
                "imageId": "/subscriptions/<subscriptionId>/resourceGroups/{destinationResourceGroupName}/providers/Microsoft.Compute/images/<imageName>"
        }
```

De `imageId` moet de ResourceId van de beheerde installatiekopie. Gebruik `az image list` om beschikbare installatiekopieën weer te geven.


### <a name="sharedimageversion-source"></a>SharedImageVersion bron
Hiermee stelt u de broninstallatiekopie een bestaande versie van de installatiekopie in de galerie met installatiekopieën van een gedeeld. Versie van de installatiekopie moet zijn van een ondersteund besturingssysteem en de afbeelding moet worden gerepliceerd naar de dezelfde regio bevinden als uw Azure Image Builder-sjabloon. 

```json
        "source": { 
            "type": "SharedImageVersion", 
            "imageVersionID": "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/p  roviders/Microsoft.Compute/galleries/<sharedImageGalleryName>/images/<imageDefinitionName/versions/<imageVersion>" 
   } 
```

De `imageVersionId` moet de ResourceId van versie van de installatiekopie. Gebruik [lijst met de versie van de installatiekopie van de az sig](/cli/azure/sig/image-version#az-sig-image-version-list) naar versies van de installatiekopie van weergeven.

## <a name="properties-customize"></a>Eigenschappen: aanpassen


Image Builder biedt ondersteuning voor meerdere aanpassers. Aanpassers zijn functies die worden gebruikt voor het aanpassen van uw installatiekopie, zoals het uitvoeren van scripts of opnieuw opstarten van servers. 

Bij het gebruik van `customize`: 
- U kunt meerdere aanpassers gebruiken, maar ze hebben een unieke `name`.
- Aanpassers uitvoeren in de volgorde die is opgegeven in de sjabloon.
- Als één systeemaanpasser mislukt, wordt het hele aanpassing onderdeel de mislukt en een fout rapporteren.
- Houd rekening met hoeveel tijd uw installatiekopie-build wordt vereist, en aanpassen van de eigenschap 'buildTimeoutInMinutes' zodat er voldoende image builder voldoende tijd om te voltooien.
- Het wordt ten zeerste aanbevolen dat u het script grondig testen voordat u deze gebruikt in een sjabloon. Opsporen van fouten in het script op uw eigen virtuele machine is eenvoudig.
- Plaats gevoelige gegevens niet in de scripts. 
- De locaties van het script moeten worden openbaar toegankelijk zijn, tenzij u [MSI](https://github.com/danielsollondon/azvmimagebuilder/tree/master/quickquickstarts/7_Creating_Custom_Image_using_MSI_to_Access_Storage).

```json
        "customize": [
            {
                "type": "Shell",
                "name": "<name>",
                "scriptUri": "<path to script>"
            },
            {
                "type": "Shell",
                "name": "<name>",
                "inline": [
                    "<command to run inline>",
                ]
            }

        ],
```     

 
De sectie aanpassen is een matrix. Azure Image Builder wordt uitgevoerd via de aanpassers in opeenvolgende volgorde. Een storing in een systeemaanpasser mislukt het bouwproces. 
 
 
### <a name="shell-customizer"></a>Shell systeemaanpasser

De systeemaanpasser shell ondersteunt shellscripts uitvoeren, moet deze openbaar toegankelijk zijn voor de IB voor toegang tot deze.

```json
    "customize": [ 
        { 
            "type": "Shell", 
            "name": "<name>", 
            "scriptUri": "<link to script>"        
        }, 
    ], 
        "customize": [ 
        { 
            "type": "Shell", 
            "name": "<name>", 
            "inline": "<commands to run>"
        }, 
    ], 
```

Ondersteuning voor het besturingssysteem: Linux 
 
Eigenschappen aanpassen:

- **type** -Shell 
- **naam** : naam voor het bijhouden van de aanpassing 
- **scriptUri** -URI naar de locatie van het bestand 
- **inline** -matrix van shell-opdrachten, gescheiden door komma's.
 
> [!NOTE]
> Bij het uitvoeren van de shell-systeemaanpasser met ISO RHEL-bron, moet u ervoor zorgen van uw eerste aanpassing shell verwerkt met een server van de rechten van een Red Hat registreren voordat er een aanpassing optreedt. Zodra aanpassing voltooid is, moet de registratie van het script verwijderen met de server waar u recht op.

### <a name="windows-restart-customizer"></a>Windows opnieuw opstarten systeemaanpasser 
De systeemaanpasser opnieuw starten kunt u een Windows-VM opnieuw opstarten en wacht totdat deze weer online komt, Hiermee kunt u voor het installeren van software die worden opgestart moet.  

```json 
     "customize": [ 
            "type{ ": "WindowsRestart", 
            "restartCommand": "shutdown /r /f /t 0 /c", 
            "restartCheckCommand": "echo Azure-Image-Builder-Restarted-the-VM  > buildArtifacts/azureImageBuilderRestart.txt",
            "restartTimeout": "5m"
         }],
```

Ondersteuning voor het besturingssysteem: Windows
 
Eigenschappen aanpassen:
- **Type**: WindowsRestart
- **restartCommand** -opdracht voor het uitvoeren van het opnieuw opstarten (optioneel). De standaardwaarde is `'shutdown /r /f /t 0 /c \"packer restart\"'`.
- **restartCheckCommand** -opdracht om te controleren als opnieuw opstarten is voltooid (optioneel). 
- **restartTimeout** -out die is opgegeven als een tekenreeks van de grootte en de eenheid opnieuw. Bijvoorbeeld, `5m` (5 minuten) of `2h` (2 uur). De standaardwaarde is: '5m'


### <a name="powershell-customizer"></a>PowerShell systeemaanpasser 
De systeemaanpasser shell ondersteunt met PowerShell-scripts en inline-opdracht, moet de scripts openbaar toegankelijk zijn voor de IB voor toegang tot deze zijn.

```json 
     "customize": [
        { 
             "type": "PowerShell",
             "name":   "<name>",  
             "scriptUri": "<path to script>" 
        },  
        { 
             "type": "PowerShell", 
             "name": "<name>", 
             "inline": "<PowerShell syntax to run>", 
             "valid_exit_codes": "<exit code>" 
         } 
    ], 
```

Ondersteuning voor het besturingssysteem: Windows en Linux

Eigenschappen aanpassen:

- **type** – PowerShell.
- **scriptUri** -URI naar de locatie van de PowerShell-scriptbestand. 
- **inline** – Inline-opdrachten om te worden uitgevoerd, gescheiden door komma's.
- **valid_exit_codes** : optioneel, geldige codes die kunnen worden geretourneerd van het script/inline-opdracht, dit zal voorkomen dat de gerapporteerde fout van het script/inline-opdracht.

### <a name="file-customizer"></a>Bestand systeemaanpasser

De systeemaanpasser bestand kunt image builder downloaden van een bestand vanuit een GitHub- of Azure storage. Hebt u een installatiekopie van build-pijplijn die is gebaseerd op de build-artefacten, kunt u vervolgens de systeemaanpasser bestand te downloaden van de build-share instellen en de artefacten verplaatsen naar de installatiekopie.  

```json
     "customize": [ 
         { 
            "type": "File", 
             "name": "<name>", 
             "sourceUri": "<source location>",
             "destination": "<destination>" 
         }
     ]
```

Ondersteuning voor het besturingssysteem: Linux- en Windows 

Bestandseigenschappen systeemaanpasser:

- **sourceUri** -een opslageindpunt dat toegankelijk is dit GitHub- of Azure-opslag kan zijn. U kunt slechts één bestand, niet een hele map downloaden. Als u nodig hebt voor het downloaden van een directory, gebruikt u een gecomprimeerd bestand, wordt met behulp van de Shell of PowerShell aanpassers decomprimeren. 
- **bestemming** : dit is de naam van de volledige doel pad en de bestandsnaam. Elk waarnaar wordt verwezen, pad en de submappen bestaat, gebruikt u de Shell of PowerShell aanpassers deze vooraf instellen op. Het script aanpassers kunt u het pad maken. 

Dit wordt ondersteund door de mappen van Windows en Linux-paden, maar er zijn enkele verschillen: 
- Linux-besturingssysteem van – het enige pad Image builder naar schrijven kunt is map/Tmp.
- Windows: geen beperkingen pad, maar het pad moet bestaan.
 
 
Als er een fout is probeert te downloaden van het bestand en plaatst deze in een bepaalde map, mislukken de stap aanpassen en dit is in de customization.log.

Bestanden in het bestand systeemaanpasser kunnen worden gedownload uit Azure Storage met behulp van [MSI](https://github.com/danielsollondon/azvmimagebuilder/tree/master/quickquickstarts/7_Creating_Custom_Image_using_MSI_to_Access_Storage).

### <a name="generalize"></a>Generaliseren 
Standaard wordt Azure Image Builder ook de inrichting' code aan het einde van elke fase van de afbeelding aanpassen aan generaliseer de installatiekopie uitgevoerd. Generaliseren is een proces waarbij de afbeelding is ingesteld, zodat deze opnieuw kan worden gebruikt voor het maken van meerdere virtuele machines. Azure Image Builder maakt gebruik van Sysprep voor Windows-VM's. Voor Linux, Azure Image Builder wordt uitgevoerd ' waagent-inrichting '. 

De opdrachten Image Builder-gebruikers om te generaliseren zijn mogelijk niet geschikt is voor elke situatie, zodat Azure Image Builder u deze opdracht kunt, indien nodig aanpassen. 

Als u bestaande aanpassing migreert en u verschillende Sysprep/waagent-opdrachten gebruikt, kunt u met behulp van de Image Builder algemene opdrachten, en als het maken van de virtuele machine is mislukt, kunt uw eigen Sysprep of in waagent-opdrachten.

Als Azure Image Builder maakt u een aangepaste installatiekopie van Windows is en u een virtuele machine van maken en zoeken die het maken van de virtuele machine is mislukt of niet wordt voltooid, moet u Raadpleeg de documentatie van Windows Server Sysprep of een aanvraag voor ondersteuning met de Windows Server Sysprep-klantondersteuning Services-team, die kan oplossen en advies geven over de juiste Sysprep-gebruik.


#### <a name="default-sysprep-command"></a>De opdracht Sysprep standaard
```powershell
echo '>>> Waiting for GA to start ...'
while ((Get-Service RdAgent).Status -ne 'Running') { Start-Sleep -s 5 }
while ((Get-Service WindowsAzureTelemetryService).Status -ne 'Running') { Start-Sleep -s 5 }
while ((Get-Service WindowsAzureGuestAgent).Status -ne 'Running') { Start-Sleep -s 5 }
echo '>>> Sysprepping VM ...'
if( Test-Path $Env:SystemRoot\\windows\\system32\\Sysprep\\unattend.xml ){ rm $Env:SystemRoot\\windows\\system32\\Sysprep\\unattend.xml -Force} & $Env:SystemRoot\\System32\\Sysprep\\Sysprep.exe /oobe /generalize /quiet /quit
while($true) { $imageState = Get-ItemProperty HKLM:\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Setup\\State | Select ImageState; if($imageState.ImageState -ne 'IMAGE_STATE_GENERALIZE_RESEAL_TO_OOBE') { Write-Output $imageState.ImageState; Start-Sleep -s 5  } else { break } }
```
#### <a name="default-linux-deprovision-command"></a>Inrichting van de standaardopdracht-Linux

```bash
/usr/sbin/waagent -force -deprovision+user && export HISTSIZE=0 && sync
```

#### <a name="overriding-the-commands"></a>De opdrachten te overschrijven
Als u wilt overschrijven de opdrachten, gebruikt u de PowerShell- of -Shell-script-provisioners de om opdrachtbestanden te maken met de exacte naam en plaats u ze in de juiste mappen:

* Windows: c:\DeprovisioningScript.ps1
* Linux: /tmp/DeprovisioningScript.sh

Image Builder wordt deze opdrachten worden gelezen en deze worden geschreven naar de logboeken AIB 'customization.log'. Zie [probleemoplossing](https://github.com/danielsollondon/azvmimagebuilder/blob/master/troubleshootingaib.md#collecting-and-reviewing-aib-logs) over het verzamelen van Logboeken.
 
## <a name="properties-distribute"></a>Eigenschappen: distribueren

Azure Image Builder ondersteunt drie distributie doelen: 

- **managedImage** - beheerde installatiekopie.
- **sharedImage** -galerie met installatiekopieën gedeeld.
- **VHD** -VHD in een storage-account.

U kunt een installatiekopie distribueert naar beide van de doeltypen in dezelfde configuratie, raadpleegt u [voorbeelden](https://github.com/danielsollondon/azvmimagebuilder/blob/7f3d8c01eb3bf960d8b6df20ecd5c244988d13b6/armTemplates/azplatform_image_deploy_sigmdi.json#L80).

Omdat er meer dan één doel moet worden gedistribueerd, Image Builder onderhoudt een status voor elke distributie doel dat kan worden geopend door het uitvoeren van query's de `runOutputName`.  De `runOutputName` is een object dat u kunt een query voor informatie over het distribueren van die van de distributie plaatsen. U kunt bijvoorbeeld de locatie van de VHD- of regio's waar de versie van de installatiekopie is gerepliceerd naar een query. Dit is een eigenschap van het doel van elke distributie. De `runOutputName` moet uniek zijn voor elk doel distributie.
 
### <a name="distribute-managedimage"></a>Distribueren: managedImage

De uitvoer van de installatiekopie is een beheerde installatiekopie-resource.

```json
"distribute": [
        {
"type":"managedImage",
       "imageId": "<resource ID>",
       "location": "<region>",
       "runOutputName": "<name>",
       "artifactTags": {
            "<name": "<value>",
             "<name>": "<value>"
               }
         }]
```
 
Distribueer eigenschappen:
- **type** – managedImage 
- **imageId** -Resource-ID van de installatiekopie van het doel, verwachte indeling: /subscriptions/<subscriptionId>/resourceGroups/<destinationResourceGroupName>/providers/Microsoft.Compute/images/<imageName>
- **locatie** -locatie van de beheerde installatiekopie.  
- **runOutputName** : de unieke naam voor het identificeren van de distributie.  
- **artifactTags** -optioneel door de gebruiker opgegeven waarde-paar labels.
 
 
> [!NOTE]
> De doelresourcegroep moet bestaan.
> Als u wilt dat de installatiekopie die wordt gedistribueerd naar een andere regio, wordt de implementatietijd verhoogd. 

### <a name="distribute-sharedimage"></a>Distribueren: sharedImage 
De galerie met installatiekopieën gedeelde Azure is een nieuwe Image Management-service waarmee beheren van replicatie voor de installatiekopie van regio's, versiebeheer en het delen van aangepaste installatiekopieën. Azure Image Builder biedt ondersteuning voor distributie met deze service, zodat u installatiekopieën van regio's ondersteund door gedeelde Afbeeldingsgalerieën kunt distribueren. 
 
De galerie met installatiekopieën van een gedeelde bestaat uit: 
 
- Galerie - Container voor meerdere gedeelde afbeeldingen. Een galerie wordt geïmplementeerd in één regio.
- Afbeelding definities - een conceptuele groepering voor afbeeldingen. 
- Versies van een installatiekopie - dit is een type installatiekopie die wordt gebruikt voor het implementeren van een virtuele machine of scale set. Versies van een installatiekopie kunnen worden gerepliceerd naar andere regio's waar virtuele machines moeten worden geïmplementeerd.
 
Voordat u kunt distribueren naar de galerie met installatiekopieën, moet u een galerie en de definitie van een installatiekopie maken, Zie [Gedeelde afbeeldingen](shared-images.md). 

```json
{
     "type": "sharedImage",
     "galleryImageId": “<resource ID>”,
     "runOutputName": "<name>",
     "artifactTags": {
          "<name": "<value>",
           "<name>": "<value>"
             }
     "replicationRegions": [
        "<region where the gallery is deployed>",
        "<region>"
    ]}
``` 

Eigenschappen voor gedeelde afbeeldingsgalerieën distribueren:

- **type** -sharedImage  
- **galleryImageId** -ID van de galerie met installatiekopieën van de gedeelde. De indeling: /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.Compute/galleries/<sharedImageGalleryName>/images/<imageGalleryName>.
- **runOutputName** : de unieke naam voor het identificeren van de distributie.  
- **artifactTags** -optioneel door de gebruiker opgegeven waarde-paar labels.
- **replicationRegions** -matrix van regio's voor replicatie. Een van de regio's moet de regio waar de galerie is geïmplementeerd.
 
> [!NOTE]
> U kunt Azure Image Builder gebruiken in een andere regio voor de galerie, maar de service Azure Image Builder moet de installatiekopie van het overbrengen tussen de datacenters en dit duurt langer. Image Builder wordt automatisch versie van de afbeelding, op basis van een monotone geheel getal zijn, u kunt dit momenteel niet opgeven. 

### <a name="distribute-vhd"></a>Distribueren: VHD  
U kunt uitvoeren op een VHD. U kunt vervolgens de VHD kopiëren en gebruiken om te publiceren op Azure MarketPlace of gebruik met Azure Stack.  

```json
 { 
     "type": "VHD",
     "runOutputName": "<VHD name>",
     "tags": {
          "<name": "<value>",
           "<name>": "<value>"
             }
 }
```
 
Ondersteuning voor het besturingssysteem: Windows en Linux

Distribueer VHD parameters:

- **type** -VHD.
- **runOutputName** : de unieke naam voor het identificeren van de distributie.  
- **tags** -optioneel door de gebruiker opgegeven waarde-paar labels.
 
Azure Image Builder staat niet toe dat de gebruiker om op te geven van een opslaglocatie voor de account, maar u kunt de status van een query de `runOutputs` om op te halen van de locatie.  

```azurecli-interactive
az resource show \
   --ids "/subscriptions/$subscriptionId/resourcegroups/<imageResourceGroup>/providers/Microsoft.VirtualMachineImages/imageTemplates/<imageTemplateName>/runOutputs/<runOutputName>"  | grep artifactUri 
```

> [!NOTE]
> Nadat de VHD is gemaakt, kopieert u deze zo snel mogelijk naar een andere locatie. De VHD is opgeslagen in een opslagaccount in de tijdelijke resourcegroep gemaakt als de sjabloon van de installatiekopie wordt verzonden naar de service Azure Image Builder. Als u de sjabloon afbeelding verwijdert, verliest u de VHD. 
 
## <a name="next-steps"></a>Volgende stappen

Er zijn voorbeeldgegevens .json-bestanden voor verschillende scenario's in de [Azure Image Builder GitHub](https://github.com/danielsollondon/azvmimagebuilder).
 
 
 
 
 
 
 
 
 
 
