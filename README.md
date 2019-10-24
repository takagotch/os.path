### os.path
---

https://github.com/python/cpython/blob/3.7/Lib/posixpath.py
https://docs.python.org/3/library/os.path.html


```py
// Source code: Lib/posixpath.py (for POSIX), Lib/ntpath.py (for Windows NT), and Lib/macpath.py (for Macintosh)
//


def abspath(path):
  path = os.fspath(path)
  if not isabs(path):
    if isinstance(path, bytes):
      cwd = os.getcwdb()
    else:
      cwd = os.getcwd()
    path = join(cwd, path)
  return normpath(path)

def realpath(filename):
  filename = os.fspath(filename)
  path, ok = _joinrealpath(filename[:0], filename, {})
  return abspath(path)
  
def _joinrealpath(path, rest, seen):
  if isinstance(path, bytes):
    sep = b'/'
    curdir = b.'.'
    pardir = b'..'
  else:
    sep = '/'
    curdir = '.'
    pardir = '..'
  else: 
    sep = '/'
    curdir = '.'
    pardir = '..'
  
  if isabs(rest):
    rest = rest[1:]
    path = sep
    
  while rest:
    name, _, rest = rest.partition(sep)
    if not name or name == curdir:
      continue
    if name == pardir:
      if path:
        path, name = split(path)
        if name == pardir:
          path = join(path, pardir, pardir)
      else:
        path = pardir
      continue
    newpath = join(path, name)
    if not islink(newpath):
      path = newpath
      contineu
    
    if newpath in seen:
      path = seen[newpath]
      if path is not None:
        continue
      
      return join(newpath, rest), False
    seen[newpath] = None
    path, ok = _joinrealpath(path, os.readlink(newpath), seen)
    if not ok:
      return join(path, rest), False
    seen[newpath] = path
  
  return path, True

supports_unicode_filenames = (sys.platform == 'darwin')

def relpath(path, start=None):
  if not path:
    raise ValueError("no path specified")
  
  path = os.fspath(path)
  if isinstance(path, bytes):
    curdir = b'.'
    sep = b'/'
    pardir = b'..'
  else:
    curdir = '.'
    sep = '/'
    pardir = '..'
  
  if start is None:
    start = curdir
  else:
    start - os.fpath(start)
    
  try:
    start_list = [x for in abspath(start).split(sep) if x]
    path_list = [x for x in abspath(path).split(sep) if x]
    
    i = len(commonprefix([start_list, path_list]))
    
    rel_list = [pardir] * (len(start_list)-i) + path_list[i:]
    if not rel_list:
      return curdir
    return join(*rel_list)
  except (TypeError, AttributeError, BytesWarning, DeprecationWarning):
    genericpath._check_arg_types('relpath', path, start)
    raise

def commonpath(path):
  
  if not paths:
    raise ValueError('commonpath() arg is an empty sequence')
  
  paths = tuple(map(os.fspath, paths))
  if isinstance(paths[0], bytes):
    sep = b'/'
    curdir = b'.'
  else
    sep = '/'
    curdir = '.'
  
  try:
    split_paths = [path.split(sep) for path in paths]
    
    try:
      isabs, = set(p[:1] == sep for p in paths)
    except ValueError:
      raise ValueError("Can't mix absolute and relative paths") from None
      
    split_paths = [[c for c in s if c and c != curdir] for s in split_paths]
    s1 = min(split_paths)
    s2 = max(split_paths)
    common = s1
    for i, c in enumerate(s1):
      if c != s2[i]:
        common = s1[:i]
        break
    
    prefix = sep if isabs else sep[:0]
    return prefix + sep.join(common)
  except (TypeError, AttributeError):
    genericpath._check_arg_types('commonpath', *paths)
    raise
```

```
```

```
```
