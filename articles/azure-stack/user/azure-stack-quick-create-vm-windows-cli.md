---
title: Een Windows-machine maken in Azure Stack met behulp van Azure CLI | Microsoft Docs
description: Informatie over het maken van een virtuele Windows-machine in Azure Stack met behulp van Azure CLI
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 01/14/2019
ms.author: mabrigg
ms.custom: mvc
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: 15dd878b7ff322ac8243b37649f16fed4e068af4
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58091712"
---
# <a name="quickstart-create-a-windows-server-virtual-machine-by-using-azure-cli-in-azure-stack"></a>Snelstartgids: een Windows Server-machine maken met behulp van Azure CLI in Azure Stack

‎*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

U kunt een Windows Server 2016 virtuele machine maken met behulp van de Azure CLI. Volg de stappen in dit artikel te maken en gebruiken van een virtuele machine. Dit artikel vindt u ook de volgende stappen uit:

* Verbinding maken met de virtuele machine met een externe client.
* De IIS-webserver installeren en de standaard-startpagina weergeven.
* Uw resources opschonen.

## <a name="prerequisites"></a>Vereisten

* Zorg ervoor dat uw Azure Stack-operators toegevoegd de **Windows Server 2016** installatiekopie naar de Azure Stack marketplace.

* Azure Stack is vereist voor een specifieke versie van Azure CLI voor het maken en beheren van de resources. Als u geen Azure-CLI geconfigureerd voor Azure Stack, volgt u de stappen voor het [installeren en configureren van Azure CLI](azure-stack-version-profiles-azurecli2.md).

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep is een logische container waarin u kunt implementeren en beheren van Azure Stack-resources. Uitvoeren van uw Azure Stack-omgeving, de [az-groep maken](/cli/azure/group#az-group-create) opdracht om een resourcegroep te maken.

> [!NOTE]
>  Waarden zijn voor de variabelen in de codevoorbeelden toegewezen. U kunt echter nieuwe waarden toewijzen als u wilt.

Het volgende voorbeeld wordt een resourcegroep met de naam myResourceGroup in de lokale locatie.

```cli
az group create --name myResourceGroup --location local
```

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken

Een virtuele machine (VM) maken met behulp van de [az vm maken](/cli/azure/vm#az-vm-create) opdracht. Het volgende voorbeeld wordt een virtuele machine met de naam myVM. In dit voorbeeld wordt Demouser voor de naam van een gebruiker met beheerdersrechten en Demouser@123 als het wachtwoord van de gebruiker. Deze waarden die geschikt is voor uw omgeving te wijzigen.

```cli
az vm create \
  --resource-group "myResourceGroup" \
  --name "myVM" \
  --image "Win2016Datacenter" \
  --admin-username "Demouser" \
  --admin-password "Demouser@123" \
  --use-unmanaged-disk \
  --location local
```

Wanneer de virtuele machine wordt gemaakt, de **PublicIPAddress** parameter in de uitvoer bevat de openbare IP-adres voor de virtuele machine. Noteer dit adres omdat u deze nodig om toegang tot de virtuele machine.

## <a name="open-port-80-for-web-traffic"></a>Poort 80 openen voor webverkeer

Omdat deze virtuele machine wordt gebruikt om uit te voeren van de IIS-webserver, moet u poort 80 openen voor internetverkeer.

Gebruik de [az vm open-port](/cli/azure/vm) opdracht om poort 80 te openen.

```cli
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="connect-to-the-virtual-machine"></a>Verbinding maken met de virtuele machine

Gebruik de volgende opdracht om te maken van een extern bureaublad-verbinding met uw virtuele machine. 'Openbare IP-adres' vervangen door het IP-adres van uw virtuele machine. Wanneer u wordt gevraagd, typt u de gebruikersnaam en het wachtwoord die u hebt gebruikt voor de virtuele machine.

```
mstsc /v <Public IP Address>
```

## <a name="install-iis-using-powershell"></a>IIS installeren met behulp van PowerShell

Nu dat u zich hebt aangemeld bij de virtuele machine, kunt u PowerShell gebruiken om IIS te installeren. Start PowerShell op de virtuele machine en voer de volgende opdracht uit:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-the-iis-welcome-page"></a>De welkomstpagina van IIS weergeven

U kunt een webbrowser van uw keuze gebruiken om de standaard IIS-welkomstpagina weer te geven. Gebruik het openbare IP-adres dat is beschreven in de vorige sectie de standaardpagina bezoekt.

![Standaardsite van IIS](./media/azure-stack-quick-create-vm-windows-cli/default-iis-website.png)

## <a name="clean-up-resources"></a>Resources opschonen

Schoon de resources die u niet meer nodig. Gebruik de [az group delete](/cli/azure/group#az-group-delete) opdracht voor het verwijderen van de resourcegroep, de virtuele machine, en alle gerelateerde resources.

```cli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze quickstart maakt u een eenvoudige virtuele machine voor Windows-Server geïmplementeerd. Voor meer informatie over virtuele machines van Azure Stack, blijven [overwegingen voor virtuele Machines in Azure Stack](azure-stack-vm-considerations.md).
