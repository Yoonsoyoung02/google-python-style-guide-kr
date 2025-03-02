2.3 Packages
2.3 패키지

Import each module using the full pathname location of the module.
각 모듈은 완전한 경로를 통해 호출합니다.

2.3.1 Pros
찬성 의견

Avoids conflicts in module names or incorrect imports due to the module search path not being what the author expected. Makes it easier to find modules.
모듈 검색 경로가 작성자가 예상한 것과 다를 경우 발생할 수 있는 모듈 이름의 충돌이나 잘못된 호출을 방지합니다.
또한, 모듈을 더 쉽게 찾을 수 있도록 합니다.

2.3.2 Cons
반대 의견

Makes it harder to deploy code because you have to replicate the package hierarchy. Not really a problem with modern deployment mechanisms.
패키지 계층 구조를 유지해야 하므로 코드 배포가 복잡해질 수 있습니다.
그러나 최근 배포 시스템에서 패키지 구조의 유지가 일반적이므로 큰 문제가 되지 않습니다.

2.3.3 Decision
결정

All new code should import each module by its full package name.
모든 새로운 코드는 전체 패키지 경로를 통해 각 모듈을 호출해야합니다.

Imports should be as follows:
모듈의 호출 방식은 다음과 같습니다.

Yes:

# Reference absl.flags in code with the complete name (verbose).

import absl.flags
from doctor.who import jodie

\_FOO = absl.flags.DEFINE_string(...)
Yes:

# Reference flags in code with just the module name (common).

from absl import flags
from doctor.who import jodie

\_FOO = flags.DEFINE_string(...)
(assume this file lives in doctor/who/ where jodie.py also exists)

No:

# Unclear what module the author wanted and what will be imported. The actual

# import behavior depends on external factors controlling sys.path.

# Which possible jodie module did the author intend to import?

import jodie

The directory the main binary is located in should not be assumed to be in sys.path despite that happening in some environments. This being the case, code should assume that import jodie refers to a third-party or top-level package named jodie, not a local jodie.py.

메인 바이너리가 위치한 디렉터리가 sys.path에 포함된다고 가정해서는 안 됩니다.
일부 환경에서는 그렇게 동작할 수 있지만, 항상 보장되는 것은 아닙니다.
따라서, import jodie라는 코드가 로컬의 jodie.py 파일이 아니라, 서드파티 패키지나 최상위 패키지로 인식될 수 있다고 가정해야 합니다.
