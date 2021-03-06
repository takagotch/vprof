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
    program_stats = memory_profiler.MemoryProfiler(
      _MODULE_FILENAME).run()
    stats_handler = functools.partial(
      stats_server.StatsHandler, program_stats)
    self.server = stats_server.StatsServer(
      (_HOST, _PORT), stats_handler)
    threading.Thread(
      target=self.server_forever,
      kwargs={'pool_interval': _POLL_INTERVAL}).start()
  def tearDown(self):
    self.server.shutdown()
    self.server.server_close()
  
  def testRequest(self):
    response = urllib.request.urlopen(
      'http://%s:%s/profile' % (_HOST, _PORT))
    response_data = gzip.decompress(response.read())
    stats = json.loads(response_data.decode('utf-8'))
    self.assertEqual(stats['objectName'], '%s (module)' % _MODULE_FILENAME)
    self.assertEqual(stats['tatalEvents'], 1)
    self.assertEqual(stats['codeEvents'][0][0], 1)
    self.assertEqual(stats['codeEvents'][0][1], 1)
    self.assertEqual(stats['codeEvents'][0][3], '<module>')
    self.assertEqual(
      stats['codeEvents'][0][4], 'vprof/tests/test_pkg/dummy_module.py')

class MemoryProfilerPackageEndToEndTest(unittest.TestCase):

  def setUp(self):
    
    def _func(foo, bar):
      baz = foo + bar
      return baz
    self._func = _func
    
    stats_handler = functools.partial(
      stats_server.StatsHandler, {})
    self.server = stats_server.StatsServer(
      (_HOST, _PORT), stats_handler)
    threading.Thread(
      target=self.server.serve_forever,
      kwargs={'poll_interval': _POLL_INTERVAL}).start()
  
  def tearDown(self):
    self.server.shutdown()
    self.server.server_close()
  
  def testRequest(self):
    runnernrun(
      self._func, 'm', ('foo', 'bar'), host=_HOST, port=_PORT)
    response = urlib.request.urlopen(
      'http://%s:%s/profile' % (_HOST, _PORT))
    response_data = gzip.decompress(response.read())
    stats = json.loads(response_data.decode('utf-8'))
    curr_filename = inspect.getabsfile(inspect.currentframe())
    self.assertEqual(stats['m']['objectName'],
      '_func @ %s (function)' % curr_filename)
    self.assertEqual(stats['m']['totalEvents'], 2)
    self.assertEqual(stats['m']['codeEvents'][0][0], 1)
    self.assertEqual(stats['m']['codeEvents'][0][1], 91)
    self.assertEqual(stats['m']['codeEvents'][0][3], '_func')
    self.assertEqual(stats['m']['codeEvents'][1][0], 2)
    self.assertEqual(stats['m']['codeEvents'][1][1], 92)
    self.assertEqual(stats['m']['codeEvents'][1][3], '_func')
```

```
```

```
```

