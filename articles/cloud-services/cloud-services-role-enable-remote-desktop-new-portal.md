---
title: Inschakelen van extern bureaublad-Connection voor een rol in Azure-Cloudservices | Microsoft Docs
description: Het configureren van uw azure-cloud-servicetoepassing voor het toestaan van extern bureaublad-verbindingen
services: cloud-services
documentationcenter: 
author: mmccrory
manager: timlt
editor: 
ms.assetid: 73ea1d64-1529-4d72-b58e-f6c10499e6bb
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: mmccrory
ms.openlocfilehash: b9ae4442f57170746eb0de94849b09625be51264
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a>Verbinding met extern bureaublad voor een rol in Azure Cloudservices inschakelen
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
> * [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)

Extern bureaublad kunt u toegang tot het bureaublad van een rol in Azure wordt uitgevoerd. U kunt een verbinding met extern bureaublad gebruiken oplossen en analyseren van problemen met uw toepassing, terwijl deze wordt uitgevoerd.

U kunt een verbinding met extern bureaublad in uw rol tijdens het ontwikkelen van inschakelen door de extern bureaublad-modules in de servicedefinitie van de of u kunt Extern bureaublad via de extern bureaublad-uitbreiding inschakelen. De gewenste aanpak is het gebruik van de extern bureaublad-extensie, zoals u extern bureaublad inschakelen kunt, zelfs nadat de toepassing zonder opnieuw te implementeren van uw toepassing wordt geïmplementeerd.

## <a name="configure-remote-desktop-from-the-azure-portal"></a>Extern bureaublad configureren via de Azure portal
De Azure-portal maakt gebruik van de aanpak van extern bureaublad-extensie zodat u extern bureaublad inschakelen kunt, zelfs nadat de toepassing wordt geïmplementeerd. De **extern bureaublad** blade voor uw cloudservice kunt u extern bureaublad inschakelen door het lokale Administrator-account gebruikt om verbinding met de virtuele machines te wijzigen, het certificaat in verificatie gebruikt en de verloopdatum instellen.

1. Klik op **Cloudservices**, klik op de naam van de cloudservice en klik vervolgens op **extern bureaublad**.

    ![Extern bureaublad voor cloud-services](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop.png)

2. Kies of u wilt extern bureaublad inschakelen voor een afzonderlijke functie of voor alle rollen en klik vervolgens op de waarde van de schakelbaar naar **ingeschakeld**.

3. Vul de vereiste velden voor gebruikersnaam, wachtwoord, verlopen en certificaat.

    ![Extern bureaublad voor cloud-services](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop_Details.png)

   > [!WARNING]
   > Alle exemplaren van de functie wordt opnieuw gestart wanneer u eerst Extern bureaublad inschakelen en klik op OK (vinkje). Als u wilt voorkomen dat de computer opnieuw is opgestart, moet het certificaat dat wordt gebruikt voor het versleutelen van het wachtwoord worden geïnstalleerd op de rol. Om te voorkomen dat een herstart [upload een certificaat voor de cloudservice](cloud-services-configure-ssl-certificate-portal.md#step-3-upload-a-certificate) en keer vervolgens terug naar dit dialoogvenster.
   >
   >
3. In **rollen**, selecteer de rol die u wilt bijwerken of selecteer **alle** voor alle functies.

4. Wanneer u klaar bent met de updates voor uw configuratie, klikt u op **opslaan**. Het duurt enige tijd duren voordat uw rolinstanties gereed zijn voor het ontvangen van verbindingen.

## <a name="remote-into-role-instances"></a>De afstand in rolinstanties
Zodra de extern bureaublad is ingeschakeld op de functies, kunt u een verbinding rechtstreeks vanuit de Azure-Portal te starten:

1. Klik op **exemplaren** openen de **exemplaren** blade.
2. Selecteer een rolexemplaar met extern bureaublad die zijn geconfigureerd.
3. Klik op **Connect** voor het downloaden van een RDP-bestand voor de rolinstantie.

    ![Extern bureaublad voor cloud-services](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop_Connect.png)

4. Klik op **Open** en vervolgens **Connect** starten van de extern bureaublad-verbinding.

>[!NOTE]
> Als uw cloudservice zich bevindt achter een NSG, moet u mogelijk maken van regels die verkeer op poorten toestaan **3389** en **20000**.  Extern bureaublad maakt gebruik van poort **3389**.  Cloud Service-exemplaren worden verdeeld, zodat u direct welk exemplaar verbinding maken met kan niet bepalen.  De *RemoteForwarder* en *RemoteAccess* agents RDP-verkeer te beheren en dat de client verzenden van een cookie RDP en geeft u een afzonderlijk exemplaar verbinding maken met.  De *RemoteForwarder* en *RemoteAccess* agents vereisen die poort **20000*** worden geopend die worden geblokkeerd als er een NSG.

## <a name="additional-resources"></a>Aanvullende resources

[Cloud-Services configureren hoe](cloud-services-how-to-configure-portal.md)
[extern bureaublad-services FAQ - Cloud](cloud-services-faq.md)
