test-workspace
==============

This repo contains an encrypted git repo inside it.  You can see the refs in plain text and the hashes of the various objects in the database, but all the object contents are encrypted using AES in CBC mode.  The IV for each object is generated randomly and embedded as the first 16 bytes of the value.

The only copy of the AES symmetric key exists in my browser's IndexedDB in another tab.  The tab that created this repo usies [js-git][] with a [js-github][] backend that writes to github using thier CORS enabled and proprietary [REST API][].

The AES key isn't stored in plain text even on my machine's local private storage.  It's encrypted with my RSA public key that was generated in the browser.  The RSA private key required to decrypt the AES symmetric key is also stored in indexedDB, but it's stored as an encrypted PEM file.

If you wanted to access the contents of this repo, you would have to both gain physical access to my device, know where to look and get the passphrase from me.  The passphrase is not stored anywhere but in the memory of my browser page.

This repo is treated as a generic filesystem store.  It's history is not kept around.  Every push done by the library is a force push with no parent.  History isn't needed because the secondary git repo inside the git repo contains all it's own history through the git object graph.  In this outer filesystem, encrypted files are tagged by marking them as symlinks in the git tree structure.  Symlinks can technically contain anything so in this case they contain deflated and encrypted git objects. 

This experiment is made possible by [pako][], [forge][] and my [js-git][] and [js-github][] projects.

This entire experiment was created, developed, hosted, tested, and deployed from my chromebook using [tedit][] without ever touching a command line.  I could have done the same thing with a few hours of work using any display chromebook at walmart.  There is no cloud server involved anywhere other than github and only for storing the encrypted filesystem somewhere public.

[pako]: https://github.com/nodeca/pako
[forge]: https://github.com/digitalbazaar/forge
[js-git]: https://github.com/creationix/js-git
[js-github]: https://github.com/creationix/js-github
[REST API]: https://developer.github.com/v3/git/
[tedit]: https://chrome.google.com/webstore/detail/tedit-development-environ/ooekdijbnbbjdfjocaiflnjgoohnblgf
