---
title: 'Quickstart: een persoonlijk Docker-register in Azure maken met Azure Portal'
description: Leer snel een persoonlijk Docker-containerregister maken met Azure Portal.
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
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="create-a-container-registry-using-the-azure-portal"></a>Een containerregister maken met Azure Portal

Een Azure-containerregister is een persoonlijk Docker-register in Azure waar u uw persoonlijke installatiekopieën van de Docker-container kunt opslaan en beheren. In deze quickstart maakt u een containerregister in Azure Portal.

Als u het register met deze quickstart wilt voltooien, moet Docker lokaal zijn geïnstalleerd. Docker biedt pakketten die eenvoudig Docker op elke configureren op elk [Mac](https://docs.docker.com/docker-for-mac/)-, [Windows](https://docs.docker.com/docker-for-windows/)- of [Linux](https://docs.docker.com/engine/installation/#supported-platforms)-systeem.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij Azure Portal op https://portal.azure.com.

## <a name="create-a-container-registry"></a>Een containerregister maken

Selecteer **Nieuw** > **Containers** > **Azure Container Registry**.

![Een containerregister maken met Azure Portal][qs-portal-01]

Voer waarden in voor **Registernaam** en **Resourcegroep**. De registernaam moet uniek zijn binnen Azure en mag 5 tot 50 alfanumerieke tekens bevatten. Maak een nieuwe resourcegroep met de naam `myResourceGroup`, en selecteer 'Basic' voor **SKU**. Selecteer **Maken** om de ACR-instantie te implementeren.

![Een containerregister maken met Azure Portal][qs-portal-03]

In deze quickstart maken we een *Basic*-register. Azure Container Registry is beschikbaar in verschillende SKU's, zoals kort beschreven in de onderstaande tabel. Zie [SKU's voor containerregisters](container-registry-skus.md) voor uitgebreide details.

[!INCLUDE [container-registry-sku-matrix](../../includes/container-registry-sku-matrix.md)]

Als het bericht **Implementatie voltooid** wordt weergegeven, selecteert u het containerregister in de portal en vervolgens **Toegangstoetsen**.

![Een containerregister maken met Azure Portal][qs-portal-05]

Selecteer onder **Gebruiker met beheerdersrechten** de optie **Inschakelen**. Noteer de volgende waarden:

* Aanmeldingsserver
* Gebruikersnaam
* wachtwoord

Deze waarden gebruikt u in de volgende stappen bij het werken met uw register met de Docker-opdrachtregelinterface.

![Een containerregister maken met Azure Portal][qs-portal-06]

## <a name="log-in-to-acr"></a>Aanmelden bij ACR

Voordat u installatiekopieën van containers gaat pushen en pullen, moet u zich aanmelden bij het ACR-exemplaar. Gebruik hiervoor de opdracht [docker login](https://docs.docker.com/engine/reference/commandline/login/). Vervang de waarden *username*, *password* en *login server* door de waarden die u in de vorige stap hebt genoteerd (respectievelijk gebruikersnaam, wachtwoord en aanmeldingsserver).

```bash
docker login --username <username> --password <password> <login server>
```

De opdracht retourneert `Login Succeeded` nadat deze is voltooid. Mogelijk ziet u een waarschuwing die het gebruik van de parameter `--password-stdin` aanraadt. Hoewel buiten het bestek van dit artikel, wordt u deze best practice aangeraden. Zie de verwijzing voor de opdracht [docker login](https://docs.docker.com/engine/reference/commandline/login/) voor meer informatie.

## <a name="push-image-to-acr"></a>Installatiekopie naar ACR overdragen met een push-bewerking

Als u een installatiekopie naar uw Azure Container Registry wilt pushen, moet u eerst over een installatiekopie beschikken. Voer zo nodig de volgende opdracht uit om een bestaande installatiekopie vanuit Docker Hub te 'pullen'.

```bash
docker pull microsoft/aci-helloworld
```

Voorzie de installatiekopie van een label met de naam van de ACR-aanmeldingsserver voordat u de kopie naar het register pusht. Label de installatiekopie met de opdracht [docker tag](https://docs.docker.com/engine/reference/commandline/tag/). Vervang *login server* door de naam van de aanmeldingsserver die u eerder hebt genoteerd.

```
docker tag microsoft/aci-helloworld <login server>/aci-helloworld:v1
```

Gebruik ten slotte [docker push](https://docs.docker.com/engine/reference/commandline/push/) om de installatiekopie naar de ACR-instantie te pushen. Vervang *login server* door de naam van de ACR-instantie.

```
docker push <login server>/aci-helloworld:v1
```

De uitvoer van een geslaagde `docker push`-opdracht is soortgelijk aan:

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

Als u de installatiekopieën in uw ACR-instantie wilt weergeven, gaat u naar het register in de portal en selecteert u **Opslagplaatsen**. Selecteer vervolgens de opslagplaats die u met `docker push` hebt gemaakt.

In dit voorbeeld selecteren we de opslagplaats **aci helloworld** en zien we de met `v1` gelabelde installatiekopie onder **TAGS**.

![Een containerregister maken met Azure Portal][qs-portal-09]

## <a name="clean-up-resources"></a>Resources opschonen

Verwijder de resourcegroep **myResourceGroup** als u deze niet meer nodig hebt. Als u dat doet, worden ook de resourcegroep, de ACR-instantie en alle containerinstallatiekopieën verwijderd.

![Een containerregister maken met Azure Portal][qs-portal-08]

## <a name="next-steps"></a>Volgende stappen

In deze quickstart hebt u een Azure Container Registry met de Azure-opdrachtregelinterface gemaakt. Als u Azure Container Registry wilt gebruiken met Azure Container Instances, gaat u door naar de zelfstudie Azure Container Instances.

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