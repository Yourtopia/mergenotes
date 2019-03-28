# Tapir release (v2.19.0)

## Goals

As we're very busy wrapping up the project which has funded CryptPad's development so far, this release is very small.
We've requested assistance improving the state of our translations, and received some very helpful contributions.

## Update notes

* We discovered that `container-start.sh` erroneously made a full copy of the `customize.dist` directory. This caused issues when updating to newer versions of CryptPad, where the customize directory was out of date with the rest of the instance.
  * if you have installed using docker, and have not customized your instance, you can safely remove everything in the `customize` directory **after having backed up your config.js file**. Your instance should fall back to using the default versions of those files instead of the outdated copies.
  * if you have customized your instance, you'll need to be more careful about cleaning up. Remove the files which you haven't modified, and compare your modified files against the latest versions of the default files. Merge your changes into the updated versions, and you should have an easier time updating in the future.

## Features

* We've rearranged the example server configuration file to make it easier to read and understand
* CryptPad now features a Russian translation which is 10% complete
* Our German translation has received a few fixes
* One of our Romanian colleagues has begun updating the Romanian translation, which is currently 39% complete
* **NOTE**: we're still learning our way around using weblate. We haven't given credit to these contributions because we're unsure if their authors want to be named. Going forward we'll figure out a system for giving proper credit where it is desired.

## Bug fixes

* As noted above, we've made some small changes to `container-start.sh` so that new docker images are correctly initialized

# Sloth release (v2.18.0)

## Goals

This release was developed during a busy period, so it contains fewer features than normal.
In particular we aimed to improve some aspects of our infrastructure, including finishing our deployment of _weblate_ for translations.

## Features

* Inserting `[TOC]` into the code editor while in markdown mode will render a table of contents in the preview pane.
* The code and slide editors also features some usability improvements pertaining to how tabs are handled, as it was possible to mix tabs and spaces unintentionally.
* The search bar in users drives now displays an _x_ while displaying search results, allowing users to easily return to the default view of their drive with a click.
* We've updated our translation guide to describe our new policies and procedures for translating CryptPad.
* We've added some additional features to our debugging application to help some users that reported difficulty finding documents in the history of their CryptDrives.

## Bug fixes

* We discovered that some additional validation we'd applied to document hashes had falsely identified some old URLs as invalid, and updated the validation to correctly account for those edge cases.
* We noticed that it was not possible to use arrow keys to navigate within some inputs in the drive, and fixed the issue.
* We also realized that some values were not correctly initialized for new accounts, and restored the intended behaviour.
* We've added a clientside migration to users' accounts to remove some duplicated values, making drives take up slightly less space over time.

# Raccoon release (v2.17.0)

## Goals

For this release we planned to resolve issues discovered in our beta release of encrypted spreadsheets, work towards providing an easier experience for contributors who wish to translate CryptPad, and resolve some minor usability issues that had been bothering us.

## Update notes

* This release introduces a new clientside dependency. Run `bower update` to install `requirejs-plugins`.
* We investigated using [Weblate](https://weblate.org/) for translating CryptPad, but in order to do so we have to migrate from our current translation format (Javascript files) to JSON. Administrators running recent version of CryptPad shouldn't have any trouble using the new system as long as they have not modified their translation files directly. Extensions to the translation dictionaries present in `/customize/translations/` should continue to work as expected. Anyone experiencing difficulty upgrading from older version of CryptPad to 2.17.0 can visit our chat channel for advice on how to proceed.

## Features

* We've received some updates from some of our German-speaking contributors to our Deutsch translation.
* We now perform more strict validation for the secret values encoded after the hash, since one of our users discovered that CryptPad failed silently when provided with an invalid hash.
* As requested, the CryptDrive now displays a lock icon for password protected pads.
* When you click 'Show in folder' from the _search_ or _recent pads_ interface, the selected file will be at the top of the screen. Previously the file was selected, but we didn't scroll to its location in the resulting folder, so it could be out of view if that folder had many files.
* We've tweaked the styles of some of the rendered Markdown in both our code and slide editors.
* Finally, we've added the same _pad creation screen_ to our spreadsheet editor as is normally present within our other editors. This will allow users to mark a spreadsheet as _owned_ (allowing them to delete it at a later time) and as having a pre-set expiration time.

## Bug fixes

* Very long words and lines are now wrapped correctly in the Kanban app.
* The rest of the bug fixes for this release were all applied to the spreadsheet editor:
  * Spreadsheets with additional worksheets were prone to errors caused when some clients did not receive instructions to update the identifier for a worksheet. This caused those spreadsheets to fail to load entirely.
  * We have added two buttons to the spreadsheet editor's app toolbar:
    * a _properties_ button like those on our other editors, to provide basic information about the document
    * an _import_ button, to process exported documents. Unlike our other import buttons, the spreadsheet editor is currently limited to importing when you are the only editor present in the session.
  * We've resolved some errors in how the history of a spreadsheet was counted against user quotas. Similarly, we've made sure to delete some extraneous information associated with spreadsheets when they are deleted from users' CryptDrives.
  * In the event of a server error, the spreadsheet editor will lock itself and proceed in read-only mode

# Quokka release (v2.16.0)

## Goals

We set aside an additional week for this release in order to deploy _encrypted spreadsheets_, which we've been working toward for a long time.
This feature combines our usual focus on privacy with OnlyOffice's spreadsheet editor.

At least for this first release we're still considering this functionality to be **highly experimental**.
We've done our best to make this new application fun and easy to use, however, it will still require a lot of work before it supports all the features that you can expect from our other editors.
We welcome you to try it out and report any difficulties you encounter, though you may want to wait before you start using it for all your financial documents.

## Update notes

* OnlyOffice requires more lax Content Security Policy headers than the rest of the platform. Compare your configuration against `config.example.js`.
* If you are running a customized `application_config.js`, you may need to update `availablePadTypes` and `registeredOnlyTypes`. See [the wiki](https://github.com/xwiki-labs/cryptpad/wiki/Application-config) for more details.
* In addition to a few serverside changes for the new spreadsheet editor, this release fixes a bug that affected system administrators who had set custom limits for some users and disabled communication with our payment server. Restart your server after updating for these changes to take effect.

## Features

* We've implemented a feature we call _ephemeral channels_, which we use for displaying other users' cursors in our rich text, code, and slide editors. Ephemeral channels behave exactly like our regular server messaging infrastructure except that no history is stored.
* We've added additional highlighting modes in our code editor for C, C++, Java, and Objective-C
* We've imposed a limit of five items for the table which displays upload progress, in order to keep it from taking up too much space on the screen when users upload many files in one session.

## Bugfixes

* [@3n2pS3P5kG23S96yxRbUHAZajuH2F](https://github.com/3n2pS3P5kG23S96yxRbUHAZajuH2F) reported an issue shortly after our last release which threw an error if our feedback API was disabled. The fix was on our master branch, but now it will be properly tagged.
* We noticed an issue in our code editor where imported .md files were interpreted as text, instead of markdown. This caused the preview pane to stop working.
* We also discovered an issue which had broken our CryptDrive import function, but as far as we know it did not affect any users. It should be working as intended now.
* Unfortunately, we don't do a lot of testing on Internet Explorer 11, but one of our users was kind enough to report an error. We tracked down a few uses of APIs which do not exist on IE11, and replaced them with compatible functions, so now users of IE11 will be able to enjoy CryptPad once more.

# Pademelon release (v2.15.0)

## Goals

For this release we planned to improve upon last release's introduction of the display of other users' cursors in our code and slide editors by adding the same functionality to our rich text editor.

Beyond just producing software, the CryptPad team has also begun to produce peer-reviewed papers.
We have previously published [Private Document Editing with Some Trust](https://dl.acm.org/citation.cfm?doid=3209280.3209535) as a part of the 2018 proceedings of the ACM Symposium on Document Engineering.
We have recently been accepted for publication as a part of [HCI-CPT](http://2019.hci.international/hci-cpt): the first international conference on HCI (Human Computer Interaction) for cybersecurity, privacy and trust.
In preparation for this publication we've begun to collect additional usage data in order to inform the wider community of our findings regarding usability of cryptography-based collaboration systems.

## Update notes

* Updating to version 2.15.0 from 2.14.0 should only require that update to the latest clientside code via git, and update any cache-busting parameters you've set.
* Several of our third-party clientside dependencies have been updated, and you may optionally run `bower update` to receive their latest versions.
* As explained above, we have added a number of new keys to our existing feedback system. The new keys are detailed below
  * HOME_SUPPORT_CRYPTPAD informs us when users discover our opencollective campaign from the CryptPad home page
  * UPGRADE_ACCOUNT informs us when someone clicks the upgrade account button from their CryptDrive or settings page
  * SUPPORT_CRYPTPAD is not active on our CryptPad instance, since this key is only sent when clicking the _donate button_ which is shown when upgraded accounts are disabled
  * DELETE_ACCOUNT_AUTOMATIC informs us when somebody deletes their account automatically from the settings page. Automatic account deletion is only available for accounts created since version 1.29.0
  * DELETE_ACCOUNT_MANUAL informs us when a user generates the proof of their account ownership which is required for manual account deletion. This feature is available only for accounts predating version 1.29.0
  * OWNED_DRIVE_MIGRATION informs us when a user migrates their CryptDrive from our legacy format (which does not support automatic deletion) to our newer format (which does) via the settings page
  * PASSWORD_CHANGED informs us when a user changes their password from the settings page
  * NO_WEBRTC informs us when a users browser does not support WebRTC at all via a crude test which never actually runs any WebRTC-based code
  * SUBSCRIPTION_BUTTON informs us when a user navigates to our paid account administration panel from their settings page
  * LOGOUT_EVERYWHERE informs us when a user executes the command to log out of their account on all remote devices from the settings page
* We've implemented the ability to configure which applications are available on a particular CryptPad instance via `cryptpad/customize/application_config.js`. Two arrays (`config.availablePadTypes` and `config.registeredOnlyTypes`) define which applications are available to everyone, and which applications are available to registered users. Due to a bug which was discovered, this behaviour is incorrect for our encrypted file viewer, and as a result encrypted files cannot currently be disabled. This will be addressed in our next release.

## Features

* Our rich text editor now displays other users' cursors when editing with a group. Preferences for this behaviour can be defined via the settings page.
* Links in our rich text editor can now be clicked more easily, as a small tooltip with a clickable link will be displayed above the editable link in the document.
* Users who wish to be notified of spelling errors in their rich text pads can enable spellcheck via the settings page.
* As noted above, various pad types can be disabled by instance administrators via `customize/application_config.js`.
* We've enabled a feature in the settings page which will migrate users' CryptDrive from our legacy format to our latest format (which supports automatic deletion). Only users with accounts dating back to version 1.29.0 will notice any difference.
* We've worked to improve some usability issues presented by the interaction of _owned files_ and _shared folders_. Since only the owner of an owned document can delete it the owner must keep a record of that document in their CryptDrive even if they place it in a shared folder (where someone else could delete it while they are offline). As such, owned documents were always copied to shared folders instead of being moved, and this proliferation of copies made it more difficult for users to organize their CryptDrives. Duplicated owned documents which are kept in your CryptDrive can now be hidden via the settings page. If those files are removed from a shared folder by another user, the hidden duplicate will be revealed in the root of your CryptDrive's tree.
* Finally, we've implemented the ability to copy documents to multiple shared folders via an entry in the right-click menu for any such document.

## Bugfixes

* We've improved the styles for displaying other users' cursors in the code and slide editors to avoid moving your view of the text when someone else highlights it.
* We've also changed some of the logic for how often other users' cursors are updated and displayed, so as to maximize the accuracy of their position and not show incorrect placements while you are typing.
* We fixed a bug which caused errors while loading your CryptDrive after a shared folder had been deleted.

# Opossum release (v2.14.0)

## Goals

For this release we chose to focus on our in-pad chat functionality and the ability to show your cursor's position to other users in the same pad.

## Update notes

* We've released an updated version of a serverside dependency: `chainpad-server`
  * this addresses a recently introduced bug which is capable of sending more history than clients require under certain circumstances
  * to use this updated dependency, run `npm update` and restart your server

## Features

* Our code editor is now capable of displaying other user's cursors within your view of the document.
  * this is enabled by default, but you can choose not to share your own cursor, and to disable the display of other users' cursors in your document
  * your initial color is chosen randomly, but you can choose any color you like within the settings page alongside the other configuration options for cursors
* After some consideration, we have chosen to change the permissions around the chat functionality embedded within every pad.
  * previously we had allowed viewers to participate in chat, even though they could not change the document.
  * we decided that this was counter-intuitive
  * in the event of an XSS vulnerability it could be used as a vector for privilege escalation
  * as such, we have modified our embedded chat functionality to only allow editors to participate
  * this change is not backwards-compatible, and so the embedded chat boxes will have dropped their older history
    * our assumption is that this will be an improvement for the majority of our users, and that it's fairly safe to drop older history given that chat is a relatively new feature
    * if this has affected you in an adverse way, the information is still accessible, and you can contact us if you need a way to recover that information
* Finally, it is now possible to print the rendered markdown content in our code editor, thanks to a contribution from [@joldie](https://github.com/joldie)

# Numbat release (v2.13.0)

## Goals

This release features long-awaited improvements to our Rich Text Pad.
This work was done over a short period, and we're releasing it now so that users can take advantage of the improvements as soon as possible.

## Update notes

* We've fixed a bug related to chat via an update to our messaging server. To install the update, run `npm update`. This server improvement is backwards compatible, so you can update your clientside or serverside dependencies in either order. Restart your server for the changes to take effect.
* You can run `bower update` in order to take advantage of the latest clientside dependencies. Depending on when you last updated you may benefit from updates to Codemirror or some other clientside libraries.

## Features

* We've refactored a great deal of CryptPad's Remote Procedure Call mechanisms related to chat. This should simplify CryptPad and make potential bugs less likely to occur.

## Bugfixes

* The behaviour of the cursor in our rich text editor has been greatly improved. Your experience when collaboratively editing should be noticeably better.
* Characters inserted into rich text pads were sometimes dropped due to a race condition between CKEditor and ChainPad, but this asynchronous behaviour has been resolved. As such the editor should be much more reliable.
* Deleting chat history from the server now removes it from your chat interface and that of remote messengers, where it previously would require a reload of the interface to see the correct chat history.
* We now correctly set owners of a shared chat channel such that either chat participant in a one-to-one room can delete the history.
* If you request history with a `lastKnownHash` which is not in the history, the server informs you that it is not there via a direct message. Clients fall back to a classic full retreival of the history. Previously this would fail, and print a message to the server's stdout.
* Firefox users may have noticed that when they clicked the dropdown menus for styles in the CKEditor toolbar, their scrollbar would jump to the top of the document. Their scroll position is now preserved in cases where it would previously have been disrupted.

# Manatee release (v2.12.0)

## Goals

For this release we aimed to address usability concerns in our Rich Text Pad, since it's our most widely used application. During this time we also received an unexpected security disclusure which we treated as being top priority.

## Update notes

* This release addresses an XSS vulnerability in our chat interface which was discovered thanks to [cyberpunky](https://twitter.com/cyberpunkych). In older versions of CryptPad, only the /contacts/ app was affected. In newer versions which feature the embedded chat interface in pads, it is possible to leverage this vulnerability against other users in the same pad. Due to our [Sandboxed iframe technique](https://blog.cryptpad.fr/2017/08/30/CryptPad-s-new-Secure-Cross-Domain-Iframe/), this vulnerability does not permit an attacker to compromise concurrent editor's accounts, as their user keys are never accessible within the scope of the domain which was subject to exploitation. However, since the chat functionality is available to viewers as well as editors, it could be leveraged to gain access to the keys which permit modification of the document. Despite this limitation, creative attackers could leverage the front-end code to perform phishing attacks, or other forms of social engineering to trick users into handing over their credentials.  We recommend that administrators of affected CryptPad instances upgrade to this version as soon as possible. Once more, we'd like to thank _cyberpunky_ for their effort to discover the issue, and for reporting the issue to us in private so that we could fix it without putting our users at risk.
* On a lighter note, this release features a server-side dependency update which fixes a non-critical bug in our websocket protocol. New users joining a channel which had never been vacated by all its users since its creation would receive the full history instead of only the latest state. To deploy the fix, run `npm update` and restart your server.

## Bugfixes

* As noted above, this release fixes an XSS vulnerability.
* We realized that each shared-folder in your CryptDrive was using a separate websocket connection to the server instead of routing over the existing websocket connection. This has been fixed.
* We've improved our _cursor-recovery script_ in the Rich Text Pad app to make it more resilient. In cases where the text changed in two places within one node of the document, your cursor could be displaced. It should behave more predictably now.
* Another problem in the Rich Text Pad app could lead to conflicts between users when one reverted the change of another. Conflicts should now resolve in a predictable fashion.
* If you were using the Rich Text Pad in its reduced-width mode (available via your /settings/ page), it was possible to scroll down beyond the white, paper-like styles of the document into an un-styled area of the page. This has been addressed.
* We discovered that the export functionality for Rich Text Pads was not working due to a semantic difference in a conditional test in Chrome. Export within Chrome should work once more, however, there are [serious privacy risks within Chrome/Chromium](https://reddit.com/r/ProtonMail/comments/9yl94k/never_connect_to_protonmail_using_chrome/) and we recommend that you consider using a more privacy-friendly browser.

## What's new

* The home page now features a badge advertising the fact that CryptPad is now a winner of the NGI award for _Privacy and Trust-enhanced technologies_. You can follow the link to our blog post which contains more information.
* It is now possible to directly download uploaded files from your CryptDrive without opening a new tab, making your content available more quickly.

# Lemur release (v2.11.0)

## Goals

This release continued the work on better customization features for community instances. We also worked on usability improvements and UI issues.

## Update notes

* This is a simple release. Just download the latest commits and update your cache-busting string.
* Customized instances may require additionnal changes in order to make customization easier to maintain in the future.
  * The static pages content (home page, FAQ, contact, privacy, etc.) has been moved from `./customize.dist/pages.js` to a `./customize.dist/pages/` directory, containing one file per page. This new structure allows administrators to override only some pages instead of all the pages at once.
  * To override a page, just make a copy of its .js file from `./customize.dist/pages` to a `./customize/pages` and make your changes.

## Features

* We've replaced our Font Awesome application icons with new custom icons. The new icons should be closer to the goals of the apps.
* We've cancelled the Ctrl+S shortcut from the browser for saving the page. In CryptPad, the result of the browser save was not usable and the content of the pads is automatically saved.
* As explained above, we've made it easier to customize some specific static pages instead of overriding all of them.
* Our Markdown renderer should display tables in a nicer and cleaner way (*Code* and *Slide* applications).
* The font size in the code and slide editors can now be changed from the *Settings* page.
* We've added a warning text to the CryptDrive export feature from the last release.

## Bugfixes

* We've found an issue causing some deleted characters to be inserted back in the document. It could happen when a least one member of the session had the tab not focused in their browser.
* We've fixed an issue with our code for detecting small (or zoomed) screens in several part of our UI. This will hide some unnecessary elements of the interface at first load and free space for the actual content of the pad.
* The "present" mode in the Slide application will no longer display the toolbar.
* We've fixed an issue in the *Pad* application where the font could be reset to Arial when making a new paragraph.
* The full CryptDrive export no longer stops when trying to export a very old poll.

# Koala release (v2.10.0)

## Goals

This release continued to improve our _shared folder_ functionality, addressed user concerns about data portability, and implemented various features for customization for different CryptPad instances.

## Update notes

* This release features updates to client-side dependencies. Run `bower update` to update the following:
  * netflux-websocket
  * chainpad-netflux
* we've added a new field (`fileHost`) in `config.example.js`. It informs clientside code what domain they should use when fetching encrypted blobs.
* Administrators can now do more to customize their CryptPad server, most notably via the ability to override specific translations. For example, the home page now features a short message which, by default, says that the server is a community-hosted instance of the CryptPad open-source project. On CryptPad.fr, we have replaced this text to talk about our organization. You can do the same by modifying files in `cryptpad/customize/translations/`, like so:

```
define(['/common/translations/messages.js'], function (Messages) {
    // Replace the existing keys in your copied file here:
    Messages.home_host = "CryptPad.fr is the official instance of the open-source CryptPad project. It is administered by XWiki SAS, the employee-owned French company which created and maintains the product.";

    return Messages;
});
```

Simply change the text assigned to `home_host` with a blurb about your own organization. We'll update the wiki soon with more info about customization.

### Features

* We've updated our features page to indicate what users get by purchasing a premium account. You can visit our accounts page directly from this list with the click of a button.
* We've updated our home page to explain more about what CryptPad is.
* As mentioned above, we've made all of our translation files overrideable.
* We've made it easier to get your data out of CryptPad, by implementing a complete export of your CryptDrive's content as a zip file. This feature is available on the _settings page_.
* Shared folders now support password protection.

### Bugfixes

* We fixed an issue which affected users of our Kanban application, which caused the color picker to pop up and get in the way at inopportune moments.
* We found that when a CryptPad code editor tab finished loading in the background, when it was focused, the markdown preview pane would be blank. We've added a check to try to re-draw the pane in these circumstances.
* We noticed that anonymous users who used our in-pad chat app could not be distinguished when they both chatted at once. We now add a string at the end of their name which makes it possible to distinguish them.
* We've updated an internal library (cryptget) such that it correctly tears down realtime sessions after connecting and loading content from the server.
  * We also added better error handling.
* At some point in the last few releases we broke export of media-tags in rich text pads. They should be back to normal now.
* Media-Tags also use the configurable value `fileHost` to construct absolute URLs, instead of using relative URLs to the server.
* Tall dropdown menus no longer use scrollbars when they are displayed with enough space to display all options.
* Chrome browser seemed to display our rich text editor correctly, except that no cursor was visible in empty documents. Users will now be able to see where their cursor is placed.
* It was possible for disconnected users' browsers to enter a bad state after reconnecting. This resulted in that pad being inaccessible until they relaunched their browser. This bad state is now detected and mitigated.
* Tags for documents in the CryptDrive were stopped functioning correctly as of the last few releases. This release fixes this bug.

# Jerboa release (v2.9.0)

## Goals

Since last release introduced several big features, this release was allocated towards usability improvements largely related to those new features.

## Update notes

This is a simple release. Just deploy the latest source.

### Features

* At a user's request, we now highlight annotated code blocks according to their language's syntax
* Shared folders can now be viewed by unregistered users (in read-only mode)
* The authentication process that we use for handling accounts has been improved so as to tolerate very slow networks more effectively
* The chat system embedded within pads can now optionally use the browser's system notifications API

### Bugfixes

* We found and fixed a race condition when initializing two tabs at once, which could leave one of the tabs in a broken state

# Ibis release (v2.8.0)

## Goals

We've been making use of some hidden features for a while, to make sure that they were safe to deploy.
This release, we worked on making _contextual chat_ and _shared folders_ available to everyone.

## Update notes

* run `bower update` to download an updated version of _marked.js_

### Features

* Our kanban application now features a much more consistent and flexible colorpicker, thanks to @MTRNord (https://github.com/MTRNord)
* File upload dialogs now allow you to upload multiple files at once
* Updated German translations thanks to [b3yond](https://github.com/b3yond/)
* An explicit pad storage policy to better suit different privacy constraints
  * _import local pads_ at login time is no longer default
* An embedded chat room in every pad, so you can work alongside your fellow editors more easily
* Promotion of our [crowdfunding campaign](https://opencollective.com/cryptpad), including a button on the home page, and a one-time dialog for users

### Bug fixes

* Updating our markdown library resolved an issue which incorrectly rendered links containing parentheses.
* We discovered an issue logging in with _very old_ credentials which were initialized without a public key. We now regenerate your keyring if you do not have public keys stored in association with your account.
* We found another bug in our login process; under certain conditions the terminating function could be called more than once.

# Hedgehog release (v2.7.0)

## Goals

This release overlapped with the publication and presentation of a paper written about CryptPad's architecture.
As such, we didn't plan for any very ambitious new features, and instead focused on bug fixes and some new workflows.

## Update notes

This is a fairly simple release. Just download the latest commits and update your cache-busting string.

### Features

* In order to address some privacy concerns, we've changed CryptPad such that pads are not immediately stored in your CryptDrive as soon as you open them. Instead, users are presented with a prompt in the bottom-right corner which asks them whether they'd like to store it manually. Alternatively, you can use your settings page to revert to the old automatic behaviour, or choose not to store, and to never be asked.
* It was brought to our attention that it was possible to upload base64-encoded images in the rich text editor. These images had a negative performance impact on such pads. From now on, if these images are detected in a pad, users are prompted to run a migration to convert them to uploaded (and encrypted) files.
* We've added a progress bar which is displayed while you are loading a pad, as we found that it was not very clear whether large pads were loading, or if they had become unresponsive due to a bug.
* We've added an option to allow users to right-click uploaded files wherever they appear, and to store that file in their CryptDrive.
* We've improved the dialog which is used to modify the properties of encrypted media embedded within rich text pads.

### Bug fixes

* Due to a particularly disastrous bug in Chrome 68 which was unfortunately beyond our power to fix, we've added a warning for anyone affected by that bug to let them know the cause.
* We've increased the module loading timeout value used by requirejs in our sharedWorker implementation to match the value used by the rest of CryptPad.

# Gibbon release (v2.6.0)

## Goals

For this release we focused on deploying two very large changes in CryptPad.
For one, we'd worked on a large refactoring of the system we use to compile CSS from LESS, so as to make it more efficient.
Secondly, we reworked the architecture we use for implementing the CryptDrive functionality, so as to integrate support for shared folders.

## Update notes

To test the _shared folders_ functionality, users can run the following command in their browser console:

`localStorage.CryptPad_SF = "1";`

Alternatively, if the instance administrator would like to enable shared folders for all users, they can do so via their `/customize/application_config.js` file, by adding the following line:

`config.disableSharedFolders = true;`

### Features

* As mentioned in the _goals_ for this release, we've merged in the work done to drastically improve performance when compiling styles. The system features documentation for anyone interested in understanding how it works.
* We've refactored the APIs used to interact with your CryptDrive, implementing a single interface with which applications can interact, which then manages any number of sub-objects each representing a shared folder. Shared folders are still disabled by default. See the _Update notes_ section for more information.
* The home page now features the same footer which has been displayed on all other information pages until now.
* We've added a slightly nicer spinner icon on loading pages.
* We've created a custom font _cp-tools_ for our custom-designed icons

### Bug fixes

* We've accepted a pull request implementing serverside support for moving files across different drives, for system administrators hosting CryptPad on systems which segregate folders on different partitions.
* We've addressed a report of an edge case in CryptPad's user password change logic which could cause users to delete their accounts.

# Fossa release (v2.5.0)

## Goals

This release took longer than usual - three weeks instead of two - due to our plans involving a complete redesign of how login and registration function.
Any time we rework a critical system within CryptPad we're very cautious about deploying it, however, this update should bring considerable value for users.
From now on, users will be able to change their passwords without losing access to their old data, however, this is very different from _password recovery_.
While we will still be unable to help you if you have forgotten your password, this update will address our inability up until this point to change your password in the event that it has been compromised in some way.

## Update notes

* v2.5.0 uses newly released features in a clientside dependency ([chainpad-netflux](https://github.com/xwiki-labs/chainpad-netflux/releases/tag/0.7.2)). Run `bower update` to make sure you have the latest version.
* Update your server config to serve /block/ with maxAge 0d, if you are using a reverse proxy, or docker. `cryptpad/docs/example.nginx.conf` has been updated to include an example.
* Restart your server after updating.
* We have added a new feedback key, `NO_CSS_VARIABLES`, in order to diagnose how many of our clients support the CSS3 functionality.

### Features

* v2.5.0 introduces support for what we have called _modern users_.
  * New registrations will use the new APIs that we've built to facillitate the ability to change your account password.
  * _Legacy registrations_ will continue to function as they always have.
  * Changing your password (via the settings page) will migrate old user accounts to the new system.
  * We'll publish a blog post in the coming weeks to explain in depth how this functionality is implemented.
* The _kanban_ application now features support for export and import of your project data.
* This release features minor improvements to the _Deutsch_ translation

### Bug fixes

* We noticed that if you entered credentials for registration, and cancelled the displayed prompt informing you that such a user was already registered, the registration interface would not unlock for further interaction. This has been fixed.
* We found that on very slow connections, or when users opened pads in Firefox without focusing the tab, requirejs would fail to load dependencies before timing out. We've increased the timeout period by a factor of ten to address such cases.

# Echidna release (v2.4.0)

## Goals

For version 2.4.0 we chose to use our time to address difficulties that some users had, and to release some features which have been in development for some time. With the recent release of the _password-protected-pads_ feature, some users desired to be able to change the passwords that they'd already set, or to add a password to a pad retroactively. Other users wanted to recover information that had accidentally been deleted from their pads, but found that the history feature was difficult to use on networks with poor connectivity. Others still found that loading pads in general was too slow.

## Update notes

* We have released new clientside dependencies, so server administrators will need to run `bower update`
* This release also depends on new serverside dependencies, so administrators will also need to run `npm update`
* This release (optionally) takes advantage of Webworker APIs, so administrators may need to update their Content Security Headers to include worker-src (and child-src for safari)
  * see cryptpad/docs/example.nginx.conf for more details regarding configuration for nginx as a reverse proxy
  * to enable webworkers as an experimental feature, add `AppConfig.disableWorkers = false;` to your `cryptpad/customize/application-config.js`
* Finally, administrators will need to restart their servers after updating, as clients will require new functionality

## What's new

### Features

* CryptPad now takes advantage of some very modern browser APIs
  * Shared Workers allow common tasks for all CryptPad editors to be handled by a single background process which runs in the background. This results in better performance savings for anyone using multiple editors at once in different tabs
  * Webworkers are used in situations where shared workers are not supported, for most of the same tasks. They are not shared amongst different tabs, but can allow for a more responsive user experience since some heavy commands will be run in the background
  * Not all browsers feature complete support for webworkers. For cases where they are not supported at all, or where cryptographic APIs are not supported within their context (https://developer.microsoft.com/en-us/microsoft-edge/platform/issues/7607496/), we fall back to an asynchronous context in the same thread
* Pads with no password can now be updated to include a password, and pads with a password can have their passwords changed
  * right-click on the pad in question, and see its properties. The following dialog will present the option to change its password
  * changing a pad's password will remove its history
* Accessing a pad's history used to require that clients fetch the entire history of the pad before they could view any of it. History retrieval is now done on an on-demand basis, approximately 100 versions of the pad at a time
  * this also features an updated UI with a slider
* We've refactored our whiteboard application to be compatible with our internal framework. As a result, it will be easier to maintain and will have all the same features as the other editors built with the same framework
* We've defined some new server-side features which will allow clients to change their user passwords in a coming release
* We've updated our messaging server implementation
  * the aspect of the server which stores and distributes history has been untangled from the aspect which tracks user lists and broadcasts messages
  * the server will now store the time when each message was received, so as to be able to allow users to view the time of edits in a later release

### Bug fixes

* When a user tries to register, but enters credentials which have already been used for that CryptPad instance, we prompt them to log in as that user. We discovered that the login had stopped working at some point. This has been fixed
* Server administrators may have seen warnings from npm when attempting to update. We have fixed invalid entries and added missing entries where appropriate such that there are no more warnings
* Static info pages have been restyled to be more responsive, thanks to @CatalinScr
* Support for friend requests in pads with version 0 hashes has been repaired
* We noticed a regression in how default titles for pads were suggested, and have implemented the intended behaviour

# Donkey release (v2.3.0)

## Goals

For this release we wanted to deploy some new features related to our encrypted file functionality.

## Update notes

* new clientside dependencies. run `bower update`
* new serverside APIs. Restart your server

## What's new

### Features

* When uploading files to your CryptDrive or a pad, users will now be prompted to protect the file with a password (in addition to some random data)
  * this adds an additional layer of security in case a third party gains access to the file's link, but not the password.
* Users are also able to claim an encrypted file as their own, allowing them the option to delete it from the server at a later date.
* We've refactored the Media-Tag library to be much smaller and easier to use.

### Bug fixes

* When setting a title for a pad which was created from a template, titles were not correctly inferred from the content of a document. This has been fixed.
* We discovered that users who had installed _AdBlock Plus_ and configured it to **Block social media icons tracking** were unable to use the _share menu_ to construct alternative links to the same pad, but with different attributes. We have worked around the problem.
* Admins who had configured their CryptPad instance to use custom icons for applications in the CryptDrive may have noticed that the same icons were not used on the home page. We've fixed this such that the same icons will be used everywhere
* We have also updated the icon for the Kanban app to a more appropriate symbol
* We found that the download button in the _file_ app was downloading the user's avatar, instead of the correct encrypted file embedded in the page. We've since fixed this

# Coati release (v2.2.0)

## Goals

For this release we wanted to continue our efforts towards improving CryptPad usability. We've also added a new Kanban application which was in its final stage for quite some time.

## What's new

### Features

* We've added a new kanban application!
  * You can create boards, add items to those boards and move items from one board to another.
  * It includes almost all the features seen in the other apps: templates, password protection, history, read-only, etc.
  * Kanban can be shared and used collaboratively.
  * This new app was prototyped by @ldubost, and based on [jkanban](https://github.com/riktar/jkanban) by @riktar
* We've improved our tagging feature.
  * When you want to add tags to a pad, you will see suggestions based on the tags you've already used
  * There is a new *Tags* category in CryptDrive for logged in users. It shows all the tags you've used in your pads and their number of use.
* In the Poll application, the line where your cursor is located will be highlighted so that you can see easily which option you're looking at.

### Bug fixes

* We've fixed two interface bugs in the Share menu which made it difficult to change the access rights for the link (edit or read-only) in some cases.
* A bug introduced in the previous version prevented loading of the drive if it contained some content from an alpha version of CryptPad.
* Some parts of our UI were using CSS values not supported by all browsers.
* Some pads created more than one year ago were not loading properly.

# Badger release (v2.1.0)

## Goals

This is a small release due to a surplus of holidays in France during the Month of May.
We'd been planning to implement _Password-protected Pads_ for a long time, but we had not found a good opportunity to do so within our roadmap.
After a generous donation from one of our users who considered this a critical feature, we were able to dedicate some resources towards delivering it to all of our users.

## Update notes

This release depends on new APIs in our `chainpad-crypto` module. Additionally, we have fixed a critical bug in `chainpad-listmap`.
Admins will need to update their clientside dependencies with `bower update` when deploying.

## What's new

### For Users

* Users can now protect their new pads with a password.
  * This makes it safer to share very sensitive links over email or messengers, as anyone who gains access to the link will still need the password to edit or view pads.
  * This also protects your pads against browsers which share your history across devices via the cloud.
  * We recommend that you share passwords using a different messenger tool.
  * Passwords cannot be set or changed after creation time (yet), so we also recommend you consider how secure your pad will need to be when you create it.
* Password protection coincides with an update to our URL encoding scheme. URLs are generally quite a bit shorter than before, while offering more functionality.
* Existing users will have a short delay the first time that they load this version of CryptPad, as it contains a migration of their CryptDrive's data format.
  * This migration is very tolerant of interuptions, so if you need to close your browser while it is in progress, you are free to do so.

### For Admins

* Admins can look forward to happier users!

### Bug fixes

* data loss when reconnecting in our poll app
* we've fixed a minor bug in our poll app which caused an increasing number of tooltips to be added to elements

# Alpaca release (v2.0.0)

This is the first release of our 2.0 cycle.

After careful consideration we've decided to name each release in this cycle after a cute animal, iterating through the letters of the Latin alphabet from A to Z.

## Goals

We wanted to update CryptPad's appearance once more, adopting the colors from our logo throughout more of its interface.

## Update notes

This release coincides with the introduction of new APIs in ChainPad, so we recommend that adminstrators update their clientside dependencies by running `bower update`.

As recent updates have updated serverside dependencies, we also recommend that you run `npm update` and _restart your server_.

## What's new

### For Users

* CryptPad 2.0.0 features a complete German-language translation, thanks to contributions from @polx, @kpcyrd, and @micealachman
* CryptPad has a new look!
  * we've adopted the color scheme of our logo for more UI elements throughout CryptPad, on the loading screen and various dialogs
  * we've customized our checkboxes and radio buttons to match
  * we've updated the look of our pad creation screen to feature up to four templates per page, with tab and button navigation
  * tooltips have been made to match the dialogs on our pad creation screen
  * clients now store their usage of various templates in their CryptDrive, and rank templates by popularity in the pad creation screen
  * we no longer show usage tips on the loading screen
* Users who visit pads which have been deleted or otherwise do not exist are now prompted to redirect to their home page
* Our poll and whiteboard apps now use an in-house CSS framework to help us maintain consistency with the other applications

### For Admins

* we've updated the example configuration file (`config.example.js`) to no longer require a leading space before the domain, as we found it to be a common source of confusion. This will only affect newly generated config files.
* our webserver has been configured to support HTTP access of the client datastore, to facilitate scripts which parse and decrypt history without having to go through our websocket infrastructure
* we no longer use a single image for our favicon and our loading screen icon, allowing admins to customize either feature of their instance independently
* We've also moved the rest of the styles for the loading screen from `/common/` into `/customize.dist/`, 
* move loading screen implementation from `/common/` to `/customize.dist/`

## Bug fixes

* don't eat tab presses when focused on register button
* idempotent picker initialization
* CKEditor fixes
  * drag and drop text
  * media-tag movement integrated as CKEditor plugin
  * avoid media-tag flicker on updates
* set content type for the 404 page

# 1.29.0

## Goals

For this release we wanted to direct our effort towards improving user experience issues surrounding user accounts.

## Update notes

This release features breaking changes to some clientside dependencies. Administrators must make sure to deploy the
latest server with npm update before updating your clientside dependencies with bower update.

## What's new

* newly registered users are now able to delete their accounts automatically, along with any personal
 information which had been created:
  * ToDo list data is automatically deleted, along with user profiles
  * all of a user's owned pads are also removed immediately in their account deletion process
* users who predate account deletion will not benefit from automatic account deletion, since the server
  does not have sufficient knowledge to guarantee that the information they could request to have deleted is strictly
  their own. For this reason, we've started working on scripts for validating user requests, so as to enable manual
  deletion by the server administrator.
  * the script can be found in cryptpad/check-account-deletion.js, and it will be a part of an ongoing
    effort to improve administrator tooling for situations like this
* users who have not logged in, but wish to use their drive now see a ghost icon which they can use to create pads.
  We hope this makes it easier to get started as a new user.
* registered users who have saved templates in their drives can now use those templates at any time, rather than only
  using them to create new pads
* we've updated our file encryption code such that it does not interfere with other scripts which may be running at
  the same time (synchronous blocking, for those who are interested)
* we now validate message signatures clientside, except when they are coming from the history keeper because clients
  trust that the server has already validated those signatures

## Bug fixes

* we've removed some dependencies from our home page that were introduced when we updated to use bootstrap4
* we now import fontawesome as css, and not less, which saves processing time and saves room in our localStorage cache
* templates which do not have a 'type' attribute set are migrated such that the pads which are created with their
  content are valid
* thumbnail creation for pads is now disabled by default, due to poor performance
  * users can enable thumbnail creation in their settings page
* we've fixed a significant bug in how our server handles checkpoints (special patches in history which contain the
  entire pads content)
  * it was possible for two users to independently create checkpoints in close proximity while the document was in a
    forked state. New users joining while the session was in this state would get stuck on one side of the fork,
    and could lose data if the users on the opposing fork overrode their changes
* we've updated our tests, which have been failing for some time because their success conditions were no longer valid
* while trying to register a previously registered user, users could cancel the prompt to login as that user.
  If they did so, the registration form remained locked. This has been fixed.
