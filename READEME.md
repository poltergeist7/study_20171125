#JSON

##JSON.parse() <br>
 : JSON 타입을 배열로 변환

 : JSON을 문자열로 파싱하며, 파싱된 값을 추가로 변환하기도 합니다. <br>

 ```
 JSON.parse(text[, reviver])
 ```
 
  ###인자값
  
  *text* <br>
  JSON으로 파싱할 문자열. JSON 문법에 대해선 JSON 오브젝트를 참조하세요.
  
  *reviver* Optional <br>
  함수가 올 경우, 리턴되기 전에 파싱된 문자열 값이 어떻게 변환될 것인지를 결정함.
  
  ###리턴
  
  주어진 JSON 텍스트에 따라 Object를 리턴함
  
  ###에러
  
  파싱할 문자열이 유효한 JSON이 아닐 경우 SyntaxError  예외를 던짐.
  
  ###JSON.parse() 사용하기
  
  ```javascript
  JSON.parse('{}');                 // {}
  JSON.parse('true');               // true
  JSON.parse('"foo"');              // "foo"
  JSON.parse('[1, 5, "false"]');    // [1, 5, "false"]
  JSON.parse('null');               // null
  ```

  *reviver*

  이 함수는 개체의 각 멤버에 대해 호출됩니다. 멤버에 중첨된 개체가 포함되어 있으면 포함되어 있으면 중첩된 개체가 부모 개체보다 먼저 변환됩니다. 멤버 각각에 대해 다음이 발생합니다.

    - *reviver* 에서 유효한 값을 반환하면 멤버 값은 변환된 값으로 바뀝니다.
    - *reviver* 에서 수신한 값과 동일한 값을 반환하면 멤버 값은 수정되지 않습니다.
    - *reviver* 가 null 또는 undefined를 반환하면 멤버가 삭제됩니다.

  ```javascript
  JSON.parse('{"p": 5}', function(k, v) {
    if (k === '') {return v; }      // if topmost value, return it,
    return v * 2;                   // else return v * 2.
  });                               // { p: 10 }

  JSON.parse('{"1": 1, "2": 2,"3":{"4": 4, "5": {"6": 6}}}', function(k, v) {
    console.log(k);                 // log the current property name, the last is "".
    return v;                       // return the unchanged property value.
  });

  // 1
  // 2
  // 4
  // 6
  // 5
  // 3
  // ""
  ```
  