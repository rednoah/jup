# jup
The `jup` (**J**ava **Up**date) project aims to provide an easy, efficient and cross-platform install and auto-update framework for Java applications.


## Design Ideas
* Require JRE. Use System JRE? Use embedded JRE? How do we update the embedded JRE?
* Standalone CLI tool similar to `npm`? e.g. `jup install update.xml`
* Updates must consist of static files only, so that no server-side software is required and all requests can be served from CDNs
* Keep all application files from previous version and use them as local cache when downloading files for the new version. Download only new or changed files and hardlink existing files from the previous version.
* Restore execute permissions from `TAR` archives on extract, or use execute flag in update descriptor
* Separate jup.exe that requires [elevated permissions](http://stackoverflow.com/questions/2818179/how-to-force-my-net-app-to-run-as-administrator-on-windows-7) and can modify `Program Files` (Windows only)
* How can we update Mac app bundles installed in `~/Applications`? How do we get write access? [AuthorizationExecuteWithPrivileges](http://stackoverflow.com/questions/32488665/replacement-for-authorizationexecutewithprivileges-temporary-root-permission-fo)?
* Execute command via `MSI`? Can we build an `MSI` package that will download the latest version via `jup` on install?
* Sign XML update descriptor, maybe via an additional XML header attribute? e.g. `<?xml version="1.0" encoding="UTF-8" signature="A1B2C3D4F5" ?>` What about multiple signatures? Use separate `update.xml.signature` files? (side effect: if `update.xml` is large, we only need to check for changes in the small `*.signature` file to know if there's any updates)
* Should we use [XML Digital Signatures](https://docs.oracle.com/javase/6/docs/technotes/guides/security/xmldsig/XMLDigitalSignature.html)? Java has an API, but it it seems overly complex (to account for use cases we don't care about).
* Do target files need to be signed? If only the `update.xml` file is signed and contains `SHA-256` checksums for all target files. Can we trust target files just by checking `SHA-256` file integrity?
* Keep local modification of original file has not been changed for this update
* Send machine identifier with HTTP requests to estimate number of active users
* Post-update hook that can call an `*.msi` that was part of the update, so that we can update the updater itself, Software Panel information, Window Shortcuts, etc
* Same rationale as [Getdown](https://github.com/threerings/getdown/wiki/Rationale)


## Related Papers
* [Survivable Key Compromise in Software Update Systems](https://isis.poly.edu/~jcappos/papers/samuel_tuf_ccs_2010.pdf)
* [Package Management Security](https://isis.poly.edu/~jcappos/papers/cappos_pmsec_tr08-02.pdf)
* [A Look In the Mirror: Attacks on Package Managers](https://isis.poly.edu/~jcappos/papers/cappos_mirror_ccs_08.pdf)
* [Diplomat: Using Delegations to Protect Community Repositories](https://isis.poly.edu/~jcappos/papers/kuppusamy_nsdi_16.pdf)


## Related Projects
* https://sparkle-project.org/
* https://theupdateframework.github.io/
* http://jupidator.panayotis.com/
* https://sourceforge.net/projects/juf/
* https://github.com/edvin/fxlauncher
* https://github.com/threerings/getdown
* https://github.com/vinumeris/updatefx
* https://github.com/UrsKR/updates-r-simple
* http://www.oracle.com/technetwork/java/javase/javawebstart/
* http://stackoverflow.com/questions/34923513/auto-update-frameworks-for-java-applications


## Status
The Microsoft Store has thoroughly solved the problem on Windows. The Mac App Store and brew cask work reasonably well on Mac. Snap works on Ubuntu, and probably all the popular Linuxes, and maybe even BSD in the future. I recommend going with native package management instead of building update functionality into end-user software.
