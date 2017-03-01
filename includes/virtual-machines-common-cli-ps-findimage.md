

## <a name="azure-cli-20-preview"></a>Azure CLI 2.0 (Preview)

Wanneer u [Azure CLI 2.0 (Preview) hebt geïnstalleerd](https://docs.microsoft.com/cli/azure/install-az-cli2), gebruikt u de opdracht `az vm image list` om een cachelijst met populaire VM-installatiekopieën weer te geven. Dit voorbeeld van de opdracht `az vm image list -o table` geeft bijvoorbeeld het volgende weer:

```
You are viewing an offline list of images, use --all to retrieve an up-to-date list
Offer          Publisher               Sku                 Urn                                                             UrnAlias             Version
-------------  ----------------------  ------------------  --------------------------------------------------------------  -------------------  ---------
WindowsServer  MicrosoftWindowsServer  2012-R2-Datacenter  MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest  Win2012R2Datacenter  latest
WindowsServer  MicrosoftWindowsServer  2008-R2-SP1         MicrosoftWindowsServer:WindowsServer:2008-R2-SP1:latest         Win2008R2SP1         latest
WindowsServer  MicrosoftWindowsServer  2012-Datacenter     MicrosoftWindowsServer:WindowsServer:2012-Datacenter:latest     Win2012Datacenter    latest
UbuntuServer   Canonical               14.04.4-LTS         Canonical:UbuntuServer:14.04.4-LTS:latest                       UbuntuLTS            latest
CentOS         OpenLogic               7.2                 OpenLogic:CentOS:7.2:latest                                     CentOS               latest
openSUSE       SUSE                    13.2                SUSE:openSUSE:13.2:latest                                       openSUSE             latest
RHEL           RedHat                  7.2                 RedHat:RHEL:7.2:latest                                          RHEL                 latest
SLES           SUSE                    12-SP1              SUSE:SLES:12-SP1:latest                                         SLES                 latest
Debian         credativ                8                   credativ:Debian:8:latest                                        Debian               latest
CoreOS         CoreOS                  Stable              CoreOS:CoreOS:Stable:latest                                     CoreOS               latest
```

### <a name="finding-all-current-images"></a>Alle huidige installatiekopieën zoeken

Gebruik de opdracht `az vm image list` met de optie `--all` om de huidige lijst met alle installatiekopieën weer te geven. In tegenstelling tot de Azure CLI 1.0-opdrachten retourneert de opdracht `az vm image list --all` standaard alle installatiekopieën in **VS West** (tenzij u een bepaald `--location`-argument opgeeft), waardoor de opdracht `--all` enige tijd in beslag neemt. Als u interactief wilt onderzoeken, gebruikt u `az vm image list --all > allImages.json`, dat een lijst van alle installatiekopieën retourneert die momenteel beschikbaar zijn in Azure en ze opslaat als een bestand voor lokaal gebruik. 

U kunt een van de verschillende opties specificeren om uw zoekopdracht te beperken tot een specifieke locatie, aanbieding, uitgever of SKU als u er al een of meer in gedachten hebt. Als u geen locatie specificeert, worden de waarden voor **VS West** geretourneerd.

### <a name="find-specific-images"></a>Specifieke installatiekopieën zoeken

Gebruik `az vm image list` met een [JMESPATH-queryfilter](https://docs.microsoft.com/cli/azure/query-az-cli2) om specifieke informatie te vinden. Het volgende geeft bijvoorbeeld de **SKU**'s weer die beschikbaar zijn voor **Debian** (houd er rekening mee dat zonder de schakelaar `--all` alleen de lokale cache van algemene installatiekopieën wordt doorzocht):

```azurecli
az vm image list --query '[?contains(offer,`Debian`)]' -o table --all
```

De uitvoer ziet er ongeveer zo uit: 
```
You are viewing an offline list of images, use --all to retrieve an up-to-date list
  Sku  Publisher    Offer    Urn                       Version    UrnAlias
-----  -----------  -------  ------------------------  ---------  ----------
    8  credativ     Debian   credativ:Debian:8:latest  latest     Debian

<list shortened for the example>
```

Als u weet waar u gaat implementeren, kunt u de algemene zoekresultaten voor installatiekopieën gebruiken samen met de opdrachten `az vm image list-skus`, `az vm image list-offers` en `az vm image list-publishers` om te bepalen wat u precies wilt en waar het kan worden geïmplementeerd. Als u bijvoorbeeld uit het vorige voorbeeld weet dat `credativ` een Debian-aanbieding heeft, kunt u `--location` en andere opties gebruiken om precies te bepalen wat u wilt. In het volgende voorbeeld wordt gezocht naar een installatiekopie voor Debian 8 in **Europa West**:

```azurecli 
az vm image show -l westeurope -f debian -p credativ --skus 8 --version 8.0.201701180
```

en de uitvoer is:

```json
{
  "dataDiskImages": [],
  "id": "/Subscriptions/<guid>/Providers/Microsoft.Compute/Locations/westeurope/Publishers/credativ/ArtifactTypes/VMImage/Offers/debian/Skus/8/Versions/8.0.201701180",
  "location": "westeurope",
  "name": "8.0.201701180",
  "osDiskImage": {
    "operatingSystem": "Linux"
  },
  "plan": null,
  "tags": null
}
```

## <a name="azure-cli-10"></a>Azure CLI 1.0 

> [!NOTE]
> In dit artikel staat hoe u installatiekopieën voor virtuele machines zoekt en selecteert met een installatie van Azure CLI 1.0 of Azure PowerShell die ondersteuning biedt voor het Azure Resource Manager-implementatiemodel. Het is hiervoor vereist om over te schakelen naar de modus Resource Manager. Ga met behulp van de Azure CLI naar die modus door `azure config mode arm` in te voeren. 
> 

De snelste manier om een installatiekopie te vinden, is door de opdracht `azure vm image list` aan te roepen en de locatie, de naam van de uitgever (niet hoofdlettergevoelig) en een aanbieding (als u deze kent) door te geven. De volgende lijst is maar een kort voorbeeld (veel lijsten zijn behoorlijk lang) voor als u weet dat Canonical de uitgever is van de aanbieding UbuntuServer.

```azurecli
azure vm image list westus canonical ubuntuserver
info:    Executing command vm image list
warn:    The parameter --sku if specified will be ignored
+ Getting virtual machine image skus (Publisher:"canonical" Offer:"ubuntuserver" Location:"westus")
data:    Publisher  Offer         Sku                OS     Version          Location  Urn
data:    ---------  ------------  -----------------  -----  ---------------  --------  --------------------------------------------------------
data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201604203  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201604203
data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201605161  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201605161
data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201606100  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201606100
data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201606270  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201606270
data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201607210  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201607210
data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201608150  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201608150
data:    canonical  ubuntuserver  16.10-DAILY        Linux  16.10.201607220  westus    canonical:ubuntuserver:16.10-DAILY:16.10.201607220
data:    canonical  ubuntuserver  16.10-DAILY        Linux  16.10.201607230  westus    canonical:ubuntuserver:16.10-DAILY:16.10.201607230
data:    canonical  ubuntuserver  16.10-DAILY        Linux  16.10.201607240  westus    canonical:ubuntuserver:16.10-DAILY:16.10.201607240
```

De kolom **Urn** is de vorm die u doorgeeft aan `azure vm quick-create`.

Vaak weet u echter nog niet wat er beschikbaar is. In dat geval kunt u installatiekopieën doorzoeken door de opdracht `azure vm image list-publishers` te gebruiken en bij de prompt een locatie voor het datacenter op te geven. De volgende lijst bevat bijvoorbeeld alle uitgevers van installatiekopieën op de locatie VS West (geef het locatieargument door door de standaardlocaties met kleine letters en zonder spaties in te voeren)

```azurecli
azure vm image list-publishers
info:    Executing command vm image list-publishers
Location: westus
+ Getting virtual machine and/or extension image publishers (Location: "westus")
data:    Publisher                                       Location
data:    ----------------------------------------------  --------
data:    a10networks                                     westus  
data:    aiscaler-cache-control-ddos-and-url-rewriting-  westus  
data:    alertlogic                                      westus  
data:    AlertLogic.Extension                            westus  
```

Deze lijsten kunnen erg lang zijn. De voorgaande voorbeeldlijst is slechts een fragment. Stel dat ik heb gezien dat Canonical inderdaad uitgever van installatiekopieën is op de locatie VS West. U kunt nu de aanbiedingen van deze leverancier vinden door `azure vm image list-offers` aan te roepen, en via de prompts de locatie en uitgever door te geven, zoals in het volgende voorbeeld:

```azurecli
azure vm image list-offers
info:    Executing command vm image list-offers
Location: westus
Publisher: canonical
+ Getting virtual machine image offers (Publisher: "canonical" Location:"westus")
data:    Publisher  Offer                      Location
data:    ---------  -------------------------  --------
data:    canonical  Ubuntu15.04Snappy          westus
data:    canonical  Ubuntu15.04SnappyDocker    westus
data:    canonical  UbunturollingSnappy        westus
data:    canonical  UbuntuServer               westus
data:    canonical  Ubuntu_Snappy_Core         westus
data:    canonical  Ubuntu_Snappy_Core_Docker  westus
info:    vm image list-offers command OK
```

Nu weten we dat in de regio VS West Canonical de **UbuntuServer**-aanbieding publiceert in Azure. Maar welke SKU's? Als u deze waarden wilt zien, kunt u `azure vm image list-skus` aanroepen en de prompt beantwoorden met de locatie, uitgever en aanbieding die u hebt gevonden.

```azurecli
azure vm image list-skus
info:    Executing command vm image list-skus
Location: westus
Publisher: canonical
Offer: ubuntuserver
+ Getting virtual machine image skus (Publisher:"canonical" Offer:"ubuntuserver" Location:"westus")
data:    Publisher  Offer         sku                Location
data:    ---------  ------------  -----------------  --------
data:    canonical  ubuntuserver  12.04.2-LTS        westus
data:    canonical  ubuntuserver  12.04.3-LTS        westus
data:    canonical  ubuntuserver  12.04.4-LTS        westus
data:    canonical  ubuntuserver  12.04.5-DAILY-LTS  westus
data:    canonical  ubuntuserver  12.04.5-LTS        westus
data:    canonical  ubuntuserver  12.10              westus
data:    canonical  ubuntuserver  14.04-beta         westus
data:    canonical  ubuntuserver  14.04.0-LTS        westus
data:    canonical  ubuntuserver  14.04.1-LTS        westus
data:    canonical  ubuntuserver  14.04.2-LTS        westus
data:    canonical  ubuntuserver  14.04.3-LTS        westus
data:    canonical  ubuntuserver  14.04.4-DAILY-LTS  westus
data:    canonical  ubuntuserver  14.04.4-LTS        westus
data:    canonical  ubuntuserver  14.04.5-DAILY-LTS  westus
data:    canonical  ubuntuserver  14.04.5-LTS        westus
data:    canonical  ubuntuserver  14.10              westus
data:    canonical  ubuntuserver  14.10-beta         westus
data:    canonical  ubuntuserver  14.10-DAILY        westus
data:    canonical  ubuntuserver  15.04              westus
data:    canonical  ubuntuserver  15.04-beta         westus
data:    canonical  ubuntuserver  15.04-DAILY        westus
data:    canonical  ubuntuserver  15.10              westus
data:    canonical  ubuntuserver  15.10-alpha        westus
data:    canonical  ubuntuserver  15.10-beta         westus
data:    canonical  ubuntuserver  15.10-DAILY        westus
data:    canonical  ubuntuserver  16.04-alpha        westus
data:    canonical  ubuntuserver  16.04-beta         westus
data:    canonical  ubuntuserver  16.04.0-DAILY-LTS  westus
data:    canonical  ubuntuserver  16.04.0-LTS        westus
data:    canonical  ubuntuserver  16.10-DAILY        westus
info:    vm image list-skus command OK
```

Met deze gegevens kunt u nu de gewenste installatiekopie vinden door de oorspronkelijke aanroep bovenaan aan te roepen.

```azurecli
azure vm image list westus canonical ubuntuserver 16.04.0-LTS
info:    Executing command vm image list
+ Getting virtual machine images (Publisher:"canonical" Offer:"ubuntuserver" Sku: "16.04.0-LTS" Location:"westus")
data:    Publisher  Offer         Sku          OS     Version          Location  Urn
data:    ---------  ------------  -----------  -----  ---------------  --------  --------------------------------------------------
data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201604203  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201604203
data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201605161  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201605161
data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201606100  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201606100
data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201606270  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201606270
data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201607210  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201607210
data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201608150  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201608150
info:    vm image list command OK
```

Nu kunt u precies de installatiekopie kiezen die u wilt gebruiken. Zie [De Azure CLI voor Mac, Linux en Windows gebruiken met Azure Resource Manager](../articles/xplat-cli-azure-resource-manager.md) voor meer informatie over hoe u snel een virtuele machine maakt met de zojuist gevonden URN-gegevens en over hoe u een sjabloon gebruikt met deze URN-gegevens.

## <a name="powershell"></a>PowerShell
> [!NOTE]
> De [nieuwste versie van Azure PowerShell](/powershell/azureps-cmdlets-docs) installeren en configureren. Als u Azure PowerShell-modules lager dan versie 1.0 gebruikt, past u nog steeds de volgende opdrachten toe, maar moet u eerst `Switch-AzureMode AzureResourceManager`. 
> 
> 

Bij het maken van een nieuwe virtuele machine met Azure Resource Manager geeft u in bepaalde gevallen een installatiekopie op in de vorm van een combinatie van de volgende installatiekopie-eigenschappen:

* Uitgever
* Aanbieding
* SKU

Deze waarden zijn bijvoorbeeld nodig voor de PowerShell-cmdlet `Set-AzureRMVMSourceImage` of in een sjabloonbestand voor een resourcegroep waarin u het type virtuele machine moet opgeven dat moet worden gemaakt.

Als u deze waarden nog niet hebt, gaat u naar de betreffende installatiekopieën om ze te bepalen:

1. Geef de uitgevers van installatiekopieën weer.
2. Geef de aanbiedingen voor een bepaalde uitgever weer.
3. Geef de SKU's voor een bepaalde aanbieding weer.

Geef eerst de uitgevers weer met de volgende opdrachten:

```powershell
$locName="<Azure location, such as West US>"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName
```

Vul de naam van de gewenste uitgever in en voer de volgende opdrachten uit:

```powershell
$pubName="<publisher>"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

Vul de naam van de gewenste aanbieding in en voer de volgende opdrachten uit:

```powershell
$offerName="<offer>"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

In de weergave van de opdracht `Get-AzureRMVMImageSku` ziet u alle informatie die u nodig hebt om de installatiekopie voor een nieuwe virtuele machine op te geven.

Hier volgt een compleet voorbeeld:

```powershell
PS C:\> $locName="West US"
PS C:\> Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName

PublisherName
-------------
a10networks
aiscaler-cache-control-ddos-and-url-rewriting-
alertlogic
AlertLogic.Extension
Barracuda.Azure.ConnectivityAgent
barracudanetworks
basho
boxless
bssw
Canonical
...
```

Voor de uitgever MicrosoftWindowsServer:

```powershell
PS C:\> $pubName="MicrosoftWindowsServer"
PS C:\> Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer

Offer
-----
WindowsServer
```

Voor de aanbieding WindowsServer:

```powershell
PS C:\> $offerName="WindowsServer"
PS C:\> Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus

Skus
----
2008-R2-SP1
2012-Datacenter
2012-R2-Datacenter
2016-Nano-Server-Technical-Previe
2016-Technical-Preview-with-Conta
Windows-Server-Technical-Preview
```

Kopieer de naam van de gewenste SKU uit deze lijst. Nu hebt u alle benodigde gegevens voor de PowerShell-cmdlet `Set-AzureRMVMSourceImage` of voor een resourcegroepsjabloon.

<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png
[8]: ./media/markdown-template-for-new-articles/copytemplate.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[gog]: http://google.com/
[yah]: http://search.yahoo.com/  
[msn]: http://search.msn.com/

<!--HONumber=Feb17_HO3-->


