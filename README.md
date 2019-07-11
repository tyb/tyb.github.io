## Tumblr API'sini consume ederek tüm verilerimin alınması ve saklanması için tasklar ve notlar:

# Gün1:
Linux üzerinde daha hızlı olabilmek için önce Eclipse ve STS(Spring Tools Suite) üzerinden ilerlemek istedim.
Ancak vazgeçtim IDEA Community Edition ile devam ettim. Burada bazı Linux command larına ihtiyacım oldu.

1. $PATH ve $JAVA_HOME'u ayarlamak
    - Benzer şekilde $LD_LIBRARY_PATH
2. Bunun için java(JRE'de) ve javac(JDK'da) executable'larının yerlerini bulmak
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

    > <div> You will need to add JAVA_HOME to your .bashrc file. Edit the: gedit ~/.bashrc Add the following lines: ## JAVA_HOME export JAVA_HOME="/usr/lib/jvm/java-9-openjdk-amd64" export PATH=$PATH:$JAVA_HOME/bin Add it to the /etc/environemnt file with: echo "JAVA_HOME=\"/usr/lib/jvm/java-9-openjdk-amd64\"" | sudo tee -a /etc/environment Close and open a new terminal. If all doesn't work then: Launch Intellij Press: ctrl+alt+shift+S The go to Platform Settings -> SDKs click to add the path for your java sdk enter image description here Now your IntelliJ should be able to see it.</div>


    asdsad

3. Git kurulumu ve remote/local workflow'u
    - /usr/bin/git de bulunuyor, IDE otomatik olarak buradan görüyor.
    - Remote'dan clone edip local'ime indirdiğim bir proje için ilgili folder'da .git folder'ı oluştu.

    ### içeriği

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

### References/Further reading/readings/materials