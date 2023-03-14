Bazel seems to do weird things when it comes to comparing `config_setting_group` and `config_setting` in terms of refinement. It seems to rely on the array ordering of the `match_all` field.

```
~/selectsrepro$ bazel run test --define=foo=1 --define=bar=1 --define=lnx=1
INFO: Analyzed target //:test (1 packages loaded, 6 targets configured).
INFO: Found 1 target...
Target //:test up-to-date:
  bazel-bin/test
INFO: Elapsed time: 0.065s, Critical Path: 0.00s
INFO: 1 process: 1 internal.
INFO: Build completed successfully, 1 total action
INFO: Build completed successfully, 1 total action
foo
```

Now, swap the order of the `config_setting_group`:

```
~/selectsrepro$ bazel run test --define=foo=1 --define=bar=1 --define=lnx=1
ERROR: /home/jwon/selectsrepro/BUILD.bazel:56:10: Illegal ambiguous match on configurable attribute "args" in //:test:
//:foo
//:linux
Multiple matches are not allowed unless one is unambiguously more specialized.
ERROR: Analysis of target '//:test' failed; build aborted:
INFO: Elapsed time: 0.055s
INFO: 0 processes.
FAILED: Build did NOT complete successfully (1 packages loaded, 4 targets configured)
FAILED: Build did NOT complete successfully (1 packages loaded, 4 targets configured)
```

```
~/selectsrepro$ bazel version
Build label: 6.1.0
Build target: bazel-out/k8-opt/bin/src/main/java/com/google/devtools/build/lib/bazel/BazelServer_deploy.jar
Build time: Mon Mar 6 17:09:47 2023 (1678122587)
Build timestamp: 1678122587
Build timestamp as int: 1678122587
```
