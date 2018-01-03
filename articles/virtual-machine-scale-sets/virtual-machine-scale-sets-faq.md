---
title: Azure virtuele-machineschaalsets Veelgestelde vragen | Microsoft Docs
description: Vind antwoorden op veelgestelde vragen over virtuele-machineschaalsets.
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/12/2017
ms.author: negat
ms.custom: na
ms.openlocfilehash: 52be84b73e70a02c43ef71917dc272060d82b42d
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="azure-virtual-machine-scale-sets-faqs"></a>Azure virtuele-machineschaalsets Veelgestelde vragen

Vind antwoorden op veelgestelde vragen over virtuele-machineschaalsets in Azure.

## <a name="autoscale"></a>Automatisch schalen

### <a name="what-are-best-practices-for-azure-autoscale"></a>Wat zijn de aanbevolen procedures voor het Azure-automatisch schalen?

Zie voor aanbevolen procedures voor automatisch schalen [aanbevolen procedures voor automatisch schalen virtuele machines](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-best-practices).

### <a name="where-do-i-find-metric-names-for-autoscaling-that-uses-host-based-metrics"></a>Waar vind ik metrische namen voor automatisch schalen die gebruikmaakt van de host gebaseerde metrische gegevens

Zie voor namen van meetwaarde voor automatisch schalen die gebruikmaakt van de host gebaseerde metrische gegevens [ondersteund met een Azure-Monitor](https://azure.microsoft.com/documentation/articles/monitoring-supported-metrics/).

### <a name="are-there-any-examples-of-autoscaling-based-on-an-azure-service-bus-topic-and-queue-length"></a>Zijn er geen voorbeelden van automatisch schalen op basis van een Azure Service Bus-onderwerp en wachtrij-lengte?

Ja. Zie voor voorbeelden van automatisch schalen op basis van een Azure Service Bus-onderwerp en wachtrij-lengte [Azure Monitor automatisch schalen algemene metrische gegevens](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/).

Gebruik de volgende JSON voor een Service Bus-wachtrij:

```json
"metricName": "MessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue"
```

Gebruik de volgende JSON voor een opslagwachtrij:

```json
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ClassicStorage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
```

Van de voorbeeldwaarden vervangt door uw resource Uniform Resource-id's (URI).


### <a name="should-i-autoscale-by-using-host-based-metrics-or-a-diagnostics-extension"></a>Moet ik automatisch schalen met behulp van metrische gegevens op de host of een extensie voor diagnostische gegevens?

U kunt een instelling voor automatisch schalen maken op een virtuele machine op hostniveau metrische gegevens of Gast OS gebaseerde metrische gegevens te gebruiken.

Zie voor een lijst van ondersteunde metrische gegevens [Azure Monitor automatisch schalen algemene metrische gegevens](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-common-metrics). 

Zie voor een volledige voorbeeld voor virtuele-machineschaalsets [geavanceerde automatisch schalen configureren met behulp van Resource Manager-sjablonen voor virtuele-machineschaalsets](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-advanced-autoscale-virtual-machine-scale-sets). 

Dit voorbeeld worden de hostniveau CPU metrische gegevens en een aantal bericht metriek.



### <a name="how-do-i-set-alert-rules-on-a-virtual-machine-scale-set"></a>Hoe stel waarschuwingsregels op een virtuele-machineschaalset?

U kunt waarschuwingen maken op de metrische gegevens voor de virtuele-machineschaalsets via PowerShell of Azure CLI. Zie voor meer informatie [Azure Monitor PowerShell Quick Start-voorbeelden](https://azure.microsoft.com/documentation/articles/insights-powershell-samples/#create-alert-rules) en [Azure Monitor platformoverschrijdende CLI Quick Start-voorbeelden](https://azure.microsoft.com/documentation/articles/insights-cli-samples/#work-with-alerts).

De TargetResourceId van de virtuele-machineschaalset ziet er als volgt: 

/Subscriptions/yoursubscriptionid/resourceGroups/yourresourcegroup/providers/Microsoft.COMPUTE/virtualMachineScaleSets/yourvmssname

U kunt elk prestatiemeteritem VM als de metriek in te stellen van een waarschuwing voor. Zie voor meer informatie [Gastbesturingssysteem metrische gegevens voor VM's van Windows Resource Manager gebaseerde](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#guest-os-metrics-resource-manager-based-windows-vms) en [Gastbesturingssysteem metrische gegevens voor virtuele Linux-machines](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#guest-os-metrics-linux-vms) in de [Azure Monitor automatisch schalen algemene metrische gegevens](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/)artikel.

### <a name="how-do-i-set-up-autoscale-on-a-virtual-machine-scale-set-by-using-powershell"></a>Hoe stel ik automatisch schalen op een virtuele-machineschaalset ingesteld met behulp van PowerShell

Als u wilt instellen op een virtuele-machineschaalset ingesteld met behulp van PowerShell voor automatisch schalen, Zie het blogbericht [automatisch schalen toevoegen aan een virtuele machine van Azure-schaalset](https://msftstack.wordpress.com/2017/03/05/how-to-add-autoscale-to-an-azure-vm-scale-set/).




## <a name="certificates"></a>Certificaten

### <a name="how-do-i-securely-ship-a-certificate-to-the-vm-how-do-i-provision-a-virtual-machine-scale-set-to-run-a-website-where-the-ssl-for-the-website-is-shipped-securely-from-a-certificate-configuration-the-common-certificate-rotation-operation-would-be-almost-the-same-as-a-configuration-update-operation-do-you-have-an-example-of-how-to-do-this"></a>Hoe veilig verzend ik een certificaat met de virtuele machine? Hoe ik inrichten met een virtuele-machineschaalset ingesteld op een website waar de SSL voor de website wordt verzonden veilig uitvoeren van een certificaatconfiguratie? (De algemene certificaat rotatie bewerking zou zijn bijna hetzelfde als de bewerking configuratie bijwerken.) Hebt u een voorbeeld van hoe u dit doet? 

Als u wilt een certificaat voor de VM veilig te verzenden, kunt u een certificaat van de klant rechtstreeks in een Windows-certificaatarchief van de sleutelkluis van de klant te installeren.

Gebruik de volgende JSON:

```json
"secrets": [
    {
        "sourceVault": {
            "id": "/subscriptions/{subscriptionid}/resourceGroups/myrg1/providers/Microsoft.KeyVault/vaults/mykeyvault1"
        },
        "vaultCertificates": [
            {
                "certificateUrl": "https://mykeyvault1.vault.azure.net/secrets/{secretname}/{secret-version}",
                "certificateStore": "certificateStoreName"
            }
        ]
    }
]
```

De code biedt ondersteuning voor Windows en Linux.

Zie voor meer informatie [maken of bijwerken een virtuele-machineschaalset](https://msdn.microsoft.com/library/mt589035.aspx).


### <a name="example-of-self-signed-certificate"></a>Voorbeeld van een zelfondertekend certificaat

1.  Een zelfondertekend certificaat maken in een sleutelkluis.

    Gebruik de volgende PowerShell-opdrachten:

    ```powershell
    Import-Module "C:\Users\mikhegn\Downloads\Service-Fabric-master\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"

    Login-AzureRmAccount

    Invoke-AddCertToKeyVault -SubscriptionId <Your SubID> -ResourceGroupName KeyVault -Location westus -VaultName MikhegnVault -CertificateName VMSSCert -Password VmssCert -CreateSelfSignedCertificate -DnsName vmss.mikhegn.azure.com -OutputPath c:\users\mikhegn\desktop\
    ```

    Met deze opdracht biedt u de invoer voor de Azure Resource Manager-sjabloon.

    Zie voor een voorbeeld van een zelfondertekend certificaat maken in een sleutelkluis [scenario's voor beveiliging van Service Fabric-cluster](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/).

2.  Wijzig de Resource Manager-sjabloon.

    Deze eigenschap toevoegen aan **virtualMachineProfile**, als onderdeel van de virtuele machine schaalset resource:

    ```json 
    "osProfile": {
        "computerNamePrefix": "[variables('namingInfix')]",
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]",
        "secrets": [
            {
                "sourceVault": {
                    "id": "[resourceId('KeyVault', 'Microsoft.KeyVault/vaults', 'MikhegnVault')]"
                },
                "vaultCertificates": [
                    {
                        "certificateUrl": "https://mikhegnvault.vault.azure.net:443/secrets/VMSSCert/20709ca8faee4abb84bc6f4611b088a4",
                        "certificateStore": "My"
                    }
                ]
            }
        ]
    }
    ```
  

### <a name="can-i-specify-an-ssh-key-pair-to-use-for-ssh-authentication-with-a-linux-virtual-machine-scale-set-from-a-resource-manager-template"></a>Kan ik een SSH-sleutelpaar moet worden gebruikt voor SSH-verificatie met een Linux-virtuele machine schaal ingesteld op basis van een Resource Manager-sjabloon opgeven?  

Ja. De REST-API voor **osProfile** is vergelijkbaar met de standard VM REST-API. 

Omvatten **osProfile** in uw sjabloon:

```json 
"osProfile": {
    "computerName": "[variables('vmName')]",
    "adminUsername": "[parameters('adminUserName')]",
    "linuxConfiguration": {
        "disablePasswordAuthentication": "true",
        "ssh": {
            "publicKeys": [
                {
                    "path": "[variables('sshKeyPath')]",
                    "keyData": "[parameters('sshKeyData')]"
                }
            ]
        }
    }
}
```
 
Dit blok JSON wordt gebruikt in [de 101-vm-SSH-sleutelbestand GitHub Quick Start-sjabloon](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json).
 
Het profiel wordt ook gebruikt bij [snelstartsjabloon met de grelayhost.json GitHub](https://github.com/ExchMaster/gadgetron/blob/master/Gadgetron/Templates/grelayhost.json).

Zie voor meer informatie [maken of bijwerken een virtuele-machineschaalset](https://msdn.microsoft.com/library/azure/mt589035.aspx#linuxconfiguration).
  

### <a name="how-do-i-remove-deprecated-certificates"></a>Hoe kan ik afgeschaft certificaten verwijderen? 

Om te verwijderen afgeschaft certificaten, het oude certificaat in de lijst van de certificaten kluis te verwijderen. Laat de certificaten die u wilt blijven op de computer in de lijst. Het certificaat wordt niet verwijderd uit alle virtuele machines. Deze ook heeft niet het certificaat toevoegen aan nieuwe virtuele machines die zijn gemaakt in de virtuele-machineschaalset. 

Schrijven om het certificaat verwijderen uit bestaande virtuele machines, een extensie voor aangepaste scripts u handmatig de certificaten uit uw certificaatarchief.
 
### <a name="how-do-i-inject-an-existing-ssh-public-key-into-the-virtual-machine-scale-set-ssh-layer-during-provisioning-i-want-to-store-the-ssh-public-key-values-in-azure-key-vault-and-then-use-them-in-my-resource-manager-template"></a>Hoe ik een bestaande openbare SSH-sleutel invoeren in de virtuele machine scale set SSH-laag tijdens het inrichten? Ik wil de openbare SSH-sleutelwaarden opslaan in Azure Sleutelkluis en deze vervolgens in mijn Resource Manager-sjabloon gebruiken.

Als u de virtuele machines alleen met een openbare SSH-sleutel biedt, moet u niet om de openbare sleutels voor Sleutelkluis. Openbare sleutels zijn niet geheim.
 
Wanneer u een Linux-VM maakt, kunt u openbare SSH-sleutels als tekst zonder opmaak bieden:

```json
"linuxConfiguration": {
    "ssh": {
        "publicKeys": [
            {
                "path": "path",
                "keyData": "publickey"
            }
        ]
    }
```
 
linuxConfiguration elementnaam | Vereist | Type | Beschrijving
--- | --- | --- | --- |  ---
SSH | Nee | Verzameling | Hiermee geeft u de belangrijkste SSH-configuratie voor een Linux-besturingssysteem
pad | Ja | Tekenreeks | Hiermee geeft u het pad van het Linux waar de SSH-sleutels of het certificaat moet zich bevinden
keyData | Ja | Tekenreeks | Hiermee geeft u een base64-gecodeerd openbare SSH-sleutel

Zie voor een voorbeeld [de 101-vm-SSH-sleutelbestand GitHub Quick Start-sjabloon](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json).

 
### <a name="when-i-run-update-azurermvmss-after-adding-more-than-one-certificate-from-the-same-key-vault-i-see-the-following-message"></a>Wanneer ik uitvoeren `Update-AzureRmVmss` na het toevoegen van meer dan één certificaat van de dezelfde sleutelkluis, verschijnt het volgende bericht:
 
>Update-AzureRmVmss: Lijst geheim bevat herhaalde exemplaren van /subscriptions/ < Mijn-abonnement-id > / resourceGroups/internal-rg-dev/providers/Microsoft.KeyVault/vaults/internal-keyvault-dev die niet is toegestaan.
 
Dit kan gebeuren als u probeert opnieuw toevoegen dezelfde kluis in plaats van een nieuw certificaat voor de kluis voor de bestaande bronkluis. De `Add-AzureRmVmssSecret` opdracht werkt niet goed als u extra geheimen toevoegt.
 
Als u wilt meer geheimen van de dezelfde sleutelkluis toevoegt, moet u de $vmss.properties.osProfile.secrets[0].vaultCertificates-lijst bijgewerkt.
 
Zie voor de verwachte invoerstructuur [maken of bijwerken wordt ingesteld door een virtuele machine](https://msdn.microsoft.com/library/azure/mt589035.aspx).
 
Het geheim niet vinden in de virtuele machine scale set-object dat zich in de sleutelkluis. Vervolgens voegt u uw certificaatverwijzing (de URL en de naam van de geheime store) aan de lijst die zijn gekoppeld aan de kluis.

> [!NOTE] 
> Op dit moment kan u certificaten verwijderen uit de virtuele machines met behulp van de virtuele machine scale set API.
>

Nieuwe virtuele machines hebben niet het oude certificaat. Virtuele machines die het certificaat hebben en die al zijn geïmplementeerd hebben echter het oude certificaat.
 
### <a name="can-i-push-certificates-to-the-virtual-machine-scale-set-without-providing-the-password-when-the-certificate-is-in-the-secret-store"></a>Kan ik certificaten push naar de virtuele-machineschaalset instellen zonder het wachtwoord als het certificaat in de geheime store is?

U hoeft niet te hardcoded wachtwoorden in scripts. U kunt dynamisch ophalen van wachtwoorden met de machtigingen die u met het implementatiescript uitvoeren. Als u een script dat een certificaat van de geheime archiefsleutel verplaatst hebt kluis, de geheime store `get certificate` opdracht levert ook het wachtwoord van het pfx-bestand.
 
### <a name="how-does-the-secrets-property-of-virtualmachineprofileosprofile-for-a-virtual-machine-scale-set-work-why-do-i-need-the-sourcevault-value-when-i-have-to-specify-the-absolute-uri-for-a-certificate-by-using-the-certificateurl-property"></a>Hoe de eigenschap geheimen van virtualMachineProfile.osProfile voor een virtuele-machineschaalset werk instellen? Waarom moet ik de waarde sourceVault wanneer ik heb de absolute URI voor een certificaat opgeven via de eigenschap certificateUrl? 

Een verwijzing van Windows Remote Management (WinRM)-certificaat moet aanwezig zijn in de eigenschap geheimen van het profiel. 

Het doel van die wijzen op de bronkluis is om af te dwingen toegangsbeheerlijst (ACL) toegangscontrolebeleid die aanwezig zijn in Azure Cloud Service-model van een gebruiker. Als de bronkluis niet wordt opgegeven, zou gebruikers die niet gemachtigd zijn om te implementeren of toegang tot een sleutelkluis geheimen kunnen via Compute Resource Provider (CRP). ACL's bestaan zelfs voor bronnen die niet bestaan.

Als u een onjuiste bron kluis-ID, maar een geldige sleutelkluis-URL opgeeft, wordt er een fout gerapporteerd bij het pollen van de bewerking.
 
### <a name="if-i-add-secrets-to-an-existing-virtual-machine-scale-set-are-the-secrets-injected-into-existing-vms-or-only-into-new-ones"></a>Als ik geheimen toevoegen aan een bestaande wordt virtuele-machineschaalset ingesteld, worden de geheimen ingevoegd in de bestaande virtuele machines, of alleen nieuwe? 

Certificaten worden toegevoegd aan alle virtuele machines, zelfs vooraf bestaande toepassingsgroepen. Als uw virtuele-machineschaalset upgradePolicy-eigenschap is ingesteld op **handmatige**, het certificaat wordt toegevoegd aan de virtuele machine wanneer u een handmatige update op de virtuele machine uitvoeren.
 
### <a name="where-do-i-put-certificates-for-linux-vms"></a>Waar ik certificaten plaatsen voor virtuele Linux-machines?

Zie voor meer informatie over het implementeren van certificaten voor Linux VM's, [certificaten implementeren op virtuele machines van een door de klant beheerd sleutelkluis](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/).
  
### <a name="how-do-i-add-a-new-vault-certificate-to-a-new-certificate-object"></a>Hoe voeg een nieuw certificaat voor de kluis naar een nieuw certificaatobject?

Als u wilt een kluis-certificaat toevoegen aan een bestaande geheim, Zie het volgende PowerShell-voorbeeld. Gebruik slechts één geheime object.
 
```powershell
$newVaultCertificate = New-AzureRmVmssVaultCertificateConfig -CertificateStore MY -CertificateUrl https://sansunallapps1.vault.azure.net:443/secrets/dg-private-enc/55fa0332edc44a84ad655298905f1809
 
$vmss.VirtualMachineProfile.OsProfile.Secrets[0].VaultCertificates.Add($newVaultCertificate)
 
Update-AzureRmVmss -VirtualMachineScaleSet $vmss -ResourceGroup $rg -Name $vmssName
```
 
### <a name="what-happens-to-certificates-if-you-reimage-a-vm"></a>Wat gebeurt er met certificaten als de installatiekopie van een virtuele machine?

Als u een virtuele machine installatiekopie, worden certificaten verwijderd. Hiermee verwijdert u het volledige besturingssysteem schijf teruggezet. 
 
### <a name="what-happens-if-you-delete-a-certificate-from-the-key-vault"></a>Wat gebeurt er als u een certificaat uit de sleutelkluis verwijderen?

Als het geheim wordt verwijderd uit de sleutelkluis en u voert `stop deallocate` voor alle VM's en start deze opnieuw, of er een fout optreedt. De fout treedt op omdat de CRP hoeft op te halen van de geheimen van de sleutelkluis, maar niet kunnen worden. In dit scenario kunt u de certificaten verwijderen uit het model van virtuele machine scale set. 

Het onderdeel CRP geheimen van de klant niet bewaard is gebleven. Als u `stop deallocate` voor alle virtuele machines in de virtuele-machineschaalset, de cache wordt verwijderd. In dit scenario worden geheimen opgehaald uit de sleutelkluis.

U kunt dit probleem niet tegenkomen wanneer uitbreiden, omdat er een kopie van het geheim in de Azure Service Fabric (in het model van één fabric tenant).
 
### <a name="why-do-i-have-to-specify-the-exact-location-for-the-certificate-url-httpsname-of-the-vaultvaultazurenet443secretsexact-location-as-indicated-in-service-fabric-cluster-security-scenarioshttpsazuremicrosoftcomdocumentationarticlesservice-fabric-cluster-security"></a>Waarom heb ik de exacte locatie opgeven voor de certificaat-URL (https://<name of the vault>.vault.azure.net:443/secrets/<exact location>), zoals aangegeven in [scenario's voor beveiliging van Service Fabric-cluster](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/)?
 
De documentatie van Azure Sleutelkluis wordt aangegeven dat de ophalen geheim REST-API de nieuwste versie van het geheim retourneren moet als de versie is niet opgegeven.
 
Methode | URL
--- | ---
GET | https://mykeyvault.Vault.Azure.NET/secrets/ {geheim name} / {geheim versie}? api-version = {api-versie}

Vervang {*geheim naam*} met de naam en vervang {*geheim versie*} met de versie van het geheim die u wilt ophalen. De geheime versie kan worden uitgesloten. In dat geval wordt wordt de huidige versie opgehaald.
  
### <a name="why-do-i-have-to-specify-the-certificate-version-when-i-use-key-vault"></a>Waarom heb ik geef de certificaatversie wanneer ik Sleutelkluis?

Het doel van de Sleutelkluis-vereiste om op te geven van de certificaatversie is zodat het voor de gebruiker duidelijk welk certificaat wordt geïmplementeerd op hun virtuele machines.

Als u een virtuele machine maken en werk vervolgens het geheim in de sleutelkluis, wordt het nieuwe certificaat wordt niet gedownload naar uw virtuele machines. Maar uw virtuele machines worden weergegeven als verwijzing en nieuwe virtuele machines ophalen van het nieuwe geheim. Om dit te voorkomen, moet u verwijzen naar een geheime versie.

### <a name="my-team-works-with-several-certificates-that-are-distributed-to-us-as-cer-public-keys-what-is-the-recommended-approach-for-deploying-these-certificates-to-a-virtual-machine-scale-set"></a>Mijn team werkt met verschillende certificaten die worden gedistribueerd naar ons als cer openbare sleutels. Wat is de aanbevolen aanpak voor het implementeren van deze certificaten op een virtuele-machineschaalset instellen?

Openbare sleutels op een schaal van de virtuele machine ingesteld, kunt u een .pfx-bestand met CER-bestanden genereren .cer implementeren. U doet dit door gebruik `X509ContentType = Pfx`. Bijvoorbeeld: het cer-bestand laden als een object x509Certificate2 in C# of PowerShell en roept u de methode. 

Zie voor meer informatie [X509Certificate.Export methode (X509ContentType, String)](https://msdn.microsoft.com/library/24ww6yzk(v=vs.110.aspx)).

### <a name="i-do-not-see-an-option-for-users-to-pass-in-certificates-as-base64-strings-most-other-resource-providers-have-this-option"></a>Een optie voor gebruikers om op te geven certificaten als base64-tekenreeksen ik niet wordt weergegeven. De meeste andere resourceproviders hebt deze optie.

Om te emuleren doorgeven in een certificaat als een base64-tekenreeks, kunt u de meest recente samengestelde URL in het Resource Manager-sjabloon extraheren. Neem de volgende JSON-eigenschap in de Resource Manager-sjabloon:

```json 
"certificateUrl": "[reference(resourceId(parameters('vaultResourceGroup'), 'Microsoft.KeyVault/vaults/secrets', parameters('vaultName'), parameters('secretName')), '2015-06-01').secretUriWithVersion]"
```
 
### <a name="do-i-have-to-wrap-certificates-in-json-objects-in-key-vaults"></a>Heb ik inpakken van certificaten in JSON-objecten in sleutelkluizen?

In de virtuele-machineschaalsets en virtuele machines moeten certificaten worden verpakt in JSON-objecten. 

We bieden ook ondersteuning voor het inhoudstype application/x-pkcs12. Zie voor instructies over het gebruik van de x-toepassing-pkcs12 [PFX-certificaten in Azure Key Vault](http://www.rahulpnath.com/blog/pfx-certificate-in-azure-key-vault/).
 
We ondersteunen momenteel geen cer-bestanden. .Cer-bestanden wilt gebruiken, moet u deze in containers .pfx exporteren.



## <a name="compliance-and-security"></a>Compatibiliteit en beveiliging

### <a name="are-virtual-machine-scale-sets-pci-compliant"></a>Zijn de virtuele-machineschaalsets PCI-compatibel?

Schaalsets van virtuele machines zijn een thin-API-laag bovenop de CRP. Beide componenten maken deel uit van het computerplatform in de boomstructuur van de Azure-service.

Vanuit het perspectief van naleving zijn schaalsets van virtuele machines een fundamenteel onderdeel van het Azure-computerplatform. Ze delen een team, hulpprogramma's, processen, implementatiemethode, beveiligingsmechanismen, JIT-compilatie (just-in-time), bewaking, waarschuwingen enz. met de CRP zelf. Schaalsets van virtuele machines zijn compatibel met PCI (Payment Card Industry) omdat de CRP deel uitmaakt van het huidige PCI DSS-attestation (Data Security Standard).

Zie [Microsoft Trust Center](https://www.microsoft.com/TrustCenter/Compliance/PCI) voor meer informatie.

### <a name="does-azure-managed-service-identityhttpsdocsmicrosoftcomazureactive-directorymsi-overview-work-with-virtual-machine-scale-sets"></a>Biedt [Azure Managed Service-identiteit](https://docs.microsoft.com/azure/active-directory/msi-overview) werken met virtuele-machineschaalsets?

Ja. Hier ziet u enkele voorbeeld MSI-sjablonen in Azure Quickstart-sjablonen. Linux: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-msi-linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-msi-linux). Windows: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-msi-windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-msi-windows).


## <a name="extensions"></a>Extensies

### <a name="how-do-i-delete-a-virtual-machine-scale-set-extension"></a>Hoe verwijder ik een virtuele machine uitbreiding van de schaal

Gebruik de volgende PowerShell-voorbeeld voor het verwijderen van een uitbreiding van schaal de virtuele machine:

```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName "resource_group_name" -VMScaleSetName "vmssName" 

$vmss=Remove-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "extensionName"

Update-AzureRmVmss -ResourceGroupName "resource_group_name" -VMScaleSetName "vmssName" -VirtualMacineScaleSet $vmss
```
 
U vindt de waarde extensienaam in `$vmss`.
   
### <a name="is-there-a-virtual-machine-scale-set-template-example-that-integrates-with-operations-management-suite"></a>Er is dat een virtuele-machineschaalset voorbeeldsjabloon die met Operations Management Suite integreert?

Voor een virtuele-machineschaalset ingesteld voorbeeld van de sjabloon die in combinatie met Operations Management Suite, Zie het tweede voorbeeld in [een Azure Service Fabric-cluster implementeren en schakel de bewaking met behulp van logboekanalyse](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/ServiceFabric).
   
### <a name="extensions-seem-to-run-in-parallel-on-virtual-machine-scale-sets-this-causes-my-custom-script-extension-to-fail-what-can-i-do-to-fix-this"></a>Extensies lijken te parallel worden uitgevoerd op virtuele-machineschaalsets. Dit zorgt ervoor dat de extensie voor aangepaste scripts mislukken. Wat kan ik doen om dit probleem oplossen?

Zie voor meer informatie over het sequentiëren van de uitbreiding in virtuele-machineschaalsets, [sequentiëren uitbreiding in virtuele Azure-machine-schaalsets](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/).
 
 
### <a name="how-do-i-reset-the-password-for-vms-in-my-virtual-machine-scale-set"></a>Hoe stel ik het wachtwoord voor virtuele machines in mijn virtuele-machineschaalset

Er zijn twee manieren voor het wijzigen van het wachtwoord voor virtuele machines in-schaalsets.

- De virtuele machine scale set model rechtstreeks wijzigen. Beschikbaar met Compute API 2017-12-01 en hoger.

    Werk de beheerdersreferenties rechtstreeks in het model met een schaal (bijvoorbeeld met de Azure Resource Explorer, PowerShell of CLI). Zodra de schaalaanpassingsset bijgewerkt, worden alle nieuwe is hebben virtuele machines de nieuwe referenties Bestaande virtuele machines hebben alleen de nieuwe referenties als ze zijn teruggezet. 

- Het wachtwoord met behulp van de VM-extensies voor toegang opnieuw instellen.

    Gebruik het volgende PowerShell-voorbeeld:
    
    ```powershell
    $vmssName = "myvmss"
    $vmssResourceGroup = "myvmssrg"
    $publicConfig = @{"UserName" = "newuser"}
    $privateConfig = @{"Password" = "********"}
     
    $extName = "VMAccessAgent"
    $publisher = "Microsoft.Compute"
    $vmss = Get-AzureRmVmss -ResourceGroupName $vmssResourceGroup -VMScaleSetName $vmssName
    $vmss = Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name $extName -Publisher $publisher -Setting $publicConfig -ProtectedSetting $privateConfig -Type $extName -TypeHandlerVersion "2.0" -AutoUpgradeMinorVersion $true
    Update-AzureRmVmss -ResourceGroupName $vmssResourceGroup -Name $vmssName -VirtualMachineScaleSet $vmss
    ```


### <a name="how-do-i-add-an-extension-to-all-vms-in-my-virtual-machine-scale-set"></a>Hoe voeg ik een extensie voor alle VM's in mijn virtuele-machineschaalset

Als updatebeleid is ingesteld op **automatische**, alle virtuele machines opnieuw distribueren van de sjabloon met de extensie-eigenschappen van nieuwe updates.

Als updatebeleid is ingesteld op **handmatige**eerst werk de extensie en alle exemplaren in uw virtuele machines vervolgens handmatig bijwerken.

  
### <a name="if-the-extensions-associated-with-an-existing-virtual-machine-scale-set-are-updated-are-existing-vms-affected-that-is-will-the-vms-not-match-the-virtual-machine-scale-set-model-or-are-they-ignored-when-an-existing-machine-is-service-healed-or-reimaged-are-the-scripts-that-are-currently-configured-on-the-virtual-machine-scale-set-executed-or-are-the-scripts-that-were-configured-when-the-vm-was-first-created-used"></a>Als de uitbreidingen die zijn gekoppeld aan een bestaande virtuele-machineschaalset worden bijgewerkt, zijn de virtuele machines die van invloed op een bestaande? (Dat wil zeggen, wordt de virtuele machines *niet* overeen met het model van virtuele machine scale set?) Of worden ze genegeerd? Wanneer een bestaande computer wordt service-moeten worden hersteld of teruggezet, zijn de scripts die momenteel zijn geconfigureerd op de virtuele-machineschaalset uitgevoerd of zijn gebruikt de scripts die zijn geconfigureerd als de virtuele machine is gemaakt?

Als de definitie van de extensie in de virtuele-machineschaalset ingesteld model wordt bijgewerkt en de eigenschap upgradePolicy is ingesteld op **automatische**, het bijwerken van de virtuele machines. Als de eigenschap upgradePolicy is ingesteld op **handmatige**, extensies zijn gemarkeerd als niet overeenkomt met het model. 

Als een bestaande VM-service moeten worden hersteld, wordt deze weergegeven als de computer opnieuw is opgestart en de extensies worden niet opnieuw uitgevoerd. Als deze wordt teruggezet, is als de OS-schijf vervangen door de broninstallatiekopie. Alle specialisatie uit het laatste model, zoals-extensies worden uitgevoerd.
 
### <a name="how-do-i-join-a-virtual-machine-scale-set-to-an-azure-ad-domain"></a>Hoe ik deelnemen aan een virtuele-machineschaalset ingesteld op een Azure AD-domein?

Als u wilt deelnemen aan een virtuele-machineschaalset ingesteld op een domein, Azure Active Directory (Azure AD), kunt u een uitbreiding definiëren. 

Gebruik de eigenschap JsonADDomainExtension definiëren van een uitbreiding:

```json
"extensionProfile": {
    "extensions": [
        {
            "name": "joindomain",
            "properties": {
                "publisher": "Microsoft.Compute",
                "type": "JsonADDomainExtension",
                "typeHandlerVersion": "1.3",
                "settings": {
                    "Name": "[parameters('domainName')]",
                    "OUPath": "[variables('ouPath')]",
                    "User": "[variables('domainAndUsername')]",
                    "Restart": "true",
                    "Options": "[variables('domainJoinOptions')]"
                },
                "protectedsettings": {
                    "Password": "[parameters('domainJoinPassword')]"
                }
            }
        }
    ]
}
```
 
### <a name="my-virtual-machine-scale-set-extension-is-trying-to-install-something-that-requires-a-reboot-for-example-commandtoexecute-powershellexe--executionpolicy-unrestricted-install-windowsfeature-name-fs-resource-manager-includemanagementtools"></a>De extensie scale set van mijn virtuele machine probeert te installeren iets dat moet worden opgestart. Bijvoorbeeld 'commandToExecute': ' powershell.exe - ExecutionPolicy Unrestricted Install-WindowsFeature – naam FS-Resource-Manager – IncludeManagementTools '

Als uw scale set-extensie van virtuele machine installeren iets dat moet worden opgestart wilt, kunt u de extensie Azure Automation Desired State Configuration (DSC automatisering). Als het besturingssysteem Windows Server 2012 R2 is, wordt Azure ophaalt in de Windows Management Framework (WMF) 5.0-instellingen opnieuw wordt opgestart, en vervolgens doorgaat met de configuratie. 
 
### <a name="how-do-i-turn-on-antimalware-in-my-virtual-machine-scale-set"></a>Hoe kan ik antimalware inschakelen in mijn virtuele-machineschaalset?

Om in te schakelen anti-malware op uw virtuele-machineschaalset, gebruik de volgende PowerShell-voorbeeld:

```powershell
$rgname = 'autolap'
$vmssname = 'autolapbr'
$location = 'eastus'
 
# Retrieve the most recent version number of the extension.
$allVersions= (Get-AzureRmVMExtensionImage -Location $location -PublisherName "Microsoft.Azure.Security" -Type "IaaSAntimalware").Version
$versionString = $allVersions[($allVersions.count)-1].Split(".")[0] + "." + $allVersions[($allVersions.count)-1].Split(".")[1]
 
$VMSS = Get-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname
echo $VMSS
Add-AzureRmVmssExtension -VirtualMachineScaleSet $VMSS -Name "IaaSAntimalware" -Publisher "Microsoft.Azure.Security" -Type "IaaSAntimalware" -TypeHandlerVersion $versionString
Update-AzureRmVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $VMSS 
```

### <a name="i-need-to-execute-a-custom-script-thats-hosted-in-a-private-storage-account-the-script-runs-successfully-when-the-storage-is-public-but-when-i-try-to-use-a-shared-access-signature-sas-it-fails-this-message-is-displayed-missing-mandatory-parameters-for-valid-shared-access-signature-linksas-works-fine-from-my-local-browser"></a>Ik moet een aangepast script dat wordt gehost in een persoonlijke opslagaccount uitvoeren. Het script met succes wordt uitgevoerd wanneer de opslag is openbaar, maar wanneer ik probeer te gebruiken een Shared Access Signature (SAS), is mislukt. Dit bericht wordt weergegeven: 'Verplichte parameters ontbreekt voor geldige Shared Access Signature'. Koppeling + SAS werkt prima vanuit Mijn lokale browser.

Voor het uitvoeren van een aangepast script dat wordt gehost in een persoonlijke opslagaccount, beveiligde instellingen instellen met de opslagaccountsleutel en de naam. Zie voor meer informatie [aangepast Script uitbreiding voor Windows](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/#template-example-for-a-windows-vm-with-protected-settings).


## <a name="networking"></a>Netwerken
 
### <a name="is-it-possible-to-assign-a-network-security-group-nsg-to-a-scale-set-so-that-it-applies-to-all-the-vm-nics-in-the-set"></a>Is het mogelijk een Netwerkbeveiligingsgroep (NSG) toewijzen aan een schaalset, zodat deze van toepassing op alle VM NIC's in de set?

Ja. Een Netwerkbeveiligingsgroep kan rechtstreeks op een schaal ingesteld door ernaar wordt verwezen in de sectie networkInterfaceConfigurations van het netwerkprofiel worden toegepast. Voorbeeld:

```json
"networkProfile": {
    "networkInterfaceConfigurations": [
        {
            "name": "nic1",
            "properties": {
                "primary": "true",
                "ipConfigurations": [
                    {
                        "name": "ip1",
                        "properties": {
                            "subnet": {
                                "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                            }
                "loadBalancerInboundNatPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                                }
                            ],
                            "loadBalancerBackendAddressPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                                 }
                            ]
                        }
                    }
                ],
                "networkSecurityGroup": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/networkSecurityGroups/', variables('nsgName'))]"
                }
            }
        }
    ]
}
```

### <a name="how-do-i-do-a-vip-swap-for-virtual-machine-scale-sets-in-the-same-subscription-and-same-region"></a>Hoe kan ik geen VIP's wisselen voor virtuele-machineschaalsets in het hetzelfde abonnement en dezelfde regio dat doen?

Als er twee virtuele-machineschaalsets met de front-ends van Azure Load Balancer en ze zich in hetzelfde abonnement en dezelfde regio, kan u toewijzing van het openbare IP-adressen van elkaar en toewijzen aan de andere. Zie [VIP's wisselen: blauw groen implementatie in Azure Resource Manager](https://msftstack.wordpress.com/2017/02/24/vip-swap-blue-green-deployment-in-azure-resource-manager/) bijvoorbeeld. Dit betekent dat er een vertraging maar de bronnen worden weergegeven op het netwerk ongedaan/toegewezen niveau. Een snellere mogelijkheid is het gebruik van Azure Application Gateway met twee back-endpools en een regel voor doorsturen. U kunt ook u uw toepassing met kan host [-Azure App service](https://azure.microsoft.com/services/app-service/) die ondersteuning biedt voor het snel overschakelen tussen sites omwisselen fasering en productie.
 
### <a name="how-do-i-specify-a-range-of-private-ip-addresses-to-use-for-static-private-ip-address-allocation"></a>Hoe geef ik een bereik van particuliere IP-adressen te gebruiken voor statische privé IP-adrestoewijzing

IP-adressen zijn geselecteerd uit een subnet op dat u opgeeft. 

De toewijzingsmethode van virtuele machine scale set IP-adressen is altijd 'dynamische' hoeft niet, maar dat dat deze IP-adressen kunnen wijzigen. In dit geval betekent 'dynamische' alleen dat u het IP-adres niet in een PUT-aanvraag opgeeft. Geef de statische ingesteld met behulp van het subnet. 
    
### <a name="how-do-i-deploy-a-virtual-machine-scale-set-to-an-existing-azure-virtual-network"></a>Hoe implementeer ik een virtuele-machineschaalset ingesteld op een bestaande virtuele Azure-netwerk 

Zie voor het implementeren van een virtuele-machineschaalset ingesteld op een bestaand virtueel netwerk van Azure [implementeren een schaal van de virtuele machine ingesteld op een bestaand virtueel netwerk](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-existing-vnet). 

### <a name="how-do-i-add-the-ip-address-of-the-first-vm-in-a-virtual-machine-scale-set-to-the-output-of-a-template"></a>Hoe kan ik het IP-adres van de eerste virtuele machine in een virtuele-machineschaalset ingesteld op de uitvoer van een sjabloon toevoegen

Zie voor informatie over het toevoegen van het IP-adres van de eerste virtuele machine in een virtuele-machineschaalset ingesteld op de uitvoer van een sjabloon [Azure Resource Manager: Get virtuele-machineschaalsets privé IPs](http://stackoverflow.com/questions/42790392/arm-get-vmsss-private-ips).

### <a name="can-i-use-scale-sets-with-accelerated-networking"></a>Kan ik schaalsets met versnelde toegang gebruiken?

Ja. Voor het gebruik van versnelde netwerken instellingen enableAcceleratedNetworking op true in de schaal van set networkInterfaceConfigurations. Bijvoorbeeld
```json
"networkProfile": {
    "networkInterfaceConfigurations": [
    {
        "name": "niconfig1",
        "properties": {
        "primary": true,
        "enableAcceleratedNetworking" : true,
        "ipConfigurations": [
                ]
            }
            }
        ]
        }
    }
    ]
}
```

### <a name="how-can-i-configure-the-dns-servers-used-by-a-scale-set"></a>Hoe kan ik de DNS-servers die worden gebruikt door een schaalset configureren?

Toevoegen voor het maken van een virtuele-machineschaalset ingesteld met een aangepaste DNS-configuratie, een dnsSettings JSON-pakket naar de sectie scale set networkInterfaceConfigurations. Voorbeeld:
```json
    "dnsSettings":{
        "dnsServers":["10.0.0.6", "10.0.0.5"]
    }
```

### <a name="how-can-i-configure-a-scale-set-to-assign-a-public-ip-address-to-each-vm"></a>Hoe kan ik een schaal ingesteld op een openbaar IP-adres toewijzen aan elke virtuele machine configureren?

Zorg ervoor dat de API-versie van de resource Microsoft.Compute/virtualMAchineScaleSets 2017-03-30 voor het maken van een virtuele-machineschaalset waarmee een openbaar IP-adres worden toegewezen aan elke VM en voeg toe een _publicipaddressconfiguration_ JSON pakket met de schaal ipConfigurations sectie ingesteld. Voorbeeld:

```json
    "publicipaddressconfiguration": {
        "name": "pub1",
        "properties": {
        "idleTimeoutInMinutes": 15
        }
    }
```

### <a name="can-i-configure-a-scale-set-to-work-with-multiple-application-gateways"></a>Kan ik een ingesteld op het werken met meerdere Toepassingsgateways schaal configureren?

Ja. U kunt de resource-id's voor meerdere Application Gateway back-end-adresgroepen te toevoegen de _applicationGatewayBackendAddressPools_ lijst de _ipConfigurations_ sectie van de schaal netwerk instellen profiel.

## <a name="scale"></a>Schalen

### <a name="in-what-case-would-i-create-a-virtual-machine-scale-set-with-fewer-than-two-vms"></a>In welk geval zou ik een virtuele-machineschaalset ingesteld met minder dan twee virtuele machines maken?

Een reden voor het maken van een virtuele-machineschaalset ingesteld met minder dan twee virtuele machines kan worden gebruikt van de elastische eigenschappen van een virtuele-machineschaalset. U kunt bijvoorbeeld een virtuele-machineschaalset ingesteld met nul VM's voor het definiëren van uw infrastructuur zonder te betalen VM gebruikskosten implementeren. Vervolgens, wanneer u klaar om te implementeren van virtuele machines bent, verhoogt de 'capaciteit' van de schaal van de virtuele machine ingesteld op de productie-exemplaren.

Een andere reden dat u een virtuele-machineschaalset ingesteld met minder dan twee virtuele machines kunt maken, is als u zich zorgen over minder dan de beschikbaarheid in met behulp van een beschikbaarheidsset met discrete virtuele machines. Virtuele-machineschaalsets bieden u een manier om te werken met niet gedifferentieerde compute-eenheden die vervangbare zijn. Deze uniformiteit is een belangrijke onderscheid voor virtuele-machineschaalsets versus beschikbaarheidssets. Veel staatloze werkbelastingen volgen afzonderlijke eenheden niet. Als de werkbelasting komt, kunt u omlaag schalen naar een compute-eenheid en vervolgens opschalen op-veel wanneer de werkbelasting toeneemt.

### <a name="how-do-i-change-the-number-of-vms-in-a-virtual-machine-scale-set"></a>Hoe kan ik het aantal virtuele machines in een virtuele-machineschaalset wijzigen?

Als u wilt wijzigen van het aantal virtuele machines in een virtuele-machineschaalset ingesteld in de Azure portal, van de virtuele-machineschaalset instellen sectie met eigenschappen, klikt u op de blade 'Schalen' en gebruik de schuifregelaar. Zie voor andere manieren om te wijzigen van het aantal exemplaren [wijzigen van het aantal exemplaren van een virtuele-machineschaalset](https://msftstack.wordpress.com/2016/05/13/change-the-instance-count-of-an-azure-vm-scale-set/).

### <a name="how-do-i-define-custom-alerts-for-when-certain-thresholds-are-reached"></a>Hoe ik aangepaste waarschuwingen voor als bepaalde drempelwaarden zijn bereikt definiëren?

Hebt u enige flexibiliteit in hoe u waarschuwingen voor de opgegeven drempelwaarden verwerken. U kunt bijvoorbeeld aangepaste webhooks definiëren. Het volgende voorbeeld van de webhook is van een Resource Manager-sjabloon:

```json
{
    "type": "Microsoft.Insights/autoscaleSettings",
    "apiVersion": "[variables('insightsApi')]",
    "name": "autoscale",
    "location": "[parameters('resourceLocation')]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]"
    ],
    "properties": {
        "name": "autoscale",
        "targetResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/',  resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
        "enabled": true,
        "notifications": [
            {
                "operation": "Scale",
                "email": {
                    "sendToSubscriptionAdministrator": true,
                    "sendToSubscriptionCoAdministrators": true,
                    "customEmails": [
                        "youremail@address.com"
                    ]
                },
                "webhooks": [
                    {
                        "serviceUri": "https://events.pagerduty.com/integration/0b75b57246814149b4d87fa6e1273687/enqueue",
                        "properties": {
                            "key1": "custommetric",
                            "key2": "scalevmss"
                        }
                    }
                ]
            }
        ],
```

In dit voorbeeld wordt gaat een waarschuwing naar Pagerduty.com wanneer een drempelwaarde is bereikt.



## <a name="patching-and-operations"></a>Patching en bewerkingen

### <a name="how-do-i-create-a-scale-set-in-an-existing-resource-group"></a>Hoe maak ik een schaal instellen in een bestaande resourcegroep

Schaalsets maken in een bestaande resource groep is nog niet mogelijk via de Azure portal, maar u kunt een bestaande resourcegroep opgeven bij het implementeren van een schaal van een Azure Resource Manager-sjabloon instellen. U kunt ook een bestaande resourcegroep opgeven bij het maken van een schaal ingesteld met Azure PowerShell of CLI.

### <a name="can-we-move-a-scale-set-to-another-resource-group"></a>Kan er een schaalwaarde ingesteld op een andere resourcegroep verplaatsen?

Ja, u kunt schaalset resources aan een nieuw abonnement of resourcegroep verplaatsen.

### <a name="how-to-i-update-my-virtual-machine-scale-set-to-a-new-image-how-do-i-manage-patching"></a>Hoe bijwerken ik voor mijn virtuele-machineschaalset ingesteld op een nieuwe installatiekopie? Hoe u beheer patchen?

De schaal van uw virtuele machine ingesteld op een nieuwe installatiekopie en patchen beheren, Zie [upgraden van een virtuele-machineschaalset](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-upgrade-scale-set).

### <a name="can-i-use-the-reimage-operation-to-reset-a-vm-without-changing-the-image-that-is-i-want-reset-a-vm-to-factory-settings-rather-than-to-a-new-image"></a>Kan ik het terugzetten van de installatiekopie opnieuw gebruiken om in te stellen van een virtuele machine zonder de afbeelding te wijzigen? (Dat wil zeggen, ik wil een virtuele machine opnieuw instellen naar de fabrieksinstellingen in plaats van een nieuwe installatiekopie.)

Ja, kunt u het terugzetten van de installatiekopie opnieuw in te stellen van een virtuele machine zonder de afbeelding te wijzigen. Echter, als uw virtuele-machineschaalset verwijst naar een platforminstallatiekopie met `version = latest`, uw virtuele machine kunt bijwerken naar een hoger installatiekopie van het besturingssysteem bij het aanroepen van `reimage`.

Zie voor meer informatie [beheren van alle VM's in een virtuele-machineschaalset](https://docs.microsoft.com/rest/api/virtualmachinescalesets/manage-all-vms-in-a-set).

### <a name="is-it-possible-to-integrate-scale-sets-with-azure-oms-operations-management-suite"></a>Is het mogelijk schaalsets integreren met Azure OMS (Operations Management Suite)?

Ja, kunt u door het installeren van de OMS-extensie op schaal instellen virtuele machines. Hier volgt een voorbeeld van Azure CLI:
```
az vmss extension set --name MicrosoftMonitoringAgent --publisher Microsoft.EnterpriseCloud.Monitoring --resource-group Team-03 --vmss-name nt01 --settings "{'workspaceId': '<your workspace ID here>'}" --protected-settings "{'workspaceKey': '<your workspace key here'}"
```
U vindt de vereiste workspaceId en workspaceKey in de OMS-portal. Klik op de tegel instellingen op de pagina overzicht. Klik op het tabblad bronnen verbonden aan de bovenkant.

Opmerking: als uw schaalset _upgradePolicy_ is ingesteld op handmatig, moet u de uitbreiding voor de alle VM's in de set toegepast door het aanroepen van upgrade op deze. In de CLI zou dit _az vmss update-exemplaren_.

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="how-do-i-turn-on-boot-diagnostics"></a>Hoe schakel ik op diagnostische gegevens over opstarten?

Om in te schakelen diagnostische gegevens over opstarten, moet u eerst een opslagaccount maken. Zet dit JSON-blok in uw virtuele-machineschaalset **virtualMachineProfile**, en de virtuele-machineschaalset bijwerken:

```json
"diagnosticsProfile": {
    "bootDiagnostics": {
        "enabled": true,
        "storageUri": "http://yourstorageaccount.blob.core.windows.net"
    }
}
```

Wanneer een nieuwe virtuele machine wordt gemaakt, ziet u de eigenschap InstanceView van de virtuele machine de details voor de schermafbeelding, enzovoort. Hier volgt een voorbeeld:
 
```json
"bootDiagnostics": {
    "consoleScreenshotBlobUri": "https://o0sz3nhtbmkg6geswarm5.blob.core.windows.net/bootdiagnostics-swarmagen-4157d838-8335-4f78-bf0e-b616a99bc8bd/swarm-agent-9574AE92vmss-0_2.4157d838-8335-4f78-bf0e-b616a99bc8bd.screenshot.bmp",
    "serialConsoleLogBlobUri": "https://o0sz3nhtbmkg6geswarm5.blob.core.windows.net/bootdiagnostics-swarmagen-4157d838-8335-4f78-bf0e-b616a99bc8bd/swarm-agent-9574AE92vmss-0_2.4157d838-8335-4f78-bf0e-b616a99bc8bd.serialconsole.log"
  }
```


## <a name="virtual-machine-properties"></a>Eigenschappen van virtuele machine

### <a name="how-do-i-get-property-information-for-each-vm-without-making-multiple-calls-for-example-how-would-i-get-the-fault-domain-for-each-of-the-100-vms-in-my-virtual-machine-scale-set"></a>Hoe krijg eigenschapsinformatie voor elke virtuele machine zonder dat meerdere aanroepen? Bijvoorbeeld, hoe ik krijgt het foutdomein voor elk van de 100 virtuele machines in mijn virtuele-machineschaalset?

Als u eigenschapsinformatie voor elke virtuele machine zonder dat meerdere aanroepen, u kunt aanroepen `ListVMInstanceViews` als volgt een REST-API `GET` op de volgende bron-URI:

/Subscriptions/ < subscription_id > /resourceGroups/ < resource_group_name > /providers/Microsoft.Compute/virtualMachineScaleSets/ < scaleset_name > / virtuele machines? $expand = instanceView & $select = instanceView

### <a name="can-i-pass-different-extension-arguments-to-different-vms-in-a-virtual-machine-scale-set"></a>Kan ik andere uitbreiding argumenten doorgeven aan andere virtuele machines in een virtuele-machineschaalset

Nee, u kunt andere uitbreiding argumenten niet doorgeven voor andere VM's in een virtuele-machineschaalset. Extensies kunnen echter op basis van de unieke eigenschappen van de virtuele machine die ze worden uitgevoerd, bijvoorbeeld als u op de naam van de machine fungeren. Extensies kunnen ook een query metagegevens van het exemplaar op http://169.254.169.254 voor meer informatie over de VM.

### <a name="why-are-there-gaps-between-my-virtual-machine-scale-set-vm-machine-names-and-vm-ids-for-example-0-1-3"></a>Waarom zijn er onderbrekingen tussen mijn namen voor machines van virtuele machine scale set VM en VM-id's? Bijvoorbeeld: 0, 1, 3...

Er onderbrekingen tussen de namen voor machines van virtuele machine scale set VM en VM-id's zijn omdat uw virtuele-machineschaalset **overprovision** eigenschap is ingesteld op de standaardwaarde van **true**. Als overmatige inrichting is ingesteld op **true**, meer virtuele machines dan aangevraagd worden gemaakt. Extra virtuele machines vervolgens verwijderd. In dit geval u krijgen hogere implementatie betrouwbaarheid, maar regels ten koste van de naam van het aaneengesloten en aaneengesloten Network Address Translation (NAT). 

U kunt deze eigenschap instellen op **false**. Voor kleine virtuele machine-schaalsets beïnvloeden niet dit betrouwbaarheid van de implementatie aanzienlijk.

### <a name="what-is-the-difference-between-deleting-a-vm-in-a-virtual-machine-scale-set-and-deallocating-the-vm-when-should-i-choose-one-over-the-other"></a>Wat is het verschil tussen een virtuele machine in een virtuele-machineschaalset verwijderen en toewijzing van de virtuele machine? Wanneer moet ik een dan de andere kiezen?

Het belangrijkste verschil tussen een virtuele machine in een virtuele-machineschaalset verwijderen en toewijzing van de virtuele machine is die `deallocate` niet verwijderd van de virtuele harde schijven (VHD's). Er zijn kosten voor opslag die is gekoppeld aan die wordt uitgevoerd `stop deallocate`. U kunt een of andere gebruiken voor een van de volgende redenen:

- U wilt stoppen compute-kosten betaalt, maar u wilt behouden van de status van de virtuele machines.
- U wilt sneller dan u tot een virtuele-machineschaalset uitbreiden kan starten van een set van virtuele machines.
  - Betrekking heeft op dit scenario, u mogelijk hebt gemaakt uw eigen engine voor het automatisch schalen en wilt dat een snellere end-to-end-schaal.
- U hebt een virtuele-machineschaalset die ongelijkmatig verdeeld is over domeinen met fouten of update-domeinen. Dit wordt mogelijk omdat u selectief virtuele machines verwijderd, of omdat de virtuele machines zijn verwijderd na overmatige inrichting. Met `stop deallocate` gevolgd door `start` op de virtuele machine schaal instelt gelijkmatig distributie van de virtuele machines over de domeinen met fouten of update-domeinen.

