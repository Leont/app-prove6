[![Build Status](https://travis-ci.org/Leont/app-prove6.svg?branch=master)](https://travis-ci.org/Leont/app-prove6)

NAME
====

prove6 - Run tests through a TAP harness.

USAGE
=====

```shell
prove6 [options] [files or directories]
```

<table class="pod-table">
<caption>Boolean options</caption>
<tbody>
<tr> <td>-v</td> <td>--verbose</td> <td>Print all test lines.</td> </tr> <tr> <td>-l</td> <td>--lib</td> <td>Add &#39;lib&#39; to the path for your tests (-Ilib).</td> </tr> <tr> <td></td> <td>--shuffle</td> <td>Run the tests in random order.</td> </tr> <tr> <td></td> <td>--ignore-exit</td> <td>Ignore exit status from test scripts.</td> </tr> <tr> <td></td> <td>--reverse</td> <td>Run the tests in reverse order.</td> </tr> <tr> <td>-q</td> <td>--quiet</td> <td>Suppress some test output while running tests.</td> </tr> <tr> <td>-Q</td> <td>--QUIET</td> <td>Only print summary results.</td> </tr> <tr> <td></td> <td>--timer</td> <td>Print elapsed time after each test.</td> </tr> <tr> <td></td> <td>--trap</td> <td>Trap Ctrl-C and print summary on interrupt.</td> </tr> <tr> <td></td> <td>--help</td> <td>Display this help</td> </tr> <tr> <td></td> <td>--version</td> <td>Display the version</td> </tr>
</tbody>
</table>

<table class="pod-table">
<caption>Options with arguments</caption>
<tbody>
<tr> <td>-I</td> <td>--incdir</td> <td>Library paths to include.</td> </tr> <tr> <td>-e</td> <td>--exec</td> <td>Interpreter to run the tests (&#39;&#39; for compiled</td> </tr> <tr> <td></td> <td></td> <td>tests.)</td> </tr> <tr> <td></td> <td>--ext</td> <td>Set the extensions for tests (default &lt;t rakutest t6&gt;)</td> </tr> <tr> <td></td> <td>--harness</td> <td>Define test harness to use. See TAP::Harness.</td> </tr> <tr> <td></td> <td>--reporter</td> <td>Result reporter to use.</td> </tr> <tr> <td>-j</td> <td>--jobs</td> <td>Run N test jobs in parallel (try 9.)</td> </tr> <tr> <td></td> <td>--cwd</td> <td>Run in certain directory</td> </tr> <tr> <td></td> <td>--err=stderr</td> <td>Direct the test&#39;s $*ERR to the harness&#39; $*ERR.</td> </tr> <tr> <td></td> <td>--err=merge</td> <td>Merge test scripts&#39; $*ERR with their $*OUT.</td> </tr> <tr> <td></td> <td>--err=ignore</td> <td>Ignore test script&#39; $*ERR.</td> </tr>
</tbody>
</table>

NOTES
=====

Default Test Directory
----------------------

If no files or directories are supplied, `prove6` looks for all files matching the pattern `*.{t,t6,rakutest}` under the directory <t>.

Colored Test Output
-------------------

Colored test output is the default, but if output is not to a terminal, color is disabled.

Color support requires `Terminal::ANSIColor` on Unix-like platforms. If the necessary module is not installed colored output will not be available.

Exit Code
---------

If the tests fail `prove6` will exit with non-zero status.

`-e`
----

Normally you can just pass a list of Raku tests and the harness will know how to execute them. However, if your tests are not written in Raku or if you want all tests invoked exactly the same way, use the `-e` switch:

    prove6 -e='/usr/bin/ruby -w' t/
    prove6 -e='/usr/bin/perl -Tw -mstrict -Ilib' t/
    prove6 -e='/path/to/my/customer/exec'

`--err`
-------

  * `--err=stderr`

    Direct the test's $*ERR to the harness' $*ERR.

    This is the default behavior.

  * `--err=merge`

    If you need to make sure your diagnostics are displayed in the correct order relative to test results you can use the `--err=merge` option to merge the test scripts' $*ERR into their $*OUT.

    This guarantees that $*OUT (where the test results appear) and $*ERR (where the diagnostics appear) will stay in sync. The harness will display any diagnostics your tests emit on $*ERR.

    Caveat: this is a bit of a kludge. In particular note that if anything that appears on $*ERR looks like a test result the test harness will get confused. Use this option only if you understand the consequences and can live with the risk.

    PS: Currently not supported.

  * `--err=ignore`

    Ignore the test script' $*ERR

`--trap`
--------

The `--trap` option will attempt to trap SIGINT (Ctrl-C) during a test run and display the test summary even if the run is interrupted

$*REPO
------

`prove6` introduces a separation between "options passed to the raku which runs prove6" and "options passed to the raku which runs tests"; this distinction is by design. Thus the raku which is running a test starts with the default `$*REPO`. Additional library directories can be added via the `RAKULIB` environment variable or via the `-Ilib` option to `prove6`.

