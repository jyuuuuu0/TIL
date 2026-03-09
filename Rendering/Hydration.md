# Hydration

프론트엔드에서 말하는 **Hydration(하이드레이션)**은 서버에서 미리 생성된 HTML에 JavaScript를 연결하여 인터랙션이 가능한 페이지로 만드는 과정을 의미한다.

## Hydration 사용

최근 프론트엔드 프레임워크인 React, Next.js 등은 SSR(Server Side Rendering) 방식을 많이 사용한다.

Hydration 과정은 다음과 같다.

1. 서버가 HTML을 미리 생성하여 브라우저에 전달한다.
2. 브라우저는 전달받은 HTML을 먼저 화면에 렌더링한다.
3. 이후 JavaScript 파일이 로드된다.
4. JavaScript가 HTML에 이벤트와 상태(state)를 연결한다.

→ 이 과정이 Hydration이다.

### 비유

쉽게 비유하면 다음과 같다.

- SSR HTML → 이미 만들어진 마네킹
- Hydration → 마네킹에 **생명(이벤트와 상태)**을 부여하는 과정

즉,

- 서버(Server): 정적인 HTML 생성
- 클라이언트(Client): Hydration을 통해 인터랙션 활성화
