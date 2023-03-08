Bazel seems to not think that `:foo` is a unambiguously more specialized version of `:linux`.

```
~/selectsrepro$ bazel run test --define=foo=1
ERROR: /home/jwon/selectsrepro/BUILD.bazel:42:10: Illegal ambiguous match on configurable attribute "args" in //:test:
//:foo
//:linux
Multiple matches are not allowed unless one is unambiguously more specialized.
ERROR: Analysis of target '//:test' failed; build aborted:
INFO: Elapsed time: 0.035s
INFO: 0 processes.
FAILED: Build did NOT complete successfully (0 packages loaded, 0 targets configured)
ERROR: Build failed. Not running target
```
```
~/selectsrepro$ bazel run test
INFO: Build option --define has changed, discarding analysis cache.
INFO: Analyzed target //:test (0 packages loaded, 165 targets configured).
INFO: Found 1 target...
INFO: NOTICE: Checking support for different sandboxes
INFO: NOTICE: linux-sandbox: returning cached support value: true
INFO: NOTICE: Determined that linux-sandbox is supported
Target //:test up-to-date:
  bazel-bin/test
INFO: Elapsed time: 0.094s, Critical Path: 0.00s
INFO: 1 process: 1 internal.
INFO: Build completed successfully, 1 total action
INFO: Running command line: bazel-bin/test linux
linux
```
