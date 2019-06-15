---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 9a5a2d92f70c411c46ebb4efb35e17e9b0c477ca
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66161598"
---
1. Dapl, rdmacm ibverbs en mlx4 installeren

   ```bash
   sudo apt-get update

   sudo apt-get install libdapl2 libmlx4-1

   ```

2. In /etc/waagent.conf, schakelt u RDMA door de volgende configuratieregels uncommenting. U moet toegang tot de hoofdmap om dit bestand te bewerken.
  
   ```
   OS.EnableRDMA=y

   OS.UpdateRdmaDriver=y
   ```

3. Toevoegen of wijzigen van de volgende geheugeninstellingen in KB in het bestand /etc/security/limits.conf. U moet toegang tot de hoofdmap om dit bestand te bewerken. Voor testdoeleinden kunt u memlock instellen op onbeperkt. Bijvoorbeeld: `<User or group name>   hard    memlock   unlimited`.

   ```
   <User or group name> hard    memlock <memory required for your application in KB>

   <User or group name> soft    memlock <memory required for your application in KB>
   ```
  
4. Intel MPI bibliotheek installeren. Een van beide [kopen en downloaden](https://software.intel.com/intel-mpi-library/) de bibliotheek van Intel of download de [gratis evaluatieversie](https://registrationcenter.intel.com/en/forms/?productid=1740).

   ```bash
   wget http://registrationcenter-download.intel.com/akdlm/irc_nas/tec/9278/l_mpi_p_5.1.3.223.tgz
   ```
 
   Alleen Intel MPI 5.x runtimes worden ondersteund.
 
   Zie voor stappen van de installatie, de [Intel MPI-bibliotheek installatiehandleiding](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html).

5. Ptrace voor niet-hoofdmap niet foutopsporingsprogramma processen (die nodig zijn voor de meest recente versies van Intel MPI) inschakelen.
 
   ```bash
   echo 0 | sudo tee /proc/sys/kernel/yama/ptrace_scope
   ```
