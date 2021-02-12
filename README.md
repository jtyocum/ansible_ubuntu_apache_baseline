# Apache2 Baseline for Ubuntu

This security baseline was tested on Ubuntu 18.04. It should work on other versions, and likely works on Debian. The baseline
enables SystemD sandboxing for Apache, tweaks a couple basic security settings, and install Mod_Security with the
OWASP Core Ruleset.

*NOTE:* This baseline assumes you have already installed Apache using another role.

## Configuration

Because the baseline enables sandboxing, you may need to configure the paths that Apache, and child processes can write to. The
write path can be set using the "a2_sandbox_rw_path" variable. This variable is simply a space delimited string of allowed paths. It
defaults to: /run /var/log /var/www /var/cache /tmp /var/tmp

*NOTE:* Sandboxing doesn't override file system permissions. In other words, if the Apache user doesn't have write permission, the
sandbox policy won't suddently allow it to.

## Sanbox Policy

Below is the default sandbox policy, which you may wish to use on other servers, such as PHP-FPM.

	[Service]
	CapabilityBoundingSet=~CAP_SYS_ADMIN
	ProtectSystem=strict
	ProtectHome=yes
	ReadWritePaths=/run /var/log /var/www /var/cache /tmp /var/tmp
	PrivateDevices=yes
	ProtectClock=yes
	ProtectKernelTunables=yes
	ProtectKernelModules=yes
	ProtectKernelLogs=yes
	ProtectControlGroups=yes
	LockPersonality=yes
	SystemCallFilter=~@clock @cpu-emulation @debug @obsolete @module @mount @raw-io @reboot @swap
	SystemCallErrorNumber=EPERM
