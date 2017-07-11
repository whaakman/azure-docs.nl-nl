---
title: Uw eerste Jenkins-master op een Linux (Ubuntu)-VM op Azure maken
description: Maak gebruik van de oplossingssjabloon voor het implementeren van Jenkins.
services: app-service\web
documentationcenter: 
author: mlearned
manager: douge
editor: 
ms.assetid: 8bacfe3e-7f0b-4394-959a-a88618cb31e1
ms.service: multiple
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 6/7/2017
ms.author: mlearned
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 1500c02fa1e6876b47e3896c40c7f3356f8f1eed
ms.openlocfilehash: 748f1994d0ee5b6c6a988bca8e5b694b29c0b8c5
ms.contentlocale: nl-nl
ms.lasthandoff: 06/30/2017

---

<a id="create-your-first-jenkins-master-on-a-linux-ubuntu-vm-on-azure" class="xliff"></a>

# Uw eerste Jenkins-master op een Linux (Ubuntu)-VM op Azure maken

Deze Quick Start toont hoe u de nieuwste stabiele Jenkins-versie installeert op een Linux (Ubuntu 14.04 TNS)-VM samen met de hulpprogramma's en invoegtoepassingen die zijn geconfigureerd om te werken met Azure. Tot de hulpmiddelen behoren:
<ul>
<li>GIT voor bronbeheer</li>
<li>Azure referentie-invoegtoepassing om veilig verbinding te maken</li>
<li>Azure VM Agents-invoegtoepassing voor flexibel bouwen, testen en continue integratie</li>
<li>Azure Storage-invoegtoepassing voor de opslag van artefacten</li>
<li>Azure CLI voor het implementeren van apps met behulp van scripts</li>
</ul>

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Maak een gratis Azure-account.
> * Maak een Jenkins-master op een Azure-VM met een oplossingssjabloon. 
> * Voer de eerste configuratie voor Jenkins uit.
> * Installeer voorgestelde invoegtoepassingen.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

<a id="create-the-vm-in-azure-by-deploying-the-solution-template-for-jenkins" class="xliff"></a>

## Maak de virtuele machine in Azure door de oplossingssjabloon voor Jenkins te implementeren

Met Azure-snelstartsjablonen kunt u snel en betrouwbaar complexe technologie in Azure implementeren.  Met Azure Resource Manager kunt u uw toepassingen inrichten aan de hand van een [declaratieve sjabloon.](https://azure.microsoft.com/en-us/resources/templates/?term=jenkins) U kunt in één enkele sjabloon meerdere services plus de bijbehorende afhankelijkheden implementeren. U gebruikt dezelfde sjabloon om uw toepassing herhaaldelijk te implementeren in elke fase van de levenscyclus van de toepassing.

Bekijk [abonnementen en prijzen](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/bitnami.jenkins?tab=PlansAndPrice) voor deze sjabloon voor informatie over de verschillende opties.

Ga naar [De marktplaatsinstallatiekopie voor Jenkins](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/azure-oss.jenkins?tab=Overview) en klik op **NU OPHALEN**  

Klik in Azure Portal op **Maken**.  Deze sjabloon vereist het gebruik van Resource Manager zodat de vervolgkeuzelijst voor het sjabloonmodel is uitgeschakeld.
   
![Dialoogvenster Azure-portal](./media/install-jenkins-solution-template/ap-create.png)

In het tabblad **Basisinstellingen configureren**:

![Basisinstellingen configureren](./media/install-jenkins-solution-template/ap-basic.png)

* Geef een naam aan uw Jenkins-instantie.
* Selecteer een VM-schijftype.  Kies een grotere virtuele machine en SSD voor betere prestaties voor productieworkloads.  Meer informatie over Azure-schijftypes vindt u [hier.](https://docs.microsoft.com/en-us/azure/storage/storage-premium-storage)
* Gebruikersnaam: moet voldoen aan lengtevereisten en mag geen gereserveerde woorden of niet-ondersteunde tekens bevatten. Namen als 'admin' zijn niet toegestaan.  Kijk [hier](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/faq) voor meer informatie voor de vereisten van de gebruikersnaam en het wachtwoord.
* Verificatietype: maak een instantie die wordt beveiligd door een wachtwoord of [openbare SSH-sleutel](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/ssh-from-windows). Als u een wachtwoord gebruikt, moet dat aan 3 van de volgende voorwaarden voldoen: één kleine letter, één hoofdletter, één cijfer en één speciaal teken.
* Laat het Jenkins-releasetype op **TNS** staan
* Selecteer een abonnement.
* Maak een nieuwe resourcegroep of selecteer een bestaande.
* Selecteer een locatie.

Op het tabblad **Extra opties configureren**:

![selecteer aanvullende opties](./media/install-jenkins-solution-template/ap-addtional.png)

* geef een domeinnaamlabel op als unieke identificatie van de Jenkins-master.

Klik op **OK** om naar de volgende stap te gaan. 

Nadat de validatie is geslaagd, klikt u op **OK** om de sjabloon en de parameters te downloaden. 

Selecteer vervolgens **Kopen** voor het inrichten van alle resources.

<a id="setup-ssh-port-forwarding" class="xliff"></a>

## Doorsturen via SSH-poort instellen

Standaard gebruikt de Jenkins-instantie het http-protocol en luistert op poort 8080. Gebruikers moeten niet worden geverifieerd via onbeveiligde protocollen.
    
Stel doorsturen via poort in voor het weergeven van de Jenkins-gebruikersinterface op uw lokale machine.

<a id="if-you-are-using-windows" class="xliff"></a>

### Als u van Windows gebruikmaakt:

Installeer PuTTY en voer deze opdracht uit als u een wachtwoord gebruikt voor het beveiligen van Jenkins:
```
putty.exe -ssh -L 8080:localhost:8080 <username>@<Domain name label>.<location>.cloudapp.azure.com
```
* Voer het wachtwoord in om u aan te melden.

![Voer het wachtwoord in om u aan te melden.](./media/install-jenkins-solution-template/jenkins-pwd.png)

Als u SSH gebruikt, moet u deze opdracht uitvoeren:
```
putty -i <private key file including path> -L 8080:localhost:8080 <username>@<Domain name label>.<location>.cloudapp.azure.com
```

<a id="if-you-are-using-linux-or-mac" class="xliff"></a>

### Als u een Linux- of Mac gebruikt:

Als u een wachtwoord gebruikt voor het beveiligen van uw Jenkins-master moet u deze opdracht uitvoeren:
```
ssh -L 8080:localhost:8080 <username>@<Domain name label>.<location>.cloudapp.azure.com
```
* Voer het wachtwoord in om u aan te melden.

Als u SSH gebruikt, moet u deze opdracht uitvoeren:
```
ssh -i <private key file including path> -L 8080:localhost:8080 <username>@<Domain name label>.<location>.cloudapp.azure.com
```

<a id="connect-to-jenkins" class="xliff"></a>

## Verbinding maken met Jenkins
Nadat u uw tunnel hebt gestart, gaat u naar http://localhost: 8080/ op uw lokale machine.

Ontgrendel het Jenkins-dashboard voor de eerste keer met het initiële beheerderswachtwoord.

![Jenkins ontgrendelen](./media/install-jenkins-solution-template/jenkins-unlock.png)

SSH naar de virtuele machine en voer `sudo cat /var/lib/jenkins/secrets/initialAdminPassword` uit om een token te krijgen.

![Jenkins ontgrendelen](./media/install-jenkins-solution-template/jenkins-ssh.png)

U wordt gevraagd de voorgestelde invoegtoepassingen te installeren.

Maak vervolgens een gebruiker met beheerdersrechten voor uw Jenkins-master.

Uw Jenkins-instantie is nu gereed om te gebruiken. U kunt een alleen-lezen weergave openen door naar http://\<openbare DNS-naam van de instantie die u zojuist hebt gemaakt\> te gaan.

![Jenkins is klaar.](./media/install-jenkins-solution-template/jenkins-welcome.png)

<a id="next-steps" class="xliff"></a>

## Volgende stappen

In deze zelfstudie hebt u:

> [!div class="checklist"]
> * Een Jenkins-master gemaakt met de oplossingssjabloon.
> * De initiële configuratie van Jenkins uitgevoerd.
> * Invoegtoepassingen geïnstalleerd.

Volg deze link om te zien hoe u Azure VM-Agents kunt gebruiken voor continue integratie met Jenkins.

> [!div class="nextstepaction"]
> [Virtuele machines in Azure als Jenkins-agents](jenkins-azure-vm-agents.md)

