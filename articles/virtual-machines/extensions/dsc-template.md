---
title: Desired State Configuration-extensie met Azure Resource Manager-sjablonen
description: Meer informatie over de Sjabloondefinitie van de Resource Manager-voor de extensie Desired State Configuration (DSC) in Azure.
services: virtual-machines-windows
documentationcenter: ''
author: DCtheGeek
manager: carmonm
editor: ''
tags: azure-resource-manager
keywords: dsc
ms.assetid: b5402e5a-1768-4075-8c19-b7f7402687af
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 05/02/2018
ms.author: dacoulte
ms.openlocfilehash: d007869bb8bad1a2f0775a1ab2c1bf5d27c1cb8f
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37866217"
---
# <a name="desired-state-configuration-extension-with-azure-resource-manager-templates"></a>Desired State Configuration-extensie met Azure Resource Manager-sjablonen

In dit artikel beschrijft de Azure Resource Manager-sjabloon voor de [handler voor Desired State Configuration (DSC)-extensie](dsc-overview.md).

> [!NOTE]
> U kunt tegenkomen enigszins schema voorbeelden. De wijziging in het schema is opgetreden in de release van oktober 2016. Zie voor meer informatie, [bijwerken vanuit een eerdere indeling](#update-from-a-previous-format).

## <a name="template-example-for-a-windows-vm"></a>Voorbeeld van de sjabloon voor een Windows-VM

Het volgende codefragment wordt de **Resource** gedeelte van de sjabloon.
De DSC-extensie neemt standaard extensie-eigenschappen.
Zie voor meer informatie, [VirtualMachineExtension klasse](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.compute.models.virtualmachineextension?view=azure-dotnet.).

```json
{
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "[concat(parameters('VMName'),'/Microsoft.Powershell.DSC')]",
    "apiVersion": "2017-12-01",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', parameters('VMName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Powershell",
        "type": "DSC",
        "typeHandlerVersion": "2.76",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "protectedSettings": {
                "Items": {
                    "registrationKeyPrivate": "registrationKey"
                }
            },
            "publicSettings": {
                "configurationArguments": [{
                        "Name": "RegistrationKey",
                        "Value": {
                            "UserName": "PLACEHOLDER_DONOTUSE",
                            "Password": "PrivateSettingsRef:registrationKeyPrivate"
                        }
                    },
                    {
                        "RegistrationUrl": "registrationUrl"
                    },
                    {
                        "NodeConfigurationName": "nodeConfigurationName"
                    }
                ]
            }
        }
    }
}
```

## <a name="template-example-for-windows-virtual-machine-scale-sets"></a>Hiermee stelt u het voorbeeld van de sjabloon voor virtuele-machineschaalset in Windows

Een virtuele machine scale set knooppunt heeft een **eigenschappen** sectie waarvoor een **VirtualMachineProfile, extensionProfile** kenmerk.
Onder **extensies**, de gegevens voor DSC-extensie toe te voegen.

De DSC-extensie neemt standaard extensie-eigenschappen.
Zie voor meer informatie, [VirtualMachineScaleSetExtension klasse](/dotnet/api/microsoft.azure.management.compute.models.virtualmachinescalesetextension?view=azure-dotnet).

```json
"extensionProfile": {
    "extensions": [{
        "type": "Microsoft.Compute/virtualMachines/extensions",
        "name": "[concat(parameters('VMName'),'/Microsoft.Powershell.DSC')]",
        "apiVersion": "2017-12-01",
        "location": "[resourceGroup().location]",
        "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', parameters('VMName'))]"
        ],
        "properties": {
            "publisher": "Microsoft.Powershell",
            "type": "DSC",
            "typeHandlerVersion": "2.76",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "protectedSettings": {
                    "Items": {
                        "registrationKeyPrivate": "registrationKey"
                    }
                },
                "publicSettings": {
                    "configurationArguments": [{
                            "Name": "RegistrationKey",
                            "Value": {
                                "UserName": "PLACEHOLDER_DONOTUSE",
                                "Password": "PrivateSettingsRef:registrationKeyPrivate"
                            },
                        },
                        {
                            "RegistrationUrl": "registrationUrl"
                        },
                        {
                            "NodeConfigurationName": "nodeConfigurationName"
                        }
                    ]
                }
            },
        }
    }]
}
```

## <a name="detailed-settings-information"></a>Instellingen voor gedetailleerde informatie

Gebruik het volgende schema in de **instellingen** sectie van de Azure-DSC-extensie in Resource Manager-sjabloon.

Zie voor een lijst van de argumenten die beschikbaar voor een script voor de configuratie van de standaard zijn, [configuratiescript standaard](#default-configuration-script).

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

| Naam van eigenschap | Type | Beschrijving |
| --- | --- | --- |
| settings.wmfVersion |tekenreeks |Hiermee geeft u de versie van Windows Management Framework (WMF) die moet worden geïnstalleerd op de virtuele machine. Als deze eigenschap instelt op **nieuwste** installeert de meest recente versie van WMF. Op dit moment de enige mogelijke waarden voor deze eigenschap zijn **4.0**, **5.0**, **5.0PP**, en **nieuwste**. Deze mogelijke waarden zijn afhankelijk van updates. De standaardwaarde is **nieuwste**. |
| settings.configuration.url |tekenreeks |Hiermee geeft u de URL-locatie van waaruit het ZIP-bestand van uw DSC-configuratie gedownload. Als de opgegeven URL is een SAS-token voor toegang vereist, stelt u de **protectedSettings.configurationUrlSasToken** eigenschap met de waarde van uw SAS-token. Deze eigenschap is vereist als **settings.configuration.script** of **settings.configuration.function** zijn gedefinieerd. Als er geen waarde is opgegeven voor deze eigenschappen, de extensie roept de standaard-configuratiescript voor het instellen van de metagegevens van de locatie van Configuration Manager (LCM) en moeten worden opgegeven als argumenten. |
| settings.configuration.script |tekenreeks |Hiermee geeft u de bestandsnaam van het script dat de definitie van de DSC-configuratie bevat. Met dit script moet zich in de hoofdmap van het ZIP-bestand dat moet worden gedownload vanaf de URL die is opgegeven door de **configuration.url** eigenschap. Deze eigenschap is vereist als **settings.configuration.url** of **settings.configuration.script** zijn gedefinieerd. Als er geen waarde is opgegeven voor deze eigenschappen, de extensie roept de standaard-configuratiescript om in te stellen LCM metagegevens en moeten worden opgegeven als argumenten. |
| settings.configuration.function |tekenreeks |Hiermee geeft u de naam van de DSC-configuratie. De configuratie met de naam moet worden opgenomen in het script dat **configuration.script** definieert. Deze eigenschap is vereist als **settings.configuration.url** of **settings.configuration.function** zijn gedefinieerd. Als er geen waarde is opgegeven voor deze eigenschappen, de extensie roept de standaard-configuratiescript om in te stellen LCM metagegevens en moeten worden opgegeven als argumenten. |
| settings.configurationArguments |Verzameling |Hiermee definieert u de parameters die u wilt doorgeven aan uw DSC-configuratie. Deze eigenschap is niet versleuteld. |
| settings.configurationData.url |tekenreeks |Geeft de URL waaruit u uw gegevens (.psd1)-configuratiebestand te downloaden om te gebruiken als invoer voor uw DSC-configuratie. Als de opgegeven URL is een SAS-token voor toegang vereist, stelt u de **protectedSettings.configurationDataUrlSasToken** eigenschap met de waarde van uw SAS-token. |
| settings.privacy.dataEnabled |tekenreeks |Hiermee of verzamelen van telemetriegegevens uitgeschakeld. De enige mogelijke waarden voor deze eigenschap zijn **inschakelen**, **uitschakelen**, **''**, of **$null**. Deze eigenschap verlaten leeg of null zijn, kunt telemetrie. De standaardwaarde is **''**. Zie voor meer informatie, [Azure DSC-extensie gegevensverzameling](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/). |
| settings.advancedOptions.downloadMappings |Verzameling |Hiermee definieert u alternatieve locaties van waaruit WMF gedownload. Zie voor meer informatie, [Azure DSC-extensie 2.8 en downloaden van de extensie afhankelijkheden toewijzen aan uw eigen locatie](http://blogs.msdn.com/b/powershell/archive/2015/10/21/azure-dsc-extension-2-2-amp-how-to-map-downloads-of-the-extension-dependencies-to-your-own-location.aspx). |
| protectedSettings.configurationArguments |Verzameling |Hiermee definieert u de parameters die u wilt doorgeven aan uw DSC-configuratie. Deze eigenschap is versleuteld. |
| protectedSettings.configurationUrlSasToken |tekenreeks |Hiermee geeft u de SAS-token gebruiken voor toegang tot de URL die **configuration.url** definieert. Deze eigenschap is versleuteld. |
| protectedSettings.configurationDataUrlSasToken |tekenreeks |Hiermee geeft u de SAS-token gebruiken voor toegang tot de URL die **configurationData.url** definieert. Deze eigenschap is versleuteld. |

## <a name="default-configuration-script"></a>Standaard-configuratiescript

Zie voor meer informatie over de volgende waarden [Local Configuration Manager-basisinstellingen](/powershell/dsc/metaconfig#basic-settings).
U kunt de DSC-extensie standaard-configuratiescript gebruiken alleen de LCM om eigenschappen te configureren die worden vermeld in de volgende tabel.

| Naam van eigenschap | Type | Beschrijving |
| --- | --- | --- |
| settings.configurationArguments.RegistrationKey |SecureString |De vereiste eigenschap. Hiermee geeft u de sleutel die wordt gebruikt om een knooppunt te registreren bij Azure Automation-service als het wachtwoord van een PowerShell-referentieobject. Deze waarde automatisch worden gedetecteerd met behulp van de **listkeys** methode op basis van het Automation-account. De waarde moet worden beveiligd als een beveiligde instelling. |
| settings.configurationArguments.RegistrationUrl |tekenreeks |De vereiste eigenschap. Hiermee geeft u de URL van het Automation-eindpunt waar het knooppunt probeert te registreren. Deze waarde automatisch worden gedetecteerd met behulp van de **verwijzing** methode op basis van het Automation-account. |
| settings.configurationArguments.NodeConfigurationName |tekenreeks |De vereiste eigenschap. Hiermee geeft u de configuratie van de knooppunten in het Automation-account om toe te wijzen aan het knooppunt. |
| settings.configurationArguments.ConfigurationMode |tekenreeks |Hiermee geeft u de modus voor LCM. Geldige opties zijn onder andere **ApplyOnly**, **ApplyandMonitor**, en **ApplyandAutoCorrect**.  De standaardwaarde is **ApplyandMonitor**. |
| settings.configurationArguments.RefreshFrequencyMins | UInt32 | Hiermee geeft u op hoe vaak LCM probeert om te controleren met de Automation-account voor updates.  Standaardwaarde is **30**.  De minimumwaarde is **15**. |
| settings.configurationArguments.ConfigurationModeFrequencyMins | UInt32 | Hiermee geeft u op hoe vaak LCM valideert de huidige configuratie. Standaardwaarde is **15**. De minimumwaarde is **15**. |
| settings.configurationArguments.RebootNodeIfNeeded | booleaans | Hiermee geeft u op of een knooppunt worden automatisch opnieuw opgestart kan als een bewerking DSC om vraagt. Standaardwaarde is **false**. |
| settings.configurationArguments.ActionAfterReboot | tekenreeks | Hiermee geeft u op wat gebeurt er na een opnieuw opstarten bij het toepassen van een configuratie. Geldige opties zijn **ContinueConfiguration** en **de StopConfiguration**. Standaardwaarde is **ContinueConfiguration**. |
| settings.configurationArguments.AllowModuleOverwrite | booleaans | Hiermee geeft u op of LCM bestaande modules op het knooppunt overschrijft. Standaardwaarde is **false**. |

## <a name="settings-vs-protectedsettings"></a>Instellingen voor Visual Studio. ProtectedSettings

Alle instellingen worden opgeslagen in een tekstbestand instellingen op de virtuele machine.
Eigenschappen die worden vermeld onder **instellingen** zijn openbare eigenschappen.
Openbare eigenschappen worden niet in het tekstbestand instellingen versleuteld.
Eigenschappen die worden vermeld onder **protectedSettings** zijn versleuteld met een certificaat en worden niet weergegeven in tekst zonder opmaak in het bestand met instellingen op de virtuele machine.

Als de configuratie moet referenties, kunt u de referenties in opnemen **protectedSettings**:

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

## <a name="example-configuration-script"></a>Configuratie-voorbeeldscript

Het volgende voorbeeld ziet het standaardgedrag voor het DSC-extensie, die is het metagegevensinstellingen bieden aan LCM en registreren bij de service Automation DSC.
Configuratie-argumenten zijn vereist.
Configuratie argumenten worden doorgegeven aan de standaard-configuratiescript LCM metagegevens instellen.

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
        "RegistrationUrl": "[parameters('registrationUrl1')]",
        "NodeConfigurationName": "nodeConfigurationNameValue1"
    }
},
"protectedSettings": {
    "Items": {
        "registrationKeyPrivate": "[parameters('registrationKey1']"
    }
}
```

## <a name="example-using-the-configuration-script-in-azure-storage"></a>Voorbeeld met behulp van een script voor de configuratie in Azure Storage

Het volgende voorbeeld komt uit de [DSC-extensie-handler overzicht](dsc-overview.md).
In dit voorbeeld maakt gebruik van Resource Manager-sjablonen in plaats van de cmdlets voor het implementeren van de extensie.
De IisInstall.ps1-configuratie op te slaan, plaats deze in een ZIP-bestand en upload het bestand in een toegankelijke URL.
In dit voorbeeld maakt gebruik van Azure Blob-opslag, maar u kunt een ZIP-bestanden downloaden vanaf elke willekeurige locatie.

In het Resource Manager-sjabloon, Hiermee geeft u de volgende code de virtuele machine om te downloaden van het juiste bestand en voer vervolgens de juiste PowerShell-functie:

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

## <a name="update-from-a-previous-format"></a>Bijwerken van een vorige indeling

Alle instellingen in de vorige indeling van de extensie (en de openbare eigenschappen hebben **ModulesUrl**, **ConfigurationFunction**, **SasToken**, of  **Eigenschappen van**) automatisch worden aangepast aan de huidige indeling van de extensie.
Ze net zoals voor worden uitgevoerd.

Het volgende schema ziet u wat de vorige instellingenschema hebt bekeken, zoals:

```json
"settings": {
    "WMFVersion": "latest",
    "ModulesUrl": "https://UrlToZipContainingConfigurationScript.ps1.zip",
    "SasToken": "SAS Token if ModulesUrl points to private Azure Blob Storage",
    "ConfigurationFunction": "ConfigurationScript.ps1\\ConfigurationFunction",
    "Properties": {
        "ParameterToConfigurationFunction1": "Value1",
        "ParameterToConfigurationFunction2": "Value2",
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

Hier ziet u hoe de vorige indeling worden aangepast aan de huidige indeling:

| Naam van eigenschap | Vorige schema equivalent |
| --- | --- |
| settings.wmfVersion |Instellingen. WMFVersion |
| settings.configuration.url |settings.ModulesUrl |
| settings.configuration.script |Eerste deel van de instellingen. ConfigurationFunction (vóór \\ \\) |
| settings.configuration.function |Tweede deel van de instellingen. ConfigurationFunction (nadat \\ \\) |
| settings.configurationArguments |Instellingen. Eigenschappen |
| settings.configurationData.url |protectedSettings.DataBlobUri (zonder de SAS-token) |
| settings.privacy.dataEnabled |Instellingen. Privacy.DataEnabled |
| settings.advancedOptions.downloadMappings |settings.AdvancedOptions.DownloadMappings |
| protectedSettings.configurationArguments |protectedSettings.Properties |
| protectedSettings.configurationUrlSasToken |settings.SasToken |
| protectedSettings.configurationDataUrlSasToken |SAS-token van protectedSettings.DataBlobUri |

## <a name="troubleshooting---error-code-1100"></a>Probleemoplossing - foutcode 1100

Foutcode 1100 duidt op een probleem met de invoer van de gebruiker naar de DSC-extensie.
De tekst van deze fouten varieert, en kan worden gewijzigd.
Hier volgen enkele van de fouten die u kunt tegenkomen, en hoe u deze kunt oplossen.

### <a name="invalid-values"></a>Ongeldige waarden

"Privacy.dataCollection is '{0}'.
De enige mogelijke waarden zijn ", 'Inschakelen' en 'Disable'".
"WmfVersion is '{0}'.
Alleen de mogelijke waarden zijn... en de 'nieuwste' ".

**Probleem**: een opgegeven waarde is niet toegestaan.

**Oplossing**: Wijzig de ongeldige waarde in een geldige waarde.
Voor meer informatie, Zie de tabel in [Details](#details).

### <a name="invalid-url"></a>De URL is ongeldig

"ConfigurationData.url is '{0}'. Dit is geen geldige URL' ' DataBlobUri is '{0}'. Dit is geen geldige URL' ' Configuration.url is '{0}'. Dit is geen geldige URL'

**Probleem**: een opgegeven URL is niet geldig.

**Oplossing**: Controleer de opgegeven URL's.
Zorg ervoor dat alle URL's omgezet in geldige locaties dat de extensie op de externe computer openen kunt.

### <a name="invalid-configurationargument-type"></a>Ongeldig ConfigurationArgument-type

"Ongeldige configurationArguments type {0}"

**Probleem**: de *ConfigurationArguments* eigenschap kan niet worden omgezet naar een **hashtabel** object.

**Oplossing**: Controleer uw *ConfigurationArguments* eigenschap een **hashtabel**.
Ga als volgt de indeling die is opgegeven in het voorgaande voorbeeld. Bekijk voor offertes, komma's en accolades.

### <a name="duplicate-configurationarguments"></a>Dubbele ConfigurationArguments

"Gevonden dubbele argumenten{0}' in zowel openbare als beveiligde configurationArguments"

**Probleem**: de *ConfigurationArguments* in de openbare-instellingen en de *ConfigurationArguments* in beveiligde instellingen eigenschappen met dezelfde naam hebben.

**Oplossing**: Verwijder een van de dubbele eigenschappen.

### <a name="missing-properties"></a>Ontbrekende eigenschappen

"Configuration.function vereist configuration.url of configuration.module is opgegeven."

"Configuration.url vereist dat configuration.script is opgegeven."

"Configuration.script vereist dat configuration.url is opgegeven."

"Configuration.url vereist dat configuration.function is opgegeven."

"ConfigurationUrlSasToken vereist dat configuration.url is opgegeven."

"ConfigurationDataUrlSasToken vereist dat configurationData.url is opgegeven."

**Probleem**: een gedefinieerde eigenschap moet een andere eigenschap ontbreekt.

**Oplossingen**:

- Geef de eigenschap ontbreekt.
- Verwijder de eigenschap waarvoor de eigenschap ontbreekt.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [ingesteld met behulp van de virtuele-machineschaalset met de Azure-DSC-extensie](../../virtual-machine-scale-sets/virtual-machine-scale-sets-dsc.md).
- Lees meer informatie over [van DSC beveiligde Referentiebeheer](dsc-credentials.md).
- Krijgen een [Inleiding tot de Azure-DSC-extensie-handler](dsc-overview.md).
- Voor meer informatie over PowerShell DSC, gaat u naar de [PowerShell-documentatiecentrum](/powershell/dsc/overview).
