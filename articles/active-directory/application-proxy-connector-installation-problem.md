---
title: Problemen bij het installeren van de Agent Connector voor toepassingsproxy | Microsoft Docs
description: Het oplossen van problemen die u mee te maken mogelijk bij het installeren van de Connector voor toepassingsproxy-Agent
services: active-directory
documentationcenter: 
author: ajamess
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 8fbd707b6708661ab0d655afadff2b18694a981e
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/13/2018
---
# <a name="problem-installing-the-application-proxy-agent-connector"></a>Problemen bij het installeren van de Connector voor toepassingsproxy-Agent

Microsoft AAD Application Proxy Connector is een onderdeel van het interne domein die gebruikmaakt van uitgaande verbindingen tot stand brengen van de verbinding van de cloud beschikbaar eindpunt met het interne domein.

## <a name="general-problem-areas-with-connector-installation"></a>Algemene probleemgebieden met installatie van de Connector

Wanneer de installatie van een connector is mislukt, is de hoofdoorzaak meestal een van de volgende gebieden:

1.  **Connectiviteit** – voor het voltooien van een geslaagde installatie, de nieuwe connector moet registreren en toekomstige vertrouwensgegevens tot stand brengen. Dit wordt gedaan door verbinding te maken met de cloudservice AAD Application Proxy.

2.  **Vertrouwensrelatie tot stand brengen** : de nieuwe connector maakt u een zelfondertekend certificaat en registreert met de cloudservice.

3.  **Verificatie van de beheerder** – tijdens de installatie van de gebruiker beheerdersreferenties om de installatie van de Connector te voltooien moet opgeven.

## <a name="verify-connectivity-to-the-cloud-application-proxy-service-and-microsoft-login-page"></a>Controleer of de verbinding met de toepassingsproxy van Cloud-service en Microsoft Login pagina

**Doelstelling:** Controleer of de machine connector verbinding met het eindpunt voor AAD Application Proxy-registratie, alsmede de aanmeldingspagina voor Microsoft maken kan.

1.  Open een browser en Ga naar de volgende webpagina: <https://aadap-portcheck.connectorporttest.msappproxy.net> , en controleren of de verbinding met VS-midden en VS-Oost datacenters met poorten 80 en 443 werkt.

2.  Als een van deze poorten is niet geslaagd (geen een groen vinkje), Controleer of de Firewall of een back-end-proxy heeft \*. msappproxy.net met poorten 80 en 443 correct gedefinieerd.

3.  Open een browser (door tabs gescheiden) en Ga naar de volgende webpagina: <https://login.microsoftonline.com>, zorg ervoor dat u op deze pagina aanmelden zich.

## <a name="verify-machine-and-backend-components-support-for-application-proxy-trust-cert"></a>Controleer of de Machine- en back-end-onderdelen ondersteund voor toepassingsproxy vertrouwensrelatie cert

**Doelstelling:** controleren of de connector-machine, back-end-proxy en firewall het certificaat dat is gemaakt door de connector voor toekomstige vertrouwensrelatie kunnen ondersteunen.

>[!NOTE]
>De connector probeert te maken van een certificaat SHA512 gebruikt die wordt ondersteund door TLS1.2. Als de machine of de back endfirewall en proxy biedt geen ondersteuning voor TLS1.2, mislukt de installatie.
>
>

**Het probleem oplossen:**

1.  Controleer of de computer ondersteunt TLS1.2 – TLS 1.2 moeten ondersteuning voor alle Windows-versies na 2012 R2. Als uw computer connector van een versie van 2012 R2 of vóór, zorg ervoor dat zijn de volgende KB op de computer geïnstalleerd: <https://support.microsoft.com/help/2973337/sha512-is-disabled-in-windows-when-you-use-tls-1.2>

2.  Neem contact op met de beheerder van uw netwerk en vraagt u om te controleren dat de back-end-proxy- en firewallservers niet SHA512 voor uitgaand verkeer blokkeren.

## <a name="verify-admin-is-used-to-install-the-connector"></a>Controleer of de beheerder wordt gebruikt om de connector te installeren

**Doelstelling:** Controleer of de gebruiker die probeert te installeren van de connector is een beheerder met de juiste referenties. Op dit moment wordt moet de gebruiker een globale beheerder voor de installatie mislukt.

**Om te controleren of zijn de referenties correct:**

Verbinding maken met <https://login.microsoftonline.com> en dezelfde referenties gebruikt. Zorg ervoor dat de aanmelding is geslaagd. U kunt de gebruikersrol controleren door te gaan naar **Azure Active Directory**  - &gt; **gebruikers en groepen**  - &gt; **alle gebruikers**. 

Selecteer uw gebruikersaccount, klikt u vervolgens 'Directory rol' in het resulterende menu. Controleer of de geselecteerde rol 'Globale beheerder'. Als u geen toegang tot alle pagina's langs deze stappen zijn, bent u niet een globale beheerder.

## <a name="next-steps"></a>Volgende stappen
[Azure AD-toepassingsproxy connectors begrijpen](application-proxy-understand-connectors.md)
