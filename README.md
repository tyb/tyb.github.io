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

### Güne retro bir bakış
Ubuntu gerçekten berbat! Yani linux'a sözüm yok, ama yıllar geçse de şu Gnome, KDE bilmem ne bir türlü adam olamadı. 
Mühendislik adına gerçekten büyük bir ayıp bu. 

Bugün öncelikle sadece bir hibernate'e alayım dedim, ama hibernate için `ALT + power` tuşuna basmak gerekiyordu, bunu farkeder etmez,
Kapatmak istiyor musun uyarı mesajına cancel dediğim halde bilgisayar shutdown oldu. 

Yeniden açtığımda bu defa IDEA saçmalamaya başladı. IDEA'nın da Eclipse'in de Allah nasıl biliyorsa öyle yapsın! 
Sanırım sorun, IDEA force bir şekilde kapatıldığından `indexing` dediği şeyi yapamadı. Tekrar açtığımda da `Loading project...` kısmında takılı kaldı.

`File > Invalidate caches & restart` diyerek çözme olaylarına girdik yine. Neyse bu şekilde yaptım bu defa yine takılmalar oldu. 

`/snap/intellij-idea-community/152/bin` altında `idea.vmoptions` ve `idea64.vmoptions` dan IDEA'yı daha fazla RAM kullanması için optimize etmek istedim.
Readonly yetkisinden dolayı editleyemedim. `chown`, `chmod 777 -R ...` denedim filan ama olmadı. 

`Help > Edit Custom VM Options` dan editleyebildim:

```
# custom IntelliJ IDEA VM options

-Xms1024m
-Xmx2048m
-XX:ReservedCodeCacheSize=240m
-XX:+UseConcMarkSweepGC
-XX:SoftRefLRUPolicyMSPerMB=50
-ea
-Dsun.io.useCanonCaches=false
-Djava.net.preferIPv4Stack=true
-Djdk.http.auth.tunneling.disabledSchemes=""
-XX:+HeapDumpOnOutOfMemoryError
-XX:-OmitStackTraceInFastThrow
-Dawt.useSystemAAFontSettings=lcd
-Dsun.java2d.renderer=sun.java2d.marlin.MarlinRenderingEngine
-Dsun.tools.attach.tmp.only=true
```

Sonra uygulamayı yaparken, senkron olarak ayrı projede not aldığım README.md dosyasını bir türlü açamadı IDEA.
`Markdown Navigator`'u kurmuştum plugin olarak daha önce ve IDEA kendisinin `Markdown Support` plugin'i ile çakıştığını birinden birini seçmem gerektiğini söyledi.
Default'u seçip geçmiştim ve güzel çalışıyordu ama o yanlışlıkla Shutdown'dan sonra bozuldu. Bu defa Settings'den diğerini seçtim ve sonunda uygulama geliştirmeye devam edebildim.

Son sözüm DELL bilgisayarıma. `Affordable` olduğu için aldığım bu MVP developer bilgisayarım hem ses çıkartıyor hem de biraz ısınıyor. 
Ayrıca hem gömülü hem harici ekran kartı var ama yine Ubuntu hazretleri bir türlü NVIDIA driver'ını yükleyemediğinden gömülüyü kullanmak zorunda kalıyorum.

Bir ara NVIDIA driver'ı kurmak için türlü yollar denerken sistem göçtü format atıldı ve Ruby ile %70'ini yaptığım proje de silindi.
Capslock tam algılamıyor gibi bu da deli ediyor beni. 

Ayrıcaaaa, ben bu bilgisayarı elimle bakmadan internetten okuyarak almıştım ama beyefendiler klavyede yön tuşlarını minnacık yapmışlar. Bu da durduk yere beni deli ediyor. 

Neyse, GOD DAMN UBUNTU!, GOD DAMN DELL! diyip geçiyorum.

## Boilerplate MVP REST api oluşturup main class ile birlikte build etmek:

Main:
```java
package io.github.tyb;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication

public class TumblrConsumerRestApiApplication {
    public static void main(String[] args) {
        SpringApplication.run(TumblrConsumerRestApiApplication.class, args);
    }

}
```

application.properties için:
`server.port=8080`

ve pom.xml'de

```xml
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>

        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <filtering>true</filtering>
            </resource>
        </resources>
    </build>
```


Controller:
```java
package io.github.tyb.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.ResponseBody;

import java.util.List;

@Controller
public class TumblerConsumerController {

    @RequestMapping(method = RequestMethod.GET, value="/tumblr/posts")
    @ResponseBody
    public List<?> getPosts() {
        return null;
    }

}
```

#### Run configurations:
`Run > Edit Configurations > Templates > Application` olarak
`name`, Configurations tab'ında Main class ve VM Options, working directory, classpath ve jre 
ve de öncesinde build alınsın vs. isteniyorsa onlar belirtilir.

#### Localde yeni oluşturulmuş bu projenin git ile ilişkilendirilmesi
1. Local'de: proje path'indeyken `git init` dememiz gerekecekti ama IDEA VCS altından yapabiliriz.
oluşan config dosyasına proje folder'ında .git ismiyle oluşturulmuş repo'daki config dosyasından bakarsak:
```
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
```
2. Local'de git ile ilişkilendirdikten sonra commit diyerek yine IDEA VCS üzerinden yaptık. 
3. Remote eklemek için IDEA CVS de Git > Remotes kısmında github adresimi ekleyeceğim, sonrasında config dosyasına IDE'nin GUI'sinden remote bilgisini oluşturmuş olacağız.
    - Ama öncesinde github'da repo'nun ismini yazıp oluşturmak gerekiyor. 
    - `origin` etiketiyle `https://github.com/tyb/tumblrConsumer.git` i ekledim. 
    Config dosyamız aşağıdaki gibi oldu:
    ```
    [core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
    [remote "origin"]
        url = https://github.com/tyb/tumblrConsumer.git
        fetch = +refs/heads/*:refs/remotes/origin/*
    ```   
    Aslında yaptığımız `git remote add origin https://github.com/tyb/tumblrConsumer.git` idi. 
4. Push dersek remote'a gönderir.
`git push -u origin master`

Şu ana kadar yapılan işlemler aslında:
```
echo "# tumblrConsumer" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/tyb/tumblrConsumer.git
git push -u origin master
```

#### Git global config dosyasının ayarlanması

**TODO: **

### Optimizing Ubuntu

#### Install preload to speed up application load time:
> Preload is a daemon that runs in the background and analyzes user behavior and frequently run applications. 

`sudo apt-get install preload`


#### Reduce overheating:
> Overheating is a common problem in computers these days. An overheated computer runs quite slow. It takes ages to open a program when your CPU fan is running like Usain Bolt. 
> There are two tools which you can use to reduce overheating and thus get a better system performance in Ubuntu, TLP and CPUFREQ.
> To install and use TLP, use the following commands in a terminal:

```
sudo add-apt-repository ppa:linrunner/tlp
sudo apt-get update
sudo apt-get install tlp tlp-rdw
sudo tlp start
```

> You don’t need to do anything after installing TLP. It works in the background.
> To install CPUFREQ indicator use the following command:
`sudo apt-get install indicator-cpufreq`
> Restart your computer and use the Powersave mode in it:

Burada `cpufrequtils` diye bir başka tool var, buna da bak ve indicator-cpufreq kurulduktan sonra adam GUI'den powersave modu seçmiş. 
O GUI nerede?

#### Increasing swap space
[Change swap size in Ubuntu](https://bogdancornianu.com/change-swap-size-in-ubuntu/)
```
taha@taha-Inspiron-3558:~$ sudo swapoff -a
[sudo] password for taha: 
taha@taha-Inspiron-3558:~$ sudo dd if=/dev/zero of=/swapfile bs=1M count=8000
8000+0 records in
8000+0 records out
8388608000 bytes (8,4 GB, 7,8 GiB) copied, 154,837 s, 54,2 MB/s
taha@taha-Inspirn-3558:~$ sudo mkswap /swapfile
mkswap: /swapfile: insecure permissions 0644, 0600 suggested.
Setting up swapspace version 1, size = 7,8 GiB (8388603904 bytes)
no label, UUID=159f8da8-1032-447a-8145-bb15bcbb7de7
taha@taha-Inspiron-3558:~$ sudo swapon /swapfile
swapon: /swapfile: insecure permissions 0644, 0600 suggested.
taha@taha-Inspiron-3558:~$ grep SwapTotal /proc/meminfo
SwapTotal:       8191996 kB

```

#### Lightweight GUI managers instead of GNOME
[installing xfce](https://itsfoss.com/install-xfce-desktop-xubuntu/)

[ya da](https://www.techradar.com/how-to/how-to-speed-up-ubuntu-1804) 

`sudo apt-get install lubuntu-desktop`

Bunu kurarken `gdm3` ya da `sddm` GUI manager'larından birini seçtiriyor.

#### monitoring startup services
`service --status-all `
`service tla status`

#### To empty the temporary cache used by ‘apt-get’, run the command:

`sudo apt-get clean`

## Wrap-ups: tab closing session for previous days

**şunlara bakmıştım:**
1. > where is jdk and jre on linux
2. > how to locate and find java installation in linux ya da how to find jdk location in ubuntu
3. > how to add jdk path in idea in linux
[How to setup SDK in IntelliJ IDEA?](https://stackoverflow.com/questions/43661829/how-to-setup-sdk-in-intellij-idea)
>  To find the path where java is installed on ubuntu, you can run the following command from terminal:
> `$ whereis java`
> You may get something like this:
> `java: /usr/bin/java /etc/java /usr/share/java /usr/lib/jvm`
> Which means that the java resides at one of the above paths as for example /usr/bin/java
> So, that directory should designate in IntelliJ. You can configure in the Project Structure, press Ctrl + Alt + Shift + S, choose Platform Settings -> SDKs, click on green button (+), select the home directory for JDK.
4. > ubuntu jdk 11 installed but i dont have javac
>  It seems you have installed JRE (Java Runtime Environment) only. javac comes under JDK (Java Development Kit) package.
> To install JDK, open terminal and type following command:
> `sudo apt-get install openjdk-7-jdk`
>> Beni yanıldan JRE'nin JDK folder'ı altında yüklü olmasıydı, ama yine de `javac`'ın olmamasından durumu hemen anlamam gerekirdi. 
5. How to Hibernate Ubuntu Gnome 16.04 from the GUI? 
> `sudo systemctl hibernate`
6. idea community edition spring boot project
> IDEA'nın free'sinde Spring boot plugin olarak gelmiyor ve yükleyemiyorsun. Çok sorun değil, sadece spring boot projesi olarak fancy gösterimler yok.
7. maven settings.xml for spring boot
> global repository ve proxy ayarları içindi.
8. why is ubuntu so slow on my laptop
> sorun büyük oranda GNOME'dan kaynaklıydı ve ufak tefek diğer ayarları yaptım: preload, tla, indicator-cpufreq, swap space ve global olarak IDEA vmoptions -Xmx ayarları.
9. how to open system monitor on ubuntu?
10. Why is IntelliJ IDEA hanging on “Indexing”?
11. spring boot rest api tutorial
12. idea community spring boot run configuration
13. Maven in 5 Minutes
14. spring boot react setup settings configuration
> Bu şekilde yazdım ama react'ın spring boot ile çok ilgisi yok, ama yine de onu da IDEA altında tutacağım.
> Bu aslında Gün3'te yapacağım konu.

## Further readings:
- [What is the rationale for the `/usr` directory?](https://askubuntu.com/questions/130186/what-is-the-rationale-for-the-usr-directory)
> Short version:
> As your link already said, /usr is a place for system-wide, read-only files. So all your installed software goes there. It does not duplicate any names of / except /bin and /lib, but, originally, with a different purpose: /bin, /lib is only for binaries and libraries required for booting, while /usr/bin, /usr/lib is for all the other executables and libraries. (now be a good boy and don't ask about /sbin, this is the Short Version after all)
> Nowadays, the distinction between "required for booting" and not has diminished, since most modern distros, including Ubuntu, cannot properly boot without several files from /usr. And that's why there is a strong movement towards merging /usr/bin and /bin, so probably in the near future (Ubuntu 12.10 perhaps?) /bin will be a symlink to /usr/bin.
> But maybe you are confusing /usr and /usr/local? Because yes, there is (and should be) a lot of duplicated directory names. More on that later...


# Gün3:

## Öncelikle lombok, h2 ve jpa'yı da ayarlayalım.

### `domain` yani `model` için okuduğum tutorial'dan `ilişki` de içeren örnekler:

```java
package io.github.tyb.domain.tutorial;

import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import lombok.NoArgsConstructor;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;
import javax.persistence.ManyToMany;
import java.time.Instant;
import java.util.Set;

@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder
@Entity
public class Event {

    @Id
    @GeneratedValue
    private Long id;
    private Instant date;
    private String title;
    private String description;
    @ManyToMany
    private Set<User> attendees;
}
```

ve 

```java
package io.github.tyb.domain.tutorial;

import lombok.Data;
import lombok.NoArgsConstructor;
import lombok.NonNull;
import lombok.RequiredArgsConstructor;

import javax.persistence.*;
import java.util.Set;

@Data
@NoArgsConstructor
@RequiredArgsConstructor
@Entity
@Table(name = "user_group")
public class Group {

    @Id
    @GeneratedValue
    private Long id;
    @NonNull
    private String name;
    private String address;
    private String city;
    private String stateOrProvince;
    private String country;
    private String postalCode;
    @ManyToOne(cascade=CascadeType.PERSIST)
    private User user;

    @OneToMany(fetch = FetchType.EAGER, cascade=CascadeType.ALL)
    private Set<Event> events;
}
```

ve de 

```java
package io.github.tyb.domain.tutorial;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

import javax.persistence.Entity;
import javax.persistence.Id;

@Data
@NoArgsConstructor
@AllArgsConstructor
@Entity
public class User {

    @Id
    private String id;
    private String name;
    private String email;
}
```

### JPA sample:

ayrıca `Spring Data JPA` daki `repository` abstraction'larını kullanıyor:

```java
package io.github.tyb.repository;

import io.github.tyb.domain.tutorial.Group;
import org.springframework.data.jpa.repository.JpaRepository;

import java.util.List;

public interface GroupRepository extends JpaRepository<Group, Long> {
    Group findByName(String name);
}
```

## Controller sample'ı da bulunsun:

```java
package io.github.tyb.controller.tutorial;

import io.github.tyb.domain.tutorial.Group;
import io.github.tyb.repository.GroupRepository;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import javax.validation.Valid;
import java.net.URI;
import java.net.URISyntaxException;
import java.util.Collection;
import java.util.Optional;

@RestController
@RequestMapping("/api")
class GroupController {

    private final Logger log = LoggerFactory.getLogger(GroupController.class);
    private GroupRepository groupRepository;

    public GroupController(GroupRepository groupRepository) {
        this.groupRepository = groupRepository;
    }

    @GetMapping("/groups")
    Collection<Group> groups() {
        return groupRepository.findAll();
    }

    @GetMapping("/group/{id}")
    ResponseEntity<?> getGroup(@PathVariable Long id) {
        Optional<Group> group = groupRepository.findById(id);
        return group.map(response -> ResponseEntity.ok().body(response))
                .orElse(new ResponseEntity<>(HttpStatus.NOT_FOUND));
    }

    @PostMapping("/group")
    ResponseEntity<Group> createGroup(@Valid @RequestBody Group group) throws URISyntaxException {
        log.info("Request to create group: {}", group);
        Group result = groupRepository.save(group);
        return ResponseEntity.created(new URI("/api/group/" + result.getId()))
                .body(result);
    }

    @PutMapping("/group/{id}")
    ResponseEntity<Group> updateGroup(@Valid @RequestBody Group group) {
        log.info("Request to update group: {}", group);
        Group result = groupRepository.save(group);
        return ResponseEntity.ok().body(result);
    }

    @DeleteMapping("/group/{id}")
    public ResponseEntity<?> deleteGroup(@PathVariable Long id) {
        log.info("Request to delete group: {}", id);
        groupRepository.deleteById(id);
        return ResponseEntity.ok().build();
    }
}


```

## Seed ya da data initializer işini yapmak:

Bunun için `org.springframework.boot.CommandLineRunner`'ı implemente ediyoruz:

```java
package io.github.tyb;

import io.github.tyb.domain.tutorial.Event;
import io.github.tyb.domain.tutorial.Group;
import io.github.tyb.repository.GroupRepository;
import org.springframework.boot.CommandLineRunner;
import org.springframework.stereotype.Component;

import java.time.Instant;
import java.util.Collections;
import java.util.stream.Stream;

@Component
class Seeder implements CommandLineRunner {

    private final GroupRepository repository;

    public Initializer(GroupRepository repository) {
        this.repository = repository;
    }

    @Override
    public void run(String... strings) {
        Stream.of("Denver JUG", "Utah JUG", "Seattle JUG",
                "Richmond JUG").forEach(name ->
                repository.save(new Group(name))
        );

        Group djug = repository.findByName("Denver JUG");
        Event e = Event.builder().title("Full Stack Reactive")
                .description("Reactive with Spring Boot + React")
                .date(Instant.parse("2018-12-12T18:00:00.000Z"))
                .build();
        djug.setEvents(Collections.singleton(e));
        repository.save(djug);
        repository.findAll().forEach(System.out::println);
    }
}
```

Bu kısım sanırım uygulamayı run ettiğimizde çalışıyor ve data temporary olarak tabloya atılıyor.
> If you start your app (using ./mvnw spring-boot:run) after adding this code, you’ll see the list of groups and events displayed in your console.  

## React ile bir UI oluşturmak için settings/configuration/development environment

1. Create React App 
> Create React App is a command line utility that generates React projects for you. It’s a convenient tool because it also offers commands that will build and optimize your project for production. It uses webpack under the covers for building. If you want to learn more about webpack, I recommend webpack.academy.
    - Scaffolding için yani Code Structure/code organization/phsical code için boilerplate dosya ve klasör yapısını oluşturur
    `yarn create react-app app`
    - Package Manager/Dependency manager/Bundle Manager/Bundler olarak `yarn`'ı kullanır. 
    `yarn add bootstrap@4.1.3 react-cookie@3.0.4 react-router-dom@4.3.1 reactstrap@6.5.0`
    - Build tool olarak `webpack`'i kullanır
    >> webpack ile component'ler dependency'leri ile birlikte build edilip (sanırım) `node` modulleri oluşturuluyor.
    
### yarn

zaten linux distrosunda kurulu olarak geliyor.
**TODO:** teknik detaylarına bakılacak.

```
taha@taha-Inspiron-3558:~$ yarn --version
1.12.1
```

#### workflow:
```
yarn create react-app app
cd app
yarn add bootstrap@4.1.3 react-cookie@3.0.4 react-router-dom@4.3.1 reactstrap@6.5.0
```

#### scaffolding and setting react development environment with yarn

```
taha@taha-Inspiron-3558:~$ pwd
/home/taha
taha@taha-Inspiron-3558:~$ ls -ltr
total 68
-rw-r--r-- 1 taha taha 8980 Eki 21  2018 examples.desktop
drwxr-xr-x 2 taha taha 4096 Eki 21  2018 Videos
drwxr-xr-x 2 taha taha 4096 Eki 21  2018 Templates
drwxr-xr-x 2 taha taha 4096 Eki 21  2018 Public
drwxr-xr-x 2 taha taha 4096 Eki 21  2018 Pictures
drwxr-xr-x 2 taha taha 4096 Eki 21  2018 Music
drwxr-xr-x 3 taha taha 4096 Eki 24  2018 git
drwxr-xr-x 4 taha taha 4096 Eki 25  2018 eclipse-workspace
drwxr-xr-x 3 taha taha 4096 Eki 30  2018 Desktop
drwxr-xr-x 2 taha taha 4096 Eki 31  2018 Documents
drwxr-xr-x 2 taha taha 4096 Haz 23 22:23 Downloads
-rw-r--r-- 1 taha taha    0 Haz 29 21:12 troubleshoot-logs.txt
drwxr-xr-x 5 taha taha 4096 Tem 13 13:38 IdeaProjects
drwxr-xr-x 5 taha taha 4096 Tem 13 15:57 snap
-rw-rw-r-- 1 taha taha   86 Tem 14 17:34 yarn.lock
drwxrwxr-x 2 taha taha 4096 Tem 14 17:34 node_modules
taha@taha-Inspiron-3558:~$ cd IdeaProjects/
taha@taha-Inspiron-3558:~/IdeaProjects$ ls -ltr
total 12
drwxr-xr-x 3 taha taha 4096 Tem 13 15:56 tyb.github.io
drwxr-xr-x 6 taha taha 4096 Tem 14 03:37 tumblrConsumer
drwxr-xr-x 4 taha taha 4096 Tem 14 17:58 tyb_github
taha@taha-Inspiron-3558:~/IdeaProjects$ cd tumblrConsumer/
taha@taha-Inspiron-3558:~/IdeaProjects/tumblrConsumer$ yarn create react-app reactClient
yarn create v1.12.1
[1/4] Resolving packages...
[2/4] Fetching packages...
[3/4] Linking dependencies...
[4/4] Building fresh packages...
success Installed "create-react-app@3.0.1" with binaries:
      - create-react-app
[####################################################################] 92/92Could not create a project called "reactClient" because of npm naming restrictions:
  *  name can no longer contain capital letters
error Command failed.
Exit code: 1
Command: /home/taha/.yarn/bin/create-react-app
Arguments: reactClient
Directory: /home/taha/IdeaProjects/tumblrConsumer
Output:

info Visit https://yarnpkg.com/en/docs/cli/create for documentation about this command.
taha@taha-Inspiron-3558:~/IdeaProjects/tumblrConsumer$ yarn create react-app react_client
yarn create v1.12.1
[1/4] Resolving packages...
[2/4] Fetching packages...
[3/4] Linking dependencies...
[4/4] Building fresh packages...
success Installed "create-react-app@3.0.1" with binaries:
      - create-react-app
[####################################################################] 92/92
Creating a new React app in /home/taha/IdeaProjects/tumblrConsumer/react_client.                                                                          

Installing packages. This might take a couple of minutes.
Installing react, react-dom, and react-scripts...

yarn add v1.12.1
[1/4] Resolving packages...
warning react-scripts > fsevents@2.0.6: Please update: there are crash fixes
[2/4] Fetching packages...
info fsevents@1.2.9: The platform "linux" is incompatible with this module.
info "fsevents@1.2.9" is an optional dependency and failed compatibility check. Excluding it from installation.
info fsevents@2.0.6: The platform "linux" is incompatible with this module.
info "fsevents@2.0.6" is an optional dependency and failed compatibility check. Excluding it from installation.
info fsevents@2.0.6: The engine "node" is incompatible with this module. Expected version "^8.16.0 || ^10.6.0 || >=11.0.0". Got "8.11.4"
[3/4] Linking dependencies...
warning "react-scripts > @typescript-eslint/eslint-plugin@1.6.0" has unmet peer dependency "typescript@*".
warning "react-scripts > @typescript-eslint/parser@1.6.0" has unmet peer dependency "typescript@*".
warning "react-scripts > ts-pnp@1.1.2" has unmet peer dependency "typescript@*".
warning "react-scripts > @typescript-eslint/eslint-plugin > @typescript-eslint/typescript-estree@1.6.0" has unmet peer dependency "typescript@*".
warning "react-scripts > @typescript-eslint/eslint-plugin > tsutils@3.10.0" has unmet peer dependency "typescript@>=2.8.0 || >= 3.2.0-dev || >= 3.3.0-dev || >= 3.4.0-dev".
[4/4] Building fresh packages...
success Saved lockfile.
success Saved 11 new dependencies.
info Direct dependencies
├─ react-dom@16.8.6
├─ react-scripts@3.0.1
└─ react@16.8.6
info All dependencies
├─ babel-preset-react-app@9.0.0
├─ eslint-config-react-app@4.0.1
├─ fork-ts-checker-webpack-plugin@1.1.1
├─ microevent.ts@0.1.1
├─ react-app-polyfill@1.0.1
├─ react-dev-utils@9.0.1
├─ react-dom@16.8.6
├─ react-error-overlay@5.1.6
├─ react-scripts@3.0.1
├─ react@16.8.6
└─ worker-rpc@0.1.1
Done in 397.95s.

Success! Created react_client at /home/taha/IdeaProjects/tumblrConsumer/react_client
Inside that directory, you can run several commands:

  yarn start
    Starts the development server.

  yarn build
    Bundles the app into static files for production.

  yarn test
    Starts the test runner.

  yarn eject
    Removes this tool and copies build dependencies, configuration files
    and scripts into the app directory. If you do this, you can’t go back!

We suggest that you begin by typing:

  cd react_client
  yarn start

Happy hacking!
Done in 415.06s.
```

Buradan görüleceği üzere, `typescript`, `eslint`, `babel` vb. ile de ilgileneceğiz.

Ayrıca `yarn create react-app app` iki şey yapıyor:
1. `yarn create v1.12.1`: bu sanırım sadece dependency listesiyle beraber scaffolding yapan kısım.
Ayrıca `/home/taha/IdeaProjects/tumblrConsumer/react_client` altında dosyaları oluşturuyor. 
2. hemen ardından `yarn add v1.12.1` çalışıyor ki bu da sanırım dependency listesinden dependency'leri yani `module` leri yani `package` ları yani `library` leri alıp 
kendi içlerindeki package'lar ile birlikte indirip bunları resolve ediyor ve link ediyor ardından da package'ları oluşturuyor.

##### Peki bu dependency'ler/module'ler/library'ler nereye ve nasıl indi?

Ve `react_client` folder'ına bakınca zaten olay aydınlanıyor. Bu bildiğin `node.js` projesi.
`node_modules` folder'ı ile `package_json` direkt görünüyor. 

package_json'daki temel dependency'ler şöyle:

```json
{
  "name": "react_client",
  "version": "0.1.0",
  "private": true,
  "dependencies": {
    "react": "^16.8.6",
    "react-dom": "^16.8.6",
    "react-scripts": "3.0.1"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
  "eslintConfig": {
    "extends": "react-app"
  },
  "browserslist": {
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  }
}
```

**Yarn da aslında npm ya da npx gibi**. Yani `npm` ya da `npx` de kullanabilirdik:

```
npx create-react-app my-app
cd my-app
npm start
```

ya da 

```
npm init react-app my-app
```

###### References

1. [facebook - getting started](https://facebook.github.io/create-react-app/docs/getting-started)
2. [How do I get globally installed node modules to run on terminal](https://stackoverflow.com/questions/50556589/how-do-i-get-globally-installed-node-modules-to-run-on-terminal)
>  If you are asking how to start the application, you can use
> `node server.js`
> if your entrypoint file is called server. Alternatively use npm start, documented here: https://docs.npmjs.com/cli/start
> To see what is installed,
> `npm -g list`
> will tell you what is installed globally.
3. [Where does npm install the packages?](https://flaviocopes.com/where-npm-install-packages/)


# References/Further reading/readings/materials