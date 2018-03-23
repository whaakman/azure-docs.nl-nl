---
title: 'Quickstart: een persoonlijk Docker-register in Azure maken met Azure Portal'
description: Leer snel een persoonlijk Docker-containerregister maken met Azure Portal.
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: quickstart
ms.date: 03/03/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: db112f7f8f486093509a86f9781c30133925c25f
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="create-a-container-registry-using-the-azure-portal"></a>Een containerregister maken met Azure Portal

Een Azure-containerregister is een persoonlijk Docker-register in Azure waar u uw persoonlijke installatiekopieën van de Docker-container kunt opslaan en beheren. In deze snelstart gaat u een containerregister maken met Azure Portal, een containerinstallatiekopie naar het register pushen en ten slotte de container vanuit het register in Azure Container Instances (ACI) implementeren.

Als u het register met deze quickstart wilt voltooien, moet Docker lokaal zijn geïnstalleerd. Docker biedt pakketten die eenvoudig Docker configureren op elk [Mac][docker-mac]-, [Windows][docker-windows]- of [Linux][docker-linux]-systeem.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij Azure Portal op https://portal.azure.com.

## <a name="create-a-container-registry"></a>Een containerregister maken

Selecteer **Een resource maken** > **Containers** > **Azure Container Registry**.

![Een containerregister maken met Azure Portal][qs-portal-01]

Voer waarden in voor **Registernaam** en **Resourcegroep**. De registernaam moet uniek zijn binnen Azure en mag 5 tot 50 alfanumerieke tekens bevatten. Maak een nieuwe resourcegroep met de naam `myResourceGroup`, en selecteer 'Basic' voor **SKU**. Selecteer **Maken** om de ACR-instantie te implementeren.

![Een containerregister maken met Azure Portal][qs-portal-03]

In deze quickstart maken we een *Basic*-register. Azure Container Registry is beschikbaar in verschillende SKU's, zoals kort beschreven in de onderstaande tabel. Zie [SKU's voor containerregisters][container-registry-skus] voor uitgebreide details.

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

Voordat u installatiekopieën van containers gaat pushen en pullen, moet u zich aanmelden bij het ACR-exemplaar. Gebruik hiervoor de opdracht [docker login][docker-login]. Vervang de waarden *username*, *password* en *login server* door de waarden die u in de vorige stap hebt genoteerd (respectievelijk gebruikersnaam, wachtwoord en aanmeldingsserver).

```bash
docker login --username <username> --password <password> <login server>
```

De opdracht retourneert `Login Succeeded` nadat deze is voltooid. Mogelijk ziet u een waarschuwing die het gebruik van de parameter `--password-stdin` aanraadt. Hoewel buiten het bestek van dit artikel, wordt u deze best practice aangeraden. Zie de verwijzing voor de opdracht [docker login][docker-login] voor meer informatie.

## <a name="push-image-to-acr"></a>Installatiekopie naar ACR overdragen met een push-bewerking

Als u een installatiekopie naar uw Azure Container Registry wilt pushen, moet u eerst over een installatiekopie beschikken. Voer zo nodig de volgende opdracht uit om een bestaande installatiekopie vanuit Docker Hub te 'pullen'.

```bash
docker pull microsoft/aci-helloworld
```

Voorzie de installatiekopie van een label met de naam van de ACR-aanmeldingsserver voordat u de kopie naar het register pusht. Label de installatiekopie met de opdracht [docker tag][docker-tag]. Vervang *login server* door de naam van de aanmeldingsserver die u eerder hebt genoteerd.

```bash
docker tag microsoft/aci-helloworld <login server>/aci-helloworld:v1
```

Gebruik ten slotte [docker push][docker-push] om de installatiekopie naar de ACR-instantie te pushen. Vervang *login server* door de naam van de ACR-instantie.

```bash
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

## <a name="deploy-image-to-aci"></a>Installatiekopie implementeren naar ACI

Om vanuit het register te implementeren naar een exemplaar, moeten we naar de opslagplaats (aci-helloworld) navigeren en vervolgens op het weglatingsteken naast v1 klikken.

![Een Azure Container exemplaar starten vanuit de portal][qs-portal-10]

Selecteer **Uitvoeringsinstantie** in het contextmenu dat wordt weergegeven:

![Contextmenu voor starten van ACI][qs-portal-11]

Geef een waarde op voor **Containernaam**, zorg ervoor dat het juiste abonnement is geselecteerd, selecteer bij **Resourcegroep** de bestaande groep myResourceGroup en klik vervolgens op **OK** om het Azure Container-exemplaar te starten.

![Implementatieopties voor starten van ACI][qs-portal-12]

Wanneer de implementatie is gestart, verschijnt er een tegel op uw portaldashboard waarop de voortgang van de implementatie wordt weergegeven. Wanneer de implementatie is voltooid, wordt de tegel bijgewerkt met de nieuwe containergroep **mycontainer**.

![Implementatiestatus van ACI][qs-portal-13]

Selecteer de containergroep mycontainer om de eigenschappen van de container weer te geven. Noteer het **IP-adres** van de containergroep, evenals de **STATUS** van de container.

![Details van ACI-container][qs-portal-14]

## <a name="view-the-application"></a>De toepassing weergeven

Als de container de status **Actief** heeft, gaat u in uw browser naar het IP-adres dat u in de vorige stap hebt genoteerd om de toepassing weer te geven.

![Hello world-app in browser][qs-portal-15]

## <a name="clean-up-resources"></a>Resources opschonen

Als u uw resources wilt opschonen, navigeert u naar de resourcegroep **myResourceGroup** in de portal. Nadat de resourcegroep is geladen, klikt u op **Resourcegroep verwijderen** om de resourcegroep te verwijderen, evenals het containerregister van Azure en alle exemplaren van het register.

![Een containerregister maken met Azure Portal][qs-portal-08]

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een Azure-containerregister gemaakt met de Azure CLI en een exemplaar van het register gestart via Azure Container Instances. Ga verder met de zelfstudie voor Azure Container Instances om meer te leren over ACI.

> [!div class="nextstepaction"]
> [Zelfstudies voor Azure Container Instances][container-instances-tutorial-prepare-app]

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
[qs-portal-10]: ./media/container-registry-get-started-portal/qs-portal-10.png
[qs-portal-11]: ./media/container-registry-get-started-portal/qs-portal-11.png
[qs-portal-12]: ./media/container-registry-get-started-portal/qs-portal-12.png
[qs-portal-13]: ./media/container-registry-get-started-portal/qs-portal-13.png
[qs-portal-14]: ./media/container-registry-get-started-portal/qs-portal-14.png
[qs-portal-15]: ./media/container-registry-get-started-portal/qs-portal-15.png

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - internal -->
[container-instances-tutorial-prepare-app]: ../container-instances/container-instances-tutorial-prepare-app.md
[container-registry-skus]: container-registry-skus.md
