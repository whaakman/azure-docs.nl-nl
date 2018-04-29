---
title: Een Windows-machine maken op Azure-Stack met Azure CLI | Microsoft Docs
description: Informatie over het maken van een virtuele machine van Windows op de Stack van Azure met Azure CLI
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: E26B246E-811D-44C9-9BA6-2B3CE5B62E83
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 04/23/2018
ms.author: mabrigg
ms.custom: mvc
ms.openlocfilehash: 381c1c37b0675d97adc058979a5d9b5c4fd2cc8b
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="quickstart-create-a-windows-server-virtual-machine-by-using-azure-cli-in-azure-stack"></a>Snelstartgids: een virtuele machine van Windows Server maken met behulp van Azure CLI in Azure-Stack

‎*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

U kunt een Windows Server 2016 virtuele machine maken met behulp van de Azure CLI. Volg de stappen in dit artikel maken en gebruiken van een virtuele machine. In dit artikel hebt u ook de volgende stappen uit:

* Verbinding maken met de virtuele machine met een externe client.
* De IIS-webserver installeren en de standaardstartpagina weergeven.
* Opschonen van uw resources.

## <a name="prerequisites"></a>Vereisten

* Zorg ervoor dat uw Azure-Stack-operator toegevoegd de **Windows Server 2016** installatiekopie naar de Stack van Azure marketplace.

* Azure-Stack is vereist voor een specifieke versie van Azure CLI maken en beheren van de resources. Als u geen Azure CLI geconfigureerd voor Azure-Stack, volg de stappen voor [Azure CLI installeren en configureren](azure-stack-version-profiles-azurecli2.md).

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep is een logische container waarin u kunt implementeren en beheren van resources voor Azure-Stack. Uitvoeren van uw Azure-Stack-omgeving, de [az groep maken](/cli/azure/group#az_group_create) opdracht om een resourcegroep te maken.

>[!NOTE]
 Waarden zijn voor de variabelen in de codevoorbeelden toegewezen. Als u wilt, kunt u echter nieuwe waarden toewijzen.

Het volgende voorbeeld maakt een resourcegroep met de naam myResourceGroup in de lokale locatie.

```cli
az group create --name myResourceGroup --location local
```

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken

Maken van een virtuele machine (VM) met behulp van de [az vm maken](/cli/azure/vm#az_vm_create) opdracht. Het volgende voorbeeld wordt een virtuele machine met de naam myVM. In dit voorbeeld wordt Demouser voor de naam van een gebruiker met beheerdersrechten en Demouser@123 als het wachtwoord van de gebruiker. Deze waarden te wijzigen in iets dat geschikt is voor uw omgeving.

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

Wanneer de virtuele machine wordt gemaakt, de **PublicIPAddress** parameter in de uitvoer bevat de openbare IP-adres voor de virtuele machine. Noteer dit adres omdat u deze toegang krijgt tot de virtuele machine nodig hebt.

## <a name="open-port-80-for-web-traffic"></a>Poort 80 openen voor webverkeer

Omdat deze virtuele machine uitvoeren van de IIS-webserver wilt, moet u poort 80 voor Internet-verkeer te openen.

Gebruik de [az vm open poort](/cli/azure/vm#open-port) opdracht poort 80 te openen.

```cli
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="connect-to-the-virtual-machine"></a>Verbinding maken met de virtuele machine

Gebruik de volgende opdracht voor het maken van een extern bureaublad-verbinding met uw virtuele machine. "Openbaar IP-adres" vervangen door het IP-adres van uw virtuele machine. Wanneer u wordt gevraagd, typt u de gebruikersnaam en het wachtwoord dat u voor de virtuele machine gebruikt.

```
mstsc /v <Public IP Address>
```

## <a name="install-iis-using-powershell"></a>IIS installeren met behulp van PowerShell

Nu dat u zich hebt aangemeld bij de virtuele machine, kunt u PowerShell gebruiken om IIS te installeren. Start PowerShell op de virtuele machine en voer de volgende opdracht:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-the-iis-welcome-page"></a>De welkomstpagina van IIS weergeven

U kunt een webbrowser van uw keuze om weer te geven van de standaardwelkomstpagina van IIS. Gebruik het openbare IP-adres is beschreven in de vorige sectie de standaardpagina te bezoeken.

![Standaardsite van IIS](./media/azure-stack-quick-create-vm-windows-cli/default-iis-website.png)

## <a name="clean-up-resources"></a>Resources opschonen

Opschonen van de resources die u niet langer nodig. Gebruik de [az groep verwijderen](/cli/azure/group#az_group_delete) opdracht voor het verwijderen van de resourcegroep, de virtuele machine, en alle bijbehorende resources.

```cli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids kunt u een eenvoudige virtuele machine voor Windows Server geïmplementeerd. Voor meer informatie over virtuele machines van Azure-Stack blijven [overwegingen voor virtuele Machines in Azure Stack](azure-stack-vm-considerations.md).
