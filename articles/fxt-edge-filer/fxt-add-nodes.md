---
title: Microsoft Azure FXT Edge Filer clusterconfiguratie - knooppunten toevoegen
description: Knooppunten toevoegen aan de cache van Azure FXT Edge Filer opslag
author: ekpgh
ms.service: fxt-edge-filer
ms.topic: tutorial
ms.date: 06/20/2019
ms.author: v-erkell
ms.openlocfilehash: 970639eec8c16540d8d7653f1d8bb01e4a397080
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67450497"
---
# <a name="tutorial-add-cluster-nodes"></a>Zelfstudie: Clusterknooppunten toevoegen 

Een nieuw Azure FXT Edge Filer-cluster wordt gemaakt met slechts één knooppunt. U moet ten minste twee meer knooppunten toevoegen en hoge beschikbaarheid inschakelen voordat u een andere configuratie uitvoert. 

In deze zelfstudie wordt uitgelegd hoe u clusterknooppunten toevoegen en de hoge beschikbaarheid (HA) inschakelen. 

In deze zelfstudie leert u het volgende: 

> [!div class="checklist"]
> * Knooppunten toevoegen aan het cluster FXT
> * HA inschakelen

De stappen in deze zelfstudie duren ongeveer 45 minuten.

Voordat u deze zelfstudie begint, power op de knooppunten die u wilt toevoegen en [hun eerste wachtwoord](fxt-node-password.md). 

## <a name="1-load-the-cluster-nodes-page"></a>1. Laden van de clusterknooppunten-pagina

Open het Configuratiescherm van het cluster in een webbrowser en meld u aan als beheerder. (Gedetailleerde instructies vindt u in de overzichtsartikel onder [Open de instellingen voor pagina's](fxt-cluster-create.md#open-the-settings-pages).)

Het bevat het Configuratiescherm de **Dashboard** tabblad wanneer deze wordt geopend. 

![Deelvenster Dashboard beheren (eerste tabblad)](media/fxt-cluster-config/dashboard-1-node.png)

Deze afbeelding ziet u het dashboard van een nieuw cluster, met één knooppunt.

## <a name="2-locate-the-node-to-add"></a>2. Zoek het knooppunt om toe te voegen

Als u knooppunten toevoegen, klikt u op de **instellingen** tabblad en kies de **FXT knooppunten** pagina in de **Cluster** sectie.

![Configuratiescherm-instellingen (tweede tabblad) tabblad met Cluster > FXT knooppunten geladen](media/fxt-cluster-config/settings-fxt-nodes.png)

De **FXT knooppunten - deelname** lijst toont alle niet-toegewezen FXT knooppunten (de meeste datacenters hebben slechts een paar. Zoek de FXT knooppunten die u wilt toevoegen aan het cluster.

> [!Tip] 
> Als u het knooppunt niet vinden op het gewenste de **Unjoined** weergeven, moet u controleren of het voldoet aan deze vereisten:
> 
> * Het wordt ingeschakeld, en heeft een [basis van wachtwoorden in te stellen](fxt-node-password.md).
> * Het is verbonden met een netwerk dat u kunt openen. Als u VLAN's gebruikt, moet dit worden op hetzelfde VLAN als het cluster.
> * Het kan worden gedetecteerd met het protocol Bonjour. 
>
>   Sommige firewallinstellingen blokkeren de TCP/UDP-poorten die worden gebruikt door Bonjour, waarmee wordt voorkomen dat het besturingssysteem FXT de knooppunten automatisch te detecteren.
> 
> Als het knooppunt dat u wilt toevoegen, niet in de lijst, kunt u deze oplossingen proberen: 
> 
> * Klik op de **handmatig detecteren** knop op het dashboard zoeken door IP-adres.
> 
> * Tijdelijke IP-adressen handmatig toewijzen. Dit is zeldzame maar nodig zijn als u gelabelde VLAN's gebruikt en de knooppunten zich niet op het juiste netwerk of uw netwerk zelf toegewezen IP-adressen niet toelaat. Volg de instructies in de oudere versie van dit document om [Stel handmatig een statisch IP-adres](https://azure.github.io/Avere/legacy/create_cluster/4_8/html/static_ip.html).

De naam van het knooppunt, IP-adres, softwareversie en status van in aanmerking komt, worden weergegeven in de lijst. Normaal gesproken de **Status** kolom een zegt 'Wil deelnemen aan' of beschrijving van een systeem- of hardware probleem waardoor het knooppunt niet in aanmerking voor deelname aan het cluster.

De **acties** kolom heeft knoppen waarmee u het knooppunt aan het cluster toevoegen of bijwerken van de software. De bijwerkknop wordt automatisch de softwareversie die overeenkomt met de knooppunten in het cluster al geïnstalleerd.

Alle knooppunten in een cluster moet dezelfde versie van het besturingssysteem gebruiken, maar u hoeft niet te werken software voordat u een knooppunt toevoegt. Nadat u op de **toestaan Join** knop, de join-proces van het cluster automatisch gecontroleerd en installeert de OS-software die overeenkomt met de versie op het cluster.

Lees voor meer informatie over de opties op deze pagina [ **Cluster** > **FXT knooppunten** ](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_fxt_nodes.html) in de handleiding voor het Cluster.

## <a name="3-click-the-allow-to-join-button"></a>3. Klik op de knop 'Toestaan om te koppelen' 

Klik op de **toestaan om te koppelen*** knop in de **acties** kolom voor het knooppunt dat u wilt toevoegen.

Nadat u op de knop klikt, wordt de status van het knooppunt veranderen als de software wordt bijgewerkt ter voorbereiding op toe te voegen aan het cluster. 

De onderstaande afbeelding ziet u een knooppunt dat wordt momenteel lid worden van het cluster (waarschijnlijk dat is dit het ophalen van een update van het besturingssysteem voordat deze is toegevoegd). Er is geen knoppen worden weergegeven in de **acties** kolom voor knooppunten die momenteel wordt toegevoegd aan het cluster.

![een rij van de knooppunttabel, met de naam van een knooppunt, IP-adres, softwareversie, het bericht 'Toegestaan om toe te voegen' en een lege laatste kolom](media/fxt-cluster-config/node-join-in-process.png)

Na enkele ogenblikken wordt het nieuwe knooppunt, moet worden weergegeven in de lijst met clusterknooppunten aan de bovenkant van de **FXT knooppunten** instellingenpagina. 

Herhaal dit proces om toe te voegen van de andere knooppunten in uw cluster. U hoeft niet te wachten op één knooppunt te voltooien voordat u begint met een ander lid worden van het cluster.

## <a name="enable-high-availability"></a>Hoge beschikbaarheid inschakelen

Nadat u een tweede knooppunt aan het cluster hebt toegevoegd, ziet u mogelijk een waarschuwingsbericht wordt weergegeven in het deelvenster Beheer Dashboard die de hoge beschikbaarheid-functie niet is geconfigureerd. 

Hoge beschikbaarheid (HA) kunt de clusterknooppunten om te compenseren voor elkaar, als een uitvalt. HA wordt niet standaard ingeschakeld.

![Dashboardtabblad met het bericht 'het cluster heeft meer dan één knooppunt, maar HA is niet ingeschakeld...' in de tabel voorwaarden](media/fxt-cluster-config/no-ha-2-nodes.png)

> [!Note] 
> Schakel geen HA totdat er ten minste drie knooppunten in het cluster.

Volg deze procedure om in te schakelen HA: 

1. Load de **hoge beschikbaarheid** pagina in de **Cluster** sectie van de **instellingen** tabblad.

   ![HA-configuratiepagina (Cluster > hoge beschikbaarheid). Het "Inschakelen HA" selectievakje is aan de bovenkant en de verzendknop is aan de onderkant.](media/fxt-cluster-config/enable-ha.png)

2. Klik op het selectievakje **inschakelen HA** en klikt u op de **indienen** knop. 

Een waarschuwing wordt weergegeven op de **Dashboard** om te bevestigen dat HA is ingeschakeld.

![Dashboard-tabel van het bericht "HA is nu volledig geconfigureerd"](media/fxt-cluster-config/ha-configured-alert.png)


## <a name="next-steps"></a>Volgende stappen

Na het toevoegen van alle knooppunten in het cluster, de installatie wilt voortzetten door het configureren van langdurige opslag van uw cluster.

> [!div class="nextstepaction"]
> [Back-end-opslag toevoegen en instellen van de virtuele-naamruimte](fxt-add-storage.md)