---
layout: post
title: PDO의 SQL Injection 방어 원리
categories: [일상 중 궁금했던]
comments: true
---

### 1. SQL Injection의 원리
```php
$username = $_GET['username'];
$query = "SELECT * FROM users WHERE username = '$username'";
$stmt = $pdo->query($query);
```
- 사용자가 `' OR 1=1 --` 입력 시, 모든 사용자 정보가 조회됨.

### 2. PDO의 Prepared Statement로 방어
```php
$query = "SELECT * FROM users WHERE username = :username";
$stmt = $pdo->prepare($query);
$stmt->bindValue(':username', $_GET['username'], PDO::PARAM_STR);
$stmt->execute();
```
- 쿼리와 데이터를 분리하여 Injection 공격을 방지.

### 3. PDO가 SQL Injection을 방어하는 원리
- 쿼리와 데이터 분리: SQL 구문이 변하지 않음.  
- 자동 이스케이프 처리: `bindValue()` 사용 시 내부적으로 안전한 변환.  
- 데이터 타입 지정: `PDO::PARAM_STR` 등으로 데이터 형식 보호.

### 4. 요약
| 방법 | SQL Injection 방어 효과 |
|------|--------------------|
| Prepared Statement 사용 | SQL과 데이터를 분리하여, Injection 공격을 원천 차단 |
| bindValue() 또는 bindParam() 사용 | 입력값을 자동으로 이스케이프 처리하여 안전하게 처리 |
| 데이터 타입 지정 (`PDO::PARAM_*`) | 데이터 형식을 명확하게 설정하여 보안 강화 |

### 결론  
PDO의 Prepared Statement를 사용하면 SQL Injection을 효과적으로 방어할 수 있음. 다만, SQL 쿼리 자체를 동적으로 생성할 때는 주의 필요.

