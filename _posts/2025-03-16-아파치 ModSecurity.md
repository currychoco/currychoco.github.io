---
layout: post
title: 아파치 ModSecurity
categories: [일상 중 궁금했던]
comments: true
---

# 목표
다양한 IP가 특정 URI에 너무 자주 접근하는 경우 차단하기

---

# ModSecurity 설정 요약

```apache
SecAction "id:9000000, phase:1, initcol:ip=%{REMOTE_ADDR}, pass, nolog"

SecRule REQUEST_URI "@beginsWith /login" \
  "id:9000001, phase:2, pass, nolog, \
   setvar:ip.login_attempts=+1, \
   expirevar:ip.login_attempts=60"

SecRule IP:LOGIN_ATTEMPTS "@gt 10" \
  "id:9000002, phase:2, deny, status:429, log, \
   msg:'Too many login attempts from this IP'"

# 확인
- 서버 열어서 해당 방법 되는지 확인해보기
