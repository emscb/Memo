##### ConfigParser

- python 2.x 에서는 `ConfigParser`
- 3.x 에서는 `configparser` (PEP 때문에)

```python
import sys
if sys.version_info[0] == 2:
    import ConfigParser
else:
    import configparser
```

