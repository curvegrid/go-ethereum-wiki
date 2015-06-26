Geth has quite a nice logging system, capable of creating leveled log entries tagged with various parts of the system. This helps enormously during debugging to see exactly what the system is doing, what branches it's taking, etc. However, logs are not particularly useful when the system does work correctly, just not very optimally: one - or even a  handful - of logged events is not really statistically relevant, and tracing more in log files can quickly become unwieldy.

The goal of the Geth metrics system is that - similar to logs - we should be able to add arbitrary metric collection to any part of the code without requiring fancy constructs to analyze them (counter variables, public interfaces, crossing over the APIs, console hooks, etc). Instead, we should just "update" metrics whenever and wherever needed, and have them automatically collected, surfaced through the APIs, queryable and visualizable for analysis.

To that extent, Geth currently implement two types of metrics:
 * **Meters**: Analogous to physical meters (electricity, water, etc), they are capable of measuring the *amount* of "things" that pass through and at the *rate* at which they do that. A meter doesn't have a specific unit of measure (byte, block, malloc, etc), it just counts arbitrary *events*. At any point in time it can report:
   * *Total number of events* that passed through the meter
   * *Mean throughput rate* of the meter since startup (events / second)
   * *Weighted throughput rate* in the last *1*, *5* and *15* minutes (events / second)
     * (*"weighted" means that throughput in recent seconds count more that in older ones*)
 * **Timers**: Extension of *meters*, where not only the occurrence of some event is measured, its *duration* is also collected. Similarly to meters, a timer can also measure arbitrary events, but each requires a duration to be assigned individually. Beside **all** the reports a meter can generate, a timer has additionally:
   * *Percentiles (5, 20, 50, 80, 95)*, reporting that some percentage of the events took less than the reported time to execute (*e.g. Percentile 20 = 1.5s would mean that 20% of the measured events took less time than 1.5 seconds to execute; inherently 80%(=100%-20%) took more that 1.5s*)
     * Percentile 5: minimum durations (this is as fast as it gets)
     * Percentile 50: well behaved samples (boring, just to give an idea)
     * Percentile 80: general performance (these should be optimised)
     * Percentile 95: worst case outliers (rare, just handle gracefully)
