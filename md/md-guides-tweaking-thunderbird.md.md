**Thunderbird**

Don't advertise version numbers or software:

 - Tools -> Options -> Advanced -> General -> Config Editor -> Right click and add a new String.  Preference Name is "general.useragent.override" and the value should be a blank string.

You probably don't want this option, but if you're curious about SMTP or extra paranoid, you can enable all the headers in a scroll box on message preview.

 - Tools -> Options -> Advanced -> General -> Config Editor -> Filter on 'show_headers' and set it to 2


**Enigmail**

Don't advertise version numbers or software:

 - In OpenPGP Preferences, Advanced: Uncheck 'Add Enigmail comment'
 - Tools -> Options -> Advanced -> General -> Config Editor -> Filter on 'addHeaders' -> Set to 'false' (double click)
 - Edit ~/.gnupg/gpg.conf and add the following at the bottom: no-emit-version
 - Alternatively, in OpenPGP Preferences, Advanced: Add the following addition paramaters: --no-emit-version