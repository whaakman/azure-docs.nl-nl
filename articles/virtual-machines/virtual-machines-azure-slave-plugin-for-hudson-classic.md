---
title: De ondergeschikte Azure-Plugin gebruiken met Hudson CI | Microsoft Docs
description: Beschrijft hoe u Azure slave-invoegtoepassing gebruiken met Hudson CI.
services: virtual-machines-linux
documentationcenter: ''
author: rmcmurray
manager: wpickett
editor: ''
ms.assetid: b2083d1c-4de8-4a19-a615-ccc9d9b6e1d9
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: ef24e356c9ac8424fc519a3b16af5d37a20e706f
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57999793"
---
# <a name="how-to-use-the-azure-slave-plug-in-with-hudson-continuous-integration"></a>De ondergeschikte Azure-Plugin gebruiken met Hudson CI
Invoegtoepassing voor Hudson slave Azure kunt u de ondergeschikte knooppunten op Azure in te richten bij het uitvoeren van gedistribueerde bouwt.

## <a name="install-the-azure-slave-plug-in"></a>De Azure Slave-invoegtoepassing installeren
1. Klik in het dashboard Hudson **Hudson beheren**.
2. In de **beheren Hudson** pagina, klikt u op **Plug-ins beheren**.
3. Klik op de **beschikbaar** tabblad.
4. Klik op **zoeken** en het type **Azure** om te beperken van de lijst om te relevante invoegtoepassingen.
   
    Als u ervoor kiezen om te bladeren door de lijst met beschikbare invoegtoepassingen, vindt u de Azure slave invoegtoepassing onder de **Clusterbeheer en gedistribueerde bouwen** sectie de **anderen** tabblad.
5. Schakel het selectievakje voor **Azure Slave Plugin**.
6. Klik op **Install**.
7. Opnieuw opstarten Hudson.

Nu dat de invoegtoepassing is geïnstalleerd, worden de volgende stappen uit het configureren van de invoegtoepassing met uw Azure-abonnementsprofiel en een sjabloon te maken die wordt gebruikt bij het maken van de virtuele machine voor de slave-knooppunt.

## <a name="configure-the-azure-slave-plug-in-with-your-subscription-profile"></a>De Azure Slave-invoegtoepassing configureren met uw abonnementsprofiel
Een abonnementsprofiel, ook wel aangeduid als de publicatie-instellingen, is een XML-bestand met beveiligde referenties en aanvullende informatie, moet u werken met Azure in uw ontwikkelingsomgeving. Voor het configureren van de Azure slave-invoegtoepassing, hebt u het volgende nodig:

* Uw abonnements-id
* Een beheercertificaat voor uw abonnement

Deze kunnen worden gevonden in uw [abonnementsprofiel]. Hieronder volgt een voorbeeld van een abonnementsprofiel.

    <?xml version="1.0" encoding="utf-8"?>

        <PublishData>

          <PublishProfile SchemaVersion="2.0" PublishMethod="AzureServiceManagementAPI">

        <Subscription

              ServiceManagementUrl="https://management.core.windows.net"

              Id="<Subscription ID>"

              Name="Pay-As-You-Go"
            ManagementCertificate="<Management certificate value>" />

          </PublishProfile>

    </PublishData>

Nadat u het abonnementsprofiel van uw hebt, volgt u deze stappen voor het configureren van de Azure slave-invoegtoepassing.

1. Klik in het dashboard Hudson **Hudson beheren**.
2. Klik op **systeem configureren**.
3. Schuif omlaag in de pagina en zoek de **Cloud** sectie.
4. Klik op **toevoegen van nieuwe cloud > Microsoft Azure**.
   
    ![nieuwe cloud toevoegen][add new cloud]
   
    Hiermee wordt de velden weergegeven waarin u wilt invoeren van gegevens van uw abonnement.
   
    ![profiel configureren][configure profile]
5. De abonnement-id en beheercertificaat van uw abonnementsprofiel voor het Kopieer en plak deze in de juiste velden.
   
    Bij het kopiëren van de abonnement-id en beheercertificaat, **niet** inclusief de aanhalingstekens die de waarden.
6. Klik op **Verify configuration**.
7. Wanneer de configuratie is geverifieerd, klikt u op **opslaan**.

## <a name="set-up-a-virtual-machine-template-for-the-azure-slave-plug-in"></a>Een virtuele-machinesjabloon instellen voor de Azure-Slave plug-in
Een virtuele-machinesjabloon definieert de parameters die de invoegtoepassing wordt gebruikt om u te maken van een knooppunt slave op Azure. In de volgende stappen maakt we sjabloon voor een Ubuntu-VM.

1. Klik in het dashboard Hudson **Hudson beheren**.
2. Klik op **systeem configureren**.
3. Schuif omlaag in de pagina en zoek de **Cloud** sectie.
4. Binnen de **Cloud** sectie, zoeken **Azure VM-sjabloon toevoegen** en klikt u op de **toevoegen** knop.
   
    ![vm-sjabloon toevoegen][add vm template]
5. Geef de naam van een cloudservice in de **naam** veld. Als de opgegeven naam naar een bestaande cloudservice verwijst, wordt de virtuele machine worden ingericht in die service. Azure wordt anders maakt u een nieuwe.
6. In de **beschrijving** veld, typ een beschrijving van de sjabloon die u maakt. Deze informatie is uitsluitend bestemd voor administratieve doeleinden, en wordt niet gebruikt in een virtuele machine wordt ingericht.
7. In de **Labels** veld **linux**. Dit label wordt gebruikt voor het identificeren van de sjabloon die u maakt en vervolgens wordt gebruikt om te verwijzen naar de sjabloon bij het maken van een taak Hudson.
8. Selecteer een regio waar de virtuele machine wordt gemaakt.
9. Selecteer de juiste VM-grootte.
10. Geef een opslagaccount waar de virtuele machine wordt gemaakt. Zorg ervoor dat deze zich in dezelfde regio als de cloudservice die u wilt gebruiken. Als u wilt dat nieuwe opslag moet worden gemaakt, kunt u dit veld leeg laten.
11. Bewaartijd Hiermee geeft u het aantal minuten voordat Hudson een niet-actieve ondergeschikte server wordt verwijderd. Laat dit op de standaardwaarde van 60.
12. In **gebruik**, selecteert u de juiste voorwaarde wanneer dit knooppunt ondergeschikte server wordt gebruikt. Selecteer nu **gebruikmaken van dit knooppunt zo veel mogelijk**.
    
     Op dit moment zou uw formulier er enigszins ongeveer als volgt:
    
     ![sjabloonconfiguratie][template config]
13. In **installatiekopie familie of Id** u moet opgeven welke installatiekopie wordt geïnstalleerd op de virtuele machine. U kunt selecteren uit een lijst van families van de installatiekopie of een aangepaste installatiekopie opgeven.
    
     Als u kiezen uit een lijst met afbeelding families wilt, voert u het eerste teken (hoofdlettergevoelig) van de naam van de installatiekopie-familie. Bijvoorbeeld: typen **U** verschijnt nu een lijst met Ubuntu-Server-families. Wanneer u in de lijst selecteert, wordt Jenkins de nieuwste versie van die installatiekopie van die familie gebruiken bij het inrichten van uw virtuele machine.
    
     ![OS-familie lijst][OS family list]
    
     Als u een aangepaste installatiekopie die u wilt gebruiken in plaats daarvan hebt, voer de naam van die aangepaste installatiekopie. Namen van aangepaste installatiekopieën worden niet weergegeven in een lijst, zodat u ervoor te zorgen dat de naam correct is opgegeven.    
    
     Voor deze zelfstudie, typt u **U** een lijst met Ubuntu-installatiekopieën en selecteer **Ubuntu Server 14.04 TNS**.
14. Voor **Launch method**, selecteer **SSH**.
15. Het onderstaande script kopiëren en plakken in de **Init script** veld.
    
         # Install Java
    
         sudo apt-get -y update
    
         sudo apt-get install -y openjdk-7-jdk
    
         sudo apt-get -y update --fix-missing
    
         sudo apt-get install -y openjdk-7-jdk
    
         # Install git
    
         sudo apt-get install -y git
    
         #Install ant
    
         sudo apt-get install -y ant
    
         sudo apt-get -y update --fix-missing
    
         sudo apt-get install -y ant
    
     De **Init script** wordt uitgevoerd nadat de virtuele machine is gemaakt. In dit voorbeeld wordt het script geïnstalleerd Java, git en ant.
16. In de **gebruikersnaam** en **wachtwoord** velden, voert u de gewenste waarden voor de administrator-account die wordt gemaakt op de virtuele machine.
17. Klik op **Verify Template** om te controleren of de opgegeven parameters geldig zijn.
18. Klik op **Opslaan**.

## <a name="create-a-hudson-job-that-runs-on-a-slave-node-on-azure"></a>Maak een Hudson-taak die wordt uitgevoerd op een knooppunt ondergeschikte server op Azure
In deze sectie maakt u een Hudson-taak die wordt uitgevoerd op een knooppunt ondergeschikte server op Azure.

1. Klik in het dashboard Hudson **nieuwe taak**.
2. Voer een naam voor de taak die u maakt.
3. Selecteer voor het taaktype **bouwen van een taak free-style software**.
4. Klik op **OK**.
5. Selecteer in de configuratiepagina van de taak **beperken waar dit project kan worden uitgevoerd**.
6. Selecteer **knooppunt en het label menu** en selecteer **linux** (We hebben opgegeven dit label bij het maken van de virtuele-machinesjabloon in de vorige sectie).
7. In de **bouwen** sectie, klikt u op **build-stap toevoegen** en selecteer **shell uitvoeren**.
8. Bewerken van het volgende script, vervangen **{uw github-account-name}**, **{naam van uw project}**, en **{uw projectmap}** met waarden van toepassing en plak het bewerkte script in het tekstvak dat wordt weergegeven.
   
        # Clone from git repo
   
        currentDir="$PWD"
   
        if [ -e {your project directory} ]; then
   
              cd {your project directory}
   
              git pull origin master
   
        else
   
              git clone https://github.com/{your github account name}/{your project name}.git
   
        fi
   
        # change directory to project
   
        cd $currentDir/{your project directory}
   
        #Execute build task
   
        ant
9. Klik op **Opslaan**.
10. In het dashboard Hudson vindt u de taak die u zojuist hebt gemaakt en klik op de **plannen van een build** pictogram.

Hudson wordt vervolgens maakt u een ondergeschikte server-knooppunt met behulp van de sjabloon in de vorige sectie hebt gemaakt en voert u het script dat u hebt opgegeven in de build-stap voor deze taak.

## <a name="next-steps"></a>Volgende stappen
In het [Azure Java Developer Center] vindt u meer informatie over het gebruik van Azure met Java.

<!-- URL List -->

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[abonnementsprofiel]: https://go.microsoft.com/fwlink/?LinkID=396395

<!-- IMG List -->

[add new cloud]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addcloud.png
[configure profile]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-configureprofile.png
[add vm template]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addnewvmtemplate.png
[template config]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-templateconfig1-withdata.png
[OS family list]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-oslist.png

