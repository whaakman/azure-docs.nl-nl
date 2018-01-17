<!--save a copy of this file to your local repo. It's important that you follow the naming conventions by starting with the service name, and use lowercase only for the file name. See "file-names-and-locations.md" under the "contributor-guide" folder in your repo.

Info to help you use the template are enclosed in the Markdown comments using the caret, hyphen, dash syntax. Delete these from your file.

Text not wrapped in comment syntax is intended to be used as is, or with updates enclosed in [  ]. Add the info and delete the bracket. 

Pay attention to spacing and indents. They affect formatting. 

--> 

<!--replace this with Properties and Tags sections. These are required sections. See "article-metadata.md" in under the "contributor-guide" folder in your repo. Attributes in each section can be placed on separate lines to make them easier to read and check-->

# <a name="use-azure-powershell-to-task"></a>Azure PowerShell gebruiken voor het [task]
In dit artikel laat zien u hoe [taak] met opdrachten van zowel de Azure-module en de Azure Resource Manager-module. Dit is bedoeld om u informatie over de nieuwe opdrachten evenals migreren van bestaande scripts naar de nieuwe opdrachten te helpen.

## <a name="prerequisite-install-a-recent-version-of-azure-powershell"></a>Voorwaarde: Installeer een recente versie van Azure PowerShell
Als u dit nog niet hebt gedaan, installeert u ten minste de [versienummer]-versie van Azure PowerShell op de lokale computer. Als u een eerdere versie gebruikt, geen deze de Azure Resource Manager-cmdlets in dit artikel wordt beschreven. Zie deze artikelen voor meer informatie:

* [Het installeren en configureren van Azure PowerShell](install-configure-powershell.md) voor instructies over het instellen van Azure PowerShell.
* [Met Windows PowerShell met Resource Manager](powershell-azure-resource-manager.md) voor basisbeginselen over het gebruik van Resource Manager.

> [!NOTE]
> De meeste taken moet u met de beheerdersniveau Azure PowerShell-prompt.
> 
> 

## <a name="command-comparison"></a>Vergelijking van de opdracht
Dit [tabel | sectie] toont de opdrachtsyntaxis.

<!--[optional image - to use an image in this article, add a folder with the same name as the article file name without extension, inside the Media folder of the repo. Use only this folder to store the images. Don't attempt to use a common folder to share images you want to use in more than 1 file.]
Then, use the following syntax to add a reference to the image in your article:
![](./media/name-of-file-without-extension/image-name-no-spaces.png)
-->

<!--if a command string uses variables, define the variables first, using the  following construction. In some cases the variable is straightforward and doesn't need much explanation. But parameters such as location and size can benefit from brief explanation or listing all accepted values:--> 

De opdrachtvoorbeelden van deze gebruikt de volgende variabelen:

$FriendlyName"<Describe value>"

<!-- if it makes more sense to present this in a table, use this. Otherwise, delete. The table won't render until it's in GitHub or published to Sandbox.-->

| Servicebeheer | Resource Manager |
| --- | --- |
| `syntax` |`syntax` |

<!--if it makes more sense to present this one command block after the other instead of a table, use this. Otherwise, delete-->

[Korte inleiding zin over de opdracht. Weglaten als er echt niets in te spreken. Maar als u gebruikmaakt van benaderingen, zoals een de pijplijn wordt uitgelegd dat]:

    [command string]

## <a name="script-examples"></a>Voorbeelden van scripts
Hier volgt een voorbeeld waarin [cmdlet namen)] naar [task]. Dit omvat de opdrachten die:

* [korte-term gebruikt, heeft, is, enzovoort]
* [de volgende korte term] 

<!--include this statement if it uses variables that weren't introduced earlier--> It includes the following variables:

* [variabele 1]
* [variabele 2]

<!--This shows you how a recent example was presented as well as how it was formatted. Preceding each line with one tab or four spaces to format in a code block-->

    $family="Windows Server 2012 R2 Datacenter"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vmname="AZDC1"
    $vmsize="Medium"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $cred=Get-Credential -Message "Type the name and password of the local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

    $vm1 | Set-AzureSubnet -SubnetNames "BackEnd"

    $vm1 | Set-AzureStaticVNetIP -IPAddress 192.168.244.4

    $disksize=20
    $disklabel="DCData"
    $lun=0
    $hcaching="None"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

    $svcname="Azure-TailspinToys"
    $vnetname="AZDatacenter"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname


## <a name="additional-resources"></a>Aanvullende resources
<!--At a minimum, include a link back to the migration task list article. Use the formats shown below. See create-links-markdown.md for more info -->
<!--use this format for links to other articles, such as the migration task list. -->
[Beschikbaarheid van beheren](../articles/virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

<!--To link to an ACOM page outside the /documentation/ subdomain (such as a pricing page, SLA page or anything else that is not a documentation article), use an absolute URL, but omit the locale:

    [link text](http://azure.microsoft.com/pricing/details/virtual-machines/)-->

<!--use this for URLs outside of ACOM. Be sure to locale, and if you're linking to the Azure library on MSDN, include the '/azure/' part of the URL-->
[Documentatie voor virtuele machines](https://msdn.microsoft.com/library/azure/jj156003.aspx)

