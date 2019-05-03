---
title: Azure Blockchain-Service-beveiliging
description: Azure Blockchain-Service gegevens toegang en beveiliging-concepten
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: seal
manager: femila
ms.openlocfilehash: dd0a33364ed9395a85478798e47352c533bd47dc
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028198"
---
# <a name="azure-blockchain-service-security"></a>Azure Blockchain-Service-beveiliging

Azure Blockchain-Service maakt gebruik van verschillende mogelijkheden van Azure voor uw gegevens veilig en beschikbaar. Gegevens beveiligd met behulp van isolatie, versleuteling en verificatie.

## <a name="isolation"></a>Isolatie

Azure Blockchain-Service-resources worden geïsoleerd in een virtueel privénetwerk. Elk knooppunt en validatie aan de transactie is een virtuele machine (VM). Virtuele machines in één virtueel netwerk kunnen niet communiceren rechtstreeks op virtuele machines in een ander virtueel netwerk. Isolatie zorgt ervoor dat communicatie blijft privé binnen het virtuele netwerk. Zie voor meer informatie over Azure-netwerkisolatie [isolatie in de openbare Cloud van Azure](../../security/azure-isolation.md#networking-isolation).

![VNET-diagram](./media/data-security/vnet.png)

## <a name="encryption"></a>Versleuteling

Gebruikersgegevens zijn opgeslagen in Azure storage. Gebruikersgegevens worden versleuteld in beweging en at-rest voor beveiliging en vertrouwelijkheid. Zie voor meer informatie: [Azure Storage-beveiligingshandleiding](../../storage/common/storage-security-guide.md).

## <a name="authentication"></a>Verificatie

Transacties kunnen worden verzonden naar blockchain-knooppunten via een RPC-eindpunt. Clients communiceren met een transactie-knooppunt via een reverse proxy-server die gebruikersverificatie en versleutelt de gegevens via SSL.

![Verificatiediagram](./media/data-security/authentication.png)

Er zijn drie modi van verificatie van RPC-toegang.

### <a name="basic-authentication"></a>Basisverificatie

Basisverificatie maakt gebruik van een HTTP-verificatie-header met de gebruikersnaam en wachtwoord. Gebruikersnaam is de naam van de blockchain-knooppunt. Wachtwoord wordt ingesteld tijdens het inrichten van een lid of een knooppunt. Het wachtwoord kan worden gewijzigd met behulp van de Azure portal of de CLI.

### <a name="access-keys"></a>Toegangssleutels

Sneltoetsen gebruiken een willekeurige tekenreeks die is opgenomen in de eindpunt-URL. Twee toegangssleutels helpen sleutelroulatie inschakelen. Sleutels kunnen worden hersteld vanuit de Azure portal en CLI.

### <a name="azure-active-directory"></a>Azure Active Directory

Azure Active Directory (Azure AD) maakt gebruik van een op claims gebaseerde verificatiemechanisme waarbij de gebruiker is geverifieerd door Azure AD met behulp van Azure AD-gebruikersreferenties. Azure AD biedt cloudgebaseerde identiteitsbeheer en kan klanten één identiteit gebruiken voor een hele onderneming en toegang tot toepassingen in de cloud. Azure Blockchain-Service kan worden geïntegreerd met Azure AD ID federation, eenmalige aanmelding en meervoudige verificatie inschakelen. U kunt gebruikers, groepen en toepassingsrollen toewijzen in uw organisatie voor toegang tot blockchain lid en het knooppunt.

De proxy van de client Azure AD is beschikbaar op [GitHub](https://github.com/Microsoft/azure-blockchain-connector/releases). De clientproxy leidt de gebruiker naar de aanmeldingspagina van Azure AD en haalt een bearer-token bij een geslaagde verificatie. De gebruiker maakt vervolgens een Ethereum-clienttoepassing zoals Geth of Truffle verbinding met de clientproxy-eindpunt. Ten slotte, als een transactie is ingediend, de clientproxy injects het bearer-token in de http-header en de omgekeerde proxy valideert het token met behulp van OAuth-protocol.

## <a name="keys-and-ethereum-accounts"></a>Sleutels en Ethereum-accounts

Bij het inrichten van een lid van Azure Blockchain-Service, wordt een Ethereum-account en een openbare en persoonlijke sleutelpaar gegenereerd. De persoonlijke sleutel wordt gebruikt voor het verzenden van transacties naar de blockchain. Het Ethereum-account is de laatste 20 bytes van de openbare sleutel hash. De Ethereum-account wordt ook een portefeuille genoemd.

De persoonlijke en openbare sleutelpaar wordt opgeslagen als een sleutelbestand in JSON-indeling. De persoonlijke sleutel is versleuteld met behulp van het wachtwoord hebt ingevoerd toen de blockchain grootboek-service is gemaakt.

Persoonlijke sleutels worden gebruikt om digitaal te ondertekenen transacties. In private blockchains vertegenwoordigt een slimme overeenkomst ondertekend door een persoonlijke sleutel van de ondertekenaar identiteit. Om te controleren of de geldigheid van de handtekening, kan de ontvanger de openbare sleutel van de ondertekenaar vergelijken met het adres van de handtekening wordt berekend.

Constellation sleutels worden gebruikt voor het aanduiden van een Quorum-knooppunt. Constellation sleutels worden gegenereerd op het moment van het knooppunt wordt ingericht en zijn opgegeven in de parameter privateFor van een persoonlijke transactie in Quorum.

## <a name="next-steps"></a>Volgende stappen

[Azure Blockchain Service transactie knooppunten configureren](configure-transaction-nodes.md)
