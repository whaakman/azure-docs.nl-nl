---
title: Office 365-controlelaag in Azure virtuele WAN
description: Meer informatie over besturingsvlak Office 365 in virtuele WAN.
author: cherylmc
ms.service: virtual-wan
services: virtual-wan
ms.topic: article
ms.date: 9/24/2018
ms.author: cherylmc
ms.openlocfilehash: 815b91295540e93f8f0ffbc002fcf02ce1c13365
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46992671"
---
# <a name="office-365-control-plane-in-virtual-wan"></a>Office 365-controlelaag in virtuele WAN

Virtuele WAN-klanten met bepaalde SDWAN apparaten kunnen Internet O365-groepen beleidsregels voor vertrouwde verkeer configureren in Azure portal. Hiermee kunt:
- O365-verkeer naar het Microsoft-netwerk dicht bij de gebruiker die optimale gebruikerservaring.
- Voorkomt verkeer back-ophalen en haar pining, zodat de WAN-kosten.
- Het leveren van de principes van O365-connectiviteit.

## <a name="faqs"></a>Veelgestelde vragen
### <a name="what-is-the-customer-benefit"></a>Wat is het voordeel van de klant?
Met deze functie in virtuele WAN-kunnen klanten nu opgeven de categorieën voor het verkeer van Office 365 die ze voor directe internet-groepen vertrouwen. Dit vertrouwde proxy's van Office 365-verkeer wordt omzeilen en de route rechtstreeks vanuit de locatie van de gebruiker naar de dichtstbijzijnde POP van Microsoft. Hiermee voorkomt u verkeer back-slepen en haar pining, dus optimale gebruikerservaring te bieden en opslaan van WAN-kosten. 

### <a name="what-are-the-office-365-traffic-categories"></a>Wat zijn de categorieën van Office 365-verkeer?
Office 365-eindpunten vertegenwoordigen netwerkadressen en subnetten. Eindpunten kunnen worden URL's, IP-adressen of IP-adresbereiken. URL's kunnen een FQDN-naam, zoals *account.office.net*, of een jokerteken-URL, zoals **. office365.com*. Eindpunten zijn onderverdeeld in drie categorieën - **optimaliseren**, **toestaan**, en **standaard**, op basis van hun ernst. Meer informatie over de eindpunt-categorieën zijn [hier](https://docs.microsoft.com/office365/enterprise/office-365-network-connectivity-principles#BKMK_Categories).

### <a name="which-office-365-traffic-category-is-recommended-by-microsoft-for-direct-internet-breakout"></a>Welke categorie Office 365-verkeer wordt door Microsoft aanbevolen voor direct internet-groepen?
De **optimaliseren** categorie is de meest kritieke netwerkeindpunten en is vereist voor het overslaan van SSL-einde en inspecteren en andere netwerkapparaten voor beveiliging. Er moet direct Internet uitgaande dicht bij gebruikers. Deze eindpunten vertegenwoordigen Office 365-scenario's die het meest gevoelig zijn voor de prestaties, latentie en beschikbaarheid van het netwerk. Deze categorie bevat kleine (orde van grootte ~ 10) van sleutel-URL's en een gedefinieerde set IP-subnetten aan core Office 365-werkbelastingen, zoals Exchange Online, SharePoint Online, Skype voor bedrijven Online en toegewezen Microsoft Teams. 

De **toestaan** categorie wordt ook aanbevolen voor direct uitgaand verkeer van Internet. Toestaan dat het netwerkverkeer Hoewel sommige netwerklatentie kan tolereren. Eindpunten in de categorieën optimaliseren en toestaan dat zijn alle die wordt gehost in Microsoft-datacenters en beheerd als onderdeel van Office 365. De categorie Standaard kunnen worden omgeleid naar een standaardlocatie voor de uitgaand verkeer van Internet en niet vereisen directe uitgaand verkeer van Internet of overslaan van SSL-einde en controleren van apparaten.

### <a name="how-do-i-set-my-o365-policies-via-virtual-wan"></a>Hoe stel ik mijn Office 365-beleid via virtuele WAN?
U kunt beleid via beschikken de **virtuele WAN** -> **instellingen** -> **configuratie** tabblad. Hier kunt u uw favoriete categorieën van O365-verkeer voor directe internet-groepen.

![Office 365-controlelaag configureren in virtuele WAN](media/virtual-wan-office365-overview/configure-office365-control-plane.png)

### <a name="how-does-this-work"></a>Hoe werkt dit?

1.  O365-verkeer krijgt de Microsoft-netwerk dicht bij de gebruiker optimale ervaring geven.
2.  Beleid voor route worden verbruikt door SDWAN. Beveiliging-proxy's voor de vertrouwde categorieën omzeilt en het lokale direct-groepen uitvoert voor deze categorieën.
3.  Vorige slepen en verkeer haar pining worden vermeden opslaan van WAN-kosten.

### <a name="which-partner-devices-support-this-via-virtual-wan"></a>Welke partner-apparaten ondersteunen dit via virtuele WAN?
Citrix ondersteunt momenteel deze beleidsregels via virtuele WAN.

### <a name="what-happens-to-the-remaining-categories-of-untrusted-o365-traffic"></a>Wat gebeurt er met de overige categorieën (niet-vertrouwde) O365-verkeer?
Resterende van Office 365-verkeer wordt gaat u als volgt klanten standaard internet verkeer het pad.

### <a name="what-if-i-have-already-specified-my-o365-policies-via-my-sdwan-provider"></a>Wat gebeurt er als ik al hebt opgegeven mijn O365-beleid via Mijn SDWAN-provider?
Als u beleid via de SDWAN UX en Azure virtuele WAN opgeeft, wordt het beleid dat is ingesteld in virtuele WAN voorrang.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [virtuele WAN](virtual-wan-about.md).