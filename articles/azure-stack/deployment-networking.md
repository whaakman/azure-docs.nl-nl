---
title: Azure-netwerkverkeer van de Stack implementatie | Microsoft Docs
description: Dit artikel wordt beschreven wat u kunt verwachten over netwerken implementatieprocessen Azure-Stack.
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
ms.date: 05/21/2018
ms.author: jeffgilb
ms.reviewer: wamota
ms.openlocfilehash: b808875e66e867b84e2971c6a5bd031d108d003b
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34654933"
---
# <a name="about-deployment-network-traffic"></a>Over het netwerkverkeer voor implementatie
Begrijpen hoe netwerkverkeer loopt tijdens Azure Stack is implementatie het essentieel om ervoor te zorgen, de implementatie. Dit artikel begeleidt u bij verwachte netwerkverkeer tijdens het implementatieproces voor een goed begrip van wat ze kunnen verwachten.

Deze afbeelding ziet u de onderdelen en verbindingen die zijn betrokken bij het implementatieproces:

![Azure netwerktopologie van de Stack-implementatie](media/deployment-networking/figure1.png)

> [!NOTE]
> In dit artikel beschrijft de vereisten voor de implementatie van een verbonden, Zie voor meer informatie over andere implementatiemethoden, [Stack Azure-implementatiemodellen verbinding](azure-stack-connection-models.md).

### <a name="the-deployment-vm"></a>De VM-implementatie
De Azure-Stack-oplossing omvat een groep servers die worden gebruikt voor het hosten van de Azure-Stack-onderdelen en een extra server met de naam van de Hardware Lifecycle Host (HLH). Deze server wordt gebruikt om te implementeren en beheren van de levenscyclus van uw oplossing en als host fungeert voor de implementatie van virtuele machine (DVM) tijdens de implementatie.

## <a name="deployment-requirements"></a>Vereisten voor de implementatie
Voordat de implementatie wordt gestart, zijn er bepaalde minimale vereisten die kunnen worden gevalideerd door de OEM om te controleren of de implementatie is voltooid. Kennis van dat deze vereisten kunt u de omgeving voorbereiden en zorg ervoor dat de validatie slaagt, zijn dit:

-   [Certificaten](azure-stack-pki-certs.md)
-   [Azure-abonnement](https://azure.microsoft.com/free/?b=17.06)
-   Toegang tot het internet
-   DNS
-   NTP

> [!NOTE]
> Dit artikel is gericht op de laatste drie vereisten. Zie voor meer informatie over de eerste twee de bovenstaande koppelingen.

## <a name="deployment-network-traffic"></a>Implementatie-netwerkverkeer
De DVM is geconfigureerd met een IP-adres uit het BMC-netwerk en toegang tot het netwerk met internet vereist. Hoewel niet alle van de BMC netwerkonderdelen externe routering of toegang tot Internet nodig, sommige OEM-specifieke onderdelen met behulp van IP-adressen met dit netwerk ook mogelijk deze.

Tijdens de implementatie verifieert de DVM tegen Azure Active Directory (Azure AD) met een Azure-account van uw abonnement. Als u wilt doen, vereist de DVM internettoegang tot een lijst met URL's en specifieke poort. U vindt de volledige lijst in de [publiceren eindpunten](azure-stack-integrate-endpoints.md) documentatie. De DVM maakt gebruik van een DNS-server voor het doorsturen van DNS-aanvragen door interne onderdelen voor de externe URL's. De interne DNS-server stuurt deze aanvragen naar het adres van DNS-doorstuurserver die u aan de OEM voorafgaand aan de implementatie leveren. Hetzelfde geldt voor de NTP-server, een betrouwbare Time-Server is vereist voor het onderhouden van de consistentie en tijdsynchronisatie voor alle onderdelen van de Azure-Stack.

De toegang tot Internet vereist door de DVM tijdens de implementatie is alleen uitgaand, geen binnenkomende oproepen worden gemaakt tijdens de implementatie. Houd er rekening mee dat het IP-als bron gebruikt en die Stack Azure biedt geen ondersteuning voor configuraties van webproxy's. Indien nodig, moet u een transparentproxy of de NAT voor toegang tot het internet te verstrekken. Nadat de implementatie is voltooid, worden alle communicatie tussen Azure en Azure Stack aangebracht via het externe netwerk met behulp van openbare VIP's.

Netwerkconfiguraties op Azure-Stack switches bevatten toegangsbeheerlijsten (ACL's) die verkeer tussen bepaalde netwerkbronnen en bestemmingen te beperken. De DVM is het enige onderdeel met onbeperkte toegang; zelfs de HLH is zeer beperkt. U kunt uw OEM vragen over aanpassingsopties voor het beheer en toegang vanaf uw netwerken te vereenvoudigen. Vanwege deze ACL's is het belangrijk om te voorkomen dat de DNS- en NTP-serveradressen wijzigen tijdens de implementatie. Als u doet dit, moet u alle virtuele switches voor de oplossing opnieuw te configureren.

Nadat de implementatie is voltooid, blijft de opgegeven DNS- en NTP-serveradressen rechtstreeks worden gebruikt door de onderdelen van het systeem. Bijvoorbeeld, als u DNS-aanvragen controleren nadat de implementatie is voltooid, wordt de bron gewijzigd van de DVM IP in een adres uit het bereik van het externe netwerk.

Nadat Azure-Stack is geïmplementeerd, kan uw partner OEM de DVM gebruiken voor aanvullende taken voor na de implementatie. Echter moet, wanneer alle implementatietaken en na de implementatie zijn voltooid, de OEM verwijderen en de DVM verwijderen uit de HLH.

## <a name="next-steps"></a>Volgende stappen
[Azure registratie valideert](azure-stack-validate-registration.md)
