---
title: Foutopsporing op afstand met continue levering inschakelen | Microsoft Docs
description: Meer informatie over het foutopsporing op afstand inschakelen wanneer u doorlopende levering implementeren naar Azure
services: cloud-services
documentationcenter: .net
author: mikejo
manager: ghogen
editor: 
ms.assetid: 7d423639-3b2f-4ca5-ac5a-9ac19a217c29
ms.service: cloud-services
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 11/18/2016
ms.author: mikejo
ms.openlocfilehash: 1a30b42e6e84edf9a7cef861aaf6a60e87c473d0
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="enable-remote-debugging-when-using-continuous-delivery-to-publish-to-azure"></a>Schakel foutopsporing op afstand in bij het gebruik van onafgebroken levering om te publiceren op Azure
U kunt foutopsporing op afstand inschakelen in Azure, voor cloudservices of virtuele machines, wanneer u [continue levering](cloud-services-dotnet-continuous-delivery.md) publiceren naar Azure met de volgende stappen.

## <a name="enabling-remote-debugging-for-cloud-services"></a>Foutopsporing op afstand voor cloudservices inschakelen
1. Op de agent build instellen van de eerste omgeving voor Azure zoals wordt beschreven in [Command-Line bouwen voor Azure](http://msdn.microsoft.com/library/hh535755.aspx).
2. Omdat de foutopsporing op afstand-runtime (msvsmon.exe) vereist voor het pakket is, installeert de **externe hulpprogramma's voor Visual Studio**.

   * [Externe hulpprogramma's voor Visual Studio 2017](https://go.microsoft.com/fwlink/?LinkId=746570)
   * [Externe hulpprogramma's voor Visual Studio 2015](https://go.microsoft.com/fwlink/?LinkId=615470)
   * [Externe hulpprogramma's voor Visual Studio 2013 Update 5](https://www.microsoft.com/download/details.aspx?id=48156)
    
   Als alternatief kunt kunt u de binaire bestanden voor foutopsporing op afstand kopiëren van een systeem met Visual Studio is geïnstalleerd.

3. Een certificaat te maken, zoals wordt beschreven in [certificaten voor Azure Cloud Services-overzicht](cloud-services-certs-create.md). Houd de pfx en het RDP-certificaatvingerafdruk en upload het certificaat naar de doel-cloudservice.
4. Gebruik de volgende opties op de opdrachtregel MSBuild te verpakken met foutopsporing op afstand is ingeschakeld. (Vervangen door feitelijke paden naar uw systeem- en bestanden voor de hoek tussen items.)
   
   ```cmd
   msbuild /TARGET:PUBLISH /PROPERTY:Configuration=Debug;EnableRemoteDebugger=true;VSX64RemoteDebuggerPath="<remote tools path>";RemoteDebuggerConnectorCertificateThumbprint="<thumbprint of the certificate added to the cloud service>";RemoteDebuggerConnectorVersion="2.7" "<path to your VS solution file>"
   ```
   
   `VSX64RemoteDebuggerPath`het pad naar de map is waarin msvsmon.exe in de externe hulpprogramma's voor Visual Studio.
   `RemoteDebuggerConnectorVersion`is de Azure SDK-versie in uw cloudservice. Het moet ook overeenkomen met de versie met Visual Studio is geïnstalleerd.

5. Publiceren naar de doel-cloudservice met behulp van de pakket- en .cscfg-bestand gegenereerd in de vorige stap.
6. Importeer het certificaat (.pfx-bestand) met de machine met Visual Studio met Azure SDK voor .NET is geïnstalleerd. Zorg ervoor dat voor het importeren naar de `CurrentUser\My` certificaatarchief, anders koppelen aan het foutopsporingsprogramma in Visual Studio zal mislukken.

## <a name="enabling-remote-debugging-for-virtual-machines"></a>Foutopsporing op afstand voor virtuele machines inschakelen
1. Maak een virtuele machine van Azure. Zie [maken van een virtuele Machine met WindowsServer](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) of [maken en beheren van virtuele Machines in Visual Studio in Azure](../virtual-machines/windows/classic/manage-visual-studio.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
2. Op de Azure portal] (http://go.microsoft.com/fwlink/p/?LinkID=269851), gaat u naar de virtuele machine van de virtuele machine **RDP CERTIFICAATVINGERAFDRUK**. Deze waarde wordt gebruikt voor de `ServerThumbprint` waarde in de configuratie voor de uitbreiding.
3. Maken van een clientcertificaat, zoals wordt beschreven in [certificaten voor Azure Cloud Services-overzicht](cloud-services-certs-create.md) (Houd de pfx en het RDP-certificaatvingerafdruk).
4. Azure Powershell installeren (versie 0.7.4 of hoger) zoals wordt beschreven in [installeren en configureren van Azure PowerShell](/powershell/azure/overview).
5. Voer het volgende script de RemoteDebug-uitbreiding in te schakelen. Vervang de paden en persoonlijke gegevens door uw eigen, zoals de naam van abonnement, servicenaam en vingerafdruk.
   
   > [!NOTE]
   > Dit script is geconfigureerd voor Visual Studio 2015. Als u Visual Studio 2013 of Visual Studio 2017, wijzigt u de `$referenceName` en `$extensionName` toewijzingen hieronder aan `RemoteDebugVS2013` of `RemoteDebugVS2017`.

    ```powershell   
    Add-AzureAccount

    Select-AzureSubscription "My Microsoft Subscription"

    $vm = Get-AzureVM -ServiceName "mytestvm1" -Name "mytestvm1"

    $endpoints = @(
                    ,@{Name="RDConnVS2013"; PublicPort=30400; PrivatePort=30398}
                    ,@{Name="RDFwdrVS2013"; PublicPort=31400; PrivatePort=31398}
                )

    foreach($endpoint in $endpoints)
    {
        Add-AzureEndpoint -VM $vm -Name $endpoint.Name -Protocol tcp -PublicPort $endpoint.PublicPort -LocalPort $endpoint.PrivatePort
    }

    $referenceName = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug.RemoteDebugVS2015"
    $publisher = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug"
    $extensionName = "RemoteDebugVS2015"
    $version = "1.*"
    $publicConfiguration = "<PublicConfig><Connector.Enabled>true</Connector.Enabled><ClientThumbprint>56D7D1B25B472268E332F7FC0C87286458BFB6B2</ClientThumbprint><ServerThumbprint>E7DCB00CB916C468CC3228261D6E4EE45C8ED3C6</ServerThumbprint><ConnectorPort>30398</ConnectorPort><ForwarderPort>31398</ForwarderPort></PublicConfig>"

    $vm | Set-AzureVMExtension -ReferenceName $referenceName -Publisher $publisher -ExtensionName $extensionName -Version $version -PublicConfiguration $publicConfiguration

    foreach($extension in $vm.VM.ResourceExtensionReferences)
    {
        if(($extension.ReferenceName -eq $referenceName) `
        -and ($extension.Publisher -eq $publisher) `
        -and ($extension.Name -eq $extensionName) `
        -and ($extension.Version -eq $version))
        {
            $extension.ResourceExtensionParameterValues[0].Key = 'config.txt'
            break
        }
    }

    $vm | Update-AzureVM
    ```

6. Importeer het certificaat (.pfx) met de machine met Visual Studio met Azure SDK voor .NET is geïnstalleerd.

