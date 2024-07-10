---
layout: post
title: Stored XSS 방어 방법
categories: [일상 중 궁금했던]
comments: true
---

# Stored XSS 대응 및 `Content-Security-Policy (CSP)` 적용 가이드

## 1. Stored XSS란?
- **저장형 XSS**: 악성 스크립트가 서버에 저장되어 다른 사용자의 브라우저에서 실행됨.
- **발생 원인**: 필터링 없이 DB에 저장 후 출력, `<script>`, `<img onerror>`, `<iframe>` 태그 미제거.

## 2. XSS 대응 방법
### 입력값 필터링
```php
$safe_input = htmlspecialchars($user_input, ENT_QUOTES, 'UTF-8'); // HTML 이스케이프
$filtered_input = strip_tags($user_input, '<b><i><p>'); // 특정 태그만 허용
```

### 출력값 이스케이프
```php
echo htmlspecialchars($stored_data, ENT_QUOTES, 'UTF-8');
```

### 보안 헤더 적용
```php
header("X-XSS-Protection: 1; mode=block"); // 브라우저에서 XSS 차단
header("Content-Security-Policy: default-src 'self'; script-src 'self';"); // 외부 스크립트 차단
```

---

## 3. `Content-Security-Policy (CSP)` 적용 시 고려사항
| 기능 | 문제 발생 가능 여부 |
|------|----------------|
| 외부 CDN(jQuery 등) | 차단됨 |
| Google Analytics, 광고 코드 | 차단됨 |
| reCAPTCHA, SNS 로그인 | 차단됨 |
| 인라인 `<script>` 실행 | 차단됨 |
| 내부 도메인 JavaScript | 정상 작동 |

---

## 4. 기존 기능 유지하면서 CSP 적용하는 방법
### 특정 도메인 허용
```php
header("Content-Security-Policy: default-src 'self'; script-src 'self' https://code.jquery.com https://www.google-analytics.com;");
```
### 인라인 스크립트 실행 허용 (비추천)
```php
header("Content-Security-Policy: default-src 'self'; script-src 'self' 'unsafe-inline';");
```
### reCAPTCHA 등 외부 API 허용
```php
header("Content-Security-Policy: default-src 'self'; script-src 'self' https://www.google.com https://www.gstatic.com;");
```
### 차단된 스크립트 로그 확인
```php
header("Content-Security-Policy: default-src 'self'; script-src 'self' 'unsafe-inline'; report-uri /csp-report-endpoint;");
```

---

## 5. 결론: CSP 적용 시 주의할 점
- **CDN을 사용한다면 특정 도메인 허용 필요**
- **인라인 스크립트 (`onclick` 등) 많이 사용하면 기능 깨질 가능성 높음**
- **차단 로그를 확인하면서 조정하는 것이 최선**

**최적 설정 예시 (CDN 및 reCAPTCHA 허용, 인라인 스크립트 차단)**
```php
header("Content-Security-Policy: default-src 'self'; script-src 'self' https://code.jquery.com https://www.google.com https://www.gstatic.com;");
```
**보안 유지 + 주요 기능 정상 작동 가능!**





































