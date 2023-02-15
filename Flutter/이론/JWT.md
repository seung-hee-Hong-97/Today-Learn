# JWT

- JSON Web Token 약자
- Header, Payload, Signature로 이루어져 있음
- Base 64로 인코딩 되어 있음
- `Header`는 토큰의 종류와 암호와 알고리즘 등 토큰에 대한 정보가 들어 있음
- `Payload`는 발행일, 만료일, 사용자 ID 등 사용자 검증에 필요한 정보가 들어 있음
- `Signature`는 Base 64로 인코딩 된 Header와 Payload를 알고리즘으로 싸인한 값이 들어 있음 이 값을 기반으로 토큰이 발급된 뒤로 조작되었는지 확인할 수 있음

<br />

## JWT To JSON

JWT를 Decoding 하면 각각 Header, Payload, Signature 형태로 JSON으로 변환됨

JSON을 Encoding하면 JWT 토큰 형태로 변환됨

<br />

# Refresh Token & Access Token

- 두 토큰 모두 JWT 기반
- `Access Token`은 API 요청을 할 때 검증용 토큰으로 사용됨. 즉 인증이 필요한 API를 사용할 때는 꼭 Access Token을 Header에 넣어서 보내야 함
- `Refresh Token`은 Access Token을 추가적으로 발급할 때 사용함. Access Token을 새로고침 하는 기능이 있음
- `Access Token`은 유효기간이 짧고 `Refresh Token`은 유효기간이 김
- 자주 노출 되는 `Access Token`은 유효기간을 짧게 해서 Token이 탈취 되어도 해커가 오래 사용하지 못하도록 방지할 수 있음
- 상대적으로 노출이 적은 `Refresh Token`의 경우 `Access Token`을 새로 발급 받을 때만 사용되기 때문에 탈취 가능성이 적음

<br />

| 비교 요소         | Access Token                                             | Refresh Token                               |
| ----------------- | -------------------------------------------------------- | ------------------------------------------- |
| 자주 노출 되는가? | Header에 Access Token을 실어서 보내기 때문에 자주 노출됨 | 상대적으로 노출이 적음                      |
| 유효기간은?       | 짧음                                                     | 상대적으로 김                               |
| 사용 용도는?      | API 요청을 할 때 검증용 토큰으로 사용됨                  | Access Token을 추가적으로 발급 할 때 사용됨 |

<br />

## Flutter에서 토큰 Encode

```dart
import 'dart:convert';

final exampleString = 'test@example.ai:testtest';

Codec<S, T> stringToBase64 = utf8.fuse('T에 입력한 타입 값');

String token = stringToBase64.encode(exampleString);
```

> `S` : Encode하기 위해 넣어줄 값의 타입
>
> `T` : 반환 받을 값의 타입
