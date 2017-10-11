



Afhankelijk van uw omgeving en opties, kunt het script alle de clusterinfrastructuur, met inbegrip van het virtuele netwerk van Azure storage-accounts, cloudservices, domeincontroller, lokale of externe SQL-databases, hoofdknooppunt en extra clusterknooppunten maken. U kunt ook het script vooraf bestaande Azure-infrastructuur gebruiken en alleen de knooppunten in de HPC-cluster maken.

Zie voor achtergrondinformatie over het plannen van een cluster HPC Pack, de [productevaluatie en Planning](https://technet.microsoft.com/library/jj899596.aspx) en [aan de slag](https://technet.microsoft.com/library/jj899590.aspx) inhoud in de TechNet Library voor HPC Pack 2012 R2.

## <a name="prerequisites"></a>Vereisten
* **Azure-abonnement**: U kunt een abonnement in de globale Azure of Azure China service gebruiken. Uw abonnement limieten van invloed op het aantal en type van clusterknooppunten die u kunt implementeren. Zie voor informatie [Azure-abonnement en Servicelimieten, quota's en beperkingen](../articles/azure-subscription-service-limits.md).
* **Windows-clientcomputer met Azure PowerShell 0.8.10 of hoger geïnstalleerd en geconfigureerd**: Zie [aan de slag met Azure PowerShell](/powershell/azureps-cmdlets-docs) voor installatie-instructies en stappen verbinding maken met uw Azure-abonnement.
* **HPC Pack IaaS-implementatiescript**: downloaden en uitpakken van de meest recente versie van het script van de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Controleer de versie van het script dat door het uitvoeren van `New-HPCIaaSCluster.ps1 –Version`. In dit artikel is gebaseerd op versie 4.5.2 van het script.
* **Script-configuratiebestand**: Maak een XML-bestand dat het script gebruikt om de HPC-cluster te configureren. Zie voor informatie over en voorbeelden secties verderop in dit artikel en het bestand Manual.rtf die wordt meegestuurd met het script voor implementatie.

## <a name="syntax"></a>Syntaxis
```PowerShell
New-HPCIaaSCluster.ps1 [-ConfigFile] <String> [-AdminUserName]<String> [[-AdminPassword] <String>] [[-HPCImageName] <String>] [[-LogFile] <String>] [-Force] [-NoCleanOnFailure] [-PSSessionSkipCACheck] [<CommonParameters>]
```
> [!NOTE]
> Voer het script als beheerder.
> 
> 

### <a name="parameters"></a>Parameters
* **ConfigFile**: Hiermee geeft u het bestandspad van het configuratiebestand te beschrijven de HPC-cluster. Zie voor meer informatie over het configuratiebestand in dit onderwerp of in het bestand Manual.rtf in de map waarin het script.
* **AdminUserName**: Hiermee geeft u de gebruikersnaam. Als het domein-forest is gemaakt door het script, wordt dit de gebruikersnaam van de lokale beheerder voor alle virtuele machines en de domeinnaam van de beheerder. Als het domeinforest al bestaat, geeft u hiermee de domeingebruiker als de gebruikersnaam van de lokale beheerder HPC Pack te installeren.
* **AdminPassword**: Hiermee geeft u het administrator wachtwoord. Als op de opdrachtregel niet is opgegeven, wordt het script vraagt u het wachtwoord.
* **HPCImageName** (optioneel): Hiermee geeft u de naam van de HPC Pack VM-installatiekopie gebruikt voor het implementeren van de HPC-cluster. Het moet de installatiekopie van een opgegeven Microsoft HPC Pack vanuit Azure Marketplace. Als niet wordt opgegeven (meestal aanbevolen), het script kiest de meest recente gepubliceerde [HPC Pack 2012 R2-afbeelding](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/). De meest recente installatiekopie is gebaseerd op Windows Server 2012 R2 Datacenter met HPC Pack 2012 R2 Update 3 is geïnstalleerd.
  
  > [!NOTE]
  > Implementatie mislukt als u een geldige afbeelding zijn HPC Pack niet opgeeft.
  > 
  > 
* **LogFile** (optioneel): Hiermee geeft u het logboekbestandspad van de implementatie. Als u niet opgeeft, wordt het script een logboekbestand gemaakt in de map temp van de computer waarop het script wordt uitgevoerd.
* **Force** (optioneel): de bevestiging vragen onderdrukt.
* **NoCleanOnFailure** (optioneel): Hiermee geeft u de Azure VM's zijn niet geïmplementeerd worden niet verwijderd. Deze virtuele machines handmatig verwijderen voordat u het script om door te gaan met de implementatie of de implementatie mislukken.
* **PSSessionSkipCACheck** (optioneel): voor elke cloudservice met virtuele machines die door dit script wordt geïmplementeerd, wordt automatisch een zelfondertekend certificaat gegenereerd door Azure en alle virtuele machines in de cloudservice dit certificaat als externe Windows standaardwaarde gebruiken Certificate Management (WinRM). Als u wilt implementeren HPC-functies in deze virtuele Azure-machines, het script standaard, wordt tijdelijk geïnstalleerd deze certificaten op de lokale Computer\\vertrouwde basiscertificeringsinstanties van de client de beveiligingsfout 'niet-vertrouwde Certificeringsinstantie' onderdrukken tijdens het uitvoeren van script. De certificaten worden verwijderd wanneer het script is voltooid. Als deze parameter wordt opgegeven, de certificaten zijn niet geïnstalleerd op de clientcomputer en de beveiligingswaarschuwing wordt onderdrukt.
  
  > [!IMPORTANT]
  > Deze parameter wordt niet aanbevolen voor productie-implementaties.
  > 
  > 

### <a name="example"></a>Voorbeeld
Het volgende voorbeeld wordt een HPC Pack-cluster met behulp van het configuratiebestand *MyConfigFile.xml*, en Hiermee geeft u de beheerdersreferenties voor het installeren van het cluster.

```PowerShell
.\New-HPCIaaSCluster.ps1 –ConfigFile MyConfigFile.xml -AdminUserName <username> –AdminPassword <password>
```

### <a name="additional-considerations"></a>Aanvullende overwegingen
* Het script kunt taak indienen via de webportal HPC Pack of de REST-API van HPC Pack optioneel in staat.
* Het script kunt eventueel aangepaste scripts voor vóór en na configuratie uitvoeren op het hoofdknooppunt desgewenst extra software te installeren of andere instellingen te configureren.

## <a name="configuration-file"></a>Configuratiebestand
Het configuratiebestand voor het script voor implementatie is een XML-bestand. Het schemabestand HPCIaaSClusterConfig.xsd is in de map HPC Pack IaaS implementatie script. **IaaSClusterConfig** is het hoofdelement van het configuratiebestand dat de onderliggende elementen beschreven in het bestand Manual.rtf in de script-map van het implementatietype bevat.

