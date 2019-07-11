## Tumblr API'sini consume ederek tüm verilerimin alınması ve saklanması için tasklar ve notlar:

# Gün1:
Linux üzerinde daha hızlı olabilmek için önce Eclipse ve STS(Spring Tools Suite) üzerinden ilerlemek istedim.
Ancak vazgeçtim IDEA Community Edition ile devam ettim. Burada bazı Linux command larına ihtiyacım oldu.

1. $PATH ve $JAVA_HOME'u ayarlamak
2. Bunun için java(JRE'de) ve javac(JDK'da) executable'larının yerlerini bulmak
    - Bunun için [Where can I find the Java SDK in Linux?](https://stackoverflow.com/questions/5251323/where-can-i-find-the-java-sdk-in-linux)
    - sdfsdfsdf
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