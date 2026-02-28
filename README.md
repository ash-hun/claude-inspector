# Claude Inspector

Claude Code의 5가지 프롬프트 메커니즘이 실제로 어떤 API 페이로드를 만드는지 시각화하고 테스트하는 데스크탑 앱.

![Claude Inspector Screenshot](https://github.com/kangraemin/claude-inspector/raw/main/public/screenshot.png)

## 다루는 메커니즘

| 메커니즘 | 주입 위치 | 활성화 방식 | 직접 전송 |
|---|---|---|---|
| **CLAUDE.md** | `messages[].content` → `<system-reminder>` | 자동 (파일 존재 시) | ✅ |
| **Output Style** | `system[]` 추가 블록 | `/output-style` 명령 | ✅ |
| **Slash Command** | `messages[].content` → `<command-message>` | 사용자 명시적 호출 | ✅ |
| **Skill** | `tool_result` (Skill `tool_use` 후) | 모델이 자율 결정 | 🔍 Inspect Only |
| **Sub-Agent** | 격리된 별도 API 호출 | Task 도구 위임 | 🔍 Inspect Only |

## 설치 및 실행

```bash
git clone https://github.com/kangraemin/claude-inspector.git
cd claude-inspector
npm install
npm start        # Electron 데스크탑 앱
```

### 웹 브라우저 모드

```bash
npm run web      # http://localhost:3000
```

### 배포용 빌드

```bash
npm run dist     # release/ 폴더에 .dmg / .exe 생성
```

## 사용법

1. 상단 탭에서 메커니즘 선택
2. 왼쪽 패널에서 내용 입력 (실시간으로 오른쪽 JSON 업데이트)
3. API Key 입력 후 **Send to Claude** → 실제 응답 확인
4. **Copy JSON** 으로 페이로드 복사

> Skill과 Sub-Agent는 Claude Code 런타임이 필요한 구조라 Inspect Only.
> API Key는 UI에서만 입력받고 코드에 저장되지 않습니다.

## 기술 스택

- **Electron** — 크로스 플랫폼 데스크탑
- **@anthropic-ai/sdk** — Anthropic API 호출 (main process IPC)
- **Vanilla JS** — 프레임워크 없음, 빌드 스텝 없음
- **highlight.js** — JSON 신택스 하이라이팅

## 참고

이 프로젝트는 [Reverse Engineering Claude Code](https://) 아티클을 기반으로 구현했습니다.
