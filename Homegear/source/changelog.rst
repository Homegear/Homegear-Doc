Changelog
#########

0.7.35
******

Features
========

Homegear Core
-------------

* Redirect from Homegear's entry webpage to admin UI if it exists.

Kodi
----

* Added variables STOP, NEXT and PREVIOUS.

Intertechno
-----------

* Log raw packets on loglevel 5.
* Changes to (better) support motion detectors.

HomeMatic BidCoS
----------------

* Allow unencrypted HM-LGW connections.

Improvements
============

Node-BLUE
---------

* Replaced untranslated texts by translation placeholders.
* Added `signin.json` and translated sign-in page.
* Locale can now be passed by setting `$_SESSION['locale'].
* Started translating into German.
* Improved node presence-light.

Fixes
=====

Homegear Core
-------------

* Global service messages could not be saved into database or deleted from database.
* Fixed crash on RPC device definition reload in families with dynamic device definitions.

Script Engine
-------------

* PHPs `putenv()` and `getenv()` were not replaced by our own implementation.
* Our PHP `putenv()` is now using `setenv()` instead of `putenv()`.

HomegearWS
----------

* Fixed error where colon was appended to URL even when port was empty.
