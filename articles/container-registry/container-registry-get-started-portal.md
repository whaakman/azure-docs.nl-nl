---
title: Quick Start - een persoonlijke Docker-register in Azure maken met de Azure portal
description: Snel informatie over het maken van een persoonlijke register van de Docker-container met de Azure-portal.
services: container-registry
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
tags: 
keywords: 
ms.assetid: 53a3b3cb-ab4b-4560-bc00-366e2759f1a1
ms.service: container-registry
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/31/2017
ms.author: marsma
ms.custom: 
ms.openlocfilehash: 514fa3490e480647f0923c99bd9606a3726d4d30
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="create-a-container-registry-using-the-azure-portal"></a>Een containerregister maken met Azure Portal

Een Azure container-register is een persoonlijke Docker-register in Azure kunt u opslaan en beheren van uw persoonlijke installatiekopieën van de Docker-container. In deze snelstartgids kunt u het register van een container maken met de Azure-portal.

U moet voor het voltooien van deze snelstartgids Docker lokaal geïnstalleerd hebben. Docker biedt pakketten die eenvoudig Docker op elke configureren op elk [Mac](https://docs.docker.com/docker-for-mac/)-, [Windows](https://docs.docker.com/docker-for-windows/)- of [Linux](https://docs.docker.com/engine/installation/#supported-platforms)-systeem.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Aanmelden bij de Azure portal op https://portal.azure.com.

## <a name="create-a-container-registry"></a>Een containerregister maken

Selecteer **nieuwe** > **Containers** > **Azure Container register**.

![Een register container maken in de Azure portal][qs-portal-01]

Voer waarden in voor **naam van Routeringsregister** en **resourcegroep**. De registernaam moet uniek zijn binnen Azure en 5 50 alfanumerieke tekens bevatten. Maak een nieuwe resourcegroep met de naam `myResourceGroup`, en voor **SKU**, selecteert u 'Basic'. Selecteer **maken** voor het implementeren van het ACR-exemplaar.

![Een register container maken in de Azure portal][qs-portal-03]

In deze snelstartgids maken we een *Basic* register. Azure Container register is beschikbaar in diverse verschillende SKU's, een korte beschrijving van de volgende tabel. Zie voor uitgebreide informatie over elk [Container register SKU's](container-registry-skus.md).

[!INCLUDE [container-registry-sku-matrix](../../includes/container-registry-sku-matrix.md)]

Wanneer de **implementatie is voltooid** bericht wordt weergegeven, selecteert u het register van de container in de portal en selecteer vervolgens **toegangssleutels**.

![Een register container maken in de Azure portal][qs-portal-05]

Onder **gebruiker met beheerdersrechten**, selecteer **inschakelen**. Noteer de volgende waarden:

* Login-server
* Gebruikersnaam
* wachtwoord

U kunt deze waarden in de volgende stappen gebruiken tijdens het werken met het register met de Docker CLI.

![Een register container maken in de Azure portal][qs-portal-06]

## <a name="log-in-to-acr"></a>Aanmelden bij ACR

Voordat u installatiekopieën van containers gaat pushen en pullen, moet u zich aanmelden bij het ACR-exemplaar. Gebruik hiervoor de [docker aanmelding](https://docs.docker.com/engine/reference/commandline/login/) opdracht. Vervang de *gebruikersnaam*, *wachtwoord*, en *login-server* waarden met die in de vorige stap hebt genoteerd.

```bash
docker login --username <username> --password <password> <login server>
```

De opdracht retourneert `Login Succeeded` eenmaal is voltooid. U ziet misschien ook een beveiligingswaarschuwing aanbevelen van het gebruik van de `--password-stdin` parameter. Hoewel het gebruik ervan buiten het bereik van dit artikel is, wordt aangeraden na deze aanbevolen procedure. Zie de [docker aanmelding](https://docs.docker.com/engine/reference/commandline/login/) -opdrachten voor meer informatie.

## <a name="push-image-to-acr"></a>Push-installatiekopie naar ACR

Voor het pushen van een installatiekopie toe aan het register van de Container Azure, moet u eerst een afbeelding hebben. Indien nodig, voer de volgende opdracht voor het ophalen van een bestaande installatiekopie van Docker-Hub.

```bash
docker pull microsoft/aci-helloworld
```

Voordat u de installatiekopie aan het register pushen, moet u de installatiekopie met de naam van de ACR aanmelding labelen. Label van de installatiekopie met de [docker-tag](https://docs.docker.com/engine/reference/commandline/tag/) opdracht. Vervang *login-server* met de aanmeldings-servernaam die u eerder hebt genoteerd.

```
docker tag microsoft/aci-helloworld <login server>/aci-helloworld:v1
```

Gebruik tot slot [docker push](https://docs.docker.com/engine/reference/commandline/push/) voor de push-installatiekopie van het naar het ACR-exemplaar. Vervang *login-server* met de aanmeldingsnaam van de server van uw ACR-exemplaar.

```
docker push <login server>/aci-helloworld:v1
```

De uitvoer van een geslaagde `docker push` opdracht is vergelijkbaar met:

```
The push refers to a repository [uniqueregistryname.azurecr.io/aci-helloworld]
7c701b1aeecd: Pushed
c4332f071aa2: Pushed
0607e25cc175: Pushed
d8fbd47558a8: Pushed
44ab46125c35: Pushed
5bef08742407: Pushed
v1: digest: sha256:f2867748615cc327d31c68b1172cc03c0544432717c4d2ba2c1c2d34b18c62ba size: 1577
```

## <a name="list-container-images"></a>Containerinstallatiekopieën opvragen

Als de afbeeldingen in uw ACR-exemplaar wilt weergeven, gaat u naar het register in de portal en selecteer **opslagplaatsen**, selecteert u de opslagplaats die u hebt gemaakt met `docker push`.

In dit voorbeeld selecteren we de **aci helloworld** opslagplaats en we kunnen zien de `v1`-TIF onder **labels**.

![Een register container maken in de Azure portal][qs-portal-09]

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, verwijdert u de **myResourceGroup** resourcegroep. In dat geval wordt de resourcegroep, ACR-exemplaar en alle container afbeeldingen worden verwijderd.

![Een register container maken in de Azure portal][qs-portal-08]

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids kunt u een Azure Container Registry gemaakt met de Azure CLI. Als u gebruiken van Azure Container register met exemplaren van Azure-Container wilt, blijven de zelfstudie exemplaren van Azure-Container.

> [!div class="nextstepaction"]
> [Zelfstudies voor Azure Container Instances](../container-instances/container-instances-tutorial-prepare-app.md)

<!-- IMAGES -->
[qs-portal-01]: ./media/container-registry-get-started-portal/qs-portal-01.png
[qs-portal-02]: ./media/container-registry-get-started-portal/qs-portal-02.png
[qs-portal-03]: ./media/container-registry-get-started-portal/qs-portal-03.png
[qs-portal-04]: ./media/container-registry-get-started-portal/qs-portal-04.png
[qs-portal-05]: ./media/container-registry-get-started-portal/qs-portal-05.png
[qs-portal-06]: ./media/container-registry-get-started-portal/qs-portal-06.png
[qs-portal-07]: ./media/container-registry-get-started-portal/qs-portal-07.png
[qs-portal-08]: ./media/container-registry-get-started-portal/qs-portal-08.png
[qs-portal-09]: ./media/container-registry-get-started-portal/qs-portal-09.png