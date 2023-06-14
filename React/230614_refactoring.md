## 리팩토링
**작성일자**
230614

### 문제 상황
- 회사 연혁 데이터를 영어와 한글로 나누어 뿌려야 했다. 회사 연혁은 json으로 영문, 한글 버전으로 나누어 관리하고 있었고, `map()` 함수를 이용해 데이터를 뿌리고 있었는데, 처음에는 코드를 중복으로 작성해두었고, 이후 리팩토링할 예정이었다.
- 마침(...) 모바일 버전 웹 사이트를 퍼블리싱하는 과정에서 전달받은 디자인 구조가 많이 바뀌어서 해당 코드를 리팩토링해야 하는 상황에 직면했다.

<br>

### 이전에 작성한 코드

```
<div className={window.localStorage.getItem('language') === 'en' ? 'history-contents active' : 'history-contents'}>
  {HISTORY_DATA.map(category => {
      return (
          <>
              {category.historys.map(data => {
                  // HisDesc state == true인 것만 출력하도록 구현하기
                  return (
                      <HisDesc className={selectedYear === category.year ? 'active' : ''}>
                          <div className='desc-top'>
                              {data.date}
                          </div>
                          <div className='desc-body'>
                              {data.content.map(desc => {
                                  return (
                                      <div>{desc}<br></br></div>

                                  )
                              })}

                          </div>
                      </HisDesc>
                  )
              })}
          </>    
      )
  })}
</div>

<div className={window.localStorage.getItem('language') === 'ko' ? 'history-contents active' : 'history-contents'}>
  {HISTORY_DATA_KO.map(category => {
      return (
          <>
              {category.historys.map(data => {
                  return (
                      <HisDesc className={selectedYear === category.year ? 'active' : ''}>
                          <div className='desc-top'>
                              {data.date}
                          </div>
                          <div className='desc-body'>
                              {data.content.map(desc => {
                                  return (
                                      <div>{desc}<br></br></div>

                                  )
                              })}

                          </div>
                      </HisDesc>
                  )
              })}
          </>    
      )
  })}
</div>
```
 
부끄럽지만... 코드가 중복되어 불필요하게 코드가 늘어난 상황이다.

<br>

### 리팩토링

```
const example = () => {
    // 언어에 따른 데이터 선택
    let historyData;
    if (window.localStorage.getItem('language') === 'en') {
        historyData = HISTORY_DATA;
    } else {
        historyData = HISTORY_DATA_KO;
    }

    return (
        <div>
            {historyData.map(category => {
                return (
                    <>
                        {category.historys.slice(0).reverse().map(data => {
                            return (
                                <HisDesc className={selectedYear === category.year ? 'active' : ''}>
                                    <div className='desc-top'>
                                        {data.date}
                                    </div>
                                    <div className='desc-body'>
                                        {data.content.map(desc => {
                                            return (
                                                <div>{desc}<br></br></div>
                                            )
                                        })}
                                    </div>
                                </HisDesc>
                            )
                        })}
                    </>
                )
            })}
        </div>
    )
}
```

return문 이전에 `historyData`를 미리 정의했다. 설정된 언어에 따라 영문 데이터를 선택할지, 한글 데이터를 선택할지 결정했더니 코드를 중복해서 쓸 필요가 없어졌다!

### 간단한 comment
아직까지 리팩토링하지 않고 그냥 두었다기엔 너무 쉽게 해결 방법을 떠올려버려서 스스로에게 아쉬움이 들었다.(물론 한달 전에 비해 내가 성장해서 그런 걸 수도 있다...) 상황상 어쩔 수 없이 넘어가기보다는 조금이라도 더 좋은 코드를 작성할 수 없을지 끊임없이 고민하는 태도를 가져야겠다.

이 코드도 지금 생각하기엔 최선이지만, 다른 사람들이 보기엔 그렇지 않을 수도 있으니 더 클린한 코드가 없을지 계속해서 고민해봐야겠다.
