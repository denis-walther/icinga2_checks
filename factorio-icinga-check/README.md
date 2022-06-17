# Small Icinga2 check for factorio game servers

Since I use Icinga2 and also run a factorio game server I wanted to have a check running for the game service on the server.

This check can check for the 4 standard states:

| Exit Code | State | Service State |
| --- | --- | --- |
| 0 | OK | UP / OK |
| 1 | WARNING | Host is up but the service might be dead |
| 2 | CRITICAL | The service does not respond at all or the host also died |
| 3 | UNKNOWN | The service has a strange state |

To use this check simply clone the repository:

    git clone https://github.com/JustDenisYT/factorio-icinga-check.git 

Then copy the check to ```/usr/lib/nagios/plugins``` and give it executable permissions: ```chmod +x /usr/lib/nagios/plugins/check_factorio```

After that you need to define the check in your **templates.conf** like this:

    object CheckCommand "factorio" {
      import "plugin-check-command"
      command = [ PluginDir + "/check_factorio" ]
    }


And to use the service check you can simply define it in your **services.conf** like this:

    apply Service "factorio" {
      import "generic-service"
      check_command = "factorio"
      assign where host.name == NodeName
    }


