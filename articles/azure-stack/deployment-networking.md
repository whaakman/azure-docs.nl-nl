---
title: Azure Stack-implementatie-netwerkverkeer | Microsoft Docs
description: Dit artikel wordt beschreven wat u kunt verwachten over netwerken processen voor Azure Stack-implementatie.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2018
ms.author: jeffgilb
ms.reviewer: wamota
ms.lastreviewed: 08/30/2018
ms.openlocfilehash: 3e53d79682772be7eacb649148ceeaec7d9f2865
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55247575"
---
# <a name="about-deployment-network-traffic"></a>Over het netwerkverkeer voor implementatie
Inzicht krijgen in hoe verkeer stroomt tijdens Azure Stack-implementatie is essentieel om ervoor te zorgen dat een geslaagde implementatie. Dit artikel begeleidt u bij verwachte netwerkverkeer tijdens het implementatieproces voor een goed begrip van wat u kunt verwachten.

Deze afbeelding ziet u de onderdelen en verbindingen die betrokken zijn bij het implementatieproces:

![Azure Stack-implementatie-netwerktopologie](media/deployment-networking/figure1.png)

> [!NOTE]
> In dit artikel beschrijft de vereisten voor de implementatie van een verbonden, voor meer informatie over andere methoden voor het implementeren, Zie [Azure Stack-verbinding implementatiemodellen](azure-stack-connection-models.md).

### <a name="the-deployment-vm"></a>De VM-implementatie
De Azure Stack-oplossing bevat een groep servers die worden gebruikt voor het hosten van Azure Stack-onderdelen en een extra server met de naam van de Hardware Lifecycle Host (HLH). Deze server wordt gebruikt voor het implementeren en beheren van de levenscyclus van uw oplossing en als host fungeert voor de implementatie van virtuele machine (DVM) tijdens de implementatie.

## <a name="deployment-requirements"></a>Vereisten voor de implementatie
Voordat de implementatie is gestart, zijn er bepaalde minimale vereisten die kunnen worden gevalideerd door de OEM om te controleren of de implementatie is voltooid. Informatie over dat deze vereisten kunt u de omgeving voorbereiden en zorg ervoor dat de validatie slaagt, zijn dit:

-   [Certificaten](azure-stack-pki-certs.md)
-   [Azure-abonnement](https://azure.microsoft.com/free/?b=17.06)
-   Toegang tot het internet
-   DNS
-   NTP

> [!NOTE]
> In dit artikel richt zich op de laatste drie vereisten. Zie de bovenstaande koppelingen voor meer informatie over de eerste twee.

## <a name="deployment-network-traffic"></a>Implementatie-netwerkverkeer
De DVM is geconfigureerd met een IP-adres uit het BMC-netwerk en toegang tot het netwerk met internet vereist. Hoewel niet alle van de BMC-netwerkonderdelen externe routering of toegang tot Internet vereist, kan sommige OEM-specifieke onderdelen met behulp van IP-adressen van dit netwerk deze mogelijk ook vereist.

Tijdens de implementatie, wordt de DVM geverifieerd op basis van Azure Active Directory (Azure AD) met behulp van een Azure-account van uw abonnement. Als u wilt doen, is de DVM vereist om een lijst van specifieke poort en URL's toegang tot internet. U vindt de volledige lijst in de [eindpunten publiceren](azure-stack-integrate-endpoints.md) documentatie. De DVM zal gebruikmaken van een DNS-server voor het doorsturen van DNS-aanvragen door interne onderdelen voor de externe URL's. De interne DNS-server verzendt deze aanvragen voor het adres van de DNS-doorstuurserver die u aan de OEM voorafgaand aan de implementatie. Hetzelfde geldt voor de NTP-server, een betrouwbare Time-Server is vereist voor het handhaven van consistentie en tijdsynchronisatie voor alle onderdelen van Azure Stack.

De toegang tot internet vereist voor de DVM tijdens de implementatie is alleen uitgaande, er worden geen inkomende aanroepen worden gemaakt tijdens de implementatie. Houd er rekening mee dat deze het IP-als bron gebruikt en die Azure Stack biedt geen ondersteuning voor configuraties van webproxy's. Indien nodig, moet u voor een transparante proxy of een NAT voor toegang tot het internet. Tijdens de implementatie van starten enkele interne onderdelen toegang tot internet via het externe netwerk met behulp van openbare VIP's. Nadat de implementatie is voltooid, wordt alle communicatie tussen Azure en Azure Stack gemaakt via het externe netwerk met behulp van openbare VIP's.

Configuraties van het netwerk op Azure Stack-switches bevatten toegangsbeheerlijsten (ACL's) die beperken het verkeer tussen bepaalde netwerkbronnen en bestemmingen. De DVM is het enige onderdeel met onbeperkte toegang; zelfs de HLH is zeer beperkt. U kunt uw OEM vragen over aanpassingsopties om de beheer- en toegangsbeheer van uw netwerken. Vanwege deze ACL's is het belangrijk om te voorkomen dat de adressen van de DNS- en NTP-server wijzigen tijdens de implementatie. Als u dit doet, moet u alle van de schakelopties voor de oplossing opnieuw te configureren.

Nadat de implementatie is voltooid, blijft de opgegeven DNS- en NTP-serveradressen rechtstreeks worden gebruikt door de onderdelen van het systeem. Bijvoorbeeld, als u DNS-aanvragen controleren nadat de implementatie is voltooid, wordt de bron gewijzigd van de IP DVM naar een adres uit het adresbereik van het externe netwerk.

Nadat de implementatie is voltooid, blijft de opgegeven DNS- en NTP-server-adressen worden gebruikt door de onderdelen van het systeem via het SDN met behulp van het externe netwerk. Bijvoorbeeld, als u DNS-aanvragen controleren nadat de implementatie is voltooid, wordt de bron gewijzigd van de DVM IP op een openbare VIP-adres.

## <a name="next-steps"></a>Volgende stappen
[Valideren van de registratie van Azure](azure-stack-validate-registration.md)
