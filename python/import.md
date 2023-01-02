# 가져오기(ikmport)

```python
# 옳은 예
import os
import sys
```

```python
# 잘못된 예
import sys, os
```

```python
# 옳은 예
from subprocess import Popen, PIPE
```

`절대 경로` 가져오기가 권장된다. 대체적으로 이 편이 가독성이 좋다.

```python
import mypkg.sibling     #절대경로 예시
from mypkg import sibling
from mypkg.sibling import example
```

하지만, *명시적 상대경로 가져오기*는 절대경로 가져오기의 허용 가능한 대안이다.  
특히, `불필요하게 장황하고 복잡한 패키지 레이아웃을 처리할 땐`, 절대경로 가져오기를 사용하는 것보다 나을 수 있다.

```python
from . import sibling
from .sibling import example
```
