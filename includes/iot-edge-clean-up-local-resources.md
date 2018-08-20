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
ms.openlocfilehash: cdb60ffb3ba3c31c336c8b74c46621792c707f74
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/11/2018
ms.locfileid: "40047009"
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