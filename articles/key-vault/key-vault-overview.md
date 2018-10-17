---
title: Overzicht van Azure Key Vault| Microsoft Docs
description: Azure Key Vault is een cloudservice die werkt als een beveiligd geheimenarchief.
services: key-vault
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 34af20ee-3fa7-4f28-9d98-6168b1759764
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.date: 07/17/2018
ms.author: barclayn
ms.openlocfilehash: aae7836448ff27b4c80d7bb53e108034ee52db1c
ms.sourcegitcommit: 5843352f71f756458ba84c31f4b66b6a082e53df
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/01/2018
ms.locfileid: "47586288"
---
# <a name="what-is-azure-key-vault"></a>Wat is Azure Sleutelkluis?

Met Azure Key Vault kunt u de volgende problemen oplossen
- **Geheimenbeheer** - Met Azure Key Vault kunt u veilig de toegang tot tokens, wachtwoorden, certificaten, API-sleutels en andere geheimen opslaan en strikt beheren
- **Sleutelbeheer** - U kunt Azure Key Vault ook gebruiken als een oplossing voor sleutelbeheer. Met Azure Key Vault kunt u eenvoudig de versleutelingssleutels maken en beheren waarmee uw gegevens worden versleuteld. 
- **Certificaatbeheer** - Azure Key Vault is ook een service waarmee u eenvoudig openbare en persoonlijke SSL/TLS-certificaten (Secure Sockets Layer/Transport Layer Security) kunt inrichten, beheren en implementeren voor gebruik met Azure en uw interne verbonden bronnen. 
- **Sla geheimen op met hardwarebeveiligingsmodules (HSM's)** - De geheimen en sleutels kunnen worden beveiligd door software of met FIPS 140-2 niveau 2-validatie-HSM's

## <a name="why-use-azure-key-vault"></a>Waarom zou ik Azure Key Vault gebruiken?

### <a name="centralize-application-secrets"></a>Toepassingsgeheimen centraliseren

Door de opslag van toepassingsgeheimen te centraliseren in Azure Key Vault kunt u de verdeling ervan bepalen. Key Vault vermindert de kans dat geheimen per ongeluk worden gelekt aanzienlijk. Bij gebruik van Key Vault hoeven ontwikkelaars van toepassingen geen beveiligingsinformatie meer in de toepassing zelf op te slaan. Daardoor hoeft deze informatie geen deel van de code uit te maken. Een toepassing wil bijvoorbeeld verbinding maken met een database. In plaats van de verbindingsreeks op te slaan in de app-codes, bewaart u deze veilig in Key Vault.

Uw toepassingen hebben veilig toegang tot de gegevens die ze nodig hebben door gebruik te maken van URI's waarmee ze specifieke versies van een geheim kunnen ophalen nadat de sleutel of het geheim van de toepassing is opgeslagen in Azure Key Vault. Dit gebeurt zonder dat u aangepaste code moet schrijven om de geheime informatie te beveiligen.

### <a name="securely-store-secrets-and-keys"></a>Geheimen en sleutels veilig opslaan

Geheimen en sleutels worden beveiligd door Azure. Hiervoor wordt gebruikgemaakt van algoritmen, sleutellengten en HSM's (Hardware Security Modules) die voldoen aan de industriestandaard. De gebruikte HSM's zijn Federal Information Processing Standards (FIPS) 140-2 Level 2 gevalideerd.

Voor toegang tot een sleutelkluis is de juiste verificatie en autorisatie vereist voordat een aanroeper (gebruiker of toepassing) toegang kan krijgen. Met verificatie wordt de identiteit van de aanroeper vastgesteld, terwijl autorisatie bepaalt welke bewerkingen de aanroeper mag uitvoeren.

Verificatie wordt uitgevoerd via Azure Active Directory. Autorisatie kan worden uitgevoerd via op rollen gebaseerd toegangsbeheer (RBAC) of Key Vault-toegangsbeleid. RBAC wordt gebruikt bij het beheren van de kluizen. Toegangsbeleid tot sleutelkluizen wordt gebruikt bij pogingen om toegang te krijgen tot gegevens in een kluis.

Sleutelkluizen van Azure kunnen software- of hardware-HSM beveiligd zijn. Voor situaties waar extra zekerheid is vereist, kunt u sleutels in HSM's (Hardware Security Modules) importeren of genereren die nooit verdergaan dan de HSM-grens. Microsoft gebruikt Thales-hardwarebeveiligingsmodules. U kunt Thales-hulpprogramma's gebruiken om een sleutel te verplaatsen van uw HSM naar Azure Key Vault.

Tot slot is Azure Key Vault zodanig ontworpen dat Microsoft uw gegevens niet kan zien of extraheren.

### <a name="monitor-access-and-use"></a>Toegang tot en gebruik van controles

Nadat u enkele sleutelkluizen hebt gemaakt, kunt u controleren hoe en wanneer er toegang tot uw sleutels en geheimen is gezocht. U kunt dit doen door registratie voor Key Vault in te schakelen. U kunt Azure Key Vault voor het volgende configureren:

- Archiveren naar een opslagaccount.
- Streamen naar een Event Hub.
- De logboeken verzenden naar Log Analytics.

U hebt de controle over uw logboeken en kunt ze beveiligen door de toegang te beperken. Bovendien kunt u logboeken verwijderen die u niet meer nodig hebt.

### <a name="simplified-administration-of-application-secrets"></a>Vereenvoudigd beheer van toepassingsgeheimen

Bij het opslaan van waardevolle gegevens moet u verschillende stappen uitvoeren. Beveiligingsinformatie moet worden beschermd, moet een bepaalde levenscyclus volgen en moet maximaal beschikbaar zijn. Azure Key Vault vereenvoudigt veel hiervan door:

- Het wegnemen van de noodzaak tot interne kennis van Hardware Security Modules
- Het op korte termijn omhoog schalen om de gebruikspieken van uw organisatie aan te kunnen.
- Het repliceren van de inhoud van uw Key Vault binnen een regio en naar een secundaire regio. Dat zorgt voor een maximale beschikbaarheid en de beheerder hoeft geen actie te ondernemen om de failover te activeren.
- Het bieden van standaard Azure-beheeropties via de portal, Azure CLI en PowerShell.
- Het automatiseren van bepaalde taken voor certificaten die u aanschaft bij openbare CA's, zoals registreren en verlengen.

Bovendien kunt u met sleutelkluizen van Azure toepassingsgeheimen van elkaar scheiden. Toepassingen krijgen alleen toegang tot de kluis die ze mogen benaderen en ze mogen alleen specifieke bewerkingen uitvoeren. U kunt een Azure-sleutelkluis per toepassing maken en de geheimen die in een bepaalde sluitelkleus zijn opgeslagen beperken tot een specifieke toepassing en team van ontwikkelaars.

### <a name="integrate-with-other-azure-services"></a>Integreren met andere Azure-services

Key Vault wordt als een beveiligd archief in Azure gebruikt voor het vereenvoudigen van scenario's zoals [Azure Disk Encryption](../security/azure-security-disk-encryption.md), de [altijd versleuteld]( https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine)-functionaliteit in SQL Server en Azure SQL Database, en [Azure-webtoepassingen]( https://docs.microsoft.com/azure/app-service/web-sites-purchase-ssl-web-site). Key Vault zelf kan worden geïntegreerd met opslagaccounts, Event Hubs en Log Analytics.

## <a name="next-steps"></a>Volgende stappen

- [Snelstart: Een Azure-sleutelkluis maken met behulp van de CLI](quick-create-cli.md)
- [Een Azure-webtoepassing configureren zodat deze een geheim vanuit een sleutelkluis kan lezen](tutorial-web-application-keyvault.md)
