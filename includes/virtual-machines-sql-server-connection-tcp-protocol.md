---
author: rothja
ms.service: virtual-machines-sql
ms.topic: include
ms.date: 10/26/2018
ms.author: jroth
ms.openlocfilehash: 8b919608dfc562d8db77619d5215a6828a53a4aa
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51263960"
---
1. Zoek naar **Configuration Manager** terwijl u via Extern bureaublad bent verbonden met de virtuele machine:

    ![SSCM openen](./media/virtual-machines-sql-server-connection-tcp-protocol/sql-server-configuration-manager.png)

1. Breid in het consolevenster van SQL Server Configuration Manager het gedeelte **SQL Server-netwerkconfiguratie** uit.

1. Klik in het consolevenster op **Protocollen voor MSSQLSERVER** (de naam van het standaardexemplaar). Klik in het detailvenster met de rechtermuisknop op **TCP** en vervolgens op **Inschakelen** als deze optie nog niet is ingeschakeld.

    ![TCP inschakelen](./media/virtual-machines-sql-server-connection-tcp-protocol/enable-tcp.png)

1. Klik in het consolevenster op **SQL Server-services**. In het detailvenster met de rechtermuisknop op **SQL Server (*exemplaarnaam*)** (het standaardexemplaar is **SQL Server (MSSQLSERVER)**), en klik vervolgens op **opnieuwstarten**, om te stoppen en opnieuw opstarten van het exemplaar van SQL Server.

    ![Database-engine opnieuw starten](./media/virtual-machines-sql-server-connection-tcp-protocol/restart-sql-server.png)

1. Sluit SQL Server Configuration Manager.

Zie [Enable or Disable a Server Network Protocol](https://msdn.microsoft.com/library/ms191294.aspx) (Een servernetwerkprotocol in- of uitschakelen) voor meer informatie over het inschakelen van protocollen voor de database-engine van SQL Server.
