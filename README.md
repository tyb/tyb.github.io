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

# References/Further reading/readings/materials