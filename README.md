# "Idena Linux-Node" control script

## Why this script exists in first place .. ?

So that one can - detach - and mark idena-node instance as unique process during command line startup execution. This kind of action enables user to focus on other tasks within one single instance via ssh, for example. With *two words*. 

And Two word combo's are:

    $ idna status       # tells the node status
    $ idna start        # starts the node - if not already running. *(the status will tell)
    $ idna stop         # kills (stops) the node and all it's child processes.

    # Anything other than this will mimic friendly error-alike message.

    
**Note:**
  It doesn't need to be  - `idna` -  It can be anything readable. :)


##  Tested over ssh in LOCAL \*(LAN/cable) network

Client machine

     Debian 10 Buster (Debian 5.9.15-1~bpo10+1 (2020-12-31) x86_64 GNU/Linux)

Node Machine

     Debian 10 Buster (Debian 5.10.13-1~bpo10+1 (2021-02-11) x86_64 GNU/Linux)


## Consider the following:

- **`$Version`** or **`0.24.3`** as Version 

- **`earthling_doe`** as User \*(You, reading this) 

- **`192.168.0.120`** as Remote machine's example IP
 
- **`192.168.0.150`** as Client machine's example IP

- '**`$`**' (without singlequotes) before `command` indicates user \*(and NOT) root shell.


### Assumptions/cosiderations

- Assuming that client machine `192.168.0.150` will be connected to remote/local network node.

    ( **`NFS`** node address with port example: `http://192.168.0.120:9009` )

- Assuming that `earthling_doe` from *client machine* (`192.168.0.150`) is whitelisted user in ssh config file on *node machine* (`192.168.0.120`) and that `earthling_doe` is logged *`as@on`*: `earthling_doe@node_machine`.

- Assuming that `earthling_doe` has already downloaded *`idena-node-linux-$Version`* and has placed it somewhere outside it's `$HOME` directory, eg: `/opt/Idena/idena-node-linux-$Version`

- Assuming that `earthling_doe` is using external configuration file (look inside `idna` file which one that is `LINE 20`, follow `# the comments`)

- Assuimng that `earthling_doe` MUST have \*(best, safest practice) `$HOME/bin` in it's path ..

    .. and if not, place the following at the bottom of `$HOME/.profile` file:
    
      # Set PATH so it includes user's private bin if it exists
      if [ -d "$HOME/bin" ]; then
         export PATH="$HOME/bin:$PATH"
      fi

- Assuming that `idna-ctrl` is a directory where copied files from this very repo reside

- Assuming that `$HOME` equals `/home/earthling_doe`


Keep in mind that NFS server on `node_machine` IS NOT required, unless You want to share live console stdout to other LAN machines. 


### Setup procedure

#### STEP 1 (create symbolic link to idena-linux-node executable):

One need to create symlink to the exectuable file and to allow its - `execution`. 
  (omit the following command if not already executed)

    $ sudo  chown -R earthling_doe:earthling_doe /opt/Idena/
    
  Create symbolic link: 
   
    $ ln -s /opt/Idena/idena-node-linux-0.24.3 $HOME/bin/idena

#### STEP 2 (make idena-linux-node - truly executable):

Make idena-linux-node executable directly via previously created symlink:

      $ chmod +x $HOME/bin/idena

#### STEP 3 (copy/make control script):

1. Copy `idna` file to `$HOME/bin/idna`.
    
    *note that `/readable/path/to/idena-ctrl/` may be any accessible dir.*

        $ cp /readable/path/to/idena-ctrl/idna $HOME/bin/idna

    .. or any other name for that matter if `idna` is .. too short .. ?

        $ cp /readable/path/to/idena-ctrl/idna $HOME/bin/idna-miner

    One can also do it on harder way, by hand and - mouse.

    If that's the case, then create blank `idna` file in `$HOME/bin` directory.

        $ cd $HOME/bin && touch idna && nano idna;

    Open \*(on client PC) `idena-ctrl/idna` file and copy contents \*( `ctrl + a` --> `ctrl + c` \*(or right-mouse-click and 'copy') ), 
    <br> then (right click) paste into ssh window where nano buffer is opened, save file and close nano.

      - Terminal emulators are treating `ctrl + shift + V` (no, it is not `ctrl + v` ) as - paste - command.
      - Press `ctrl + o` then `enter` \*(to save and to confirm file name) then `ctrl + x` to exit nano.

#### STEP 4 (make script executable):

This or that - `$HOME/bin/idna` must become executable.

    $ chmod +x $HOME/bin/idna

#### STEP 5 (configuration file):

Copy `.goidna.json` file into `$HOME/.goidna.json` from `idna-ctrl`

In order to adjust index key values properly.

1. Provide correct \*(and the same) `Datadir` and `IpfsConf.DataDir` values.
2. Provide true `RPC.HTTPhost` ( `"0.0.0.0" === 'listen on all interfaces'` ) `node_machine` address. 

Optionally adjust `RPC.HTTPhost`, `RPC.HTTPport.*` and (.. or should You ?) `IpfsConf.IpfsPort.*` indexes.

### Script control

**`CHECK/START:`**

    $ idna status
 
  Is there a word "*NOT*" in output response .. ?
 
  .. if there is:
 
    $ idna start
 
  Otherwise, it is probably running (refer to final note below).

**`STOP:`**

    $ idna stop

## FINAL NOTE

 You may want to check with `htop` command.

 Once when htop is opened, press/hit F4 and type `netmine`.
 You should see idena-linux-node `nickname` (netmine) process with it's child subprocesses onscreen.

 `netmine` is actually fakename for non-existent `service` control, thanks to bash `exec` function.

 And if `earthling_doe` changes config file name (`idna` file) from `$HOME/.goidna.json` to '`$HOME/yabadabadooya.json`',
 where '`$HOME/yabadabadooya.json`' will exist with real, working and valid contents - the better. :)

 *(**hint**: Type `exec --help` if bored and look for -a option. Very nifty.)*
