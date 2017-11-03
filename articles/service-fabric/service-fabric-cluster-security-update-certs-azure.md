---
title: Het beheren van certificaten in een Azure Service Fabric-cluster | Microsoft Docs
description: Beschrijft hoe nieuwe certificaten, rollovercertificaat, toevoegen en verwijderen van certificaat naar of van een Service Fabric-cluster.
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 91adc3d3-a4ca-46cf-ac5f-368fb6458d74
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/09/2017
ms.author: chackdan
ms.openlocfilehash: c433e8683755e454f9561f094269c3daccf78a62
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="add-or-remove-certificates-for-a-service-fabric-cluster-in-azure"></a>Toevoegen of verwijderen van certificaten voor een Service Fabric-cluster in Azure
Het is raadzaam dat u raken met het Service Fabric X.509-certificaten gebruikt en vertrouwd met zijn de [security scenario's](service-fabric-cluster-security.md). U moet begrijpen wat een certificaat in het cluster is en wat wordt gebruikt voor, voordat u verder.

Het service fabric kunt u twee clustercertificaten, een primaire en een secundaire opgeven bij het configureren van Certificaatbeveiliging tijdens het maken van het cluster naast clientcertificaten. Raadpleeg [maken van een azure-cluster via de portal](service-fabric-cluster-creation-via-portal.md) of [maken van een azure-cluster via Azure Resource Manager](service-fabric-cluster-creation-via-arm.md) voor meer informatie over het instellen op voor het maken. Als u slechts één cluster certificaat opgeeft op voor het maken, klikt u vervolgens die wordt gebruikt als het primaire certificaat. U kunt een nieuw certificaat na het maken van het cluster toevoegen als een secundaire database.

> [!NOTE]
> Voor een beveiligde cluster moet altijd u ten minste één geldig (niet ingetrokken en niet verlopen)-cluster certificaat (primair of secundair) dat is geïmplementeerd (zo niet, de cluster stopt werkt). 90 dagen voordat alle geldige certificaten verlopen, bereikt het systeem genereert een waarschuwing trace en ook een waarschuwingsgebeurtenis status op het knooppunt. Er is momenteel geen e-mail of andere berichten naar service fabric worden verzonden over dit onderwerp. 
> 
> 

## <a name="add-a-secondary-cluster-certificate-using-the-portal"></a>Een secundaire cluster certificaat via de portal toevoegen

Secundaire cluster certificaat kan niet worden toegevoegd via de Azure portal. U moet Azure powershell gebruiken voor die. Het proces wordt verderop in dit document beschreven.

## <a name="swap-the-cluster-certificates-using-the-portal"></a>Wisselen van de clustercertificaten met behulp van de portal

Nadat u hebt een certificaat secundaire cluster is geïmplementeerd als u wilt wisselen van de primaire en secundaire, Ga naar de blade beveiliging en selecteer de optie 'Wisseling met primaire' in het contextmenu wisselen van de secundaire cert met het primaire certificaat.

![Swap-certificaat][Delete_Swap_Cert]

## <a name="remove-a-cluster-certificate-using-the-portal"></a>Het certificaat voor een cluster met behulp van de portal verwijderen

Voor een beveiligde cluster, moet u altijd ten minste één geldig (niet ingetrokken en niet verlopen)-certificaat (primair of secundair) geïmplementeerd zo niet, het cluster niet meer werkt.

Een secundair certificaat worden gebruikt voor een clusterbeveiliging navigeren naar de blade beveiliging verwijderen en selecteer de optie 'Verwijderen' in het contextmenu op de secundaire certificaat.

Als uw bedoeling is om het certificaat dat is gemarkeerd als primaire verwijderen, wordt moet u eerst met de secundaire uitwisselen, en verwijder vervolgens de secundaire nadat de upgrade is voltooid.

## <a name="add-a-secondary-certificate-using-resource-manager-powershell"></a>Toevoegen van een secundair certificaat met behulp van Resource Manager Powershell

Deze stappen wordt ervan uitgegaan dat bekend bent met de werking van Resource Manager en moet ten minste één Service Fabric-cluster met een Resource Manager-sjabloon hebt geïmplementeerd en de sjabloon die u gebruikt voor het instellen van het cluster bij de hand hebt. Ook wordt ervan uitgegaan dat u vertrouwd met een JSON bent.

> [!NOTE]
> Als u zoekt naar een voorbeeldsjabloon en parameters die u kunt volgen langs of als uitgangspunt, klikt u vervolgens het downloaden van deze [git-opslagplaats](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample). 
> 
> 

### <a name="edit-your-resource-manager-template"></a>De Resource Manager-sjabloon bewerken

Voorbeeld 5-VM-1-NodeTypes-Secure_Step2.JSON bevat voor een eenvoudige van volgende langs alle bewerkingen we brengen. het voorbeeld is beschikbaar op [git-opslagplaats](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample).

**Zorg ervoor dat u alle stappen**

**Stap 1:** Open de Resource Manager-sjabloon die u hebt gebruikt voor het implementeren van Cluster. (Als u het voorbeeld hebt gedownload uit de bovenstaande opslagplaats, 5-VM-1-NodeTypes-Secure_Step1.JSON vervolgens gebruiken voor het implementeren van een beveiligde cluster en vervolgens opent u dat de sjabloon).

**Stap 2:** toevoegen **twee nieuwe parameters** 'secCertificateThumbprint' en 'secCertificateUrlValue' van type 'tekenreeks' aan de parametersectie van de sjabloon. U kunt het volgende codefragment kopiëren en toevoegen aan de sjabloon. Afhankelijk van de bron van uw sjabloon mogelijk hebt u al deze gedefinieerd als dus verplaatsen naar de volgende stap. 
 
```JSON
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

**Stap 3:** wijzigingen aanbrengen in de **Microsoft.ServiceFabric/clusters** resource - de resourcedefinitie 'Microsoft.ServiceFabric/clusters' niet vinden in de sjabloon. Onder de eigenschappen van deze definitie van vindt u 'Certificaat' JSON tag, die u moet er ongeveer als de volgende JSON-fragment:

   
```JSON
      "properties": {
        "certificate": {
          "thumbprint": "[parameters('certificateThumbprint')]",
          "x509StoreName": "[parameters('certificateStoreValue')]"
     }
``` 

Voeg een nieuwe tag 'thumbprintSecondary' en hieraan een waarde '[parameters('secCertificateThumbprint')]'.  

In dat geval nu de resourcedefinitie als in de volgende eruitzien moet (afhankelijk van de bron van de sjabloon niet mogelijk precies zoals het onderstaande codefragment). 

```JSON
      "properties": {
        "certificate": {
          "thumbprint": "[parameters('certificateThumbprint')]",
          "thumbprintSecondary": "[parameters('secCertificateThumbprint')]",
          "x509StoreName": "[parameters('certificateStoreValue')]"
     }
``` 

Als u wilt **rollover van het certificaat**, geef het nieuwe certificaat als primaire en de huidige primaire als secundaire verplaatsen. Dit resulteert in de overschakeling van uw huidige primaire certificaat voor het nieuwe certificaat in één stap van de implementatie.

```JSON
      "properties": {
        "certificate": {
          "thumbprint": "[parameters('secCertificateThumbprint')]",
          "thumbprintSecondary": "[parameters('certificateThumbprint')]",
          "x509StoreName": "[parameters('certificateStoreValue')]"
     }
``` 


**Stap 4:** wijzigingen aanbrengen in **alle** de **Microsoft.Compute/virtualMachineScaleSets** resourcedefinities - de bron Microsoft.Compute/virtualMachineScaleSets gevonden definitie. Blader naar de 'publisher': 'Microsoft.Azure.ServiceFabric' onder 'virtualMachineProfile'.

In de instellingen voor service fabric publisher ziet u er ongeveer als volgt.

![Json_Pub_Setting1][Json_Pub_Setting1]

Voeg de nieuwe cert vermeldingen toe

```JSON
               "certificateSecondary": {
                    "thumbprint": "[parameters('secCertificateThumbprint')]",
                    "x509StoreName": "[parameters('certificateStoreValue')]"
                    }
                  },

```

De eigenschappen ziet er nu als volgt

![Json_Pub_Setting2][Json_Pub_Setting2]

Als u wilt **rollover van het certificaat**, geef het nieuwe certificaat als primaire en de huidige primaire als secundaire verplaatsen. Dit resulteert in de overschakeling van uw huidige certificaat naar het nieuwe certificaat in één stap van de implementatie. 


```JSON
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
De eigenschappen ziet er nu als volgt

![Json_Pub_Setting3][Json_Pub_Setting3]


**Stap 5:** wijzigingen aanbrengen in **alle** de **Microsoft.Compute/virtualMachineScaleSets** resourcedefinities - de bron Microsoft.Compute/virtualMachineScaleSets gevonden definitie. Blader naar de 'vaultCertificates':, onder 'OSProfile'. Dit ziet er ongeveer als volgt.


![Json_Pub_Setting4][Json_Pub_Setting4]

De secCertificateUrlValue aan toevoegen. Gebruik het volgende fragment:

```Json
                  {
                    "certificateStore": "[parameters('certificateStoreValue')]",
                    "certificateUrl": "[parameters('secCertificateUrlValue')]"
                  }

```
Nu ziet de resulterende Json er ongeveer als volgt.
![Json_Pub_Setting5][Json_Pub_Setting5]


> [!NOTE]
> Zorg ervoor dat u stap 4 en 5 voor de definities voor de resource Nodetypes/Microsoft.Compute/virtualMachineScaleSets hebben herhaald in de sjabloon. Als u een van beide mist, wordt het certificaat niet geïnstalleerd op dat VMSS u onvoorspelbare resultaten in het cluster hebt, met inbegrip van het cluster tijdelijk niet beschikbaar (als u uiteindelijk geen geldige certificaten die de cluster voor beveiliging gebruiken kunt. Zo Controleer, voordat u doorgaat.
> 
> 


### <a name="edit-your-template-file-to-reflect-the-new-parameters-you-added-above"></a>Bewerk het sjabloonbestand zodat de nieuwe parameters die u hebt toegevoegd
Als u het voorbeeld van de [git-opslagplaats](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample) als u wilt volgen, kunt u beginnen met wijzigingen aanbrengen in het voorbeeld 5-VM-1-NodeTypes-Secure.paramters_Step2.JSON 

De parameter-bestand voor uw Resource Manager-sjabloon bewerken, de twee nieuwe parameters voor secCertificateThumbprint en secCertificateUrlValue toevoegen. 

```JSON
    "secCertificateThumbprint": {
      "value": "thumbprint value"
    },
    "secCertificateUrlValue": {
      "value": "Refers to the location URL in your key vault where the certificate was uploaded, it is should be in the format of https://<name of the vault>.vault.azure.net:443/secrets/<exact location>"
     },

```

### <a name="deploy-the-template-to-azure"></a>De sjabloon te implementeren in Azure

- U bent nu klaar voor het implementeren van uw sjabloon naar Azure. Open een opdrachtprompt voor Azure PS-versie 1 +.
- Aanmelden bij uw Azure-Account en selecteer de specifieke azure-abonnement. Dit is een belangrijke stap voor mensen die toegang tot meer dan één azure-abonnement hebben.

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId <Subcription ID> 

```

Test de sjabloon voordat deze is geïmplementeerd. Gebruik dezelfde resourcegroep die uw cluster wordt momenteel geïmplementeerd op.

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName <Resource Group that your cluster is currently deployed to> -TemplateFile <PathToTemplate>

```

Implementeer de sjabloon in de resourcegroep. Gebruik dezelfde resourcegroep die uw cluster wordt momenteel geïmplementeerd op. Voer de opdracht New-AzureRmResourceGroupDeployment. U hoeft niet te geven de modus, aangezien de standaardwaarde is **incrementele**.

> [!NOTE]
> Als u de modus op voltooid instellen, kunt u resources die zich niet in uw sjabloon per ongeluk verwijderen. Dus gebruik deze niet in dit scenario.
> 
> 

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName <Resource Group that your cluster is currently deployed to> -TemplateFile <PathToTemplate>
```

Hier volgt een voorbeeld van een ingevuld van de dezelfde powershell.

```powershell
$ResouceGroup2 = "chackosecure5"
$TemplateFile = "C:\GitHub\Service-Fabric\ARM Templates\Cert Rollover Sample\5-VM-1-NodeTypes-Secure_Step2.json"
$TemplateParmFile = "C:\GitHub\Service-Fabric\ARM Templates\Cert Rollover Sample\5-VM-1-NodeTypes-Secure.parameters_Step2.json"

New-AzureRmResourceGroupDeployment -ResourceGroupName $ResouceGroup2 -TemplateParameterFile $TemplateParmFile -TemplateUri $TemplateFile -clusterName $ResouceGroup2

```

Zodra de implementatie voltooid is, verbinding maken met uw cluster met behulp van het nieuwe certificaat en enkele query's uitvoeren. Als u kunnen doen bent. Vervolgens kunt u het oude certificaat verwijderen. 

Als u een zelfondertekend certificaat gebruikt, vergeet niet om te importeren in uw lokale TrustedPeople-certificaatarchief.

```powershell
######## Set up the certs on your local box
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\TrustedPeople -FilePath c:\Mycertificates\chackdanTestCertificate9.pfx -Password (ConvertTo-SecureString -String abcd123 -AsPlainText -Force)
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My -FilePath c:\Mycertificates\chackdanTestCertificate9.pfx -Password (ConvertTo-SecureString -String abcd123 -AsPlainText -Force)

```
Voor snelle naslag hier is de opdracht verbinding maken met een beveiligde cluster 

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
Voor snelle naslag hier is de opdracht cluster health ophalen

```powershell
Get-ServiceFabricClusterHealth 
```

## <a name="deploying-application-certificates-to-the-cluster"></a>Implementatie van certificaten van de toepassing aan het cluster.

U kunt dezelfde stappen gebruiken zoals wordt beschreven in stap 5 hierboven om de certificaten die aan de knooppunten van een keyvault is geïmplementeerd. u moet alleen definiëren en andere parameters gebruikt.


## <a name="adding-or-removing-client-certificates"></a>Het toevoegen of verwijderen van clientcertificaten

Naast de clustercertificaten, kunt u clientcertificaten voor het uitvoeren van beheerbewerkingen op een service fabric-cluster toevoegen.

U kunt twee soorten clientcertificaten - beheerder toevoegen of alleen-lezen. Deze kunnen vervolgens worden gebruikt om toegang aan de bewerkingen en zoekopdrachten op het cluster te beheren. Standaard worden de clustercertificaten toegevoegd aan de lijst met toegestane Admin certificaten.

u kunt een willekeurig aantal clientcertificaten opgeven. Elke toevoeging/verwijdering resulteert in een configuratie-update met de service fabric-cluster


### <a name="adding-client-certificates---admin-or-read-only-via-portal"></a>Clientcertificaten - beheerder toe te voegen of alleen-lezen via de portal

1. Navigeer naar de blade beveiliging en selecteer de '+ verificatie' knop boven op het tabblad Beveiliging.
2. Kies op de blade 'Authentication toevoegen' het 'verificatietype' - 'Client alleen-lezen' of 'Admin client'
3. Kies nu de autorisatie-methode. Hiermee geeft u aan de Service Fabric of het opzoeken van dit certificaat met behulp van de onderwerpnaam of de vingerafdruk. Het is in het algemeen niet verstandig uit veiligheidsoverwegingen met de autorisatiemethode van de onderwerpnaam. 

![Clientcertificaat toevoegen][Add_Client_Cert]

### <a name="deletion-of-client-certificates---admin-or-read-only-using-the-portal"></a>Verwijderen van clientcertificaten - beheer of alleen-lezen met behulp van de portal

Een secundair certificaat worden gebruikt voor een clusterbeveiliging navigeren naar de blade beveiliging verwijderen en selecteer de optie 'Verwijderen' in het contextmenu op de specifieke certificaat.



## <a name="next-steps"></a>Volgende stappen
Lees deze artikelen voor meer informatie over het Clusterbeheer:

* [Het upgradeproces service Fabric-Cluster en de verwachtingen van u](service-fabric-cluster-upgrade.md)
* [Op rollen gebaseerde toegang instellen voor clients](service-fabric-cluster-security-roles.md)

<!--Image references-->
[Delete_Swap_Cert]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_09.PNG
[Add_Client_Cert]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_13.PNG
[Json_Pub_Setting1]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_14.PNG
[Json_Pub_Setting2]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_15.PNG
[Json_Pub_Setting3]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_16.PNG
[Json_Pub_Setting4]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_17.PNG
[Json_Pub_Setting5]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_18.PNG


