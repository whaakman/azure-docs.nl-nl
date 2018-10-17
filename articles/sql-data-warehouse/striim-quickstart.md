---
title: Snel starten met Azure SQL Data Warehouse Realtimeplatform | Microsoft Docs
description: Snel aan de slag met Realtimeplatform en Azure SQL Data Warehouse.
services: sql-data-warehouse
author: hirokib
manager: jrj
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 10/12/2018
ms.author: elbutter
ms.reviewer: craigg
ms.openlocfilehash: 09f3126725b9a2cd6a8277447c0eed4a16838429
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49355455"
---
# <a name="striim-azure-sql-dw-marketplace-offering-install-guide"></a>Realtimeplatform Azure SQL DW Marketplace bieden installatiehandleiding

In deze snelstartgids wordt ervan uitgegaan dat u al een bestaand exemplaar van SQL Data Warehouse.

Zoek naar Realtimeplatform in de Azure Marketplace, en selecteer het Realtimeplatform voor integratie van gegevens naar SQL Data Warehouse (tijdelijke) optie 

![Realtimeplatform installeren][install]

Configureer de Striim-VM met de opgegeven eigenschappen, noteert de Realtimeplatform clusternaam, het wachtwoord en het beheerderswachtwoord

![Realtimeplatform configureren][configure]

Als geïmplementeerd, klikt u op <VM Name>- masternode in Azure portal, klik op verbinding maken en kopiëren van de aanmelding met lokaal VM-account 

![Realtimeplatform verbinden met SQL datawarehouse][connect]

Download de sqljdbc42.jar van <https://www.microsoft.com/en-us/download/details.aspx?id=54671> naar uw lokale computer. 

Open een opdrachtregelvenster en wijzig de mappen in waar u het JDBC jar hebt gedownload. SCP het jar-bestand voor uw VM Striim, het adres en het wachtwoord ophalen uit de Azure-portal

![Jar-bestand kopiëren naar uw virtuele machine][copy-jar]

Een andere opdrachtregelvenster openen of met een ssh hulpprogramma voor ssh naar het Realtimeplatform van cluster

![Voeg SSH toe aan het cluster][ssh]

Voer de volgende opdrachten het JDBC jar-bestand naar Realtimeplatform van lib map, verplaatsen en starten en stoppen van de server uit.

   1. sudo su
   2. map/Tmp cd
   3. MV sqljdbc42.jar /opt/striim/lib
   4. systemctl stop realtimeplatform-knooppunt
   5. systemctl stop realtimeplatform-DBMS-systemen
   6. systemctl start realtimeplatform-DBMS-systemen
   7. systemctl start realtimeplatform-knooppunt

![Het Realtimeplatform cluster starten][start-striim]

Nu, open uw favoriete browser en navigeer naar <DNS Name>: 9080

![Navigeer naar het aanmeldingsscherm][navigate]

Meld u aan met de gebruikersnaam en het wachtwoord die u instelt in Azure portal en selecteer de gewenste wizard aan de slag, of Ga naar de pagina Apps om te starten met behulp van het slepen en neerzetten-gebruikersinterface

![Meld u aan met referenties][login]



[install]: ./media/striim-quickstart/install.png
[configure]: ./media/striim-quickstart/configure.png
[connect]:./media/striim-quickstart/connect.png
[copy-jar]:./media/striim-quickstart/copy-jar.png
[ssh]:./media/striim-quickstart/ssh.png
[start-striim]:./media/striim-quickstart/start-striim.png
[navigate]:./media/striim-quickstart/navigate.png
[login]:./media/striim-quickstart/login.png