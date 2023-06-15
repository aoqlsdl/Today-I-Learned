## playsinline 
**작성일자**
230615

### 문제 상황
- 회사 홈페이지 모바일 버전 퍼블리싱을 하던 중, 아이폰으로 동영상이 포함된 페이지에 접속할 경우 비디오가 자동으로 최대화되는 현상이 발생하는 것을 확인했다.

### 배운 개념
- 문제 해결을 위해 다음과 같이 코드를 수정했다.
    ```
    // 수정 전
    <video src={VideoName} autoPlay muted loop></video>

    // 수정 후
    <video src={VideoName} autoPlay muted loop playsInline></video>
    ```
- IOS 정책에 따르면, 동영상을 자동 재생하기 위해서는 전체화면으로 전환해야 한다. 동영상을 전체 화면으로 보여주고 싶지 않다면 `<video>` 태그에 `playsInline` 속성을 추가해야 한다.

> ➕ video에서 주로 사용하는 속성 정리<br>
> _이미 알고 있는 속성들이지만 정리하는 김에 함께 정리해두기_ <br>
> - `autoPlay` : 비디오를 자동 재생할 때 사용
> - `muted` : 비디오 음량을 최소화. 브라우저 정책상 동영상에 소리가 포함되어 있을 경우 자동 재생이 불가하기 때문에 동영상을 자동 재생하고 싶을 경우 반드시 포함시켜 줘야 한다.

### 간단한 comment