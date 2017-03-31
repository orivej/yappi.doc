# Reference Manual (v0.92)

<font face='Consolas'>
yappi.<b>start</b>(builtins=False, profile_threads=True)<br>
<br>
<blockquote>Starts profiling all threads in the current interpreter instance. This function can be called from any thread at any time. Resumes profiling if stop() is called previously.</blockquote>

<blockquote>Current parameters:<br>
<table><thead><th> <b>Param</b> </th><th> <b>Description</b> </th></thead><tbody>
<tr><td> builtins     </td><td> Profile builtin functions used by standart Python modules. It is <i>False</i> by <i>default</i>. </td></tr>
<tr><td> profile_threads </td><td>  Profile all of the threads if 'true', else, profile only the calling thread. </td></tr></blockquote></tbody></table>

<font face='Consolas'>
yappi.<b>stop</b>()<br>
</font>

<blockquote>Stops the currently running yappi instance. Same profiling session might be resumed later by calling start().</blockquote>


<font face='Consolas'>
yappi.<b>clear_stats</b>()<br>
</font>

<blockquote>Clears the profiler results. The results stays in memory unless application(all threads including the main thread) exists or clear_stats() is called.</blockquote>


<font face='Consolas'>
yappi.<b>get_func_stats</b>()<br>
</font>

<blockquote>Returns the function stats as <a href='https://code.google.com/p/yappi/wiki/YFuncStats_v092'>YFuncStats</a>  object.</blockquote>


<font face='Consolas'>
yappi.<b>get_thread_stats</b>()<br>
</font>

<blockquote>Returns the thread stats as <a href='https://code.google.com/p/yappi/wiki/YThreadStats_v092'>YThreadStats</a>  object.</blockquote>


<font face='Consolas'>
yappi.<b>is_running</b>()<br>
</font>

<blockquote>Returns a boolean indicating whether profiler is running or not.</blockquote>


<font face='Consolas'>
yappi.<b>get_clock_type</b>()<br>
</font>

<blockquote>Returns information about the underlying clock type Yappi uses to measure timing.<br>
Return values can either be <i>CPU</i> or <i>Wall</i>.</blockquote>

<font face='Consolas'>
yappi.<b>set_clock_type</b>(type)<br>
</font>

<blockquote>Sets the underlying clock type. <i>type</i> can be following:</blockquote>

<table><thead><th> <b>Clock Type</b> </th><th> <b>Description</b> </th></thead><tbody>
<tr><td> Wall              </td><td> <a href='http://en.wikipedia.org/wiki/Wall_time'>Details</a> </td></tr>
<tr><td> CPU               </td><td> <a href='http://en.wikipedia.org/wiki/CPU_time'>Details</a> </td></tr></tbody></table>

<font face='Consolas'>
yappi.<b>get_clock_time</b>()<br>
</font>

<blockquote>Returns the current clock time with regard to the underlying clock type.</blockquote>

<font face='Consolas'>
yappi.<b>get_clock_info</b>()<br>
</font>

<blockquote>Returns a dict containing the OS API used for timing, the precision of the underlying clock.</blockquote>

<pre><code>'''
Example on Windows:
'''
&gt;&gt;&gt; import yappi
&gt;&gt;&gt; yappi.get_clock_info()
{'resolution': '100ns', 'api': 'getthreadtimes'}
&gt;&gt;&gt;
</code></pre>

<font face='Consolas'>
yappi.<b>get_mem_usage</b>()<br>
</font>

<blockquote>Returns the internal memory usage of the profiler itself.</blockquote>


<font face='Consolas'>
yappi.<b>shift_context_time</b>(context_id, amount)<br>
</font>
<blockquote>Adjust a context's start time, and the time of all functions currently on<br>
the context's stack. 'amount' is in the same units as get_clock_type(). A<br>
negative 'amount' increases the 'ttot' statistic for this context and all<br>
functions on the stack, and a positive 'amount' decreases 'ttot'.</blockquote>

<font face='Consolas'>
yappi.<b>set_context_id_callback</b>(callback)<br>
</font>
<blockquote>Use a number other than thread_id to determine the current context.<br>
The callback must take no arguments and return an integer.</blockquote>

<pre><code>'''
Example:
'''
&gt;&gt;&gt; import greenlet, yappi
&gt;&gt;&gt; yappi.set_context_id_callback(lambda: id(greenlet.getcurrent()))
</code></pre>

<font face='Consolas'>
yappi.<b>set_context_name_callback</b>(callback)<br>
</font>
<blockquote>Use a string other than the current thread's class name as the context's<br>
name. The callback must take no arguments and return a string.</blockquote>

<pre><code>'''
Example:
'''
&gt;&gt;&gt; import greenlet, yappi
&gt;&gt;&gt; yappi.set_context_name_callback(lambda: greenlet.getcurrent().__class__.__name__)
</code></pre>

<font face='Consolas'>
yappi.<b>convert2pstats</b>(stats)<br>
</font>

<blockquote>Converts the internal stat type of yappi(which is returned by a call to YFuncStats.get()) as <a href='http://docs.python.org/3.4/library/profile.html#module-pstats'>pstats</a> object.</blockquote>

<font face='Consolas'>
yappi.<b>profile(clock_type="cpu", profile_builtins=False, return_callback=None)</b>
</font>

<blockquote>A profile decorator that can be used to profile a single call.<br>
<i>clock_type</i> and <i>profile_builtins</i> parameters are same as yappi.<i>start</i>()<br>
return_callback is a callable that is called when function finishes executing. (If function<br>
is recursive return_callback will be called when the last function finishes.)<br>
Yappi provides the actual function object as first parameter and the function stats as second<br>
to this callback.</blockquote>

<pre><code>'''
Example:
Below example seem simple at first but it does a lot. It simply merges different
profiling results of a function together into a single file and save it as 
&lt;function_name&gt;.profile.
'''
import yappi
def aggregate(func, stats):
    fname = "%s.profile" % (func.__name__)
    try: 
        stats.add(fname)
    except IOError:
        pass
    stats.save(fname)

@yappi.profile(return_callback=aggregate)
def sum(x, y):
    return x+y
</code></pre>

</font>