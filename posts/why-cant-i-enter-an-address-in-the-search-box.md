.. title: Why Can't I Enter An Address In The Search Box
.. date: 2008-01-19 05:36
.. author: Graham Wheeler
.. category: Mobile
.. slug: why-cant-i-enter-an-address-in-the-search-box

A common source of confusion for our users is the distinction we make
between 'what' and 'where'. We generally search for 'what' (which is the
text you can enter on the main form), centering the results on the
'where' (which is the location shown just below). If you want to change
the location, *or if you want to search for an address*, you need to
click on 'Choose a new location'; you can then enter the address on the
Location screen and click 'Find' (note: the screen shots shown below
come from a test version of the app we run on the desktop; the menus at
the top will be on the bottom on your phone, accessible via the two
softkeys; I just used the desktop version here as it was quicker for
me):

[![image](http://blufiles.storage.msn.com/y1pAcgpTMaaCBuKMThFUcoLUuA2a8qL_hlYotP9UT4tIAubhQ3CsZmDL6W44Lk1KUp1E4eoo_V7NJI?PARTNER=WRITER)](http://blufiles.storage.msn.com/y1pAcgpTMaaCBubcKxBhXBHiF5wxlbx7rbsWacM1pycCrsRRP90lgmae_DV5yxgmZ19ihX7hjVACpE?PARTNER=WRITER)

[![image](http://blufiles.storage.msn.com/y1pAcgpTMaaCBubt8zFX1Oj8NMawibS1R3BVP0cc7ryarimA0pvDzh49jAWDq21NFjzOblWVtam6tg?PARTNER=WRITER)](http://blufiles.storage.msn.com/y1pAcgpTMaaCBsflSxvtiSEBaR59Vw2kPpOidvJbM5SimjvYktlC3CZSuE0OJxFWToeeRtXOiAdo7E?PARTNER=WRITER)

[![image](http://blufiles.storage.msn.com/y1pAcgpTMaaCBtRlJq1kjSN5yzmLJS1fMqx3iYFqHOTSIE57sMX67jSU3AgDJ9p1aPlsH51gdeuxcw?PARTNER=WRITER)](http://blufiles.storage.msn.com/y1pAcgpTMaaCBu-fc50UQqgpK-9dk-BJE0eQI4jfvx6iI8isrEzl4qbDLhttb3EWDj9iTaravRObmk?PARTNER=WRITER)

The app will then try to locate the address you entered, and if
successful will show you the result, after which you should click
'Done':

[![image](http://blufiles.storage.msn.com/y1pAcgpTMaaCBvfibw9VK5w7FDRScSMG_z345AreYrD1oEu6GwniWVUhhrxLDlgqQL51RyRnfEhL68?PARTNER=WRITER)](http://blufiles.storage.msn.com/y1pAcgpTMaaCBtrH4Bd0kxYX8-2Rxisx1ybUa0LsJCq0uUE-3BPCfTU0iy1aFGZ1BFQSQZqbPaY2Sc?PARTNER=WRITER)

You'll then be back in the main view with the new address shown as your
search location:

[![image](http://blufiles.storage.msn.com/y1pAcgpTMaaCBuVI0Az9AbBHr1RVnGXAjj1MHPxP9s8mv-Yxa6k9yBCgvuOIe9YSGAIQEnpZ3kc8o8?PARTNER=WRITER)](http://blufiles.storage.msn.com/y1pAcgpTMaaCBvgo8D883CET1sH83Alvma_fYiqeFDTQ0CfcKFRwoUX5GBDODnfqHa6LOolqZLpJjI?PARTNER=WRITER)

You can the click on the Map icon to show a map centered on this
address, or do a search (which will be centered on the address), or
click on Directions for directions. There is something odd about
directions, though - we use your set location as the start point instead
of the end point:

[![image](http://blufiles.storage.msn.com/y1pAcgpTMaaCBvgTL65sq7jrLOHQgLPIwk7y81hhlMXkrLn0GSIXg0YQECWuvaf72i-xnpFGlDjYNU?PARTNER=WRITER)](http://blufiles.storage.msn.com/y1pAcgpTMaaCBs5L8EGDsqvc8Bx2SxUoN_hFYuPXwDthkpUX159EwgWIB-q2nKlb6YatTOJDljVORI?PARTNER=WRITER)

You can use left or right DPad actions to spin through the location
lists to change this, or, if the shown end point is actually your start
point, you can just do the route and then reverse it:

[![image](http://blufiles.storage.msn.com/y1pAcgpTMaaCBtgB0-kAlJJlHWfwwHhHvC8Wac50R3le-WU1gOW6CyRFyeGft1yE3IiSvMJ4PcEFfk?PARTNER=WRITER)](http://blufiles.storage.msn.com/y1pAcgpTMaaCBvmm41JZjtWhWcEpxFsUYKtqE4ZtvXvND0-VlGVqxyQi0qjvtRJRN8Jhn2wp-ReCvI?PARTNER=WRITER)

"But why don't you just allow addresses in the search box?", I hear some
of you ask. Well, the white page and yellow page search services that we
consume don't support mixed queries of what and where together, and we
didn't want to try make the app on the phone second guess what you're
trying to do (actually early versions of the app did do this - if the
what/where search failed we would try again treating the what as a
where - but that produced some strange results so we took it out).

The good news is that there are services now that we can use that do
support mixed queries so in some future release we should handle this
much better.
