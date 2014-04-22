My Own Gollum setup
===================


Requirement
-----------

I need to setup gollum + dropbox on one Linux server, so it can be access easily from Windows machine due to fact that gollum doesn't support Windows yet.


### Installaion

need to install **Dropbox & command line interface**, **Gollum**, **Supervisord**, 

##### Dropbox & command line interface 

64-bit:

`cd ~ && wget -O - "https://www.dropbox.com/download?plat=lnx.x86_64" | tar xzf `

`mkdir -p ~/bin`

Download the CLI interface via  wget:


`wget -O ~/bin/dropbox.py "https://www.dropbox.com/download?dl=packages/dropbox.py"`

Set the permissions so you can execute the CLI interface:

`chmod +x ~/bin/dropbox.py`
 


Use utility to start and exclude files you dont want to sync


```bash

$ ~/bin/dropbox.py start
$ ~/bin/dropbox.py help exclude
dropbox exclude [list]
dropbox exclude add [DIRECTORY] [DIRECTORY] ...
dropbox exclude remove [DIRECTORY] [DIRECTORY] ...
```



##### Gollum

* use rmv or rbenv to install ruby
* use gem to install gollum  
`gem install gollum`
* use rvm to create a wrapper for running gollum  
`rvm wrapper 1.9.3 startup gollum`



##### Supervisord

* use setuptools to install 
`easy_install supervisor`

* create configuration file
`echo_supervisord_conf > /etc/supervisord.conf`

* update for managing dropbox & gollum  
```
[program:gollum]
command=/home/jzk/.rvm/bin/startup_gollum   /home/jzk/Dropbox/wiki --config  /home/jzk/Dropbox/wiki/authentication.rb 

[program:dropbox]
;command=python /home/jzk/dropbox.py start
command=/home/jzk/.dropbox-dist/dropbox
```

* to automatically start supervisord on Linux (Ubuntu)  
```
sudo curl https://gist.github.com/howthebodyworks/176149/raw/88d0d68c4af22a7474ad1d011659ea2d27e35b8d/supervisord.sh > /etc/init.d/supervisord
sudo chmod +x /etc/init.d/supervisord
sudo update-rc.d supervisord defaults
```

* Make ensure correct pid in /etc/supervisord.conf which is mapped in /etc/init.d/supervisord   
`example: pidfile=/var/run/supervisord.pid`

* Stop and Start work properly   
```
service supervisord stop
service supervisord start
```