# Installing and Managing Software
Managing software for DEB based distros. APT - Advanced Packaging Tool.

- `apt-cache search string`: search for string.
- `apt-get update`: is used to download package information from all configured sources.
- `apt-get upgrade`: is used to install available upgrades of all packages currently installed on the system.
- `apt-get install package`: install package.
- `apt-get remove package`: remove package, leaving configuration.
- `apt-get purge package`: remove package, deleting configuration.
- `apt-cache show package`: display information about package.
- `dpkg -l`: list installed packages.
- `dpkg -S /path/to/file`: list file's package.
- `dpkg -L package`: list all files in package.
- `dpkg -i package.deb`: install package.