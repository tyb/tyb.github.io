## Tumblr API'sini consume ederek tüm verilerimin alınması ve saklanması için tasklar ve notlar:

# Gün1:
Linux üzerinde daha hızlı olabilmek için önce Eclipse ve STS(Spring Tools Suite) üzerinden ilerlemek istedim.
Ancak vazgeçtim IDEA Community Edition ile devam ettim. Burada bazı Linux command larına ihtiyacım oldu.

1. $PATH ve $JAVA_HOME'u ayarlamak 
    - Benzer şekilde $LD_LIBRARY_PATH
2. Bunun için java(JRE'de) ve javac(JDK'da) executable'larının yerlerini bulmak ve kurmak
    - Bunun için [Where can I find the Java SDK in Linux?](https://stackoverflow.com/questions/5251323/where-can-i-find-the-java-sdk-in-linux)
        > how to locate/find executable/sdk
        >> you can type `readlink -f $(which java)` to find the location of the java command
        >> On Ubuntu, it looks like it is in `/usr/lib/jvm/java-6-openjdk/` for OpenJDK,
        and in some other subdirectory of `/usr/lib/jvm/` for Suns JDK (and other implementations as well, I think).

        sorun şu ki her ne kadar *symbolic link* üzerinden jre'de executable'ı(jdk'daki için javac yazabilirdim) bulsam da SDK bu değil.
        Nitekim, IDEA'da `The selected directory is not a valid home for JDK` hatası alıyorum.

        - `$ ls -lh /usr/lib/jvm/`,
        - `whereis javac`,
        - `find`,
        - `locate`,
        - `which`

        linux command'larının pratik kullanımlarına bakılacak.

        **TODO:** bunları Gist olarak ekle.

    - **TODO:** sonuç olarak hala SDK'yı ekleyemedim.

    > <div> You will need to add JAVA_HOME to your .bashrc file.
    > Edit the: 

    > `gedit ~/.bashrc`

    > Add the following lines:

    ```
    ## JAVA_HOME

    export JAVA_HOME="/usr/lib/jvm/java-9-openjdk-amd64"

    export PATH=$PATH:$JAVA_HOME/bin
    ```

    yani

    ```
    taha@taha-Inspiron-3558:~$ ls -l /usr/lib/jvm/
    total 4
    lrwxrwxrwx 1 root root   25 Eyl 20  2018 default-java -> java-1.11.0-openjdk-amd64
    lrwxrwxrwx 1 root root   21 Eki 10  2018 java-1.11.0-openjdk-amd64 -> java-11-openjdk-amd64
    drwxr-xr-x 7 root root 4096 Haz 23 22:45 java-11-openjdk-amd64
    taha@taha-Inspiron-3558:~$ vim ~/.bashrc
    taha@taha-Inspiron-3558:~$ vim /etc/environment
    taha@taha-Inspiron-3558:~$ sudo su
    root@taha-Inspiron-3558:/home/taha# vim /etc/environment
    root@taha-Inspiron-3558:/home/taha# cat .bashrc
    root@taha-Inspiron-3558:/home/taha# cat /etc/environment
    PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games"
    JAVA_HOME="/usr/lib/jvm/java-11-openjdk-amd64"
    ```

    **Bu arada**,
    - **"how to set environment variables persistently in linux"** diye bak. Yani bu işin daha usable olanı vardı.
    - ayrıca -500f gibi `cat` de sadece sondan belli satırları yazdırma kısımlarını sıkça kullan.
    - vim'de geri alma yani ctrl-z fonksiyonu ve yapıştırma insert moddayken nasıl oluyordu.
    - aşağıda yaptığı gibi `sudo tee` yi aktif olarak kullan.
    - .bashrc ve diğer konfig ler çoğu kişinin github'ında .dotfiles olarak duruyor, onlardan kopyala.

    Ayrıca terminal'i kapatık açtık, duruyor(zaten durması gerekiyordu, çünkü doğrudan /etc/de değişiklik yapmıştık.(??)
    ```
    taha@taha-Inspiron-3558:~$ echo $PATH
    /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/usr/lib/jvm/java-11-openjdk-amd64/bin
    taha@taha-Inspiron-3558:~$ echo $JAVA_HOME
    /usr/lib/jvm/java-11-openjdk-amd64
    ```

    > Add it to the `/etc/environemnt` file with:

    > `echo "JAVA_HOME=\"/usr/lib/jvm/java-9-openjdk-amd64\"" | sudo tee -a /etc/environment`

    > Close and open a new terminal. If all doesn't work then:
    Launch Intellij Press: ctrl+alt+shift+S The go to Platform Settings ->
    SDKs click to add the path for your java sdk enter image description here Now your IntelliJ should be able to see it.
    </div>


    **TODO:** Böyle uzun kopyala yapıştır code içerikli yazılar nasıl en kolayca blockquote içine alınabiliyor?
    `> <div> paragraphs...</div>` şeklinde oluyor gibiydi ama tekrar bozuldu; markdown içinde ne kadar html kullanabilsek de
    `\n` ler ile ilgili durumlar bunlar.
    
    Yine SDK'yı göremiyor IDEA. Şöyle devam ettim:
    ```
    taha@taha-Inspiron-3558:~$ update-alternatives --config java
    There is only one alternative in link group java (providing /usr/bin/java): /usr/lib/jvm/java-11-openjdk-amd64/bin/java
    Nothing to configure.
    
    taha@taha-Inspiron-3558:~$ java --version
    openjdk 11.0.3 2019-04-16
    OpenJDK Runtime Environment (build 11.0.3+7-Ubuntu-1ubuntu218.10.1)
    OpenJDK 64-Bit Server VM (build 11.0.3+7-Ubuntu-1ubuntu218.10.1, mixed mode, sharing)
    taha@taha-Inspiron-3558:~$ sudo apt-get install openjdk-11-jdk
    ```
    
    Sorun ubuntu'da default olarak sadece JRE'nin olması ve JDK'nın olmamasıydı. Yanıltan kısım ise JRE'nin de JDK folder'ında olmasıydı.
    Yine de kurulumdan önce `javac`'ı bulamıyorduk. Yukarıdaki kurulumdan sonra, ki aynı folder'a kurulumu eklediğim için
     yukarıdaki environment variable'ları değiştirmeme gerek kalmadı. 


3. Git kurulumu ve remote/local workflow'u
    - /usr/bin/git de bulunuyor, IDE otomatik olarak buradan görüy  or.
    - Remote'dan clone edip local'ime indirdiğim bir proje için ilgili folder'da .git folder'ı oluştu.

    **içeriği:**

    ```
    [core]
            repositoryformatversion = 0
            filemode = true
            bare = false
            logallrefupdates = true
    [remote "origin"]
            url = https://github.com/tyb/tyb.github.io.git
            fetch = +refs/heads/*:refs/remotes/origin/*
    [branch "master"]
            remote = origin
            merge = refs/heads/master
    ```

# Gün2:

Şu anda Git kurulu, IDEA kurulu, JDK kurulu.
IDEA'da bir projede sadece tyb.github.io'yu editliyorum.
Diğer IDEA projesinde gerçek projeyi yapacağım. 

## PBIs(epic ya da feature şeklinde daha çok)

1. Boilerplate spring backend code 
    1. API consume 
        - Official Tumblr api'sini kullanabilmek için önce hesabımdan bir API Key alıyorum. 
        Public/Private ya da Secret Token için.
        - Doğrudan kendim API'yi çağırabilirdim ama `Jumblr` java client api consumer wrapper library'sini kullanacağım.
    2. API'den alınanlar 20'şerlik listeler olarak dönüyor. Dönen JSON datası doğrudan client'a gönderilecek. 
        - Doğrudan veritabanına kaydetmiyorum, çünkü gelen sonuçları takip etmek istiyorum. 
            - Çok fazla veri alacağımdan olası hata durumlarında kaldığım yeri takip edebilmek ve uzun süreceğinden
            - önyüzde bir progress bar ve her gelen sonucu geçiçi olarak ekranda gösterme durumu olacak. 
        - Bunun için `websocket` ya da `stomp` kullanabilirim ya da `firebase`. Bu pub/sub yapısı. 
        Teknik olarak bu konuyu enlemesine ve derinlemesine işleyen bir yazı yazacağım. Ama şimdilik şunu ayırt etmekte fayda var:
        MVP yani `minimum viable product` için seçimimi `websocket`. 
        > pub/sub; queue, topic, stream olarak ayrıştırılabilir. 
        > Burada teknik olarak topic mantığı olacak. Yani unordered messages and multiple consumer for one message.
        > Bunun yerine 20'lik olarak almak için Comet, AJAX çağrıları ya da Long Polling vs. yapabilirdim. 
    3. **Daha sonra: 1. ve 2. adımlar yapıldıktan sonra 1.3 yapılacak**
        DBMS(MySQL ya da Postgres) ya da Cassandra ya da MongoDB 
        - kurulumu, 
        - konfigürasyonu: directory'ler ve roller, şifreler, vs. 
        - path'ler hakkında genel bilgiler. data directory neresi, diğer directory'ler.
        JDBC Driver 
        - Konfigürasyon
        - Hibernate konfigürasyon
        - JPA Konfigürasyon
        - Migrations 
        - Seeds
        - Lombok
2. Boilerplate frontend code 
    - Minimum react components
    - Progress bar component
    - Widget, card, material UI Panel components
    - Bootstrap templates 
    - CSS Layout: positioning
    - Backend'e erişim: Redux'a ihtiyaç duyulan noktalar
    - Node.js kurulum ve component'lerin compile edilmesi
    - Packaging: Webpack, gulp, grunt ??
    - Dependency check/bundler: yarn, babel vs. 
    - Third party library kullanımı 
    - Ekranda Resume, ertele vs. gibi tuşlar da olmalı. 
        Buradan server'a http isteği ya da websocket üzerinden istek gönderilebilmeli. 

## STS(Spring tool suite) gibi IDEA üzerinden en kısa yoldan spring projesi oluşturmak.

Community edition'da builtin olarak spring initializer yok. 
O yüzden New > Project deyip workspace'imiz altında bir maven projesi oluşturuyoruz:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>io.github.tyb</groupId>
    <artifactId>tumblrConsumer</artifactId>
    <version>1.0-SNAPSHOT</version>

</project>
```

**maven** builtin olarak idea ile birlikte geliyor. 

**classpath** imiz:
```jshelllanguage
taha@taha-Inspiron-3558:~$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:
/usr/lib/jvm/java-11-openjdk-amd64/bin
taha@taha-Inspiron-3558:~$ echo $JAVA_HOME
/usr/lib/jvm/java-11-openjdk-amd64
```
IDEA classpath olarak JDK'yı görüyor. Ayrıca third party library'lerim de `/home/taha/.m2/repository` altında. 

### spring boot starter parent ve jar'lar ayarlanarak spring boot initializer vs. kullanmadan kendimiz oluşturalım:

[https://start.spring.io/](https://start.spring.io/)'dan gidip aşağıdakileri seçip

1. web starter
    - rest, mvc, tomcat 
2. security
    - TODO
3. data jpa
4. websocket 
    - SockJS
    - STOMP

İndirmeden, explore project dersek;

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.1.6.RELEASE</version>
    <relativePath/> <!-- lookup parent from repository -->
  </parent>
  <groupId>com.example</groupId>
  <artifactId>demo</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <name>demo</name>
  <description>Demo project for Spring Boot</description>
  <properties>
    <java.version>1.8</java.version>
  </properties>
  <dependencies>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-security</artifactId>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-websocket</artifactId>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-test</artifactId>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework.security</groupId>
      <artifactId>spring-security-test</artifactId>
      <scope>test</scope>
    </dependency>
  </dependencies>
  <build>
    <plugins>
      <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
      </plugin>
    </plugins>
  </build>
</project>

```

maven bu dependency'leri indiremiyor çünkü 
1. `/home/taha/.m2/settings.xml` de reposityory ve proxy ayarları olmayabilir.

Ubuntu Gnome'da .m2 gibi hidden resource'ları göstermek için `CTRL + H` yapılır. 

`.m2 repository` altına bakınca görüleceği üzere, import changes ya da reimport changes yaptığımız halde artifact'ler alınamamış. 

Görüleceği üzere `settings.xml` ve `profiles.xml` dosyaları yok.
IDE'den proje sağ tık - maven - create settings.xml ve diğeri denilerek oluşturulabilir. 

####repository adresleri ve proxy ayarları

settings.xml global, pom.xml ise local'dir bu açıdan.
`maven settings.xml for spring boot` olarak arattım.

MVP olarak içerik:
```xml
<profiles>
        <profile>
            <id>securecentral</id>
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
            <!--Override the repository (and pluginRepository) "central" from the
               Maven Super POM -->
            <repositories>
                <repository>
                    <id>central</id>
                    <url>https://repo1.maven.org/maven2</url>
                    <releases>
                        <enabled>true</enabled>
                    </releases>
                </repository>
            </repositories>
            <pluginRepositories>
                <pluginRepository>
                    <id>central</id>
                    <url>https://repo1.maven.org/maven2</url>
                    <releases>
                        <enabled>true</enabled>
                    </releases>
                </pluginRepository>
            </pluginRepositories>
        </profile>
</profiles>
<pluginGroups></pluginGroups>
<proxies></proxies>
<servers></servers>
<mirrors></mirrors>

```

ayrıca istenirse şu şekilde daha custom repo eklenebilir:
```xml
    <repositories>
        <repository>
            <id>spring-snapshots</id>
            <name>Spring Snapshots</name>
            <url>https://repo.spring.io/snapshot</url>
            <snapshots>
                <enabled>true</enabled>
            </snapshots>
        </repository>
        <repository>
            <id>spring-milestones</id>
            <name>Spring Milestones</name>
            <url>https://repo.spring.io/milestone</url>
            <snapshots>
                <enabled>false</enabled>
            </snapshots>
        </repository>
    </repositories>

    <pluginRepositories>
        <pluginRepository>
            <id>spring-snapshots</id>
            <name>Spring Snapshots</name>
            <url>https://repo.spring.io/snapshot</url>
            <snapshots>
                <enabled>true</enabled>
            </snapshots>
        </pluginRepository>
        <pluginRepository>
            <id>spring-milestones</id>
            <name>Spring Milestones</name>
            <url>https://repo.spring.io/milestone</url>
            <snapshots>
                <enabled>false</enabled>
            </snapshots>
        </pluginRepository>
    </pluginRepositories>
```



# References/Further reading/readings/materials