---
title: Desired State Configuration-uitbreiding met Azure Resource Manager-sjablonen | Microsoft Docs
description: Resource Manager-sjabloon definitie voor de gewenste status Configuration Extension in Azure
services: virtual-machines-windows
documentationcenter: 
author: mgreenegit
manager: timlt
editor: 
tags: azure-resource-manager
keywords: dsc
ms.assetid: b5402e5a-1768-4075-8c19-b7f7402687af
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 02/02/2018
ms.author: migreene
ms.openlocfilehash: f638d1530541526316f6e409f1efd44f136992a5
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="desired-state-configuration-extension-with-azure-resource-manager-templates"></a>Desired State Configuration-uitbreiding met Azure Resource Manager-sjablonen

In dit artikel beschrijft de Azure Resource Manager-sjabloon voor de [Desired State Configuration extensie handler](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

*Opmerking: u kunt iets anders schema voorbeelden tegenkomen. * 
 *De wijziging in het schema is opgetreden in de oktober 2016 vrijgeven.* 
 *Details worden vermeld in de sectie van deze pagina met de titel*
*[bijwerkt op basis van de vorige indeling](##Updating-from-the-Previous-Format)*.

## <a name="template-example-for-a-windows-vm"></a>Voorbeeld van de sjabloon voor een virtuele machine van Windows

Er geldt het volgende fragment in de sectie Resource van de sjabloon.
De DSC-extensie neemt standaard extensie eigenschappen zoals beschreven in [VirtualMachineExtension-klasse](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.management.compute.models.virtualmachineextension?view=azure-dotnet.)

```json
            "name": "Microsoft.Powershell.DSC",
            "type": "extensions",
             "location": "[resourceGroup().location]",
             "apiVersion": "2015-06-15",
             "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.72",
                  "autoUpgradeMinorVersion": true,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                    "configurationArguments": {
                        {
                            "Name": "RegistrationKey",
                            "Value": {
                                "UserName": "PLACEHOLDER_DONOTUSE",
                                "Password": "PrivateSettingsRef:registrationKeyPrivate"
                            },
                        },
                        "RegistrationUrl" : "[parameters('registrationUrl1')]",
                        "NodeConfigurationName" : "nodeConfigurationNameValue1"
                        }
                        },
                        "protectedSettings": {
                            "Items": {
                                        "registrationKeyPrivate": "[parameters('registrationKey1']"
                                    }
                        }
                    }
```

## <a name="template-example-for-windows-vmss"></a>Voorbeeld van de sjabloon voor Windows VMSS

Een knooppunt VMSS heeft een sectie 'Eigenschappen' met de 'VirtualMachineProfile', 'extensionProfile'-kenmerk. DSC wordt toegevoegd onder 'extensions'.

De DSC-extensie neemt standaard extensie eigenschappen zoals beschreven in [VirtualMachineScaleSetExtension klasse](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.management.compute.models.virtualmachinescalesetextension?view=azure-dotnet).

```json
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.72",
                        "autoUpgradeMinorVersion": true,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configurationArguments": {
                                {
                                    "Name": "RegistrationKey",
                                    "Value": {
                                        "UserName": "PLACEHOLDER_DONOTUSE",
                                        "Password": "PrivateSettingsRef:registrationKeyPrivate"
                                    },
                                },
                                "RegistrationUrl" : "[parameters('registrationUrl1')]",
                                "NodeConfigurationName" : "nodeConfigurationNameValue1"
                        }
                        },
                        "protectedSettings": {
                            "Items": {
                                        "registrationKeyPrivate": "[parameters('registrationKey1']"
                                    }
                        }
                    }
                ]
            }
        }
```

## <a name="detailed-settings-information"></a>Informatie over de gedetailleerde instellingen

Het volgende schema is voor het gedeelte instellingen van de Azure DSC-uitbreiding in een Azure Resource Manager-sjabloon.

*Voor een lijst van de argumenten die beschikbaar zijn voor het script van de configuratie standaard*
*Zie de sectie met de naam*
*[standaard configuratiescript](##Default-Configuration-Script) *.

```json

"settings": {
  "wmfVersion": "latest",
  "configuration": {
    "url": "http://validURLToConfigLocation",
    "script": "ConfigurationScript.ps1",
    "function": "ConfigurationFunction"
  },
  "configurationArguments": {
    "argument1": "Value1",
    "argument2": "Value2"
  },
  "configurationData": {
    "url": "https://foo.psd1"
  },
  "privacy": {
    "dataCollection": "enable"
  },
  "advancedOptions": {
    "downloadMappings": {
      "customWmfLocation": "http://myWMFlocation"
    }
  }
},
"protectedSettings": {
  "configurationArguments": {
    "parameterOfTypePSCredential1": {
      "userName": "UsernameValue1",
      "password": "PasswordValue1"
    },
    "parameterOfTypePSCredential2": {
      "userName": "UsernameValue2",
      "password": "PasswordValue2"
    }
  },
  "configurationUrlSasToken": "?g!bber1sht0k3n",
  "configurationDataUrlSasToken": "?dataAcC355T0k3N"
}

```

## <a name="details"></a>Details

| De naam van eigenschap | Type | Beschrijving |
| --- | --- | --- |
| settings.wmfVersion |tekenreeks |Hiermee geeft u de versie van het Windows Management Framework die op de virtuele machine moet worden geïnstalleerd. Als u deze eigenschap instelt op 'nieuwste' installeert de meest recente versie van WMF. Mogelijke waarden voor deze eigenschap alleen huidige zijn **'4.0', '5.0', ' 5.0PP' en 'nieuwste'**. Deze mogelijke waarden zijn onderworpen aan updates. De standaardwaarde is 'nieuwste'. |
| settings.configuration.url |tekenreeks |Hiermee geeft u de URL-locatie van waaruit u uw DSC configuration zip-bestand te downloaden. Als de URL die u een SAS-token is vereist om toegang te krijgen, moet u de eigenschap protectedSettings.configurationUrlSasToken ingesteld op de waarde van de SAS-token. Deze eigenschap is vereist als settings.configuration.script en/of settings.configuration.function zijn gedefinieerd. Als geen waarde voor deze eigenschappen opgeeft, de extensie belt het configuratiescript standaard om in te stellen LCM metagegevens en moeten worden opgegeven als argumenten. |
| settings.configuration.script |tekenreeks |Hiermee geeft u de bestandsnaam van het script met de definitie van de DSC-configuratie. Dit script moet zich in de hoofdmap van het zip-bestand van de URL die is opgegeven door de eigenschap configuration.url gedownload. Deze eigenschap is vereist als settings.configuration.url en/of settings.configuration.script zijn gedefinieerd. Als geen waarde voor deze eigenschappen opgeeft, de extensie belt het configuratiescript standaard om in te stellen LCM metagegevens en argumenten moeten worden toegepast. |
| settings.configuration.function |tekenreeks |Geeft de naam van de DSC-configuratie. De configuratie met de naam moet worden opgenomen in het script dat is gedefinieerd door configuration.script. Deze eigenschap is vereist als settings.configuration.url en/of settings.configuration.function zijn gedefinieerd. Als geen waarde voor deze eigenschappen opgeeft, de extensie belt het configuratiescript standaard om in te stellen LCM metagegevens en moeten worden opgegeven als argumenten. |
| settings.configurationArguments |Verzameling |Hiermee definieert u de parameters die u wilt doorgeven aan uw DSC-configuratie. Deze eigenschap is niet versleuteld. |
| settings.configurationData.url |tekenreeks |Geeft de URL waaruit u uw gegevens (.psd1) configuratiebestand te downloaden om te gebruiken als invoer voor uw DSC-configuratie. Als de URL die u een SAS-token is vereist om toegang te krijgen, moet u de eigenschap protectedSettings.configurationDataUrlSasToken ingesteld op de waarde van de SAS-token. |
| settings.privacy.dataEnabled |tekenreeks |Schakelt verzamelen van telemetriegegevens in of uit. De enige mogelijke waarden voor deze eigenschap zijn **'Inschakelen', 'Uitschakelen', ', of $null**. Als u deze eigenschap leeg of null kan telemetrie. De standaardwaarde is % d. [Meer informatie](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/) |
| settings.advancedOptions.downloadMappings |Verzameling |Hiermee definieert u alternatieve locaties waarvan de WMF downloaden. [Meer informatie](http://blogs.msdn.com/b/powershell/archive/2015/10/21/azure-dsc-extension-2-2-amp-how-to-map-downloads-of-the-extension-dependencies-to-your-own-location.aspx) |
| protectedSettings.configurationArguments |Verzameling |Hiermee definieert u de parameters die u wilt doorgeven aan uw DSC-configuratie. Deze eigenschap is versleuteld. |
| protectedSettings.configurationUrlSasToken |tekenreeks |Hiermee geeft u de SAS-token voor toegang tot de URL die is gedefinieerd door configuration.url. Deze eigenschap is versleuteld. |
| protectedSettings.configurationDataUrlSasToken |tekenreeks |Hiermee geeft u de SAS-token voor toegang tot de URL die is gedefinieerd door configurationData.url. Deze eigenschap is versleuteld. |

## <a name="default-configuration-script"></a>Standaard-configuratiescript

Zie de pagina met documentatie voor meer informatie over deze waarden [lokale Configuration Manager-basisinstellingen](https://docs.microsoft.com/en-us/powershell/dsc/metaconfig#basic-settings).
Alleen de LCM eigenschappen in de onderstaande tabel worden geconfigureerd met behulp van de configuratie van DSC-extensie Standaardscript.

| De naam van eigenschap | Type | Beschrijving |
| --- | --- | --- |
| settings.configurationArguments.RegistrationKey |SecureString |Vereiste eigenschap. Hiermee geeft u de sleutel voor een knooppunt met de service Azure Automation registreren als het wachtwoord van een PowerShell-referentieobject. Deze waarde automatisch worden gedetecteerd via de methode listkeys tegen het Automation-account en moet worden beveiligd als een beveiligde instelling. |
| settings.configurationArguments.RegistrationUrl |tekenreeks |Vereiste eigenschap. Hiermee geeft u de Url van het Azure Automation-eindpunt waar het knooppunt probeert te registreren. Deze waarde automatisch worden gedetecteerd met behulp van de methode op basis van het Automation-account. |
| settings.configurationArguments.NodeConfigurationName |tekenreeks |Vereiste eigenschap. Hiermee geeft u de configuratie van de knooppunten in het Azure Automation-account toewijzen aan het knooppunt. |
| settings.configurationArguments.ConfigurationMode |tekenreeks |Hiermee wordt de modus voor de lokale Configuration Manager. Geldige opties zijn 'ApplyOnly', 'ApplyandMonitor' en 'ApplyandAutoCorrect'.  De standaardwaarde is 'ApplyandMonitor'. |
| settings.configurationArguments.RefreshFrequencyMins | uint32 | Hiermee geeft u op hoe vaak LCM probeert om te controleren met het Automation-account voor updates.  Standaardwaarde is 30.  Minimumwaarde is 15. |
| settings.configurationArguments.ConfigurationModeFrequencyMins | uint32 | Hiermee geeft u op hoe vaak LCM valideert de huidige configuratie.  Standaardwaarde is 15.  Minimumwaarde is 15. |
| settings.configurationArguments.RebootNodeIfNeeded | booleaans | Hiermee geeft u op of een knooppunt kan worden automatisch opnieuw opgestart als een DSC-bewerking wordt gevraagd.  Standaard is ingesteld op false. |
| settings.configurationArguments.ActionAfterReboot | tekenreeks | Hiermee geeft u op wat er gebeurt na opnieuw opstarten als een configuratie toepassen. Geldige opties zijn 'ContinueConfiguration' en 'StopConfiguration'. Standaardwaarde is 'ContinueConfiguration'. |
| settings.configurationArguments.AllowModuleOverwrite | booleaans | Hiermee geeft u op of LCM bestaande modules op het knooppunt wordt overschreven.  Standaard is ingesteld op false. |

## <a name="settings-vs-protectedsettings"></a>Vs instellingen. ProtectedSettings

Alle instellingen worden opgeslagen in een tekstbestand instellingen op de virtuele machine.
Eigenschappen onder 'instellingen' zijn eigenschappen public, omdat ze niet zijn versleuteld in het tekstbestand instellingen.
Eigenschappen onder 'protectedSettings' zijn versleuteld met een certificaat en worden niet weergegeven als tekst zonder opmaak in dit bestand op de virtuele machine.

Als de configuratie moet referenties, kunnen ze worden opgenomen in protectedSettings:

```json
"protectedSettings": {
    "configurationArguments": {
        "parameterOfTypePSCredential1": {
               "userName": "UsernameValue1",
               "password": "PasswordValue1"
        }
    }
}
```

## <a name="example"></a>Voorbeeld

Het volgende voorbeeld is het standaardgedrag voor de DSC-uitbreiding, geef de metagegevensinstellingen naar lokale Configuration Manager en registreren bij de service Azure Automation DSC.
Configuratie-argumenten zijn vereist en wordt doorgegeven aan de standaard-configuratiescript LCM metagegevens instellen.

```json
"settings": {
    "configurationArguments": {
        {
            "Name": "RegistrationKey",
            "Value": {
                "UserName": "PLACEHOLDER_DONOTUSE",
                "Password": "PrivateSettingsRef:registrationKeyPrivate"
            },
        },
        "RegistrationUrl" : "[parameters('registrationUrl1')]",
        "NodeConfigurationName" : "nodeConfigurationNameValue1"
  }
},
"protectedSettings": {
    "Items": {
                "registrationKeyPrivate": "[parameters('registrationKey1']"
            }
}
```

## <a name="example-using-configuration-script-in-azure-storage"></a>Voorbeeld configuratiescript gebruiken in Azure Storage

Het volgende voorbeeld is afgeleid van de sectie 'Aan de slag' van de [overzichtspagina DSC-uitbreiding Handler](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
In dit voorbeeld maakt gebruik van Resource Manager-sjablonen in plaats van de cmdlets voor het implementeren van de extensie.
Sla de configuratie 'IisInstall.ps1', plaats deze in een. ZIP-bestand en upload het bestand in een toegankelijke URL.
In dit voorbeeld gebruikt Azure blob-opslag, maar het is mogelijk te downloaden. ZIP-bestanden vanaf elke willekeurige locatie.

De volgende code wordt in de Azure Resource Manager-sjabloon, de virtuele machine het juiste bestand downloaden en uitvoeren van de betreffende PowerShell-functie:

```json
"settings": {
    "configuration": {
        "url": "https://demo.blob.core.windows.net/",
        "script": "IisInstall.ps1",
        "function": "IISInstall"
    }
},
"protectedSettings": {
    "configurationUrlSasToken": "odLPL/U1p9lvcnp..."
}
```

## <a name="updating-from-the-previous-format"></a>Bijwerken van de vorige indeling

Alle instellingen in de vorige indeling (met de openbare eigenschappen ModulesUrl, ConfigurationFunction, SasToken of eigenschappen) automatisch aanpassen aan de huidige indeling en uitgevoerd, net zoals vroeger.

Het volgende schema is wat de vorige instellingenschema staande:

```json
"settings": {
    "WMFVersion": "latest",
    "ModulesUrl": "https://UrlToZipContainingConfigurationScript.ps1.zip",
    "SasToken": "SAS Token if ModulesUrl points to private Azure Blob Storage",
    "ConfigurationFunction": "ConfigurationScript.ps1\\ConfigurationFunction",
    "Properties":  {
        "ParameterToConfigurationFunction1":  "Value1",
        "ParameterToConfigurationFunction2":  "Value2",
        "ParameterOfTypePSCredential1": {
            "UserName": "UsernameValue1",
            "Password": "PrivateSettingsRef:Key1"
        },
        "ParameterOfTypePSCredential2": {
            "UserName": "UsernameValue2",
            "Password": "PrivateSettingsRef:Key2"
        }
    }
},
"protectedSettings": {
    "Items": {
        "Key1": "PasswordValue1",
        "Key2": "PasswordValue2"
    },
    "DataBlobUri": "https://UrlToConfigurationDataWithOptionalSasToken.psd1"
}
```

Hier ziet u hoe de vorige indeling wordt aangepast aan de huidige indeling:

| De naam van eigenschap | Vorige Schema Equivalent |
| --- | --- |
| settings.wmfVersion |settings.WMFVersion |
| settings.configuration.url |settings.ModulesUrl |
| settings.configuration.script |Eerste deel van de instellingen. ConfigurationFunction (voordat '\\\\') |
| settings.configuration.function |Tweede deel van de instellingen. ConfigurationFunction (nadat '\\\\') |
| settings.configurationArguments |settings.Properties |
| settings.configurationData.url |protectedSettings.DataBlobUri (zonder SAS-token) |
| settings.privacy.dataEnabled |settings.Privacy.DataEnabled |
| settings.advancedOptions.downloadMappings |settings.AdvancedOptions.DownloadMappings |
| protectedSettings.configurationArguments |protectedSettings.Properties |
| protectedSettings.configurationUrlSasToken |settings.SasToken |
| protectedSettings.configurationDataUrlSasToken |SAS-token van protectedSettings.DataBlobUri |

## <a name="troubleshooting---error-code-1100"></a>Probleemoplossing - foutcode 1100

Foutcode 1100 geeft aan dat er een probleem met de invoer van de gebruiker toe aan de DSC-uitbreiding.
De tekst van deze fouten variabele en kan worden gewijzigd.
Hier zijn enkele van de fouten die mogelijk worden uitgevoerd in- en hoe u deze kunt oplossen.

### <a name="invalid-values"></a>Ongeldige waarden

'Privacy.dataCollection is {0}.
De enige mogelijke waarden zijn ', 'Enable' en 'Disable' '.
'WmfVersion is {0}.
Alleen de mogelijke waarden zijn... en de 'nieuwste' '.

Probleem: Een opgegeven waarde is niet toegestaan.

Oplossing: Wijzig de ongeldige waarde in een geldige waarde.
Zie de tabel in de sectie Details.

### <a name="invalid-url"></a>De URL is ongeldig

'ConfigurationData.url is {0}. Dit is geen geldige URL' 'DataBlobUri is {0}. Dit is geen geldige URL' 'Configuration.url is {0}. Dit is geen geldige URL'

Probleem: Een opgegeven dat URL is niet geldig.

Oplossing: Controleer de opgegeven URL's.
Zorg ervoor dat alle URL's omgezet naar geldige locaties dat de uitbreiding op de externe computer openen kunt.

### <a name="invalid-configurationargument-type"></a>Ongeldig Type voor ConfigurationArgument

'Ongeldige configurationArguments type {0}'

Probleem: De eigenschap ConfigurationArguments kan niet worden omgezet in een hash-object.

Oplossing: Controleer de eigenschap ConfigurationArguments een hashtabel.
Volg de indeling die is opgegeven in het voorgaande voorbeeld.
Kijk uit voor aanhalingstekens, komma's en accolades.

### <a name="duplicate-configurationarguments"></a>Dubbele ConfigurationArguments

'Dubbele argumenten {0} gevonden in de openbare en beveiligde configurationArguments'

Probleem: De ConfigurationArguments in instellingen voor openbare en de ConfigurationArguments in beveiligde instellingen bevatten eigenschappen met dezelfde naam.

Oplossing: Verwijder een van de dubbele eigenschappen.

### <a name="missing-properties"></a>Ontbrekende eigenschappen
"Configuration.function moet configuration.url of configuration.module worden opgegeven."

"Configuration.url vereist dat configuration.script is opgegeven."

"Configuration.script vereist dat configuration.url is opgegeven."

"Configuration.url vereist dat configuration.function is opgegeven."

"ConfigurationUrlSasToken vereist dat configuration.url is opgegeven."

"ConfigurationDataUrlSasToken vereist dat configurationData.url is opgegeven."

Probleem: Een gedefinieerde eigenschap moet een andere eigenschap die ontbreekt.

Oplossingen:

- Geef de eigenschap ontbreekt.
- Verwijder de eigenschap die de eigenschap ontbreekt moet.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over DSC en virtuele-machineschaalsets [virtuele Machine schaal wordt ingesteld met de Azure DSC-extensie](../../virtual-machine-scale-sets/virtual-machine-scale-sets-dsc.md).

Meer informatie vinden op [van DSC beveiligde Referentiebeheer](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Zie voor meer informatie over de Azure DSC-uitbreiding handler [Inleiding tot de extensie Azure Desired State Configuration handler](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Voor meer informatie over PowerShell DSC [gaat u naar de PowerShell-documentatiecentrum](https://msdn.microsoft.com/powershell/dsc/overview).
