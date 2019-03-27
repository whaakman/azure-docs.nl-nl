---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 6f0d2d59ed50c743adb19027c404bfa83a1886f1
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58484862"
---
Afhankelijk van uw omgeving en de opties, kunt het script alle de clusterinfrastructuur, met inbegrip van het virtuele netwerk van Azure storage-accounts, cloudservices, domeincontroller, lokale of externe SQL-databases, hoofdknooppunt en extra clusterknooppunten maken. U kunt ook het script vooraf bestaande Azure-infrastructuur gebruiken en alleen de knooppunten in de HPC-cluster maken.

Zie voor achtergrondinformatie over het plannen van een HPC Pack-cluster, de [productevaluatie en Planning](https://technet.microsoft.com/library/jj899596.aspx) en [aan de slag](https://technet.microsoft.com/library/jj899590.aspx) inhoud in de TechNet Library voor HPC Pack 2012 R2.

## <a name="prerequisites"></a>Vereisten
* **Azure-abonnement**: In de globale Azure of Azure China-service kunt u een abonnement. Limieten voor uw abonnement van invloed zijn op het aantal en type van de clusterknooppunten die u kunt implementeren. Zie voor meer informatie, [Azure-abonnement en Servicelimieten, quotums en beperkingen](../articles/azure-subscription-service-limits.md).
* **Windows-clientcomputer met Azure PowerShell 0.8.10 of hoger is geïnstalleerd en geconfigureerd**: Zie [aan de slag met Azure PowerShell](/powershell/azureps-cmdlets-docs) voor installatie-instructies en stappen om te verbinden met uw Azure-abonnement.
* **HPC Pack IaaS-implementatiescript**: Downloaden en uitpakken van de meest recente versie van het script uit de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Controleer de versie van het script door uit te voeren `New-HPCIaaSCluster.ps1 –Version`. In dit artikel is gebaseerd op versie 4.5.2 van het script.
* **Configuratie van scriptbestand**: Maak een XML-bestand dat het script wordt gebruikt om de HPC-cluster te configureren. Zie voor informatie over en voorbeelden, secties verderop in dit artikel en het bestand Manual.rtf die wordt meegestuurd met het implementatiescript.

## <a name="syntax"></a>Syntaxis
```powershell
New-HPCIaaSCluster.ps1 [-ConfigFile] <String> [-AdminUserName]<String> [[-AdminPassword] <String>] [[-HPCImageName] <String>] [[-LogFile] <String>] [-Force] [-NoCleanOnFailure] [-PSSessionSkipCACheck] [<CommonParameters>]
```
> [!NOTE]
> Voer het script als beheerder.
> 
> 

### <a name="parameters"></a>Parameters
* **ConfigFile**: Hiermee geeft u het pad van het configuratiebestand voor het beschrijven van de HPC-cluster. Zie voor meer informatie over het configuratiebestand in dit onderwerp of in het bestand Manual.rtf in de map met het script.
* **AdminUserName**: Hiermee geeft u de naam van de gebruiker. Als de domein-forest is gemaakt door het script, wordt dit de naam van de lokale administrator-gebruiker voor alle virtuele machines en de domeinnaam van de beheerder. Als de domein-forest al bestaat, geeft dit de domeingebruiker als de naam van de lokale beheerder-gebruiker HPC Pack te installeren.
* **AdminPassword**: Hiermee geeft u het wachtwoord van de beheerder. Indien niet opgegeven in de opdrachtregel, wordt het script vraagt u het wachtwoord.
* **HPCImageName** (optioneel): Hiermee geeft u de naam van de HPC Pack VM-installatiekopie gebruikt voor het implementeren van de HPC-cluster. Het moet een geleverde Microsoft HPC Pack-installatiekopie uit de Azure Marketplace. Als niet is opgegeven (meestal aanbevolen), het script kiest de meest recente gepubliceerde [HPC Pack 2012 R2-image](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/). De meest recente installatiekopie is gebaseerd op Windows Server 2012 R2 Datacenter met HPC Pack 2012 R2 Update 3 is geïnstalleerd.
  
  > [!NOTE]
  > Implementatie mislukt als u een geldige HPC Pack-afbeelding niet opgeeft.
  > 
  > 
* **Het logboekbestand** (optioneel): Hiermee geeft u het pad van het logboek van de implementatie. Indien niet opgegeven, wordt in het script wordt een logboekbestand gemaakt in de map temp van de computer die het script is uitgevoerd.
* **Force** (optioneel): Onderdrukt de bevestigingspromts.
* **NoCleanOnFailure** (optioneel): Hiermee geeft u de Azure-VM's die worden niet geïmplementeerd worden niet verwijderd. Deze virtuele machines handmatig verwijderen voordat u het script om door te gaan met de implementatie of de implementatie kan mislukken.
* **PSSessionSkipCACheck** (optioneel): Een zelfondertekend certificaat wordt automatisch gegenereerd door Azure voor elke cloudservice met virtuele machines die door dit script worden geïmplementeerd, en alle virtuele machines in de cloudservice gebruikt dit certificaat als het certificaat voor standaard Windows Remote Management (WinRM). Voor het implementeren van HPC-functies in deze Azure-VM's, het script wordt standaard tijdelijk installeert u deze certificaten in de lokale Computer\\vertrouwde basiscertificeringsinstanties van de clientcomputer moet worden onderdrukt de fout 'niet-vertrouwde Certificeringsinstantie'-beveiliging tijdens het uitvoeren van script. De certificaten worden verwijderd wanneer het script is voltooid. Als deze parameter is opgegeven, de certificaten zijn niet geïnstalleerd op de clientcomputer en de beveiligingswaarschuwing wordt onderdrukt.
  
  > [!IMPORTANT]
  > Deze parameter wordt niet aanbevolen voor productie-implementaties.
  > 
  > 

### <a name="example"></a>Voorbeeld
Het volgende voorbeeld wordt een HPC Pack-cluster met behulp van het configuratiebestand *MyConfigFile.xml*, en geeft u beheerdersreferenties voor het installeren van het cluster.

```powershell
.\New-HPCIaaSCluster.ps1 –ConfigFile MyConfigFile.xml -AdminUserName <username> –AdminPassword <password>
```

### <a name="additional-considerations"></a>Aanvullende overwegingen
* Taken verzenden via de webportal van HPC Pack of de REST-API van HPC Pack kunt (optioneel) inschakelen door het script.
* Het script kan eventueel aangepaste scripts voor vóór en na configuratie uitgevoerd op het hoofdknooppunt als u wilt installeren van extra software of andere instellingen te configureren.

## <a name="configuration-file"></a>Configuratiebestand
Het configuratiebestand voor het script voor implementatie is een XML-bestand. Het schemabestand HPCIaaSClusterConfig.xsd is in de scriptmap van HPC Pack IaaS-implementatie. **IaaSClusterConfig** is het hoofdelement van het configuratiebestand, waarin de onderliggende elementen beschreven in het bestand Manual.rtf in de script-implementatiemap.

