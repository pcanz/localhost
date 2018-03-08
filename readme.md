#	Apache localhost

This script will run the Apache server as a localhost, which allows the web browser to be used to browse local files. The localhost server will not respond to network requests, only to local requests (on IP 127.0.0.1 loopback).

The Apache server is the defacto web standard server. It is well developed and documented. But it is a large complex system, so even to configure it to run as a localhost requires significant learning. There are also several tricks needed to deal with macOS system files and access permissions.

This `localhost` script aims to make it as simple as possible to run the Apache server as a local host server. It will save a copy of the Apache config file and restore that after running the localhost server.

The script has an associated `localhost.conf` file that is used to run the Apache server. The script will automatically load this file with the users access permissions and the required port and root directory. The `localhost.conf` file may be manually edited if more specific configurations are required.

##	Installation

This script has been developed to run on Apple macOS/OSX, which has Apache installed with a config file at: `/etc/apache2/httpd.conf`.

The localhost command script may be loaded into any directory, but the `localhost.conf` file must be loaded into the same directory as the script file.

It is convenient to load the script into the users home directory, in which case the script is available as: `~/localhost`, with the configuration file in: `~/localhost.conf`.

The script will need to be given exec permission:

	$ sudo chmod u+x ~/localhost

##	Usage

The script will start or stop the localhost server using the standard port 80 with the users home directory ~/ as the domain root.

Simple usage from the Terminal only requires the command line:

	$ ~/localhost

to start or stop the localhost server.

The browser can be given the URL: `localhost`, or `127.0.0.1`, and the browser should then display the users file system home directory. If a different port is used, then a URL like: `localhost:8080` can be entered.

The command allows the localhost to be run on a different port, and/or root directory:

	$ ~/localhost [port] [root]

For example:

	$ ~/localhost 8080 ~/mysite

	$ ~/localhost ~/some/dir

The defaults are the same as:

	$ ~/localhost 80 ~/

##	Background

I did not find it easy to configure Apache as a localhost server, hence this command script. However, if you do wish to configure Apache directly here are my notes that may help in conjunction with the Apple guide for using Apache, and the Apache documentation.

To start and stop the Apache server, the Terminal commands are:

	$ sudo apachectl start
	$ sudo apachectl stop

To run Apache as a localhost server you will need to edit the config file: `/etc/apache2/httpd.conf`. But this is hidden by default, so you have to find it first. In the Finder:
 
*	Enable "Show all filename extensions" in the Finder, via: Preferences/Advanced.
*	Navigate to the device: "Macintoch HD" to see the root of the system file directory.
*	Hit the Command-Shift-. key to reveal the hidden system files.

There is also a trick to find the full path name for a file: in the Finder select a file, then: Control-key mouse-click for a pop-up menu, then use the Option-key to modify the menu to show: Copy file as pathname, which can then be used to copy the full file path name to your clip-board.

To open the httpd.conf text file in your editor may require the same Command-Shot-. (dot) key to show the system files. Save a copy first, the localhost script uses: `httpd.conf.saved` to save and restore `httpd.conf`.

The major things to edit in httpd.conf:

*	`Listen 80	==>	Listen 127.0.0.1:80`
*	`User _www	==>	User your-username`
*	`DocumentRoot "/Library/WebServer/Documents"	==>	DocumentRoot "/Users/your-username"`
*	`<Directory "/Library/WebServer/Documents">	==>	<Directory "/Users/your-username">`

In the Terminal the command `whoami` will provide `your-username`.


##	References

Apache documentation: <https://httpd.apache.org/docs/2.4/>

Apple guide for using Apache as a localhost server: <https://discussions.apple.com/docs/DOC-11238>
