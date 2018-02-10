1. Het installatieprogramma kopiëren naar een lokale map (bijvoorbeeld map) op de server die u wilt beveiligen. Voer de volgende opdrachten in een terminal:
  ```
  cd /tmp ;

  tar -xvzf Microsoft-ASR_UA*release.tar.gz
  ```
2. Voer de volgende opdracht voor het installeren van de Mobility-Service:

  ```
  sudo ./install -d <Install Location> -r MS -v VmWare -q
  ```
3. Zodra de installatie is voltooid, moet de Mobility-Service ophalen geregistreerd en de configuratieserver. Voer de volgende opdracht voor het registreren van de Mobility-Service met de configuratieserver.

  ```
  /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <CSIP> -P /var/passphrase.txt
  ```

#### <a name="mobility-service-installer-command-line"></a>Mobility-Service installer-opdrachtregel

```
Usage:
./install -d <Install Location> -r <MS|MT> -v VmWare -q
```

|Parameter|Type|Beschrijving|Mogelijke waarden|
|-|-|-|-|
|-r |Verplicht|Geeft aan of de Mobility-Service (MS) moet worden geïnstalleerd of MasterTarget(MT) moet worden geïnstalleerd.|MS </br> MT|
|-d |Optioneel|Locatie waar de Mobility-Service moet worden geïnstalleerd|/usr/local/ASR|
|-v|Verplicht|Hiermee geeft u het platform waarop de Mobility-Service is ophalen geïnstalleerd </br> </br>-** VMware: deze waarde wordt gebruikt als u mobility-service op een virtuele machine uitgevoerd installeert op *VMware vSphere ESXi-Hosts*, * Hyper-V-Hosts en *fysieke Servers* </br> -** Azure: Gebruik deze waarde als u agent op een virtuele machine van Azure IaaS installeert| VMware </br> Azure|
|-q|Optioneel|Hiermee geeft u het installatieprogramma uitvoeren in de stille modus| N/A|


#### <a name="mobility-service-configuration-command-line"></a>Mobility-Service configuration vanaf de opdrachtregel

```
Usage:
cd /usr/local/ASR/Vx/bin
UnifiedAgentConfigurator.sh -i <CSIP> -P <PassphraseFilePath>
```

|Parameter|Type|Beschrijving|Mogelijke waarden|
|-|-|-|-|
|-i |Verplicht|IP-adres van de configuratieserver|Een geldig IP-adres|
|-P |Verplicht|Volledig pad naar het bestand waarin de wachtwoordzin voor verbinding is opgeslagen|Een geldige map|
