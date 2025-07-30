# 11-team-ellu-be
KTB 판교 2기 11조 백엔드 리포지토리
 
## 🚀 기술 스택

**Spring Boot · JPA · PostgreSQL**   


**Redis**  
캐싱, 세션 정보, Pod 간 SSE/Websocket 이벤트 브로드캐스팅  


**Kafka**  
비동기 처리를 위한 큐, 실시간 챗봇 스트림 처리


**WebSocket**    
실시간 공유 캘린더


**Server-Sent Events(SSE)**    
채팅, 알림

## ✨ 프로젝트 소개

팀 단위 협업을 위한 자동화 캘린더서비스 Looper입니다.

반복적인 협업 과정을 자동화하여 팀 생산성을 극대화하고, 개인화된 맞춤형 계획을 제공하는 것을 목표로 개발된 서비스입니다.


주요 기능:

* **실시간 일정 공유**를 통한 팀 간 원활한 일정 조율
* **AI 기반 태스크 자동 생성**: 회의록(텍스트/음성파일)을 업로드하면, 프로젝트 생성 시 입력한 위키 정보를 바탕으로 AI가 주요 태스크 및 상세 스케줄 자동 생성
* **개인 맞춤형 챗봇 기능**을 통해 사용자의 일정 계획(운동, 공부 등의 계획)과 업무 관리 지원


<img width="2526" height="834" alt="image" src="https://github.com/user-attachments/assets/d0f28c87-41a5-4a0a-992e-547fce1615be" />

## 최적화 

**데이터의 특성과 접근 패턴에 따라 적절한 Redis 캐싱 전략을 적용하여 성능을 최적화하고 데이터베이스 부하를 줄였습니다.**

캐시 적중률: 0.96019914651 (약 96%)  

프로젝트 정보 (조회 빈도 높고 변경 거의 없음): Cache Aside + Long TTL 

-> 프로젝트 정보 조회 latency 32.3% 개선 (95 percentile)   

알림 (자주 추가되지만 수정/삭제 없음): Write-Through Cache + TTL

  
TTL에 Jitter를 적용하여 Cache Avalanche를 방지하고 분산 락을 적용하여 Cache Stampede를 방지했습니다.



**인덱싱을 적용하고 N+1 문제 해결하여 쿼리 성능을 개선했습니다.**

Seq Scan → Index Scan 전환, Filter 최소화    
-> execution time 감소    
  

