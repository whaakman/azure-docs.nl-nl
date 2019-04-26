---
title: Verbinding met extern bureaublad voor een rol in Azure Cloudservices inschakelen | Microsoft Docs
description: Uw azure-cloud service-toepassing om toe te staan van verbindingen met extern bureaublad configureren
services: cloud-services
documentationcenter: ''
author: mmccrory
manager: timlt
editor: ''
ms.assetid: 73ea1d64-1529-4d72-b58e-f6c10499e6bb
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: mmccrory
ms.openlocfilehash: e9e5308f63034efefc0616997301bfc1b383fd84
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60527370"
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a>Verbinding met extern bureaublad voor een rol in Azure Cloudservices inschakelen

> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
> * [Visual Studio](cloud-services-role-enable-remote-desktop-visual-studio.md)

Extern bureaublad kunt u toegang tot het bureaublad van een functie die wordt uitgevoerd in Azure. U kunt een extern bureaublad-verbinding oplossen en diagnosticeren van problemen met uw toepassing terwijl deze wordt uitgevoerd.

U kunt een extern bureaublad-verbinding in uw rol tijdens het ontwikkelen van inschakelen via de extern bureaublad-modules in uw servicedefinitie van de of u kunt Extern bureaublad via de extern bureaublad-uitbreiding inschakelen. De aanpak van voorkeur is het gebruik van de extern bureaublad-extensie, zoals u extern bureaublad inschakelen kunt, zelfs nadat de toepassing wordt geïmplementeerd zonder dat om uw toepassing opnieuw te implementeren.

## <a name="configure-remote-desktop-from-the-azure-portal"></a>Extern bureaublad configureren vanuit de Azure portal

De Azure portal maakt gebruik van de aanpak van extern bureaublad-extensie, zodat u extern bureaublad inschakelen kunt, zelfs nadat de toepassing wordt geïmplementeerd. De **extern bureaublad** instellingen voor uw cloudservice kunt u extern bureaublad inschakelen, wijzigt het lokale Administrator-account waarmee verbinding wordt gemaakt met de virtuele machines wordt het certificaat gebruikt bij de verificatie en de vervaldatum instellen de datum.

1. Klik op **Cloudservices**, selecteert u de naam van de cloudservice en selecteer vervolgens **extern bureaublad**.

    ![Extern bureaublad voor cloud services](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop.png)

2. Kies of u extern bureaublad inschakelen voor een afzonderlijke rol of voor alle rollen, en vervolgens de waarde van de wisselaar te wijzigen **ingeschakeld**.

3. Vul de vereiste velden voor gebruikersnaam, wachtwoord, verlopen en certificaat in.

    ![Extern bureaublad voor cloud services](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop_Details.png)

   > [!WARNING]
   > Alle rolinstanties wordt opnieuw gestart wanneer u eerst Extern bureaublad inschakelen en selecteer **OK** (ingeschakeld). Om te voorkomen dat een opnieuw opstarten, moet het certificaat dat wordt gebruikt voor het versleutelen van het wachtwoord worden geïnstalleerd op de rol. Om te voorkomen dat een herstart [upload een certificaat voor de cloudservice](cloud-services-configure-ssl-certificate-portal.md#step-3-upload-a-certificate) en ga vervolgens terug naar dit dialoogvenster.

4. In **rollen**, selecteer de rol die u wilt bijwerken of selecteer **alle** voor alle rollen.

5. Wanneer u klaar bent met uw configuratie-updates, selecteert u **opslaan**. Het duurt een paar seconden voordat uw rolinstanties gereed zijn voor verbindingen voor ontvangen.

## <a name="remote-into-role-instances"></a>Extern bureaublad verbinding met de rolinstanties

Zodra de extern bureaublad is ingeschakeld op de rollen, kunt u een verbinding rechtstreeks vanuit de Azure-portal starten:

1. Klik op **exemplaren** openen de **exemplaren** instellingen.
2. Selecteer een exemplaar van rol met extern bureaublad die zijn geconfigureerd.
3. Klik op **Connect** voor het downloaden van een RDP-bestand voor de rolinstantie.

    ![Extern bureaublad voor cloud services](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop_Connect.png)

4. Klik op **Open** en vervolgens **Connect** starten van de extern bureaublad-verbinding.

>[!NOTE]
> Als uw cloudservice zich bevindt achter een NSG, moet u mogelijk te maken van regels die verkeer via poorten **3389** en **20000**.  Extern bureaublad maakt gebruik van poort **3389**.  Cloud Service-exemplaren worden verdeeld, zodat u kunt niet rechtstreeks welk exemplaar verbinding maken bepalen met.  De *RemoteForwarder* en *RemoteAccess* agents RDP-verkeer beheren en dat de client voor het verzenden van een cookie RDP en verbinding maken met een afzonderlijke-exemplaar opgeven.  De *RemoteForwarder* en *RemoteAccess* agents vereist die poort **20000*** openen, dat kan worden geblokkeerd als er een NSG is.

## <a name="additional-resources"></a>Aanvullende resources

[Cloud Services configureren](cloud-services-how-to-configure-portal.md)
