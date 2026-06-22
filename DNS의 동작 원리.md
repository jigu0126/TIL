왜 쓰는가?

편해서, DNS가 없다면 모든 웹 사이트의 IP 주소를 외우고 다녀야 한다.

DNS 네임 스페이스

!image.png

우리가 보는 도메인 구조

이렇게 트리 구조로 만들 수 있다.

!image.png

- root 도메인 : 네임 스페이스 상 최상위 도메인, 출발지 역할, 전 세계 13개의 원본
- 최상위 도메인(Top Level Domain, TLD) : 국가 혹은 일반 최상위 도메인
ex ) .kr, .jp, .net, .com, .org
- sub domain : 2차 이상 도메인, 최상위 도메인이 관리하는 하위 도메인
ex ) google, naver
- 하위노드는 무제한으로 만들 수 있으나 도메인 전체 길이는 제한이 있음 (255 byte)
