`sudo apt-get update`
`sudo apt-get upgrade`
`sudo apt-get install g++ libgnutls28-dev uuid-dev uuid-runtime cmake gnutls-dev gnutls-bin`
`git clone --recurse-submodules https://github.com/GothenburgBitFactory/taskserver.git`
`cmake -DCMAKE_BUILD_TYPE=release .`
`cd taskserver/`
> edit .bashrc and add this line: `export TASKDDATA=/var/taskd`
`source .bashrc`
`cd $HOME/taskserver/`
`cmake -DCMAKE_BUILD_TYPE=release .`
`make`
`sudo make install`
`sudo mkdir -p $TASKDDATA`
`taskd init`
`sudo chown YOUR_USER /var/taskd`
`taskd init`
`cd $HOME/taskserver/`
`cp -r pki/ /var/taskd/`
`cd /var/taskd/pki/`
> edit vars and change CN to your hostname
> edit /etc/hostname and make sure it points to the value you put in `vars`. The restart the machine fo hostname to be changed
`cd /var/taskd/pki/`
`./generate`
`./generate.client `
`cp *.pem $TASKDDATA`
`taskd config --force client.key $TASKDDATA/client.key.pem`
`taskd config --force server.cert $TASKDDATA/server.cert.pem`
`taskd config --force server.key $TASKDDATA/server.key.pem`
`taskd config --force server.crl $TASKDDATA/server.crl.pem`
`taskd config --force ca.cert $TASKDDATA/ca.cert.pem`
`cd $TASKDDATA/..`
`taskd config --force log /var/taskd/taskd.log`
`taskd config --force pid.file /var/taskd/taskd.pid`
`taskd config --force server 0.0.0.0:53589`
`sudo groupadd taskdGroup`
`sudo useradd -r -s /bin/false -g taskdGroup taskdUser`
`sudo chown taskdUser:taskdGroup /var/taskd/`
*edit ~/taskserver/scripts/systemd/taskd.service and change ExecStart/User/Group/WorkingDirectory
`cp ~/taskserver/scripts/systemd/taskd.service /etc/systemd/system/ `
`sudo cp ~/taskserver/scripts/systemd//taskd.service /etc/systemd/system/ `
`sudo  systemctl daemon-reload`
`sudo  systemctl start taskd.service`
`sudo chown taskdUser:taskdGroup /var/taskd/config`
`sudo chmod 644 /var/taskd/config `
`sudo chown taskdUser:taskdGroup /var/taskd/taskd.log `
`sudo chmod 644 /var/taskd/taskd.log `
`sudo chown taskdUser:taskdGroup /var/taskd/*.pem `
`sudo  systemctl enable taskd.service`
`taskd add org YOUR_ORGNAME`
`sudo adduser YOUR_USER taskdGroup `
`sudo chgrp -R taskdGroup orgs/`
`sudo chmod -R 770 orgs/`
`taskd add user 'YOUR_ORGNAME' 'YOUR_TASK_USER'`
`cd /var/taskd/pki/`
`./generate.client YOUR_TASK_USER`
`sudo chgrp -R taskdGroup /var/taskd/orgs/`
`sudo chmod -R 770 /var/taskd/orgs/ `
