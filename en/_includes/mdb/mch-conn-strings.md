### clickhouse-client {#clickhouse-client}

**Before connecting:**

1. Connect the [DEB repository]({{ ch.docs }}/getting-started/install/#install-from-deb-packages) {{ CH }}:

   ```bash
   sudo apt update && sudo apt install --yes apt-transport-https ca-certificates dirmngr && \
   sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 8919F6BD******** && \
   echo "deb https://packages.{{ ch-domain }}/deb stable main" | sudo tee \
   /etc/apt/sources.list.d/clickhouse.list
   ```

1. Install the dependencies:

   ```bash
   sudo apt update && sudo apt install --yes clickhouse-client
   ```

1. Download the configuration file for `clickhouse-client`:

   {% include [ClickHouse client config](mch/client-config.md) %}

{% list tabs group=connection %}


- Connecting without SSL {#without-ssl}

   ```bash
   clickhouse-client --host <FQDN_of_any_{{ CH }}_host> \
                     --user <username> \
                     --database <DB_name> \
                     --port 9000 \
                     --ask-password
   ```


- Connecting via SSL {#with-ssl}

   {% include [default-connstring](./mch/default-connstring.md) %}

{% endlist %}

To learn how to get a host FQDN, see [this guide](../../managed-clickhouse/operations/connect.md#fqdn).

After running the command, enter the user password to complete the connection process.

Once connected to the DBMS, run `SELECT @@version;`.

### cURL {#curl}

{% list tabs group=connection %}


- Connecting without SSL {#without-ssl}

   ```bash
   curl --header "X-ClickHouse-User: <DB_username>" \
        --header "X-ClickHouse-Key: <DB_user_password>" \
        'http://<Any_{{ CH }}_host_FQDN>:8123/?database=<DB_name>&query=SELECT%20version()'
   ```


- Connecting via SSL {#with-ssl}

   ```bash
   curl --cacert {{ crt-local-dir }}{{ crt-local-file-root }} \
        --header "X-ClickHouse-User: <DB_username>" \
        --header "X-ClickHouse-Key: <DB_user_password>" \
        'https://<FQDN_of_any_{{ CH }}_host>:8443/?database=<DB_name>&query=SELECT%20version()'
   ```

{% endlist %}

To learn how to get a host FQDN, see [this guide](../../managed-clickhouse/operations/connect.md#fqdn).

### Go {#go}

**Before connecting, install the dependencies:**

```bash
sudo apt update && sudo apt install --yes golang git
```

{% list tabs group=connection %}


- Connecting without SSL {#without-ssl}

   `connect.go`

   ```go
   package main

   import (
       "fmt"
       "net/http"
       "io/ioutil"
   )

   func main() {

       const DB_HOST = "<FQDN_of_any_{{ CH }}_host>"
       const DB_NAME = "<DB_name>"
       const DB_USER = "<DB_user_name>"
       const DB_PASS = "<DB_user_password>"

       conn := &http.Client{
           Transport: &http.Transport{},
       }

       req, _ := http.NewRequest("GET", fmt.Sprintf("http://%s:8123/", DB_HOST), nil)
       query := req.URL.Query()
       query.Add("database", DB_NAME)
       query.Add("query", "SELECT version()")

       req.URL.RawQuery = query.Encode()

       req.Header.Add("X-ClickHouse-User", DB_USER)
       req.Header.Add("X-ClickHouse-Key", DB_PASS)

       resp, err := conn.Do(req)
       if err != nil {
           panic(err)
       }

       defer resp.Body.Close()

       data, _ := ioutil.ReadAll(resp.Body)
       fmt.Println(string(data))
   }
   ```


- Connecting via SSL {#with-ssl}

   `connect.go`

   ```go
   package main

   import (
       "fmt"
       "net/http"
       "io/ioutil"
       "crypto/x509"
       "crypto/tls"
   )

   func main() {

       const DB_HOST = "<FQDN_of_any_{{ CH }}_host>"
       const DB_NAME = "<DB_name>"
       const DB_USER = "<DB_user_name>"
       const DB_PASS = "<DB_user_password>"

       const CACERT = "{{ crt-local-dir }}{{ crt-local-file-root }}"

       caCert, err := ioutil.ReadFile(CACERT)
       if err != nil {
           panic(err)
       }
       caCertPool := x509.NewCertPool()
       caCertPool.AppendCertsFromPEM(caCert)
       conn := &http.Client{
           Transport: &http.Transport{
           TLSClientConfig: &tls.Config{
               RootCAs: caCertPool,
           },
       },
       }

       req, _ := http.NewRequest("GET", fmt.Sprintf("https://%s:8443/", DB_HOST), nil)
       query := req.URL.Query()
       query.Add("database", DB_NAME)
       query.Add("query", "SELECT version()")

       req.URL.RawQuery = query.Encode()

       req.Header.Add("X-ClickHouse-User", DB_USER)
       req.Header.Add("X-ClickHouse-Key", DB_PASS)

       resp, err := conn.Do(req)
       if err != nil {
           panic(err)
       }

       defer resp.Body.Close()

       data, _ := ioutil.ReadAll(resp.Body)
       fmt.Println(string(data))
   }
   ```

{% endlist %}

To learn how to get a host FQDN, see [this guide](../../managed-clickhouse/operations/connect.md#fqdn).

**Connection:**

```bash
go run connect.go
```

### Java {#java}

**Before connecting:**

1. Install the dependencies:

   ```bash
   sudo apt update && sudo apt install --yes default-jdk maven
   ```

1. Create a folder for the Maven project:

   ```bash
   cd ~/ && mkdir --parents project/src/java/com/example && cd project/
   ```

1. Create a configuration file for Maven:

   {% cut "pom.xml" %}

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">

     <modelVersion>4.0.0</modelVersion>
     <groupId>com.example</groupId>
     <artifactId>app</artifactId>
     <packaging>jar</packaging>
     <version>0.1.0</version>
     <properties>
       <maven.compiler.source>1.8</maven.compiler.source>
       <maven.compiler.target>1.8</maven.compiler.target>
     </properties>
     <dependencies>
       <dependency>
         <groupId>com.clickhouse</groupId>
         <artifactId>clickhouse-jdbc</artifactId>
         <version>0.2.4</version>
       </dependency>
       <dependency>
         <groupId>org.slf4j</groupId>
         <artifactId>slf4j-simple</artifactId>
         <version>1.7.30</version>
       </dependency>
     </dependencies>
     <build>
       <finalName>${project.artifactId}-${project.version}</finalName>
       <sourceDirectory>src</sourceDirectory>
       <resources>
         <resource>
           <directory>src</directory>
         </resource>
       </resources>
       <plugins>
         <plugin>
           <groupId>org.apache.maven.plugins</groupId>
           <artifactId>maven-assembly-plugin</artifactId>
           <executions>
             <execution>
               <goals>
                 <goal>attached</goal>
               </goals>
               <phase>package</phase>
               <configuration>
                 <descriptorRefs>
                   <descriptorRef>
                   jar-with-dependencies</descriptorRef>
                 </descriptorRefs>
                 <archive>
                   <manifest>
                     <mainClass>com.example.App</mainClass>
                   </manifest>
                 </archive>
               </configuration>
             </execution>
           </executions>
         </plugin>
         <plugin>
           <groupId>org.apache.maven.plugins</groupId>
           <artifactId>maven-jar-plugin</artifactId>
           <version>3.1.0</version>
           <configuration>
             <archive>
               <manifest>
                 <mainClass>com.example.App</mainClass>
               </manifest>
             </archive>
           </configuration>
         </plugin>
       </plugins>
     </build>
   </project>
   ```

   {% endcut %}

   Up-to-date versions of dependencies for Maven:

   * [clickhouse-jdbc](https://mvnrepository.com/artifact/com.clickhouse/clickhouse-jdbc)
   * [slf4j-simple](https://mvnrepository.com/artifact/org.slf4j/slf4j-simple)

{% list tabs group=connection %}


- Connecting without SSL {#without-ssl}

   `src/java/com/example/App.java`

   ```java
   package com.example;

   import java.sql.*;

   public class App {
       public static void main(String[] args) {
       String DB_HOST    = "<FQDN_of_any_{{ CH }}_host>";
       String DB_NAME    = "<DB_name>";
       String DB_USER    = "<DB_user_name>";
       String DB_PASS    = "<DB_user_password>";

       String DB_URL = String.format("jdbc:clickhouse://%s:8123/%s", DB_HOST, DB_NAME);

       try {
           Class.forName("com.clickhouse.jdbc.ClickHouseDriver");

           Connection conn = DriverManager.getConnection(DB_URL, DB_USER, DB_PASS);
           ResultSet rs = conn.createStatement().executeQuery("SELECT version()");
           if(rs.next()) {System.out.println(rs.getString(1));}

           conn.close();
       }
       catch(Exception ex) {ex.printStackTrace();}
       }
   }
   ```


- Connecting via SSL {#with-ssl}

   `src/java/com/example/App.java`

   ```java
   package com.example;

   import java.sql.*;

   public class App {
       public static void main(String[] args) {
       String DB_HOST    = "<FQDN_of_any_{{ CH }}_host>";
       String DB_NAME    = "<DB_name>";
       String DB_USER    = "<DB_user_name>";
       String DB_PASS    = "<DB_user_password>";

       String CACERT     = "{{ crt-local-dir }}{{ crt-local-file-root }}";

       String DB_URL = String.format("jdbc:clickhouse://%s:8443/%s?ssl=1&sslmode=strict&sslrootcert=%s", DB_HOST, DB_NAME, CACERT);

       try {
           Class.forName("com.clickhouse.jdbc.ClickHouseDriver");
           Connection conn = DriverManager.getConnection(DB_URL, DB_USER, DB_PASS);
           ResultSet rs = conn.createStatement().executeQuery("SELECT version()");
           if(rs.next()) {System.out.println(rs.getString(1));}

           conn.close();
       }
       catch(Exception ex) {ex.printStackTrace();}
       }
   }
   ```

{% endlist %}

To learn how to get a host FQDN, see [this guide](../../managed-clickhouse/operations/connect.md#fqdn).

**Connection:**

```bash
mvn clean package && \
java -jar target/app-0.1.0-jar-with-dependencies.jar
```

### Node.js {#nodejs}

**Before connecting, install the dependencies:**

```bash
sudo apt update && sudo apt install --yes nodejs npm && \
npm install querystring
```

{% list tabs group=connection %}


- Connecting without SSL {#without-ssl}

   `app.js`

   ```js
   "use strict"
   const http = require('http');
   const querystring = require('querystring');
   const fs = require('fs');

   const DB_HOST = "<FQDN_of_any_{{ CH }}_host>";
   const DB_NAME = "<DB_name>";
   const DB_USER = "<DB_user_name>";
   const DB_PASS = "<DB_user_password>";

   const options = {
       'method': 'GET',
       'path': '/?' + querystring.stringify({
           'database': DB_NAME,
           'query': 'SELECT version()',
       }),
       'port': 8123,
       'hostname': DB_HOST,
       'headers': {
           'X-ClickHouse-User': DB_USER,
           'X-ClickHouse-Key': DB_PASS,
       },
   };

   const rs = http.request(options, (res) => {
       res.setEncoding('utf8');
       res.on('data', (chunk) => {
           console.log(chunk);
       });
   });

   rs.end();
   ```


- Connecting via SSL {#with-ssl}

   `app.js`

   ```js
   "use strict"
   const https = require('https');
   const querystring = require('querystring');
   const fs = require('fs');

   const DB_HOST = "<FQDN_of_any_{{ CH }}_host>";
   const DB_NAME = "<DB_name>";
   const DB_USER = "<DB_user_name>";
   const DB_PASS = "<DB_user_password>";

   const CACERT = "{{ crt-local-dir }}{{ crt-local-file-root }}";

   const options = {
       'method': 'GET',
       'ca': fs.readFileSync(CACERT),
       'path': '/?' + querystring.stringify({
           'database': DB_NAME,
           'query': 'SELECT version()',
       }),
       'port': 8443,
       'hostname': DB_HOST,
       'headers': {
           'X-ClickHouse-User': DB_USER,
           'X-ClickHouse-Key': DB_PASS,
       },
   };

   const rs = https.request(options, (res) => {
       res.setEncoding('utf8');
       res.on('data', (chunk) => {
           console.log(chunk);
       });
   });

   rs.end();
   ```

{% endlist %}

To learn how to get a host FQDN, see [this guide](../../managed-clickhouse/operations/connect.md#fqdn).

**Connecting:**

```bash
node app.js
```

### ODBC {#odbc}

Setup methods for [Linux](#odbc-linux) and [Windows](#odbc-windows) are different.

#### Linux {#odbc-linux}

**Before connecting:**

1. Install the dependencies:

   ```bash
   sudo apt update && sudo apt install --yes git unixodbc build-essential cmake \
   libpoco-dev libssl-dev libicu-dev unixodbc-dev && \
   cd ~/ && git clone https://github.com/ClickHouse/clickhouse-odbc.git && \
   cd clickhouse-odbc/ && git submodule update --init
   ```

1. Build the driver from the source files:

   ```bash
   cd ~/clickhouse-odbc/ && mkdir build && cd build && \
   cmake -DCMAKE_BUILD_TYPE=RelWithDebInfo .. && \
   cmake --build . --config RelWithDebInfo
   ```

1. After the build process is complete, copy the driver files to the `/usr/local/lib64/` directory:

   ```bash
   sudo mkdir --parents /usr/local/lib64 && sudo cp driver/*.so /usr/local/lib64/
   ```

1. Register the {{ CH }} ODBC driver by adding the following lines to the `odbcinst.ini` file ([sample file](https://github.com/ClickHouse/clickhouse-odbc/blob/master/packaging/odbcinst.ini.sample)):

   {% cut "/etc/odbcinst.ini" %}

   ```ini
   [ODBC Drivers]
   ClickHouse ODBC Driver (ANSI)    = Installed
   ClickHouse ODBC Driver (Unicode) = Installed

   [ClickHouse ODBC Driver (ANSI)]
   Description = ODBC Driver (ANSI) for ClickHouse
   Driver      = /usr/local/lib64/libclickhouseodbc.so
   Setup       = /usr/local/lib64/libclickhouseodbc.so
   UsageCount  = 1

   [ClickHouse ODBC Driver (Unicode)]
   Description = ODBC Driver (Unicode) for ClickHouse
   Driver      = /usr/local/lib64/libclickhouseodbcw.so
   Setup       = /usr/local/lib64/libclickhouseodbcw.so
   UsageCount  = 1
   ```

   {% endcut %}

{% list tabs group=connection %}


- Connecting without SSL {#without-ssl}

   `/etc/odbc.ini`

   ```ini
   [ClickHouse]
   Driver = ClickHouse ODBC Driver (Unicode)
   Server = <FQDN_of_any_{{ CH }}_host>
   Database = <DB_name>
   UID = <DB_user_name>
   PWD = <DB_user_password>
   Port = 8123
   Proto = http
   ```


- Connecting via SSL {#with-ssl}

   `/etc/odbc.ini`

   ```ini
   [ClickHouse]
   Driver = ClickHouse ODBC Driver (Unicode)
   Server = <FQDN_of_any_{{ CH }}_host>
   Database = <DB_name>
   UID = <DB_user_name>
   PWD = <DB_user_password>
   Port = 8443
   Proto = https
   SSLMode = allow
   CertificateFile = {{ crt-local-dir }}{{ crt-local-file-root }}
   CALocation = /etc/ssl/certs/ca-certificates.crt
   ```

{% endlist %}

To learn how to get a host FQDN, see [this guide](../../managed-clickhouse/operations/connect.md#fqdn).

**Connection:**

```bash
isql -v ClickHouse
```

Once connected to the DBMS, run `SELECT @@version;`.

#### Windows {#odbc-windows}

1. [Install the clickhouse-odbc driver](https://github.com/ClickHouse/clickhouse-odbc#installation) with the appropriate bit depth. For example, if you are using a 32-bit application, install the driver with the same number of bits to connect through ODBC.
1. [Run the <q>ODBC Data Source Administrator</q>](https://learn.microsoft.com/en-us/sql/database-engine/configure-windows/open-the-odbc-data-source-administrator?view=sql-server-ver16).
1. In the **User DSN** tab, click **Add...**.
1. Select the {{ CH }} driver with suitable encoding and click **Done**.
1. Specify the parameters for connecting to the {{ CH }} cluster:

   {% list tabs group=connection %}

   - Connecting without SSL {#without-ssl}

      * **Name**: Name for the connection
      * **Host**: [FQDN of any {{ CH }} host](../../managed-clickhouse/operations/connect.md#fqdn)
      * **Port**: `{{ port-mch-http }}`
      * **Database**: DB name
      * **User**: DB user name
      * **Password**: DB user password

   - Connecting via SSL {#with-ssl}

      * **Name**: Name for the connection
      * **Host**: [FQDN of any {{ CH }} host](../../managed-clickhouse/operations/connect.md#fqdn)
      * **Port**: `{{ port-mch-http }}`
      * **Database**: DB name
      * **SSLMode**: `Allow`
      * **User**: DB user name
      * **Password**: DB user password

   {% endlist %}

1. Click **OK**.
1. Connect to the {{ CH }} cluster through ODBC, for example, using Microsoft Excel.

### PHP {#php}

**Before connecting:**

1. Install the dependencies:

   ```bash
   sudo apt update && sudo apt install --yes php
   ```

1. Make sure that the `allow_url_fopen` parameter is set to `On` in the PHP settings:

   ```bash
   cat /etc/php/7.4/apache2/php.ini | grep "allow_url_fopen"
   ```

   Otherwise, set this value and restart Apache:

   ```bash
   sudo sed -i 's/\(^allow_url_fopen = \).*/\1On/' /etc/php/7.4/apache2/php.ini && \
   sudo systemctl restart apache2
   ```

{% list tabs group=connection %}


- Connecting without SSL {#without-ssl}

   `connect.php`

   ```php
   <?php
       $host = '<FQDN_of_any_{{ CH }}_host>';
       $db = '<DB_name>';
       $query = 'SELECT version()';

       $auth = [
           'X-ClickHouse-User: <DB_user_name>',
           'X-ClickHouse-Key: <DB_user_password>',
       ];

       $context = stream_context_create([
           'http' => [
               'method' => 'GET',
               'protocol_version' => 1.1,
               'header' => $auth
           ]
       ]);

       $url = sprintf('http://%s:8123/?database=%s&query=%s', $host, $db, urlencode($query));

       $rs = file_get_contents($url, false, $context);
       print_r($rs);
   ?>
   ```


- Connecting via SSL {#with-ssl}

   `connect.php`

   ```php
   <?php
       $host = '<FQDN_of_any_{{ CH }}_host>';
       $db = '<DB_name>';
       $query = 'SELECT version()';

       $auth = [
           'X-ClickHouse-User: <DB_user_name>',
           'X-ClickHouse-Key: <DB_user_password>',
       ];

       $ssl = [
           'cafile' => '{{ crt-local-dir }}{{ crt-local-file-root }}',
           'verify_peer' => true,
       ];

       $context = stream_context_create([
           'http' => [
               'method' => 'GET',
               'protocol_version' => 1.1,
               'header' => $auth
           ],
           'ssl' => $ssl
       ]);

       $url = sprintf('https://%s:8443/?database=%s&query=%s', $host, $db, urlencode($query));

       $rs = file_get_contents($url, false, $context);
       print_r($rs);
   ?>
   ```

{% endlist %}

To learn how to get a host FQDN, see [this guide](../../managed-clickhouse/operations/connect.md#fqdn).

**Connection:**

```bash
php connect.php
```

### PowerShell {#powershell}

{% list tabs group=connection %}


- Connecting without SSL {#without-ssl}

   ```powershell
   curl.exe `
       -H "X-ClickHouse-User: <DB_user_name>" `
       -H "X-ClickHouse-Key: <DB_user_password>" `
       'http://<FQDN_of_any_{{ CH }}_host>:8123/?database=<DB_name>&query=SELECT+version()'
   ```


- Connecting via SSL {#with-ssl}

   ```powershell
   curl.exe `
       -H "X-ClickHouse-User: <DB_user_name>" `
       -H "X-ClickHouse-Key: <DB_user_password>" `
       'https://<FQDN_of_any_{{ CH }}_host>:8443/?database=<DB_name>&query=SELECT+version()'
   ```

{% endlist %}

To learn how to get a host FQDN, see [this guide](../../managed-clickhouse/operations/connect.md#fqdn).

### Python (clickhouse-driver) {#python-clickhouse-driver}

**Before connecting, install the dependencies:**

```bash
sudo apt update && sudo apt install --yes python3 python3-pip && \
pip3 install clickhouse-driver
```

{% list tabs group=connection %}


- Connecting without SSL {#without-ssl}

   `connect.py`

   ```python
   from clickhouse_driver import Client

   client = Client(host='<FQDN_of_any_{{ CH }}_host>',
                   user='<DB_user_name>',
                   password='<DB_user_password>',
                   port=9000)

   print(client.execute('SELECT version()'))
   ```


- Connecting via SSL {#with-ssl}

   `connect.py`

   ```python
   from clickhouse_driver import Client

   client = Client(host='<FQDN_of_any_{{ CH }}_host>',
                   user='<DB_user_name>',
                   password='<DB_user_password>',
                   port=9440,
                   secure=True,
                   verify=True,
                   ca_certs='{{ crt-local-dir }}{{ crt-local-file-root }}')

   print(client.execute('SELECT version()'))
   ```

{% endlist %}

To learn how to get a host FQDN, see [this guide](../../managed-clickhouse/operations/connect.md#fqdn).

**Connection:**

```bash
python3 connect.py
```

### Python (requests) {#python-requests}

**Before connecting, install the dependencies:**

```bash
sudo apt update && sudo apt install --yes python3 python3-pip && \
pip3 install requests
```

{% list tabs group=connection %}


- Connecting without SSL {#without-ssl}

   `connect.py`

   ```python
   import requests

   response = requests.get(
       'http://{0}:8123'.format('<FQDN_of_any_{{ CH }}_host>'),
       params={
           'query': 'SELECT version()',
       },
       headers={
           'X-ClickHouse-User': '<DB_user_name>',
           'X-ClickHouse-Key': '<DB_user_password>',
       })

   response.raise_for_status()
   print(response.text)
   ```


- Connecting via SSL {#with-ssl}

   `connect.py`

   ```python
   import requests

   response = requests.get(
       'https://{0}:8443'.format('<FQDN_of_any_{{ CH }}_host>'),
       params={
           'query': 'SELECT version()',
       },
       verify='{{ crt-local-dir }}{{ crt-local-file-root }}',
       headers={
           'X-ClickHouse-User': '<DB_user_name>',
           'X-ClickHouse-Key': '<DB_user_password>',
       })

   response.raise_for_status()
   print(response.text)
   ```

{% endlist %}

To learn how to get a host FQDN, see [this guide](../../managed-clickhouse/operations/connect.md#fqdn).

**Connection:**

```bash
python3 connect.py
```

### Ruby {#ruby}

**Before connecting, install the dependencies:**

```bash
sudo apt update && sudo apt install --yes ruby
```

{% list tabs group=connection %}


- Connecting without SSL {#without-ssl}

   `connect.rb`

   ```ruby
   require "net/http"
   require "uri"

   DB_HOST = "<FQDN_of_any_{{ CH }}_host>"
   DB_NAME = "<DB_name>"
   DB_USER = "<DB_user_name>"
   DB_PASS = "<DB_user_password>"

   QUERYSTRING = { :database => DB_NAME, :query => "SELECT version()" }

   uri = URI("http://" + DB_HOST + "/")
   uri.port = 8123
   uri.query = URI.encode_www_form(QUERYSTRING)

   req = Net::HTTP::Get.new(uri)
   req.add_field("X-ClickHouse-User", DB_USER)
   req.add_field("X-ClickHouse-Key", DB_PASS)

   conn = Net::HTTP.new(uri.host, uri.port)

   rs = conn.request(req)
   puts rs.body
   ```


- Connecting via SSL {#with-ssl}

   `connect.rb`

   ```ruby
   require "net/http"
   require "openssl"
   require "uri"

   DB_HOST = "<FQDN_of_any_{{ CH }}_host>"
   DB_NAME = "<DB_name>"
   DB_USER = "<DB_user_name>"
   DB_PASS = "<DB_user_password>"

   QUERYSTRING = { :database => DB_NAME, :query => "SELECT version()" }

   uri = URI("https://" + DB_HOST + "/")
   uri.port = 8443
   uri.query = URI.encode_www_form(QUERYSTRING)

   req = Net::HTTP::Get.new(uri)
   req.add_field("X-ClickHouse-User", DB_USER)
   req.add_field("X-ClickHouse-Key", DB_PASS)

   conn = Net::HTTP.new(uri.host, uri.port)
   conn.ca_file = "{{ crt-local-dir }}{{ crt-local-file-root }}"
   conn.use_ssl = true
   conn.verify_mode = OpenSSL::SSL::VERIFY_PEER

   rs = conn.request(req)
   puts rs.body
   ```

{% endlist %}

To learn how to get a host FQDN, see [this guide](../../managed-clickhouse/operations/connect.md#fqdn).

**Connection:**

```bash
ruby connect.rb
```
