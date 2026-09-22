# 구현 내역

Lv 1. Docker로 MySQL과 Redis 설정: Docker MySQL/Redis 접속 정보를 `application.properties`에 설정.

Lv 2. SQL을 JPA 인덱스로 표현하기: `ChatMessage`에 `(world_id, created_at)` 인덱스를 추가.

Lv 3. 요청 검증과 DTO: 플레이어 등록: 닉네임 검증과 중복 체크를 추가해 `POST /players`를 완성.

Lv 4. 월드 생성: `duringCreation()` 잠금 안에서 루트 월드 개수를 확인해 3개 초과 시 `WORLD_LIMIT_REACHED`를 반환하도록 구현해 동시 생성 시 레이스 컨디션을 방지.

Lv 5. 채팅 저장과 내역 조회: 메시지 저장 시 월드 존재 여부를 검증하고, 최근 채팅 조회는 최신순 조회 결과를 오래된 순으로 뒤집어 반환하도록 구현.

Lv 6. 최근 채팅 조회 API 구현: `GET /worlds/{worldId}/chats?limit=50`을 매핑해 캐시/월드 존재 검사를 포함한 조회 서비스와 연결.

Lv 7. WebSocket 연결과 사용자 식별: 핸드셰이크 시 닉네임/월드ID를 조회해 세션 속성에 저장하고, 존재하지 않으면 각각 에러 코드로 거부.

Lv 8. HandshakeInterceptor 등록: WebSocket 핸들러 등록에 인터셉터를 연결해 Lv7의 식별 로직이 실제 연결에 적용되도록 함.

Lv 9. 월드별 WebSocket 세션 관리: 월드+닉네임 단위로 WebSocket 세션을 등록/조회하는 레지스트리를 구현해 중복 등록을 방지.

Lv 10. Redis 접속 상태 관리: 월드별 접속자를 Redis ZSet(TTL 기반)으로 등록/제거해 온라인 여부와 접속자 수를 관리.

Lv 11. 메시지 라우팅과 Ping/Pong: 메시지 타입별 핸들러 라우팅을 연결하고, ping 수신 시 presence TTL을 갱신 후 pong으로 응답.

Lv 12. 플레이어 이동 요청 처리: 이동 메시지를 파싱해 엔진에 이동 요청을 전달하고 좌표가 반영되도록 구현.

Lv 13. 채팅 요청 처리와 응답 구성: 채팅 메시지를 저장하고 응답 DTO를 구성(실시간 전송은 Lv14에서 완성).

Lv 14. 같은 월드의 참여자에게 채팅 전송: 같은 월드에 있는 모든 참여자(발신자 포함)에게 채팅 메시지를 실시간으로 전달.

Lv 15. 접속자 목록 조회: 요청 시 현재 월드의 열린 세션들에서 닉네임을 추출해 요청자에게만 접속자 목록을 응답.
