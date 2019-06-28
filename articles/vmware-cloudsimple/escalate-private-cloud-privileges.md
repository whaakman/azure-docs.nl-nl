---
title: Bevoegdheden voor persoonlijke cloud - Azure VMware-oplossing door CloudSimple escaleren
description: Hierin wordt beschreven hoe u bevoegdheden op uw persoonlijke cloud voor beheerfuncties in vCenter uit te breiden
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/05/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: d11c88b91b13cca13120a9203e376fdc2c3d6d8d
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2019
ms.locfileid: "67332781"
---
# <a name="escalate-private-cloud-vcenter-privileges-from-the-cloudsimple-portal"></a>Private Cloud vCenter bevoegdheden vanuit de portal CloudSimple escaleren 

Voor beheerderstoegang tot uw vCenter Private Cloud, kunt u tijdelijk de bevoegdheden CloudSimple escaleren.  Met verhoogde bevoegdheden, kunt u oplossingen voor VMware installeren, identiteit bronnen toevoegen en beheren van gebruikers.

Nieuwe gebruikers kunnen worden gemaakt op de vCenter-SSO-domein en toegang krijgen tot vCenter.  Wanneer u nieuwe gebruikers maakt, voegt u ze aan de groepen van de ingebouwde CloudSimple voor toegang tot vCenter.  Zie voor meer informatie, [CloudSimple Private Cloud machtigingsmodel van VMware vCenter](https://docs.azure.cloudsimple.com/learn-private-cloud-permissions/).

> [!CAUTION]
> Breng geen configuratiewijzigingen voor onderdelen. Acties die worden uitgevoerd tijdens de geëscaleerde bevoegde status kunnen dit negatieve gevolgen voor uw systeem of kunnen ervoor zorgen dat uw systeem niet meer beschikbaar zijn.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="escalate-privileges"></a>Bevoegdheden escaleren

1. Toegang tot de [CloudSimple portal](access-cloudsimple-portal.md).

2. Open de **Resources** pagina, selecteert u de Privécloud waarvan u wilt om privileges te escaleren.

3. Kies onder aan de overzichtspagina onder **vSphere bevoegdheden wijzigen**, klikt u op **escaleren**.

    ![VSphere-bevoegdheid wijzigen](media/escalate-private-cloud-privilege.png)

4. Selecteer het type van de gebruiker vSphere.  Alleen **CloudOwner@cloudsimple.local** lokale gebruiker, kan worden geëscaleerd.

5. Selecteer het tijdsinterval escaleren in de vervolgkeuzelijst. Kies de kortste periode waarmee u kunt de taak te voltooien.

6. Schakel het selectievakje in om te bevestigen dat u bekend bent met de risico's.

    ![Dialoogvenster bevoegdheden escaleren](media/escalate-private-cloud-privilege-dialog.png)

7. Klik op **OK**.

8. De escalatieproces kan enkele minuten duren. Na het voltooien klikt u op **OK**.

De uitbreiding van bevoegdheden wordt gestart en de looptijd is tot het einde van het geselecteerde interval.  U kunt zich aanmelden met uw vCenter-privécloud administratieve taken uitvoeren.

> [!IMPORTANT]
> Slechts één gebruiker kunt bevoegdheden hebben uitgebreid.  U moet ongedaan maken van de gebruiker heeft bevoegdheden die escaleren voordat u kunt een andere gebruiker heeft bevoegdheden die escaleren.

## <a name="extend-privilege-escalation"></a>Uitbreiding van bevoegdheden uitbreiden

Als u nodig als u meer tijd om uw taken te voltooien hebt, kunt u de periode van de uitbreiding van bevoegdheden uitbreiden.  Kies dat de extra escaleren tijdsinterval waarmee u de administratieve taken uit te voeren.

1. Op de **Resources** > **Privéclouds** in de portal voor CloudSimple, selecteert u de Privécloud waarvan u wilt uitbreiden, uitbreiding van bevoegdheden.

2. Aan de onderkant van het tabblad Overzicht, klikt u op **uitbreiding van bevoegdheden uitbreiden**.

    ![Uitbreiding van bevoegdheden uitbreiden](media/de-escalate-private-cloud-privilege.png)

3. Selecteer een tijdsinterval escaleren in de vervolgkeuzelijst. Bekijk de nieuwe escalatie-eindtijd.

4. Klik op **opslaan** om uit te breiden het interval.

## <a name="de-escalate-privileges"></a>Ongedaan maken escaleren bevoegdheden

Zodra uw administratieve taken voltooid zijn, moet u uw bevoegdheden ongedaan maken escaleren.  

1. Op de **Resources** > **Privéclouds** in de portal voor CloudSimple, selecteert u de Privécloud waarvan u wilt ongedaan maken om privileges te escaleren.

2. Klik op **ongedaan maken escaleren**.

3. Klik op **OK**.

> [!IMPORTANT]
> Afmelden bij vCenter en meld u aan na het ongedaan maken escaleren bevoegdheden eventuele fouten te voorkomen.

## <a name="next-steps"></a>Volgende stappen

* [Bronnen voor vCenter-id ingesteld voor het gebruik van Active Directory](https://docs.azure.cloudsimple.com/set-vcenter-identity/)
* Installeren van de back-upoplossing [back-up van werkbelasting van virtuele machines](https://docs.azure.cloudsimple.com/backup-workloads-veeam/)