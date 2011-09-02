# Instant Message Privacy and Security

This short guide will explain how to set yourself up for encrypted instant messaging via any network by installing Pidgin and OTR ("Off-the-Record"). I'll write it to address the MS Windows user, but the principles apply to any platform. There's a section at the end for all you Mac and Linux users. If you're more exotic than that, I guess you know what you're doing. ;)

## Get Pidgin

[Pidgin](http://pidgin.im) is an alternative IM client that supports pretty much any protocol/network out there. That includes your basic ICQ/AIM/MSN but maybe most notably also Google Talk and Facebook chat. The latter use a protocol called XMPP a.k.a. Jabber.

For the record, there is a bunch of other IM clients out there. Let's focus on Pidgin for the moment because it is officially supported by the OTR developers. See "Other Clients/Platforms" below for alternatives.

Grab the latest version from [http://pidgin.im/](http://pidgin.im/) and install it. When started for the first time, it should prompt you with a friendly setup screen to enter the details to your IM accounts. You don't have to do all in one sweep, more can always be added later. See the help section on the Pidgin homepage for more info.

## Get and Activate the OTR Plugin

[Off-the-Record](http://www.cypherpunks.ca/otr/) is an add-on system that adds strong encryption on top of any plain old IM network. It is usually installed as a plugin to your IM client. Go to [http://www.cypherpunks.ca/otr/](http://www.cypherpunks.ca/otr/) and find the Pidgin plugin for Windows in the "Downloads" section. Download and install. :)

When you (re)start Pidgin, select "Plugins" from the "Tools" menu. Find the Off-the-Record plugin in the list and enable it.

Whenever you start a conversation now with someone who has OTR, your clients should automatically switch to an encrypted connection.
Actually, the first time this happens, Pidgin will take a moment to generate your OTR key. A window will pop up telling you about the progress; it takes a little moment.

## Verify Your Peers!

This is pretty important, but it also requires some effort from you. If the encryption you're using is supposed to be worth its salt, be sure that the person you're talking to is really them. The easiest way for this is the "Question & Answer" method. Start a conversation and select "Authenticate Buddy" from the "OTR" menu. Enter a question where you are confident that only your friend will know the answer. Enter the expected answer and send the request. Your buddy will have to type in the answer *exactly* the way you did, so make sure the question is precise.

## Video

There is a very nice video on YouTube showing the steps:

 * [(WATCH ON YOUTUBE)](http://www.youtube.com/watch?v=aV6-s9o9bVw)
 * [Another video's about how to install Pidgin on windows 7 and Adium on MacOSX with OTR](http://telekommunist.nl/tutorials/)

## Links

 * [Documentation from the OTR project](http://www.cypherpunks.ca/otr/index.php#docs)
 * [Pidgin help](http://pidgin.im/support/)

## Other Clients/Platforms

 * MacOS: [Adium](http://adium.im/) comes with OTR out of the box!
 * Pidgin also works on Linux and you should find it in your package manager, along with the OTR plugin.
 * [Miranda](http://www.miranda-im.org/) and [Trillian](http://www.trillian.im/) are other popular IM clients for Windows that have OTR plugins:
   * [OTR for Miranda](http://addons.miranda-im.org/details.php?action=viewfile&id=2644)
   * [OTR for Trillian](http://trillianotr.kittyfox.net/)
 * For those who already use IRC, [BitlBee](http://www.bitlbee.org/) presents your contacts as an IRC server and includes OTR support.
 * [Comprehensive list of OTR-enabled software](http://www.cypherpunks.ca/otr/software.php)
