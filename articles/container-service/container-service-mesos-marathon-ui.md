---
title: Een Azure Container Service-cluster beheren met de Marathon-gebruikersinterface | Microsoft Docs
description: Implementeer containers naar een Azure Container Service-cluster met behulp van de webgebruikersinterface van Marathon.
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Docker, Containers, Micro-services, Mesos, Azure
ms.assetid: d148ed1e-b582-4d51-944f-1ac7ae3c4fd6
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/02/2016
ms.author: danlep
translationtype: Human Translation
ms.sourcegitcommit: c472963048e842e795e17df39567a8380d8543a9
ms.openlocfilehash: b076fabd964cfb583bb4049d1373268d0d2762fb


---
# <a name="manage-an-azure-container-service-dcos-cluster-through-the-marathon-web-ui"></a>Een DC/OS-cluster in Azure Container Service beheren via de webgebruikersinterface van Marathon
DC/OS biedt een omgeving voor het implementeren en schalen van geclusterde workloads terwijl de onderliggende hardware wordt onttrokken. Op de DC/OS ligt een framework dat de planning en uitvoering van rekenworkloads regelt.

Er zijn frameworks beschikbaar voor veel populaire werkbelastingen, maar in dit document wordt specifiek gekeken naar het maken en schalen van containerimplementaties met Marathon. 


## <a name="prerequisites"></a>Vereisten
Voer het uitvoeren van deze voorbeelden hebt u een DC/OS-cluster nodig dat is geconfigureerd in Azure Container Service. U hebt ook een externe verbinding met dit cluster nodig. Zie de volgende artikelen voor meer informatie over deze items:

* [Een Azure Container Service-cluster implementeren](container-service-deployment.md)
* [Verbinding maken met een Azure Container Service-cluster](container-service-connect.md)

## <a name="explore-the-dcos-ui"></a>De DC/OS-gebruikersinterface verkennen
Wanneer een SSH-tunnel (Secure Shell) is [ingesteld](container-service-connect.md), gaat u naar http://localhost/. Hierdoor wordt de DC/OS-webgebruikersinterface geladen met informatie over het cluster, zoals gebruikte resources, actieve agents en actieve services.

![DC/OS-webgebruikersinterface](./media/dcos/dcos2.png)

## <a name="explore-the-marathon-ui"></a>De Marathon-gebruikersinterface verkennen
Ga naar http://localhost/Marathon om de Marathon-gebruikersinterface weer te geven. In dit scherm kunt u een nieuwe container of een andere toepassing starten op het DC/OS-cluster in Azure Container Service. U ziet ook informatie over actieve containers en toepassingen.  

![Marathon-gebruikersinterface](./media/dcos/dcos3.png)

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

![Nieuwe toepassingsgebruikersinterface: algemeen](./media/dcos/dcos4.png)

![Nieuwe toepassingsgebruikersinterface: Docker-container](./media/dcos/dcos5.png)

![Nieuwe toepassingsgebruikersinterface: poort- en servicedetectie](./media/dcos/dcos6.png)

Als u de containerpoort statisch wilt toewijzen aan een poort op de agent, gebruikt u de JSON-modus. U doet dit door de wizard Nieuwe toepassing over te schakelen naar de **JSON-modus** met behulp van de wisselknop. Voer daarna de volgende instelling in onder de sectie `portMappings` van de definitie van de toepassing. In dit voorbeeld wordt poort 80 van de container gebonden aan poort 80 van de DC/OS-agent. Nadat u deze wijziging hebt aangebracht, kunt u deze wizard uit JSON-modus halen.

```none
"hostPort": 80,
```

![Nieuwe toepassingsgebruikersinterface: voorbeeld van poort 80](./media/dcos/dcos13.png)

Als u statuscontroles wilt inschakelen, stelt u een pad in op het tabblad **Statuscontroles**.

![Nieuwe toepassingsgebruikersinterface: statuscontroles](./media/dcos/dcos_healthcheck.png)

Het DC/OS-cluster wordt geïmplementeerd met een set persoonlijke en openbare agents. Om te zorgen dat het cluster toegang heeft tot toepassingen vanaf internet, moet u de toepassingen implementeren naar een openbare agent. Hiertoe selecteert u het tabblad **Optioneel** van de wizard Nieuwe toepassing en voert u **slave_public** in bij de **Geaccepteerde resourcerollen**.

Klik vervolgens op **Toepassing maken**.

![Nieuwe toepassingsgebruikersinterface: instellingen openbare agent](./media/dcos/dcos14.png)

Op de hoofdpagina van Marathon ziet u de implementatiestatus voor de container. Eerst wordt de status **Implementeren** weergegeven. Wanneer de implementatie is voltooid, verandert de status in **Uitvoeren**.

![Hoofdpagina van Marathon: implementatiestatus van container](./media/dcos/dcos7.png)

Als u terugschakelt naar de DC/OS-webgebruikersinterface (http://localhost/), ziet u dat een taak (in dit geval een container in Docker-indeling) wordt uitgevoerd op het DC/OS-cluster.

![DC/OS-webgebruikersinterface: taak die wordt uitgevoerd op het cluster](./media/dcos/dcos8.png)

U kunt het clusterknooppunt waarop de taak wordt uitgevoerd weergeven door op het tabblad **Knooppunten** te klikken.

![DC/OS-webgebruikersinterface: clusterknooppunt van taak](./media/dcos/dcos9.png)

## <a name="scale-your-containers"></a>Uw containers schalen
U kunt de Marathon-gebruikersinterface gebruiken om het aantal exemplaren van een container te schalen. Als u dit wilt doen, gaat u naar de pagina **Marathon**, selecteert u de container die u wilt schalen en klikt u op **Toepassing schalen**. In het dialoogvenster **Toepassing schalen** voert u het aantal gewenste containerexemplaren in en klikt u op **Toepassing schalen**.

![Marathon-gebruikersinterface: dialoogvenster Toepassing schalen](./media/dcos/dcos10.png)

Nadat de bewerking voor het aanpassen van de schaal is voltooid, ziet u meerdere exemplaren van dezelfde taak verdeeld over DC/OS-agents.

![Dashboard in DC/OS-webgebruikersinterface: taak verspreid over agents](./media/dcos/dcos11.png)

![DC/OS-webgebruikersinterface: knooppunten](./media/dcos/dcos12.png)

## <a name="next-steps"></a>Volgende stappen
* [Werken met DC/OS en de Marathon API](container-service-mesos-marathon-rest.md)

* Gedetailleerde uitleg over Azure Container Service met Mesos

    > [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON203/player]
    > 



<!--HONumber=Dec16_HO1-->


