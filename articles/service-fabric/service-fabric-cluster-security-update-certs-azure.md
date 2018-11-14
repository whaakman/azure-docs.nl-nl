---
title: Beheren van certificaten in een Azure Service Fabric-cluster | Microsoft Docs
description: Beschrijft hoe u nieuwe certificaten, rollovercertificaat, toevoegen en verwijderen van certificaat naar of van een Service Fabric-cluster.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: ''
ms.assetid: 91adc3d3-a4ca-46cf-ac5f-368fb6458d74
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/13/2018
ms.author: aljo-microsoft
ms.openlocfilehash: cca48e78278cf234d6f6062eadac4f85ecbcb79b
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51625926"
---
# <a name="add-or-remove-certificates-for-a-service-fabric-cluster-in-azure"></a>Toevoegen of verwijderen van certificaten voor een Service Fabric-cluster in Azure
Het verdient aanbeveling dat u raken met het Service Fabric maakt gebruik van x.509-certificaten en vertrouwd met zijn de [clusterbeveiligingsscenario's](service-fabric-cluster-security.md). U moet begrijpen wat een clustercertificaat is en wat wordt gebruikt voor, voordat u verder gaat.

Azure Service Fabrics SDK's standaard certificaat load gedrag, is het implementeren en gebruiken van een gedefinieerde certificaat met een vervaldatum verste weg in de toekomst; ongeacht de definitie van de primaire of secundaire configuratie. Terugvallen naar de klassieke gedrag wordt niet aanbevolen geavanceerde actie, en is vereist als de waarde van de parameter 'UseSecondaryIfNever' instellen op false in de configuratie van uw Fabric.Code.

Service fabric kunt u twee clustercertificaten, een primaire en een secundaire opgeven bij het configureren van Certificaatbeveiliging tijdens het maken van een cluster, naast de clientcertificaten. Raadpleeg [het maken van een azure-cluster via de portal](service-fabric-cluster-creation-via-portal.md) of [het maken van een azure-cluster via Azure Resource Manager](service-fabric-cluster-creation-via-arm.md) voor meer informatie over het instellen van deze clusters op voor het maken. Als u slechts één clustercertificaat opgeeft bij het maken, klikt u vervolgens die wordt gebruikt als het primaire certificaat voor. Na het maken van een cluster, kunt u een nieuw certificaat toevoegen als een secundaire.

> [!NOTE]
> Voor een beveiligd cluster moet altijd u ten minste één geldige (niet ingetrokken en niet verlopen)-cluster certificaat (primaire of secundaire) dat is geïmplementeerd (zo niet, de cluster-stopt werkt). 90 dagen voordat alle geldige certificaten verlopen, bereikt het systeem genereert een waarschuwing tracering en ook een statusgebeurtenis waarschuwing op het knooppunt. Er is momenteel geen e-mailadres of andere berichten die in dit artikel Service Fabric verzendt. 
> 
> 

## <a name="add-a-secondary-cluster-certificate-using-the-portal"></a>Toevoegen van een secundair clustercertificaat met behulp van de portal
Secundair clustercertificaat kan niet worden toegevoegd via Azure portal, Azure powershell gebruiken. Het proces wordt verderop in dit document beschreven.

## <a name="remove-a-cluster-certificate-using-the-portal"></a>Verwijderen van een clustercertificaat met behulp van de portal
Voor een beveiligd cluster moet u altijd ten minste één geldig (niet ingetrokken en niet verlopen)-certificaat. Het certificaat dat is geïmplementeerd met verst in de verlopende datum in de toekomst worden gebruikt en verwijdert het zorgt ervoor dat de werking van uw cluster stoppen; Zorg ervoor dat alleen verwijderen van het certificaat is verlopen of een niet-gebruikte certificaat dat de eerste aflopen verloopt.

Een niet-gebruikte cluster security-certificaat, navigeer naar de sectie beveiliging verwijderen en selecteer de optie 'Delete' in het contextmenu op het ongebruikte certificaat.

Als uw bedoeling is om te verwijderen van het certificaat dat is gemarkeerd als primair, klikt u vervolgens moet u voor het implementeren van een secundair certificaat met een vervaldatum verder in de toekomst dan het primaire certificaat voor het inschakelen van het gedrag van automatische rollover; het primaire certificaat verwijderen nadat de automatische rollover is voltooid.

## <a name="add-a-secondary-certificate-using-resource-manager-powershell"></a>Een secundair certificaat met behulp van Resource Manager Powershell toevoegen
> [!TIP]
> Het is nu beter en eenvoudiger manier om toe te voegen een secundair certificaat met de [Add-AzureRmServiceFabricClusterCertificate](/powershell/module/azurerm.servicefabric/add-azurermservicefabricclustercertificate) cmdlet. U hoeft niet te volgen van de rest van de stappen in deze sectie.  Bovendien hoeft u niet de sjabloon die oorspronkelijk hebt gebruikt om te maken en implementeren van het cluster bij het gebruik van de [Add-AzureRmServiceFabricClusterCertificate](/powershell/module/azurerm.servicefabric/add-azurermservicefabricclustercertificate) cmdlet.

Deze stappen wordt ervan uitgegaan dat u bekend bent met de werking van Resource Manager en ten minste één Service Fabric-cluster met behulp van Resource Manager-sjabloon hebt geïmplementeerd en de sjabloon die u hebt gebruikt voor het instellen van het cluster bij de hand hebt. Ook wordt ervan uitgegaan dat u ervaring hebt met JSON zijn.

> [!NOTE]
> Als u een voorbeeldsjabloon en parameters die u gebruiken kunt om te volgen langs of als een beginpunt zoekt, klikt u vervolgens downloaden van dit [git-opslagplaats](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample). 
> 
> 

### <a name="edit-your-resource-manager-template"></a>Uw Resource Manager-sjabloon bewerken

Voorbeeld 5-VM-1-NodeTypes-Secure_Step2.JSON bevat gemakkelijker te maken van de volgende langs alle bewerkingen we brengen. het voorbeeld is beschikbaar op [git-opslagplaats](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample).

**Zorg ervoor dat u alle stappen**

1. Open de Resource Manager-sjabloon waarmee u zich hebt u een Cluster implementeren. (Als u het voorbeeld van de voorgaande opslagplaats hebt gedownload, 5-VM-1-NodeTypes-Secure_Step1.JSON vervolgens gebruiken om een beveiligd cluster te implementeren en vervolgens opent u die sjabloon).

2. Voeg **twee nieuwe parameters** "secCertificateThumbprint" en "secCertificateUrlValue" van typt u 'tekenreeks' aan de sectie van uw sjabloon. U kunt het volgende codefragment kopiëren en deze toevoegen aan de sjabloon. Afhankelijk van de bron van de sjabloon, mogelijk hebt u al deze gedefinieerd, als u dus verplaatsen naar de volgende stap. 
 
    ```json
       "secCertificateThumbprint": {
          "type": "string",
          "metadata": {
            "description": "Certificate Thumbprint"
          }
        },
        "secCertificateUrlValue": {
          "type": "string",
          "metadata": {
            "description": "Refers to the location URL in your key vault where the certificate was uploaded, it is should be in the format of https://<name of the vault>.vault.azure.net:443/secrets/<exact location>"
          }
        },
    
    ```

3. Wijzigingen aanbrengen in de **Microsoft.ServiceFabric/clusters** resource - de resourcedefinitie 'Microsoft.ServiceFabric/clusters' niet vinden in de sjabloon. Onder de eigenschappen van deze definitie, vindt u "Certificate" JSON code, waarmee er ongeveer als de volgende JSON-fragment ziet:
   
    ```JSON
          "properties": {
            "certificate": {
              "thumbprint": "[parameters('certificateThumbprint')]",
              "x509StoreName": "[parameters('certificateStoreValue')]"
         }
    ``` 

    Toevoegen van een nieuwe tag "thumbprintSecondary" en wijs hieraan een waarde '[parameters('secCertificateThumbprint')]'.  

    Nu de resourcedefinitie moet eruitzien als de volgende (afhankelijk van de bron van de sjabloon, mogelijk niet precies zoals het onderstaande codefragment). 

    ```JSON
          "properties": {
            "certificate": {
              "thumbprint": "[parameters('certificateThumbprint')]",
              "thumbprintSecondary": "[parameters('secCertificateThumbprint')]",
              "x509StoreName": "[parameters('certificateStoreValue')]"
         }
    ``` 

    Als u wilt **rollover van het certificaat**, geef het nieuwe certificaat als primaire en het verplaatsen van de huidige primaire als secundaire. Dit resulteert in de rollover van uw huidige primaire certificaat voor het nieuwe certificaat in één implementatiestap.
    
    ```JSON
          "properties": {
            "certificate": {
              "thumbprint": "[parameters('secCertificateThumbprint')]",
              "thumbprintSecondary": "[parameters('certificateThumbprint')]",
              "x509StoreName": "[parameters('certificateStoreValue')]"
         }
    ``` 

4. Wijzigingen aanbrengen in **alle** de **Microsoft.Compute/virtualMachineScaleSets** resourcedefinities - Zoek de definitie van de resource Microsoft.Compute/virtualMachineScaleSets. Blader naar het 'publisher': 'Microsoft.Azure.ServiceFabric' onder 'virtualMachineProfile'.

    In de instellingen van de uitgever van Service Fabric ziet u er ongeveer als volgt.
    
    ![Json_Pub_Setting1][Json_Pub_Setting1]
    
    Het nieuwe certificaat vermeldingen toevoegen aan het
    
    ```json
                   "certificateSecondary": {
                        "thumbprint": "[parameters('secCertificateThumbprint')]",
                        "x509StoreName": "[parameters('certificateStoreValue')]"
                        }
                      },
    
    ```

    De eigenschappen moeten er nu als volgt uitzien
    
    ![Json_Pub_Setting2][Json_Pub_Setting2]
    
    Als u wilt **rollover van het certificaat**, geef het nieuwe certificaat als primaire en het verplaatsen van de huidige primaire als secundaire. Dit resulteert in de rollover van uw huidige certificaat naar het nieuwe certificaat in één implementatiestap.     

    ```json
                   "certificate": {
                       "thumbprint": "[parameters('secCertificateThumbprint')]",
                       "x509StoreName": "[parameters('certificateStoreValue')]"
                         },
                   "certificateSecondary": {
                        "thumbprint": "[parameters('certificateThumbprint')]",
                        "x509StoreName": "[parameters('certificateStoreValue')]"
                        }
                      },
    ```

    De eigenschappen moeten er nu als volgt uitzien    
    ![Json_Pub_Setting3][Json_Pub_Setting3]

5. Wijzigingen aanbrengen in **alle** de **Microsoft.Compute/virtualMachineScaleSets** resourcedefinities - Zoek de definitie van de resource Microsoft.Compute/virtualMachineScaleSets. Blader naar het 'vaultCertificates':, onder 'OSProfile'. Dit ziet er ongeveer als volgt.

    ![Json_Pub_Setting4][Json_Pub_Setting4]
    
    De secCertificateUrlValue aan toevoegen. Gebruik het volgende codefragment:
    
    ```json
                      {
                        "certificateStore": "[parameters('certificateStoreValue')]",
                        "certificateUrl": "[parameters('secCertificateUrlValue')]"
                      }
    
    ```
    De resulterende Json moet nu ongeveer als volgt uitzien.
    ![Json_Pub_Setting5][Json_Pub_Setting5]


> [!NOTE]
> Zorg ervoor dat u stap 4 en 5 voor de definities voor de resource Nodetypes/Microsoft.Compute/virtualMachineScaleSets hebben herhaald in de sjabloon. Als u een van beide missen, wordt het certificaat niet geïnstalleerd op dat de virtuele-machineschaalset en u onvoorspelbare resultaten in het cluster hebt, met inbegrip van het cluster uitvalt (als u uiteindelijk geen geldige certificaten die het cluster voor beveiliging gebruiken kunt. Zo Controleer, voordat u doorgaat.
> 
> 

### <a name="edit-your-template-file-to-reflect-the-new-parameters-you-added-above"></a>Bewerk het sjabloonbestand aanleiding van de nieuwe parameters die u hebt toegevoegd
Als u het voorbeeld van de [git-opslagplaats](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample) wilt volgen, kunt u beginnen met het aanbrengen van wijzigingen in het voorbeeld 5-VM-1-NodeTypes-Secure.parameters_Step2.JSON 

De parameter Resource Manager-sjabloon in het bestand bewerken, de twee nieuwe parameters voor secCertificateThumbprint en secCertificateUrlValue toevoegen. 

```JSON
    "secCertificateThumbprint": {
      "value": "thumbprint value"
    },
    "secCertificateUrlValue": {
      "value": "Refers to the location URL in your key vault where the certificate was uploaded, it is should be in the format of https://<name of the vault>.vault.azure.net:443/secrets/<exact location>"
     },

```

### <a name="deploy-the-template-to-azure"></a>De sjabloon implementeren in Azure

- U bent nu klaar voor het implementeren van uw sjabloon naar Azure. Open de opdrachtprompt van een Azure-PS-versie 1 +.
- Meld u aan bij uw Azure-Account en selecteer de specifieke azure-abonnement. Dit is een belangrijke stap voor mensen die toegang tot meer dan één azure-abonnement hebben.

```powershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId <Subcription ID> 

```

Test de sjabloon voordat deze is geïmplementeerd. Gebruik dezelfde resourcegroep die uw cluster wordt momenteel geïmplementeerd in.

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName <Resource Group that your cluster is currently deployed to> -TemplateFile <PathToTemplate>

```

De sjabloon implementeren in uw resourcegroep. Gebruik dezelfde resourcegroep die uw cluster wordt momenteel geïmplementeerd in. Voer de opdracht New-AzureRmResourceGroupDeployment. U hoeft niet om op te geven van de modus, omdat de standaardwaarde is **incrementele**.

> [!NOTE]
> Als u de modus op voltooid instellen, kunt u per ongeluk resources die zich niet in uw sjabloon verwijderen. Dus gebruik deze niet in dit scenario.
> 
> 

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName <Resource Group that your cluster is currently deployed to> -TemplateFile <PathToTemplate>
```

Hier volgt een voorbeeld van een ingevuld van de dezelfde powershell.

```powershell
$ResourceGroup2 = "chackosecure5"
$TemplateFile = "C:\GitHub\Service-Fabric\ARM Templates\Cert Rollover Sample\5-VM-1-NodeTypes-Secure_Step2.json"
$TemplateParmFile = "C:\GitHub\Service-Fabric\ARM Templates\Cert Rollover Sample\5-VM-1-NodeTypes-Secure.parameters_Step2.json"

New-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroup2 -TemplateParameterFile $TemplateParmFile -TemplateUri $TemplateFile -clusterName $ResourceGroup2

```

Zodra de implementatie voltooid is, verbinding maken met uw cluster met behulp van het nieuwe certificaat en enkele query's uit te voeren. Als u kunnen doen. Vervolgens kunt u het oude certificaat verwijderen. 

Als u een zelfondertekend certificaat gebruikt, vergeet niet om te importeren in uw lokale TrustedPeople-certificaatarchief.

```powershell
######## Set up the certs on your local box
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\TrustedPeople -FilePath c:\Mycertificates\chackdanTestCertificate9.pfx -Password (ConvertTo-SecureString -String abcd123 -AsPlainText -Force)
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My -FilePath c:\Mycertificates\chackdanTestCertificate9.pfx -Password (ConvertTo-SecureString -String abcd123 -AsPlainText -Force)

```
Voor snelle naslag volgt de opdracht voor verbinding met een beveiligd cluster 

```powershell
$ClusterName= "chackosecure5.westus.cloudapp.azure.com:19000"
$CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7SD1D630F8F3" 

Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
    -X509Credential `
    -ServerCertThumbprint $CertThumbprint  `
    -FindType FindByThumbprint `
    -FindValue $CertThumbprint `
    -StoreLocation CurrentUser `
    -StoreName My
```
Voor snelle naslag volgt de opdracht voor het ophalen van de clusterstatus

```powershell
Get-ServiceFabricClusterHealth 
```

## <a name="deploying-application-certificates-to-the-cluster"></a>Certificaten van de toepassing implementeren naar het cluster.

U kunt dezelfde stappen gebruiken zoals wordt beschreven in de voorgaande stap 5 om de certificaten die zijn geïmplementeerd in een Key Vault op de knooppunten. U moet alleen definiëren en gebruiken van verschillende parameters.


## <a name="adding-or-removing-client-certificates"></a>Toevoegen of verwijderen van clientcertificaten

Naast de clustercertificaten, kunt u clientcertificaten voor het uitvoeren van beheerbewerkingen op een Service Fabric-cluster toevoegen.

U kunt twee soorten clientcertificaten - beheerder toevoegen of alleen-lezen. Deze kunnen vervolgens worden gebruikt voor het beheren van toegang tot de admin operations- en querybewerkingen op het cluster. Standaard worden de clustercertificaten toegevoegd aan de lijst met toegestane certificaten beheerder.

u kunt een willekeurig aantal clientcertificaten opgeven. Elke toevoeging/verwijdering resulteert in een configuratie-update in de Service Fabric-cluster


### <a name="adding-client-certificates---admin-or-read-only-via-portal"></a>Clientcertificaten - beheerder toe te voegen of alleen-lezen via de portal

1. Navigeer naar de sectie Beveiliging en selecteer de + verificatie knop boven op de Beveiligingssectie.
2. Kies het 'verificatietype' - 'Alleen-lezen-client' of 'Admin client' in de sectie 'Verificatie toevoegen'
3. Nu voor kiezen de autorisatiemethode. Hiermee geeft u aan Service Fabric of het opzoeken van dit certificaat met behulp van de onderwerpnaam of de vingerafdruk. Het is in het algemeen niet een goede gewoonte om de autorisatiemethode van de naam van het onderwerp te gebruiken. 

![Clientcertificaat toevoegen][Add_Client_Cert]

### <a name="deletion-of-client-certificates---admin-or-read-only-using-the-portal"></a>Het verwijderen van clientcertificaten - beheerder of alleen-lezen met behulp van de portal

Verwijderen van een secundair certificaat worden gebruikt voor clusterbeveiliging, navigeer naar de sectie Beveiliging en de optie 'Verwijderen' selecteert in het contextmenu op de specifieke certificaat.

## <a name="next-steps"></a>Volgende stappen
Lees deze artikelen voor meer informatie over het Clusterbeheer:

* [Het upgradeproces service Fabric-Cluster en de verwachtingen van u](service-fabric-cluster-upgrade.md)
* [Toegang op basis van rollen voor clients instellen](service-fabric-cluster-security-roles.md)

<!--Image references-->
[Add_Client_Cert]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_13.PNG
[Json_Pub_Setting1]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_14.PNG
[Json_Pub_Setting2]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_15.PNG
[Json_Pub_Setting3]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_16.PNG
[Json_Pub_Setting4]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_17.PNG
[Json_Pub_Setting5]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_18.PNG


