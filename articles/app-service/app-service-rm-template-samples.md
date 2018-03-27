---
title: Voorbeeldsjablonen van Azure Resource Manager - App Service | Microsoft Docs
description: Voorbeelden van Azure Resource Manager-sjablonen voor de functie Web Apps van App Service
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
ms.openlocfilehash: 155b47fc4c664701ec0f21bdc5ae34f3d7f666ff
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="azure-resource-manager-templates-for-web-apps"></a>Azure Resource Manager-sjablonen voor Web Apps

De volgende tabel bevat koppelingen naar Azure Resource Manager-sjablonen voor de functie Web Apps van Azure App Service. Zie [Hulp bij het implementeren van web-apps met Azure Resource Manager-sjablonen](web-sites-rm-template-guidance.md) voor aanbevelingen om veelvoorkomende fouten te vermijden bij het maken van web-app-sjablonen.

| | |
|-|-|
|**Een web-app implementeren**||
| [Deploy a Web App linked to a GitHub repository](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-github-deploy) (Een web-app implementeren die is gekoppeld aan een GitHub-opslagplaats)| Hiermee wordt een Azure-web-app geïmplementeerd die code van GitHub haalt. |
| [Web app with custom deployment slots](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-custom-deployment-slots) (Web-app met aangepaste implementatiesites)| Hiermee wordt een Azure-web-app met aangepaste implementatiesites/-omgevingen geïmplementeerd. |
|**Een web-app configureren**||
| [Web app certificate from Key Vault](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-certificate-from-key-vault) (Web-app-certificaat uit Key Vault)| Hiermee wordt een Azure-web-app-certificaat uit het Azure Key Vault-geheim geïmplementeerd en voor SSL-binding gebruikt. |
| [Web app with a custom domain](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-custom-domain) (Web-app met aangepast domein)| Hiermee wordt een Azure-web-app met een aangepaste hostnaam geïmplementeerd. |
| [Web app with custom domain and SSL](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-custom-domain-and-ssl) (Web-app met aangepast domein en SSL)| Hiermee wordt een Azure-web-app met een aangepaste hostnaam geïmplementeerd en wordt een web-app-certificaat uit Key Vault opgehaald voor SSL-binding. |
| [Web app with a GoLang extension](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-with-golang) (Web-app met een GoLang-extensie)| Hiermee wordt een Azure-web-app met de GoLang-site-extensie geïmplementeerd. Daarna kunt u webtoepassingen uitvoeren die zijn ontwikkeld in GoLang op Azure. |
| [Web app with Java 8 and Tomcat 8](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-java-tomcat) (Web-app met Java 8 en Tomcat 8)| Hiermee wordt een Azure-web-app geïmplementeerd waarvoor Java 8 en Tomcat 8 zijn ingeschakeld. Daarna kunt u Java-toepassingen uitvoeren in Azure. |
|**Linux-web-app**||
| [Web app on Linux with MySQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-linux-managed-mysql) (Web-app op Linux met MySQL) | Hiermee wordt een Azure-web-app geïmplementeerd op Linux met Azure Database for MySQL. |
| [Web app on Linux with PostgreSQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-linux-managed-postgresql) (Web-app op Linux met PostgreSQL) | Hiermee wordt een Azure-web-app geïmplementeerd op Linux met Azure Database for PostgreSQL. |
|**Web-app met verbonden resources**||
| [Web app with MySQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-managed-mysql) (Web-app met MySQL)| Hiermee wordt een Azure-web-app geïmplementeerd op Windows met Azure Database for MySQL. |
| [Web app with PostgreSQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-managed-postgresql) (Web-app met PostgreSQL)| Hiermee wordt een Azure-web-app geïmplementeerd op Windows met Azure Database for PostgreSQL. |
| [Web app with a SQL database](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-sql-database) (Web-app met een SQL-database)| Hiermee worden een Azure-web-app en een SQL-database geïmplementeerd op serviceniveau Basic. |
| [Web app with a Blob storage connection](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-blob-connection) (Web-app met een Blobopslag-verbinding)| Hiermee wordt een Azure-web-app geïmplementeerd met een verbindingsreeks voor Azure Blobopslag. Daarna kunt u Blobopslag gebruiken vanuit de web-app. |
| [Web app with Redis cache](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-with-redis-cache) (Web-app met Redis-cache)| Hiermee wordt een Azure-web-app met een Redis-cache geïmplementeerd. |
|**App Service Environment voor PowerApps**||
| [Create an App Service environment v2](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-asev2-create)(Een App Service-omgeving v2 maken) | Hiermee wordt een App Service-omgeving v2 in uw virtuele netwerk gemaakt. |
| [Create an App Service environment v2 with an ILB Address](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-asev2-ilb-create/) (Een App Service-omgeving v2 met een ILB-adres (interne load balancer) maken) | Hiermee wordt een App Service-omgeving v2 in uw virtuele netwerk gemaakt met een privéadres voor de interne load balancer. |
| [Configure the default SSL certificate for an ILB App Service environment or an ILB App Service environment v2](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-ase-ilb-configure-default-ssl) (Het standaard-SSL-certificaat configureren voor een ILB App Service-omgeving of een ILB App Service-omgeving v2) | Hiermee wordt het standaard-SSL-certificaat geconfigureerd voor een ILB App Service-omgeving of een ILB App Service-omgeving v2. |
| | |
