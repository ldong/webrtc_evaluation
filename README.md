# WebRTC Video Evaluation

Author: Lin Dong

Date: 2/28/2016

## Objective

Leverage WebRTC P2P technology with recording functionality.

## Note
Record WebRTC on Client side is booming, i.e. [muaz-khan/RecordRTC](github.com/muaz-khan/RecordRTC), [H0201030/record-rtc-together](github.com/H0201030/record-rtc-together). What about server side? If we records video feed in the backend, is it drawing back to the 90s? Of course not. Anyway, lets evaluate the current state of art softwares at the time of writing.

## List of Apps

1. [oovoo](http://developer.ooVoo.com):
    * [oovoo webrtc demo](https://developers.oovoo.com/webrtcdemo/1)
    * [oovoo webrtc sdk](https://github.com/oovoodev/WebRTC-SDK)

	*Feedbacks*:

	1. Not Free
	2. Lack of recording functionality

2. [Kurento](http://www.kurento.org/):
    * [Kurento Media Server](https://github.com/Kurento/kurento-media-server)
    * [Kurento/kurento-tutorial-js](https://github.com/Kurento/kurento-tutorial-js)
    * [Kurento/kurento-tutorial-node](https://github.com/Kurento/kurento-tutorial-node)

	*Feedbacks*:

	1. Free and open sourced
	2. Recording functionality

So, I decided to go and try Kurenton

## Install Kurenton on Ubuntu
1. `sudo apt-get install build-essential openjdk-7-jdk maven`
2. `echo "export JAVA_HOME=/usr/lib/jvm/java-1.7.0-openjdk-amd64"` > .bashrc
3. `curl -sL https://deb.nodesource.com/setup | sudo bash - ;sudo apt-get install -y nodejs; npm install -g bower`
4.  Install Kurento 6.0

    ```
    echo "deb http://ubuntu.kurento.org trusty kms6" | sudo tee /etc/apt/sources.list.d/kurento.list
    wget -O - http://ubuntu.kurento.org/kurento.gpg.key | sudo apt-key add -
    sudo apt-get update
    sudo apt-get install kurento-media-server-6.0
    ```

4. `sudo service kurento-media-server-6.0 start` and `sudo service kurento-media-server-6.0 stop`

5. Download repo

    ```
    git clone https://github.com/Kurento/kurento-tutorial-java.git
    cd kurento-tutorial-java/kurento-one2one-call-recording/
    git checkout 6.2.1
    mvn compile exec:java
    ```

6. Add the following to `pom.xml` file

    ```
    <properties>

        // existing
        <maven.compiler.source>1.7</maven.compiler.source>
        <maven.compiler.target>1.7</maven.compiler.target>
    </properties>
    ```

6. Run `mvn compile exec:java` to install bower and its necessary libraries

7. Update necessary libraries

    ```
    cd YOUR_PATH/kurento-tutorial-java/kurento-one2one-call-recording/src/main/resources/static/bower_components/adapter.js
    npm install
    grunt
    cp ./out/adapter.js .
    ```

8. `cd kurento-one2one-call-recording/src/main/resources/static`
9. `mkdir jss` and copy `kurento-utils.js` to jss directory
10. change `getUserMedia` to `navigator.getUserMedia` in `kurento-utils.js`
11. Replace `<script src="./js/kurento-utils.js"></script>` to `<script src="./jss/kurento-utils.js"></script>` in `index.html`
12. Run `mvn compile exec:java` again and open `https://localhost:8443/`, make sure `sudo service kurento-media-server-6.0 start` is running

PS: modified version of kurento-utils.js and adapter.js are attached in js_files directory for your convenience

## Reference
1. [oovoo alternative](http://alternativeto.net/software/oovoo/?license=opensource)
2. [Kurento Docs](http://doc-kurento.readthedocs.org/en/stable/tutorials/java/tutorial-helloworld.html)
3. [Tutorial: Video Call 1 to 1 with recording](http://doc-kurento.readthedocs.org/en/stable/tutorials/java/tutorial-one2one-adv.html)
