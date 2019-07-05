---
title: Toegang tot het Configuratiescherm Avere vFXT - Azure
description: Verbinding maken met het cluster vFXT en de browser gebaseerde Avere het Configuratiescherm om de vFXT Avere configureren
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 06/24/2019
ms.author: v-erkell
ms.openlocfilehash: 830be92d37f304598cca05c3ac80973158c38a59
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67439971"
---
# <a name="access-the-vfxt-cluster"></a>Toegang tot het cluster vFXT

Instellingen wijzigen en de Avere vFXT-cluster bewaken, gebruik Avere in het Configuratiescherm. Configuratiescherm Avere is een browser gebaseerde grafische interface voor het cluster.

Omdat het cluster vFXT zich binnen een virtueel particulier netwerk bevindt, moet u een SSH-tunnel maken of een andere methode gebruiken om te bereiken van beheer van IP-adres van het cluster. Er zijn twee eenvoudige stappen: 

1. Maak een verbinding tussen uw werkstation en het privé-vnet 
1. Laden van het Configuratiescherm van het cluster in een webbrowser 

> [!NOTE] 
> In dit artikel wordt ervan uitgegaan dat u een openbaar IP-adres op de clustercontroller of op een andere virtuele machine in het virtuele netwerk van uw cluster hebt ingesteld. In dit artikel wordt beschreven hoe u die virtuele machine gebruiken als host voor toegang tot het cluster. Als u een VPN of ExpressRoute voor vnet-toegang gebruikt, gaat u naar [verbinding maken met het Configuratiescherm Avere](#connect-to-the-avere-control-panel-in-a-browser).

Voordat u verbinding maakt, zorg ervoor dat het SSH openbaar/persoonlijk sleutelpaar die u hebt gebruikt bij het maken van de clustercontroller is geïnstalleerd op uw lokale computer. Raadpleeg de documentatie van de SSH-sleutels bij [Windows](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows) of voor [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys) als u hulp nodig hebt. (Als u een wachtwoord in plaats van een openbare sleutel gebruikt, u wordt gevraagd te voeren wanneer u verbinding maakt.) 

## <a name="create-an-ssh-tunnel"></a>Een SSH-tunnel maken 

U kunt een SSH-tunnel vanaf de opdrachtregel van een-op basis van Linux of Windows 10 clientsysteem maken. 

Een SSH-opdracht met dit formulier tunneling gebruiken: 

ssh -L *local_port*:*cluster_mgmt_ip*:443 *controller_username*\@*controller_public_IP*

Deze opdracht maakt verbinding met van het cluster management IP-adres via het IP-adres van de domeincontroller van het cluster.

Voorbeeld:

```sh
ssh -L 8443:10.0.0.5:443 azureuser@203.0.113.51
```

Verificatie wordt automatisch als u uw openbare SSH-sleutel gebruikt om het cluster te maken en de overeenkomende sleutel is geïnstalleerd op het clientsysteem. Als u een wachtwoord gebruikt, wordt u in te voeren door het systeem gevraagd.

## <a name="connect-to-the-avere-control-panel-in-a-browser"></a>Verbinding maken met het Configuratiescherm Avere in een browser

Deze stap maakt gebruik van een webbrowser verbinding maken met het hulpprogramma voor configuratie uitgevoerd op het cluster vFXT.

* Voor de verbinding van een SSH-tunnel, open uw webbrowser en navigeer naar `https://127.0.0.1:8443`. 

  U verbinding met het cluster-IP-adres tijdens het maken van de tunnel, zodat u alleen wilt gebruiken van het localhost IP-adres in de browser. Als u een andere lokale poort dan 8443 gebruikt, in plaats daarvan uw poortnummer gebruiken.

* Als u een VPN of ExpressRoute gebruikt voor het bereiken van het cluster, gaat u naar het cluster-IP-adres voor beheer in uw browser. Voorbeeld: ``https://203.0.113.51``

Afhankelijk van uw browser, moet u mogelijk op **Geavanceerd** en controleer of dat het is veilig om door te gaan naar de pagina.

Voer de gebruikersnaam `admin` en het beheerderswachtwoord dat u hebt opgegeven bij het maken van het cluster.

![Schermafbeelding van de Avere aanmeldingspagina gevuld met de gebruikersnaam 'admin' en een wachtwoord](media/avere-vfxt-gui-login.png)

Klik op **aanmelding** of druk op enter op het toetsenbord.

## <a name="next-steps"></a>Volgende stappen

Nu dat u toegang het cluster tot kunt, de mogelijkheid [ondersteunen](avere-vfxt-enable-support.md).
