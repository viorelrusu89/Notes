#Writing Kibana 5 Plugins

I am following Tim Roes tutorial [Writing Kibana 4 Plugins – Basics](https://www.timroes.de/2015/12/02/writing-kibana-4-plugins-basics/) and updating it for Kibana 5.

I assume the followings:
* You have successfully installed kibana 5 for development. You can find a good guide [here](https://github.com/jgbarah/Notes/blob/master/kibana-devel.md).
*You have installed a suitable version of elasticSearch for your Kibana.
* You are using nvm for node.js version managing installing it as specified [here](https://github.com/jgbarah/Notes/blob/master/kibana-devel.md#install-the-nvm-tool)

### Install node.js and dependencies

Now, in the kibana directory, I will run nvm to install the appropriate version of node.js, and then npm to install the required JavaScript dependencies. Before that, I "activate" nvm (see above):

```
source ../nvm/nvm.sh
nvm install "$(cat .node-version)"
npm install
```


##Running Kibana in dev mode
```
bin/kibana --dev
```

In my case, the execution failed because the default file watchers of my ubuntu distribution are not enough. The error is not very descriptive:

 failed to watch files!  Error: watch /home/vio/kibanadev2/src/plugins ENOSPC
    at exports._errnoException (util.js:870:11)

What I did is to permanently modify the maximum notify watchers variable in sysctl:

```
echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p
```

Rerun
```
bin/kibana --dev
```

If everything is ok, you should get a message about lazy optimization at the end.


##Create plugin

I import Tim Roes plugin
```
cd installedPlugins
git clone https://github.com/timroes/tr-k4p-clock
```
I had to make some changes to make this plugin work in Kibana 5 (not sure if version fix or just a mistake in original script)

line 24:
```
-- var TemplateVisType = Private(require('ui/template_vis_type/TemplateVisType'));
++ var TemplateVisType = Private(require('ui/template_vis_type/template_vis_type'));
```



