Hortonworks, setup eclipse.
Liberamente ispirato da questo: https://hadoopi.wordpress.com/2013/05/25/setup-maven-project-for-hadoop-in-5mn/

Preferenze-> Maven ~/.m2/settings.xml:
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0                      http://maven.apache.org/xsd/settings-1.0.0.xsd"> 
  <profiles>
    <profile>
      <repositories>
        <repository>
          <releases>
            <enabled>true</enabled>
            <updatePolicy>always</updatePolicy>
            <checksumPolicy>warn</checksumPolicy>
          </releases>
          <snapshots>
            <enabled>false</enabled>
            <updatePolicy>never</updatePolicy>
            <checksumPolicy>fail</checksumPolicy>
          </snapshots>
          <id>HDPReleases</id>
          <name>HDP Releases</name>
          <url>http://repo.hortonworks.com/content/repositories/releases/</url>
          <layout>default</layout>
        </repository>
      </repositories> 
    </profile>
  </profiles>
</settings>
(settings repo hortonworks: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.7/bk_user-guide/content/user-guide-setup-maven-repo.html)
Presa lista dipendenze da qui, cambiata versione con RELEASE, per avere quelle della sandbox: https://hadoopi.wordpress.com/2013/05/25/setup-maven-project-for-hadoop-in-5mn/
(attenzione aggiornare java a 1.7, e su macosx non servono i jdk.tool)
maven build (guarda qui: https://books.sonatype.com/m2eclipse-book/reference/running-sect-running-maven-builds.html)

Carica il jar sulla macchina virtuale:
cd ~/.m2/repository/me/dacol/hadoop/mapreduce/0.0.1-SNAPSHOT/
scp -P 2222 mapreduce-0.0.1-SNAPSHOT.jar root@127.0.0.1:/root

Carica tutto su HW hdp.
genera due file di esempio:
echo "cat sat mat" | hadoop fs -put - wordcount/input/1.txt
echo "dog lay mat" | hadoop fs -put - wordcount/input/2.txt

Lancia il job con:
hadoop jar mapreduce-0.0.1-SNAPSHOT.jar me.dacol.hadoop.mapreduce.WordCount wordcount/input/ wordcount/output

Vedi risultato con:
hadoop fs -cat wordcount/output/part-r-00000


