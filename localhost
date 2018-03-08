#! /bin/bash

# To run Apachee as a localhost server (with no external access)
#  enables URL: localhost in the browser (or: localhost:<port>)

# To toggle start or stop a localhost server:
#
#	$ ~/localhost
#
# The Apache httpd.conf file is saved and restored.

# To instal this script:
#
# This script may be in any directory, say: ~/localhost
# But the localhost.conf file MUST must be in the same directory
# as the script, so: ~/localhost.conf
#
# The script needs exec permission:
#	$ chmod +x ~/localhost
# then it can be run as:
#	$ ~/localhost

# To start the local host with a specific port and/or root directory:
#	$ localhost [port] [root] | start | stop
#	port = Listen port number, defaults to 80
#	root = DocumentRoot directory, defaults to ~/
#
# Example:
#	$ localhost ~/my/sites
#	$ localhost stop

# The localhost.conf file may be edited for any other options.

# Apache config file...
CONF=/etc/apache2/httpd.conf
# to save a copy of CONF before running as localhost...
SAVED=/etc/apache2/httpd.conf.saved

# get write permission for Apache config file...
if [ ! -w $CONF ]; then
	sudo chmod a+w $CONF
fi

RUNNING=$(eval ps aux | grep httpd | wc -l )

if [ "$1" = stop ] || [ $RUNNING != 1 ]; then
	sudo apachectl stop
	echo "Apache localhost stopped...."
	# restore httpd.conf (saved when localhost was last started)
	if [ -f $SAVED ]; then
		sudo cp $SAVED $CONF
	fi
	exit 0
fi

SCRIPT_DIR=$(cd $(dirname $0) && pwd)
# config file for localhost...
LOCAL=${SCRIPT_DIR}/localhost.conf
# this should not happen, there must be a default config file...
if [ ! -f $LOCAL ]; then
	echo "Can't find config file: $LOCAL"
	exit 1
fi

PORT=80
ROOT=~

if [ $# = 2 ]; then
	PORT=$1
	ROOT=$2
elif [ $# = 1 ] && [ $1 != start ]; then
	if [[ ${1:0:1} =~ [1-9] ]]; then
		PORT=$1
	else
		ROOT=$1
	fi
fi

if [ ! -d $ROOT ]; then
	echo "$ROOT is not a directory..."
	exit 2
fi
if [[ $ROOT =~ [!] ]]; then
	echo "Sorry, can't allow ! in directory name: $ROOT"
	exit 3
fi

# Ready to config and start Apache, save a copy of httpd.conf
sudo cp $CONF $SAVED

Who="'s/<WHOAMI>/$(whoami)/'"

Listen="'s/^Listen .*$/Listen 127.0.0.1:$PORT/'"

Root="'s!<ROOT>!$ROOT!'"

$(eval sed -e "$Who" -e "$Listen" -e "$Root" $LOCAL > $CONF)

echo $(grep ^Listen < $CONF)
echo $(grep ^DocumentRoot < $CONF)

sudo apachectl start
echo "Apache localhost running...."

exit 0
