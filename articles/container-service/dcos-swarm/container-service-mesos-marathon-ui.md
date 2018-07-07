---
title: Beheren Azure DC/OS-cluster met Marathon-gebruikersinterface
description: Implementeer containers naar een Azure Container Service-cluster met behulp van de webgebruikersinterface van Marathon.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 04/04/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: a22bddf48f97d961d481e2aedb42f7d645f3e678
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2018
ms.locfileid: "37903078"
---
# <a name="manage-an-azure-container-service-dcos-cluster-through-the-marathon-web-ui"></a>Een DC/OS-cluster in Azure Container Service beheren via de webgebruikersinterface van Marathon

DC/OS biedt een omgeving voor het implementeren en schalen van geclusterde workloads terwijl de onderliggende hardware wordt onttrokken. Op de DC/OS ligt een framework dat de planning en uitvoering van rekenworkloads regelt.

Er zijn frameworks beschikbaar voor veel populaire werkbelastingen, geeft dit document wordt beschreven hoe u aan de slag met Marathon-containers implementeren. 


## <a name="prerequisites"></a>Vereisten
Voer het uitvoeren van deze voorbeelden hebt u een DC/OS-cluster nodig dat is geconfigureerd in Azure Container Service. U hebt ook een externe verbinding met dit cluster nodig. Zie de volgende artikelen voor meer informatie over deze items:

* [Een Azure Container Service-cluster implementeren](container-service-deployment.md)
* [Verbinding maken met een Azure Container Service-cluster](../container-service-connect.md)

> [!NOTE]
> In dit artikel wordt ervan uitgegaan dat u naar het DC/OS-cluster zijn tunneling via uw lokale poort 80.
>

## <a name="explore-the-dcos-ui"></a>De DC/OS-gebruikersinterface verkennen
Een Secure Shell (SSH)-tunnel [tot stand gebracht](../container-service-connect.md), blader naar http://localhost/. Hierdoor wordt de DC/OS-webgebruikersinterface geladen met informatie over het cluster, zoals gebruikte resources, actieve agents en actieve services.

![DC/OS-webgebruikersinterface](./media/container-service-mesos-marathon-ui/dcos2.png)

## <a name="explore-the-marathon-ui"></a>De Marathon-gebruikersinterface verkennen
Als u wilt zien van de Marathon-gebruikersinterface, blader naar http://localhost/marathon. In dit scherm kunt u een nieuwe container of een andere toepassing starten op het DC/OS-cluster in Azure Container Service. U ziet ook informatie over actieve containers en toepassingen.  

![Marathon-gebruikersinterface](./media/container-service-mesos-marathon-ui/dcos3.png)

## <a name="deploy-a-docker-formatted-container"></a>Een met Docker ingedeelde container implementeren
Voor het implementeren van een nieuwe container via Marathon, klikt u op **Toepassing maken** en voert u de volgende gegevens op de tabbladen van het formulier in:

| Veld | Waarde |
| --- | --- |
| Id |nginx |
| Geheugen | 32 |
| Installatiekopie |nginx |
| Netwerk |Overbrugd |
| Hostpoort |80 |
| Protocol |TCP |

![Nieuwe toepassingsgebruikersinterface: algemeen](./media/container-service-mesos-marathon-ui/dcos4.png)

![Nieuwe toepassingsgebruikersinterface: Docker-container](./media/container-service-mesos-marathon-ui/dcos5.png)

![Nieuwe toepassingsgebruikersinterface: poort- en servicedetectie](./media/container-service-mesos-marathon-ui/dcos6.png)

Als u de containerpoort statisch wilt toewijzen aan een poort op de agent, gebruikt u de JSON-modus. U doet dit door de wizard Nieuwe toepassing over te schakelen naar de **JSON-modus** met behulp van de wisselknop. Voer daarna de volgende instelling in onder de sectie `portMappings` van de definitie van de toepassing. In dit voorbeeld wordt poort 80 van de container gebonden aan poort 80 van de DC/OS-agent. Nadat u deze wijziging hebt aangebracht, kunt u deze wizard uit JSON-modus halen.

```none
"hostPort": 80,
```

![Nieuwe toepassingsgebruikersinterface: voorbeeld van poort 80](./media/container-service-mesos-marathon-ui/dcos13.png)

Als u statuscontroles wilt inschakelen, stelt u een pad in op het tabblad **Statuscontroles**.

![Nieuwe toepassingsgebruikersinterface: statuscontroles](./media/container-service-mesos-marathon-ui/dcos_healthcheck.png)

Het DC/OS-cluster wordt geïmplementeerd met een set persoonlijke en openbare agents. Om te zorgen dat het cluster toegang heeft tot toepassingen vanaf internet, moet u de toepassingen implementeren naar een openbare agent. Hiertoe selecteert u het tabblad **Optioneel** van de wizard Nieuwe toepassing en voert u **slave_public** in bij de **Geaccepteerde resourcerollen**.

Klik vervolgens op **Toepassing maken**.

![Nieuwe toepassingsgebruikersinterface: instellingen openbare agent](./media/container-service-mesos-marathon-ui/dcos14.png)

Op de hoofdpagina van Marathon ziet u de implementatiestatus voor de container. Eerst wordt de status **Implementeren** weergegeven. Wanneer de implementatie is voltooid, verandert de status in **Uitvoeren**.

![Hoofdpagina van Marathon: implementatiestatus van container](./media/container-service-mesos-marathon-ui/dcos7.png)

Als u terugschakelt naar de DC/OS-webgebruikersinterface (http://localhost/), ziet u dat een taak (in dit geval een Docker container indeling) wordt uitgevoerd op het DC/OS-cluster.

![DC/OS-webgebruikersinterface: taak die wordt uitgevoerd op het cluster](./media/container-service-mesos-marathon-ui/dcos8.png)

U kunt het clusterknooppunt waarop de taak wordt uitgevoerd weergeven door op het tabblad **Knooppunten** te klikken.

![DC/OS-webgebruikersinterface: clusterknooppunt van taak](./media/container-service-mesos-marathon-ui/dcos9.png)

## <a name="reach-the-container"></a>De container bereiken

In dit voorbeeld wordt de toepassing uitgevoerd op een openbare agent-knooppunt. U de toepassing vanaf internet bereiken door te bladeren naar de FQDN-naam van het cluster-agent: `http://[DNSPREFIX]agents.[REGION].cloudapp.azure.com`, waarbij:

* **DNSPREFIX** het DNS-voorvoegsel is dat is opgegeven tijdens de implementatie van het cluster.
* **REGION** de regio is waarin uw resourcegroep zich bevindt.

    ![Nginx vanaf Internet](./media/container-service-mesos-marathon-ui/nginx.png)


## <a name="next-steps"></a>Volgende stappen
* [Werken met DC/OS en de Marathon API](container-service-mesos-marathon-rest.md)

* Gedetailleerde uitleg over Azure Container Service met Mesos

    > [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON203/player]
    > 
