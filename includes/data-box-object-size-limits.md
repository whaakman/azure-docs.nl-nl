---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 05/21/2019
ms.author: alkohli
ms.openlocfilehash: eb12adf8f8523686b1d8deda2776eb203a76e954
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66244588"
---
Hier vindt u de grootte van de Azure-objecten die kunnen worden geschreven. Zorg ervoor dat alle bestanden die worden geüpload, aan deze limieten voldoen.

| Azure-objecttype | Standaardlimiet                                             |
|-------------------|-----------------------------------------------------------|
| Blok-blob        | ~ 4.75 TiB                                                 |
| Pagina-blobs         | 8 TiB <br> Elk bestand dat is geüpload in de indeling van pagina-blob moet zijn uitgelijnd 512 bytes (een integraal meerdere), anders het uploaden is mislukt. <br> VHD- en VHDX zijn dan 512 bytes uitgelijnd. |
| Azure Files        | 1 TiB                                                      |
| Managed Disks     | 4 TiB <br> Zie voor meer informatie over de grootte en beperkingen: <li>[Schaalbaarheidsdoelen van Standard-SSD 's](../articles/virtual-machines/windows/disks-types.md#standard-ssd)</li><li>[Schaalbaarheidsdoelen van Premium SSD 's](../articles/virtual-machines/windows/disks-types.md#standard-hdd)</li><li>[Schaalbaarheidsdoelen van Standard HDD 's](../articles/virtual-machines/windows/disks-types.md#premium-ssd)</li><li>[Prijzen en facturering van beheerde schijven](../articles/virtual-machines/windows/disks-types.md#billing)</li>  
