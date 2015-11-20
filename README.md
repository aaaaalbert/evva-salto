# evva-salto

In this repository we discuss a 2012 deployment of
EVVA SALTO electronic door locks we had access to.

Our insights include:
* The electronic keys in use ([Maxim Integrated's DS2433 "iButton"](http://datasheets.maximintegrated.com/en/ds/DS2433.pdf) [1])
  don't require any authentication before being read out (or written to) by
  either an electronic lock, or actually anything that speaks the [1-Wire protocol](https://www.maximintegrated.com/en/app-notes/index.mvp/id/1796).
* The interface between doors and locks is trivially interceptable, e.g.
  using a laptop soundcard and a chopped-up audio cable.
* We used this method to reverse-engineer
 - how door locks communicate with keys (read access permissions, append
  to access log),
 - what network-connected door locks do (read permissions, read out full
  access logs), and
 - how parts of the data structure stored on the keys work.
* Using an Arduino, [an existing 1-Wire library](http://playground.arduino.cc/Learning/OneWire) 
  and 30 additional lines
  of C code, we reprogrammed the key.
* Turns out that the lock also trusts the key, e.g. happily dereferences a
  write pointer that we can control, so that we can
 - prevent the lock from writing to the access log, or
 - make a networked lock think that a key's access log is empty.

We found all of the above just by looking at one physical SALTO
installation, plus publicly available datasheets.

Footnotes:

[1] Funnily enough, this PDF from 2010 sports a watermark reading 
"NOT RECOMMENDED FOR NEW DESIGNS" on every page.
