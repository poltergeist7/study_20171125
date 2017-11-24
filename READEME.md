#JSON

 ##JSON.parse() <br>

 : JSON을 문자열로 파싱하며, 파싱된 값을 추가로 변환하기도 합니다. <br>

  ###문법

  ```javascript
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

  // both will throw a SyntaxError
  JSON.parse('[1, 2, 3, 4, ]');
  JSON.parse('{"foo" : 1, }');
  ```

##JSON.stringify() <br>

 : 자바스크립트 값을 JSON 문자열로 변환하고, 리플레이서 (replacer) 함수가 지정되어있을 때 선택적으로 바꾸거나, 리플레이서 배열이 지정되어있을 때 지정된 속성만 선택적으로 포함할 수 있다.

  ###문법

  ```
  JSON.stringify(value[, replacer[, space]])
  ```

  ###value

  : JSON 문자열로 변환할 값

  ###replacer

  : 문자열화 프로세스의 작동을 변경하는 함수, 혹은 JSON 문자열에 포함될 값 객체의 속성들을 선택하기 위한 화이트리스트(whitelist)로 쓰이는 String 과 Number 객체들의 배열, 이 값이 null 이거나 제공되지 않으면, 객체의 모든 속성들이 JSON 문자열 결과에 포함된다.

  ###space

  : 가독성을 목적으로  JSON 문자열 출력에 공백을 삽입하는데 사용되는 String 또는 Number 객체.

  JSON.stringfy() 는 그 값을 나타내는 JSON 표기법으로 변환한다.
   - 배열이 아닌 객체의 속성들은 어떤 특정한 순서에 따라 문자열화 될 것이라고 보장되지 않는다, 같은 객체의 문자열화에 있어서 속성의 순서에 의존하지 않는다.
   - Boolean, Number, String 객체들은 문자열화 될 때 전통적인 변환 의미에 따라 연관된 기본형(primitive) 값으로 변환된다.
   - undefined, 함수, 심볼(symbol)은 변환될 때 생략되거나 (객체 안에 있을 경우) null 로 변환된다(배열 안에 있을 경우).
   - 심볼을 키로 가지는 속성들은 replacer 함수를 사용하더라도 완전히 무시된다.
   - 열거 불가능한 속성들은 무시된다.

   ```javascript
   JSON.stringify({});                  // '{}'
   JSON.stringify(true);                // 'true'
   JSON.stringify('foo');               // '"foo"'
   JSON.stringify([1, 'false', false]); // '[1,"false",false]'
   JSON.stringify({ x: 5 });            // '{"x":5}'

   JSON.stringify(new Date(2006, 0, 2, 15, 4, 5))
   // '"2006-01-02T15:04:05.000Z"'

   JSON.stringify({ x: 5, y: 6 });
   // '{"x":5,"y":6}' or '{"y":6,"x":5}'
   JSON.stringify([new Number(1), new String('false'), new Boolean(false)]);
   // '[1,"false",false]'

   // Symbols:
   JSON.stringify({ x: undefined, y: Object, z: Symbol('') });
   // '{}'
   JSON.stringify({ [Symbol('foo')]: 'foo' });
   // '{}'
   JSON.stringify({ [Symbol.for('foo')]: 'foo' }, [Symbol.for('foo')]);
   // '{}'
   JSON.stringify({ [Symbol.for('foo')]: 'foo' }, function(k, v) {
     if (typeof k === 'symbol') {
       return 'a symbol';
     }
   });
   // '{}'

   // Non-enumerable properties:
   JSON.stringify( Object.create(null, { x: { value: 'x', enumerable: false }, y: { value: 'y', enumerable: true } }) );
   // '{"y":"y"}'
   ```

   ###replacer *매개 변수*

   : replacer 매개변수는 함수 또는 배열이 될 수 있다. 함수일 때는 문자열화 될 key 와 value, 2개의 매개변수를 받는다. key 가 발견된 객체는 리플레이서의 this 매개변수로 제공된다. Initially it gets called with an empty key representing the object being stringified, and it then gets called for each property on the object or array being stringified. 이것은 JSON 문자열에 추가되어야 하는 값을 반환해야한다:
   Number 를 반환하면, JSON 문자열에 추가될 때 그 수를 나타내는 문자열이 그 속성의 값으로 사용된다.

    - String 을 반환하면, 그것이 JSON 문자열에 추가될 때 속성의 값으로 사용된다.
    - Boolean 을 반환하면, 그것이 JSON 문자열에 추가될 때 "true" 또는 "false" 이 속성의 값으로 사용된다.
    - 다른 객체를 반환하면, 그 객체는 replacer 함수를 각각의 속성에 대해 호출하며 순환적으로 JSON 문자열로 문자열화된다. 그 객체가 함수인 경우에는 JSON 문자열에 아무것도 추가되지 않는다.
    - undefined 를 반환하면, 그 속성은 JSON 문자열 결과에 포함되지 않는다.