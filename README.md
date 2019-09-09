### vprof
---
https://github.com/nvdv/vprof

```py
// vprof/tests/memory_profiler_e2e.py

try:
  import __builtin__ as builtins
except ImportError:
  import builtins
builtins.intial_rss_size = 0

_HOST, _PORT = 'localhost', 12345
_MODULE_FILENAME = 'vprof/tests/test_pkg/dummy_module.py'
_PACKAGE_PATH = 'vprof/tests/test_pkg'
_POLL_INTERVAL = 0.01

class MemoryProfilerModuleEndToEndTest(unittest.TestCase):
  
  def setUp(self):
  

```

```
```

```
```

