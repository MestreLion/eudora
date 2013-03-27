Eudora in Ubuntu
================

Installer
---------

From **install --help**:

	Usage: install [options] [[--tarball] FILE]

	An installer for Eudora Mail in Debian/Ubuntu/Mint

	Includes uninstall script. Tested with v7.1.0.9

	Options:
	-h|--help  - show this page.
	-q|--quiet - supress informative messages.

	--system
	--user
		choose system-wide or per-user install. Default is user

	--exec=NAME
		executable name, also used as a prefix for for naming the
		install directory, icon and .desktop files. Default "eudora"

	--prefix=DIR
		parent of the install directory. Default is
		"~/.local/share/wineprefixes"

	--name=NAME
		friendly application name, for menu entries. Default "Eudora Mail"

	--installer FILE
		the .exe install file downloaded from Eudora Mail's website.
		If none is provided, a menu will show currently downloaded files.

	--uninstall
		Uninstall Eudora Mail. Combine with --system, --exec and --prefix to
		uninstall from a custom install

Eudora (once installed)
-----------------------

From **eudora --help**:


	Usage: eudora [OPTIONS] [ MAILTO-URI | ADDRESS(ES)... | FILE(S)... ]
	       eudora [OPTIONS] --raw [ARGUMENTS...]
	       eudora { --help | --manual | --version }

	Launches Eudora Mail under Wine

	Options:
	--config FILE     Settings file to read. Default is ~/.config/eudora/eudora.conf
	--wineprefix DIR  Unix path to wine's windows environment where Eudora Mail is
	--exefolder DIR   Unix path where Eudora.exe is located
	--datafolder DIR  Unix path to Eudora's data Folder
	--window VALUE    Sets window state. VALUE may be normal, minimized or maximized
	--debug           Turns on debug mode
	--raw             Relay all command-line arguments unparsed to Eudora.exe

	Use "eudora --manual" for additional info

And its **manual**:

	eudora - launcher for Qualcomm's Eudora Mail under wine

	Usage

	eudora [OPTIONS] [ MAILTO-URI | ADDRESS(ES)... | FILE(S)... ]

	eudora [OPTIONS] --raw [ARGUMENTS...]

	eudora { --help | --manual | --version }


	Description

	Eudora is a Mail Client for windows. This launcher provides a basic interface
	for native enviroment to use Eudora running under Wine compatability layer.

	It is not recommended for the user to directly invoke this eudora launcher
	with arguments. Its main purpose is to be registered in a native desktop
	environment as default email client / mailto handler, so Eudora can be
	integrated and used as a native email client by tools like xdg-email, Nautilus'
	"Send to -> Email", Simple Scan, Web browser "mailto:" links, etc.

	After eudora is set as default email client, it is strongly encouraged that
	xdg-email tool is used for launching eudora, by both direct user invocation
	and scripting. It has features like URI encoding, exit codes for missing files,
	and its command-line options are much more user-friendly.


	Options

	Eudora and email options:

	MAILTO-URI
	    A mailto uri in the format "mailto:[ADDRESS][?OPTION[&OPTION(S)...]]",
	    according to RFC2368. Valid OPTIONS are "to=ADDRESS", "cc=ADDRESS",
	    "bcc=ADDRESS", "subject=SUBJECT", "body=BODY". Aditionaly, "attach=FILE",
	    while not part of RFC2368, is also accepted.

	    Some mailto uri examples:
	    mailto:aaa@aaa.com?subject=hello&body=Email%20test
	    mailto:attach=/dir/file1&attach=/dir/file2

	    Since mailto uri is not parsed (except for attachments, see note below),
	    and must be properly encoded, it is highly recommended that xdg-email
	    utility is used to compose the mailto uri and invoke eudora.

	    If any valid attachment is present in the uri, all other fields are ignored
	    See Attachments section for details and limitations on attachment handling.

	ADDRESS(ES)...
		A valid (list of) email address(es). If any character other than letters,
		digits or @.-\/ are present in the address, it must be properly url-encoded.

		As with mailto uri, it is highly recommended that xdg-email utility is used
		to compose the mailto uri and invoke eudora.

	FILE(S)...
	    A (list of) file to be sent as attachment. If any valid file is given, any
	    mailto uri or address(es) are ignored. See Attachments section for details
	    and limitations on attachment handling. Files are required to be present
	    after eudora returns. Test for Eudora.exe process if deleting temporary
	    files is required.

	Settings and control options:

	--config FILE
	    Full path and filename of configuration file where settings will be taken
	    from. Default is "~/.config/eudora/eudora.conf"
	    Overwrites EUDORA_CONFIG environment variable

	--wineprefix DIR
	    Path to wine's virtual windows environment where Eudora Mail is installed.
	    Overwrites WINEPREFIX environment variable

	--exefolder DIR
	    The full (unix) path where Eudora is installed (where Eudora.exe is located)
	    Default is "$WINEPREFIX/dosdevices/c:/Program Files/Qualcomm/Eudora"

	--datafolder DIR
	    The full (unix) path to Eudora's Data Folder, which stores user's mailboxes,
	    emails and settings. Default is:
	    "$WINEPREFIX/dosdevices/c:/users/$USER/Application Data/Qualcomm/Eudora"

	--window normal | max[imized] | min[imized]
	    Sets Eudora's window state when launched. Default is "normal"

	--debug
	    Turns on shell attribute x (set -x), to print commands and their arguments
	    as they are executed, and redirects all output that otherwise would be
	    silenced to ~/.config/eudora/eudora.log (see Configuration Files section).

	--raw
	    Relay all command-line arguments (except --raw itself and above options)
	    directly to Eudora.exe windows executable. No parsing of addresses, mailto:
	    URI, file handling or translation from unix paths to windows paths is
	    performed. For testing purposes only.

	Above options take precedence over environment variables or settings in the
	configuration file. See Environment Variables and Configuration Files sections.

	Generic options:

	--help
	    Show command synopsis.

	--manual
	    Show this manualpage.

	--version
	    Show the launcher version information.


	Attachments

	Eudora.exe accepts as command line arguments either a mailto uri OR a list of
	filenames to be attached, but not both. Furthermore, any "attach" field of a
	mailto uri is silently ignored, since it is not part of original RFC2368
	specification. Thus, it is currently not possible to launch Eudora.exe with both
	attachments and other (to, cc, bcc, subject, body) fields.

	So, when an "attach" field is present is in the mailto uri, file is tested and,
	if valid, it is passed as command line argument and the mailto uri argument is
	discarted. Any other valid files passed directly in command line also makes
	eudora (this launcher) silently ignore and discart any mailto uri before
	launching Eudora.exe

	Valid files are the ones that satisfy ALL following conditions:

	- File must exist and not be a directory

	- Full path must contain a target mapped to a wine drive (C:, D:, Z: etc)
	  or contain a target of one of wine's user folders (My Documents, Desktop,
	  My Pictures, etc - usually mapped via winecfg's Desktop Integration to native
	  user folders $HOME, $\HOME/Desktop, $HOME/Pictures etc)

	- Due to a bug in Eudora.exe not properly handling escaping characters, file
	  name and path, when translated to a windows syntax path, must not cointain any
	  spaces

	All non-valid files, either from mailto uri or command line, are silently
	ignored.

	All file names and paths, in both mailto uri or command line argument, must use
	Unix syntax (/path/to/file). Files attached via mailto uri may start with a
	"file://" prefix (that is stripped before testing) and must be url encoded.
	Url decode and translation to windows path syntax (C:\path\to\file) is performed
	by eudora. Relative paths are accepted and properly translated.


	Environment Variables

	eudora honours the following environment variables, which takes precedence over
	settings in the cofiguration file but may be overriden by respective command
	line options.

	WINEPREFIX
	    Path to wine's virtual windows environment where Eudora Mail is installed.
	    See wine documentation for more details. Default is "~/.wine"
	    May be overwritten by --wineprefix option

	EUDORA_CONFIG
	    Full path and filename of configuration file where settings will be taken
	    from. Default is "~/.config/eudora/eudora.conf"
	    May be overwritten by --config option

	wine, used internally to launch Eudora, is also affected by several other
	environment variables, such as WINEPATH and WINEDEBUG, and can affect eudora.


	Configuration files

	All configuration files are stored in ~/.config/eudora. The following are used:

	eudora.log
		Dump of execution commands when debug mode is activated

	eudora.conf
	    Default configuration settings, in option=value format. This file will be
	    sourced by the launcher, so #comments are allowed but extra caution must be
	    taken with its syntax. These settings may be overriden by environment
	    variable WINEPREFIX or respective command line options.

	    Allowed options are:
		WINEPREFIX
		exefolder
		datafolder
		window
		debug
		raw

	    If --config FILE option is used, settings file is read from FILE instead of
	    the default ~/.config/eudora/eudora.conf.


	Exit Codes

	An exit code of 0 indicates success while a non-zero exit code indicates
	failure. The following failure codes can be returned:

	1   Could not find Eudora directory. See exefolder option


	Examples

	eudora mailto:someone@somewhere.com

	eudora mailto:eudora@qualcomm.com?cc=a@b.c&subject=Eudora+Lives&body=..kind+of

	eudora /tmp/somefile.txt

	eudora mailto:?attach=/tmp/AttachMe&attach=file:///tmp/AndMe' ../test/MeToo.txt

	eudora /tmp/OkToAttach.jpg "/tmp/no spaces allowed.mp3" /tmp/SorryNoDirsEither/

	eudora mailto:dontmix@withattaches.com?body=i+will+be+ignored&attach=/tmp/file


Written by
----------

Rodigo Silva (MestreLion) <linux@rodrigosilva.com>

Licenses and Copyright
----------------------

Copyright (C) 2011 Rodigo Silva (MestreLion) <linux@rodrigosilva.com>.

License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>.

This is free software: you are free to change and redistribute it.

There is NO WARRANTY, to the extent permitted by law.

**IMPORTANT NOTE: The above copyright notice and GPL licence are for this launcher
and associated native files only! Eudora Mail itself is propietary software,
copyright of Qualcomm Inc. Wine and its tools are free software under different
copyright and license. See <http://www.eudora.com> and <http://www.winehq.org>**