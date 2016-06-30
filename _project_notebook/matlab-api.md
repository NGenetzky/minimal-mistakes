## Matlab API

## Brainstorming Improvements

Consider a struct that has all of the variables defined on the micro controller.
With the variables defined in matlab and with a easy way to modify them from 
matlab; basicly you have control over the micro controller variables. All the
variables would have to be serializable into and from a character string.

Alternatively the data from the micro controller could be published a cloud service
like [thingspeak][ts1] and then [read from matlab][ts2]. This has the added
benefit of caching the data so that matlab does not have to constantly be checking
for new data. It also has the benefit of supporting other platforms (like 
Arduino or Raspberry Pi).

## Conclusion

I think the most important detail would be to have a very consistent interface
between the Matlab and the Particle device. It should be kept very simple and
something that does not require major adaptations in the future.

[ts1]: https://thingspeak.com/
[ts2]: http://www.mathworks.com/hardware-support/thingspeak.html
