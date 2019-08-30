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
        - `find . -iname '*{part_of_word}*' -print`,
        - `locate`,
        - `which`
        - `grep -R hello /home`

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
    - /usr/bin/git de bulunuyor, IDE otomatik olarak buradan görüyor.
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

###retro bir bakış
Ubuntu gerçekten berbat! Yani linux'a sözüm yok, ama yıllar geçse de şu Gnome, KDE bilmem ne bir türlü adam olamadı. 
Mühendislik adına gerçekten büyük bir ayıp bu. 

öncelikle sadece bir hibernate'e alayım dedim, ama hibernate için `ALT + power` tuşuna basmak gerekiyordu, bunu farkeder etmez,
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

making swap size permanent:

```
sudo vim /etc/fstab
```

add

```
/swapfile none swap sw 0 0
```

default swappiness value 60 (1-100)

`sudo sysctl vm.swappiness=10`

permanent olması için

`/etc/sysctl.conf` dosyasına `vm.swappiness=10` satırı eklenir.

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
1. [What is the rationale for the `/usr` directory?](https://askubuntu.com/questions/130186/what-is-the-rationale-for-the-usr-directory)
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

## Database Seed ya da data initializer işini yapmak:

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


## Using Spring Boot CLI

### Scaffolding fastest way

`spring init --name=flyway-demo --dependencies=web,mysql,data-jpa,flyway flyway-demo`

## Configuring MySQL and Hibernate for Spring Boot

Spring boot uygulamasını başlattığımıda connect olurken ortaya çıkan yetki sorunlarını genel olarak aşmak için:

> Change the file my.cnf (in my Ubuntu-system he is placed at `/etc/mysql/my.cnf`). In the end i added that code:

```
root@taha-Inspiron-3558:/var/log/mysql# nano /etc/mysql/my.cnf
[mysqld]
skip-grant-tables
root@taha-Inspiron-3558:/var/log/mysql# /etc/init.d/mysql restart
[ ok ] Restarting mysql (via systemctl): mysql.service.
```

bundan sonra artık `sudo mysql -u root -p` demeden doğrudan `mysql -u root -p` diyerek girebildim. 

artık `ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password` hatasını geçmiş olduk.

hemen sonra root haricinde bir kullanıcı oluşturdum ve datasource'uma bunu verdim.

```
mysql> GRANT ALL PRIVILEGES ON *.* TO 'taha'@'localhost' IDENTIFIED BY '1'; Query OK, 0 rows affected, 1 warning (0.00 sec)

mysql> SELECT User, Host, authentication_string FROM mysql.user;
+------------------+-----------+-------------------------------------------+
| User             | Host      | authentication_string                     |
+------------------+-----------+-------------------------------------------+
| root             | localhost | *E6CC90B878B948C35E92B003C792C46C58C4AF40 |
| mysql.session    | localhost | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE |
| mysql.sys        | localhost | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE |
| debian-sys-maint | localhost | *ED020BAF618432391D17273887FE59B5DFF1E334 |
| taha             | localhost | *E6CC90B878B948C35E92B003C792C46C58C4AF40 |
+------------------+-----------+-------------------------------------------+
5 rows in set (0.00 sec)
mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.05 sec)

mysql> quit
Bye
taha@taha-Inspiron-3558:/var/log/mysql$ sudo /etc/init.d/mysql restart
```

hibernate database initialization ya da dll creation işini yapabilmek için veritabanının olması gerekiyor, çünkü connection string'inde yer alacak:

```
mysql> CREATE DATABASE tumblr;
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| tumblr             |
+--------------------+
5 rows in set (0.03 sec)

mysql> use tumblr;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> show tables;
```

aşağıda detaylarını açıklayacağım config dosyası ki şunları içeriyor:
1. datasource ve dolayısıyla hibernate entity manager 
2. ddl yani database schema autocreation from entities yani database initialization
3. generating ddl creation script without creating database schema.

application.properties
```
server.port=8080

spring.datasource.url=jdbc:mysql://localhost:3306/tumblr
spring.datasource.username=taha
spring.datasource.password=1
spring.datasource.testWhileIdle = true
spring.datasource.validationQuery = SELECT 1
spring.datasource.driver-class-name=com.mysql.jdbc.Driver

spring.jpa.open-in-view=false
spring.jpa.database-platform=org.hibernate.dialect.MySQL5InnoDBDialect
spring.jpa.generate-ddl=true
spring.jpa.show-sql=true

spring.jpa.hibernate.ddl-auto=create
spring.jpa.hibernate.naming_strategy=org.hibernate.cfg.ImprovedNamingStrategy
#spring.jpa.hibernate.naming.physical-strategy=org.hibernate.boot.model.naming.PhysicalNamingStrategyStandardImpl
#spring.jpa.hibernate.use-new-id-generator-mappings=false

# How to generate a ddl creation script with a modern Spring Boot + Data JPA and Hibernate setup?
#spring.jpa.properties.javax.persistence.schema-generation.create-source=metadata
#spring.jpa.properties.javax.persistence.schema-generation.scripts.action=create
#spring.jpa.properties.javax.persistence.schema-generation.scripts.create-target=create.sql
#spring.jpa.properties.hibernate.hbm2ddl.auto=create
#spring.jpa.properties.hibernate.format_sql=true
#spring.jpa.properties.hibernate.default_schema = schemaName

spring.flyway.enabled=false

# Logs sql statements
logging.level.org.hibernate.SQL=DEBUG
logging.level.org.hibernate.type=TRACE
```

## database migrations and migration files:

Migration'ları `liquibase` ya da `flyway` ile yapabiliriz. 
Aslında production-scale için hibernate'in Entity'lerde db schema autocreation'u yerine migration'ları kullanmak doğru olanı. 
Ancak RAD için hibernate database initialization/database schema autocreation ile başlayabiliriz, hatta ileride detaylandıracağım; 
`profile` set ederek geliştirme ortamı için birini prod için diğerini set edebiliriz.

bugün yaşağıdığım sorunlar ddl autocreation, migrations ve ddl generation'ları birlikte kullanmak ile de ilgiliydi:
1. sadece birini bir anda yapmak gerekir. ddl autocreation yapılacaksa:

```
spring.jpa.open-in-view=false
spring.jpa.database-platform=org.hibernate.dialect.MySQL5InnoDBDialect
spring.jpa.generate-ddl=true
spring.jpa.show-sql=true

spring.jpa.hibernate.ddl-auto=create
spring.jpa.hibernate.naming_strategy=org.hibernate.cfg.ImprovedNamingStrategy
#spring.jpa.hibernate.naming.physical-strategy=org.hibernate.boot.model.naming.PhysicalNamingStrategyStandardImpl
#spring.jpa.hibernate.use-new-id-generator-mappings=false
```

sadece bu olmalı. 

```
#How to generate a ddl creation script with a modern Spring Boot + Data JPA and Hibernate setup?
#spring.jpa.properties.javax.persistence.schema-generation.create-source=metadata
#spring.jpa.properties.javax.persistence.schema-generation.scripts.action=create
#spring.jpa.properties.javax.persistence.schema-generation.scripts.create-target=create.sql
```

bu **olmamalı**.

2. eğer ddl generation yapılacaksa sadece aşağıdaki kısım olmalı, eğer ikisi de olursa sadece generate eder, veritabanında oluşturmaz.

```
spring.jpa.hibernate.ddl-auto=create
spring.jpa.hibernate.naming_strategy=org.hibernate.cfg.ImprovedNamingStrategy

#How to generate a ddl creation script with a modern Spring Boot + Data JPA and Hibernate setup?
spring.jpa.properties.javax.persistence.schema-generation.create-source=metadata
spring.jpa.properties.javax.persistence.schema-generation.scripts.action=create
spring.jpa.properties.javax.persistence.schema-generation.scripts.create-target=create.sql
```

bu şekilde çalıştığında ddl generation script'ini project directory root'unda `create.sql` ismiyle oluşturur.

3. migrations kullanılacaksa `/src/main/resources/db/migration` altında `Vx.x__init.sql` formatlarında dosyalar oluşturulmalı. 
Burada dikkat edilmesi gereken, migrations ve ddl creation'un aynı anda olması isteniyorsa flyway önce/sonra çalışması için konfigüre edilmeli. 
flyway script'leri olup da çalışmasın isteniyorsa properties'de:
`spring.flyway.enabled=false` eklenmeli. 

4. loading initial data/seeding/sampling - eğer seed yapılacaksa yani DML ile fake data ya da structured data db'ye populate edilecekse `CommandLineRunner` kullanılabilir. 
Ancak bu da ddl creation'dan önce çalışıyor olabilir, dikkat etmek gerekir. 
`ApplicationRunner` da kullanılabilir. 
Uygulama run olurken bunlar çalışır. 

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


/*TODO:
 Bu hibernate DLL creation'dan önce çalıştığından table doesn't exist hatası alıyor.
 */
class seeder {

}
/*
@Component
class Seeder implements CommandLineRunner {

    private final GroupRepository repository;

    public Seeder(GroupRepository repository) {
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
*/

```

programmatically:
You can catch ApplicationReadyEvent then insert demo data, for example:

```java
@Component
public class DemoData {

    @Autowired
    private final EntityRepository repo;

    @EventListener
    public void appReady(ApplicationReadyEvent event) {

        repo.save(new Entity(...));
    }
}
```

Or you can implement CommandLineRunner or ApplicationRunner, to load demo data when an application is fully started:

```java
@Component
public class DemoData implements CommandLineRunner {

    @Autowired
    private final EntityRepository repo;

    @Override
    public void run(String...args) throws Exception {

        repo.save(new Entity(...));
    }
}
```

```java
@Component
public class DemoData implements ApplicationRunner {

    @Autowired
    private final EntityRepository repo;

    @Override
    public void run(ApplicationArguments args) throws Exception {

        repo.save(new Entity(...));
    }
}
```

Or even implement them like a Bean right in your Application (or other 'config') class:

```java
@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

    @Bean
    public CommandLineRunner demoData(EntityRepository repo) {
        return args -> { 

            repo.save(new Entity(...));
        }
    }
}
```

ya da 

```java
@SpringBootApplication  
public class Application {

@Autowired
private UserRepository userRepository;

public static void main(String[] args) {
    SpringApplication.run(Application.class, args);
}

@Bean
InitializingBean sendDatabase() {
    return () -> {
        userRepository.save(new User("John"));
        userRepository.save(new User("Rambo"));
      };
   }
}
```

5. Eğer `import.sql` ile migrations ya da diğerleri kullanılmadan schema'nın son halini ve DML ifadelerini tek bir bulk dosyada tutup bunu up-to-date tutacaksak
bunun yüklemesi için de ddl autocreation'u devre dışı bırakmak gerekir.

> Spring Boot can automatically create the schema (DDL scripts) of your DataSource and initialize it (DML scripts). It loads SQL from the standard root classpath locations: schema.sql and data.sql, respectively. In addition, Spring Boot processes the schema-${platform}.sql and data-${platform}.sql files (if present), where platform is the value of spring.datasource.platform. This allows you to switch to database-specific scripts if necessary. For example, you might choose to set it to the vendor name of the database (hsqldb, h2, oracle, mysql, postgresql, and so on).

Bunun için:
`spring.datasource.initialization-mode=always` -> embedded db'ler yerine hepsinde bu işlemi yapması için.

ve ddl autocreation olmamalı:

`spring.jpa.hibernate.ddl-auto=none`

ayrıca,

> If you really want to use the hibernate property prefix it with spring.jpa.properties. as those are added as is as properties to the EntityManagerFactory. See here in the Spring Boot reference guide.

`spring.jpa.properties.hibernate.hbm2ddl.import_files=file1.sql,file2.sql`

> However you can also use the spring.datasource.data and spring.datasource.schema properties to your advantage. They default to respectively data and schema. As you can see in the DataSourceInitializer class. You can also set them and they take a comma separated list of resources.

`spring.datasource.data=classpath:/data-domain.sql,file:/c:/sql/data-reference.sql,data-complex.sql`

from [Multiple SQL import files in Spring Boot](https://stackoverflow.com/questions/24508223/multiple-sql-import-files-in-spring-boot)

> It gets even better because the resource loading also allows loading resources with ant-style patterns.

```
spring.datasource.data=/META-INF/sql/init-*.sql
spring.datasource.schema=/META-INF/sql/schema-*.sql 
```

ve de 

`spring.datasource.data=classpath:accounts.sql, classpath:books.sql, classpath:reviews.sql`

### Creating a Flyway Migration script

Formatını yukarıda belirttim. 
Birinden birini kullanmak gerekir. 

from [Spring Boot: Hibernate and Flyway boot order](https://stackoverflow.com/questions/37097876/spring-boot-hibernate-and-flyway-boot-order)

> I had the same issue.

> I wanted my schema to be created by hibernate because of it's database independence. I already went through the trouble of figuring out a nice schema for my application in my jpa classes, I don't like repeating myself.

> But I want some data initialization to be done in a versioned manner which flyway is good at.

> Spring boot runs flyway migrations before hibernate. To change it I overrode the spring boot initializer to do nothing. Then I created a second initializer that runs after hibernate is done. All you need to do is add this configuration class:

```java
import org.flywaydb.core.Flyway;
import org.springframework.boot.autoconfigure.flyway.FlywayMigrationInitializer;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.DependsOn;

@Configuration
public class MigrationConfiguration {


    /**
     * Override default flyway initializer to do nothing
     */
    @Bean
    FlywayMigrationInitializer flywayInitializer(Flyway flyway) {
        return new FlywayMigrationInitializer(flyway, (f) ->{} );
    }


    /**
     * Create a second flyway initializer to run after jpa has created the schema
     */
    @Bean
    @DependsOn("entityManagerFactory")
    FlywayMigrationInitializer delayedFlywayInitializer(Flyway flyway) {
        return new FlywayMigrationInitializer(flyway, null);
    }
}
```

> That code needs java 8, If you have java 7 or earlier, replace (f)->{} with an inner class that implements FlywayMigrationStrategy

> Of course you can do this in xml just as easily.

> Make sure to add this to your application.properties:

`flyway.baselineOnMigrate = true`

### generating a ddl creation script with Spring Data JPA Hibernate - schema generation

[How to generate a ddl creation script with a modern Spring Boot + Data JPA and Hibernate setup?](https://stackoverflow.com/questions/36966337/how-to-generate-a-ddl-creation-script-with-a-modern-spring-boot-data-jpa-and-h)

[Database Initialization](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-database-initialization.html)
> An SQL database can be initialized in different ways depending on what your stack is. Of course, you can also do it manually, provided the database is a separate process. It is recommended to use a single mechanism for schema generation.
> JPA has features for DDL generation, and these can be set up to run on startup against the database. 

Yukarıda belirttim. 

### to insert simple test data by implementing a ApplicationRunner

Yukarıda `CommandLineRunner` versiyonu var. 

## installing and mysql server on ubuntu

### How can I check if mysql is installed on ubuntu?

1. `dpkg --get-selections | grep mysql`
2. `which mysqld`
> mysql" may be found even if mysql and mariadb is uninstalled, but not "mysqld". It is faster than `rpm -qa | grep mysqld`.

```
taha@taha-Inspiron-3558:~/IdeaProjects/tumblrConsumer$ which mysql
/usr/bin/mysql
taha@taha-Inspiron-3558:~/IdeaProjects/tumblrConsumer$ which mysqld
/usr/sbin/mysqld
```

3. `mysql --version`
4. `locate mysqld_safe`
5. `ls /etc/init.d | grep mysql`
6. `ldconfig -p | grep mysqlclient`


yani ubuntu ile beraber kurulu olarak gelmiş mysql server.

### installation

```
sudo apt-get update
sudo apt-get install mysql-server
```

> The installer installs MySQL and all dependencies.
> After installation is complete, the mysql_secure_installation utility runs. This utility prompts you to define the mysql root password and other security related options, including removing remote access to the root user and setting the root password.

#### allow remote access:

`sudo ufw allow mysql`
> If you have iptables enabled and want to connect to the MySQL database from another machine, you must open a port in your server’s firewall (the default port is 3306). You don’t need to do this if the application that uses MySQL is running on the same server.

### mysql server servisini yönetmek

servisin durumuna bakarak:

`sudo service mysql status`

```
mysql.service - MySQL Community Server
   Loaded: loaded (/lib/systemd/system/mysql.service; enabled; vendor preset: 
   Active: active (running) since Sun 2019-07-14 23:34:42 +03; 11min ago
  Process: 28645 ExecStart=/usr/sbin/mysqld --daemonize --pid-file=/run/mysqld
  Process: 28616 ExecStartPre=/usr/share/mysql/mysql-systemd-start pre (code=e
 Main PID: 28647 (mysqld)
    Tasks: 28 (limit: 4195)
   Memory: 167.9M
   CGroup: /system.slice/mysql.service
           └─28647 /usr/sbin/mysqld --daemonize --pid-file=/run/mysqld/mysqld.

Tem 14 23:34:35 taha-Inspiron-3558 systemd[1]: Starting MySQL Community Server
Tem 14 23:34:42 taha-Inspiron-3558 systemd[1]: Started MySQL Community Server.
```

ya da process olarak bakarak

```
taha@taha-Inspiron-3558:~/IdeaProjects/tumblrConsumer$ ps aux | grep mysql
mysql    28647  0.1  4.2 1390160 166488 ?      Sl   23:34   0:01 /usr/sbin/mysqld --daemonize --pid-file=/run/mysqld/mysqld.pid
```

ya da dinlediği port üzerinden

```
taha@taha-Inspiron-3558:~/IdeaProjects/tumblrConsumer$ sudo lsof -i:3306
COMMAND   PID  USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
mysqld  28647 mysql   27u  IPv4 223956      0t0  TCP localhost:mysql (LISTEN)

taha@taha-Inspiron-3558:~/IdeaProjects/tumblrConsumer$ sudo netstat -vulntp |grep -i mysql
tcp        0      0 127.0.0.1:3306          0.0.0.0:*               LISTEN      28647/mysqld 
```

ya da `mysqladmin` ile:
```
taha@taha-Inspiron-3558:~/IdeaProjects/tumblrConsumer$ mysqladmin processlist
mysqladmin: connect to server at 'localhost' failed
error: 'Access denied for user 'taha'@'localhost' (using password: NO)'
```

#### mysql server servisi başlatmak:

`systemctl start mysql`

ya da 

`sudo /etc/init.d/mysql start`

##### launch at reboot/ on startup

`systemctl enable mysql`

#### çalışan servisin durumuna bakmak:

`sudo systemctl status mysql`

### working on mysql server 

#### with mysql shell

`/usr/bin/mysql -u root -p`

### forgotten root password 

#### resetting root password

1. `sudo /etc/init.d/mysql stop`
> stopping mysql server service
2. `sudo mysqld_safe --skip-grant-tables &`
> starting mysql without a password

```
taha@taha-Inspiron-3558:~/IdeaProjects/tumblrConsumer$ 2019-07-14T21:12:27.544395Z mysqld_safe Logging to syslog.
2019-07-14T21:12:27.551855Z mysqld_safe Logging to '/var/log/mysql/error.log'.
2019-07-14T21:12:27.556798Z mysqld_safe Directory '/var/run/mysqld' for UNIX socket file don't exists.
```

düzeltme için:

```
taha@taha-Inspiron-3558:~/IdeaProjects/tumblrConsumer$ sudo mkdir -p /var/run/mysqld
taha@taha-Inspiron-3558:~/IdeaProjects/tumblrConsumer$ sudo chown mysql:mysql /var/run/mysqld
```

sonra aynı adımdan devam:

```
taha@taha-Inspiron-3558:~/IdeaProjects/tumblrConsumer$ sudo mysqld_safe --skip-grant-tables &
[1] 31461
taha@taha-Inspiron-3558:~/IdeaProjects/tumblrConsumer$ 2019-07-14T21:19:15.400184Z mysqld_safe Logging to syslog.
2019-07-14T21:19:15.410578Z mysqld_safe Logging to '/var/log/mysql/error.log'.
2019-07-14T21:19:15.534597Z mysqld_safe Starting mysqld daemon with databases from /var/lib/mysql
mysql -u root
Welcome to the MySQL moni....

mysql> use mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> update user set authentication_string=PASSWORD("1") where User='root'; Query OK, 1 row affected, 1 warning (0.06 sec)
Rows matched: 1  Changed: 1  Warnings: 1
```

Burada `UPDATE user SET plugin="mysql_native_password";` bunu da yapmak gerekiyor.

> If you miss the third 'set plugin' statement, you'll successfully update your password but still won't be able to connect to your server. My system, for example, had the plugin value set to 'auth socket.' Even with the right login details, my server threw errors about my missing socket, and I needed to shut down, restart in safe mode, and switch both values again.

> Finally, the 'flush privileges' command reloads the server's in-memory copy of the grant tables. Modifying the user table with UPDATE doesn't load the changes into the tables immediately, unlike the higher-level GRANT or SET commands.

from [ACCESS DENIED: Reset MySQL root user password ](https://dev.to/oneearedmusic/access-denied-reset-mysql-root-user-password-2hk4) by  Erika Burdon

Aslında buradaki konu:

`UPDATE mysql.user SET plugin = 'mysql_native_password' WHERE user = 'root' AND plugin = 'unix_socket';
FLUSH PRIVILEGES;`

> System like Ubuntu prefers to use auth_socket plugin.
> Instead you may want to back with the mysql_native_password, which will require user/password to authenticate.

Ek olarak,

`SHOW GRANTS FOR 'root'@'localhost';`'ı da kullan.

yine ve yeniden son defa özet:

> This is specific to Ubuntu 18.04 LTS and MySQL 5.x Followed this link Follow everything from here onwards:

```
sudo mysql_secure_installation
sudo mysql
```

> Once logged into MySQL then from the MySQL prompt execute these commands:

```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
FLUSH PRIVILEGES;
```

> Now verify that the table has the password for the root

`SELECT user,authentication_string,plugin,host FROM mysql.user;`

> This solved my issue and now i am able to login.

```
mysql> flush privileges;
Query OK, 0 rows affected (0.06 sec)

mysql> quit
Bye
taha@taha-Inspiron-3558:~/IdeaProjects/tumblrConsumer$ sudo /etc/init.d/mysql stop
[ ok ] Stopping mysql (via systemctl): mysql.service.
taha@taha-Inspiron-3558:~/IdeaProjects/tumblrConsumer$ sudo /etc/init.d/mysql start
[....] Starting mysql (via systemctl): mysql.serviceJob for mysql.service failed.
See "systemctl status mysql.service" and "journalctl -xe" for details.
 failed!
```

başlatamadı, hata var; bakalım:

```
taha@taha-Inspiron-3558:/var/log/mysql$ sudo tail -f /var/log/mysql/error.log
2019-07-14T21:36:49.160614Z 0 [ERROR] InnoDB: Unable to lock ./ibdata1 error: 11
2019-07-14T21:36:49.160649Z 0 [Note] InnoDB: Check that you do not already have another mysqld process using the same InnoDB data or log files.
```

> Run the following commands to kill mysql processes running on the server
`sudo killall -u mysql`
> or
`sudo killall -9 mysql`
> Note – Flags -u kills all the processes started by user mysql -9 flag kills all the mysql process running by all users across the server. For proper mysql processes cleanup you can run both commands without any harm. Start mysql server daemon
`sudo service mysql start`

Buralarda çok takıldım ama sonuç olarak şifreyi değiştirebilmişim. 
Restart edince zaten startup'da mysql server service'i başlamış oluyor. 

**Önemli olan:** `sudo mysql -u root -p` ile mysql shell'ine girmek.
[ERROR 1698 (28000): Access denied for user 'root'@'localhost'](https://stackoverflow.com/questions/39281594/error-1698-28000-access-denied-for-user-rootlocalhost)

### paths and directories and config file locations

data directory:
```
mysql> select @@datadir
    -> ;
+-----------------+
| @@datadir       |
+-----------------+
| /var/lib/mysql/ |
+-----------------+
1 row in set (0.01 sec)
```

#### references:

1. [How To Move a MySQL Data Directory to a New Location on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-move-a-mysql-data-directory-to-a-new-location-on-ubuntu-16-04)
> `mysql > select @@datadir;`
> `sudo rsync -av /var/lib/mysql /mnt/volume-nyc1-01`
> `sudo mv /var/lib/mysql /var/lib/mysql.bak`
> `sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf` burada datadir tanımı değiştirilir: `datadir=/mnt/volume-nyc1-01/mysql` eklenir.
```
taha@taha-Inspiron-3558:~$ cat /etc/mysql/mysql.conf.d/mysqld.cnf 
#
# The MySQL database server configuration file.
...
[mysqld]
#
# * Basic Settings
#
user            = mysql
pid-file        = /var/run/mysqld/mysqld.pid
socket          = /var/run/mysqld/mysqld.sock
port            = 3306
basedir         = /usr
datadir         = /var/lib/mysql
tmpdir          = /tmp
lc-messages-dir = /usr/share/mysql
skip-external-locking
...
# Error log - should be very few entries.
#
log_error = /var/log/mysql/error.log
...
# Here you can see queries with especially long duration
#slow_query_log         = 1
#slow_query_log_file    = /var/log/mysql/mysql-slow.log
#long_query_time = 2
#log-queries-not-using-indexes
```

## managing database operations:

### create, delete, access databases and tables
```
SHOW DATABASES;
CREATE DATABASE database name;
DROP DATABASE database name;
USE events;
SHOW tables; 
```

### creating listing changing table schema:

CREATE TABLE potluck (id INT NOT NULL PRIMARY KEY AUTO_INCREMENT, 
name VARCHAR(20),
food VARCHAR(30),
confirmed CHAR(1), 
signup_date DATE);

DESCRIBE potluck;

ALTER TABLE potluck ADD email VARCHAR(40) AFTER name;  
ALTER TABLE potluck DROP email;

### users and roles

#### view users:
`SELECT User, Host, authentication_string FROM mysql.user;`

#### add db users:
```
INSERT INTO mysql.user (User,Host,authentication_string,ssl_cipher,x509_issuer,x509_subject)
VALUES('demouser','localhost',PASSWORD('demopassword'),'','','');
FLUSH PRIVILEGES;

GRANT ALL PRIVILEGES ON demodb.* to demouser@localhost;
FLUSH PRIVILEGES;

SHOW GRANTS FOR 'demouser'@'localhost';
```

## Wrap-up/review/retro of the day ya da take aways/keys to take away

1. > spring boot lombok tutorial
2. > hibernate import.sql
`data.sql` ve `schema.sql`

## retro & backlog
1. hibernate mysql generates ddl but dont update database
> Sebebi hem ddl generation hem de ddl autocreation'unun aynı anda olması.
>> spring data jpa hibernate mysql generates ddl but does not create tables on database
2. using flyway and hibernate ddl create together
3. spring data hibernate ddl schema creation after initializer seed commandlinerunner
4. Why Spring Boot doesn't create schema for MySQL DB while using "spring.jpa.hibernate.ddl-auto=create"?
> çözüm yukarıda anlattığım generation ve creation'u birlikte kullanmaktı ama buraya şunu da yazmışlar:
`spring.datasource.url=jdbc:mysql://localhost:3309/course_api_db?createDatabaseIfNotExist=true`
5. Table 'DBNAME.hibernate_sequence' doesn't exist
> `spring.jpa.hibernate.use-new-id-generator-mappings`
6. mysql create user with root privileges
7. spring data jpa generate dll or flyway
8. spring boot hibernate create database table schema from entities
9. spring boot database migrations

## further reading
1. [Combine Hibernate's automatic schema creation and database versioning](https://stackoverflow.com/questions/18536256/combine-hibernates-automatic-schema-creation-and-database-versioning/18809483)
> a solution for both development and production environments and a workflow
2. [#HOWTO: Best Practices for Flyway and Hibernate with Spring Boot](https://rieckpil.de/howto-best-practices-for-flyway-and-hibernate-with-spring-boot/)
> With the Flyway dependency on the classpath, Spring Boot will initialize everything for you. Once the first connection is established to the database, Flyway will create a flyway_schema_history table to track the already applied database scripts with their checksum. 
> (For lazy developer) Let Hibernate create an initial version of the schema
ddl generation'dan bahsediyor:
>> Typing the SQL scripts for a bigger (already existing) JPA model might be laborious. Fortunately, JPA offers a feature for the schema-generation (for lazy developers). With JPA you can output the DDL scripts to a file and modify/adjust them if needed. For this I often create a specific Spring profile and connect to a local database:
```
spring:
  profiles: generatesql
  datasource:
    url: jdbc:postgresql://localhost:5432/postgres
    username: postgres
  flyway:
    enabled: false
  jpa:
    properties:
      javax: 
        persistence:
          schema-generation:
            create-source: metadata
            scripts:
              action: create
              create-target: src/main/resources/ddl_jpa_creation.sql
```
3. [database initialization](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-database-initialization.html)
> Initialize a Database Using JPA
> Initialize a Database Using Hibernate
> Initialize a Database
>> Spring Boot can automatically create the schema (DDL scripts) of your DataSource and initialize it (DML scripts). It loads SQL from the standard root classpath locations: schema.sql and data.sql, respectively. In addition, Spring Boot processes the schema-${platform}.sql and data-${platform}.sql files (if present), where platform is the value of spring.datasource.platform. This allows you to switch to database-specific scripts if necessary. For example, you might choose to set it to the vendor name of the database (hsqldb, h2, oracle, mysql, postgresql, and so on).
4. [Spring Boot insert sample data into database upon startup](https://stackoverflow.com/questions/44749286/spring-boot-insert-sample-data-into-database-upon-startup/44754394)
5. [Spring Boot - Loading Initial Data](https://stackoverflow.com/questions/38040572/spring-boot-loading-initial-data)
6. [Spring Boot Reference Documentation](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/#getting-started-installing-the-cli)
7. [Should you create or generate your table model?](https://thoughts-on-java.org/create-generate-table-model/) By Thorben Janssen
8. [Standardized schema generation and data loading with JPA 2.1](https://thoughts-on-java.org/standardized-schema-generation-data-loading-jpa-2-1/) By Thorben Janssen
9. [Spring Boot Database Migrations with Flyway](https://www.callicoder.com/spring-boot-flyway-database-migration-example/)
10. [Introduction to JPA Using Spring Boot Data](https://dzone.com/articles/introduction-to-jpa-using-spring-boot-data-jpa)

# Gün4:

Bugüne geçmeden, github'da teams altında değil, doğrudan kendi hesabımın altında `Projects > New Project > Private olarak Automated Kanban Board'lı bir proje oluşturdum`
Olabildiğince `finer-grained` PBI'lar olarak yaptıklarımı ve yapmakta olduklarımı ve kalan tüm yapılacak PBI'ları ekledim. 
1. 12 tane yapılan
2. 15 tane yapılacak (*)bu belli bir fonksiyonelite için gerekli olduğundan bu kadar fazla. Yani bir günde yapılmasa da tamamı yapıldığında bir milestone olacak. 
3. 33 tane (*) Bazısı Epic ya da Feature şeklinde çok genel.   

## spring boot profiles to manage environments

> Spring Profiles helps segregating your application configurations, and make them available only in certain environments. ﻿
> An application run on many different environments. For example, Dev, QA, Test, Stage, Production etc. 
> Therefore, an application may need different configurations on different environments.. 
> In other words, configurations like databases, messaging systems, server ports, security will be different from environment to environment.

### how to set and use different profiles in spring boot

>  There are multiple ways to set profiles for your springboot application.

1. Enabling active profile in application.yml
`spring.profiles.active=dev`

2. Programmatic way:
`SpringApplication.setAdditionalProfiles("dev");`

ya da 

```java
@SpringBootApplication
public class Application {

    public static void main(String[] args) {
    new SpringApplicationBuilder()
            .sources(Application.class)
            .profiles("prod")
            .run(args);
    }
}
```
3. Tests make it very easy to specify what profiles are active
`@ActiveProfiles("dev")`

4. In a Unix environment
`export spring_profiles_active=dev`

5. JVM System Parameter from Run configurations
`-Dspring.profiles.active=dev`
    
6. By specifying in command line while running .jar file.
`java -jar file.jar --spring.profiles.active=production`


### @Profile on @Bean methods
> Spring Profiles are not limited to deployment environments. In other words, It can be used to bring any variation in application profile. 

```java
        @Bean
        @Profile("oracle")
        public DataSource oracleDataSource(){
            DataSource dataSource;
            // implementation skipped
            return dataSource;
        }
        @Bean
        @Profile("mysql")
        public DataSource mySqlDataSource(){
            DataSource dataSource;
            // implementation skipped
            return dataSource;
        }
```
from [How to use Spring Profiles – Tutorial with Examples](https://www.amitph.com/how-to-use-spring-profiles/)

or more generic/enterprise:

```java
Config.class

public interface Config {
   setup();   
}

@Profile("dev")
@Component
public class DevConfig implements Config {

    @Override
    public void setup() {
        //setup some configuration here
        System.out.println("Development configuration setup");
    }
}

@Profile("prod")
@Component
public class ProdConfig implements Config {

    @Override
    public void setup() {
        //setup some configuration here
        System.out.println("Production configuration setup");
    }
}


@SpringBootApplication
public class Application {

    private Config config;

    @Autowired
    Application(Config config) {
        this.config = config;
    }

    public static void main(String[] args) {
        new SpringApplicationBuilder()
                .sources(Application.class)
                .profiles("prod")
                .run(args);
    }

    @Bean
    CommandLineRunner execute() {
        return args -> config.setup();
    }
}
```

or 

```java
@Autowired
private Environment environment;void environmentSpecificMethod() {
    if (environment.acceptsProfiles("prod")) {
        System.out.println("This will be executed only in production mode");
        //do some stuff here
    } else {
        System.out.println("This will be executed for all other profiles");
    }
}
```

from [Spring Boot Profiles](https://medium.com/@ihorsokolyk/spring-boot-profiles-9349291f6e7b)

## spring boot security 

### disable security - how to exclude default security autoconfiguration

dependency olarak spring security'i eklediğimizde biz herhangi bir şey konfigüre etmesek de default olarak basic security kısmı eklenir. 
test amaçlı olarak curl, postman ya da herhangi bir test sınıfından mevcut rest api'mizi test etmek istediğimizde not authorized hatası alırız bu durumda:

```
taha@taha-Inspiron-3558:~$ curl -v http://localhost:8080/tumblr/posts
*   Trying 127.0.0.1...
* TCP_NODELAY set
* Connected to localhost (127.0.0.1) port 8080 (#0)
> GET /tumblr/posts HTTP/1.1
> Host: localhost:8080
> User-Agent: curl/7.61.0
> Accept: */*
> 
< HTTP/1.1 401 
< Set-Cookie: JSESSIONID=B3A261EBA30152C4BB412CE4A6F5B7F3; Path=/; HttpOnly
< WWW-Authenticate: Basic realm="Realm"
< X-Content-Type-Options: nosniff
< X-XSS-Protection: 1; mode=block
< Cache-Control: no-cache, no-store, max-age=0, must-revalidate
< Pragma: no-cache
< Expires: 0
< X-Frame-Options: DENY
< Content-Type: application/json;charset=UTF-8
< Transfer-Encoding: chunked
< Date: Sat, 20 Jul 2019 21:27:23 GMT
<                                                                                                                                     
* Connection #0 to host localhost left intact                                                                                         
{"timestamp":"2019-07-20T21:27:23.937+0000","status":401,"error":"Unauthorized","message":"Unauthorized","path":"/tumblr/posts"}

taha@taha-Inspiron-3558:~$ curl -v http://localhost:8080/tumblr/posts                                                                       
*   Trying 127.0.0.1...                                                                                                               
* TCP_NODELAY set
* Connected to localhost (127.0.0.1) port 8080 (#0)
> GET /tumblr/posts HTTP/1.1
> Host: localhost:8080
> User-Agent: curl/7.61.0
> Accept: */*
> 
< HTTP/1.1 200 
< Content-Length: 0
< Date: Sat, 20 Jul 2019 21:59:14 GMT
```

### Spring boot getting 401 unauthorized status code for simple get request

> You need to configure Spring Security, by default all routes all secured for authrorization

#### references:

1. [How to secure REST API with Spring Boot and Spring Security?](https://stackoverflow.com/questions/32548372/how-to-secure-rest-api-with-spring-boot-and-spring-security)

```java
@Configuration
public class WebSecurityConfiguration extends WebSecurityConfigurerAdapter {

    @Override
    public void configure(HttpSecurity http) throws Exception {
        http.requestMatchers().antMatchers("/users/all").permitAll();
    }
}
```

> To discard the security auto-configuration and add our own configuration, we need to exclude the SecurityAutoConfiguration class.

```java
@SpringBootApplication(exclude = { SecurityAutoConfiguration.class })
public class SpringBootSecurityApplication {
 
    public static void main(String[] args) {
        SpringApplication.run(SpringBootSecurityApplication.class, args);
    }
}
```

> Or by adding some configuration into the application.properties file:
	
`spring.autoconfigure.exclude=org.springframework.boot.autoconfigure.security.SecurityAutoConfiguration`

## Test a REST API with curl

```
curl -v http://www.example.com/
curl -v http://localhost:8082/spring-rest/foos/9
curl -d 'id=9&name=xxx' http://localhost:8082/spring-rest/foos/new
```

> if the endpoint expects json send request body as json and set header 

`curl -d '{"id":9,"name":"xxx"}' -H 'Content-Type: application/json' http://localhost:8082/spring-rest/foos/new`

> PUT, DELETE 
```
curl -d @request.json -H 'Content-Type: application/json' -X PUT http://localhost:8082/spring-rest/foos/9
curl -X DELETE http://localhost:8082/spring-rest/foos/9
```

> custom headers:

`curl -d @request.json -H "Content-Type: application/json" 
  -H "Accept: application/json" http://localhost:8082/spring-rest/foos/new`
  
> authentication
>> authentication with curl spring boot rest 

## spring boot properties 

1. spring boot propertysource and value

### inheriting properties

[Spring Boot Inherit application.properties from dependency](https://stackoverflow.com/questions/35663679/spring-boot-inherit-application-properties-from-dependency)

> One possible solution is to
1. > apply a custom profile to the dependency
2. > include inheritable settings in the application-customprofile.properties
3. > have the dependent(s) set spring.profiles.include=customprofile in application[-{profile}].properties (note: if set in application.properties, it applies for all profiles)


## spring boot maven multi module project

## spring externalizing secrets

## Where is the Tumblr API Key? / Tumblr API 2: Where is the “OAUTH_TOKEN” and “OAUTH_TOKEN_SECRET”

> You can get the consumer_key and consumer_secret after registering a new app at Tumblr [here](https://www.tumblr.com/oauth/apps) (it is called OAuth Consumer Key and there is a link below to get the Secret key of the app).
> The token and token_secret keys are generated after allowing access of an app to a Tumblr account. They will be returned as parameters after returning to the callback script.

### detailed explanation:

> Probably this is old thread and you might have figured out how to work with it, Although I am trying to post the entire process for some newbies here, As it took a while for me to understand the entire process and work flow.
> I have worked a lot with OAuth v2 and Tumblr API.
1. > First and foremost we need to get registered to tumblr and once its done you get CONSUMER KEY and SECRET. These are the initial set of keys for further process.
2. > After you have registerd and trying to communicate to the provider, we need request for REQUEST TOKEN and SECRET. This is one time access and it has nonce time attached to them. You can get that here (https://api.tumblr.com/console/calls/user/info).
3. > Once you have REQUEST TOKEN AND SECRET. At this point you have registered your application and granted requested access to provider. Now you need to authorize yourself with the provider using /authorize url. At this point you get back OAUTH TOKEN and OAUTH VERFIER.
4. > Once you have above tokens last step of this process is to fetch ACCESS TOKEN ANS TOKEN SECRET by apssing OAUTH TOKEN and CONSUMER KEY using /access/ url. After this step is succesfull you have ACCESS TOKEN.
5. > Now store your CONSUMER KEY AND SECRET from first step and ACCESS TOKEN AND TOKEN SECRET from 4th step somewhere safe and use these keys in future for any communication to the provider.
> NOTE: 1. Its generally assumed that access token expire but in reality they don't expire. They will expire only if user revokes the access. 2. After you have your token you can change your login credentials of Tumblr any number of times, this WILL NOT EFFECT the keys fetched.
> I hope this is helpful for someone looking for the process and myths and questions regarding the process.

## elaborate rest api

```
@Controller
// To elaborate api to manage from security or request routing or api maintenance perspectives.
@RequestMapping("/api")
public class TumblerConsumerController {
    @Autowired
    private Consumer tumblrConsumer;

    @RequestMapping(method = RequestMethod.GET, value="/tumblr/posts")
    @ResponseBody
    public void getPosts()   {
        tumblrConsumer.consume();
    }
}
```

## consuming rest api with gson / spring boot json response processing

### references
1. [Invoking RESTful Web Service using API in java.net and GSON](https://javabeat.net/invoking-restful-web-service-using-api-in-java-net-and-gson/)
2. spring boot gson generic type
    1. [GSON - Serializing and Deserializing Generic Types ](https://www.javaguides.net/2018/10/gson-serializing-and-deserializing-generic-types.html)
    2. [Gson: Deserialization of Generic Types](https://dzone.com/articles/deserialization-1)
3. Consuming a RESTful Web Service in spring boot

## configuring hibernate relationships between entities

spring boot hibernate entity relationships

1. [What is the difference between Unidirectional and Bidirectional JPA and Hibernate associations?](https://stackoverflow.com/questions/5360795/what-is-the-difference-between-unidirectional-and-bidirectional-jpa-and-hibernat)
>This is a Unidirectional association
```java
public class User {
    private int     id;
    private String  name;
    @ManyToOne
    @JoinColumn(
            name = "groupId")
    private Group   group;
}

public class Group {
    private int     id;
    private String  name;
}
```

> The Bidirectional association

```java
public class User {
    private int     id;
    private String  name;
    @ManyToOne
    @JoinColumn(
            name = "groupId")
    private Group   group;
}
public class Group {
    private int         id;
    private String      name;
    @OneToMany(mappedBy="group")
    private List<User>  users;
}
```
2. [JPA and Hibernate Many To Many Relationship Mapping Example with Spring Boot and MySQL](https://hellokoding.com/jpa-many-to-many-relationship-mapping-example-with-spring-boot-maven-and-mysql/)
3. [JPA / Hibernate One to Many Mapping Example with Spring Boot](https://www.callicoder.com/hibernate-spring-boot-jpa-one-to-many-mapping-example/)


### configuring enum types in entities

[The best way to map an Enum Type with JPA and Hibernate](https://vladmihalcea.com/the-best-way-to-map-an-enum-type-with-jpa-and-hibernate/)

## backlog

1. react bootstrap card layout
2. integrating bootstrap with react
3. using react-bootstrap
4. using third party libraries in react
5. why do we need react router
6. spring boot react websocket
7. spring boot profiles
8. spring boot rest simple authentication to test with curl
9. spring boot cannot resolve permitall
10. spring boot rest bypass security permitting all users all paths for test purposes
11. java maven idea class resolve but some methods does not resolve site:stackoverflow.com
Burada öyle bir hata yaptım ki... Normalde çokça defa idea class ya da metotları cannot resolve diye kırmızı gösterirdi.
Bunun için `invalidate caches` derdik ya da `.idea` dosyasını silerdik. Beni bu yanılttı ve hızlıca yapmaya çalışırken bir şeye dikkat edemedim.
Yaptığım şey şuydu, hızlıca bir sınıf oluşturup içerisine third party library'deki bir metot çağrısını ekledim. Sınıfı görüyordu ama metodu görmüyordu. 
Hatta sonrasında `System.out.println` vardı ve `System.out`u görürken `println`i görmüyordu.
Program derlenmiyordu, aslında buradan uyanmaya çalıştım çünkü idea'dan kaynaklanan durum olsa kırmızı görünür ama derlenirdi. 
Burada hiç derlenmiyordu çünkü metot çağrısını doğrudan sınıfın içine(constructor ya da metot içine değil) yazmış olmamdı :)
12. using jumblr
13. tumblr api consumer
14. tumblr api jumblr textpost
> title ve body yani en önemli kısmı dönmüyor jumblr wrapper'ı. 
Post Pojo'su var ve bundan türüyen TextPost pojo'su var bu title ve body içeriyor ama getPosts yaparken dönen değer Post'a alınmış. 
TextPost'u almak için değil de api'yi kullanarak paylaşım yapmak için düşünmüşler.
raw olarak kendim api'yi consume edeceğim. 
15. when declaring relationships in hibernate how must we do bidirectional or one directional

JDK 11 den kaynaklandığını düşünüp JDK 8'i kurup konfigüre etmiştim hatta.

> Checking JAVA_HOME on Linux
`$JAVA_HOME/bin/javac -version`

## References

1. [react-bootstrap library - cards component](https://react-bootstrap.github.io/components/cards/)
2. [using third-party libraries from react](https://www.sitepoint.com/integrating-bootstrap-with-react/)
3. [adding bootstrap](https://facebook.github.io/create-react-app/docs/adding-bootstrap)
4. [Import Bootstrap As a Dependency](https://blog.logrocket.com/how-to-use-bootstrap-with-react-a354715d1121/)
5. [How do I use/include third party libraries in react?](https://stackoverflow.com/questions/45658200/how-do-i-use-include-third-party-libraries-in-react)
6. [Why I don't use React-Router](https://asleepysamurai.com/articles/why-i-dont-use-react-router)
7. [Learning Redux Without Using Redux](https://asleepysamurai.com/articles/learning-redux-without-using-redux)
8. [The What, Why and How of React (Routers)](https://dev.to/mangel0111/the-what-why-and-how-of-react-routers-41b)
9. [You might not need React Router](https://www.freecodecamp.org/news/you-might-not-need-react-router-38673620f3d/)
10. [Full Stack Reactive with Spring WebFlux, WebSockets, and React](https://developer.okta.com/blog/2018/09/25/spring-webflux-websockets-react)
11. [stomp-spring-react](https://github.com/akarasev/stomp-spring-react/blob/develop/src/App.js)
12. [Sample about Websocket using React and Spring Boot Oauth2 to send a notification between user.](https://github.com/zcmgyu/websocket-spring-react)
13. [Using Custom React Hooks to Simplify Forms](https://upmostly.com/tutorials/using-custom-react-hooks-simplify-forms)
14. [The OAuth 2.0 Authorization Framework](https://tools.ietf.org/html/rfc6749
15. [Where is the Tumblr API Key?]()

## follow

1. [bg dam - asleepysamurai](https://asleepysamurai.com/) mainly on `#react`

# Gün5

## serializing/marshalling when creating json from object to send to client in a rest api(mainly on request processing) 
## deserializing/unmarshalling when creating object from json to persist data(mainly on response processing)

## .net core api development on ubuntu

## design decisions on the usage of Post superclass from Jumblr
### downcasting Post to TextPost.
1. [Does the visitor pattern violate the Liskov Substitution Principle](https://softwareengineering.stackexchange.com/questions/312276/does-the-visitor-pattern-violate-the-liskov-substitution-principle)
2. How to avoid (downcasting) and instanceof? 
3. instanceof Operator and Visitor Pattern Replacement in Java 8
4. Avoiding instanceof vs abstract God Class. Do I have an alternative?
5. how to access subclass extended state
6. visitor to get subclass without downcasting or instanceof
7. which design pattern to use get subclass without downcasting or instanceof
8. how to assign extended subtype data class to super type in inheritance hierarchy
9. PAPER - DiggingintotheVisitorPattern
10. Proxy, Decorator, Adapter and Bridge Patterns
11. The Visitor Pattern Re-visited 
12. Tips on how to balance Polymorphism/Inheritance/TypeData architecture with Prototype/Flyweight pattern in a complex card game?
13. Java Polymorphism - Selecting correct method based on subtype
14. Visitor Pattern or polymorphism?
15. Be not afraid of the Visitor, the big, bad Composite, or their little friend Double Dispatch - Posted by Jeremy Miller on October 31, 2007
16. When should I use the Visitor Design Pattern? [closed] #important
17. visitor design pattern to handle polymorphism in inheritance hierarchy extended data classes
    - visitor design pattern to manage extension inheritance data classes
    - visitor to manage extension inheritance pojo data classes
    - why i cannot assign pojo subtype to parent
18. Accessing subclass members through super class reference variables

Belki de en doğrusu amaca yönelik olarak en straightforward yolu uygulamaktı. Bu da `instanceof` kontrolüydü:
```java
List<Post> posts =  blog.draftPosts();
Post post = posts.get(0);
if(post instanceof TextPost) {
    System.out.println("textPost: " + ((TextPost) post).getTitle());
    System.out.println("textPost: " + ((TextPost) post).getBody());
    ...
}
```
Ek olarak reflection ile deneyebilirdik bu işlemi ya da stream functionalları ile instanceof kontrolünü kolaylaştırabilirdik.
Bazıları doğrusunun zaten instanceof kontrolü olduğunu söylüyor. Çünkü sınırlı sayıda sınıfın var zaten diyorlar.

Jumblr şöyleydi:
```java
public class Post extends Resource {
    public enum PostType {
        TEXT("text"),
        PHOTO("photo"),
        QUOTE("quote"),
        LINK("link"),
        CHAT("chat"),
        AUDIO("audio"),
        VIDEO("video"),
        ANSWER("answer"),
        POSTCARD("postcard"),
        UNKNOWN("unknown");

        private final String mType;

        PostType(final String type) {
            this.mType = type;
        }

        public String getValue() {
            return this.mType;
        }
    }

    protected PostType type;
    private Long id;
    private String author;
    private String reblog_key;
    private String blog_name;
    //...
}
```

ve

```java
public class TextPost extends SafePost {

    private String title;
    private String body;

    public String getTitle() {
        return title;
    }
    //...
}

public class QuotePost extends SafePost {

    private String text;
    private String source;
    
    public String getText() {
        return text;
    }
    //...
}

class SafePost extends Post {
    
    @Override
    public void save() {
        try {
            super.save();
        } catch (IOException ex) {
            // No files involved, no IOException
        }
    }

}
```

deserialize ederken:

```java
public class PostDeserializer implements JsonDeserializer<Object> {

    @Override
    public Object deserialize(JsonElement je, Type type, JsonDeserializationContext jdc) throws JsonParseException {
        
        //Taha: Reflection kullanımına dikkat.
        JsonObject jobject = je.getAsJsonObject();
        String typeName = jobject.get("type").getAsString();
        String className = typeName.substring(0, 1).toUpperCase() + typeName.substring(1) + "Post";
        try {
            Class<?> clz = Class.forName("com.tumblr.jumblr.types." + className);
            return jdc.deserialize(je, clz);
        } catch (ClassNotFoundException e) {
            System.out.println("deserialized post for unknown type: " + typeName);
            return jdc.deserialize(je, UnknownTypePost.class);
        }
    }
}
```

ResponseWrapper.java:

```java
private Gson gsonParser() {
    return new GsonBuilder().
        registerTypeAdapter(Post.class, new PostDeserializer()).
        create();
}
```

olduğundan ve yine ResponseWrapper'da:

```java
// NOTE: needs to be duplicated logic due to Java erasure of generic types
public List<Post> getPosts() {
        Gson gson = gsonParser();
        JsonObject object = (JsonObject) response;
        List<Post> l = gson.fromJson(object.get("posts"), new TypeToken<List<Post>>() {}.getType());
        for (Post e : l) { e.setClient(client); }
        return l;
    }
```

Burada, alınan JSON datasında posts json objesi bir List<Post> tur diyoruz ve bunu alıyoruz. posts'un altındaki her bir post, `registerTypeAdapter` ile ilişkilendirildiğinden
ve alt tiplere göre ilişkilendirildiğinden response'daki type'a göre uygun alt tipe alınmış olacak ve alt tipi de Post supertype'ında saklayacağız. 

Ayrıca yukarıda yorumda belirtildiği type erasure konusundan kurtulmak için bir çözümüm var, onu uygula.


## OAuth processing
[ScribeJava, the simple OAuth client Java lib!](https://github.com/scribejava/scribejava)

# Gün6:

## yapılanlar
1. worked on jsoup: like a web crawler, after get a page and parsed it get links and fetch those links' pages:
ekşisözlükteki gündem alınarak her bir gündem maddesi için oradaki entryleri fav sayısına göre sıralayarak bir map'te tuttum. Bu desc sıralayıp en populer 10 entry'yi göstermek için kullanılabilir. Ayrıca tumblr'daki etiketleri indirip LDA vs. ya da lucene indexing ile oluştuurlmuş etiketlere göre sadece ilgi alanıma girenlerden belli popülerleri ya da tamamını göster diyebilirim.
    1.1 Pagination eksik. 
    1.2 AJAX ile işlem yapan ya da javascript ile JSON getiren sayfaların parse edilmesi noktasında Selenium WebDriver ya da diğer tool'lara bakılacak. 
    1.3 jsoup ile POST da yapılabiliyor ya da header parametreleri de gönderilebiliyor, bunlara bak.
    1.4 en uç senaryoda web sayfası HttpClient ile bir şekilde alındıktan sonra sadece html'i parse etmek için kullanılabilir.
    1.5 benim tumblr ya da diğer içeriklerim html olacağından bunları hem rich text editor'de göstermek üzere html üzere saklarken hem de bazı durumlarda bununla html'i parse edebiliriz.

2. pretty print senaryolarını tam uygulayamadım, ama reflection ile field'lara ulaşıp yazdırdım.

3. Gson ya da Jackson farketmez, Circular reference içeren bir obje ile işlem yaptığımmızda Stackoverflowerror alıyoruz. 

4. array, list, set, map işlemlerinde java 8 lambda, functional ve stream API'yi kullandım. daha birçok olası senaryoda extensively kullanılacak. 

## development workflow için.

ubuntu'da screen recording and saving as a gif yani `gif recording` tool'u olarak `peek`'i kurdum.

# Gün7:

Bugün aşağıdaki durumlardan dolayı verimli çalışamadım:

1. Intellij IDEA otomatik olarak upgrade olmuş. Bu yüzden LXDE Lubuntu start menüdeki `Programming > Intellij IDEA` işlevsiz kalmış. Basınca launch etmiyordu.
Çözüm için araştırdım. Lubuntu'da kısayollar `.desktop` uzantılı olup içi de aşağıdaki gibi oluyormuş. 
Sorun şu ki IDEA upgrade sırasında mevcut `.dekstop`ı silmiş. 

Aşağıdaki gibi bir kısayol oluşturup,
 
```
taha@taha-Inspiron-3558:/$ cat ~/idea.desktop 
[Desktop Entry]
Type=Application
Terminal=false
Exec=/bin/sh /snap/intellij-idea-community/163/bin/idea.sh
Name=Intellij
Icon=/snap/intellij-idea-community/163/bin/idea.png
```

bunu aşağıya iki isimle koydum, ama olmadı.

```
taha@taha-Inspiron-3558:~/.local/share/applications$ ll
total 16
drwxr-xr-x  2 taha taha 4096 Tem 27 19:36 ./
drwxr-xr-x 25 taha taha 4096 Tem 26 02:58 ../
-rw-rw-r--  1 taha taha  173 Tem 27 19:35 idea.desktop
-rw-rw-r--  1 taha taha  173 Tem 27 19:36 jetbrains-idea-ce.desktop
```

Mevcut uzantılar burada ama burası root'un yetki alanında olduğundan burada oluşturamıyorum yukarıdaki yerde oluşturmak gerekiyordu. 
Buraya bakınca jetbrains-idea-ce.desktop gerçekten de yoktu.  

```
taha@taha-Inspiron-3558:/usr/share/applications$ ls -a
.
..
2048-qt.desktop
apport-gtk.desktop
apturl.desktop
bluetooth-sendto.desktop
code.desktop
code-url-handler.desktop
com.canonical.launcher.amazon.desktop
compton-conf.desktop
compton.desktop
com.uploadedlobster.peek.desktop
defaults.list
emacs.desktop
...
```

Yeni versiyonu `/snap/intellij-idea-community/163` yani 2019.2'yi kullanmak istediğimde VM hatası verip başlamıyordu. Yine burası da Root'un yetkisinde olduğundan 
Root olsam da aşağıdaki içeriği değiştiremedim. `chown` vs. yapmak gerekiyor muhtemelen.

```
taha@taha-Inspiron-3558:/snap/intellij-idea-community/current/bin$ cat idea64.vmoptions 
-Xms128m
-Xmx750m
-XX:ReservedCodeCacheSize=240m
-XX:+UseConcMarkSweepGC
-XX:SoftRefLRUPolicyMSPerMB=50
-ea
-XX:CICompilerCount=2
-Dsun.io.useCanonPrefixCache=false
-Djava.net.preferIPv4Stack=true
-Djdk.http.auth.tunneling.disabledSchemes=""
-XX:+HeapDumpOnOutOfMemoryError
-XX:-OmitStackTraceInFastThrow
-Djdk.attach.allowAttachSelf
-Dawt.useSystemAAFontSettings=lcd
-Dsun.java2d.renderer=sun.java2d.marlin.MarlinRenderingEngine
-Dsun.tools.attach.tmp.only=true

```

Linux upgrade'lerde symbolic link oluşturup current ile güncel versiyonu tutuyor:

`/snap/intellij-idea-community/current/bin`

Eski versiyonu aşağıdaki gibi konsoldan çalıştırarak kullanıyorum :)

`taha@taha-Inspiron-3558:/snap/intellij-idea-community/152/bin$ ./idea.sh`

2. İki tane IDEA instance'ı çalışıyorken ve 30'dan fazla tab açıkken ara ara çok fazla donuyor. 
Swap space'i arttırmış olsam da çok fazla etkilemedi. RAM'i arttırmak gerekiyor ama DELL laptop'ta RAM slotu komple arka kapağın altında.
Tamamını sökmek gerekiyor. 

Bu işe girmeden acaba GPU'nun RAM'ini utilize edebilir miyim diye düşündüm ama zaten şu anda NVIDIA driver'ı göremiyor ubuntu.
Çok pis bir konu bu, bir defa uzun uğraşlar sonucu sistem göcüp tüm verilerimi kaybetmiştim. 

3. Projede `common` module Test class'ları eklemek istediğimde Project Structure bozuldu. `Project Structure > Modules` kısmını kurcalamıştım. 
Sources, test vs. kısımları olması gerektiği gibi yani sırasıyla `src/main/java` ve `src/test/java` olarak ayarladım bu defa tamamen bozuldu.
Invalidate caches vs. yaptım olmadı. Çözüm maven clean install yapmakmış ve reimport yapmak tabi ki. Bu kısım maven ile doğrudan ilgili dikkat et.

Ama bir şey keşfettim. Yeni bir module eklemek istediğimde directory'leri ben elle ekliyordum. 
Şimdi `Project Structure > Modules` kısmında `+` ya basıp ekliyorum.

## displaying ram info

`sudo dmidecode -t 17` //fiziksel bilgi, BIOS'ton slot'larla beraber
`free -m` MB cinsinden kulllanılan memory
`cat /proc/meminfo` ya da `vmstat -s` 

`htop` //görev yöneticisi olarak detaylı.

maximum RAM capacity:

```
taha@taha-Inspiron-3558:/snap/intellij-idea-community/152/bin$ sudo dmidecode -t 16
# dmidecode 3.1
Getting SMBIOS data from sysfs.
SMBIOS 2.8 present.

Handle 0x005B, DMI type 16, 23 bytes
Physical Memory Array
        Location: System Board Or Motherboard
        Use: System Memory
        Error Correction Type: None
        Maximum Capacity: 16 GB
        Error Information Handle: Not Provided
        Number Of Devices: 2
```

system information

```
taha@taha-Inspiron-3558:/snap/intellij-idea-community/152/bin$ sudo dmidecode -t 17
# dmidecode 3.1
Getting SMBIOS data from sysfs.
SMBIOS 2.8 present.

Handle 0x0060, DMI type 17, 34 bytes
Memory Device
        Array Handle: 0x005B
        Error Information Handle: Not Provided
        Total Width: 64 bits
        Data Width: 64 bits
        Size: 4096 MB
        Form Factor: SODIMM
        Set: None
        Locator: DIMM A
        Bank Locator: Not Specified
        Type: DDR3
        Type Detail: Synchronous
        Speed: 1600 MT/s
        Manufacturer: Samsung
        Serial Number: EFEAFD80
        Asset Tag: 9876543210
        Part Number: M471B5173DB0-YK0  
        Rank: 1
        Configured Clock Speed: 1600 MT/s
        
taha@taha-Inspiron-3558:/snap/intellij-idea-community/152/bin$ sudo dmidecode | grep -A 9 "System Information"
System Information
        Manufacturer: Dell Inc.
        Product Name: Inspiron 3558
        Version: Not Specified
        Serial Number: D5GVVB2
        UUID: 4C4C4544-0035-4710-8056-C4C04F564232
        Wake-up Type: Power Switch
        SKU Number: 06B0
        Family: Not Specified
```

ya da 

```
taha@taha-Inspiron-3558:/snap/intellij-idea-community/152/bin$ sudo lshw -C memory
  *-firmware                
       description: BIOS
       vendor: Dell Inc.
       physical id: 0
       version: A09
       date: 04/22/2016
       size: 64KiB
       capacity: 8128KiB
       capabilities: pci pnp upgrade shadowing cdboot bootselect edd int13floppy1200 int13floppy720 int13floppy2880 int5printscreen int9keyboard int14serial int17printer acpi usb smartbattery biosbootspecification netboot uefi
  *-cache:0
       description: L1 cache
       physical id: 42
       slot: L1 Cache
       size: 32KiB
       capacity: 32KiB
       capabilities: synchronous internal write-back instruction
       configuration: level=1
  *-cache:1
       description: L2 cache
       physical id: 47
       slot: L2 Cache
       size: 256KiB
       capacity: 256KiB
       capabilities: synchronous internal write-back unified
       configuration: level=2
  *-cache:2
       description: L3 cache
       physical id: 4c
       slot: L3 Cache
       size: 3MiB
       capacity: 3MiB
       capabilities: synchronous internal write-back unified
       configuration: level=3
  *-cache
       description: L1 cache
       physical id: 3d
       slot: L1 Cache
       size: 32KiB
       capacity: 32KiB
       capabilities: synchronous internal write-back data
       configuration: level=1
  *-memory
       description: System Memory
       physical id: 5b
       slot: System board or motherboard
       size: 4GiB
     *-bank:0
          description: SODIMM DDR3 Synchronous 1600 MHz (0,6 ns)
          product: M471B5173DB0-YK0
          vendor: Samsung
          physical id: 0
          serial: EFEAFD80
          slot: DIMM A
          size: 4GiB
          width: 64 bits
          clock: 1600MHz (0.6ns)
     *-bank:1
          description: DIMM [empty]
          physical id: 1
          slot: DIMM B
```

ultimately and short:

```
taha@taha-Inspiron-3558:/snap/intellij-idea-community/152/bin$ sudo lshw -short -C memory
H/W path       Device      Class          Description
=====================================================
/0/0                       memory         64KiB BIOS
/0/51/42                   memory         32KiB L1 cache
/0/51/47                   memory         256KiB L2 cache
/0/51/4c                   memory         3MiB L3 cache
/0/3d                      memory         32KiB L1 cache
/0/5b                      memory         4GiB System Memory
/0/5b/0                    memory         4GiB SODIMM DDR3 Synchronous 1600 MHz (0,6 ns)
/0/5b/1                    memory         DIMM [empty]
```

son olarak:

son olarak hangi ram'i satın alacağını bilmek için bunlar yeterli bilgiler olsa da DDR3L kafa karıştırabilir. 
Bu low voltage demek yani, DDR3 normalde 1.5V ded çalışırken laptopllarda pil ömrü için 1.35V de çalışan bunlar kullanılıyor. 
dmidecode, configured voltage kısmında bunu yazıyor ama bende yazmadı. Manifacturer spec'ten baktım.
SODIMM ile de Small olarak yani laptop için belirterek 
`4gb DDR3L SODIMM 1600MHz ram satın al` şeklinde aratabildim. :)

## optimizing vm options - jvm parameters 

1. Explicit Heap Memory – Xms and Xmx Options 
    - `-Xms2G -Xmx5G` 
2. Starting with Java 8, the size of Metaspace is not defined. 
Once it reaches the global limit, JVM automatically increases it, 
However, to overcome any unnecessary instability, we can set Metaspace size with:
`-XX:MaxMetaspaceSize=<metaspace size>[unit]`

Here, metaspace size denotes the amount of memory we want to assign to Metaspace.

As per Oracle guidelines, after total available memory, the second most influential factor is the proportion of the heap reserved for the Young Generation. By default, the minimum size of the YG is 1310 MB, and maximum size is unlimited.

We can assign them explicitly:
```	
-XX:NewSize=<young size>[unit] 
-XX:MaxNewSize=<young size>[unit]
```

For better stability of the application, choosing of right Garbage Collection algorithm is critical.

JVM has four types of GC implementations:

    Serial Garbage Collector
    Parallel Garbage Collector
    CMS Garbage Collector
    G1 Garbage Collector

These implementations can be declared with the below parameters:
```
-XX:+UseSerialGC
-XX:+UseParallelGC
-XX:+USeParNewGC
-XX:+UseG1GC
```
https://www.baeldung.com/jvm-parameters

# Gün8: 

## OAuth hakkında her şey:

### Understanding OAuth 2.0

1. register the application/client
> Before you can begin the OAuth process, you must first register a new app with the service. 
> When registering a new app, you usually register basic information such as application name, website, a logo, etc. 
    - > In addition, you must register a redirect URI to be used for redirecting users to for web server, browser-based, or mobile apps. 

2. set a redirect URI 
> The service will only redirect users to a registered URI, which helps prevent some attacks. 
> Any HTTP redirect URIs must be protected with TLS security, so the service will only redirect to URIs beginning with "https".
> This prevents tokens from being intercepted during the authorization process. 
> Native apps may register a redirect URI with a custom URL scheme for the application, which may look like demoapp://redirect.

3. get a Client ID(public) and Secret(confidential - keep on server) - Api Key/Api Secret ya da Consumer Key/Consumer Secret

> After registering your app, you will receive a client ID and a client secret. 
> The client ID is considered public information, and is used to build login URLs, 
> or included in Javascript source code on a page. The client secret must be kept confidential. 
> If a deployed app cannot keep the secret confidential, such as single-page Javascript apps or native apps, 
> then the secret is not used, and ideally the service shouldn't issue a secret to these types of apps in the first place

4. get authorization from the user

> The first step of OAuth 2 is to get authorization from the user. 
> **For browser-based or mobile apps, this is usually accomplished by displaying an interface provided by the service to the user**.

#### OAuth use cases

> OAuth 2 provides several "grant types" for different use cases. The grant types defined are:

    - Authorization Code for apps running on a web server, browser-based and mobile apps
    - Password for logging in with a username and password
    - Client credentials for application access
    - Implicit was previously recommended for clients without a secret, but has been superseded by using the Authorization Code grant with no secret.

##### web server apps - using Authorization Code
> Web server apps are the most common type of application you encounter when dealing with OAuth servers. 
> Web apps are written in a server-side language and run on a server where the source code of the application is not available to the public. 
> This means the application is able to use its client secret when communicating with the authorization server, which can help avoid some attack vectors.

1. Create a "Log In" link sending the user to: 
**(register ederken aldığım client ID'yi yani Api/Consumer Key'i kullanıyorum, ama Secret'ı kullanmıyorum henüz)**

`https://authorization-server.com/auth?response_type=code&client_id=**CLIENT_ID**&redirect_uri=REDIRECT_URI&scope=photos&state=1234z`

    - response_type=code - Indicates that your server expects to receive an authorization code
    - client_id - The client ID you received when you first created the application
    - redirect_uri - Indicates the URI to return the user to after authorization is complete
    - scope - One or more scope values indicating which parts of the user's account you wish to access
    - state - A random string generated by your application, which you'll verify later

> The user sees the authorization prompt:
> `The app Sample App by Taha would like the ability to access your basic information and photos. Allow Sample App access? Yes or No`

2. Get Authorization Code
**Redirect url'ime gelirken bana bir de AUTH_CODE veriyor.(Elimde Secret hala duruyor.)**

> If the user clicks "Allow," the service redirects the user back to your site with an auth code

`https://example-app.com/cb?code=AUTH_CODE_HERE&state=1234zyx`

```
    code - The server returns the authorization code in the query string
    state - The server returns the same state value that you passed
```

> You should first compare this state value to ensure it matches the one you started with. 
> You can typically store the state value in a cookie or session, and compare it when the user comes back. 
> This ensures your redirection endpoint isn't able to be tricked into attempting to exchange arbitrary authorization codes.

3. Get Access Token via Authorization code that server has given (Token Exhange)
**AUTH_CODE ve Secret'ımı kullanarak (hatta elimdeki tüm diğer bilgilerle) Access Token alıyorum.**

> Your server exchanges the auth code for an access token:

```
POST https://api.authorization-server.com/token
  grant_type=authorization_code&
  code=AUTH_CODE_HERE&
  redirect_uri=REDIRECT_URI&
  client_id=CLIENT_ID&
  client_secret=CLIENT_SECRET

    grant_type=authorization_code - The grant type for this flow is authorization_code
    code=AUTH_CODE_HERE - This is the code you received in the query string
    redirect_uri=REDIRECT_URI - Must be identical to the redirect URI provided in the original link
    client_id=CLIENT_ID - The client ID you received when you first created the application
    client_secret=CLIENT_SECRET - Since this request is made from server-side code, the secret is included
```

> The server replies with an access token and expiration time

```
{
  "access_token":"RsT5OjbzRn430zqMLgV3Ia",
  "expires_in":3600
}

or if there was an error

{
  "error":"invalid_request"
}
```

> Security: Note that the service must require apps to pre-register their redirect URIs.

### implementing oauth to consume an api

Tumblr API console üzerinden uygulamamı kaydettim:

```
 tyb_tumblr
OAuth Consumer Key: EE24eJ2hLQN4Cqm60BMXkPcldfxg5diuS580wmqlpOdlfg6f7c
Secret Key:  BQydaGVPft2j87pBeMeEluuSLm0wWsH06NrWITNX2WbAYOcpP3
```

> About Tumblr OAuth
> Tumblr supports OAuth 1.0a, accepting parameters via the Authorization header, with the HMAC-SHA1 signature method only. 
> There's probably already **an OAuth client library for your platform**.

> If you've worked with Twitter's OAuth implementation, you'll feel right at home with ours.

```
Request-token URL:
POST https://www.tumblr.com/oauth/request_token

Authorize URL:
https://www.tumblr.com/oauth/authorize

Access-token URL:
POST https://www.tumblr.com/oauth/access_token


package org.scribe.builder.api;

import org.scribe.model.Token;

public class TumblrApi extends DefaultApi10a {
    private static final String AUTHORIZE_URL = "https://www.tumblr.com/oauth/authorize?oauth_token=%s";
    private static final String REQUEST_TOKEN_RESOURCE = "http://www.tumblr.com/oauth/request_token";
    private static final String ACCESS_TOKEN_RESOURCE = "http://www.tumblr.com/oauth/access_token";

    public TumblrApi() {
    }

    public String getAccessTokenEndpoint() {
        return "http://www.tumblr.com/oauth/access_token";
    }

    public String getRequestTokenEndpoint() {
        return "http://www.tumblr.com/oauth/request_token";
    }

    public String getAuthorizationUrl(Token requestToken) {
        return String.format("https://www.tumblr.com/oauth/authorize?oauth_token=%s", requestToken.getToken());
    }
}
```

Official Tumblr API'sinde şu şekilde abstract edilmişti:

```java
// Create a new client
JumblrClient client = new JumblrClient("consumer_key", "consumer_secret");
client.setToken("oauth_token", "oauth_token_secret");
User user = jumblrClient.user();
for(Blog blog : user.getBlogs()) {
     List<Post> posts =  blog.draftPosts();
}
```

nasıl implemente edildiğine bakalım:

```java
public class JumblrClient {

    private RequestBuilder requestBuilder;
    private String apiKey;

    public JumblrClient() {
        this.requestBuilder = new RequestBuilder(this);
    }

    public JumblrClient(String consumerKey, String consumerSecret) {
        this();
        this.requestBuilder.setConsumer(consumerKey, consumerSecret);
        this.apiKey = consumerKey;
    }
    
    public void setToken(String token, String tokenSecret) {
        this.requestBuilder.setToken(token, tokenSecret);
    }
    
    private static String blogPath(String blogName, String extPath) {
        return "/blog/" + blogUrl(blogName) + extPath;
    }
    
    private static String blogUrl(String blogName) {
        return blogName.contains(".") ? blogName : blogName + ".tumblr.com";
    }
    
    //...
    
    /*
     * Bunda authentication GEREKİYOR. 
     * En azından, burayı çağırmadan önce, JumblrClient'ta this.requestBuilder.setConsumer(consumerKey, consumerSecret);  
     * ve this.requestBuilder.setToken(token, tokenSecret); demiştik.
     */
    public User user() {
        return requestBuilder.get("/user/info", null).getUser();
    }
    
    //her bir endpoint call'da önce authentication yapılır.
    public List<Post> blogPosts(String blogName, Map<String, ?> options) {
        if (options == null) {
            options = Collections.emptyMap();
        }
        Map<String, Object> soptions = JumblrClient.safeOptionMap(options);
        soptions.put("api_key", apiKey);

        String path = "/posts";
        if (soptions.containsKey("type")) {
            path += "/" + soptions.get("type").toString();
            soptions.remove("type");
        }
        return requestBuilder.get(JumblrClient.blogPath(blogName, path), soptions).getPosts();
    }
    
    public List<Post> blogDraftPosts(String blogName, Map<String, ?> options) {
        return requestBuilder.get(JumblrClient.blogPath(blogName, "/posts/draft"), options).getPosts();
    }

}

public class RequestBuilder {

    private org.scribe.oauth.Token token;
    private org.scribe.oauth.OAuthService service;
    private String hostname = "api.tumblr.com";
    private String xauthEndpoint = "https://www.tumblr.com/oauth/access_token";
    private String version = "0.0.13";
    private final JumblrClient client;

    public RequestBuilder(JumblrClient client) {
        this.client = client;
    }
    
    //Burada Scribe'ın içerisinde olan TumblrApi class'ında tumblr Oauth bilgileri var. BU bilgilere consumerKey ve consumerSecret'ımızı ekliyoruz.
    public void setConsumer(String consumerKey, String consumerSecret) {
        service = new ServiceBuilder().
        provider(TumblrApi.class).
        apiKey(consumerKey).apiSecret(consumerSecret).
        build();
    }
    
    //sign() metodunda kullanılacak.
    public void setToken(String token, String tokenSecret) {
        this.token = new Token(token, tokenSecret);
    }
    
    public ResponseWrapper get(String path, Map<String, ?> map) {
        OAuthRequest request = this.constructGet(path, map);
        sign(request);
        return clear(request.send());
    }
    
    public OAuthRequest constructGet(String path, Map<String, ?> queryParams) {
        String url = "https://" + hostname + "/v2" + path;
        OAuthRequest request = new OAuthRequest(Verb.GET, url);
        if (queryParams != null) {
            for (Map.Entry<String, ?> entry : queryParams.entrySet()) {
                request.addQuerystringParameter(entry.getKey(), entry.getValue().toString());
            }
        }
        request.addHeader("User-Agent", "jumblr/" + this.version);

        return request;
    }
    
    //elimizde token var, bu yüzden user() isteğinde de sign oluyoruz. Path'te vermesek de bizim olduğumuzu buradan biliyor.
    private void sign(OAuthRequest request) {
        if (token != null) {
            service.signRequest(token, request);
        }
    }
}

public class ResponseWrapper {
    
    public User getUser() {
        return get("user", User.class);
    }
    
    public List<Post> getPosts() {
        Gson gson = gsonParser();
        JsonObject object = (JsonObject) response;
        List<Post> l = gson.fromJson(object.get("posts"), new TypeToken<List<Post>>() {}.getType());
        for (Post e : l) { e.setClient(client); }
        return l;
    }
    
    ResponseWrapper clear(Response response) {
        if (response.getCode() == 200 || response.getCode() == 201) {
            String json = response.getBody();
            try {
                Gson gson = new GsonBuilder().
                        registerTypeAdapter(JsonElement.class, new JsonElementDeserializer()).
                        create();
                ResponseWrapper wrapper = gson.fromJson(json, ResponseWrapper.class);
                if (wrapper == null) {
                    throw new JumblrException(response);
                }
                wrapper.setClient(client);
                return wrapper;
            } catch (JsonSyntaxException ex) {
                throw new JumblrException(response);
            }
        } else {
            throw new JumblrException(response);
        }
    }
    
    private <T extends Resource> T get(String field, Class<T> k) {
        Gson gson = gsonParser();
        JsonObject object = (JsonObject) response;
        T e = gson.fromJson(object.get(field).toString(), k);
        e.setClient(client);
        return e;
    }
}


public class Blog {
    public List<Post> draftPosts() {
        return this.draftPosts(null);
    }
    
    public List<Post> draftPosts(Map<String, ?> options) {
        return client.blogDraftPosts(name, options);
    }
}

public class User {
    public List<Blog> getBlogs() {
        if (blogs != null) {
            for (Blog blog : blogs) {
                blog.setClient(client);
            }
        }
        return this.blogs;
    }
}
    
```

Sonuç olarak Jumblr ne kadar OAuth işlemleri için Scribe'ı kullanıyor. Tumblr API'si OAuth 1.0a, yani OAuth2.0 değil.

Scribe'ın OAuth implementasyonu şu şekilde:

önce call edilişi:
```java
service = new ServiceBuilder().
        provider(TumblrApi.class).
        apiKey(consumerKey).apiSecret(consumerSecret).
        build();
```

implementation:
```java
package org.scribe.builder;

import java.io.OutputStream;
import org.scribe.builder.api.Api;
import org.scribe.exceptions.OAuthException;
import org.scribe.model.OAuthConfig;
import org.scribe.model.SignatureType;
import org.scribe.oauth.OAuthService;
import org.scribe.utils.Preconditions;

public class ServiceBuilder {
    private String apiKey;
    private String apiSecret;
    private String callback = "oob";
    private Api api;
    private String scope;
    private SignatureType signatureType;
    private OutputStream debugStream;

    public ServiceBuilder() {
        this.signatureType = SignatureType.Header;
        this.debugStream = null;
    }

    public ServiceBuilder provider(Class<? extends Api> apiClass) {
        this.api = this.createApi(apiClass);
        return this;
    }

    private Api createApi(Class<? extends Api> apiClass) {
        Preconditions.checkNotNull(apiClass, "Api class cannot be null");

        try {
            Api api = (Api)apiClass.newInstance();
            return api;
        } catch (Exception var4) {
            throw new OAuthException("Error while creating the Api object", var4);
        }
    }

    public ServiceBuilder provider(Api api) {
        Preconditions.checkNotNull(api, "Api cannot be null");
        this.api = api;
        return this;
    }

    public ServiceBuilder callback(String callback) {
        Preconditions.checkNotNull(callback, "Callback can't be null");
        this.callback = callback;
        return this;
    }

    public ServiceBuilder apiKey(String apiKey) {
        Preconditions.checkEmptyString(apiKey, "Invalid Api key");
        this.apiKey = apiKey;
        return this;
    }

    public ServiceBuilder apiSecret(String apiSecret) {
        Preconditions.checkEmptyString(apiSecret, "Invalid Api secret");
        this.apiSecret = apiSecret;
        return this;
    }

    public ServiceBuilder scope(String scope) {
        Preconditions.checkEmptyString(scope, "Invalid OAuth scope");
        this.scope = scope;
        return this;
    }

    public ServiceBuilder signatureType(SignatureType type) {
        Preconditions.checkNotNull(type, "Signature type can't be null");
        this.signatureType = type;
        return this;
    }

    public ServiceBuilder debugStream(OutputStream stream) {
        Preconditions.checkNotNull(stream, "debug stream can't be null");
        this.debugStream = stream;
        return this;
    }

    public ServiceBuilder debug() {
        this.debugStream(System.out);
        return this;
    }

    public OAuthService build() {
        Preconditions.checkNotNull(this.api, "You must specify a valid api through the provider() method");
        Preconditions.checkEmptyString(this.apiKey, "You must provide an api key");
        Preconditions.checkEmptyString(this.apiSecret, "You must provide an api secret");
        return this.api.createService(new OAuthConfig(this.apiKey, this.apiSecret, this.callback, this.signatureType, this.scope, this.debugStream));
    }
}

package org.scribe.utils;

import java.util.regex.Pattern;

public class Preconditions {
    private static final String DEFAULT_MESSAGE = "Received an invalid parameter";
    private static final Pattern URL_PATTERN = Pattern.compile("^[a-zA-Z][a-zA-Z0-9+.-]*://\\S+");

    public Preconditions() {
    }

    public static void checkNotNull(Object object, String errorMsg) {
        check(object != null, errorMsg);
    }

    public static void checkEmptyString(String string, String errorMsg) {
        check(string != null && !string.trim().equals(""), errorMsg);
    }

    public static void checkValidUrl(String url, String errorMsg) {
        checkEmptyString(url, errorMsg);
        check(isUrl(url), errorMsg);
    }

    public static void checkValidOAuthCallback(String url, String errorMsg) {
        checkEmptyString(url, errorMsg);
        if (url.toLowerCase().compareToIgnoreCase("oob") != 0) {
            check(isUrl(url), errorMsg);
        }

    }

    private static boolean isUrl(String url) {
        return URL_PATTERN.matcher(url).matches();
    }

    private static void check(boolean requirements, String error) {
        String message = error != null && error.trim().length() > 0 ? error : "Received an invalid parameter";
        if (!requirements) {
            throw new IllegalArgumentException(message);
        }
    }
}

package org.scribe.model;

import java.io.OutputStream;

public class OAuthConfig {
    private final String apiKey;
    private final String apiSecret;
    private final String callback;
    private final SignatureType signatureType;
    private final String scope;
    private final OutputStream debugStream;

    public OAuthConfig(String key, String secret) {
        this(key, secret, (String)null, (SignatureType)null, (String)null, (OutputStream)null);
    }

    public OAuthConfig(String key, String secret, String callback, SignatureType type, String scope, OutputStream stream) {
        this.apiKey = key;
        this.apiSecret = secret;
        this.callback = callback;
        this.signatureType = type;
        this.scope = scope;
        this.debugStream = stream;
    }

    public String getApiKey() {
        return this.apiKey;
    }

    public String getApiSecret() {
        return this.apiSecret;
    }

    public String getCallback() {
        return this.callback;
    }

    public SignatureType getSignatureType() {
        return this.signatureType;
    }

    public String getScope() {
        return this.scope;
    }

    public boolean hasScope() {
        return this.scope != null;
    }

    public void log(String message) {
        if (this.debugStream != null) {
            message = message + "\n";

            try {
                this.debugStream.write(message.getBytes("UTF8"));
            } catch (Exception var3) {
                throw new RuntimeException("there were problems while writting to the debug stream", var3);
            }
        }

    }
}

package org.scribe.builder.api;

import org.scribe.model.OAuthConfig;
import org.scribe.oauth.OAuthService;

public interface Api {
    OAuthService createService(OAuthConfig var1);
}

package org.scribe.builder.api;

import org.scribe.model.Token;

public class TumblrApi extends DefaultApi10a {
    private static final String AUTHORIZE_URL = "https://www.tumblr.com/oauth/authorize?oauth_token=%s";
    private static final String REQUEST_TOKEN_RESOURCE = "http://www.tumblr.com/oauth/request_token";
    private static final String ACCESS_TOKEN_RESOURCE = "http://www.tumblr.com/oauth/access_token";

    public TumblrApi() {
    }

    public String getAccessTokenEndpoint() {
        return "http://www.tumblr.com/oauth/access_token";
    }

    public String getRequestTokenEndpoint() {
        return "http://www.tumblr.com/oauth/request_token";
    }

    public String getAuthorizationUrl(Token requestToken) {
        return String.format("https://www.tumblr.com/oauth/authorize?oauth_token=%s", requestToken.getToken());
    }
}

package org.scribe.builder.api;

import org.scribe.extractors.AccessTokenExtractor;
import org.scribe.extractors.BaseStringExtractor;
import org.scribe.extractors.BaseStringExtractorImpl;
import org.scribe.extractors.HeaderExtractor;
import org.scribe.extractors.HeaderExtractorImpl;
import org.scribe.extractors.RequestTokenExtractor;
import org.scribe.extractors.TokenExtractorImpl;
import org.scribe.model.OAuthConfig;
import org.scribe.model.Token;
import org.scribe.model.Verb;
import org.scribe.oauth.OAuth10aServiceImpl;
import org.scribe.oauth.OAuthService;
import org.scribe.services.HMACSha1SignatureService;
import org.scribe.services.SignatureService;
import org.scribe.services.TimestampService;
import org.scribe.services.TimestampServiceImpl;

public abstract class DefaultApi10a implements Api {
    public DefaultApi10a() {
    }

    public AccessTokenExtractor getAccessTokenExtractor() {
        return new TokenExtractorImpl();
    }

    public BaseStringExtractor getBaseStringExtractor() {
        return new BaseStringExtractorImpl();
    }

    public HeaderExtractor getHeaderExtractor() {
        return new HeaderExtractorImpl();
    }

    public RequestTokenExtractor getRequestTokenExtractor() {
        return new TokenExtractorImpl();
    }

    public SignatureService getSignatureService() {
        return new HMACSha1SignatureService();
    }

    public TimestampService getTimestampService() {
        return new TimestampServiceImpl();
    }

    public Verb getAccessTokenVerb() {
        return Verb.POST;
    }

    public Verb getRequestTokenVerb() {
        return Verb.POST;
    }

    public abstract String getRequestTokenEndpoint();

    public abstract String getAccessTokenEndpoint();

    public abstract String getAuthorizationUrl(Token var1);

    public OAuthService createService(OAuthConfig config) {
        return new OAuth10aServiceImpl(this, config);
    }
}


package org.scribe.oauth;

import java.util.Iterator;
import java.util.Map.Entry;
import java.util.concurrent.TimeUnit;
import org.scribe.builder.api.DefaultApi10a;
import org.scribe.model.OAuthConfig;
import org.scribe.model.OAuthConstants;
import org.scribe.model.OAuthRequest;
import org.scribe.model.Request;
import org.scribe.model.RequestTuner;
import org.scribe.model.Response;
import org.scribe.model.Token;
import org.scribe.model.Verifier;
import org.scribe.utils.MapUtils;

public class OAuth10aServiceImpl implements OAuthService {
    private static final String VERSION = "1.0";
    private OAuthConfig config;
    private DefaultApi10a api;

    public OAuth10aServiceImpl(DefaultApi10a api, OAuthConfig config) {
        this.api = api;
        this.config = config;
    }

    public Token getRequestToken(int timeout, TimeUnit unit) {
        return this.getRequestToken(new OAuth10aServiceImpl.TimeoutTuner(timeout, unit));
    }

    public Token getRequestToken() {
        return this.getRequestToken(2, TimeUnit.SECONDS);
    }

    public Token getRequestToken(RequestTuner tuner) {
        this.config.log("obtaining request token from " + this.api.getRequestTokenEndpoint());
        OAuthRequest request = new OAuthRequest(this.api.getRequestTokenVerb(), this.api.getRequestTokenEndpoint());
        this.config.log("setting oauth_callback to " + this.config.getCallback());
        request.addOAuthParameter("oauth_callback", this.config.getCallback());
        this.addOAuthParams(request, OAuthConstants.EMPTY_TOKEN);
        this.appendSignature(request);
        this.config.log("sending request...");
        Response response = request.send(tuner);
        String body = response.getBody();
        this.config.log("response status code: " + response.getCode());
        this.config.log("response body: " + body);
        return this.api.getRequestTokenExtractor().extract(body);
    }

    private void addOAuthParams(OAuthRequest request, Token token) {
        request.addOAuthParameter("oauth_timestamp", this.api.getTimestampService().getTimestampInSeconds());
        request.addOAuthParameter("oauth_nonce", this.api.getTimestampService().getNonce());
        request.addOAuthParameter("oauth_consumer_key", this.config.getApiKey());
        request.addOAuthParameter("oauth_signature_method", this.api.getSignatureService().getSignatureMethod());
        request.addOAuthParameter("oauth_version", this.getVersion());
        if (this.config.hasScope()) {
            request.addOAuthParameter("scope", this.config.getScope());
        }

        request.addOAuthParameter("oauth_signature", this.getSignature(request, token));
        this.config.log("appended additional OAuth parameters: " + MapUtils.toString(request.getOauthParameters()));
    }

    public Token getAccessToken(Token requestToken, Verifier verifier, int timeout, TimeUnit unit) {
        return this.getAccessToken(requestToken, verifier, new OAuth10aServiceImpl.TimeoutTuner(timeout, unit));
    }

    public Token getAccessToken(Token requestToken, Verifier verifier) {
        return this.getAccessToken(requestToken, verifier, 2, TimeUnit.SECONDS);
    }

    public Token getAccessToken(Token requestToken, Verifier verifier, RequestTuner tuner) {
        this.config.log("obtaining access token from " + this.api.getAccessTokenEndpoint());
        OAuthRequest request = new OAuthRequest(this.api.getAccessTokenVerb(), this.api.getAccessTokenEndpoint());
        request.addOAuthParameter("oauth_token", requestToken.getToken());
        request.addOAuthParameter("oauth_verifier", verifier.getValue());
        this.config.log("setting token to: " + requestToken + " and verifier to: " + verifier);
        this.addOAuthParams(request, requestToken);
        this.appendSignature(request);
        Response response = request.send(tuner);
        return this.api.getAccessTokenExtractor().extract(response.getBody());
    }

    public void signRequest(Token token, OAuthRequest request) {
        this.config.log("signing request: " + request.getCompleteUrl());
        if (!token.isEmpty()) {
            request.addOAuthParameter("oauth_token", token.getToken());
        }

        this.config.log("setting token to: " + token);
        this.addOAuthParams(request, token);
        this.appendSignature(request);
    }

    public String getVersion() {
        return "1.0";
    }

    public String getAuthorizationUrl(Token requestToken) {
        return this.api.getAuthorizationUrl(requestToken);
    }

    private String getSignature(OAuthRequest request, Token token) {
        this.config.log("generating signature...");
        String baseString = this.api.getBaseStringExtractor().extract(request);
        String signature = this.api.getSignatureService().getSignature(baseString, this.config.getApiSecret(), token.getSecret());
        this.config.log("base string is: " + baseString);
        this.config.log("signature is: " + signature);
        return signature;
    }

    private void appendSignature(OAuthRequest request) {
        switch(this.config.getSignatureType()) {
        case Header:
            this.config.log("using Http Header signature");
            String oauthHeader = this.api.getHeaderExtractor().extract(request);
            request.addHeader("Authorization", oauthHeader);
            break;
        case QueryString:
            this.config.log("using Querystring signature");
            Iterator i$ = request.getOauthParameters().entrySet().iterator();

            while(i$.hasNext()) {
                Entry<String, String> entry = (Entry)i$.next();
                request.addQuerystringParameter((String)entry.getKey(), (String)entry.getValue());
            }
        }

    }

    private static class TimeoutTuner extends RequestTuner {
        private final int duration;
        private final TimeUnit unit;

        public TimeoutTuner(int duration, TimeUnit unit) {
            this.duration = duration;
            this.unit = unit;
        }

        public void tune(Request request) {
            request.setReadTimeout(this.duration, this.unit);
        }
    }
}

package org.scribe.oauth;

import org.scribe.builder.api.DefaultApi20;
import org.scribe.model.OAuthConfig;
import org.scribe.model.OAuthRequest;
import org.scribe.model.Response;
import org.scribe.model.Token;
import org.scribe.model.Verifier;

public class OAuth20ServiceImpl implements OAuthService {
    private static final String VERSION = "2.0";
    private final DefaultApi20 api;
    private final OAuthConfig config;

    public OAuth20ServiceImpl(DefaultApi20 api, OAuthConfig config) {
        this.api = api;
        this.config = config;
    }

    public Token getAccessToken(Token requestToken, Verifier verifier) {
        OAuthRequest request = new OAuthRequest(this.api.getAccessTokenVerb(), this.api.getAccessTokenEndpoint());
        request.addQuerystringParameter("client_id", this.config.getApiKey());
        request.addQuerystringParameter("client_secret", this.config.getApiSecret());
        request.addQuerystringParameter("code", verifier.getValue());
        request.addQuerystringParameter("redirect_uri", this.config.getCallback());
        if (this.config.hasScope()) {
            request.addQuerystringParameter("scope", this.config.getScope());
        }

        Response response = request.send();
        return this.api.getAccessTokenExtractor().extract(response.getBody());
    }

    public Token getRequestToken() {
        throw new UnsupportedOperationException("Unsupported operation, please use 'getAuthorizationUrl' and redirect your users there");
    }

    public String getVersion() {
        return "2.0";
    }

    public void signRequest(Token accessToken, OAuthRequest request) {
        request.addQuerystringParameter("access_token", accessToken.getToken());
    }

    public String getAuthorizationUrl(Token requestToken) {
        return this.api.getAuthorizationUrl(this.config);
    }
}
``` 

Burada http isteklerinin gönderilmesi ve parse edilmesi için de Scribe içinde güzel bir örnek var.
```java
package org.scribe.model;

import java.io.IOException;
import java.io.UnsupportedEncodingException;
import java.net.HttpURLConnection;
import java.net.MalformedURLException;
import java.net.URL;
import java.nio.charset.Charset;
import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;
import java.util.concurrent.TimeUnit;
import org.scribe.exceptions.OAuthConnectionException;
import org.scribe.exceptions.OAuthException;

public class Request {
    private static final String CONTENT_LENGTH = "Content-Length";
    private static final String CONTENT_TYPE = "Content-Type";
    private static RequestTuner NOOP = new RequestTuner() {
        public void tune(Request _) {
        }
    };
    public static final String DEFAULT_CONTENT_TYPE = "application/x-www-form-urlencoded";
    private String url;
    private Verb verb;
    private ParameterList querystringParams;
    private ParameterList bodyParams;
    private Map<String, String> headers;
    private String payload = null;
    private HttpURLConnection connection;
    private String charset;
    private byte[] bytePayload = null;
    private boolean connectionKeepAlive = false;
    private Long connectTimeout = null;
    private Long readTimeout = null;

    public Request(Verb verb, String url) {
        this.verb = verb;
        this.url = url;
        this.querystringParams = new ParameterList();
        this.bodyParams = new ParameterList();
        this.headers = new HashMap();
    }

    public Response send(RequestTuner tuner) {
        try {
            this.createConnection();
            return this.doSend(tuner);
        } catch (Exception var3) {
            throw new OAuthConnectionException(var3);
        }
    }

    public Response send() {
        return this.send(NOOP);
    }

    private void createConnection() throws IOException {
        String completeUrl = this.getCompleteUrl();
        if (this.connection == null) {
            System.setProperty("http.keepAlive", this.connectionKeepAlive ? "true" : "false");
            this.connection = (HttpURLConnection)(new URL(completeUrl)).openConnection();
        }

    }

    public String getCompleteUrl() {
        return this.querystringParams.appendTo(this.url);
    }

    Response doSend(RequestTuner tuner) throws IOException {
        this.connection.setRequestMethod(this.verb.name());
        if (this.connectTimeout != null) {
            this.connection.setConnectTimeout(this.connectTimeout.intValue());
        }

        if (this.readTimeout != null) {
            this.connection.setReadTimeout(this.readTimeout.intValue());
        }

        this.addHeaders(this.connection);
        if (this.verb.equals(Verb.PUT) || this.verb.equals(Verb.POST)) {
            this.addBody(this.connection, this.getByteBodyContents());
        }

        tuner.tune(this);
        return new Response(this.connection);
    }

    void addHeaders(HttpURLConnection conn) {
        Iterator i$ = this.headers.keySet().iterator();

        while(i$.hasNext()) {
            String key = (String)i$.next();
            conn.setRequestProperty(key, (String)this.headers.get(key));
        }

    }

    void addBody(HttpURLConnection conn, byte[] content) throws IOException {
        conn.setRequestProperty("Content-Length", String.valueOf(content.length));
        if (conn.getRequestProperty("Content-Type") == null) {
            conn.setRequestProperty("Content-Type", "application/x-www-form-urlencoded");
        }

        conn.setDoOutput(true);
        conn.getOutputStream().write(content);
    }

    public void addHeader(String key, String value) {
        this.headers.put(key, value);
    }

    public void addBodyParameter(String key, String value) {
        this.bodyParams.add(key, value);
    }

    public void addQuerystringParameter(String key, String value) {
        this.querystringParams.add(key, value);
    }

    public void addPayload(String payload) {
        this.payload = payload;
    }

    public void addPayload(byte[] payload) {
        this.bytePayload = payload;
    }

    public ParameterList getQueryStringParams() {
        try {
            ParameterList result = new ParameterList();
            String queryString = (new URL(this.url)).getQuery();
            result.addQuerystring(queryString);
            result.addAll(this.querystringParams);
            return result;
        } catch (MalformedURLException var3) {
            throw new OAuthException("Malformed URL", var3);
        }
    }

    public ParameterList getBodyParams() {
        return this.bodyParams;
    }

    public String getUrl() {
        return this.url;
    }

    public String getSanitizedUrl() {
        return this.url.replaceAll("\\?.*", "").replace("\\:\\d{4}", "");
    }

    public String getBodyContents() {
        try {
            return new String(this.getByteBodyContents(), this.getCharset());
        } catch (UnsupportedEncodingException var2) {
            throw new OAuthException("Unsupported Charset: " + this.charset, var2);
        }
    }

    byte[] getByteBodyContents() {
        if (this.bytePayload != null) {
            return this.bytePayload;
        } else {
            String body = this.payload != null ? this.payload : this.bodyParams.asFormUrlEncodedString();

            try {
                return body.getBytes(this.getCharset());
            } catch (UnsupportedEncodingException var3) {
                throw new OAuthException("Unsupported Charset: " + this.getCharset(), var3);
            }
        }
    }

    public Verb getVerb() {
        return this.verb;
    }

    public Map<String, String> getHeaders() {
        return this.headers;
    }

    public String getCharset() {
        return this.charset == null ? Charset.defaultCharset().name() : this.charset;
    }

    public void setConnectTimeout(int duration, TimeUnit unit) {
        this.connectTimeout = unit.toMillis((long)duration);
    }

    public void setReadTimeout(int duration, TimeUnit unit) {
        this.readTimeout = unit.toMillis((long)duration);
    }

    public void setCharset(String charsetName) {
        this.charset = charsetName;
    }

    public void setConnectionKeepAlive(boolean connectionKeepAlive) {
        this.connectionKeepAlive = connectionKeepAlive;
    }

    void setConnection(HttpURLConnection connection) {
        this.connection = connection;
    }

    public String toString() {
        return String.format("@Request(%s %s)", this.getVerb(), this.getUrl());
    }
}

package org.scribe.model;

import java.io.IOException;
import java.io.InputStream;
import java.net.HttpURLConnection;
import java.net.UnknownHostException;
import java.util.HashMap;
import java.util.Iterator;
import java.util.List;
import java.util.Map;
import org.scribe.exceptions.OAuthException;
import org.scribe.utils.StreamUtils;

public class Response {
    private static final String EMPTY = "";
    private int code;
    private String body;
    private InputStream stream;
    private Map<String, String> headers;

    Response(HttpURLConnection connection) throws IOException {
        try {
            connection.connect();
            this.code = connection.getResponseCode();
            this.headers = this.parseHeaders(connection);
            this.stream = this.isSuccessful() ? connection.getInputStream() : connection.getErrorStream();
        } catch (UnknownHostException var3) {
            throw new OAuthException("The IP address of a host could not be determined.", var3);
        }
    }

    private String parseBodyContents() {
        this.body = StreamUtils.getStreamContents(this.getStream());
        return this.body;
    }

    private Map<String, String> parseHeaders(HttpURLConnection conn) {
        Map<String, String> headers = new HashMap();
        Iterator i$ = conn.getHeaderFields().keySet().iterator();

        while(i$.hasNext()) {
            String key = (String)i$.next();
            headers.put(key, ((List)conn.getHeaderFields().get(key)).get(0));
        }

        return headers;
    }

    public boolean isSuccessful() {
        return this.getCode() >= 200 && this.getCode() < 400;
    }

    public String getBody() {
        return this.body != null ? this.body : this.parseBodyContents();
    }

    public InputStream getStream() {
        return this.stream;
    }

    public int getCode() {
        return this.code;
    }

    public Map<String, String> getHeaders() {
        return this.headers;
    }

    public String getHeader(String name) {
        return (String)this.headers.get(name);
    }
}

package org.scribe.model;

import java.util.HashMap;
import java.util.Map;

public class OAuthRequest extends Request {
    private static final String OAUTH_PREFIX = "oauth_";
    private Map<String, String> oauthParameters = new HashMap();

    public OAuthRequest(Verb verb, String url) {
        super(verb, url);
    }

    public void addOAuthParameter(String key, String value) {
        this.oauthParameters.put(this.checkKey(key), value);
    }

    private String checkKey(String key) {
        if (!key.startsWith("oauth_") && !key.equals("scope")) {
            throw new IllegalArgumentException(String.format("OAuth parameters must either be '%s' or start with '%s'", "scope", "oauth_"));
        } else {
            return key;
        }
    }

    public Map<String, String> getOauthParameters() {
        return this.oauthParameters;
    }

    public String toString() {
        return String.format("@OAuthRequest(%s, %s)", this.getVerb(), this.getUrl());
    }
}

package org.scribe.extractors;

import java.util.regex.Matcher;
import java.util.regex.Pattern;
import org.scribe.exceptions.OAuthException;
import org.scribe.model.Token;
import org.scribe.utils.OAuthEncoder;
import org.scribe.utils.Preconditions;

public class TokenExtractorImpl implements RequestTokenExtractor, AccessTokenExtractor {
    private static final Pattern TOKEN_REGEX = Pattern.compile("oauth_token=([^&]+)");
    private static final Pattern SECRET_REGEX = Pattern.compile("oauth_token_secret=([^&]*)");

    public TokenExtractorImpl() {
    }

    public Token extract(String response) {
        Preconditions.checkEmptyString(response, "Response body is incorrect. Can't extract a token from an empty string");
        String token = this.extract(response, TOKEN_REGEX);
        String secret = this.extract(response, SECRET_REGEX);
        return new Token(token, secret, response);
    }

    private String extract(String response, Pattern p) {
        Matcher matcher = p.matcher(response);
        if (matcher.find() && matcher.groupCount() >= 1) {
            return OAuthEncoder.decode(matcher.group(1));
        } else {
            throw new OAuthException("Response body is incorrect. Can't extract token and secret from this: '" + response + "'", (Exception)null);
        }
    }
}

//usage
public OAuthRequest constructGet(String path, Map<String, ?> queryParams) {
        String url = "https://" + hostname + "/v2" + path;
        OAuthRequest request = new OAuthRequest(Verb.GET, url);
        if (queryParams != null) {
            for (Map.Entry<String, ?> entry : queryParams.entrySet()) {
                request.addQuerystringParameter(entry.getKey(), entry.getValue().toString());
            }
        }
        request.addHeader("User-Agent", "jumblr/" + this.version);

        return request;
    }
    
    private OAuthRequest constructPost(String path, Map<String, ?> bodyMap) {
        String url = "https://" + hostname + "/v2" + path;
        OAuthRequest request = new OAuthRequest(Verb.POST, url);

        for (Map.Entry<String, ?> entry : bodyMap.entrySet()) {
        	String key = entry.getKey();
        	Object value = entry.getValue();
        	if (value == null || value instanceof File) { continue; }
            request.addBodyParameter(key,value.toString());
        }
        request.addHeader("User-Agent", "jumblr/" + this.version);

        return request;
    }
    
    public ResponseWrapper get(String path, Map<String, ?> map) {
        OAuthRequest request = this.constructGet(path, map);
        sign(request);
        return clear(request.send());
    }
    
    private void sign(OAuthRequest request) {
        if (token != null) {
            service.signRequest(token, request);
        }
    }
    
    public void signRequest(Token token, OAuthRequest request) {
        this.config.log("signing request: " + request.getCompleteUrl());
        if (!token.isEmpty()) {
            request.addOAuthParameter("oauth_token", token.getToken());
        }

        this.config.log("setting token to: " + token);
        this.addOAuthParams(request, token);
        this.appendSignature(request);
    }
```

ayrıca signature konusunda:

```java
package org.scribe.builder.api;

import org.scribe.extractors.AccessTokenExtractor;
import org.scribe.extractors.BaseStringExtractor;
import org.scribe.extractors.BaseStringExtractorImpl;
import org.scribe.extractors.HeaderExtractor;
import org.scribe.extractors.HeaderExtractorImpl;
import org.scribe.extractors.RequestTokenExtractor;
import org.scribe.extractors.TokenExtractorImpl;
import org.scribe.model.OAuthConfig;
import org.scribe.model.Token;
import org.scribe.model.Verb;
import org.scribe.oauth.OAuth10aServiceImpl;
import org.scribe.oauth.OAuthService;
import org.scribe.services.HMACSha1SignatureService;
import org.scribe.services.SignatureService;
import org.scribe.services.TimestampService;
import org.scribe.services.TimestampServiceImpl;

public abstract class DefaultApi10a implements Api {
    public DefaultApi10a() {
    }

    public AccessTokenExtractor getAccessTokenExtractor() {
        return new TokenExtractorImpl();
    }

    public BaseStringExtractor getBaseStringExtractor() {
        return new BaseStringExtractorImpl();
    }

    public HeaderExtractor getHeaderExtractor() {
        return new HeaderExtractorImpl();
    }

    public RequestTokenExtractor getRequestTokenExtractor() {
        return new TokenExtractorImpl();
    }
    
    //signature
    public SignatureService getSignatureService() {
        return new HMACSha1SignatureService();
    }

    public TimestampService getTimestampService() {
        return new TimestampServiceImpl();
    }

    public Verb getAccessTokenVerb() {
        return Verb.POST;
    }

    public Verb getRequestTokenVerb() {
        return Verb.POST;
    }

    public abstract String getRequestTokenEndpoint();

    public abstract String getAccessTokenEndpoint();

    public abstract String getAuthorizationUrl(Token var1);

    public OAuthService createService(OAuthConfig config) {
        return new OAuth10aServiceImpl(this, config);
    }
}


package org.scribe.services;

import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import org.apache.commons.codec.binary.Base64;
import org.scribe.exceptions.OAuthSignatureException;
import org.scribe.utils.OAuthEncoder;
import org.scribe.utils.Preconditions;

public class HMACSha1SignatureService implements SignatureService {
    private static final String EMPTY_STRING = "";
    private static final String CARRIAGE_RETURN = "\r\n";
    private static final String UTF8 = "UTF-8";
    private static final String HMAC_SHA1 = "HmacSHA1";
    private static final String METHOD = "HMAC-SHA1";

    public HMACSha1SignatureService() {
    }

    public String getSignature(String baseString, String apiSecret, String tokenSecret) {
        try {
            Preconditions.checkEmptyString(baseString, "Base string cant be null or empty string");
            Preconditions.checkEmptyString(apiSecret, "Api secret cant be null or empty string");
            return this.doSign(baseString, OAuthEncoder.encode(apiSecret) + '&' + OAuthEncoder.encode(tokenSecret));
        } catch (Exception var5) {
            throw new OAuthSignatureException(baseString, var5);
        }
    }

    private String doSign(String toSign, String keyString) throws Exception {
        SecretKeySpec key = new SecretKeySpec(keyString.getBytes("UTF-8"), "HmacSHA1");
        Mac mac = Mac.getInstance("HmacSHA1");
        mac.init(key);
        byte[] bytes = mac.doFinal(toSign.getBytes("UTF-8"));
        return (new String(Base64.encodeBase64(bytes))).replace("\r\n", "");
    }

    public String getSignatureMethod() {
        return "HMAC-SHA1";
    }
}

private String getSignature(OAuthRequest request, Token token) {
        this.config.log("generating signature...");
        String baseString = this.api.getBaseStringExtractor().extract(request);
        String signature = this.api.getSignatureService().getSignature(baseString, this.config.getApiSecret(), token.getSecret());
        this.config.log("base string is: " + baseString);
        this.config.log("signature is: " + signature);
        return signature;
    }
    
private void addOAuthParams(OAuthRequest request, Token token) {
        request.addOAuthParameter("oauth_timestamp", this.api.getTimestampService().getTimestampInSeconds());
        request.addOAuthParameter("oauth_nonce", this.api.getTimestampService().getNonce());
        request.addOAuthParameter("oauth_consumer_key", this.config.getApiKey());
        request.addOAuthParameter("oauth_signature_method", this.api.getSignatureService().getSignatureMethod());
        request.addOAuthParameter("oauth_version", this.getVersion());
        if (this.config.hasScope()) {
            request.addOAuthParameter("scope", this.config.getScope());
        }

        request.addOAuthParameter("oauth_signature", this.getSignature(request, token));
        this.config.log("appended additional OAuth parameters: " + MapUtils.toString(request.getOauthParameters()));
    }
```

Bu kadar inceledikten sonra, sonuç olarak, API Console'dan aldığımız Access Token ve Secret'ımızı kullanacağız. 
Yani en başta yazdığım adım adım akışı uygulamıyoruz. 

```java
public void setConsumer(String consumerKey, String consumerSecret) {
    service = new ServiceBuilder().
    provider(TumblrApi.class).
    apiKey(consumerKey).apiSecret(consumerSecret).
    build();
}

public void setToken(String token, String tokenSecret) {
    this.token = new Token(token, tokenSecret);
}

public ResponseWrapper get(String path, Map<String, ?> map) {
    OAuthRequest request = this.constructGet(path, map);
    sign(request);
    return clear(request.send());
}

public OAuthRequest constructGet(String path, Map<String, ?> queryParams) {
    String url = "https://" + hostname + "/v2" + path;
    OAuthRequest request = new OAuthRequest(Verb.GET, url);
    if (queryParams != null) {
        for (Map.Entry<String, ?> entry : queryParams.entrySet()) {
            request.addQuerystringParameter(entry.getKey(), entry.getValue().toString());
        }
    }
    request.addHeader("User-Agent", "jumblr/" + this.version);

    return request;
}
    
private void sign(OAuthRequest request) {
    if (token != null) {
        service.signRequest(token, request);
    }
}

public void signRequest(Token token, OAuthRequest request) {
    this.config.log("signing request: " + request.getCompleteUrl());
    if (!token.isEmpty()) {
        
        //token yani access token'ı kullandık.
        request.addOAuthParameter("oauth_token", token.getToken());
    }

    this.config.log("setting token to: " + token);
    this.addOAuthParams(request, token);
    this.appendSignature(request);
}

private void addOAuthParams(OAuthRequest request, Token token) {
    request.addOAuthParameter("oauth_timestamp", this.api.getTimestampService().getTimestampInSeconds());
    request.addOAuthParameter("oauth_nonce", this.api.getTimestampService().getNonce());
    
    //request token yani consumer key yani api key'i kullandılk.
    request.addOAuthParameter("oauth_consumer_key", this.config.getApiKey());
    request.addOAuthParameter("oauth_signature_method", this.api.getSignatureService().getSignatureMethod());
    request.addOAuthParameter("oauth_version", this.getVersion());
    if (this.config.hasScope()) {
        request.addOAuthParameter("scope", this.config.getScope());
    }
    
    //ÖNEMLİ! Burada consumer secret ve token secret ile signature elde edeceğiz.
    request.addOAuthParameter("oauth_signature", this.getSignature(request, token));
    this.config.log("appended additional OAuth parameters: " + MapUtils.toString(request.getOauthParameters()));
}

private String getSignature(OAuthRequest request, Token token) {
    this.config.log("generating signature...");
    String baseString = this.api.getBaseStringExtractor().extract(request);
    String signature = this.api.getSignatureService().getSignature(baseString, this.config.getApiSecret(), token.getSecret());
    this.config.log("base string is: " + baseString);
    this.config.log("signature is: " + signature);
    return signature;
}

public String extract(OAuthRequest request) {
    this.checkPreconditions(request);
    String verb = OAuthEncoder.encode(request.getVerb().name());
    String url = OAuthEncoder.encode(request.getSanitizedUrl());
    String params = this.getSortedAndEncodedParams(request);
    return String.format("%s&%s&%s", verb, url, params);
}

private String getSortedAndEncodedParams(OAuthRequest request) {
    ParameterList params = new ParameterList();
    params.addAll(request.getQueryStringParams());
    params.addAll(request.getBodyParams());
    params.addAll(new ParameterList(request.getOauthParameters()));
    return params.sort().asOauthBaseString();
}

public HMACSha1SignatureService() {
}

public String getSignature(String baseString, String apiSecret, String tokenSecret) {
    try {
        Preconditions.checkEmptyString(baseString, "Base string cant be null or empty string");
        Preconditions.checkEmptyString(apiSecret, "Api secret cant be null or empty string");
        return this.doSign(baseString, OAuthEncoder.encode(apiSecret) + '&' + OAuthEncoder.encode(tokenSecret));
    } catch (Exception var5) {
        throw new OAuthSignatureException(baseString, var5);
    }
}

//api secret ve token secret ile signature oluşturuluyor.
private String doSign(String toSign, String keyString) throws Exception {
    SecretKeySpec key = new SecretKeySpec(keyString.getBytes("UTF-8"), "HmacSHA1");
    Mac mac = Mac.getInstance("HmacSHA1");
    mac.init(key);
    byte[] bytes = mac.doFinal(toSign.getBytes("UTF-8"));
    return (new String(Base64.encodeBase64(bytes))).replace("\r\n", "");
}

private void appendSignature(OAuthRequest request) {
    switch(this.config.getSignatureType()) {
    case Header:
        this.config.log("using Http Header signature");
        
        //header set ediliyor.
        String oauthHeader = this.api.getHeaderExtractor().extract(request);
        request.addHeader("Authorization", oauthHeader);
        break;
    case QueryString:
        this.config.log("using Querystring signature");
        Iterator i$ = request.getOauthParameters().entrySet().iterator();

        while(i$.hasNext()) {
            Entry<String, String> entry = (Entry)i$.next();
            request.addQuerystringParameter((String)entry.getKey(), (String)entry.getValue());
        }
    }

}   

//header'ın set edilmesi.
public class HeaderExtractorImpl implements HeaderExtractor {
    private static final String PARAM_SEPARATOR = ", ";
    private static final String PREAMBLE = "OAuth ";

    public HeaderExtractorImpl() {
    }

    public String extract(OAuthRequest request) {
        this.checkPreconditions(request);
        Map<String, String> parameters = request.getOauthParameters();
        StringBuffer header = new StringBuffer(parameters.size() * 20);
        header.append("OAuth ");

        String key;
        for(Iterator i$ = parameters.keySet().iterator(); i$.hasNext(); header.append(String.format("%s=\"%s\"", key, OAuthEncoder.encode((String)parameters.get(key))))) {
            key = (String)i$.next();
            if (header.length() > "OAuth ".length()) {
                header.append(", ");
            }
        }

        return header.toString();
    }

    private void checkPreconditions(OAuthRequest request) {
        Preconditions.checkNotNull(request, "Cannot extract a header from a null object");
        if (request.getOauthParameters() == null || request.getOauthParameters().size() <= 0) {
            throw new OAuthParametersMissingException(request);
        }
    }
}

public Response send() {
    return this.send(NOOP);
}

public Response send(RequestTuner tuner) {
    try {
        this.createConnection();
        return this.doSend(tuner);
    } catch (Exception var3) {
        throw new OAuthConnectionException(var3);
    }
}

private void createConnection() throws IOException {
    String completeUrl = this.getCompleteUrl();
    if (this.connection == null) {
        System.setProperty("http.keepAlive", this.connectionKeepAlive ? "true" : "false");
        this.connection = (HttpURLConnection)(new URL(completeUrl)).openConnection();
    }

}

Response doSend(RequestTuner tuner) throws IOException {
    this.connection.setRequestMethod(this.verb.name());
    if (this.connectTimeout != null) {
        this.connection.setConnectTimeout(this.connectTimeout.intValue());
    }

    if (this.readTimeout != null) {
        this.connection.setReadTimeout(this.readTimeout.intValue());
    }

    this.addHeaders(this.connection);
    if (this.verb.equals(Verb.PUT) || this.verb.equals(Verb.POST)) {
        this.addBody(this.connection, this.getByteBodyContents());
    }

    tuner.tune(this);
    return new Response(this.connection);
}

Response(HttpURLConnection connection) throws IOException {
    try {
        connection.connect();
        this.code = connection.getResponseCode();
        this.headers = this.parseHeaders(connection);
        this.stream = this.isSuccessful() ? connection.getInputStream() : connection.getErrorStream();
    } catch (UnknownHostException var3) {
        throw new OAuthException("The IP address of a host could not be determined.", var3);
    }
}
```

# Gün9:



# tools

## devops platform(ci/cd pipeline, private code repositories, project/agile management board)
1. [GitLab](https://about.gitlab.com/free-trial/)
2. [Azure Devops](https://azure.microsoft.com/tr-tr/services/devops/)

# TODOs
1. Private repo: İleriki aşamalarda private repo'ya geçilecek. `Github` unlimited free repo sunuyor. Settings'ten private'a döndürebiliyoruz. 
Ayrıca,
> private code repository hosting
[6 places to host your git repository](https://opensource.com/article/18/8/github-alternatives)
2. Licensing: how to add a licence file to github
> [What you need to know to choose an open source license.](https://gist.github.com/nicolasdao/a7adda51f2f185e8d2700e1573d8a633)
3. Branching strategy and issue tracking


# References/Further reading/readings/materials
1. [Make Your Own Rich Text Editor in Reactjs using draftjs](https://hackernoon.com/make-your-own-rich-text-editor-in-reactjs-using-draftjs-25a0b4fb05e5)
2. [page source undaki javascript library lerine bak.](https://ncase.me/trust/)
3. [implement a radio application in react - Build your own radio streaming app with Howler.js](https://medium.com/crowdbotics/build-your-own-radio-streaming-app-with-howler-js-637f929decc0)
4. implement a youtube playlist in react ya da [React Native YouTube Replica](https://medium.com/react-native-training/react-native-youtube-replica-f378200d91f0)
5. [Create a React Native app on any OS with no build config. ](https://github.com/react-community/create-react-native-app)
6. [Building an Autocomplete Component in React](https://alligator.io/react/react-autocomplete/)
7. [How To Write a Search Component with Suggestions in React](https://dev.to/sage911/how-to-write-a-search-component-with-suggestions-in-react-d20)
8. [Build an Autocomplete widget with React and Elastic Search](https://blog.bitsrc.io/how-to-build-an-autocomplete-widget-with-react-and-elastic-search-dd4f846f784)
9. [react pdf viewer](https://react-pdf.org/styling)
10. react drawing or sketching app
11. react diagramming library: [Project Storm React Diagrams - A simple diagramming library written in react](https://projectstorm.gitbooks.io/react-diagrams/)
12. [Install OpenJDK 8 on Ubuntu Trusty](https://www.geofis.org/en/install/install-on-linux/install-openjdk-8-on-ubuntu-trusty/)
```
sudo add-apt-repository ppa:openjdk-r/ppa
sudo apt-get update
sudo apt-get install openjdk-8-jdk
sudo update-java-alternatives --list
sudo update-alternatives --config java
sudo update-alternatives --config javac
```

# backlog
1. ubuntu 18.10 experienced an internal problem plymouth
