# 🔧 Google Apps Script로 웹앱(웹훅) 만들고 숏링크로 공유하기

별도 서버 없이 간단한 API/웹훅 엔드포인트가 필요해서 **Google Apps Script(GAS) 웹앱 배포**를 찾아봤고,
그 과정에서 배포 URL이 너무 길어 **숏링크 서비스**까지 함께 알아보게 됐습니다.

---

## 1️⃣ Apps Script를 웹앱으로 배포하기

Apps Script 프로젝트는 `doGet(e)` / `doPost(e)` 함수만 정의하면 **HTTP 요청을 받는 엔드포인트**로 배포할 수 있습니다.

```js
function doGet(e) {
  const param = e.parameter.name ?? "world";
  return ContentService
    .createTextOutput(JSON.stringify({ message: `hello, ${param}` }))
    .setMimeType(ContentService.MimeType.JSON);
}

function doPost(e) {
  const body = JSON.parse(e.postData.contents);
  // 예: 시트에 웹훅 데이터 기록
  SpreadsheetApp.getActiveSpreadsheet().getSheetByName("log").appendRow([new Date(), JSON.stringify(body)]);
  return ContentService.createTextOutput("OK");
}
```

### 🔹 배포 방법

1. 에디터 상단 **배포 → 새 배포**
2. 유형 선택: **웹 앱**
3. `실행 대상` - 보통 "나(본인 계정)"으로 설정
4. `액세스 권한` - "모든 사용자"로 열어야 외부에서 웹훅으로 호출 가능
5. 배포하면 `https://script.google.com/macros/s/AKfycb.../exec` 형태의 URL 발급

⚠️ 코드를 수정해도 기존 배포 URL에는 반영되지 않고, **배포 관리에서 새 버전으로 다시 배포**해야 반영됨

---

## 2️⃣ 웹훅으로 활용하기

`doPost`를 열어두면 외부 서비스(예: 폼 제출 알림, 간단한 상태 체크 등)에서 오는 웹훅을 받아
Google Sheets에 바로 로그를 쌓거나, 조건에 따라 Slack/이메일로 알림을 보내는 용도로 쓸 수 있습니다.

- 별도 서버·배포 비용 없이 무료로 API 엔드포인트를 만들 수 있다는 점이 가장 큰 장점
- 다만 실행 시간 제한(트리거 기준 6분), 동시 요청 처리량 제한이 있어 트래픽이 많은 용도에는 부적합
