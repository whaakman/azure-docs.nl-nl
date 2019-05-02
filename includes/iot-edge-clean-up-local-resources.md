---
title: bestand opnemen
description: bestand opnemen
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 08/10/2018
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: 2b61cc8c5c448c28e96b06afa3556688a82567ed
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2019
ms.locfileid: "64866814"
---
### <a name="delete-local-resources"></a>Lokale resources verwijderen

Als u de IoT Edge-runtime en de bijbehorende resources van uw apparaat wilt verwijderen, moet u de juiste opdrachten gebruiken voor het besturingssysteem van uw apparaat. 

#### <a name="windows"></a>Windows

De IoT Edge-runtime verwijderen.

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Uninstall-SecurityDaemon
   ```

Wanneer de IoT Edge-runtime is verwijderd, worden de containers die deze heeft gemaakt gestopt. Ze worden echter niet van uw apparaat verwijderd. Geef alle containers weer.

   ```powershell
   docker ps -a
   ```

Verwijder de runtime-containers die op uw apparaat zijn gemaakt.

   ```powershell
   docker rm -f edgeHub
   docker rm -f edgeAgent
   ```

Verwijder alle aanvullende containers die zijn vermeld in de `docker ps` uitvoer door te verwijzen naar de namen van containers. 

#### <a name="linux"></a>Linux

Verwijder de IoT Edge-runtime.

   ```bash
   sudo apt-get remove --purge iotedge
   ```

Wanneer de IoT Edge-runtime is verwijderd, worden de containers die deze heeft gemaakt gestopt. Ze worden echter niet van uw apparaat verwijderd. Geef alle containers weer.

   ```bash
   sudo docker ps -a
   ```

Verwijder de runtime-containers die op uw apparaat zijn gemaakt.

   ```bash
   docker rm -f edgeHub
   docker rm -f edgeAgent
   ```

Verwijder alle aanvullende containers die zijn vermeld in de `docker ps` uitvoer door te verwijzen naar de namen van containers. 

Verwijder de container-runtime.

   ```bash
   sudo apt-get remove --purge moby
   ```