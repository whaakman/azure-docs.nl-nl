---
title: Windows virtuele bureaublad Preview-omgeving - Azure
description: De basiselementen van een Windows Virtual Desktop Preview-omgeving.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: ceed6a8bb74206b7c6689ce542482148800e4ba9
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58403507"
---
# <a name="windows-virtual-desktop-preview-environment"></a>Windows virtuele bureaublad Preview-omgeving

Windows virtuele bureaublad Preview is een service waarmee gebruikers eenvoudig en veilig toegang tot hun gevirtualiseerde desktops en RemoteApps biedt. In dit onderwerp ziet u iets meer over de algemene structuur van de virtuele Windows-bureaublad-omgeving.

## <a name="tenants"></a>Tenants

De tenant virtuele bureaublad van Windows is de primaire interface voor het beheren van uw virtuele Windows-bureaublad-omgeving. Elke tenant virtuele Windows-bureaublad moet worden gekoppeld met de Azure Active Directory met de gebruikers die zullen zich aanmelden bij de omgeving. Van de tenant virtuele Windows-bureaublad kunt u beginnen met het maken van pools van de host om uit te voeren van uw gebruikers workloads.

## <a name="host-pools"></a>Host-pools

Een groep host is een verzameling van Azure virtuele machines die op virtuele Windows-bureaublad als sessie hosts registreren tijdens het uitvoeren van de virtuele bureaublad van Windows-agent. Alle sessie hosten van virtuele machines in een groep host moet worden afkomstig is van dezelfde installatiekopie voor een consistente gebruikerservaring.

Een groep host kan een van twee typen zijn:

- Persoonlijk, waarbij elke sessiehost is toegewezen aan individuele gebruikers.
- Gepoold waar sessie hosts kunnen verbindingen accepteren van een gebruiker aan een app-groep in de groep host is geautoriseerd.

U kunt extra eigenschappen instellen op de host van toepassingen de load-balancing om gedrag te wijzigen, het aantal sessies elke sessiehost duren kan, en wat de gebruiker kan doen om sessie hosts in de groep host terwijl aangemeld bij hun virtuele Windows-bureaublad-sessies. U de resources die zijn uitgegeven aan gebruikers via app-groepen beheren.

## <a name="app-groups"></a>App-groepen

Een app-groep is een logische groepering van toepassingen zijn geïnstalleerd op de sessie-hosts in de groep host. Een app-groep kan een van twee typen zijn:

- RemoteApp, waar gebruikers toegang krijgen de RemoteApps tot u afzonderlijk selecteren en publiceren naar de app-groep
- Bureaublad, waar gebruikers toegang het volledige bureaublad tot

Een bureaublad-app-groep (met de naam 'Desktop Application groeperen') wordt standaard automatisch gemaakt wanneer u een host-pool maakt. U kunt deze app-groep op elk gewenst moment verwijderen. U niet kan echter een andere bureaublad-app-groep in de groep host maken terwijl een desktop-app-groep bestaat. Als u wilt publiceren RemoteApps, moet u een RemoteApp-app-groep maken. U kunt meerdere groepen van RemoteApp-app om te voldoen aan andere werknemer scenario's maken. Andere RemoteApp-app-groepen kunnen ook bevatten overlappende RemoteApps.

Voor het publiceren van bronnen aan gebruikers, moet u ze toewijzen aan app-groepen. Wanneer gebruikers toewijzen aan app-groepen, moet u rekening houden met de volgende zaken:

- Een gebruiker kan niet worden toegewezen aan zowel een desktop-app-groep en een RemoteApp-app-groep in de groep met dezelfde host.
- Een gebruiker kan worden toegewezen aan meerdere app-groepen binnen de pool met dezelfde host en de feed is een opeenstapeling van beide app-groepen.

## <a name="tenant-groups"></a>Tenant-groepen

In de virtuele Windows-bureaublad is de virtuele Windows-bureaublad-tenant waar de meeste van de installatie en configuratie plaatsvindt. De tenant virtuele Windows-bureaublad bevat de host-pools, app-groepen en toewijzingen van app-groep gebruikers. Er kunnen wel bepaalde situaties voordoen waarin u moet voor het beheren van meerdere virtuele Windows-bureaublad tenants tegelijk, met name als u een Cloud Service Provider (CSP) of een hosting-partner bent. In deze situaties kunt u een aangepaste groep van de virtuele Windows-bureaublad-tenant plaatst u elk van de klant virtuele Windows-bureaublad tenants en centraal beheren en toegang. Als u slechts één virtuele Windows-bureaublad tenant beheert, het concept van de groep tenant is niet van toepassing en u kunt blijven gebruiken en beheren van uw tenant aanwezig is op de standaardgroep van de tenant.

## <a name="end-users"></a>Eindgebruikers

Nadat u gebruikers hebt toegewezen aan de app-groepen, kunnen ze verbinding maken met een virtuele Windows-bureaublad-implementatie met een van de virtuele Windows-bureaublad-clients.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over gedelegeerde toegang en rollen toewijzen aan gebruikers op [gedelegeerde toegang in Windows Virtual Desktop Preview](delegated-access-virtual-desktop.md).

Zie voor meer informatie over het instellen van uw tenant virtuele Windows-bureaublad, [een tenant maken in Windows Virtual Desktop Preview](tenant-setup-azure-active-directory.md).

Als u wilt weten hoe u verbinding maken met virtuele Windows-bureaublad, ziet u een van de volgende artikelen:

- [Verbinding maken met de extern bureaublad-client op Windows 7 en Windows 10](connect-windows-7-and-10.md)
- [Verbinding maken met de webclient Windows Virtual Desktop Preview](connect-web.md)
