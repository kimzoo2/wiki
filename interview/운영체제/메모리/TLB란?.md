### TLB란?
- MMU 내에 존재하는 페이지 테이블의 **캐시 메모리**입니다. 페이지 테이블의 일부가 저장되어 있습니다. CPU가 **메모리에 두 번 이상 접근하지 않고** 캐시를 통해 프레임에 접근할 수 있도록 만들어준다. 
- (페이지 테이블이 메모리에 적재되어 있기 때문에 CPU가 가상 주소 -> 물리 주소로 변환하기 위해선 페이지 테이블 접근 1번, 실제 위치 2번 이렇게 접근해야 한다.)