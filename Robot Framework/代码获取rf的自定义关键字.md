>失败重新执行（rerun）
```python
from time import sleep

from robotide.controller import Project
from robotide.namespace import Namespace


from robotide.preferences import RideSettings
settings = RideSettings()
namespace = Namespace(settings)
project_instance= Project(namespace, settings)
project_instance.load_data("D:\workspace\HTinterfaceAutoTest_py3")


kws = project_instance.get_all_keywords()

sleep(5)

for kw in kws:
    # print(kw)
    print(kw.name)
    # print(kw.doc)
    print("~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~")
```