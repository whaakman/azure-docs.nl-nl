---
title: Parameters van de Server in Azure-Database configureren voor MySQL | Microsoft Docs
description: In dit artikel wordt beschreven hoe serverparameters MySQL in Azure-Database configureren voor MySQL met de Azure portal.
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 10/10/2017
ms.openlocfilehash: 06c7f9f6bd49ebfaf03b04cb6e30b963593bfb35
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2017
---
# <a name="how-to-configure-server-parameters-in-azure-database-for-mysql-by-using-the-azure-portal"></a>Parameters van de server in Azure-Database configureren voor MySQL met behulp van de Azure-portal

Azure MySQL-Database ondersteunt de configuratie van bepaalde parameters van de server. In dit onderwerp wordt beschreven hoe deze parameters configureren met behulp van de Azure-portal. Niet alle parameters van de server kunnen worden aangepast. 

## <a name="navigate-to-server-parameters-on-azure-portal"></a>Navigeer naar de Parameters van de Server op Azure-portal
1. Meld u aan bij de Azure portal en zoek uw Azure-Database voor de MySQL-server.
2. Onder de **instellingen** sectie, klikt u op **serverparameters** de pagina de parameters voor de Azure-Database voor MySQL openen.
3. Zoek alle instellingen die u wilt aanpassen. Controleer de **beschrijving** kolom om te begrijpen van het doel en de toegestane waarden. 
4. Klik op **opslaan** uw wijzigingen op te slaan.

![De blade parameters Azure portal-server](./media/howto-server-parameters/auzre-portal-server-parameters.png)

## <a name="list-of-configurable-server-parameters"></a>Lijst met parameters van de server worden geconfigureerd

De lijst met ondersteunde serverparameters groeit voortdurend. Gebruik het tabblad van de parameters-server in Azure-portal voor de definitie en parameters van de server op basis van de toepassingsvereisten van uw configureren. 

## <a name="nonconfigurable-server-parameters"></a>Serverparameters Nonconfigurable

De volgende parameters zijn niet configureerbaar en gekoppeld aan uw [prijscategorie](concepts-service-tiers.md). 

| **Prijscategorie** | **InnoDB buffergroep (MB)** | **Maximum aantal verbindingen** |
| :------------------------ | :-------- | :----------- |
| Basic 50 | 1024 | 50 | 
| Basic 100  | 2560 | 100 | 
| Standaard 100 | 2560 | 200 | 
| Standaard 200 | 5120 | 400 | 
| Standaard 400 | 10240 | 800 | 
| Standaard 800 | 20480 | 1600 |

Andere server Parameterstandaardwaarden voor versie [5.7](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html) en [5.6](https://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html).

## <a name="next-steps"></a>Volgende stappen
- [Verbindingsbibliotheken voor Azure-Database voor MySQL](concepts-connection-libraries.md).
