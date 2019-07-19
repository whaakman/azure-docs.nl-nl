---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/15/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 39b61c78cbd0333183c80234256d6041a109d846
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68249919"
---
Een Azure AD-Resource-ID geeft de doel groep aan waarvoor een token dat wordt uitgegeven, kan worden gebruikt om toegang te bieden tot een Azure-resource. In het geval van Azure Storage kan de resource-ID specifiek zijn voor één opslag account of van toepassing op elk opslag account. In de volgende tabel worden de waarden beschreven die u kunt opgeven voor de resource-ID:

|Resource-ID  |Description  |
|---------|---------|
|`https://<account>.blob.core.windows.net` <br /><br /> `https://<account>.queue.core.windows.net`    | Het service-eind punt voor een gegeven opslag account. Gebruik deze waarde om een token te verkrijgen voor het goed keuren van aanvragen voor het specifieke Azure Storage account en de service. Vervang de waarde tussen haakjes door de naam van uw opslag account.      |
|`https://storage.azure.com/`     | Gebruik om een token te verkrijgen voor het machtigen van aanvragen voor een Azure Storage-account.        |
