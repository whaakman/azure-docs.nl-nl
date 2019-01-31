---
title: Toegang tot het Configuratiescherm Avere vFXT - Azure
description: Verbinding maken met het cluster vFXT en de browser gebaseerde Avere het Configuratiescherm om de vFXT Avere configureren
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: 30c03d52e31f70448eef07b4567083061605d8dd
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55300469"
---
# <a name="access-the-vfxt-cluster"></a>Toegang tot het cluster vFXT

Instellingen wijzigen en de Avere vFXT-cluster bewaken, gebruik Avere in het Configuratiescherm. Configuratiescherm Avere is een browser gebaseerde grafische interface voor het cluster.

Omdat het cluster vFXT zich binnen een virtueel particulier netwerk bevindt, moet u een SSH-tunnel maken of een andere methode gebruiken om te bereiken van beheer van IP-adres van het cluster. Er zijn twee eenvoudige stappen: 

1. Maak een verbinding tussen uw werkstation en het privé-vnet 
1. Laden van het Configuratiescherm van het cluster in een webbrowser 

> [!NOTE] 
> In dit artikel wordt ervan uitgegaan dat u een openbaar IP-adres op de clustercontroller of op een andere virtuele machine in het virtuele netwerk van uw cluster hebt ingesteld. In dit artikel wordt beschreven hoe u die virtuele machine gebruiken als host voor toegang tot het cluster. Als u een VPN of ExpressRoute voor vnet-toegang gebruikt, gaat u naar [verbinding maken met het Configuratiescherm Avere](#connect-to-the-avere-control-panel-in-a-browser).

Voordat u verbinding maakt, zorg ervoor dat het SSH openbaar/persoonlijk sleutelpaar die u hebt gebruikt bij het maken van de clustercontroller is geïnstalleerd op uw lokale computer. Raadpleeg de documentatie van de SSH-sleutels bij [Windows](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows) of voor [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys) als u hulp nodig hebt. (Als u een wachtwoord in plaats van een openbare sleutel gebruikt, u wordt gevraagd te voeren wanneer u verbinding maakt.) 

## <a name="ssh-tunnel-with-a-linux-host"></a>SSH-tunnel met een Linux-host

Als een client op basis van Linux, gebruikt u een SSH-opdracht met dit formulier tunneling: 

ssh -L *local_port*:*cluster_mgmt_ip*:443 *controller_username*@*controller_public_IP*

Deze opdracht maakt verbinding met van het cluster management IP-adres via het IP-adres van de domeincontroller van het cluster.

Voorbeeld:

```sh
ssh -L 8443:10.0.0.5:443 azureuser@203.0.113.51
```

Verificatie wordt automatisch als u uw openbare SSH-sleutel gebruikt om het cluster te maken en de overeenkomende sleutel is geïnstalleerd op het clientsysteem. Als u een wachtwoord gebruikt, wordt u in te voeren door het systeem gevraagd.

## <a name="ssh-tunnel-with-a-windows-host"></a>SSH-tunnel met een Windows-host

In dit voorbeeld wordt de algemene Windows-terminal hulpprogramma, PuTTY.

Vul in de PuTTY **hostnaam** veld met de gebruikersnaam van het cluster netwerkcontroller en het IP-adres: *your_username*@*controller_public_IP*.

Voorbeeld: ``azureuser@203.0.113.51``

In de **configuratie** Configuratiescherm:

1. Vouw **verbinding** > **SSH** aan de linkerkant. 
1. Klik op **Tunnels**. 
1. Geef een bron-poort, zoals 8443. 
1. Voer voor de bestemming van het cluster vFXT beheer van IP-adres en poort 443. 
   Voorbeeld: ``203.0.113.51:443``
1. Klik op **Add**.
1. Klik op **Openen**.

![Schermafbeelding van de Putty-toepassing die laat zien waar op om toe te voegen een tunnel te klikken](media/avere-vfxt-ptty-numbered.png)

Verificatie wordt automatisch als u uw openbare SSH-sleutel gebruikt om het cluster te maken en de overeenkomende sleutel is geïnstalleerd op het clientsysteem. Als u een wachtwoord gebruikt, wordt u in te voeren door het systeem gevraagd.

## <a name="connect-to-the-avere-control-panel-in-a-browser"></a>Verbinding maken met het Configuratiescherm Avere in een browser

Deze stap maakt gebruik van een webbrowser verbinding maken met het hulpprogramma voor configuratie uitgevoerd op het cluster vFXT.

* Voor de verbinding van een SSH-tunnel, open uw webbrowser en navigeer naar https://127.0.0.1:8443. 

  U verbinding met het cluster-IP-adres tijdens het maken van de tunnel, zodat u alleen wilt gebruiken van het localhost IP-adres in de browser. Als u een andere lokale poort dan 8443 gebruikt, in plaats daarvan uw poortnummer gebruiken.

* Als u een VPN of ExpressRoute gebruikt voor het bereiken van het cluster, gaat u naar het cluster-IP-adres voor beheer in uw browser. Voorbeeld: ``https://203.0.113.51``

Afhankelijk van uw browser, moet u mogelijk op **Geavanceerd** en controleer of dat het is veilig om door te gaan naar de pagina.

Voer de gebruikersnaam `admin` en het beheerderswachtwoord dat u hebt opgegeven bij het maken van het cluster.

![Schermafbeelding van de Avere aanmeldingspagina gevuld met de gebruikersnaam 'admin' en een wachtwoord](media/avere-vfxt-gui-login.png)

Klik op **aanmelding** of druk op enter op het toetsenbord.

## <a name="next-steps"></a>Volgende stappen

Nu dat u toegang het cluster tot kunt, de mogelijkheid [ondersteunen](avere-vfxt-enable-support.md).
