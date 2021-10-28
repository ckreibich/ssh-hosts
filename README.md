ssh-hosts
=========

ssh-hosts is a Python script that lets you search through your ssh hosts configuration (typically ~/.ssh/config) from a terminal. This can prove quite handy once your config file spans several dozen hosts.

Features
--------

* UNIX glob-style matching on host names or any part of a host specification

Deficiencies
------------

* Doesn't currently support config file includes

Examples
--------

Let's say this is your ~/.ssh/config:

    Host kermit
    HostName kermit.example.com
    User henson
    
    Host piggy1
    HostName piggy1.test.example.com
    ForwardX11 yes
    DynamicForward 8080
    User henson
    
    Host piggy2
    HostName piggy2.test.example.com
    ForwardX11 yes
    LocalForward 8080 kermit.example.com:8080
    
    Host gonzo
    HostName gonzo.test.example.com

Just running ssh-hosts will print out the full hosts file, stripped of any comments. Let's say you want to pull up the details just on host kermit:

    $ ssh-hosts kermit
    Host kermit
    HostName kermit.example.com
    User henson

Let's say you want to list all of the piggy hosts:

    $ ssh-hosts piggy*
    Host piggy1
    ForwardX11 yes
    HostName piggy1.test.example.com
    DynamicForward 8080
    User henson
    
    Host piggy2
    ForwardX11 yes
    HostName piggy2.test.example.com
    LocalForward 8080 kermit.example.com:8080

Now you want to pull up all hosts for which you have port forwarding enabled. You use the -g flag to enable global matching on all words of the host spec:

    $ ssh-hosts -g *Forward
    Host piggy1
    ForwardX11 yes
    HostName piggy1.test.example.com
    DynamicForward 8080
    User henson
    
    Host piggy2
    ForwardX11 yes
    HostName piggy2.test.example.com
    LocalForward 8080 kermit.example.com:8080

Which hosts live in the test.example.com subdomain? Multi-word searches with glob patterns work too:

    $ ssh-hosts -g "HostName *.test.example.com"
    Host gonzo
    HostName gonzo.test.example.com

    Host piggy1
    ForwardX11 yes
    HostName piggy1.test.example.com
    DynamicForward 8080
    User henson
    
    Host piggy2
    ForwardX11 yes
    HostName piggy2.test.example.com
    LocalForward 8080 kermit.example.com:8080

And, finally, you want to know the list of machines that you log into as user henson.

    $ ssh-hosts -g "User henson"
    Host kermit
    HostName kermit.example.com
    User henson
    
    Host piggy1
    ForwardX11 yes
    HostName piggy1.test.example.com
    DynamicForward 8080
    User henson
