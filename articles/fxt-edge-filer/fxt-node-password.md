---
title: Hardware - Microsoft Azure FXT Edge Filer initialiseren
description: Over het instellen van een eerste wachtwoord op Azure FXT Edge Filer knooppunten
author: ekpgh
ms.service: fxt-edge-filer
ms.topic: tutorial
ms.date: 06/20/2019
ms.author: v-erkell
ms.openlocfilehash: 11cf9f49014648fff1e78aff91c5a724a812e9e7
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67450294"
---
# <a name="tutorial-set-hardware-passwords"></a>Zelfstudie: Hardware-wachtwoorden instellen

De eerste keer dat u Energiebeheer van een Azure FXT Edge Filer-knooppunt, moet u instellen een hoofdwachtwoord. De hardwareknooppunten worden niet geleverd met een standaardwachtwoord. 

Netwerk-poorten zijn uitgeschakeld tot nadat het wachtwoord is ingesteld en de basis-gebruiker zich aanmeldt.

Voer deze stap na de installatie en het knooppunt bekabeling, maar voordat u probeert te maken van het cluster. 

In deze zelfstudie wordt uitgelegd hoe u verbinding maken met de hardware-knooppunt en het wachtwoord instellen. 

In deze zelfstudie leert u het volgende: 

> [!div class="checklist"]
> * Verbinding maken met een toetsenbord en monitor naar het knooppunt en inschakelen
> * Instellen van wachtwoorden voor de iDRAC poort en root-gebruiker op dit knooppunt
> * Meld u aan als hoofdgebruiker 

Herhaal deze stappen voor elk knooppunt dat u in uw cluster gebruiken wilt. 

In deze zelfstudie duurt circa 15 minuten. 

## <a name="prerequisites"></a>Vereisten

Voordat u deze zelfstudie begint, voert u deze stappen: 

* [Installeer](fxt-install.md) op elk knooppunt Azure FXT Edge Filer in een apparatuur rack, en koppel power kabels en netwerktoegang, zoals beschreven in de [eerdere zelfstudie](fxt-network-power.md). 
* Zoeken naar een USB-verbinding aangesloten toetsenbord en een verbonden VGA monitor die u aan de hardwareknooppunten koppelen kunt. (De seriële poort van het knooppunt is niet actief voordat u het wachtwoord instellen.)

## <a name="connect-a-keyboard-and-monitor-to-the-node"></a>Verbinding maken met een toetsenbord en monitor naar het knooppunt

Een monitor en toetsenbord fysiek verbinding met het Azure FXT Edge Filer-knooppunt. 

* De monitor verbinden met de VGA-poort.
* Verbinding maken met het toetsenbord op een van de USB-poorten. 

In dit diagram referentie gebruiken om te vinden van de poorten op de achterkant van het chassis. 

> [!NOTE]
> De seriële poort is niet actief totdat nadat het wachtwoord is ingesteld. 

![diagram van achterzijde Azure FXT Edge Filer met serienummer, VGA, en USB-poorten met het label](media/fxt-back-serial-vga-usb.png)

U kunt een KVM-switch als u wilt verbinding maken met meer dan één knooppunt naar dezelfde randapparatuur. 

De stroom op het knooppunt door de / uit-knop te drukken op de voorgrond. 

![diagram van voorgrond van Azure FXT Edge Filer - ronde / uit-knop heeft het label aan de bovenkant rechts](media/fxt-front-annotated.png)

## <a name="set-initial-passwords"></a>De eerste wachtwoorden instellen 

Het knooppunt Azure FXT Edge Filer worden verschillende berichten naar de monitor worden afgedrukt tijdens het opstarten. Na enkele ogenblikken ziet er een eerste installatiescherm als volgt:

```
------------------------------------------------------
        Microsoft FXT node initial setup
------------------------------------------------------
Password Setup
---------------
Enter a password to set iDRAC and temporary root password.
Minimum password length is 8.
Enter new password:
```

Het opgegeven wachtwoord wordt gebruikt voor twee dingen: 

* Het is de tijdelijke hoofdwachtwoord voor dit Azure FXT Edge Filer-knooppunt. 

  Dit wachtwoord wordt gewijzigd wanneer u een cluster met behulp van dit knooppunt maakt of wanneer u dit knooppunt toevoegen aan het cluster. Het wachtwoord van de cluster-beheer (die zijn gekoppeld aan de gebruiker ``admin``) is ook het hoofdwachtwoord voor alle knooppunten in een cluster.

* Het is het wachtwoord op de lange termijn voor de poort van de management iDRAC/IPMI-hardware.

  Zorg ervoor dat u het wachtwoord vergeet niet dat in het geval u zich aanmelden met IPMI later moet naar een hardwareprobleem op te lossen.

Typ en Bevestig het wachtwoord: 

```
Enter new password:**********
Re-enter password:**********
Loading AvereOS......
```

Nadat u het wachtwoord invoert, blijft het systeem wordt opgestart. Wanneer deze is voltooid, biedt een ``login:`` prompt. 

## <a name="sign-in-as-root"></a>Meld u aan als hoofdgebruiker

Meld u als ``root`` met het wachtwoord dat u zojuist hebt ingesteld. 

```
login: root
Password:**********
```

Nadat u zich hebt aangemeld als root, wordt de netwerkpoorten actief zijn en zal contact op met de DHCP-server voor IP-adressen. 

## <a name="next-steps"></a>Volgende stappen

Het knooppunt is gereed voor het onderdeel zijn van een cluster. U kunt deze gebruiken om de Azure FXT Edge Filer-cluster te maken, of u kunt [toe te voegen aan een bestaand cluster](fxt-add-nodes.md). 

> [!div class="nextstepaction"]
> [Een cluster maken](fxt-cluster-create.md)
