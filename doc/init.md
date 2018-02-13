Sample init scripts and service configuration for Bwbcoind
==========================================================

Sample scripts and configuration files for systemd, Upstart and OpenRC
can be found in the contrib/init folder.

    contrib/init/Bwbcoind.service:    systemd service unit configuration
    contrib/init/Bwbcoind.openrc:     OpenRC compatible SysV style init script
    contrib/init/Bwbcoind.openrcconf: OpenRC conf.d file
    contrib/init/Bwbcoind.conf:       Upstart service configuration file
    contrib/init/Bwbcoind.init:       CentOS compatible SysV style init script

Service User
---------------------------------

All three Linux startup configurations assume the existence of a "Bwbcoin" user
and group.  They must be created before attempting to use these scripts.
The OS X configuration assumes Bwbcoind will be set up for the current user.

Configuration
---------------------------------

At a bare minimum, Bwbcoind requires that the rpcpassword setting be set
when running as a daemon.  If the configuration file does not exist or this
setting is not set, Bwbcoind will shutdown promptly after startup.

This password does not have to be remembered or typed as it is mostly used
as a fixed token that Bwbcoind and client programs read from the configuration
file, however it is recommended that a strong and secure password be used
as this password is security critical to securing the wallet should the
wallet be enabled.

If Bwbcoind is run with the "-server" flag (set by default), and no rpcpassword is set,
it will use a special cookie file for authentication. The cookie is generated with random
content when the daemon starts, and deleted when it exits. Read access to this file
controls who can access it through RPC.

By default the cookie is stored in the data directory, but it's location can be overridden
with the option '-rpccookiefile'.

This allows for running Bwbcoind without having to do any manual configuration.

`conf`, `pid`, and `wallet` accept relative paths which are interpreted as
relative to the data directory. `wallet` *only* supports relative paths.

For an example configuration file that describes the configuration settings,
see `contrib/debian/examples/Bwbcoin.conf`.

Paths
---------------------------------

### Linux

All three configurations assume several paths that might need to be adjusted.

Binary:              `/usr/bin/Bwbcoind`  
Configuration file:  `/etc/Bwbcoin/Bwbcoin.conf`  
Data directory:      `/var/lib/Bwbcoind`  
PID file:            `/var/run/Bwbcoind/Bwbcoind.pid` (OpenRC and Upstart) or `/var/lib/Bwbcoind/Bwbcoind.pid` (systemd)  
Lock file:           `/var/lock/subsys/Bwbcoind` (CentOS)  

The configuration file, PID directory (if applicable) and data directory
should all be owned by the Bwbcoin user and group.  It is advised for security
reasons to make the configuration file and data directory only readable by the
Bwbcoin user and group.  Access to Bwbcoin-cli and other Bwbcoind rpc clients
can then be controlled by group membership.

### Mac OS X

Binary:              `/usr/local/bin/Bwbcoind`  
Configuration file:  `~/Library/Application Support/Bwbcoin/Bwbcoin.conf`  
Data directory:      `~/Library/Application Support/Bwbcoin`  
Lock file:           `~/Library/Application Support/Bwbcoin/.lock`  

Installing Service Configuration
-----------------------------------

### systemd

Installing this .service file consists of just copying it to
/usr/lib/systemd/system directory, followed by the command
`systemctl daemon-reload` in order to update running systemd configuration.

To test, run `systemctl start Bwbcoind` and to enable for system startup run
`systemctl enable Bwbcoind`

### OpenRC

Rename Bwbcoind.openrc to Bwbcoind and drop it in /etc/init.d.  Double
check ownership and permissions and make it executable.  Test it with
`/etc/init.d/Bwbcoind start` and configure it to run on startup with
`rc-update add Bwbcoind`

### Upstart (for Debian/Ubuntu based distributions)

Drop Bwbcoind.conf in /etc/init.  Test by running `service Bwbcoind start`
it will automatically start on reboot.

NOTE: This script is incompatible with CentOS 5 and Amazon Linux 2014 as they
use old versions of Upstart and do not supply the start-stop-daemon utility.

### CentOS

Copy Bwbcoind.init to /etc/init.d/Bwbcoind. Test by running `service Bwbcoind start`.

Using this script, you can adjust the path and flags to the Bwbcoind program by
setting the BITCOIND and FLAGS environment variables in the file
/etc/sysconfig/Bwbcoind. You can also use the DAEMONOPTS environment variable here.

### Mac OS X

Copy org.Bwbcoin.Bwbcoind.plist into ~/Library/LaunchAgents. Load the launch agent by
running `launchctl load ~/Library/LaunchAgents/org.Bwbcoin.Bwbcoind.plist`.

This Launch Agent will cause Bwbcoind to start whenever the user logs in.

NOTE: This approach is intended for those wanting to run Bwbcoind as the current user.
You will need to modify org.Bwbcoin.Bwbcoind.plist if you intend to use it as a
Launch Daemon with a dedicated Bwbcoin user.

Auto-respawn
-----------------------------------

Auto respawning is currently only configured for Upstart and systemd.
Reasonable defaults have been chosen but YMMV.
