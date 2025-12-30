+++
title = "IRC with Emacs ERC and ZNC"
author = ["Katabex"]
date = 2025-12-30T00:00:00+01:00
tags = ["Emacs", "IRC"]
draft = false
+++

RC is a protocol that is older than the Web and has a [long](<https://daniel.haxx.se/irchistory.html>) [history](<https://youtu.be/6UbKenFipjo>) behind it. The protocol is still used, but it is not really something that the majority of mainstream messaging application users would get excited about. The protocol is open and free to use, and because of this, there is not much commercial interest in investing in applications that would attract “mainstream” users.

To use the protocol properly, you not only need an IRC client application, but also a so-called IRC bouncer - a sort of proxy between your IRC client of choice and the IRC server. The bouncer is a server application that runs 24 hours per day and monitors activity on the IRC channels (chat groups) to which the user is subscribed. While the user is away, the bouncer records all activity and replays it to the IRC client when the client reconnects. The client alone may connect directly to the IRC server, but this does not guarantee that the user will see all messages posted to a channel while he or she is away.

This requirement alone is a threshold that prevents the majority of potential users from ever considering an IRC-protocol-based client as their chat application of choice. You need to know what you are doing, and going through the hassle of setting up a bouncer is a test you must pass in order to fully participate in IRC-based communities.


## My setup {#my-setup}

There are many IRC clients and a number of bouncers that can be used. My preferred setup is based on IRC client build in Emacs called [ERC](<https://www.gnu.org/software/emacs/manual/html_mono/erc.html>) and the [ZNC](<https://wiki.znc.in/ZNC>) bouncer.


### How ERC, ZNC, and IRC Interact {#how-erc-znc-and-irc-interact}

Here’s a simple diagram of how the components work together.

{{< figure src="/images/irc_setup.png" >}}

How it works:

1.  ERC client connects to ZNC bouncer running on a server.
2.  ZNC stays connected to the IRC server 24/7, recording messages while you are away.
3.  ERC fetches messages from ZNC, so you never miss conversations.


## IRC nick name registration {#irc-nick-name-registration}

The first thing that needs to be arranged in order to participate in the IRC network is the registration of your chosen IRC nickname.

Go to an IRC server that hosts the channels you want to subscribe to (for example, [libera.chat](<https://libera.chat/>)) and register your nickname using the following command:

```text
/msg NickServ REGISTER IRC_password your@email.address.com
```

-   IRC_password = your chosen password
-   `your@email.address.com` = your email address

You can use a web client provided by the IRC server to register without an IRC client. If the nickname is available, it will be registered.


## ZNC setup {#znc-setup}

Follow the installation instructions provided by the [ZNC website](<https://wiki.znc.in/ZNC>) but also keep in mind that the bouncer should run on a machine that is active 24 hours a day. Your laptop is not a good choice. You may consider Raspberry Pi or similar board to set it up as a server.

Once installed, use znc `--makeconfig` to generate an initial configuration. Follow prompts to set up your ZNC username, password, and server port.

Finally start the service and access the web administration interface.

-   Open `https://localhost:ZNC_port` in a browser.
-   Log in with your ZNC username and password.
-   Use `Manage Users` to configure the IRC networks and channels.

{{< figure src="/images/znc_web_interface.png" >}}

You can also set up a SASL authentication module to automatically authenticate with the IRC server using your registered IRC nick name and password.


## ERC configuration {#erc-configuration}

Once the ZNC setup is complete, it is time to configure your client. Below is the basic configuration for ERC, my IRC client of choice.

Uncomment `irc` in the `:app` section of in the `~/.config/doom/init.el` configuration file to enable ERC .

```emacs-lisp
(doom! :app
      irc
      )
```

Include the following configuration into the `~/.config/doom/config.el` file.

```emacs-lisp
(require 'erc)

(setq auth-sources '("~/Sync/shared/.authinfo.gpg" "~/.authinfo.gpg" "~/.authinfo" "~/.netrc"))

(defun my-erc ()
  "Connect to my ZNC bouncer."

  (interactive)
  (erc-tls
   :server "ZNC_server_address"
   :port ZNC_port
   :nick "IRC_nick_name"
   :password (auth-source-pick-first-password :host "ZNC_server_address")))
```

The configuration provides all the information needed for ERC to connect to the ZNC bouncer.

-   ZNC_server_address: IP address or domain name of the ZNC server
-   ZNC_port: The port you have chosen for ZNC access
-   ZNC_password: The ZNC user password
-   IRC_network: The name of the network set during ZNC configuration
-   IRC_nick_name: The IRC nick name you have registered

It ensures that ERC can locate the server running ZNC and authenticate with it using the password you have set for your ZNC user.

The ZNC authentication parameters must be provided in the `~/.authinfo` file located in your home directory. The file format is as follows.

```text
machine ZNC_server_address login IRC_nick_name password ZNC_user_name/IRC_network:ZNC_password port ZNC_port
```

Ensure your password consists of these three parameters:

```nil
ZNC_user_name/IRC_network:ZNC_password
```

Once you have finished editing your configuration, execute the `doom sync` command in a terminal. This will apply your changes and update Doom Emacs. After the command completes, restart Emacs to ensure that the new configuration is loaded.

To start ERC, run the command `M-x my_erc` inside Emacs. ERC will then attempt to connect to your ZNC bouncer using the credentials and server information you set up.

Once connected, ERC will create a separate buffer for each IRC channel you subscribed to when configuring your IRC network. You can switch between these buffers using standard Emacs buffer commands, such as C-x b.

After everything is working, you can start chatting immediately and ERC will automatically keep track of messages on your subscribed channels even while you are away thanks to ZNC.


## Troubleshooting {#troubleshooting}

If the connection fails, check that:

-   ZNC is running and accessible at the server address and port you specified.
-   Your .authinfo file contains the correct ZNC username and password.


## Relevant links {#relevant-links}

-   [ERC](<https://www.gnu.org/software/emacs/manual/html_mono/erc.html>)
-   [ZNC](<https://wiki.znc.in/ZNC>)
-   [ZNC installation instructions](<https://wiki.znc.in/Installation>)
