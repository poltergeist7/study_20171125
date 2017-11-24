#JSON

##JSON.parse()

 :  JSON을 문자열로 파싱하며, 파싱된 값을 추가로 변환하기도 합니다. <br>

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
  