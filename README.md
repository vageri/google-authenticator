#google-authenticator
Google Authenticator PAM module demonstrating two-factor authentication.

##Big fat warning ;)
**I do not keep this repo updated.** Please go at http://code.google.com/p/google-authenticator/source/browse/libpam to get the latest code.


This repo is patched with http://code.google.com/p/google-authenticator/issues/detail?id=27#c5 It consists in these two switches:

>	1. "pass_unconfigured": if this is specified, then the pam module will ignore users that do not have authenticator setup (it will return success).
>	2. "suffix=[xxx]": for adding a suffix to the user's homedir path. [...] this is how I deal with encrypted home directories.

As of 2011/04/25, the patch hasn't been included upstream.

##Usage
* Ubuntu users please run this command before building this program:
>	sudo apt-get install libpam0g-dev libqrencode3

* Build and install by running "make install".

* Then add the following line at the end of your PAM configuration file.
  Under Ubuntu, you should add that line to three files:
  1. /etc/pam.d/gdm
  2. /etc/pam.d/gnome-screensaver
  3. /etc/pam.d/sshd
>	auth required pam_google_authenticator.so pass_unconfigured

* If you use ecryptfs the PAM config line should look like:
>	auth required pam_google_authenticator.so pass_unconfigured suffix=.auth

* If you use ecryptfs run these commands as root, replacing username with your user name:
>	mkdir /home/username.auth<br>
>	mv /home/username/.google_authenticator /home/username.auth/.google_authenticator<br>
>	chmod 0700 /home/username.auth
>	chown username /home/username.auth

* Make sure that you have this line in /etc/ssh/sshd_config:
>	ChallengeResponseAuthentication yes

* Run the "google-authenticator" binary to create a new secret key in your home
  directory.

* If your system supports the "libqrencode" library, you will be shown a QRCode
  that you can scan using the Android/iPhone "Google Authenticator" application.

* If your system does not have this library, you have to manually enter the
  alphanumeric secret key into the Android/iPhone "Google Authenticator" application.

* In either case, after you have added the key, click-and-hold until the context
  menu shows. Then check that the key's verification value matches.

* Each time you log into your system, you will now be prompted for your
  TOTP code (timebased one-time-password) after having entered your normal user
  id and your normal UNIX account password.