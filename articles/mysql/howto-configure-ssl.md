---
title: Configureren van SSL-verbinding met het veilig verbinding maken met Azure Database for MySQL
description: Instructies voor het juiste configuratie van Azure Database for MySQL en gekoppelde toepassingen correct gebruik van SSL-verbindingen
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 07/02/2019
ms.openlocfilehash: 46aca2c1a7d40df69b89e15917ff07b983f5ff5f
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561474"
---
# <a name="configure-ssl-connectivity-in-your-application-to-securely-connect-to-azure-database-for-mysql"></a>Configureren van SSL-connectiviteit in uw toepassing veilig verbinding maken met Azure Database for MySQL
Azure Database for MySQL ondersteunt verbindingen van uw Azure Database voor MySQL-server voor clienttoepassingen met Secure Sockets Layer (SSL). Het afdwingen van SSL-verbindingen tussen uw databaseserver en clienttoepassingen zorgt dat u bent beschermt tegen 'man in the middle'-aanvallen omdat de gegevensstroom tussen de server en uw toepassing wordt versleuteld.

## <a name="step-1-obtain-ssl-certificate"></a>Stap 1: SSL-certificaat ophalen
Download het certificaat nodig om te communiceren via SSL met uw Azure Database voor MySQL-server uit [ https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem ](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) en sla het certificaatbestand op uw lokale schijf (in deze zelfstudie wordt c:\ssl bijvoorbeeld).
**Voor Microsoft Internet Explorer en Microsoft Edge:** Nadat het downloaden is voltooid, wijzig de BaltimoreCyberTrustRoot.crt.pem van het certificaat.

## <a name="step-2-bind-ssl"></a>Stap 2: Bind SSL

Voor specifieke programming taal met verbindingsreeksen, raadpleegt u de [voorbeeldcode](howto-configure-ssl.md#sample-code) hieronder.

### <a name="connecting-to-server-using-mysql-workbench-over-ssl"></a>Verbinding maken met server met behulp van MySQL Workbench via SSL
MySQL Workbench veilig via SSL verbinding te configureren. 

1. In het dialoogvenster Nieuwe verbinding instellen, gaat u naar de **SSL** tabblad. 

1. Update de **SSL gebruiken** veld 'Vereist'.

1. In de **SSL CA-bestand:** en voer de locatie van het bestand van de **BaltimoreCyberTrustRoot.crt.pem**. 
    
    ![SSL-configuratie opslaan](./media/howto-configure-ssl/mysql-workbench-ssl.png)

U kunt voor bestaande verbindingen binden van SSL met de rechtermuisknop op het pictogram voor verbinding en kiest u bewerken. Navigeer naar de **SSL** tabblad en een binding van het certificaat-bestand.

### <a name="connecting-to-server-using-the-mysql-cli-over-ssl"></a>Verbinding maken met server met behulp van de CLI MySQL via SSL
Een andere manier om de SSL-certificaat binden is het gebruik van de MySQL-opdrachtregelinterface door het uitvoeren van de volgende opdrachten. 

```bash
mysql.exe -h mydemoserver.mysql.database.azure.com -u Username@mydemoserver -p --ssl-mode=REQUIRED --ssl-ca=c:\ssl\BaltimoreCyberTrustRoot.crt.pem
```

> [!NOTE]
> Wanneer u de opdrachtregelinterface van MySQL op Windows, wordt u mogelijk een foutbericht `SSL connection error: Certificate signature check failed`. Als dit gebeurt, vervangt u de `--ssl-mode=REQUIRED --ssl-ca={filepath}` parameters met `--ssl`.

## <a name="step-3--enforcing-ssl-connections-in-azure"></a>Stap 3:  Afdwingen van SSL-verbindingen in Azure 
### <a name="using-the-azure-portal"></a>Azure Portal gebruiken
Met behulp van de Azure-portal, gaat u naar uw Azure Database voor MySQL-server en klik vervolgens op **verbindingsbeveiliging**. Gebruik de knop in-/ uitschakelen om in- of uitschakelen de **afdwingen van SSL-verbinding** instellen en klik vervolgens op **opslaan**. Microsoft raadt aan om in te schakelen altijd de **afdwingen van SSL-verbinding** instellen voor een betere beveiliging.
![enable-ssl](./media/howto-configure-ssl/enable-ssl.png)

### <a name="using-azure-cli"></a>Azure CLI gebruiken
U kunt inschakelen of uitschakelen de **ssl afdwingen** parameter met behulp van respectievelijk ingeschakeld of uitgeschakeld waarden in de Azure CLI.
```azurecli-interactive
az mysql server update --resource-group myresource --name mydemoserver --ssl-enforcement Enabled
```

## <a name="step-4-verify-the-ssl-connection"></a>Stap 4: Controleer of de SSL-verbinding
Uitvoeren van de mysql **status** opdracht uit om te controleren of u hebt verbonden met uw MySQL-server met behulp van SSL:
```dos
mysql> status
```
Controleer of dat de verbinding is versleuteld aan de hand van de uitvoer, die moet worden weergegeven:  **SSL: Codering in gebruik is AES256 SHA** 

## <a name="sample-code"></a>Voorbeeldcode
Als u wilt geen veilige verbinding maken met Azure Database for MySQL via SSL van uw toepassing, raadpleegt u de volgende codevoorbeelden:

### <a name="php"></a>PHP
```php
$conn = mysqli_init();
mysqli_ssl_set($conn,NULL,NULL, "/var/www/html/BaltimoreCyberTrustRoot.crt.pem", NULL, NULL) ; 
mysqli_real_connect($conn, 'mydemoserver.mysql.database.azure.com', 'myadmin@mydemoserver', 'yourpassword', 'quickstartdb', 3306, MYSQLI_CLIENT_SSL);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}
```
### <a name="php-using-pdo"></a>PHP (met behulp van de PDO)
```phppdo
$options = array(
    PDO::MYSQL_ATTR_SSL_CA => '/var/www/html/BaltimoreCyberTrustRoot.crt.pem'
);
$db = new PDO('mysql:host=mydemoserver.mysql.database.azure.com;port=3306;dbname=databasename', 'username@mydemoserver', 'yourpassword', $options);
```
### <a name="python-mysqlconnector-python"></a>Python (MySQLConnector Python)
```python
try:
    conn=mysql.connector.connect(user='myadmin@mydemoserver', 
        password='yourpassword', 
        database='quickstartdb', 
        host='mydemoserver.mysql.database.azure.com', 
        ssl_ca='/var/www/html/BaltimoreCyberTrustRoot.crt.pem')
except mysql.connector.Error as err:
    print(err)
```

### <a name="python-pymysql"></a>Python (PyMySQL)
```python
conn = pymysql.connect(user = 'myadmin@mydemoserver', 
        password = 'yourpassword', 
        database = 'quickstartdb', 
        host = 'mydemoserver.mysql.database.azure.com', 
        ssl = {'ssl': {'ssl-ca': '/var/www/html/BaltimoreCyberTrustRoot.crt.pem'}})
```

### <a name="django-pymysql"></a>Django (PyMySQL)
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'quickstartdb',
        'USER': 'myadmin@mydemoserver',
        'PASSWORD': 'yourpassword',
        'HOST': 'mydemoserver.mysql.database.azure.com',
        'PORT': '3306',
        'OPTIONS': {
            'ssl': {'ssl-ca': '/var/www/html/BaltimoreCyberTrustRoot.crt.pem'}
        }
    }
}
```

### <a name="ruby"></a>Ruby
```ruby
client = Mysql2::Client.new(
        :host     => 'mydemoserver.mysql.database.azure.com',
        :username => 'myadmin@mydemoserver',
        :password => 'yourpassword',
        :database => 'quickstartdb',
        :ssl_ca => '/var/www/html/BaltimoreCyberTrustRoot.crt.pem'
    )
```

### <a name="golang"></a>Golang
```go
rootCertPool := x509.NewCertPool()
pem, _ := ioutil.ReadFile("/var/www/html/BaltimoreCyberTrustRoot.crt.pem")
if ok := rootCertPool.AppendCertsFromPEM(pem); !ok {
    log.Fatal("Failed to append PEM.")
}
mysql.RegisterTLSConfig("custom", &tls.Config{RootCAs: rootCertPool})
var connectionString string
connectionString = fmt.Sprintf("%s:%s@tcp(%s:3306)/%s?allowNativePasswords=true&tls=custom",'myadmin@mydemoserver' , 'yourpassword', 'mydemoserver.mysql.database.azure.com', 'quickstartdb')   
db, _ := sql.Open("mysql", connectionString)
```
### <a name="java-mysql-connector-for-java"></a>Java (MySQL-Connector voor Java)
```java
# generate truststore and keystore in code
String importCert = " -import "+
    " -alias mysqlServerCACert "+
    " -file " + ssl_ca +
    " -keystore truststore "+
    " -trustcacerts " + 
    " -storepass password -noprompt ";
String genKey = " -genkey -keyalg rsa " +
    " -alias mysqlClientCertificate -keystore keystore " +
    " -storepass password123 -keypass password " + 
    " -dname CN=MS ";
sun.security.tools.keytool.Main.main(importCert.trim().split("\\s+"));
sun.security.tools.keytool.Main.main(genKey.trim().split("\\s+"));

# use the generated keystore and truststore 
System.setProperty("javax.net.ssl.keyStore","path_to_keystore_file");
System.setProperty("javax.net.ssl.keyStorePassword","password");
System.setProperty("javax.net.ssl.trustStore","path_to_truststore_file");
System.setProperty("javax.net.ssl.trustStorePassword","password");

url = String.format("jdbc:mysql://%s/%s?serverTimezone=UTC&useSSL=true", 'mydemoserver.mysql.database.azure.com', 'quickstartdb');
properties.setProperty("user", 'myadmin@mydemoserver');
properties.setProperty("password", 'yourpassword');
conn = DriverManager.getConnection(url, properties);
```
### <a name="java-mariadb-connector-for-java"></a>Java (MariaDB-Connector voor Java)
```java
# generate truststore and keystore in code
String importCert = " -import "+
    " -alias mysqlServerCACert "+
    " -file " + ssl_ca +
    " -keystore truststore "+
    " -trustcacerts " + 
    " -storepass password -noprompt ";
String genKey = " -genkey -keyalg rsa " +
    " -alias mysqlClientCertificate -keystore keystore " +
    " -storepass password123 -keypass password " + 
    " -dname CN=MS ";
sun.security.tools.keytool.Main.main(importCert.trim().split("\\s+"));
sun.security.tools.keytool.Main.main(genKey.trim().split("\\s+"));

# use the generated keystore and truststore 
System.setProperty("javax.net.ssl.keyStore","path_to_keystore_file");
System.setProperty("javax.net.ssl.keyStorePassword","password");
System.setProperty("javax.net.ssl.trustStore","path_to_truststore_file");
System.setProperty("javax.net.ssl.trustStorePassword","password");

url = String.format("jdbc:mariadb://%s/%s?useSSL=true&trustServerCertificate=true", 'mydemoserver.mysql.database.azure.com', 'quickstartdb');
properties.setProperty("user", 'myadmin@mydemoserver');
properties.setProperty("password", 'yourpassword');
conn = DriverManager.getConnection(url, properties);
```

### <a name="net-mysqlconnector"></a>.NET (MySqlConnector)
```csharp
var builder = new MySqlConnectionStringBuilder
{
    Server = "mydemoserver.mysql.database.azure.com",
    UserID = "myadmin@mydemoserver",
    Password = "yourpassword",
    Database = "quickstartdb",
    SslMode = MySqlSslMode.VerifyCA,
    CACertificateFile = "BaltimoreCyberTrustRoot.crt.pem",
};
using (var connection = new MySqlConnection(builder.ConnectionString))
{
    connection.Open();
}
```

## <a name="next-steps"></a>Volgende stappen
Bekijk verschillende connectiviteitsopties van toepassing na [verbindingsbibliotheken voor Azure Database for MySQL](concepts-connection-libraries.md)
