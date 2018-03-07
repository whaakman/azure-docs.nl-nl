---
title: Voorbeeldsjablonen van Azure Resource Manager - App Service | Microsoft Docs
description: Voorbeeldsjablonen van Azure Resource Manager - App Service
services: app-service
documentationcenter: app-service
author: tfitzmac
manager: timlt
editor: ggailey777
tags: azure-service-management
ms.service: app-service
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: app-service
ms.date: 02/26/2018
ms.author: tomfitz
ms.custom: mvc
ms.openlocfilehash: e28a27b9a00164fcbba6eb5d3e5435548486ffa4
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2018
---
# <a name="azure-resource-manager-templates-for-azure-web-apps"></a>Sjablonen van Azure Resource Manager voor Azure Web Apps

De volgende tabel bevat koppelingen naar Azure Resource Manager-sjablonen. Zie [Hulp bij het implementeren van web-apps met Azure Resource Manager-sjablonen](web-sites-rm-template-guidance.md) voor aanbevelingen om veelvoorkomende fouten te vermijden bij het maken van web-app-sjablonen.

| | |
|-|-|
|**Web-app implementeren**||
| [Web App linked to a GitHub repository](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-github-deploy) (Web-app gekoppeld aan een GitHub-opslagplaats)| Hiermee wordt een Azure-web-app geïmplementeerd die code van GitHub haalt. |
| [Web App with custom deployment slots](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-custom-deployment-slots) (Web-app met aangepaste implementatiesites)| Hiermee wordt een Azure-web-app met aangepaste implementatiesites/-omgevingen geïmplementeerd. |
|**Web-app configureren**||
| [Web App certificate from Key Vault](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-certificate-from-key-vault) (Web-app-certificaat uit Key Vault)| Hiermee wordt een Azure-web-app-certificaat uit het Key Vault-geheim geïmplementeerd en voor SSL-binding gebruikt. |
| [Web App with custom domain](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-custom-domain) (Web-app met aangepast domein)| Hiermee wordt een Azure-web-app met een aangepaste hostnaam geïmplementeerd. |
| [Web App with custom domain and SSL](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-custom-domain-and-ssl) (Web-app met aangepast domein en SSL)| Hiermee wordt een Azure-web-app met een aangepaste hostnaam geïmplementeerd en wordt een web-app-certificaat uit Key Vault opgehaald voor SSL-binding. |
| [Web App with GoLang extension](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-with-golang) (Web-app met GoLang-extensie)| Hiermee wordt een Azure-web-app met GoLang-site-extensie geïmplementeerd, waarmee u webtoepassingen kunt uitvoeren die zijn ontwikkeld met GoLang in Azure. |
| [Web App with Java 8 and Tomcat 8](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-java-tomcat) (Web-app met Java 8 en Tomcat 8)| Hiermee wordt een Azure-web-app met Java 8 en Tomcat 8 geactiveerd, waarmee u Java-toepassingen in Azure kunt uitvoeren. |
|**Linux-web-app**||
| [Web App on Linux with MySQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-linux-managed-mysql) (Web-app op Linux met MySQL) | Hiermee wordt een Azure-web-app geïmplementeerd op Linux met een Azure-database voor MySQL. |
| [Web App on Linux with PostgreSQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-linux-managed-postgresql) (Web-app op Linux met PostgreSQL) | Hiermee wordt een Azure-web-app geïmplementeerd op Linux met een Azure-database voor PostgreSQL. |
|**Web-app met verbonden resources**||
| [Web App with MySQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-managed-mysql) (Web-app met MySQL)| Hiermee wordt een Azure-web-app geïmplementeerd op Windows met een Azure-database voor MySQL. |
| [Web App with PostgreSQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-managed-postgresql) (Web-app met PostgreSQL)| Hiermee wordt een Azure-web-app geïmplementeerd op Windows met een Azure-database voor PostgreSQL. |
| [Web App with a SQL Database](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-sql-database) (Web-app met een SQL-database)| Hiermee worden een Azure-web-app en SQL-database geïmplementeerd op serviceniveau Basic. |
| [Web App with Blob Storage connection](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-blob-connection) (Web-app met Blob Storage-verbinding)| Hiermee wordt een Azure-web-app met een Blob Storage-verbindingsreeks geïmplementeerd, waardoor u Azure Blob Storage kunt gebruiken vanuit de web-app. |
| [Web App with Redis Cache](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-with-redis-cache) (Web-app met Redis Cache)| Hiermee wordt een Azure-web-app met Redis Cache geïmplementeerd. |
|**App Service-omgeving**||
| [Create an App Service Environment v2](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-asev2-create)(Een App Service Environment v2 maken) | Hiermee wordt een App Service Environment v2 in uw virtuele netwerk gemaakt. |
| [Create an App Service Environment v2 with an ILB Address](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-asev2-ilb-create/) (Een App Service Environment v2 met een adres voor de interne load balancer maken) | Hiermee wordt een App Service Environment v2 in uw virtuele netwerk gemaakt met een privéadres voor de interne load balancer. |
| [Configure the Default SSL Certificate for an ILB ASE or an ILB ASE v2](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-ase-ilb-configure-default-ssl) (Het standaard-SSL-certificaat configureren voor een App Service Environment met interne load balancer of een App Service Environment met interne load balancer v2) | Hiermee wordt het standaard-SSL-certificaat geconfigureerd voor een App Service Environment met interne load balancer of een App Service Environment met interne load balancer v2 |
| | |
