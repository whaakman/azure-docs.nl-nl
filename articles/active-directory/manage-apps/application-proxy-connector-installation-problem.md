---
title: Problemen bij het installeren van de Toepassingsproxyagent | Microsoft Docs
description: Het oplossen van problemen die u tegenkomen kan bij het installeren van de Toepassingsproxyagent
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: celested
ms.reviewer: japere
ms.openlocfilehash: ee2a0b9f731c9fbf7dc2fa248f7b3e9a3de3bc72
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55963316"
---
# <a name="problem-installing-the-application-proxy-agent-connector"></a>Probleem bij het installeren van de connector voor de toepassingsproxyagent

Microsoft AAD Application Proxy Connector is een intern Domeinonderdeel die gebruikmaakt van uitgaande verbindingen tot stand brengen van de verbinding van de beschikbare cloud-eindpunt met het interne domein.

## <a name="general-problem-areas-with-connector-installation"></a>Algemene probleemgebieden met installatie van Connector

Wanneer de installatie van een connector is mislukt, is de hoofdoorzaak meestal een van de volgende gebieden:

1.  **Connectiviteit** – om uit te voeren van een geslaagde installatie, de behoeften van de nieuwe connector te registreren en eigenschappen van de toekomstige vertrouwensrelatie tot stand brengen. Dit wordt gedaan door verbinding te maken met de cloudservice AAD Application Proxy.

2.  **Vertrouwensrelatie tot stand brengen** : de nieuwe connector maakt u een zelfondertekend certificaat en geregistreerd met de cloudservice.

3.  **Verificatie van de beheerder** – tijdens de installatie van de gebruiker beheerdersreferenties om de installatie van de Connector te voltooien moet opgeven.

## <a name="verify-connectivity-to-the-cloud-application-proxy-service-and-microsoft-login-page"></a>Controleer de verbinding met de Cloud Application Proxy-service en Microsoft Login-pagina

**Doel:** Controleer of dat de connector-machine verbinding met het eindpunt van het AAD Application Proxy, evenals de Microsoft-aanmeldingspagina maken kunt.

1.  Open een browser en Ga naar de volgende webpagina: <https://aadap-portcheck.connectorporttest.msappproxy.net> , en controleer of de verbinding met VS-centraal en VS-Oost-datacenters met de poorten 80 en 443 werkt.

2.  Als een van deze poorten is mislukt (geen heeft een groen vinkje), Controleer of de Firewall of een back-end-proxy heeft \*. msappproxy.net met poorten 80 en 443 correct gedefinieerd.

3.  Open een browser (gescheiden door tabs) en Ga naar de volgende webpagina: <https://login.microsoftonline.com>, zorg ervoor dat u op die pagina aanmelden zich.

## <a name="verify-machine-and-backend-components-support-for-application-proxy-trust-cert"></a>Controleer of de Machine en back-end-onderdelen ondersteunen voor toepassingsproxy vertrouwensrelatie certificaat

**Doel:** Controleer of de connector-machine, back-end-proxy en firewall het certificaat dat is gemaakt door de connector voor toekomstige vertrouwensrelatie kunnen ondersteunen.

>[!NOTE]
>De connector probeert te maken van een certificaat SHA512 gebruikt die wordt ondersteund door TLS1.2. Als de machine of de back-end-firewall en proxy wordt niet ondersteund voor TLS1.2, mislukt de installatie.
>
>

**Het probleem kunt oplossen:**

1.  Controleer of de computer ondersteunt TLS1.2: alle Windows-versies na 2012 R2 moeten ondersteuning bieden voor TLS 1.2. Als uw connector-computer van een versie van 2012 R2 of een voorafgaand is, zorg ervoor dat het volgende kB's worden geïnstalleerd op de machine: <https://support.microsoft.com/help/2973337/sha512-is-disabled-in-windows-when-you-use-tls-1.2>

2.  Neem contact op met uw netwerk-beheerder en vraag om te controleren dat de back-end-proxy en firewall niet SHA512 gebruikt voor uitgaand verkeer blokkeren.

## <a name="verify-admin-is-used-to-install-the-connector"></a>Controleer of de beheerder wordt gebruikt om de connector te installeren

**Doel:** Controleer of de gebruiker die probeert te installeren van de connector is een beheerder met de juiste referenties. Op dit moment moet de gebruiker een beheerder van de toepassing of globale beheerder voor de installatie te voltooien.

**Om te controleren of dat de referenties correct zijn:**

Verbinding maken met <https://login.microsoftonline.com> en gebruik de dezelfde referenties. Zorg ervoor dat de aanmelding is geslaagd. U kunt de gebruikersrol controleren door te gaan naar **Azure Active Directory**  - &gt; **gebruikers en groepen**  - &gt; **alle gebruikers**. 

Selecteer uw gebruikersaccount, klikt u vervolgens 'Directory rol' in het resulterende menu. Controleer of de geselecteerde rol ' beheerder van de toepassing ' of 'Globale beheerder'. Als u geen toegang tot alle pagina's die aan deze stappen bent, hebt u niet de vereiste functieservices.

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over Azure AD Application Proxy connectors](application-proxy-connectors.md)
