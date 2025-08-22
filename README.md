# claws-mail-labels
by Morgan Aldridge <morgant@makkintosshu.com>

## OVERVIEW

`claws-mail-labels` is a multi-function 'Action' script for [Claws Mail](https://claws-mail.org/) to aid in managing email message 'labels' (tags, essentially) within Gmail and/or Google Workspace IMAP accounts. It utilizes [mairix](https://github.com/vandry/mairix) for performant indexing & searching email messages in the claws-mail(1) local IMAP cache, bending the rules a bit to perform fast re-indexing of individual labels.

**IMPORTANT:** _This project is very much a work-in-progress and developed specifically for my needs. Use at your own risk!_

### Prerequisites

* perl(1)
* mairix(1)
* An IMAP account synchronized for offline use
* Claws Mail 'Actions' configured to execute `claws-mail-labels`:
    * "Show Message Labels": `~/libexec/claws-mail-actions -f %f -l`
    * "Reindex Message Labels": `~/libexec/claws-mail-actions -f %f -r`

#### Suggested

* [zenity](https://gitlab.gnome.org/GNOME/zenity)
* Fast storage device to improve message indexing/re-indexing time

### Limitations

* It can only search for labels of a single message
* It only supports IMAP accounts (though could be modified to support POP & local MH accounts)
* It cannot find messages that have not yet been downloaded from the IMAP server and stored locally
* It cannot [yet] search across multiple accounts

### Directory Structure

```
~/.claws-mail/
  labelscache/
    <server>/
      <mailbox>/
        mairixrc
        mairixrc-reindex
        mairixdb
      .../
    ../
```

## USAGE

### Initialize Mailbox

One can initialize `claws-mail-labels`'s mairix(1) configuration files for Google Gmail/Workspace IMAP account by executing (replacing `<mailbox>` with the email address, e.g. "some-fake-address@gmail.com"):

```
~/libexec/claws-mail-labels -m <mailbox>
```

### Mailbox Full Index

Once a mailbox has been initialized, one can perform a full index by executing (again, replacing `<mailbox` with the email address, e.g. "some-fake-address@gmail.com"):

```
~/libexec/claws-mail-labels -m <mailbox> -i
```

### Configure Claws Mail Actions

#### Message Labels/Show

Add a "Shell command" Action:

* **Menu name:** `Message Labels/Show`
* **Command:** `~/libexec/claws-mail-labels -f %f -l`

##### With Zenity

If you'd like a nicer GUI display, use the following command instead:

```
~/libexec/claws-mail-labels -f %f -l | zenity --list --title="Labels" --text="%f" --hide-header --column="Label"
```

#### Message Labels/Re-index

Add a "Shell command" Action:

* **Menu name:** `Message Labels/Re-index`
* **Command:** `~/libexec/claws-mail-labels -f %f -r`

## REFERENCE

* [Claws Mail FAQ: Actions](https://claws-mail.org/faq/index.php/Actions)
* [Claws Mail FAQ: Using Claws Mail with other programs](https://claws-mail.org/faq/index.php/Using_Claws_Mail_with_other_programs)
    * [Claws Mail FAQ: Using Claws Mail with other programs (How can I use Claws Mail with mairix?](https://www.claws-mail.org/faq/index.php/Using_Claws_Mail_with_other_programs#How_can_I_use_Claws_Mail_with_mairix.3F)
* [Wikipedia: MH Message Handling System](https://en.wikipedia.org/wiki/MH_Message_Handling_System)
* [Zenity Manual](https://help.gnome.org/users/zenity/stable/index.html.en)

## LICENSE

Released under the [MIT License](LICENSE).
