---
layout:     post
title:      The Dates of the Battle of Marignano
date:       2017-03-12
summary:    Because September 13th and 14th of 1515 are gregorianly not.
---

The Battle of Marignano (_La Bataille de Marignan_ in French) was a war fought between France (led by Francis I of France, _François 1er_ in French) and the Old Swiss Confederacy, between September 13 and 14 of 1515.

For some reasons, this is a famous date known by most of French people, probably because the year is easy to remember (_quinze cent quinze, Bataille de Marignan !_).

Funny thing is that we don't really know what happened in this war really. But that's another subject.

Anyway, a few weeks ago, I was writing some Java unit tests for some component about determining if a month of records was billable. Nothing exiciting really.

<script src="https://gist.github.com/pzn/d7842065f86c64a61462876bf66f8dd6.js"></script>

I am the kind of guy who likes easter eggs (who doesn't?), especially when they are unsual, and when you can learn something... What beautiful places tests are for such purpose!

I would use the year of this famous war as boundaries for testing my component.

<script src="https://gist.github.com/pzn/ec601e3191f92995960fe90085a0af28.js"></script>

To keep things short, other parts of the applications were still using `java.util.Date`. So later in the first snippet, a conversion from `java.time.LocalDateTime` to the old school `Date` is done:

<script src="https://gist.github.com/pzn/2a9c6bdb6765faa49591fb7986ddda81.js"></script>

And I noticed something weird. This code would output the following (with VM forced with _-Duser.timezone=UTC_):

    from = {LocalDateTime@643} "1515-09-13T00:00"
    dateFrom = {Date@544} "Mon Sep 03 00:00:00 UTC 1515"

Wat? How is it possible to lose 10 days when transposing a `LocalDateTime` to a `Date`?

(Showing that to my Python colleague, I immediately heard _his love_ for Java).

No, it cannot be. Please don't be a Java issue.

Hopefully, I just _lost_ a few minutes before realizing that my unit tests were behaving not as expected due to the Battle of Marignano. Today's year was working fine. So I finally gave up to go back to contemporary _boring_ dates.

Today in Canada, we switched to daylight saving time, ~~losing one hour of sleep~~ adding one hour to our clocks. Remembering not knowing why September 13th of 1515 was breaking my unit tests, today was a good day to uncover the truth about it, ending writing up this world-class Java champion _main_ of mine:

<script src="https://gist.github.com/pzn/8da6d3efac1aab7b85361478c1774e38.js"></script>

Oh, interesting. What happened in October 1582, between the 14th and the 15th?

Quickly reaching [1582](https://en.wikipedia.org/wiki/1582) on Wikipedia:

> [...] this year also saw the beginning of the Gregorian Calendar switch, when the Papal bull known as Inter gravissimas introduced the Gregorian calendar [...]

Yep, this was the date when many countries changed their Julian calendar system to the today's Gregorian calendar!

> In these countries, the year continued as normal until Thursday, October 4. However, the next day became Friday, October 15 (like a common year starting on Friday) [...]

In other words, Friday, October 5th of 1582 never existed. As much as the dates until October 15th of 1582.

Well, not exactly. These dates existed, but not in the countries who  first adopted the Gregorian calendar.

> [...] adopted by Spain, Portugal, the Polish–Lithuanian Commonwealth and most of present-day Italy from the start.

Other countries adopted the Gregorian calendar later in the history. 

[For instances](https://en.wikipedia.org/wiki/Adoption_of_the_Gregorian_calendar#Timeline):
- France adopted the Gregorian calendar two months later, _letting Sunday, December 9 be followed by Monday, December 20_
- UK and part of the Eastern USA (then possessed by the British Empire) adopted it in 1752 (more than two centuries later!)
- Greece in 1923!

For Java, [October 15, 1582 is the default point in the history when the switch happened](http://docs.oracle.com/javase/8/docs/api/java/util/GregorianCalendar.html#setGregorianChange-java.util.Date-). Meaning that, even if you set your timezone to _Europe/Paris_(was it even a thing?), you still have this bump in the history, despite France still hadn't adopted the Gregorian calendar.

And I thought timezones were the worst thing in programming!

Well ok, they are. But imagine if countries were officially using different calendars...

Hopefully, we don't have such stories as _Support for pre-Gregorian dates for billing_ in our backlogs.

--

Additional resources about the subject:

* [On This Day: In 1582, October 5 Never Happened](http://www.findingdulcinea.com/news/on-this-day/September-October-08/On-this-Day--In-1582--Oct--5-Did-Not-Exist-.html) (findingdulcinea.com)
* [Adoption of the Gregorian calendar](https://en.wikipedia.org/wiki/Adoption_of_the_Gregorian_calendar) (Wikipedia)
* [Famous take on timezones from Tom Scott of Computerphile](https://youtu.be/-5wpm-gesOY) (Youtube)
* [Leap Years: we can do better](https://youtu.be/qkt_wmRKYNQ) (Youtube)
