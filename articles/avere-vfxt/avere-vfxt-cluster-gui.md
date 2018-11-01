---
title: Toegang tot het Configuratiescherm Avere vFXT - Azure
description: Verbinding maken met het cluster vFXT en de browser gebaseerde Avere het Configuratiescherm om de vFXT Avere configureren
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: c48f0d8f7ad34db585f4deae566641b6453357e8
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50633985"
---
# <a name="access-the-vfxt-cluster"></a>Toegang tot het cluster vFXT

Instellingen wijzigen en de Avere vFXT-cluster bewaken, gebruik Avere in het Configuratiescherm. Configuratiescherm Avere is een browser gebaseerde grafische interface voor het cluster.

Omdat het cluster vFXT zich binnen een virtueel particulier netwerk bevindt, moet u een SSH-tunnel maken of een andere methode gebruiken om te bereiken van beheer van IP-adres van het cluster. Er zijn twee eenvoudige stappen: 

1. Maak een verbinding tussen uw werkstation en het privé-vnet 
1. Beheer van IP-adres van het cluster gebruiken om te laden van het Configuratiescherm in een webbrowser 

> [!NOTE] 
> In dit artikel wordt ervan uitgegaan dat u een openbaar IP-adres op de clustercontroller of op een andere virtuele machine in het virtuele netwerk van uw cluster hebt ingesteld. Als u een VPN of ExpressRoute voor vnet-toegang gebruikt, gaat u naar [verbinding maken met het Configuratiescherm Avere](#connect-to-the-avere-control-panel-in-a-browser).

Voordat u verbinding maakt, zorg ervoor dat het SSH openbaar/persoonlijk sleutelpaar die u hebt gebruikt bij het maken van de clustercontroller is geïnstalleerd op uw lokale computer. Raadpleeg de documentatie van de SSH-sleutels bij [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys) of voor [Windows](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows) als u hulp nodig hebt.  

## <a name="access-with-a-linux-host"></a>Toegang met een Linux-host

met dit formulier in: 

SSH -L *local_port*:*cluster_mgmt_ip*: 443 *controller_username*@*controller_public_IP* 

Deze opdracht maakt verbinding met van het cluster management IP-adres via het IP-adres van de domeincontroller van het cluster.

Voorbeeld:

```sh
ssh -L 8443:10.0.0.5:443 azureuser@203.0.113.51
```

Verificatie wordt automatisch als u uw openbare SSH-sleutel gebruikt om het cluster te maken en de overeenkomende sleutel is geïnstalleerd op het clientsysteem.


## <a name="access-with-a-windows-host"></a>Toegang met een Windows-host

Als u PuTTY gebruikt, vult u de **hostnaam** veld met de gebruikersnaam van het cluster netwerkcontroller en het IP-adres: *your_username*@*controller_public_IP*.

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

Verificatie wordt automatisch als u uw openbare SSH-sleutel gebruikt om het cluster te maken en de overeenkomende sleutel is geïnstalleerd op het clientsysteem.

## <a name="connect-to-the-avere-control-panel-in-a-browser"></a>Verbinding maken met het Configuratiescherm Avere in een browser

Deze stap maakt gebruik van een webbrowser verbinding maken met het hulpprogramma voor configuratie uitgevoerd op het cluster vFXT.

Open uw webbrowser en navigeer naar https://127.0.0.1:8443. 

Afhankelijk van uw browser, moet u mogelijk op **Geavanceerd** en controleer of dat het is veilig om door te gaan naar de pagina.

Voer de gebruikersnaam `admin` en het wachtwoord die u hebt opgegeven bij het maken van het cluster.

![Schermafbeelding van de Avere aanmeldingspagina gevuld met de gebruikersnaam 'admin' en een wachtwoord](media/avere-vfxt-gui-login.png)

Klik op **aanmelding** of druk op enter op het toetsenbord.

## <a name="next-steps"></a>Volgende stappen

Nu dat u toegang het cluster tot kunt, de mogelijkheid [ondersteunen](avere-vfxt-enable-support.md).
